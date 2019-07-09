# 使用LitePal操作数据库

> LitePal是一款开源的Android数据库框架，它采用了对象关系映射的模式，并将我们平时开发最常用到的一些数据库功能进行了封装，使得不用编写一行SQL语句就可以完成各种建表和增删改查的操作。

* **配置`LitePal`(3步)**

  1. **编辑`app/build.grade`**

     > 大多数的开源项目都会将版本提交到`jcenter`上，只需要在`app/build.grade`文件中声明该开源库的引用就可以了。

     ```java
     dependencies {
         implementation 'org.litepal.android:java:3.0.0'
     }
     ```

     

  2. **编写`litepal.xml`**

     在`app/src/main`中新建一个`assets`目录，在`assets`目录中新建一个`litepal.xml`文件

     接着编辑`litepal`文件,如下：

     ```xml
     <?xml version="1.0" encoding="utf-8"?>
     <litepal>
         <!--指定数据库名-->
         <dbname value="demo" />
     	<!--指定数据库版本号-->
         <version value="1" />
     	<!--指定所有的对象映射模型，用于在数据库中创建表-->
         <list>
         	<mapping class="com.test.model.Demo1" />
         	<mapping class="com.test.model.Demo2" />
         </list>
         
     </litepal>
     ```

  3. **配置`AndroidManifest.xml`**

     ```xml
     <manifest 
          xmlns:android="..."
          package="...">
          <application
              android:name="org.litepal.LitePalApplication">
         </application>
     </manifest>
     ```

* **创建数据库**

  > LitePal采取对象关系映射：将面向对象的语言和面向关系的数据库之间建立一种映射关系。使我们用面向对象的思维来操作数据库，而不用再和SQL语句打交道了。

   1. 在`litepal.xml`中配置数据库名

      `<dbname value="demo"/>`

  2. 创建`javaBean`用于创建表,并添加到`litepal.xml`中

     **创建`javaBean`**

     ```java
     public class Demo extends LitePalSupport{
         @Column(unique = true, defaultValue = "unknown")
         private String name;
         
     	@Column(nullable = false)
         private float price;
         
     	private byte[] cover;
         
         private List<Song> songs = new ArrayList<Song>();
     
         // generated getters and setters.
         ...
     }
     ```

     **@Column属性**

     1.*unique*属性表示该字段是否为唯一标识，默认为*false*。如果表中有一个字段需要唯一标识，则既可以使用该标记，也可以使用*@Table*标记中的*@UniqueConstraint*。

     *2.* *nullable*属性表示该字段是否可以为*null*值，默认为*true*。注意:(当字段为String 时 可传入""空字符传  当字段为数值时  可传进0等)

     *3.* *insertable*属性表示在使用“*INSERT*”脚本插入数据时，是否需要插入该字段的值。

     *4.*updatable*属性表示在使用“*UPDATE*”脚本插入数据时，是否需要更新该字段的值。*insertable*和*updatable属性一般多用于只读的属性，例如主键和外键等。这些字段的值通常是自动生成的。

     *5.* *columnDefinition*属性表示创建表时，该字段创建的*SQL*语句，一般用于通过*Entity*生成表定义时使用。

     *6.* *table*属性表示当映射多个表时，指定表的表中的字段。默认值为主表的表名。有关多个表的映射将在本章的*5.6*小节中详细讲述。

     *7.* *length*属性表示字段的长度，当字段的类型为*varchar*时，该属性才有效，默认为*255*个字符。

     *8.* *precision*属性和*scale*属性表示精度，当字段类型为*double*时，*precision*表示数值的总长度，*scale*表示小数点所占的位数。(设置scale 要设置precision,否则scale 不生效)

     9.ignore 为true或false 表示属性建立映射时是否忽略该属性。

     **将Demo类添加到映射模型列表中**

     ```xml
     <list>
     	<mapping class="com.example.test.Demo"/>
     </list>
     ```

     下一步操作数据库即可创建出数据库`demo`并且在`demo`中创建出`Demo表`

     例如:`LitePal.getDatabase();`

     `Demo`表是按照以下SQL语句创建出来的

     ```sql
     CREATE TABLE album (
     	id integer primary key autoincrement,
     	name text unique default 'unknown',
     	price real not null,
     	cover blob
     );
     ```

* **升级数据库**

  **只需要修改完`javabean`之后将`litepal.xml`的`version`的`value`值加1即可**

  `<version value="2"/>`

* **增删该查**

  1. 增加数据

     创建一个`javabean`对象设置完值之后调用`save()`方法即可

     ```java
     Demo demo = new Demo();
     demo.setName("test");
     demo.setPrice(10.99f);
     ...
     demo.save();
     ```

  2. 删除数据

     ```java
     //删除单个记录，Demo表,id为6的数据
     LitePal.delete(Demo.class,6);
     //删除Demo表中的所有记录
     LitePal.deleteAll(Demo.class);
     //删除符合条件的记录
     LitePal.deleteAll(Demo.class,"price > ? and name = ?","3500","xiaoming");
     ```

  3. 修改数据

     ```java
     //方法一 修改id为1的数据
     Demo demo1 = LitePal.find(Demo.class,1);
     demo1.setName("test");
     demo1.save();
     //方法二 修改id为1的数据
     Demo demo2 = new Demo();
     demo2.setName("test");
     demo2.update(1);
     //方法三 修改符合条件的数据
     Demo demo3 = new Demo();
     demo3.setName("test");
     demo3.updateAll("name = ? and price > ?","name","666");
     ```

  4. 查询数据

     ```java
     //查找Demo表的所有数据
     List<Demo> list = LitePal.findAll(Demo.class);
     //查找Demo表中id为1的数据
     Demo demo = LitePal.find(Demo.class,1);
     //连缀查询
     List<Demo> list = LitePal.select("name","price")
         					 .where("price > ? and name = ?","3500","test")
         					 .order("price desc")
         					 .offset(10)
         					 .limit(10)
         					 .find(Demo.class);
     //offset 表示偏移量,比如offset(10) 就是data11,data12...
     //通过SQL查询
     Cursor c = LitePal.findBySQL("select * from Demo where name = ? and price > ?","test","3500");
     ```

     