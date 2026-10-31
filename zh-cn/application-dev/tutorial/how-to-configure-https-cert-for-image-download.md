# cacheDownload图片加载HTTPS安全证书配置指导

## 简介

ArkUI-X 的 Image 组件加载网络图片时，图片下载能力由 cacheDownload 引擎提供。该引擎遵循系统默认的 TLS 严格校验策略，因此部分图片 URL 会因服务端证书配置问题而加载失败。本文从问题现象入手，说明失败原因，并给出服务端证书修复、应用侧信任私有 CA、改用 HTTP 明文链接三类解决途径。

本文将帮助您了解以下内容：

- 为什么部分网络图片无法加载（信任链校验与主机名校验两个独立环节）
- 如何排查并确认是证书问题（errorCode=32）
- 如何获取并配置安全证书（服务端修复、应用信任私有 CA）
- 何种场景可以改用 HTTP 明文链接及其平台配置方式

## 场景说明：部分网络图片无法加载的原因

### 现象

绝大多数网络图片加载正常，但少数图片（常见于某些 CDN 的多级子域名，例如 `g.search.alicdn.com`）加载失败，Image 组件显示异常（alt 内容或占位图），下载失败回调的 errorCode 为 32（0x20，SSL 错误）。

### 机制

cacheDownload 引擎的 HTTP 客户端（Android 平台为 `HttpsURLConnection`，iOS 平台为 `NSURLSession`）遵循**系统默认的严格 TLS 校验**，包含两个相互独立的环节：

1. **信任链校验**：服务端证书是否由系统内置受信任 CA 签发（或由应用配置的信任锚签发）。自签名证书、私有 CA 证书若未在应用侧配置信任，此环节即失败。
2. **主机名校验（RFC 6125）**：证书的 SAN（主题备用名称）必须覆盖请求的主机名。通配符证书 `*.example.com` 只匹配**单级**子域（如 `a.example.com`），不匹配多级子域（如 `b.a.example.com`）。

### 典型案例

CDN 为域名签发了 `*.alicdn.com` 通配符证书。该证书对 `img.alicdn.com`（单级子域）有效，对 `g.search.alicdn.com`（两级子域）无效——尽管两张图片使用的是同一张证书，后者的主机名校验仍然失败。这不是客户端缺陷：使用 curl/OpenSSL 或主流浏览器访问同一 URL，同样会被拒绝。

> **说明**：cacheDownload 下载失败的 errorCode 按错误环节划分，可通过 errorCode 快速定位失败层面：

| errorCode | 十六进制 | 含义 |
| --------- | -------- | ---- |
| 0 | 0x00 | DNS 错误（域名解析失败） |
| 16 | 0x10 | TCP 错误（连接建立失败） |
| 32 | 0x20 | SSL 错误（证书校验、主机名校验或协议协商失败） |
| 48 | 0x30 | HTTP 错误（统一归为 48，具体 HTTP 状态码如 404 通过 message 字段返回） |
| 255 | 0xFF | 其他错误 |

> **注意**：ArkUI-X 早期版本的 Image 网络栈曾跳过证书校验与主机名校验（trust-all），当时这类图片"能加载"，但存在安全风险（中间人攻击可篡改或替换图片内容）。当前版本启用严格校验是对安全性的增强，**不建议回退**。

## 排查：如何确认是证书问题

### 步骤 1：查看服务端握手与证书信息

在开发环境中使用 `curl` 或 `openssl` 检查目标主机的 TLS 握手过程：

```shell
curl -v https://g.search.alicdn.com/img.png
```

或：

```shell
openssl s_client -connect g.search.alicdn.com:443 -servername g.search.alicdn.com </dev/null
```

### 步骤 2：检查证书的 SAN 是否覆盖主机名

将服务端证书导出后查看其 SAN（主题备用名称）：

```shell
openssl x509 -in cert.pem -noout -text | grep -A2 "Subject Alternative Name"
```

### 步骤 3：根据报错判断问题类型

若命令输出中出现 `hostname mismatch` 或 `subjectAltName does not match` 等信息，即可确认是证书 SAN 不覆盖主机名的问题，需在服务端或 CDN 侧修复证书（见方案一）。

