# 执行SQL语句

* **获得连接**

  `Connection con = DriverManager.getConnection(url,user,pass)`

* **获得执行语句的对象**

  `Statement stat = con.createStatement()`

* **执行SQL语句**
  * `int executeUpdate(String sql)`----执行 insert，update，delete语句(DML)
  * `ResultSet executeQuery(String sql)`----执行select语句(DQL)
  * `boolean execute(String sql)`----执行select语句true执行其他语句返回false
    * 如果返回true，需要使用getResultSet()获得查询结果
    * 如果返回false，需要使用getUpdateCount()获得影响行数

* **执行批处理：**

  * `addBatch(String sql)`

  * `clearBatch()`

  * `executeBatch()`

    **特点：如果有参数，需要在sql语句中进行拼凑，存在sql注入问题。**

    

