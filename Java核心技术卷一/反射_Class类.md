# 反射——Class

> 反射库提供了一个非常丰富且精心设计的工具类。

> 能够分析类能力的程序称为反射(reflective).

**反射机制可以用来:**

* 在运行时分析类的能力
* 在运行时查看对象，例如，编写一个`toString`方法供所有类使用
* 实现通用的数组操作代码
* 利用Method对象,这个对象很像C++中的函数指针

**反射是一种功能强大且复杂的机制。使用它的主要人员是工具构造者**

* `Class类`

  > 程序运行期间，java运行时系统始终为所有对象维护一个被称为运行时的类型标识，这个信息跟踪着每个对象所属的类。虚拟机利用运行时类型信息选择相应的方法执行，可以通过专门的java类访问这些信息。保存这些信息的类被称为`Class`，`Object`类中的`getClass()方法`会返回一个`class`类型实例.

* 获得class类对象
  1. `对象名.getClass()`
  2. `Class.forName("类名")`
  3. `类型.class`

* **注意**

  > 一个class对象其实表示的是一个类型，而这个类型未必是一个类比如int不是类但它也有class对象通过`int.class`获得

* **Class 类其实是一个泛型类**

* 虚拟机为每个类型管理**一个**Class对象。因此可以用`==`实现两个对象进行比较。

* `Object newInstance()`方法

  > `newInstance()`方法默认调用类的无参构造方法,如果没有无参构造就会抛异常
>
  > 如果要调用有参构造可以使用`java.lang.reflect.Constructor`的
>
  > `Object newInstance(Object[] args)`其中`args`就是提供给构造器的方法

  案例:创建Random对象实例

```java
  String cName = "java.util.Random";
  Random r = (Random)Class.forName(cName).newInstance();//别忘了转换类型
```

* **Class类的方法**

  * `Field[] getFields()`
  * `Field getField(String name)`

  * `Field[] getDeclaredFields()`

  > `getFields()` 方法将返回一个包含Field对象的数组，这些对象记录了这个类或其超类的公有域。`getField(String name)`返回指定名称的域， `getDeclaredFields()`方法也返回包含Field对象的数组，这些对象记录了这个类的全部域但不包括从超类中继承的域。如果类中没有域，或者Class对象描述的是基本类型或数组类型，这些方法将返回一个长度为0的数组。

  * `Method[] getMethods()`
  * `Method[] getDeclaredMethods()`

  > 返回包含Method对象的数组：`getMethods()`将返回所有的公有方法，包括从超类继承来的公有方法；`getDeclaredMethods()`返回这个类或接口的全部方法但不包括由超类中继承的方法。

  * `Constructor[] getConstructors()`
  * `Constructor[] getDeclaredConstructors()`

  > `getConstructors()`返回包含 Constructor 对象的数组,其中包含了Class对象所描述的类的所有**公有**构造器
  >
  > `getDeclaredConstructors()返回包含 Constructor 对象的数组,其中包含了Class对象所描述的类的所有构造器`

  