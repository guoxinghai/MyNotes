**java语言规范要求equals必须具有下面特性:** 

* **自反性**：对于任何非空引用x，x.equals(x)应该返回true
* **对称性**：对于任何引用x，y，当且仅当y.equals(x)返回true。x.equals(y)也返回true
* **传递性**：对于任何引用x，y，z，如果x.equals(y)返回true，y.equals(z)返回true,x.equals(z)也应返回true
* **一致性**：如果x和y引用的对象没有发生变化，反复调用x.equals(y)应该返回相同的结果
* 对于任意非空引用x，x.equals(null) 应该返回 false

> 假如要实现equals的方法是Employee类

**Employee类**

```java
Class Employee{
    private String name;
    private int id;
    private double salary;
    
    public void setName(String name){
        this.name = name;
    }
    public void setId(int id){
        this.id = id;
    }
    public void setSalary(double salary){
        this.salary = salary;
    }
    
    public String getName(){
        return this.name;
    }
    public int getId(){
        return this.id;
    }
    public double getSalary(){
        return this.salary;
    }
}
```

**Manager类**

```java
class Manager extends Employee{
    private int rank;
    
    public void setRank(int rank){
        this.rank = rank;
    }
    public int getRank(){
        return this.rank;
    }
} 
```

**Employee类的equals()**

```java
public boolean equals(Object obj){
    if(obj == null){
        return false;
    }
    if(this == obj){
        return true;
    }
    if(obj.getClass() != this.getClass() ){
        return false;
    }
    Employee emp = (Employee)obj;
    
    return name.equals(emp.name)
           &&id==emp.id
           &&Double.compare(salary,emp.salary)==0;
    
}
```

**Manager类的equals()**

```java
public boolean equals(Object obj){
    if( !super.equals(obj)){	//先调用父类的方法测试
        return false;
    }
    Manager man = (Manager)obj;
    return this.rank == man.rank;
}
```

