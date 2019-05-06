# 使用pageContext获得和设置属性

* 设置一个页面作用域属性

  ```jsp
  <% Float one = new Float(42.5); %>
  <% pageContext.setAttribute("foo", one); %>
  ```

* 获得一个页面作用域属性

  `<%= pageContext.getAttribute("foo") %>`

* 使用pageContext设置一个会话作用域属性

  ```jsp
  <% Float two = new Float(22.4); %>
  <% pageContext.setAttribute(two, PageContext.SESSION_SCOPE); %>
  ```

* 使用pageContext获得一个会话作用域属性

  `<%= pageContext.getAttribute("two", PageContext.SESSION_SCOPE) %>`

* 使用pageContext获得一个应用作用域属性

  `<%= pageContext.getAttribute("mail", PageContext.APPLICATION_SCOPE) %>`

  以上代码等价于：`<%= application.getAttibute("mail")%>`

* 使用pageContext即使不知道作用域也可以查找一个属性

  `<%= pageContext.findAttribute("foo") %>`

  **`findAtrribute()`方法会首先在页面上下文查找，如果没找到就会在其他作用域找，先从最严格的作用域查起，逐步转向不那么严格的作用域。如果在一个作用域中找到属性就不会去其他作用域查找。**

