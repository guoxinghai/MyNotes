# SQL 注入问题

案例：

> 检测用户输入的用户名和密码是否正确可以使用：
>
> `String sql = "select * from userBatabase where name = ' "+userName +" ' and password = ' "+password+" ' "`

上面这条语句有可能会产生SQL攻击问题

当用户输入`userName ="test" password = "test ' or '1' = '1  "`时上面的SQl语句变成了

`select * from userBatabase where name = ' test ' and password = ' test ' or '1' = '1'` 

这样不管用户输入什么用户名和密码都将查询成功。

防止SQL攻击可以使用**PrepareStatement** 来执行SQL语句

PrepareStatement 是Statement的子类

使用PrepareStatement的好处：

* 防止SQl攻击

  > SQL语句在程序运行前已经进行了预编译，在程序运行时第一次操作数据库之前，SQL语句已经被数据库分析，编译和优化，对应的执行计划也会缓存下来并允许数据库已参数化的形式进行查询，当运行时动态地把参数传给PreprareStatement时，即使参数里有敏感字符如 or '1=1'也数据库会作为一个参数一个字段的属性值来处理而不会作为一个SQL指令。

* 提高代码的可读性，可维护性

* 提高效率

  > 每一种数据库都会尽最大努力对预编译语句提供最大的性能优化 .因为预编译语句有可能被重复调用.所以语句在被DB的编译器编译后的执行代码被缓存下来,那么下次调用时只要是相同的预编译语句就不需要编译,只要将参数直接传入编译过的语句执行代码中( **相当于一个函数，用就是了，不用重写一个来实现** )就会得到执行. 

用PrepareStatement改写案例1：

```java
String sql = "select * from userBatabase where name = ? and password = ? ";
PrepareStatement pstate = con.PrepareStatement(sql);
pstate.setString(1,userName);
pstate.setString(2,password);
ResultSet set = pstate.executeQuery();
if(set.next()){
    System.out.println("登陆成功！");
}else{
    System.out.println("登录失败！");
}
```

