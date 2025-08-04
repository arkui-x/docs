# http请求配置CA证书提示本地SSL证书错误

**【问题现象】**

http请求带本地CA证书，

```java
const client = http.createHttp()
client.request('https://xxxxxx', {
    caPath: ctx.cacheDir + '/ca.pem',
    clientCert:{
        certPath: ctx.cacheDir + '/niceloo.p12',
        keyPath: ctx.cacheDir + '/tls.bks',
        keyPassword: 'xxxxxx',
        certType: http.CertType.P12               
    }
})

 ```
请求失败，失败错误码{"code":2300058,"message":"Problem with the local SSL certificate"}


**【解决方法】**

http使用BoringSSL进行证书校验，对证书校验比较严格，推荐使用pem证书。可搜索在线转换工具，将p12证书转为pem证书。使用如下，keyPath为转换生成的privatekey。

```javaB
const client = http.createHttp()
client.request('https://xxxxxx', {
    caPath: ctx.cacheDir + '/ca.pem',
    clientCert:{
        certPath: ctx.cacheDir + '/cert.pem',
        keyPath: ctx.cacheDir + '/privatekey.pem',
        certType: http.CertType.PEM
    }
})

 ```
