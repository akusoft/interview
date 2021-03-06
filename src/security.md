# 安全

此节主要记录与安全相关的知识点。

## XSS

**XSS**（cross-site scripting）即跨站脚本攻击，通过 Web 站点漏洞，向客户端交付恶意脚本代码，实现对客户端的攻击目的。

攻击的方向有盗取 cookie（`document.cookie`）、重定向（`window.location`）等。

测试 XSS：

```html
<script>alert('XSS');</script>
```

XSS 漏洞形成的根源是服务器对用户提交数据过滤不严且对输出信息未做过滤。

XSS 具体可分为：

- **反射型 XSS**（非持久）：恶意脚本上传到服务器上，并原封不动返回，服务器不存储
- **存储型 XSS**（持久）：恶意脚本上传且存储到服务器上。常见于留言板等。
- **DOM 型**：恶意脚本不上传到服务器

要预防 XSS 可以对输出信息做 HTML 编码，这会对某些特殊字符（如 `<`、`>`、`(`、`)`、`'`、`"`、`/`、`\`、`=`）做编码。

## CSRF

**CSRF**（Cross-site request forgery）即**跨站请求伪造**，在用户非自愿、不知情的情况下提交请求。比如，修改账号密码、个人信息（email、收获地址），发送伪造的业务请求（网银、购物、投票），关注他人社交账号、推送博文等。

从信任的角度来区分：

- XSS：利用用户对站点的信任
- CSRF：利用站点对已经身份认证的信任

要预防 CSRF 可以通过以下几点：

- 对关键操作进行二次确认机制，比如修改密码时需要二次确认或验证码等
- 为表单添加隐藏的随机数（anti-CSRF token）
- Referrer 头
