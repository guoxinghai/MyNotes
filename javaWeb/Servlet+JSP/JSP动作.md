# JSP动作

* `<jsp:useBean>`有可能会自己创建一个bean

  这一行代码`<jsp:useBean id="people" class="com.hai.people" scope="request"/>`

  如果request的attribute中没有people这个bean

* `<jsp:useBean>`也可以有体

  当我们选择的`scope`中没有我们需要的bean我们可以自己定义创建的bean的属性

  ```java
  <jsp:useBean id="people" class="com.hai.people" scope="request">(这个地方没有/)
      <jsp:setProperty name="people" property="sex" value="male"/>
  </jsp:useBean>
  ```

* `bean`法则

  > 使用jsp调用的bean类需要遵循一定的规则

  * 必须有一个**无参数**的**公共**构造函数

  * 必须按命名约定来命名公共的获取方法和设置方法

    首先是`get`或`set`然后是属性名（首字母大写）

    例如`getName()`

  * 设置方法的参数类型和获取方法的返回类型是一致的

  * 性质名和类型是由获取方法和设置方法得出，而不是

    得自类成员.

  * 结合JSP使用时，性质类型最好时`String`类型,或者是

    一个**基本类型**，如果不是这样，尽管也许是一个合法

    的`bean`,你可能还得使用脚本，而不能摆脱脚本只使用

    标准动作.

* 可以建立多态的`bean`引用

  > 为`<jsp:useBean/>增加一个type属性`

  `<jsp:useBean id="people" type="com.hai.people" class="com.hai.Employee" scope="request"/>`

* 使用type但没有class

  `<jsp:useBean id="people" type="com.hai.people" scope="request"/>`

  > 如果“request”作用域中由people属性就能正常工作
  >
  > 如果没有，结果如下
  >
  > `java.lang.InstantiationException:bean people not found within scope`

* scope属性默认为"**page**"

  `<jsp:useBean id="people" class="com.hai.people" scope="page"/>`与

  `<jsp:useBean id="people" class="com.hai.people"/>`是一样的。

* type 与 class 

  `type == 引用类型`

  `class == 对象类型`

  > type 可以是抽象类，接口 但class必须是具体类

* 从请求直接到jsp不经过servlet

  * 要获得请求中表单的属性可以使用`param`属性

    **案例：**

    含有表单的页面：

    ```jsp
    <html><body>        
        <form action="TestBean.jsp">
            name :<input type="text" name="userName">
            ID	 :<input type="text" name="userID">
            <input type="submit">
        </form>
    </body></html> 
    ```

    JSP代码：

    ```jsp
    <jsp:useBean id="person" type="foo.Person" class="foo.Employee">
    	<jsp:setProperty name="person" property="name" param="userName"/>
        <jsp:setProperty name="person" property="ID" param="userID"/>
    </jsp:useBean>
    name:<jsp:getProperty name="person" property="name"/>
    ID:<jsp:getProperty name="person" property="ID"/>
    ```

  * 可以更简单

    修改表单页面：

    ```jsp
    <html><body>        
        <form action="TestBean.jsp">
            name :<input type="text" name="name"><!--将name设置成person类中的属性名-->
            ID	 :<input type="text" name="userID">
            <input type="submit">
        </form>
    </body></html> 
    ```

    JSP代码：

    ```jsp
    <jsp:useBean id="person" type="foo.Person" class="foo.Employee">
    	<jsp:setProperty name="person" property="name"/> //不用写param
        <jsp:setProperty name="person" property="ID" param="userID"/>
    </jsp:useBean>
    name:<jsp:getProperty name="person" property="name"/>
    ID:<jsp:getProperty name="person" property="ID"/>
    ```

    **如果指定了property但是没有写value和param就是在告诉容器要从有匹配名的请求参数得到值**

    **如果请求参数名与bean性质名匹配，就不需要在`<jsp:setProperty/>标记中为该性质指定值`**

  * 如果愿意，还可以更简单

    表单页面：

    ```jsp
    <html><body>        
        <form action="TestBean.jsp">
            name :<input type="text" name="name"><!--将name设置成person类中的属性名-->
            ID	 :<input type="text" name="ID"><!--将ID设置成person类中的属性名-->
            <input type="submit">
        </form>
    </body></html> 
    ```

    JSP代码：

    ```jsp
    <jsp:useBean id="person" type="foo.Person" class="foo.Employee">
    	<jsp:setProperty name="person" property="*"/>
    </jsp:useBean>
    name:<jsp:getProperty name="person" property="name"/>
    ID:<jsp:getProperty name="person" property="ID"/>
    ```

    容器会查看bean类的get和set方法获取bean的性质，并且与参数相匹配

* `<jsp:setProperty/>`会自动转换**基本类型和String**的性质

  > 假设employee类的ID属性是int类型的

  代码：`<jsp:setProperty name="person" property="ID" param="userID"/>`

  会将`userID`转换成`int`类型再赋值，原因是容器会调用`Integer.parseInt()`方法

  但是如果`userID`值为`one`将会报错。（可以在前端用javascript代码检测一遍）

* 下面这些代码都会将String转换成int

  `<jsp:setProperty name="person" property="*"/>`

  `<jsp:setProperty name="person" property="ID"/>`

  `<jsp:setProperty name="person" property="ID" value="123"/>`

  `<jsp:setProperty name="person" property="ID" param="userID"/>`

  **但是如果使用了脚本，就不会自动转换：**

  ​	`<jsp:setProperty name="person" property="ID"`

  ​	` value="<%=request.getParameter("userID")%>"/>`

* **注意：param和value不能同时使用**

