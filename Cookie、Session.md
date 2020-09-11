### Cookie、Session

**会话**：用户打开一个浏览器，点开很多链接，访问web资源，关闭浏览器，这个过程称为会话

网站证明你访问过

1、服务端给客户端一个标记，客户端下次访问服务器带上这个标记就好，cookie

2、服务器证明你登录过，下次你再次访问我就可以匹配到你，session

**cookie**

- 客户端技术（响应、请求）

- 从请求中拿到cookie；服务端响应给客户端cookie

  ![1595507496013](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595507496013.png)

  ![1595507992749](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595507992749.png)

**session**

- 服务端技术，利用这个技术我们可以保存用户的会话信息，可以将信息或者数据放在Session中
- 服务器会给每一个用户创建一个session对象
- 一个session独占一个浏览器，只要浏览器没有关，session就存在
- 用户登录之后，整个网站资源都可以访问

cookie和session区别

- cookie将用户的数据写给用户浏览器，浏览器可以保存
- Sssion是将用户的数据写到独占的Session中，服务端保存
- Session对象由服务创建



![1595510595915](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595510595915.png)

![1595510841790](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595510841790.png)

![1595510924141](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595510924141.png)