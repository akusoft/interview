# 微服务

## SSO和CAS

&emsp;&emsp;SSO只是一种单点登录的思想，所谓单点登录，就是同平台的诸多应用登陆一次，下一次就免登陆的功能。就像你在知乎首页登录一次，下一次再访问知乎专栏或是知乎日报就可以免去登录操作。

&emsp;&emsp;实现SSO的方式有很多，现在主流的就是CAS这种基于session的单点登录形式。

&emsp;&emsp; CAS分为两部分，CAS Server和CAS Client

- CAS Server用来负责用户的认证工作，就像是把第一次登录用户的一个标识存在这里，以便此用户在其他系统登录时验证其需不需要再次登录。
- CAS Client就是我们自己的应用，需要接入CAS Server端。当用户访问我们的应用时，首先需要重定向到CAS Server端进行验证，要是原来登陆过，就免去登录，重定向到下游系统，否则进行用户名密码登陆操作。

### 术语
&emsp;&emsp;Ticket Granting ticket (TGT) ：可以认为是CAS Server根据用户名密码生成的一张票，存在server端

&emsp;&emsp;Ticket-granting cookie (TGC) ：其实就是一个cookie，存放用户身份信息，由server发给client端

&emsp;&emsp;Service ticket (ST) ：由TGT生成的一次性票据，用于验证，只能用一次。相当于server发给client一张票，然后client拿着这是个票再来找server验证，看看是不是server签发的。就像是我给了你一张我的照片，然后你拿照片再来问我，这个照片是不是你。。。没错，就是这么无聊。

&emsp;&emsp;1. 用户访问网站，第一次来，重定向到 CAS Server，发现没有cookie，所以再重定向到CAS Server端的登录页面，并且URL带有网站地址，便于认证成功后跳转

形如 http ://cas-server:8100/login?service=http ://localhost:8081

service后面这个地址就是登录成功后要重定向的下游系统URL

&emsp;&emsp;2. 在登陆页面输入用户名密码认证，认证成功后cas-server生成TGT，再用TGT生成一个ST。 然后再第三次重定向并返回ST和cookie(TGC)到浏览器


&emsp;&emsp;3. 浏览器带着ST再访问想要访问的地址

http ://localhost:8081/?ticket=ST-25939-sqbDVZcuSvrvBC6MQlg5

ticket后面那一串就是ST


&emsp;&emsp;4. 浏览器的服务器收到ST后再去cas-server验证一下是否为自己签发的，验证通过后就会显示页面信息，也就是重定向到第1步service后面的那个URL

首次登陆完毕


&emsp;&emsp;5. 再登陆另一个接入CAS的网站，重定向到CAS Server，server判断是第一次来（但是此时有TGC，也就是cookie，所以不用去登陆页面了），但此时没有ST，去cas-server申请一个吧！于是重定向到cas-server

形如：http: //cas-server:8100/login?service=http ://localhost:8082 && TGC(cookie) (传目标地址和cookie）


&emsp;&emsp;6. cas-server生成了ST后重定向给浏览器

http ://localhost:8082/?ticket=ST-25939-sqfsafgefesaedswqqw5-xxxx


&emsp;&emsp;7. 浏览器的服务器收到ST后再去cas-server验证一下是否为自己签发的，验证通过后就会显示页面信息（同第4步）


