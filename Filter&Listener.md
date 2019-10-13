# Filter

步骤：
  		1. 定义一个类，实现接口Filter
 		2. 复写方法
		3. 配置拦截路径
  1. web.xml
  2. 注解

```java
@WebFilter("/*")//访问所有资源之前，都会执行该过滤器
```

```java
package cn.itcast.web.filter;

import javax.servlet.*;
import java.io.IOException;


//浏览器直接请求所有资源或者转发访问index.jsp。该过滤器才会被执行
@WebFilter(value="/*",dispatcherTypes ={ DispatcherType.FORWARD,DispatcherType.REQUEST})
public class FilterDemo5 implements Filter {


    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        System.out.println("filterDemo5....");
        chain.doFilter(req, resp);
    }

    public void init(FilterConfig config) throws ServletException {

    }

    public void destroy() {
    }

}

```

## web.xml配置

```java
web.xml配置	
		<filter>
	        <filter-name>demo1</filter-name>
	        <filter-class>cn.itcast.web.filter.FilterDemo1</filter-class>
	    </filter>
	    <filter-mapping>
	        <filter-name>demo1</filter-name>
			<!-- 拦截路径 -->
	        <url-pattern>/*</url-pattern>
	    </filter-mapping>
```

## 过滤器执行流程

1. 执行过滤器
2. 执行放行后的资源
3. 回来执行过滤器放行代码下边的代码

## 过滤器生命周期方法

1. init:在服务器启动后，会创建Filter对象，然后调用init方法。只执行一次。用于加载资源
2. doFilter:每一次请求被拦截资源时，会执行。执行多次
3. destroy:在服务器关闭后，Filter对象被销毁。如果服务器是正常关闭，则会执行destroy方法。只执行一次。用于释放资源

## 拦截路径配置

- 具体资源路径： /index.jsp   只有访问index.jsp资源时，过滤器才会被执行
- 拦截目录： /user/*	访问/user下的所有资源时，过滤器都会被执行
- 后缀名拦截： *.jsp		访问所有后缀名为jsp资源时，过滤器都会被执行
- 拦截所有资源：/*		访问所有资源时，过滤器都会被执行

拦截方式配置：资源被访问的方式

注解配置：

​	设置dispatcherTypes属性

- REQUEST：默认值。浏览器直接请求资源
- FORWARD：转发访问资源
- INCLUDE：包含访问资源
- ERROR：错误跳转资源
- ASYNC：异步访问资源

web.xml配置

 * 设置<dispatcher></dispatcher>标签即可

## 过滤器先后顺序

注解配置：按照类名的字符串比较规则比较，值小的先执行
   * 如： AFilter 和 BFilter，AFilter就先执行了。

web.xml配置： <filter-mapping>谁定义在上边，谁先执行

# 动态代理(设计模式)

实现步骤：

1. 代理对象和真实对象实现相同的接口
2. 代理对象 = Proxy.newProxyInstance();
3. 使用代理对象调用方法。
4. 增强方法

					* 增强方式：
	
	增强参数列表
	
	增强返回值类型
	
	增强方法体执行逻辑

```java
package cn.itcast.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyTest {

    public static void main(String[] args) {
        //1.创建真实对象
        Lenovo lenovo = new Lenovo();
        
        //2.动态代理增强lenovo对象
        /*
            三个参数：
                1. 类加载器：真实对象.getClass().getClassLoader()
                2. 接口数组：真实对象.getClass().getInterfaces()
                3. 处理器：new InvocationHandler()
         */
        SaleComputer proxy_lenovo = (SaleComputer) Proxy.newProxyInstance(lenovo.getClass().getClassLoader(), lenovo.getClass().getInterfaces(), new InvocationHandler() {

            /*
                代理逻辑编写的方法：代理对象调用的所有方法都会触发该方法执行
                    参数：
                        1. proxy:代理对象
                        2. method：代理对象调用的方法，被封装为的对象
                        3. args:代理对象调用的方法时，传递的实际参数
             */
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                /*System.out.println("该方法执行了....");
                System.out.println(method.getName());
                System.out.println(args[0]);




*/
                //判断是否是sale方法
                if(method.getName().equals("sale")){
                    //1.增强参数
                    double money = (double) args[0];
                    money = money * 0.85;
                    System.out.println("专车接你....");
                    //使用真实对象调用该方法
                    String obj = (String) method.invoke(lenovo, money);
                    System.out.println("免费送货...");
                    //2.增强返回值
                    return obj+"_鼠标垫";
                }else{
                    Object obj = method.invoke(lenovo, args);
                    return obj;
                }



            }
        });

        //3.调用方法

       /* String computer = proxy_lenovo.sale(8000);
        System.out.println(computer);*/

        proxy_lenovo.show();
    }
}

```

# Listener

```
* ServletContextListener:监听ServletContext对象的创建和销毁
		* void contextDestroyed(ServletContextEvent sce) ：ServletContext对象被销毁之前会调用该方法
		* void contextInitialized(ServletContextEvent sce) ：ServletContext对象创建后会调用该方法
	* 步骤：
		1. 定义一个类，实现ServletContextListener接口
		2. 复写方法
		3. 配置
			1. web.xml
<listener>
    <listener-class>cn.itcast.web.listener.ContextLoaderListener</listener-class>
</listener>
			2. 注解：
				* @WebListener	
```

## code

```xml
   <!-- web.xml配置,指定初始化参数 -->

   <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/classes/applicationContext.xml</param-value>
   </context-param>
```



```java
package cn.itcast.web.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;
import java.io.FileInputStream;


@WebListener
public class ContextLoaderListener implements ServletContextListener {

    /**
     * 监听ServletContext对象创建的。ServletContext对象服务器启动后自动创建。
     *
     * 在服务器启动后自动调用
     * @param servletContextEvent
     */
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        //加载资源文件
        //1.获取ServletContext对象
        ServletContext servletContext = servletContextEvent.getServletContext();

        //2.加载资源文件
        String contextConfigLocation = servletContext.getInitParameter("contextConfigLocation");

        //3.获取真实路径
        String realPath = servletContext.getRealPath(contextConfigLocation);

        //4.加载进内存
        try{
            FileInputStream fis = new FileInputStream(realPath);
            System.out.println(fis);
        }catch (Exception e){
            e.printStackTrace();
        }
        System.out.println("ServletContext对象被创建了。。。");
    }

    /**
     * 在服务器关闭后，ServletContext对象被销毁。当服务器正常关闭后该方法被调用
     * @param servletContextEvent
     */
    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("ServletContext对象被销毁了。。。");
    }
}

```

