### 获取对象

```
基本操作

获取session对象
HttpSession httpsession = request.getSession();

设置属性
httpSession.setAttribute("name","我真NB");

获得属性
String value = (String)httpSession.getAttribute("name");

```

### 生命周期和有效期

- 第一次访问Servlet,jsp等动态资源就自动创建(静态资源不会创建)

- Session创建后,用户继续访问,服务器就更新最后活跃时间

```java
//Session活跃时间修改
//在tomcat/conf/web.xml文件中设置，时间值为20分钟，所有的WEB应用都有效
//在单个的web.xml文件中设置，对单个web应用有效，如果有冲突，以自己的web应用为准。
<session-config>
    <session-timeout>20</session-timeout>
</session-config>  

//通过setMaxInactiveInterval()方法设置,单位秒
httpSession.setMaxInactiveInterval(60);

//使用invalidate()可以让session所有属性失效

//removeAttribute()让某个方法失效
```

### Session执行原理

浏览器访问Servlet,服务器自动创建Session,并自动发送Cookie给浏览器.此后,服务器依靠浏览器的Cookie来识别不同的浏览器.

### 浏览器禁用Cookie

服务器自动识别浏览器有没有禁用Cookie,

