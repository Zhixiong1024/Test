# IOC

> IOC(Inversion of Control)：其思想是`反转资源获取的方向`. 传统的资源查找方式要求组件向容器发起请求查找资源. 作为回应, 容器适时的返回资源. 而应用了 IOC 之后, 则是`容器主动地将资源推送给它所管理的组件, 组件所要做的仅是选择一种合适的方式来接受资源.` 这种行为也被称为查找的被动形式

# DI

> DI(Dependency Injection) — IOC 的另一种表述方式：即`组件以一些预先定义好的方式(例如: setter 方法)接受来自如容器的资源注入.` 相对于 IOC 而言，这种表述更直接

# 配置 bean

```xml
<!-- 配置一个 bean -->
<bean id="helloWorld" class="com.atguigu.spring.helloworld.HelloWorld">
    <!-- 为属性赋值 -->
    <property name="user" value="Jerry"></property>
</bean>

<!-- 通过构造器注入属性值 -->
<bean id="helloWorld3" class="com.atguigu.spring.helloworld.HelloWorld">
    <!-- 要求: 在 Bean 中必须有对应的构造器.  -->
    <constructor-arg value="Mike"></constructor-arg>
</bean>

<!-- 若一个 bean 有多个构造器, 如何通过构造器来为 bean 的属性赋值 -->
<!-- 可以根据 index 和 value 进行更加精确的定位. (了解) -->
<bean id="car" class="com.atguigu.spring.helloworld.Car">
    <constructor-arg value="KUGA" index="1"></constructor-arg>
    <constructor-arg value="ChangAnFord" index="0"></constructor-arg>
    <constructor-arg value="250000" type="float"></constructor-arg>
</bean>



<bean id="car2" class="com.atguigu.spring.helloworld.Car">
		<constructor-arg value="ChangAnMazda"></constructor-arg>
		<!-- 若字面值中包含特殊字符, 则可以使用 DCDATA 来进行赋值. (了解) -->
		<constructor-arg>
			<value><![CDATA[<ATARZA>]]></value>
		</constructor-arg>
		<constructor-arg value="180" type="int"></constructor-arg>
</bean>


<!-- 配置 bean -->
<bean id="dao5" class="com.atguigu.spring.ref.Dao"></bean>

<bean id="service" class="com.atguigu.spring.ref.Service">
    <!-- 通过 ref 属性值指定当前属性指向哪一个 bean! -->
    <property name="dao" ref="dao5"></property>
</bean>




<!-- 声明使用内部 bean -->
<bean id="service2" class="com.atguigu.spring.ref.Service">
    <property name="dao">
        <!-- 内部 bean, 类似于匿名内部类对象. 不能被外部的 bean 来引用, 也没有必要设置 id 属性 -->
        <bean class="com.atguigu.spring.ref.Dao">
            <property name="dataSource" value="c3p0"></property>
        </bean>
    </property>
</bean>



<!-- 赋值为 null -->
	<bean id="dao2" class="com.atguigu.spring.ref.Dao">
		<!-- 为 Dao 的 dataSource 属性赋值为 null -->
		<property name="dataSource"><null/></property>
	</bean>


<!-- 装配集合属性 -->
	<bean id="user" class="com.atguigu.spring.helloworld.User">
		<property name="userName" value="Jack"></property>
		<property name="cars">
			<!-- 使用 list 元素来装配集合属性 -->
			<list>
				<ref bean="car"/>
				<ref bean="car2"/>
			</list>
		</property>
	</bean>





	<!-- 声明集合类型的 bean -->
	<util:list id="cars">
		<ref bean="car"/>
		<ref bean="car2"/>
	</util:list>
	
	<bean id="user2" class="com.atguigu.spring.helloworld.User">
		<property name="userName" value="Rose"></property>
		<!-- 引用外部声明的 list -->
		<property name="cars" ref="cars"></property>
	</bean>



<!-- map集合 -->
<bean id="newPerson" class="newPerson" >
	<property name="name" value="Rose"></property>
	<property name="cars">
		<map>
			<entry key="AA" value-ref="car"></entry>
			<entry key="BB" value-ref="car2"></entry>
		</map>
	</property>
</bean>

<!-- p标签 -->
<bean id="user3" class="com.atguigu.spring.helloworld.User"
		p:cars-ref="cars" p:userName="Titannic"></bean>


<!-- 使用 parent 来继承bean 的配置 -->	
	<bean id="user4" parent="user" p:userName="Bob"></bean>
	
	<bean id="user6" parent="user" p:userName="维多利亚"></bean>



<!-- proterties配置属性值 -->	
<bean id="dataSource" class="dataSource" >
	<property name="proterties" >
		<prop key="user">root</prop>
		<prop key="password">1234</prop>
		<prop key="jdbcUrl">jdbc:mysql///test</prop>
		<prop key="driverClass">Driver</prop>
	</property>
	
</bean>


<!-- depents-on 依赖user6,没有创建user6会抛异常.多个依赖用逗号分开 -->	
	<bean id="user5" parent="user" p:userName="Backham" depends-on="user6"></bean>



<!-- autowire 可以自动根据bean的名字自动装配 -->	
<bean id="user5" class="com.atguigu.spring.helloworld.User" p:name="Tom" autowire="byName"></bean>

<!-- autowire 可以自动根据bean的类型自动装配,只能有一个类型相同的bean -->	
<bean id="user5" class="com.atguigu.spring.helloworld.User" p:name="Tom" autowire="byType"></bean>




<!-- Bean 的作用域 -->	
<!-- singelton:单例,在IOC生命周期只会创建一次 -->
<!-- prototype:每次调用getBean都会返回新的实例 -->
<bean id="user5" class="com.atguigu.spring.helloworld.User" p:name="Tom" scope="prototype"></bean>


<!-- 导入配置文件 ,db.properties是配置文件-->
<context:proterty-placeholder location="classpath:da.properties"/>

<bean id="dataSource" class="dataSource" >
	<proterty name="user" value="${user}"></proterty>
	<proterty name="password" value="${password}"></proterty>
	<proterty name="driverClass" value="${driverclass}"></proterty>
	<proterty name="jdbcUrl" value="${jdbcurl}"></proterty>
	
</bean>

<!-- ---------------------SPel-------------------------------- -->	
<bean id="adress" class="Adress" >
	<proterty name="city" value="#{Beijing}"></proterty>
	<proterty name="password" value="${password}"></proterty>
</bean>

<bean id="car" class="Car" >
	<proterty name="brand" value="audi"></proterty>
    <proterty name="price" value="5000000"></proterty>
	<proterty name="tyrePerimeter" value="#{T(java.lang.Math).PI*80}"></proterty>
</bean>

<bean id="person" class="Person" >
	<proterty name="car" value="#{car}"></proterty>
	<proterty name="city" value="#{address.city}"></proterty>
    <proterty name="info" value="#{car.price >300000 ? '金领' :'白领 "></proterty>
</bean>
<!-- ---------------------SPel-------------------------------- -->	



<!-- Bean 的生命周期方法 -->
在 Bean 的声明里设置 init-method 和 destroy-method 属性, 为 Bean 指定初始化和销毁方法.

1. 通过构造器或工厂方法创建 Bean 实例
2.为 Bean 的属性设置值和对其他 Bean 的引用
3.调用 Bean 的初始化方法
4.Bean 可以使用了
5.当容器关闭时, 调用 Bean 的销毁方法


<!-- init 和destroy 是bean的方法名 -->
<bean id="boy" class="com.atguigu.spring.helloworld.User" init-method="init" destroy-method="destroy">
    <property name="userName" value="高胜远"></property>
    <property name="wifeName" value="#{girl.userName}"></property>
</bean>



<!-- 创建 Bean 后置处理器 -->
首先实现BeanPostProcessor
重写postProcessBeforeInitialization 和postProcessAfterInitialization方法




Spring IOC 容器对 Bean 的生命周期进行管理的过程:
1.通过构造器或工厂方法创建 Bean 实例
2.为 Bean 的属性设置值和对其他 Bean 的引用
3.将 Bean 实例传递给 Bean 后置处理器的 postProcessBeforeInitialization 方法
4.调用 Bean 的初始化方法
5.将 Bean 实例传递给 Bean 后置处理器的 postProcessAfterInitialization方法
6.Bean 可以使用了
7.当容器关闭时, 调用 Bean 的销毁方法








```



```java

```