> **说明**：客户端侧若观察到 cacheDownload 下载失败回调 errorCode=32（SSL 错误），是证书问题的典型信号，可结合上述命令进一步定位。

## 前置条件

在按照本文操作前，请确认满足以下条件：

- 已集成 ArkUI-X SDK。Image 组件的网络图片下载依赖 cacheDownload 引擎，无需额外引入。
- Android 平台的图片网络访问需要在 `AndroidManifest.xml` 中声明 `android.permission.INTERNET` 权限。

## 方案一（推荐）：服务端修复证书

服务端修复证书是**唯一能同时解决"信任链校验 + 主机名校验"两类问题**的正解。

### 获取合规证书的途径

| 途径 | 说明 | 适用场景 |
| ---- | ---- | -------- |
| Let's Encrypt | 免费、自动化（ACME）、证书有效期 90 天 | 自有域名且服务端可控 |
| 商业 CA（DigiCert、GlobalSign 等） | 付费，提供 OV/EV 等级证书 | 企业生产环境 |
| CDN/云厂商证书托管 | 在 CDN 控制台为域名申请或上传证书，自动部署到边缘节点 | 使用 CDN 的图片域名（推荐，与本案例直接相关） |

### 关键要求

证书的 SAN 必须包含图片实际使用的**每一个**主机名。通配符证书 `*.example.com` 只覆盖单级子域。CDN 场景下，请在控制台为 `g.search.example.com` 这类多级子域单独配置证书，或补签 `*.search.example.com` 等覆盖对应层级的通配符证书。

**客户端无法通过任何配置绕过主机名校验**：自定义 CA 只能解决信任链问题，无法解决 SAN 不匹配问题。SAN 不覆盖主机名时，必须走本方案在服务端侧修复。

## 方案二：应用信任私有 CA（自建证书场景）

### 适用边界

本方案适用于企业内网或私有部署场景：服务端使用私有 CA 签发证书，**且该证书的 SAN 已正确覆盖主机名**。若证书 SAN 不匹配，本方案无效，请先参考方案一修复服务端证书。

### 途径 A：cacheDownload 接口参数 caPath（ArkTS，仅 Android）

在调用 `cacheDownload.download()` 时通过 `CacheDownloadOptions` 的 `caPath` 参数指定自定义 CA 证书：

```ts
import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

let options: cacheDownload.CacheDownloadOptions = {
  headers: { 'Accept': 'image/*' },
  sslType: cacheDownload.SslType.TLS,
  caPath: '/path/to/ca.pem',   // CA证书路径，仅支持.pem格式，默认使用系统预设CA证书
  cacheStrategy: cacheDownload.CacheStrategy.FORCE
};

try {
  cacheDownload.download('https://private.example.com/img/photo.jpg', options);
} catch (error) {
  let err: BusinessError = error as BusinessError;
  console.error(`Failed to download. code: ${err.code}, message: ${err.message}`);
}
```

> **注意**：`caPath` 从 API version 21 起支持，仅在 **Android** 平台生效；iOS 平台当前不生效（使用系统默认信任链）。`caPath` 为文件系统绝对路径，指向单个 PEM 格式的 CA 证书文件（多证书 bundle 只取第一个）；Android 上应使用应用沙箱内路径并确保文件已落盘。若 `caPath` 指向的文件不存在，错误归入 OTHERS（0xFF）。

> **说明**：`caPath` 与系统默认信任是**叠加**（union）关系——配置后系统对公共 CA 站点的信任不受影响；但它**只放宽信任链校验，不会跳过主机名校验**。

> **说明**：Image 组件隐式触发的下载不携带 `caPath` 参数，且其默认缓存策略（FORCE）总是重新发起网络下载——即使已通过 `cacheDownload.download()` 显式预缓存成功，Image 加载同一 URL 时仍会重新走系统默认校验。因此，**本途径不能解决 Image 组件加载失败的场景**，仅适用于应用自身通过 `cacheDownload.download()` 获取数据的场景；Image 场景请改用途径 B（Android）或方案一。

### 途径 B：平台网络安全配置（Android networkSecurityConfig）

若需在宿主 App 全局信任私有 CA（包含 Image 隐式下载场景），可使用 Android 的网络安全配置。

