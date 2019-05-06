# EL表达式

**EL表达式总是放在大括号中，前面有一个美元前缀**

例：`${firstThing.secondThing}`

firstThing可以是一个**隐式对象**或一个**Attribute**，如果是一个Attribute可以是任意作用域中的。

**EL隐式对象**

1. pageScope
2. requestScope
3. sessionScope
4. applicationScope
5. param
6. paramValues
7. header
8. headerValues
9. cookie
10. initParam
11. pageContext

**所有的隐式对象中只有pageContext不是映射(Map)，它是一个JavaBean**

**提示：**映射是一个保存键值对的集合。

**EL表达式与JSP脚本中可用的隐式对象不同，只有pageContext除外**

* 使用`.`操作符访问性质或映射值

  `${person.name}`

  > 第一个变量可以是一个隐式对象，也可以是一个属性，点号右边可以是一个映射键，也可以是一个bean性质。

* `[]`就像是更好的`.`

  > 只有当点号右边是左边变量的bean性质或者映射键时，点号才能正常工作。
  >
  > [ ]就强大多了，而且更加灵活

  `${person["name"]}`与`${person.name}`等价

* `[]`让你有更多选择

  > [ ]左边可以是Map，bean，List或一个数组

* 如果[ ]里面是一个String直接量（即用“”引起的串），可以是一个map键，或是一个bean性质，还可以是一个List或数组中的索引

* 如果[ ]里面是一个整数，只能是一个List或数组中的索引

* 如果[ ]里面是一个属性则会进行计算

  例如：`${musicMap[Ambiet]}`

  会查找`Ambiet`属性的值，如果没有则为`null`

* [ ]可以使用嵌套表达式

  `${musicMap[musicType[1]]}`

