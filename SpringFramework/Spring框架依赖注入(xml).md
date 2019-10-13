# `Spring`框架依赖注入

* [显式注入](#t1)
* [自动注入](#t2)
* [注入集合](#t3)

**以两个`Bean`为案例数据**

```java
//driver类
class Driver{
    private int id;
    private String name;
    private Car car;
}
//car类
class Car{
    private int id;
    private String type;
}
```



* <span id="t1">**显式注入**</span>

  * **通过set方法注入**

    ```java
    //首先在bean中写好set方法，例如:
    public void setCar(Car car){
        this.car = car;
    }
    //在xml配置文件中
    <beans
    	...>
    	//car对象
    	<bean id="car" class="com.hai.Car">
    		<property name="id" value="1"/>
    		<property name="type" value="bus"/>
    	</bean>
    	//driver对象
    	<bean id="driver" class="com.hai.Driver">
    		<property name="id" value="1"/>
    		<property name="name" value="张三"/>
    		<property name="car" ref="car"/>	//注意引用与普通值方式不同
    	<bean/>
    </beans>
    ```

  * **通过构造函数注入**

    ```java
    //首先在bean中写好构造方法，例如:
    public Car(int id,String type){
        this.id = id;
        this.type = type;
    }
    public Driver(ing id,String name,Car car){
        this.id = id;
        this.name = name;
        this.car = car;
    }
    //在xml配置文件中
    <beans
    	...>
    	//car对象
    	<bean id="car" class="com.hai.Car">
    		<constructor-arg name="id" value="1"/> //如果使用name属性指定值 则name的值要与构造函数中的参数名称一致
    		<constructor-arg name="type" value="bus"/>
    	</bean>
    	//driver对象
    	<bean id="driver" class="com.hai.Driver">
    		<constructor-arg index="0" value="1"/> //如果用index属性指定值则 index从0开始与构造函数中的参数一 一对应
            <constructor-arg index="1" value="张三"/>
            <constructor-arg index="2" ref="car"/> //注意引用与简单值有所不同
        <bean/>
    </beans>
    ```

    

* <span id="t2">**自动注入**</span>

  * **通过名字**

    > **注意： 通过名字注入 属性名字要与注入的bean引用的id相同**

    ```java
    //写好要注入的属性的set方法(自动注入也要依赖set方法或构造函数)
    //xml配置文件
    <beans
    	...>
    	//这里的简单值通过set方法注入
    	<bean id="car" class="com.hai.Car">
    		<property name="id" value="1"/>
    		<property name="type" value="bus"/>
    	</bean>
    	//通过名字自动注入bean引用 set方法设置简单值
    	<bean id="driver" class="com.hai.Driver" autowire="byName">
    		<property name="id" value="1"/>
    		<property name="name" value="张三"/>
    	</bean>
    ```

  * **通过类型**

    > **注意：如果配置文件中对于需要注入的bean引用超过1个就会报错 **

    ```java
    //写好要注入的属性的set方法(自动注入也要依赖set方法或构造函数)
    //xml配置文件
    <beans
    	...>
    	//这里的简单值通过set方法注入
    	<bean id="car" class="com.hai.Car">
    		<property name="id" value="1"/>
    		<property name="type" value="bus"/>
    	</bean>
    	//通过名字自动注入bean引用 set方法设置简单值
    	<bean id="driver" class="com.hai.Driver" autowire="byType">
    		<property name="id" value="1"/>
    		<property name="name" value="张三"/>
    	</bean>
    ```

  * **通过构造函数**

    > **注意: **
    >
    > 1. 如果配置文件中某种需要注入的类型的bean只有一个 构造函数中有多个接收该类型bean的参数那么这几个参数都会接收同一个bean
    > 2. 如果配置文件中某种需要注入的类型的bean有多个 构造函数中接收该类型的bean的参数名没有与配置文件中bean的id相同的则会报错,  如果接受该类型的bean的参数与配置文件中某个bean的id相同 则不会报错并会注入该bean
    > 3. 如果配置文件中某种需要注入的类型的bean有多个 并且构造函数中有多个接收该类型的bean的参数如果参数名与配置文件中的bean的id一 一对应则不会报错 ，并且正常注入.

    ```xml
    //写好bean的构造函数 如
    public Driver(Car car){
    	this.car = car;
    }
    //注意如果配置文件中对于需要注入的bean引用超过1个就会报错 
    //xml配置文件
    <beans
    	...>
    	//这里的简单值通过set方法注入
    	<bean id="car" class="com.hai.Car">
    		<property name="id" value="1"/>
    		<property name="type" value="bus"/>
    	</bean>
    	//通过名字自动注入bean引用 set方法设置简单值
    	<bean id="driver" class="com.hai.Driver" autowire="constructor">
    		<property name="id" value="1"/>
    		<property name="name" value="张三"/>
    	</bean>
    ```

* <span id="t3"> **注入集合**</span>

  * **注入列表**

    * 注入简单值

      ```xml
      <beans
             ...>
          <bean id="test" class="com.hai.Test">
              <property name="simpleList">
          	<list>
              	<value>list1</value>
                  <value>list2</value>
                  <value>list3</value>
              </list>
          </property>
          </bean>
      </beans>
      ```

    * 注入引用

      ```xml
      <beans
             ...>
          <!--对象bean-->
          <bean id="object1" class="com.hai.Obj">
          	<property name="id" value="001"/>
              <property name="type" value="object"/>
          </bean>
          <bean id="object2" class="com.hai.Obj">
          	<property name="id" value="001"/>
              <property name="type" value="object"/>
          </bean>
          <bean id="test" class="com.hai.Test">
              <property name="objectList">
          	<list>
              	<ref bean="object1"/>
                  <ref bean="object2"/>
              </list>
          </property>
          </bean>
      </beans>
      ```

  * **注入set**

    * 简单值

      ```xml
      <beans
             ...>
          <bean id="test" class="com.hai.Test">
              <property name="simpleSet">
          	<set>
              	<value>set1</value>
                  <value>set2</value>
                  <value>set3</value>
              </set>
          </property>
          </bean>
      </beans>
      ```

    * 注入引用

      ```xml
      <beans
             ...>
          <!--对象bean-->
          <bean id="object1" class="com.hai.Obj">
          	<property name="id" value="001"/>
              <property name="type" value="object"/>
          </bean>
          <bean id="object2" class="com.hai.Obj">
          	<property name="id" value="001"/>
              <property name="type" value="object"/>
          </bean>
          <bean id="test" class="com.hai.Test">
              <property name="objectSet">
          	<set>
              	<ref bean="object1"/>
                  <ref bean="object2"/>
              </set>
          </property>
          </bean>
      </beans>
      ```

  * **注入map**

    * 简单值

      ```xml
      <beans
             ...>
          <bean id="test" class="com.hai.Test">
              <property name="simpleMap">
                  <map>
                      <entry key="1" value="map1"/>
                      <entry key="2" value="map2"/>
                      <entry key="3" value="map3"/>
                  </map>
          	</property>
          </bean>
      </beans>
      ```

    * 注入引用

      ```xml
      <beans
             ...>
          <!--对象bean-->
          <bean id="object1" class="com.hai.Obj">
          	<property name="id" value="001"/>
              <property name="type" value="object"/>
          </bean>
          <bean id="object2" class="com.hai.Obj">
          	<property name="id" value="001"/>
              <property name="type" value="object"/>
          </bean>
          <bean id="test" class="com.hai.Test">
              <property name="objectMap">
          	<map>
                  <!-- key也可以是引用 用 key-ref 表示-->
              	<entry key="1" value-ref="object1"/>
                  <entry key="2" value-ref="object2"/>
              </map>
          </property>
          </bean>
      </beans>
      ```

  * **注入properties**

    > **属性列表中每个键及其对应值都是一个字符串 所以只能注入简单值**

    ```xml
    <beans
           ...>
        <bean id="test" class="com.hai.Test">
            <property name="properties">
        		<props>
                	<prop key="1">props1</prop>
                    <prop key="2">props2</prop>
                    <prop key="3">props3</prop>
                </props>
        	</property>
        </bean>
    </beans>
    ```

    