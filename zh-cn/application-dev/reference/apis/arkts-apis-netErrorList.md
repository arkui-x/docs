# @ohos.web.netErrorList (ArkWeb网络协议栈错误列表)
ArkWeb的网络协议栈错误列表。

ArkWeb网络协议栈错误码直接映射Chromium错误码<!--RP1-->net_error_list.h<!--RP1End-->。

> **说明：**
> 
> 本模块首批接口从API version 22开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```js
import { WebNetErrorList } from '@kit.ArkWeb';
```

## WebNetErrorList

ArkWeb的网络协议栈错误列表

**系统能力**：SystemCapability.Web.Webview.Core

| 名称          | 值 | 说明                                      | Android平台                             | iOS平台                        |
| ------------- | -- |----------------------------------------- |----------------------------------------- |----------------------------------------- |
|NET_OK | 0 |  访问正常。  |  支持  |  支持  |
|ERR_IO_PENDING | -1 | 异步IO操作尚未完成。| 支持 | 支持 |
|ERR_FAILED | -2 | 一般性的错误。 | 支持 | 支持 |
|ERR_ABORTED | -3 | 操作已被中止。| 支持 | 支持 |
|ERR_INVALID_ARGUMENT | -4 | 参数无效。 | 支持 | 支持 |
|ERR_INVALID_HANDLE | -5 | 文件描述符的句柄是无效的。| 支持 | 支持 |
|ERR_FILE_NOT_FOUND | -6 | 文件未找到。 | 支持 | 支持 |
|ERR_TIMED_OUT | -7 | 操作超时。| 支持 | 支持 |
|ERR_FILE_TOO_LARGE | -8 | 文件过大。| 支持 | 支持 |
|ERR_UNEXPECTED | -9 | 遇到了一个未被预期或未被特定处理的问题。| 支持 | 支持 |
|ERR_ACCESS_DENIED | -10 | 访问除了网络以外的资源被拒绝。 | 支持 | 支持 |
|ERR_NOT_IMPLEMENTED | -11 | 功能未实现，导致操作失败。 | 支持 | 支持 |
|ERR_INSUFFICIENT_RESOURCES | -12 | 系统或程序执行所需的资源不足。| 支持 | 支持 |
|ERR_OUT_OF_MEMORY | -13 | 内存溢出。| 支持 | 支持 |
|ERR_UPLOAD_FILE_CHANGED | -14 | 上传文件失败因为文件的修改时间不符合预期。 | 支持 | 支持 |
|ERR_SOCKET_NOT_CONNECTED | -15 | 长连接断开。 | 支持 | 支持 |
|ERR_FILE_EXISTS | -16 | 文件已存在。| 支持 | 支持 |
|ERR_FILE_PATH_TOO_LONG | -17 | 文件路径或者文件名过长。| 支持 | 支持 |
|ERR_FILE_NO_SPACE | -18 | 磁盘没有足够空间。  | 支持 | 支持 |
|ERR_FILE_VIRUS_INFECTED | -19 | 文件包含病毒。 | 支持 | 支持 |
|ERR_BLOCKED_BY_CLIENT | -20 | 客户端已阻止该请求。 | 支持 | 支持 |
|ERR_NETWORK_CHANGED | -21 | 网络发生变化。 | 支持 | 支持 |
|ERR_BLOCKED_BY_ADMINISTRATOR | -22 | 被管理员阻止。| 支持 | 支持 |
|ERR_SOCKET_CONNECTED | -23 | socket已经处于连接状态。| 支持 | 支持 |
|ERR_UPLOAD_STREAM_REWIND_NOT_SUPPORTED | -25 | 上传重传不支持。 | 支持 | 支持 |
|ERR_CONTEXT_SHUT_DOWN | -26 | 因为上下文已关闭导致请求失败。| 支持 | 支持 |
|ERR_BLOCKED_BY_RESPONSE | -27 | 请求失败，因为响应不满足要求（例如“X-Frame-Options”和“Content Security Policy”检查以及“Cross Origin Resource Policy”）。| 支持 | 支持 |
|ERR_CLEARTEXT_NOT_PERMITTED | -29 | 该请求被系统策略阻止，不允许部分或全部明文请求。| 支持 | 支持 |
|ERR_BLOCKED_BY_CSP | -30 | 请求被内容安全策略阻止。 | 支持 | 支持 |
|ERR_H2_OR_QUIC_REQUIRED | -31 |  由于没有H/2或QUIC会话，请求被阻止。| 支持 | 支持 |
|ERR_BLOCKED_BY_ORB | -32 | 请求被 CORB 或 ORB 阻止。 | 支持 | 支持 |
|ERR_CONNECTION_CLOSED | -100 |  连接已关闭（对应于TCP FIN）。| 支持 | 支持 |
|ERR_CONNECTION_RESET | -101 |  连接被重置（对应于TCP RST）。| 支持 | 支持 |
|ERR_CONNECTION_REFUSED | -102 | 连接被拒绝。| 支持 | 支持 |
|ERR_CONNECTION_ABORTED | -103 | 由于未接收到发送数据的ACK，连接超时。这可能包括一个未得到ACK的FIN数据包。| 支持 | 支持 |
|ERR_CONNECTION_FAILED | -104 |  连接失败。| 支持 | 支持 |
|ERR_NAME_NOT_RESOLVED | -105 |  域名无法解析。 | 支持 | 支持 |
|ERR_INTERNET_DISCONNECTED | -106 | 网络断开连接。| 支持 | 支持 |
|ERR_SSL_PROTOCOL_ERROR | -107 | SSL 协议错误。| 支持 | 支持 |
|ERR_ADDRESS_INVALID | -108 |  IP地址或端口号无效（例如，无法连接到IP地址0或端口0）。| 支持 | 支持 |
|ERR_ADDRESS_UNREACHABLE | -109 |  无法访问IP地址。这通常意味着没有到指定主机或网络的路由。 | 支持 | 支持 |
|ERR_SSL_CLIENT_AUTH_CERT_NEEDED | -110 |  服务器指定需要客户端提供SSL证书来校验身份。| 支持 | 支持 |
|ERR_TUNNEL_CONNECTION_FAILED | -111 |  无法建立通过代理的隧道连接。| 支持 | 支持 |
|ERR_NO_SSL_VERSIONS_ENABLED | -112 | 不支持SSL协议的任何版本。| 支持 | 支持 |
|ERR_SSL_VERSION_OR_CIPHER_MISMATCH | -113 | 客户端和服务器不支持通用的SSL协议版本或密码套件。| 支持 | 支持 |
|ERR_SSL_RENEGOTIATION_REQUESTED | -114 | 服务器请求重新协商（rehandshake）。| 支持 | 支持 |
|ERR_PROXY_AUTH_UNSUPPORTED | -115 | 代理请求身份验证（隧道连接使用不支持的方法）。| 支持 | 支持 |
|ERR_BAD_SSL_CLIENT_AUTH_CERT | -117 | SSL 客户端证书错误。| 支持 | 支持 |
|ERR_CONNECTION_TIMED_OUT | -118 | 连接超时。 | 支持 | 支持 |
|ERR_HOST_RESOLVER_QUEUE_TOO_LARGE | -119 | 挂起的DNS解析太多，因此队列中的请求被中止。| 支持 | 支持 |
|ERR_SOCKS_CONNECTION_FAILED | -120 |  无法为目标主机建立与 SOCKS 代理服务器的连接。    |  支持  |  支持  |
|ERR_SOCKS_CONNECTION_HOST_UNREACHABLE | -121 | 域名不可达。 | 支持 | 支持 |
|ERR_ALPN_NEGOTIATION_FAILED | -122 | 协商备用协议的请求失败。| 支持 | 支持 |
|ERR_SSL_NO_RENEGOTIATION | -123 | 对方发送了SSL no_regregation警报消息。| 支持 | 支持 |
|ERR_WINSOCK_UNEXPECTED_WRITTEN_BYTES | -124 | Winsock有时会报告写入的数据多于传递的数据。这可能是由于LSP损坏。| 支持 | 支持 |
|ERR_SSL_DECOMPRESSION_FAILURE_ALERT | -125 | SSL对等端向本端发送了致命的解压失败警报。当对等方错误地认为它支持DEFLATE压缩时，通常会发生这种情况。| 支持 | 支持 |
|ERR_SSL_BAD_RECORD_MAC_ALERT | -126 | SSL对等端向本端发送了致命的bad_record_mac警报。一个对DELEGATE支持有问题的服务器可能发生这个情况。| 支持 | 支持 |
|ERR_PROXY_AUTH_REQUESTED | -127 | 代理请求身份验证（用于隧道建立）。| 支持 | 支持 |
|ERR_PROXY_CONNECTION_FAILED | -130 | 无法创建到代理服务器的连接。域名解析或连接套接字时出错。请注意，这不包括HTTP代理的实际“CONNECT”方法期间的故障。| 支持 | 支持 |
|ERR_MANDATORY_PROXY_CONFIGURATION_FAILED | -131 |  无法使用强制代理配置。目前，这意味着无法获取、解析或执行强制的PAC脚本。| 支持 | 支持 |
|ERR_PRECONNECT_MAX_SOCKET_LIMIT | -133 | 在预连接时，已经达到了套接字池的最大套接字限制。因此不需要尝试预连接更多的套接字。| 支持 | 支持 |
|ERR_SSL_CLIENT_AUTH_PRIVATE_KEY_ACCESS_DENIED | -134 |  使用SSL客户端证书私钥的权限被拒绝。| 支持 | 支持 |
|ERR_SSL_CLIENT_AUTH_CERT_NO_PRIVATE_KEY | -135 | SSL客户端证书没有私钥。| 支持 | 支持 |
|ERR_PROXY_CERTIFICATE_INVALID | -136 |  HTTPS 代理提供的证书无效。 | 支持 | 支持 |
|ERR_NAME_RESOLUTION_FAILED | -137 | 域名解析时出错（DNS）。| 支持 | 支持 |
|ERR_NETWORK_ACCESS_DENIED | -138 | 网络被禁用。 | 支持 | 支持 |
|ERR_TEMPORARILY_THROTTLED | -139 | 因节流而取消了此请求以避免DDOS。| 支持 | 支持 |
|ERR_HTTPS_PROXY_TUNNEL_RESPONSE_REDIRECT | -140 |  通过HTTPS代理创建SSL隧道连接的请求收到302（响应正文可能包括请求失败原因的描述）。| 支持 | 支持 |
|ERR_SSL_CLIENT_AUTH_SIGNATURE_FAILED | -141 | 无法使用客户端证书的私钥对SSL客户端身份验证握手的CertificateVerify数据进行签名。| 支持 | 支持 |
|ERR_MSG_TOO_BIG | -142 | 消息太大，无法传输。（例如，超过大小阈值的UDP消息）。| 支持 | 支持 |
|ERR_WS_PROTOCOL_ERROR | -145 | Websocket协议错误。表示由于帧格式错误或其他协议冲突，正在终止连接。| 支持 | 支持 |
|ERR_ADDRESS_IN_USE | -147 | 尝试绑定已在使用的地址时返回。| 支持 | 支持 |
|ERR_SSL_HANDSHAKE_NOT_COMPLETED | -148 | 由于SSL握手尚未完成，操作失败。| 支持 | 支持 |
|ERR_SSL_BAD_PEER_PUBLIC_KEY | -149 | SSL 公钥错误。| 支持 | 支持 |
|ERR_SSL_PINNED_KEY_NOT_IN_CERT_CHAIN | -150 | 收到的证书与内置域名指定的公钥不匹配。| 支持 | 支持 |
|ERR_CLIENT_AUTH_CERT_TYPE_UNSUPPORTED | -151 | 服务器对客户端证书的请求不包含本机支持的任何类型。| 支持 | 支持 |
|ERR_SSL_DECRYPT_ERROR_ALERT | -153 | SSL对等端向本端发送了致命的decrypt_error警报。当对等方无法正确验证签名（在CertificateVerify或ServerKeyExchange中）或验证Finished消息时，通常会发生这种情况。 | 支持 | 支持 |
|ERR_WS_THROTTLE_QUEUE_TOO_LARGE | -154 | 挂起的WebSocketJob实例太多，因此没有将新Job推送到队列中。 | 支持 | 支持 |
|ERR_SSL_SERVER_CERT_CHANGED | -156 | SSL服务器证书在重新协商中更改。| 支持 | 支持 |
|ERR_SSL_UNRECOGNIZED_NAME_ALERT | -159 | SSL服务器向本端发送了致命的未识别名称警报。| 支持 | 支持 |
|ERR_SOCKET_SET_RECEIVE_BUFFER_SIZE_ERROR | -160 | 未能按请求设置套接字的接收缓冲区大小。| 支持 | 支持 |
|ERR_SOCKET_SET_SEND_BUFFER_SIZE_ERROR | -161 | 未能按请求设置套接字的发送缓冲区大小。| 支持 | 支持 |
|ERR_SOCKET_RECEIVE_BUFFER_SIZE_UNCHANGEABLE | -162 | 尽管setsockopt返回了成功的代码，但未能按请求设置套接字的接收缓冲区大小。| 支持 | 支持 |
|ERR_SOCKET_SEND_BUFFER_SIZE_UNCHANGEABLE | -163 | 尽管setsockopt返回了成功的代码，但未能按请求设置套接字的发送缓冲区大小。| 支持 | 支持 |
|ERR_SSL_CLIENT_AUTH_CERT_BAD_FORMAT | -164 | 无法将客户端证书从平台存储导入SSL库。| 支持 | 支持 |
|ERR_ICANN_NAME_COLLISION | -166 | 将主机名解析为包含IPv4地址“127.0.53.53”的IP地址列表。这是ICANN建议的一个特殊IP地址，用于表示存在名称冲突，并提醒管理员注意潜在问题。| 支持 | 支持 |
|ERR_SSL_SERVER_CERT_BAD_FORMAT | -167 | SSL服务器提供了一个无法解码的证书。这不是证书错误代码，因为没有X509Certificate对象可用。此错误是致命的。| 支持 | 支持 |
|ERR_CT_STH_PARSING_FAILED | -168 | Certificate Transparency：解析signed tree head失败。| 支持 | 支持 |
|ERR_CT_STH_INCOMPLETE | -169 | Certificate Transparency: 解析signed tree head成功，但是缺少了一些信息。 | 支持 | 支持 |
|ERR_UNABLE_TO_REUSE_CONNECTION_FOR_PROXY_AUTH | -170 | 在使用AuthController生成凭据之前，尝试重新使用连接发送代理身份验证凭据失败。调用方应使用新连接重新使用控制器。| 支持 | 支持 |
|ERR_CT_CONSISTENCY_PROOF_PARSING_FAILED | -171 | Certificate Transparency: 一致性验证解析失败。 | 支持 | 支持 |
|ERR_SSL_OBSOLETE_CIPHER | -172 | SSL服务器需要一个不受支持的密码套件，该套件已被删除。此错误将在密码套件删除后立即在一个或两个版本的回退中临时发出信号，之后回退将被删除。| 支持 | 支持 |
|ERR_WS_UPGRADE | -173 | 当WebSocket握手成功完成并且连接已升级时，URLRequest将被取消，并返回此错误代码。| 支持 | 支持 |
|ERR_READ_IF_READY_NOT_IMPLEMENTED | -174 | 套接字ReadIfReady支持未实现。 | 支持 | 支持 |
|ERR_NO_BUFFER_SPACE | -176 | 没有可用的套接字缓冲区空间。 | 支持 | 支持 |
|ERR_SSL_CLIENT_AUTH_NO_COMMON_ALGORITHMS | -177 | 在本端的客户端证书私钥和服务器的首选项之间没有共同的签名算法。| 支持 | 支持 |
|ERR_EARLY_DATA_REJECTED | -178 | TLS 1.3 early data 被服务器拒绝。这将在从套接字返回任何数据之前接收。应在禁用early data的情况下重试请求。| 支持 | 支持 |
|ERR_WRONG_VERSION_ON_EARLY_DATA | -179 | TLS 1.3 early data 版本错误。| 支持 | 支持 |
|ERR_TLS13_DOWNGRADE_DETECTED | -180 | TLS 1.3已启用，但已协商更低的版本，服务器返回一个值，表示它支持TLS 1.3。这是TLS 1.3中安全检查的一部分，但也可能表明用户使用了一个有问题的TLS-terminating代理。| 支持 | 支持 |
|ERR_SSL_KEY_USAGE_INCOMPATIBLE | -181 | 服务器的证书具有与协商的TLS密钥交换方法不兼容的keyUsage扩展。 | 支持 | 支持 |
|ERR_INVALID_ECH_CONFIG_LIST | -182 | 无法解析通过DNS获取的ECHConfigList。| 支持 | 支持 |
|ERR_ECH_NOT_NEGOTIATED | -183 | ECH已启用，但服务器无法解密加密的ClientHello。| 支持 | 支持 |
|ERR_ECH_FALLBACK_CERTIFICATE_INVALID | -184 | ECH已启用，服务器无法解密加密的ClientHello，并且没有提供对公用名称有效的证书。| 支持 | 支持 |
|ERR_CERT_COMMON_NAME_INVALID | -200 | 服务器响应时使用的证书的公用名称与主机名不匹配。这可能意味着：1.攻击者已将本端的流量重定向到他们的服务器，并提供了他们知道私钥的证书。2.服务器配置错误，使用错误的证书进行响应。3.用户在无线网络上，并被重定向到网络的登录页面。4.操作系统使用了DNS搜索后缀，服务器在地址栏中没有缩写名称的证书。| 支持 | 支持 |
|ERR_CERT_DATE_INVALID | -201 | 证书已过期。这可能意味着：1.攻击者正在出示一个旧证书，他们已设法获得该证书的私钥。2.服务器配置错误，没有提供有效的证书。3.设备的时钟错了。 | 支持 | 支持 |
|ERR_CERT_AUTHORITY_INVALID | -202 | 未信任的证书签发机构。这可能意味着：1.攻击者用真实证书代替了包含其公钥并由其表亲签名的证书。2.服务器操作员拥有来自本端不知道但应该信任的CA的合法证书。3.服务器提供了一个自签名证书，无法抵御主动攻击者。 | 支持 | 支持 |
|ERR_CERT_CONTAINS_ERRORS | -203 | 证书中包含错误。 | 支持 | 支持 |
|ERR_CERT_NO_REVOCATION_MECHANISM | -204 |证书无撤销机制，实际上，此证书不能被吊销。|支持|支持|
|ERR_CERT_UNABLE_TO_CHECK_REVOCATION | -205 | 此网站的安全证书的吊销信息不可用。这可能意味着：1。一个攻击者破坏了证书中的私钥，并阻止了本端发现证书已被吊销的尝试。2.证书未被吊销，但吊销服务器正忙或不可用。| 支持 | 支持 |
|ERR_CERT_REVOKED | -206 | 证书已撤销。本端有能力忽略这个错误，但这可能不是本端应该做的事情。| 支持 | 支持 |
|ERR_CERT_INVALID | -207 |  证书非法。  |  支持  |  支持  |
|ERR_CERT_WEAK_SIGNATURE_ALGORITHM | -208 | 服务器以使用弱签名算法签名的证书作为响应。| 支持 | 支持 |
|ERR_CERT_NON_UNIQUE_NAME | -210 |  证书中指定的域名不是唯一的。| 支持 | 支持 |
|ERR_CERT_WEAK_KEY | -211 | 服务器以包含弱密钥（例如，RSA密钥太小）的证书作为响应。| 支持 | 支持 |
|ERR_CERT_NAME_CONSTRAINT_VIOLATION | -212 | 违反证书名称约束。 | 支持 | 支持 |
|ERR_CERT_VALIDITY_TOO_LONG | -213 | 证书有效期过长。 | 支持 | 支持 |
|ERR_CERTIFICATE_TRANSPARENCY_REQUIRED | -214 |  此连接需要Certificate Transparency，但服务器未提供符合策略的CT信息。| 支持 | 支持 |
|ERR_CERT_SYMANTEC_LEGACY | -215 | 该证书链接到不再受信任的旧赛门铁克根目录。 | 支持 | 支持 |
|ERR_CERT_KNOWN_INTERCEPTION_BLOCKED | -217 | 已知该证书被设备所有者以外的实体用于拦截。| 支持 | 支持 |
|ERR_SSL_OBSOLETE_VERSION_OR_CIPHER | -218 | 连接使用过时版本的 SSL/TLS 或密码。| 支持 | 支持 |
|ERR_CERT_END | -219 | 紧跟在上一个证书错误代码之后的值。| 支持 | 支持 |
|ERR_INVALID_URL | -300 | 非法 url。 | 支持 | 支持 |
|ERR_DISALLOWED_URL_SCHEME | -301 |  不允许使用的URL scheme。 | 支持 | 支持 |
|ERR_UNKNOWN_URL_SCHEME | -302 | 未知 scheme。 | 支持 | 支持 |
|ERR_INVALID_REDIRECT | -303 | 试图加载URL导致重定向到无效的URL。| 支持 | 支持 |
|ERR_TOO_MANY_REDIRECTS | -310 | 重定向过多。 | 支持 | 支持 |
|ERR_UNSAFE_REDIRECT | -311 | 不安全的重定向。| 支持 | 支持 |
|ERR_UNSAFE_PORT | -312 | 不安全的端口。 | 支持 | 支持 |
|ERR_INVALID_RESPONSE | -320 | 非法返回。 | 支持 | 支持 |
|ERR_INVALID_CHUNKED_ENCODING | -321 | 分块传输编码错误。| 支持 | 支持 |
|ERR_METHOD_UNSUPPORTED | -322 |	方法不支持。 |	支持 |	支持 |
|ERR_UNEXPECTED_PROXY_AUTH | -323 | 意外的代理身份验证。 | 支持 | 支持 |
|ERR_EMPTY_RESPONSE | -324 | 空返回错误。 | 支持 | 支持 |
|ERR_RESPONSE_HEADERS_TOO_BIG | -325 | 返回体中 headers 太大。 | 支持 | 支持 |
|ERR_PAC_SCRIPT_FAILED | -327 | pac 脚本错误。 | 支持 | 支持 |
|ERR_REQUEST_RANGE_NOT_SATISFIABLE | -328 | 请求范围不可满足。 | 支持 | 支持 |
|ERR_MALFORMED_IDENTITY | -329 | 用于身份验证的标识无效。| 支持 | 支持 |
|ERR_CONTENT_DECODING_FAILED | -330 | 响应正文的内容解码失败。| 支持 | 支持 |
|ERR_NETWORK_IO_SUSPENDED | -331 | 操作无法完成，因为所有网络IO都已挂起。| 支持 | 支持 |
|ERR_SYN_REPLY_NOT_RECEIVED | -332 |	在流上未接收到SYN_REPLY的情况下接收到的FLIP数据。| 支持 | 支持 |
|ERR_ENCODING_CONVERSION_FAILED | -333 | 将响应转换为目标编码失败。| 支持 | 支持 |
|ERR_UNRECOGNIZED_FTP_DIRECTORY_LISTING_FORMAT | -334 | 无法识别的 ftp 目录列表格式。 | 支持 | 支持 |
|ERR_NO_SUPPORTED_PROXIES | -336 | 没有支持的代理。 | 支持 | 支持 |
|ERR_HTTP2_PROTOCOL_ERROR | -337 | HTTP/2协议错误。 | 支持 | 支持 |
|ERR_INVALID_AUTH_CREDENTIALS | -338 | 无效的身份验证凭据。| 支持 | 支持 |
|ERR_UNSUPPORTED_AUTH_SCHEME | -339 | 不支持的身份验证方案。 | 支持 | 支持 |
|ERR_ENCODING_DETECTION_FAILED | -340 | 编码检测失败。 | 支持 | 支持 |
|ERR_MISSING_AUTH_CREDENTIALS | -341 | 缺少身份验证凭据。 | 支持 | 支持 |
|ERR_UNEXPECTED_SECURITY_LIBRARY_STATUS | -342 |	意外的安全库状态。 |	支持 |	支持 |
|ERR_MISCONFIGURED_AUTH_ENVIRONMENT | -343 | 配置错误的身份验证环境。 | 支持 | 支持 |
|ERR_UNDOCUMENTED_SECURITY_LIBRARY_STATUS | -344 | 取消文档安全库状态。 | 支持 | 支持 |
|ERR_RESPONSE_BODY_TOO_BIG_TO_DRAIN | -345 | 响应体太大。| 支持 | 支持 |
|ERR_RESPONSE_HEADERS_MULTIPLE_CONTENT_LENGTH | -346 | 响应 headers 多个内容长度。 | 支持 | 支持 |
|ERR_INCOMPLETE_HTTP2_HEADERS | -347 | 不完整的 HTTP/2 headers。 | 支持 | 支持 |
|ERR_PAC_NOT_IN_DHCP | -348 | pac 不在 dhcp 中。 | 支持 | 支持 |
|ERR_RESPONSE_HEADERS_MULTIPLE_CONTENT_DISPOSITION | -349 | 响应标头多重内容处置。| 支持 | 支持 |
|ERR_RESPONSE_HEADERS_MULTIPLE_LOCATION | -350 | 多个位置的响应标头。 | 支持 | 支持 |
|ERR_HTTP2_SERVER_REFUSED_STREAM | -351 | HTTP/2 服务器拒绝流。| 支持 | 支持 |
|ERR_HTTP2_PING_FAILED | -352 |  HTTP/2 ping 失败。 | 支持 | 支持 |
|ERR_CONTENT_LENGTH_MISMATCH | -354 | 当连接关闭时，HTTP 响应主体传输的字节数少于 Content-Length 头中公布的字节数。 | 支持 | 支持 |
|ERR_INCOMPLETE_CHUNKED_ENCODING | -355 | HTTP 响应体使用分块编码传输，但在连接关闭时从未发送终止零长度块。| 支持 | 支持 |
|ERR_QUIC_PROTOCOL_ERROR | -356 | 存在QUIC协议错误。 | 支持 | 支持 |
|ERR_RESPONSE_HEADERS_TRUNCATED | -357 | HTTP 头部信息被文件结束符（EOF）截断。 | 支持 | 支持 |
|ERR_QUIC_HANDSHAKE_FAILED | -358 | QUIC 加密握手失败。这意味着服务器无法读取发送的任何请求，因此它们可能会被重新发送。 | 支持 | 支持 |
|ERR_HTTP2_INADEQUATE_TRANSPORT_SECURITY | -360 | 传输安全性不适合HTTP/2版本。 | 支持 | 支持 |
|ERR_HTTP2_FLOW_CONTROL_ERROR | -361 | 对等方违反了HTTP/2流控制。| 支持 | 支持 |
|ERR_HTTP2_FRAME_SIZE_ERROR | -362 | 对等方发送了大小不正确的HTTP/2帧。 | 支持 | 支持 |
|ERR_HTTP2_COMPRESSION_ERROR | -363 | 压缩HTTP/2 头部信息的解码或编码失败。| 支持 | 支持 |
|ERR_PROXY_AUTH_REQUESTED_WITH_NO_CONNECTION | -364 | 请求的代理身份验证没有有效的客户端套接字句柄。| 支持 | 支持 |
|ERR_HTTP_1_1_REQUIRED | -365 | 在 HTTP/2 会话中收到 HTTP_1_1_REQUIRED 错误代码。 | 支持 | 支持 |
|ERR_PROXY_HTTP_1_1_REQUIRED | -366 | 在通过 HTTP/2 会话代理时收到 HTTP_1_1_REQUIRED 错误代码。 | 支持 | 支持 |
|ERR_PAC_SCRIPT_TERMINATED | -367 | PAC 脚本已终止并必须重新加载。 | 支持 | 支持 |
|ERR_INVALID_HTTP_RESPONSE | -370 | 服务器应返回 HTTP/1.x 响应，但未返回。而不是将其视为 HTTP/0.9，返回此错误。| 支持 | 支持 |
|ERR_CONTENT_DECODING_INIT_FAILED | -371 | 内容解码初始化失败。 | 支持 | 支持 |
|ERR_HTTP2_RST_STREAM_NO_ERROR_RECEIVED | -372 | 收到带有 NO_ERROR 错误代码的 HTTP/2 RST_STREAM 帧。此错误应由 HTTP/2 代码内部处理，而不应超过 SpdyStream 层。 | 支持 | 支持 |
|ERR_HTTP2_PUSHED_STREAM_NOT_AVAILABLE | -373 | 请求声明的推送流不再可用。 | 支持 | 支持 |
|ERR_HTTP2_CLAIMED_PUSHED_STREAM_RESET_BY_SERVER | -374 | 已声明推送的流，随后服务器将其重置。发生这种情况时，应该重试请求。| 支持 | 支持 |
|ERR_TOO_MANY_RETRIES | -375 | 由于身份验证或证书无效，HTTP事务重试次数过多。| 支持 | 支持 |
|ERR_HTTP2_STREAM_CLOSED | -376 | 在已关闭的流上收到一个 HTTP/2 帧。| 支持 | 支持 |
|ERR_HTTP2_CLIENT_REFUSED_STREAM | -377 | 客户端拒绝了一个 HTTP/2 流。| 支持 | 支持 |
|ERR_HTTP2_PUSHED_RESPONSE_DOES_NOT_MATCH | -378 | 基于匹配的 URL 和请求头，一个 HTTP/2 推送的流被请求所接收，但是推送的响应头并不匹配请求。 | 支持 | 支持 |
|ERR_HTTP_RESPONSE_CODE_FAILURE | -379 | 服务器返回了non-2xx的HTTP响应代码。 | 支持 | 支持 |
|ERR_QUIC_UNKNOWN_CERT_ROOT | -380 | 在 QUIC 连接上展示的证书未链接到已知根证书，并且连接到的原始服务器不在允许未知根证书的域名列表中。| 支持 | 支持 |
|ERR_QUIC_GOAWAY_REQUEST_CAN_BE_RETRIED | -381 | 已接收到一个 GOAWAY 帧，表明请求未得到处理，因此可以安全地在不同的连接上重试。| 支持 | 支持 |
|ERR_TOO_MANY_ACCEPT_CH_RESTARTS | -382 | ACCEPT_CH 重启已被触发太多次。| 支持 | 支持 |
|ERR_INCONSISTENT_IP_ADDRESS_SPACE | -383 | 在相同的请求期间，远程端点的 IP 地址空间与先前观察到的值不同。任何受影响的请求的缓存条目都应被标记为无效。| 支持 | 支持 |
|ERR_CACHED_IP_ADDRESS_SPACE_BLOCKED_BY_LOCAL_NETWORK_ACCESS_POLICY | -384 | 缓存的远程端点的 IP 地址空间被本地网络访问检查所阻止。| 支持 | 支持 |
|ERR_CACHE_MISS | -400 |	缓存中没有请求的条目。| 支持 | 支持 |
|ERR_CACHE_READ_FAILURE | -401 | 无法从磁盘缓存中读取。| 支持 | 支持 |
|ERR_CACHE_WRITE_FAILURE | -402 | 无法写入磁盘缓存。| 支持 | 支持 |
|ERR_CACHE_OPERATION_UNSUPPORTED | -403 | 此条目不支持此操作。| 支持 | 支持 |
|ERR_CACHE_OPEN_FAILURE | -404 | 磁盘缓存无法打开此条目。| 支持 | 支持 |
|ERR_CACHE_CREATE_FAILURE | -405 | 磁盘缓存无法创建此条目。| 支持 | 支持 |
|ERR_CACHE_RACE | -406 | 多个事务正在竞相创建磁盘缓存条目。 | 支持 | 支持 |
|ERR_CACHE_CHECKSUM_READ_FAILURE | -407 | 缓存无法读取条目上的校验和记录。| 支持 | 支持 |
|ERR_CACHE_CHECKSUM_MISMATCH | -408 | 缓存发现一个具有无效校验和的条目。 | 支持 | 支持 |
|ERR_CACHE_LOCK_TIMEOUT | -409 | HTTP缓存的内部错误代码。| 支持 | 支持 |
|ERR_CACHE_AUTH_FAILURE_AFTER_READ | -410 | 在事务读取某些数据后收到质询，但凭据不可用。 | 支持 | 支持 |
|ERR_CACHE_ENTRY_NOT_SUITABLE | -411 | HTTP缓存的内部不完全错误代码| 支持 | 支持 |
|ERR_CACHE_DOOM_FAILURE | -412 | 磁盘缓存无法删除此条目。| 支持 | 支持 |
|ERR_CACHE_OPEN_OR_CREATE_FAILURE | -413 | 磁盘缓存无法打开或创建此条目。| 支持 | 支持 |
|ERR_INSECURE_RESPONSE | -501 | 服务器的响应不安全（例如，存在证书错误）。| 支持 | 支持 |
|ERR_NO_PRIVATE_KEY_FOR_CERT | -502 | 尝试导入客户端证书失败，因为用户的密钥数据库缺少相应的私钥。| 支持 | 支持 |
|ERR_ADD_USER_CERT_FAILED | -503 | 向操作系统证书数据库添加证书时发生错误。| 支持 | 支持 |
|ERR_INVALID_SIGNED_EXCHANGE | -504 |处理已签名的交换时发生错误。 |支持 |支持 |
|ERR_INVALID_WEB_BUNDLE | -505 | 处理Web Bundle源时发生错误。 | 支持 | 支持 |
|ERR_TRUST_TOKEN_OPERATION_FAILED | -506 | 执行Trust Tokens协议操作的请求失败（原因包括：预置条件失败、内部错误、不良响应）。| 支持 | 支持 |
|ERR_TRUST_TOKEN_OPERATION_SUCCESS_WITHOUT_SENDING_REQUEST | -507 | 在处理一个与Trust Tokens协议相关的操作执行请求时，系统能够执行该请求中的Trust Tokens操作，但并没有将请求发送到其指定的目的地。| 支持 | 支持 |
|ERR_FTP_FAILED | -601 | FTP控制连接命令失败的通用错误。 | 支持 | 支持 |
|ERR_FTP_SERVICE_UNAVAILABLE | -602 | 服务器目前无法满足请求。这是一个临时错误。FTP响应代码421。| 支持 | 支持 |
|ERR_FTP_TRANSFER_ABORTED | -603 | 服务器已中止传输。FTP响应代码426。| 支持 | 支持 |
|ERR_FTP_FILE_BUSY | -604 | 文件正在使用中，或在打开文件时发生了一些其他临时错误条件。FTP响应代码450。| 支持 | 支持 |
|ERR_FTP_SYNTAX_ERROR | -605 | 由于语法错误，服务器拒绝了本端的命令。FTP响应代码500、501。| 支持 | 支持 |
|ERR_FTP_COMMAND_UNSUPPORTED | -606 | 服务器不支持本端发出的命令。FTP响应代码502、504。| 支持 | 支持 |
|ERR_FTP_BAD_COMMAND_SEQUENCE | -607 | 服务器拒绝了本端的命令，因为本端没有按照正确的顺序发出命令。FTP响应代码503。| 支持 | 支持 |
|ERR_PKCS12_IMPORT_BAD_PASSWORD | -701 | 由于密码不正确，PKCS #12 导入失败。 | 支持 | 支持 |
|ERR_PKCS12_IMPORT_FAILED | -702 | 由于其他错误，PKCS #12 导入失败。| 支持 | 支持 |
|ERR_IMPORT_CA_CERT_NOT_CA | -703 | CA导入失败-不是CA证书。| 支持 | 支持 |
|ERR_IMPORT_CERT_ALREADY_EXISTS | -704 | 导入失败-数据库中已存在证书。| 支持 | 支持 |
|ERR_IMPORT_CA_CERT_FAILED | -705 | 由于其他错误，CA导入失败。| 支持 | 支持 |
|ERR_IMPORT_SERVER_CERT_FAILED | -706 | 由于某些内部错误，服务器证书导入失败。| 支持 | 支持 |
|ERR_PKCS12_IMPORT_INVALID_MAC | -707 | PKCS #12 导入失败，因为 MAC（消息认证码）无效。| 支持 | 支持 |
|ERR_PKCS12_IMPORT_INVALID_FILE | -708 | PKCS #12 导入失败，因为文件无效或已损坏。| 支持 | 支持 |
|ERR_PKCS12_IMPORT_UNSUPPORTED | -709 | 由于不支持的特性，PKCS #12 导入失败。| 支持 | 支持 |
|ERR_KEY_GENERATION_FAILED | -710 | 密钥生成失败。 | 支持 | 支持 |
|ERR_PRIVATE_KEY_EXPORT_FAILED | -712 | 无法导出私钥。| 支持 | 支持 |
|ERR_SELF_SIGNED_CERT_GENERATION_FAILED | -713 | 自签名证书生成失败。| 支持 | 支持 |
|ERR_CERT_DATABASE_CHANGED | -714 | 证书数据库已发生某种更改。| 支持 | 支持 |
|ERR_CERT_VERIFIER_CHANGED | -716 | 证书验证配置已发生某种更改。| 支持 | 支持 |
|ERR_DNS_MALFORMED_RESPONSE | -800 | DNS解析程序收到格式错误的响应。| 支持 | 支持 |
|ERR_DNS_SERVER_REQUIRES_TCP | -801 | DNS服务器需要TCP。| 支持 | 支持 |
|ERR_DNS_SERVER_FAILED | -802 | DNS服务器失败。对于以下所有错误情况，都会返回此错误。1-格式错误-名称服务器无法解释查询。2-服务器故障-名称服务器由于自身问题无法处理这个查询。3-未实现-名称服务器不支持请求的查询类型。4-拒绝-名称服务器出于策略原因拒绝执行指定的操作。| 支持 | 支持 |
|ERR_DNS_TIMED_OUT | -803 | DNS事务超时。| 支持 | 支持 |
|ERR_DNS_CACHE_MISS | -804 | 对于只查询本地源的查找，在缓存或其他本地源中未找到该条目。| 支持 | 支持 |
|ERR_DNS_SEARCH_EMPTY | -805 | 后缀搜索列表规则阻止了给定主机名的解析。| 支持 | 支持 |
|ERR_DNS_SORT_ERROR | -806 |	未能根据RFC3484对地址进行排序。| 支持 | 支持 |
|ERR_DNS_SECURE_RESOLVER_HOSTNAME_RESOLUTION_FAILED | -808 | 未能解析DNS-over-HTTPS服务器的主机名。| 支持 | 支持 |
|ERR_DNS_NAME_HTTPS_ONLY | -809 | DNS已识别请求因不安全的连接（http/ws）而被禁止。应用程序应该像处理HTTP重定向一样处理这个错误，将连接重定向到安全的https或wss。| 支持 | 支持 |
|ERR_DNS_REQUEST_CANCELED | -810 | 与此任务相关的所有 DNS 请求已被取消。| 支持 | 支持 |
|ERR_DNS_NO_MATCHING_SUPPORTED_ALPN | -811 | HTTPS记录的主机名解析预期未能使用受支持协议的ALPN值进行解析。| 支持 | 支持 |