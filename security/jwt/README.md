istio-jwt-tools 目录拷贝至 https://github.com/istio/istio/tree/master/security/tools/jwt/samples ，这里的key.pem是公网暴露的，切不可用于生产环境。
[Istio 实战：认证--RequestAuthentication](https://potterhe.github.io/posts/istio-in-action-request-authentication/index.html) 有描述 Istio 的一些设计，以及如何生成私有JWKS的一些工具和方法。这里直接使用Istio的。
