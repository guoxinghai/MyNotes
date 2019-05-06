# 反射——reflect包

​	**在java.lang.reflet包中有三个类Field, Method和Constructor分别用于描述类的域，方法和构造器, 这三个类都有一个叫做`getName()`的方法，用来返回项目的名称.Field类有一个`getType()`方法,用来返回描述域所属类型的`class`对象。Method和Constructor有能够报告参数类型的方法，Method类还有一个可以报告返回类型的方法,这三个类还有一个叫做`getModifiers()`的方法，它将返回一个整数值，用不同的位开关描述`public`和`static`这样修饰符的使用情况。另外可以利用`java.lang.reflect`包中的`Modifier`类的静态方法分析`getModifier()`返回的整数值。例如可以使用`Modifier`类中的`isPublic, isPrivate或isFinal`判断，还可以利用`Modifier.toString()方法将修饰符打印出来`**

​	**Class类中的`getFields,getMethod, getConstructosr`方法将分别返回类提供的public域，方法，构造器数组。其中包括超类的公有成员。**

​	**Class类的`getDeclareFields,getDeclareMethods,getDeclaredConstructors`方法将分别返回类中声明的全部域，方法和构造器，其中包括私有和受保护成员，但不包括超类成员。**

* **Modifier类常用方法**

  * static String toString(int modifiers)

    > 返回对应modifier中位设置的修饰符字符串表示

  * static boolean isAbstract(int modifiers)

  * static boolean isFinal(int modifiers)

  * static boolean isInterface(int modifiers)

  * static boolean isNative(int modifiers)

  * static boolean isPrivate(int modifiers)

  * static boolean isProtected(int modifiers)

  * static boolean isPublic(int modifiers)

  * static boolean isStatic(int modifiers)

  * static boolean isSynchornized(int modifiers)

  * static boolean isVolatile(int modifiers)