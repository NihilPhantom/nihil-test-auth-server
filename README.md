# nihil-test-auth-server

## 项目介绍

本项目为[NIHIL-AUTH-SERVER](https://github.com/NihilPhantom/nihil-auth-server) 的测试项目。

授权服务的搭建过程请参考[NIHIL-AUTH-SERVER 使用说明](https://github.com/NihilPhantom/nihil-auth-server/blob/main/doc/%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md)

在本项目中，通过在 pom.xml 中添加以下内容，引入了对认证服务的支持。
```xml
<dependency>
    <groupId>com.nihil</groupId>
    <artifactId>nihil-auth-server</artifactId>
    <version>1.0-SNAPSHOT</version>
    <scope>compile</scope>
</dependency>
```

在项目运行后，直接访问资源 [http://127.0.0.1:8006/nihil-test-auth-server/test](http://127.0.0.1:8006/nihil-test-auth-server/test)
时，会提示403 的错误。

## 用户登录
使用POST 请求，发送用户名密码给 `/auth/login` 以进行登录，以下为使用案例（可使用[http/test.http](http/test.http)进行测试）
```http request
POST http://127.0.0.1:8006/nihil-test-auth-server/auth/login
Content-Type: application/x-www-form-urlencoded

username=admin&password=123456
```
可以完成登录，获取到用户token。以下为响应结果
```text
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Mon, 19 Feb 2024 00:38:45 GMT
Keep-Alive: timeout=60
Connection: keep-alive

{
  "msg": "成功",
  "code": 200,
  "data": "a526fa6597a84bac8826b256117920d9"
}
```

## 受限资源访问
在请求资源时，添加 `token` 自定义头，以实现权限认证。
（这里的token，可以通过配置 `application.yum` 中的 `nihil-auth.token.header` 进行修改）
请求示例如下（可使用[http/test.http](http/test.http)进行测试）：
```http request
GET http://127.0.0.1:8006/nihil-test-auth-server/test
token: a526fa6597a84bac8826b256117920d9
```