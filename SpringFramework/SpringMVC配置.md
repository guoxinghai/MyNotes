# `SpringMVC`配置

1. **配置`dispatcherServlet`**

   > 需要在`web.xml`中注册`dispatcherServlet`,用来拦截`.jsp`以外其他的请求,并将请求交给适合的控制器进行处理

   ```xml
   <web-app
            ...>
       <servlet>
       	<servlet-name>dispatcher</servlet-name>
       	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
           <!--配置在启动服务器时即加载该servlet-->
           <load-on-startup>1</load-on-startup>
   	</servlet>
   	<servlet-mapping>
           <servlet-name>dispatcher</servlet-name>
           <url-pattern>/</url-pattern>
   	</servlet-mapping>
   </web-app>
   
   ```

2. **`SpringMVC`的配置文件**

   > 可以在`SpringMVC`中配置
   >
   > 1. 设置注解驱动
   > 2. 扫描控制器
   > 3. 配置视图解析器
   > 4. 处理静态资源（CSS，JS，Images...）

   * **文件内容**

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:mvc="http://www.springframework.org/schema/mvc"
     
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
               http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
               http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
     ">
         <!--设置注解驱动-->
         
         <!--扫描控制器-->
         
         <!--配置视图解析器-->
         
         <!--处理静态资源-->
         
     </beans>
     ```

   * **配置文件位置**

     > 两种方式

     1. 如果配置文件名字为`dispatcher-servlet.xml` 位置在`/WEB-INF/`那么就不用配置位置。

     2. 如果自定义名字需要在`web.xml`中如下配置

        ```xml
        <servlet>
                <servlet-name>dispatcher</servlet-name>
                ...
            	<!--省略-->
                <init-param>
                    <param-name>contextConfigLocation</param-name>
                    <param-value>/WEB-INF/spring_mvc.xml</param-value>
                </init-param>
        
            </servlet>
        ```

   * **设置注解驱动**

     > 以下配置可以使`dispatcherServlet`找到合适的`controller`和方法来处理请求

     `<mvc:annotation-driven />`

   * **配置视图解析器**

     > 视图解析器负责将控制器返回的视图名称解析为视图的完整名称

     ```xml
     <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
         <property name="prefix" value="/WEB-INF/jsp/"></property>
         <property name="suffix" value=".jsp"></property>
     </bean>
     ```

   * **处理静态资源**

     > 当页面请求CSS，JS，Images等静态资源时需要为其配置位置映射
     >
     > `mapping：`当请求为当前路径时 Spring MVC框架会为其返回该静态资源 其中`**`含义为包含子文件夹
     >
     > `location：`静态资源的位置

     ```xml
     <mvc:resources mapping="/css/**" location="/css/" />
     <mvc:resources mapping="/img/**" location="/img/" />
     <mvc:resources mapping="/js/**" location="/js/" />
     ```

     

3. **配置视图映射**

   > ​	一般将`.jsp`文件 放在 `/WEB-INF/jsp`中保护起来, 这样就无法通过在地址栏输入页面地址访问，需要`controller`配合`dispatcherServlet`将视图返回。
   >
   > ​	以下是为处在`/WEB-INF/jsp/`中的`jsp`页面配置映射。
   >
   > ​	`prefix：`前缀(位置)
   >
   > ​	`suffix：`后缀(后缀名)
   
   ```xml
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/jsp/"></property>
           <property name="suffix" value=".jsp"></property>
   </bean>
   ```

4. **`Bean`配置文件**

   > ​	一般将 `controller`类型的`bean` 在`springMVC`的配置文件中注册或扫描，其他类型的`bean`会在另一个配置文件中注册或扫描，注意 该配置文件如果名字不是`applicationContext.xml`
   1. 文件内容

      > 一般只是注册或扫描bean

      `<context:component-scan base-package="com.hai." />`

   2. 在`web.xml`中的配置

      > 需要注册`ContextLoaderListener`类不然即使添加了配置文件也检测不到

      ```xml
      <listener>
              <listener-class>
                  org.springframework.web.context.ContextLoaderListener
          	</listener-class>
      </listener>
      ```

   3. 配置位置

      > ​	位置一般放在/WEB-INF/目录下	如果在该目录下且名字为`applicationContext.xml`则不需要特意配置位置，但如果是自定义名称，则需要在`web.xml`中如下配置

      ```xml
      <web-app
               ..>
      <!--注意不是在某个servlet中-->
          <context-param>
          	<param-name>contextConfigLocation</param-name>
              <param-value>/WEB-INF/spring_config.xml</param-value>
          </context-param>
      </web-app>
      ```

      

   