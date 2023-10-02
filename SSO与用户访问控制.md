#wip #frontend #案例梳理 
用户访问控制主要解决的是让第三方代理用户取得资源的问题，常见在SSO登入的场景中。
# OAuth 2
1. Authorization code 用于服务端应用，先签发authorization code再签发access token
2. Implicit Flow 用于前端应用（静态或SPA), 之间将access token签发到前端
3. Resource Owner Password Credentials 直接取得用户的账号密码，由应用使用
4. **Client Credentials Flow** 用于和使用者无关，但需要验证应用本身的场景
# 参考
https://juejin.cn/post/6844904155161559048
https://medium.com/%E9%BA%A5%E5%85%8B%E7%9A%84%E5%8D%8A%E8%B7%AF%E5%87%BA%E5%AE%B6%E7%AD%86%E8%A8%98/%E7%AD%86%E8%A8%98-%E8%AA%8D%E8%AD%98-oauth-2-0-%E4%B8%80%E6%AC%A1%E4%BA%86%E8%A7%A3%E5%90%84%E8%A7%92%E8%89%B2-%E5%90%84%E9%A1%9E%E5%9E%8B%E6%B5%81%E7%A8%8B%E7%9A%84%E5%B7%AE%E7%95%B0-c42da83a6015