创建 `res/xml/network_security_config.xml`：

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config>
        <trust-anchors>
            <certificates src="@raw/private_ca"/>
            <certificates src="system"/>
        </trust-anchors>
    </base-config>
</network-security-config>
```

将 CA 证书 PEM 文件放置在 `res/raw/private_ca.pem`，并在 `AndroidManifest.xml` 的 `<application>` 标签中添加：

```xml
<application
    android:networkSecurityConfig="@xml/network_security_config"
    ... >
```

> **说明**：Android 7.0（API 24）起，应用默认仅信任系统 CA，不再信任用户安装的 CA，必须在 networkSecurityConfig 中显式声明信任来源。

### iOS 平台

`caPath` 参数在 iOS 平台当前不生效；私有 CA 信任需要在应用打包层处理（例如嵌入证书并在 `NSURLSession` 层自证，当前 cacheDownload 引擎未提供该能力）。建议 iOS 场景下服务端使用公共受信 CA 签发证书。

> **注意**：途径 A 与途径 B 都只解决信任链问题。**证书 SAN 不覆盖主机名时，必须走方案一在服务端侧修复。**

## 方案三：改用 HTTP 明文链接（不推荐，评估后使用）

### 适用场景

本方案适用于：图片内容完全公开、无篡改风险诉求，或内网可控环境，且开发者接受明文传输的安全代价。cacheDownload 引擎本身不拦截 `http://` 链接，但平台系统默认策略会阻止明文请求，需要按平台放行。

### Android

Android 9（API 28）起默认禁止明文 HTTP 流量。允许方式（二选一）：

- 在 manifest 中设置 `<application android:usesCleartextTraffic="true">`（粗放开关，允许全局明文）；
- 或使用 networkSecurityConfig 精细化控制（推荐）：

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="false">img.example.com</domain>
    </domain-config>
</network-security-config>
```

该配置只对指定域名放行明文流量，其余域名仍保持 HTTPS 强制策略。

### iOS

App Transport Security（ATS）默认要求 HTTPS。若确需明文，可在 Info.plist 中按域名放行：

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <false/>
    <key>NSExceptionDomains</key>
    <dict>
        <key>img.example.com</key>
        <dict>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
        </dict>
    </dict>
</dict>
```

优先按域名单点放行（如上例），避免设置 `NSAllowsArbitraryLoads=true` 全局放开。

> **注意**：明文传输可被窃听或篡改；若图片 URL 承载业务语义（如验证码、广告素材计费），风险更高。证书配置可行时，优先使用方案一或方案二。服务端最好同时提供 301 重定向到 HTTPS 的渐进路径，或仅在过渡期使用明文。

## 常见问题（FAQ）

### Q1：配置了自定义 CA（caPath / networkSecurityConfig）后图片仍加载失败？

请检查证书的 SAN 是否覆盖请求的主机名（见"排查"章节）。信任链校验与主机名校验相互独立，自定义 CA 只能修复信任链问题，不能修复 SAN 不匹配。

### Q2：errorCode=32 是什么错误？

SSL 层错误（0x20），涵盖证书校验失败、主机名校验失败、协议协商失败。完整对照：DNS 错误为 0x00，TCP 错误为 0x10，HTTP 错误为 0x30（具体 HTTP 状态码通过 message 字段返回），其他错误为 0xFF。

### Q3：同一张图，img.alicdn.com 能加载，g.search.alicdn.com 不能？

`*.alicdn.com` 单级通配符证书不覆盖 `g.search.alicdn.com` 这类两级子域，属于典型的 SAN 覆盖问题，需要 CDN 侧为该域名配置覆盖它的证书。

### Q4：为什么旧版本能加载，新版本不能？

旧版网络栈跳过证书校验与主机名校验（trust-all），属于不安全实现，已随网络栈演进退役。当前严格校验行为与主流浏览器、HarmonyOS 原生一致。

## 参考资料

- [cacheDownload API参考](../reference/apis/js-apis-request-cacheDownload.md)
- [http请求配置CA证书提示本地SSL证书错误（FAQ）](./faq/Development-Stage/Dev-faq-16.md)
- [Network security configuration（Android 官方）](https://developer.android.com/training/articles/security-config)
- [Preventing insecure network connections / ATS（Apple 官方）](https://developer.apple.com/documentation/security/preventing-insecure-network-connections)
