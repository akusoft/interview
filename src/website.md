# 网站

此节主要记录与网站相关的知识点。

## 实现简单的登录逻辑

## JWT

[JWT](https://jwt.io/) 全称 **Json Web Token**，一般用于前后端分离、微信小程序、APP 开发等项目的用户认证。

在上述项目中，通过传统的 Cookie/Session 实现用户认证，显然不行，**基于传统的 Token 认证**则较为可行，
此种认证方式的流程大致为：

用户登录，服务端返回 Token，并将 Token 保存在服务端，以后用户再来访问时，需要携带 Token，服务端获取 Token 后，
再去数据库中获取 Token 并做校验。

JWT 相比这种传统的 Token 认证，其大致流程为：

用户登录，服务端返回 Token，服务端不保存此 Token，以后用户再来访问时，需要携带 Token，服务端获取 Token 后，
直接对 Token 做校验。

显然，相较于传统的 Token 认证，**JWT 的优势在于其无需在服务端保存 Token**。

### JWT 实现过程：

1. 用户提交用户名和密码给服务端，若登录成功，则使用 JWT 创建一个 Token，并返回给用户。其 Token 如下所示：
    ```text
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
    ```
    此 Token 由 3 段字符串组成，并用 `.` 连接起来，具体而言：
    - 第 1 段字符串为 HEADER，内部包含哈希算法和 Token 类型。先将 JSON 转换成字符串，然后做 Base64URL 编码，如下：
        ```json
        {
          "alg": "HS256",
          "typ": "JWT"
        }
        ```
    - 第 2 段字符串为 PAYLOAD，主要为自定义值。先将 JSON 转换成字符串，然后做 Base64URL 编码，如下：
        ```json
        {
          "id": "123456",
          "name": "Howie Zhao",
          "exp": 1516239022  # 超时时间
        }
        ```
    - 第 3 段字符串主要为验证信息，其按照如下步骤生成：
        1. 将第 1 段和第 2 段编码后的字符串拼接起来；
        2. 对拼接起来的字符串进行 `HS256` 哈希加密，**并加盐**；
        3. 对 `HS256` 哈希加密后的密文再做 Base64URL 编码。
2. 以后用户再来访问时，需要携带 Token，后端需要对 Token 进行校验，按如下步骤：
    1. 获取 Token；
    2. 对 Token 进行切割；
    3. 对第 2 段进行 Base64URL 解码，并获取 PAYLOAD 信息，检测 Token 是否超时；
    4. 将第 1 段和第 2 段编码后的字符串拼接，并再次进行 `HS256` 哈希加密，**并加盐**；
    5. 对第 3 段进行 Base64URL 解码，并将解码后的字符串与上一步哈希加密后的字符串进行比较，查看是否相等；
    6. 若相等，则表示 Token 未被修改过，认证通过。
