# JSP

* **自动创建session**

  当web请求一个jsp页面的时候会**自动创建**一个session

  但是我们也可以通过设置来让jsp不创建session代码如下

  `<%@ page session="false" %>`

* **指令**

  1. **page**

     > 定义页面特定的属性，如字符编码，页面相应的内容类型，以及这个页面是否要有隐式的会话对象。page指令最多可以使用至多使用13个不同的属性。
     >
     > 4个。

     **属性**

     * import [重点]

       定义java import语句。生成的servlet类中会自动的加上一下import语句

       `import javax.servlet.*;`

       `import javax.servlet.http.*;`

       `import javax.servlet.jsp.*;`

     * isThreadSafe [重点]

       定义生成的servlet是否需要实现`SingleThreadModel`,不过你知道这是一个很糟糕的想法。默认值是`true`这代表“我的应用是线程安全的，所以不需要实现`SingleThreadModel`，**最好不要让jsp实现**`SingleThreadModel`

       **原因：**

       这种实践将导致 Web 容器创建多个 servlet 实例；即为每个用户创建一个实例。对于任何大小的应用程序，这种实践都将导致严重的性能问题。 

     * contentType [重点]

       定义jsp响应的MIME内容(和可选的字符编码)。

     * isELIgnored [重点]

       定义转换这个页面时忽略EL表达式。

     * isErrorPage [重点]

       定义当前页面是否是另一个JSP的错误页面。默认值是`false`,但是如果这个属性值是`true`，页面就能访问隐式的exception对象。

     * errorPage [重点]

       定义一个资源的URL，如果有未捕获的Throwable，就会发送到这个资源。如果这个属性指定了一个JSP，该JSP的page指令中就会有`isErrorPage=“true”`属性

     * language

       定义scriptlet, 表达式 和 声明中使用的脚本语言。现在，可取值的只有一个，就是 “java” ，但是还留有这个属性是未雨绸缪，考虑到将来可能会用到其他语言。

     * extends

       jsp会变成一个servlet类，这个属性则定义了这个类的超类。它会覆盖容器提供的类层次体系。

     * session

       定义页面是否有一个隐式的session对象，默认值为`true`

     * buffer

       定义隐式out对象(JspWriter的引用) 如何处理缓存

     * autoFlush

       定义缓存的输出是否自动刷新输出。默认是`true`

     * info

       定义放到转换后页面中的串，这样就能使用所生成servlet继承的getSrvletInfo()方法来得到这个信息。

     * pageEncoding

       定义JSP的字符编码。默认为`ISO-8859-1`(除非contentType属性已经定义了一个字符编码，或页面使用XML文档语法.

* **使JSP页面忽略脚本(scriptlet)**

  **要使jsp忽略脚本只能通过修改xml**

  ```java
  <web-app>
  	<jsp-config>
  		<jsp-property-group>
  			<url-pattern>*.jsp</url-pattern>
  			<scripting-invalid>true</scripting-invalid>
  		</jsp-property-group>
  	</jsp-config>
  </web-app>
  ```

* **使JSP页面忽略EL表达式**

  * **方法1——设置jsp的page属性**

    `<% @page isELIgnored="true"%>`

  * **方法2——修改xml**

      ```java
      <web-app>
      	<jsp-config>
      		<jsp-property-group>
      			<url-pattern>*.jsp</url-pattern>
      			<el-ignored>true</el-ignored>
      		</jsp-property-group>
      	</jsp-config>
      </web-app>
      ```

* **JSP页面是否忽略EL表达式`<% @page isELIgnored="true"%>`的级别优先于XML配置**

**隐式对象**

|         API         |  隐式对象   |
| :-----------------: | :---------: |
|      JspWriter      |     out     |
| HttpServletRequest  |   request   |
| HttpServletResponse |  response   |
|     HttpSession     |   session   |
|   ServletContext    | application |
|    ServletConfig    |   config    |
|      Throwable      |  exception  |
|     PageContext     | pageContext |
|       Object        |    page     |

**PageContext封装了其他隐式对象，所以如果向某些辅助对象提供了一个PageContext引用，这些辅助对象可以使用PageContext引用得到其他隐式对象的引用及所有作用域的属性**