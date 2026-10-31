# Image子系统变更说明

## Image组件网络图片加载HTTPS证书校验行为变更说明

**访问级别**

公开接口

**变更原因**

为提升 Image 组件加载网络图片时的传输安全性，防止中间人攻击与证书伪造风险，在 ArkUI-X SDK 中为 Image 接口的网络图片加载流程增加证书安全校验能力。

**变更影响**

该变更为不兼容变更。

- 变更前：Image 接口在加载网络图片时，对 HTTPS 证书的校验策略相对宽松，部分场景下对证书链的完整性或有效性校验不充分，自签名、过期或域名不匹配的证书可能导致加载行为不一致或静默失败。

- 变更后：从 ArkUI-X SDK 7.0 Release 开始，Image 接口在加载网络图片时执行证书安全校验。默认情况下，证书链中任一证书的哈希值不匹配、证书已过期、为自签名证书或域名不匹配时，图片加载将失败。若服务端更新了证书，而应用未及时适配，网络图片加载可能中断。

**起始 API Level**

26.0.1

**变更发生版本**

从 ArkUI-X SDK 7.0 Release 开始。

**变更的接口/组件**

Image 组件的网络图片加载能力（通过 `Image(src: ResourceStr | PixelMap | DrawableDescriptor)` 传入 HTTPS URL 时触发）。

**适配指导**

Image 组件加载网络图片失败时，若下载失败回调的 errorCode 为 32（0x20，SSL 错误），可确认为证书校验或主机名校验失败，请按以下方案适配：

- 方案一（推荐）：服务端修复证书。确保证书的 SAN（主题备用名称）覆盖图片实际使用的每一个主机名。注意通配符证书 `*.example.com` 仅匹配单级子域，不匹配 `b.a.example.com` 等多级子域。主机名校验无法在客户端绕过，SAN 不匹配时必须在服务端（或 CDN 控制台）修复证书。

- 方案二：应用信任私有 CA（自建证书场景，要求证书 SAN 已正确覆盖主机名）。Android 平台可通过 networkSecurityConfig 配置信任锚；`cacheDownload.download()` 的 `caPath` 参数仅放宽信任链校验且仅在 Android 平台生效，不能解决 Image 组件隐式下载场景的加载失败。iOS 平台建议服务端使用公共受信 CA 签发证书。

- 方案三（不推荐，评估后使用）：改用 HTTP 明文链接。Android 平台需声明 `android:usesCleartextTraffic` 或通过 networkSecurityConfig 按域名放行明文流量；iOS 平台需在 Info.plist 中按域名配置 ATS 例外。明文传输存在被窃听、篡改的风险。

详细的排查步骤、各方案的平台配置方法与常见问题，请参考[cacheDownload图片加载HTTPS安全证书配置指导](../../../application-dev/tutorial/how-to-configure-https-cert-for-image-download.md)。
