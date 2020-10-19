istio-jwt-tools 目录拷贝至 https://github.com/istio/istio/tree/master/security/tools/jwt/samples ，这里的key.pem是公网暴露的，切不可用于生产环境。
[Istio 实战：认证--RequestAuthentication](https://potterhe.github.io/posts/istio-in-action-request-authentication/index.html) 有描述 Istio 的一些设计，以及如何生成私有JWKS的一些工具和方法。这里直接使用Istio的。

envoy 的配置很多，且主要是面向api而设计的，所以较好的方式是查看 [github.com/envoyproxy/data-plane-api](https://github.com/envoyproxy/data-plane-api) 仓库中 protobuf 的定义，比官方文档更好用。

### 用例：
```sh
$ curl -v localhost:8080
* Rebuilt URL to: localhost:8080/
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> GET / HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
>
< HTTP/1.1 401 Unauthorized
< content-length: 14
< content-type: text/plain
< date: Mon, 19 Oct 2020 10:31:18 GMT
< server: envoy
<
* Connection #0 to host localhost left intact
Jwt is missing
```

```sh
$ TOKEN=$(cat istio-jwt-tools/demo.jwt)
$ curl --header "Authorization: Bearer $TOKEN" localhost:8080/anything
{
  "args": {},
  "data": "",
  "files": {},
  "form": {},
  "headers": {
    "Accept": "*/*",
    "Authorization": "Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IkRIRmJwb0lVcXJZOHQyenBBMnFYZkNtcjVWTzVaRXI0UnpIVV8tZW52dlEiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjQ2ODU5ODk3MDAsImZvbyI6ImJhciIsImlhdCI6MTUzMjM4OTcwMCwiaXNzIjoidGVzdGluZ0BzZWN1cmUuaXN0aW8uaW8iLCJzdWIiOiJ0ZXN0aW5nQHNlY3VyZS5pc3Rpby5pbyJ9.CfNnxWP2tcnR9q0vxyxweaF3ovQYHYZl82hAUsn21bwQd9zP7c-LS9qd_vpdLG4Tn1A15NxfCjp5f7QNBUo-KC9PJqYpgGbaXhaGx7bEdFWjcwv3nZzvc7M__ZpaCERdwU7igUmJqYGBYQ51vr2njU9ZimyKkfDe3axcyiBZde7G6dabliUosJvvKOPcKIWPccCgefSj_GNfwIip3-SsFdlR7BtbVUcqR-yv-XOxJ3Uc1MI0tz3uMiiZcyPV7sNCU4KRnemRIMHVOfuvHsU60_GhGbiSFzgPTAa9WTltbnarTbxudb_YEOx12JiwYToeX0DCPb43W1tzIBxgm8NxUg",
    "Content-Length": "0",
    "Host": "localhost:8080",
    "User-Agent": "curl/7.54.0",
    "X-Envoy-Expected-Rq-Timeout-Ms": "15000",
    "X-Jwt-Payload": "eyJleHAiOjQ2ODU5ODk3MDAsImZvbyI6ImJhciIsImlhdCI6MTUzMjM4OTcwMCwiaXNzIjoidGVzdGluZ0BzZWN1cmUuaXN0aW8uaW8iLCJzdWIiOiJ0ZXN0aW5nQHNlY3VyZS5pc3Rpby5pbyJ9"
  },
  "json": null,
  "method": "GET",
  "origin": "192.168.254.42",
  "url": "http://localhost:8080/anything"
}
```
