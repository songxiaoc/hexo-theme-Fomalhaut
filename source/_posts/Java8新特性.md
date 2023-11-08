---
title: Java8新特性
tag:
  - 编程学习
  - 后端
abbrlink: 397c083a
cover: https://w.wallhaven.cc/full/mp/wallhaven-mpm1o8.png
---

# 函数式接口

如果一个接口中，只声明了一个**抽象方法**，则此接口就称为函数式接口

@FunctionalInterface：显式地声明一下，类似于 @Overwrite，可以做一个验证

```java
@FunctionalInterface//@since 1.8
public interface Runnable {
    public abstract void run();
}
```



## 函数式接口的理解

Java 从诞生日起就是一直倡导“一切皆对象”，在 Java 里面面向对象 (OOP) 编程是一切。但是随着python、scala 等语言的兴起和新技术的挑战，Java 不得不做出调整以便支持更加广泛的技术要求，也即 java 不但可以支持 OOP 还可以支持 OOF (面向函数编程)

在函数式编程语言当中，函数被当做一等公民对待。在将函数作为一等公民的编程语言中，Lambda表达式的类型是函数。但是在 Java8中有所不同。在 Java8 中，Lambda 表达式是对象，而不是函数，它们必须依附于一类特别的对象类型一函数式接口。

简单的说，在 Java8 中，Lambda 表达式就是一个函数式接口的实例。这就是 Lambda 表达式和函数式接口的关系。也就是说，只要一个对象是函数式接口的实例，那么该对象就可以用 Lambda 表达式来表示。

Lambda 表达式可以<span style="color: red; font-weight: bold;">替换</span>之前的匿名内部类。



## Java 内置的四个核心函数式接口

在 java.util.function 包下，定义了很多的函数式接口。

在我们想写一些接口的时候，如果和下面几个类型的接口总体是一样的，就可以直接使用这些接口。

当我们在开发中见到这几个接口的时候，实例化可以使用 lambda 表达式。



消费型接口：接收一个参数，但是没有返回值

供给型接口：没有给参数，但是有返回值

函数型接口：接收一个参数（自变量），返回一个值（因变量）

断定型接口：接收一个参数，返回一个布尔值

![image-20220715211402078](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20220715211402078.png)



引申出来的子接口：

接收参数和返回参数类型一样、接收一个参数变成可以接收两个参数...

![image-20220716173821379](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20220716173821379.png)





**示例**：Consumer 接口

```java
@Test
public void test01() {
    // 使用匿名内部类
    happyTime(500.53, new Consumer<Double>() {
        @Override
        public void accept(Double money) {
            System.out.println("去食堂买了瓶水，价格为" + money + "元");
        }
    });
    // 使用 lambda 表达式
    happyTime(500.53, money -> System.out.println("去食堂买了瓶水，价格为" + money + "元"));
}

public void happyTime(Double money, Consumer<Double> con) {
    con.accept(money);
}
```

输出：

- "去食堂买了瓶水，价格为500.53元"

分析：

- 在 测试方法 test01 中，调用了 happyTime 方法，happyTime 方法中又调用了 accept 方法。

- happyTime 方法其中有个参数是 Consumer 接口，调用的时候，使用匿名内部类实现该接口，重写了 Consumer 接口中的 accept 方法，输出一个字符串。

- 由于动态绑定机制，happyTime 的参数 con 就是匿名内部类，调用的 accept 方法就是匿名内部类中重写的方法。 



**示例**：Predicate 接口

```java
@Test
public void test02() {
    List<String> list = Arrays.asList("北京", "天津", "西京", "普京", "东京");
    // 使用匿名内部类
    List<String> filterList = filterString(list, new Predicate<String>() {
        @Override
        public boolean test(String s) {
            return s.contains("京");
        }
    });
    // 使用 lambda 表达式
    List<String> filterListt = filterString(list, s -> s.contains("京"));
    
    System.out.println(filterList);
}

// 根据给定的规则，过滤集合中的字符串，此规则由 Predicate 的方法决定
public List<String> filterString(List<String> list, Predicate<String> pre) {
    ArrayList<String> filterList = new ArrayList<>();
    for (String s : list) {
        if (pre.test(s)) {
            filterList.add(s);
        }
    }
    return filterList;
}
```

输出：

- [北京, 西京, 普京, 东京]



# Lambda 表达式

Lambda 是一个匿名函数，我们可以把 Lambda 表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。使用它可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使 Java 的语言表达能力得到了提升。

示例：

```java
@Test
public void test2() {
    // 匿名内部类
    Comparator<Integer> com1 = new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return Integer.compare(o1, o2);
        }
    };
    int compare1 = com1.compare(2, 3);
    System.out.println(compare1);
    // lambda 表达式
    Comparator<Integer> com2 = ((o1, o2) -> Integer.compare(o1, o2));
    int compare2 = com2.compare(2, 3);
    System.out.println(compare2);
    // 方法引用
    Comparator<Integer> com3 = Integer::compare;
    int compare3 = com3.compare(2, 3);
    System.out.println(compare3);
}
```



## Lambda表达式的使用

- 1.举例：

  (o1,o2)->Integer.compare(o1,o2)

- 2.格式：

  ->：Lambda 操作符或箭头操作符

  左边：Lambda形参列表（ 其实就是接口中的抽象方法的形参列表 ）

  右边：Lambda体（ 其实就是重写的抽象方法的方法体 ）

- Lambda表达式的本质：作为<span style="color: red; font-weight: bold;">函数式接口</span>的实例（ 实现类的对象实例 ）；要想使用 Lambda 表达式，一定要使用函数式接口 



**语法格式一**：无参，无返回值

```java
@Test
public void test1() {
    Runnable r1 = new Runnable() {
        @Override
        public void run() {
            System.out.println("hello, lambda");
        }
    };
    r1.run();// 只是普通的调用方法，不是多线程

    /*
    Runnable r2 = () -> {
        System.out.println("hello, lambdaaa");
    };
    */
    // 大括号可以省略
    Runnable r2 = () -> System.out.println("hello, lambdaaa");

    r2.run();// 只是普通的调用方法，不是多线程
}
```



**语法格式二**：Lambda 需要一个参数，但是没有返回值

```java
@Test
public void test2() {
    Consumer<String> con = new Consumer<String>() {
        @Override
        public void accept(String s) {
            System.out.println(s);
        }
    };
    con.accept("谎言和誓言的区别是什么？");
    System.out.println("****************");
    Consumer<String> con1 = (String s) -> {
        System.out.println(s);
    };
    con1.accept("一个是听得人当真了，一个是说的人当真了");
}
```



**语法格式三**：数据类型可以省略，因为可由编泽器推断得出，称为：类型推断

```java
Consumer<String> con1 = (s) -> {
    System.out.println(s);
};
```

类似于泛型中的类型推断

```java
int[] arr = new int[]{1,2,3};
// 这也是类型推断
int[] arr = {1,2,3};
```



**语法格式四**：Lambda 若只需要一个参数时，参数的小括号可以省略

```java
Consumer<String> con1 = s -> {
    System.out.println(s);
};
```



**语法格式五**：Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值

```java
Comparator<Integer> com2 = (o1, o2) -> {
    System.out.println(o1);
    System.out.println(o2);
    return o1.compareTo(o2);
};
```



**语法格式六**：Lambda 体只有一条语句时，return 与大括号若有，都可以省略（如果省略了大括号，return 必须省略）

```java
public void test2() {
    Comparator<Integer> com1 = new Comparator<Integer>() {

        @Override
        public int compare(Integer o1, Integer o2) {
            return Integer.compare(o1, o2);
        }
    };
    //////////////////////////////
    Comparator<Integer> com2 = (o1, o2) -> Integer.compare(o1, o2);
}
```



## 总结

左边：Lambda 形参列表的参数类型可以省略（类型推断）如果 Lambda 形参列表只有一个参数，其一对（）也可以省略

右边：Lambda 体应该使用一对 {} 包裹；如果 Lambda 体只有一条执行语句（可能是 return 语句），可以省略这一对 {} 和 return 关键字

**写 lambda 表达式的时候，要知道抽象方法长什么样！**

- 一> 左边就是抽象方法的形参
- 一> 右边就是抽象方法的方法体



# 方法引用

方法引用是 lambda 表达式的深层次表达。通过方法的名字来指向一个方法，可以认为是 Lambda 表达式的一个语法糖。

换句话说，方法引用就是 Lambda 表达式，也就是函数式接口的一个实例。



**使用情境：**

当**要传递给 lambda 体的操作**，已经**有别的方法实现了**，可以使用方法引用。

<span style="color: red; font-weight: bold;">换句话说，就是系统写好的方法或者我们自己写的方法，操作和 lambda 体中要进行的操作是一样的，就直接把写好的方法拿过来用不就行了。</span>

**使用要求：**

对于情况一和情况二，要求接口中的 抽象方法的形参列表 和 返回值类型与方法引用的方法的形参列表 和返回值类型相同（或者是其子类型）。<span style="color: red; font-weight: bold;">这就是方法引用只要写方法名，无需关注方法的参数列表的原因</span>。

**使用格式：**

类（或对象）: : 方法名，参数列表都不用写了。具体来说可分为三种

- 对象**实例** : : 非静态方法（对象实例 : : 静态方法是错的）
- 类 : : 静态方法
- 类 : : 非静态方法 ( **比较难想到要引用的方法，需要对各种方法非常熟悉** )

注：和之前学的静态方法调用是反过来的！

- 之前，类只能调用静态方法，对象可以调用静态 / 非静态方法



**示例：**

- 情况一：**对象实例** : : 非静态方法

```java
// Consumer 中的 void accept(T t)
// PrintStream 中的 void println(T t)
@Test
public void test01() {
    // lambda 表达式
    Consumer<String> con1 = s -> System.out.println(s);
    con1.accept("hello~"); // 输出 hello~
    // 方法引用
    PrintStream ps = System.out;
    Consumer<String> con2 = ps::println; // System.out::println
    con2.accept("method~"); // 输出 method~
}

// Supplier 中的 T get()
// Employee 中的 String getName()
@Test
public void test02() {
    Employee emp = new Employee("Codd");
    // lambda 表达式
    Supplier<String> sup1 = () -> emp.getName();
    System.out.println(sup1.get()); // 输出 Codd
    // 使用 方法引用
    Supplier<String> sup2 = emp::getName;
    System.out.println(sup2.get()); // 输出 Codd
}
```



Employee 类

```java
class Employee {
    private String name;

    public Employee(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
}
```



- 情况二：类 : : 静态方法

```java
// Comparator 中的 int compare(T t1,T t2)
// Integer 中的 int compare(T t1,T t2)
@Test
public void test01() {
    // lambda 表达式
    Comparator<Integer> com1 = (t1, t2) -> Integer.compare(t1, t2);
    System.out.println(com1.compare(12, 21)); // -1
    // 方法引用
    Comparator<Integer> com2 = Integer::compare;
    System.out.println(com2.compare(21, 21)); // 0
    // 还能这么写，这是第三种情况
    Comparator<Integer> com3 = Integer::compareTo;
}

// Function 中的 R apply(T t)
// Math 中的 Long round(Double d)
@Test
public void test02() {
    // lambda 表达式
    Function<Double, Long> fun1 = t -> Math.round(t);
    System.out.println(fun1.apply(12.3)); // 12 
    // 方法引用
    Function<Double, Long> fun2 = Math::round;
    System.out.println(fun2.apply(12.6)); // 13
}
```



- 情况三：类 : : 非静态方法

```java
// Comparator 中的 int compare(T t1,T t2)
// String 中的 int t1.compareTo(t2)
// 要实现的抽象方法有两个参数，第一个参数作为要引用的方法的调用者
@Test
public void test01() {
    // lambda 表达式
    Comparator<String> com1 = (t1, t2) -> t1.compareTo(t2);
    System.out.println(com1.compare("abc", "abd")); // -1
    // 方法引用
    Comparator<String> com2 = String::compareTo;
    System.out.println(com2.compare("abc", "abe")); // -2
}

// BiPredicate 中的 boolean test(T t1,T t2)
// String 中的 boolean t1.equals(t2)
// 要实现的抽象方法有两个参数，第一个参数作为要引用的方法的调用者
@Test
public void test02() {
    // lambda 表达式
    BiPredicate<String, String> pre1 = (t1, t2) -> t1.equals(t2);
    System.out.println(pre1.test("abc", "abc")); // true
    // 方法引用
    BiPredicate<String, String> pre2 = String::equals;
    System.out.println(pre2.test("abc", "abd")); // false
}

// Function 中的 R apply(T t)
// Employee 中的 String t.getName()
// 要实现的抽象方法有一个参数，作为要引用的方法的调用者，在这里这个 t 就是 Employee 类的对象实例
@Test
public void test03() {
    Employee employee = new Employee("Jerry");
    // lambda 表达式
    Function<Employee, String> fun1 = emp -> emp.getName();
    System.out.println(fun1.apply(employee)); // Jerry
    // 方法引用
    Function<Employee, String> fun2 = Employee::getName;
}
```



# 构造器引用

和方法引用类似，函数式接口的抽象方法的形参列表和构造器的形参列表一致，抽象方法的返回类型即为构造器创建的对象的类型



**示例：**

```java
// Supplier 中的 T get()
// Employee 的无参构造器 Employee(), 可以看做返回了一个 Employee 的实例
// 即两者类型相似，因此可以使用方法引用
@Test
public void test01() {
    // lambda 表达式
    Supplier<Employee> sup1 = () -> new Employee();
    // 构造器引用
    Supplier<Employee> sup2 = Employee::new;
    Employee employee = sup2.get();
}

// Function 中的 R apply(T t)
// Employee 的单参构造器 Employee(String name)
@Test
public void test02() {
    // lambda 表达式
    Function<String, Employee> fun1 = name -> new Employee(name);
    // 构造器引用
    Function<String, Employee> fun2 = Employee::new;
    Employee employee = fun2.apply("Tom");
}

// BiFunction 中的 R apply(T t,U u)
// Employee 构造器 Employee(String name, int age)
@Test
public void test03() {
    // lambda 表达式
    BiFunction<String, Integer, Employee> fun1 = (name, age) -> new Employee(name, age);
    // 构造器引用
    BiFunction<String, Integer, Employee> fun2 = Employee::new;
    Employee employee = fun2.apply("Codd", 99);
}
```



**注：**

- 问：如果有两个单参数 int 类型的构造器，如何判断引用的是哪一个构造器？？？

  有两个单参数的 int  类型的构造器已经不满足构造器重载了，会直接报错。

<img src = "https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20220717213846913.png" width = "70%"/>



- 如何决定构造器是哪一个？

  前面指定的泛型就可以确定是使用哪一个构造器。如：当有多个单参的构造器时，就会就通过前面指定的泛型来进行区分。



# 数组引用

数组引用可以看成特殊的构造器引用。

**示例：**

```java
//Function 中的 R apply(T t)
@Test
public void test01() {
    // 返回一个数组，Integer 指定返回数组长度，String 指定返回数组的类型
    // lambda 表达式
    Function<Integer, String[]> fun1 = length -> new String[length];
    // 数组引用
    Function<Integer, String[]> fun2 = String[]::new;
    String[] arr = fun2.apply(5);
}
```



# Stream API

Stream API ( java.util.stream ) 把真正的**函数式编程风格**引入到 Java 中，是目前为止对 java 类库最好的补充，因为 Stream API 可以极大提高 java 程序员的生产力，让程序员写出高效率、干净、简洁的代码。

使用 Stream API 对集合数据进行操作，就类似于使用 SQL 对表中数据进行查询。



### 为什么要使用?

对于关系型数据库（如 MySQL，Oracle），当查询数据时，如果带有筛选条件，可以直接在 SQL 上增加过滤条件（如 where），这样从数据库查询出来的数据就是筛选之后的数据了，后端只需把这些数据返回给前端就行。（**过滤操作在 SQL 语言层面进行）**

对于非关系型数据库（如 MongDB，Redis），查询数据如果带有过滤条件，需要在 Java 层面对查询出来的数据进行处理，这就需要 Stream API。（**过滤操作在  Java 层面进行**）



### Stream 和 Collection 的区别？

- **Collection 与数据有关**，是内存层面的容器，用来存储多个数据，**面向内存**，存储在内存中

- **Steam 与计算有关**，用来操作容器中数据（一系列的中间操作：过滤、映射、归约、排序等），**面向 CPU**，通过 CPU 实现计算 



### 注意

- Stream 自己不会存储元素。和迭代器一样，迭代器本身不存储数据，数据存储在集合中。

- Stream 不会改变源对象。相反，他们会返回一个持有结果的新 Stream。（和 String 一样，不变性）

- Stream 作是延迟执行的。这意味着他们会等到需要结果的时候才执行。**只要不调用终止操作，一系列的中间操作都不会执行。一旦调用了终止操作，就不能再调用中间操作了。**



## Stream 操作的三个步骤

- 创建 Stream

  一个数据源（如：集合、数组），获取一个流

- 中间操作

  一个**中间操作链**，对数据源的数据进行处理

- 终止操作

  一旦执行终止操作，就（才）执行中间操作链，并产生结果。之后，不会再被使用。（**要想再次使用 Steam，需要重新创建一个 Steam，否则会报错**）

![image-20220718140042483](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20220718140042483.png)



### 1. 创建 Steam

创建 Steam 有四种方式。



#### 1）通过 Collection 接口中的默认方法

Java8 中的 Collection 接口被扩展，提供了两个获取流的方法。

- default Stream<E>stream()：返回一个顺序流

- default Stream<E>parallelStream()：返回一个并行流

**并行流：**（list 是有序的）并行流相当于同时有多个线程去集合中取数据，顺序就不一定是 list 中顺序了。



**示例：**

```java
ArrayList<Employee> list = new ArrayList<>();
Stream<Employee> stream = list.stream();
Stream<Employee> parallelStream = list.parallelStream();
```



#### 2）通过数组

- static <T> Stream <T> stream(T[] array)：返回一个流

重载形式，处理对应类型的数组：

- public static IntStream stream(int[] array）
- public static LongStream stream(long[] array)
- public static DoubleStream stream(double[] array)

**示例：**

```java
int[] arr = {1, 2, 3};
IntStream stream = Arrays.stream(arr);
// 指定泛型
Employee[] employees = new Employee[2];
Stream<Employee> employeeStream = Arrays.stream(employees);
```



#### 3）通过 Stream 的 of()

**示例：**

```java
// 注意这里放的是包装类，不是基本数据类型
Stream<Integer> integerStream = Stream.of(1, 2, 3);

Employee e1 = new Employee();
Employee e2 = new Employee();
Employee e3 = new Employee();
Stream<Employee> employeeStream = Stream.of(e1, e2, e3);
```



#### 4）创建无限流

- 无限流可以帮我们造数据。

- 可以使用静态方法 Stream.iterate() 和 Stream.generate() 创建无限流。

**示例：**

```java
// 从 0 开始，输出前十个偶数
Stream.iterate(0, t -> t + 2).limit(10).forEach(System.out::println);
// 产生十个随机数
Stream.generate(Math::random).limit(10).forEach(System.out::println);
```



### 2. 中间操作

中间操作就像一条流水线。这条流水线除非触发终止操作，否则不会执行任何处理。



#### 1）筛选与切片

- filter(Predicate p)：接收 Lambda，从流中排除某些元素
- limit(long maxSize)：截断流，使其元素不超过给定数量，返回 n 个元素的流
- skip(long n)：跳过前 n 个元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个**空流**（都跳过了，啥也没有）。**与 limit(n) 互补**。
- distinct()：筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素

**示例：**

```java
@Test
public void test01() {
    List<Employee> list = Employee.getEmployees();
    // filter
    Stream<Employee> stream = list.stream();
    // 查询员工表中薪资大于 30000 的员工信息
    stream.filter(employee -> employee.getSalary() > 30000).forEach(System.out::println);
    System.out.println("===================================");
    // limit
    // 查询前三条员工信息
    list.stream().limit(3).forEach(System.out::println);
    System.out.println("===================================");
    // skip
    // 跳过前三条员工信息
    list.stream().skip(3).forEach(System.out::println);
    System.out.println("===================================");
    // distinct
    // 查出所有员工信息，并去除重复元素
    // Employee 中已经重写了 equal 和 hashCode 方法
    list.stream().distinct().forEach(System.out::println);
}

// getEmployees 方法
public static List<Employee> getEmployees() {
    List<Employee> list = new ArrayList<>();
    list.add(new Employee(1001, "Tom", 12, 30000));
    list.add(new Employee(1002, "Tyler", 13, 30000));
    list.add(new Employee(1003, "Jerry", 11, 50000));
    list.add(new Employee(1004, "Codd", 29, 50000));
    // 插入一条重复数据
    list.add(new Employee(1003, "Jerry", 11, 50000));

    return list;
}
```



**输出信息：**

```java
Employee{eid=1003, name='Jerry', age=11, salary=50000.0}
Employee{eid=1004, name='Codd', age=29, salary=50000.0}
Employee{eid=1003, name='Jerry', age=11, salary=50000.0}
===================================
Employee{eid=1001, name='Tom', age=12, salary=30000.0}
Employee{eid=1002, name='Tyler', age=13, salary=30000.0}
Employee{eid=1003, name='Jerry', age=11, salary=50000.0}
===================================
Employee{eid=1004, name='Codd', age=29, salary=50000.0}
Employee{eid=1003, name='Jerry', age=11, salary=50000.0}
===================================
Employee{eid=1001, name='Tom', age=12, salary=30000.0}
Employee{eid=1002, name='Tyler', age=13, salary=30000.0}
Employee{eid=1003, name='Jerry', age=11, salary=50000.0}
Employee{eid=1004, name='Codd', age=29, salary=50000.0}
```



#### 2）映射

- map(Function f)：接收一个函数作为参数，将元素转换成其他形式或提取信息。该函数会被**应用到每个元素上**，并将其映射成一个新的元素。

- flatMap(Function f)：接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流



**map 使用示例：**

```java
@Test
public void test01() {
    List<String> list = Arrays.asList("aa", "bb", "cc", "dd");
    // 1. 把所有元素变成大写
    list.stream().map(s -> s.toUpperCase()).forEach(System.out::println);
    System.out.println("===================================");
    // 2. 获取员工名字长度大于 4 的员工姓名
    List<Employee> employees = Employee.getEmployees();
    Stream<Employee> stream = employees.stream();
    // 获得由员工姓名组成的流对象
    Stream<String> nameStream = stream.map(Employee::getName);
    nameStream.filter(name -> name.length() > 4).forEach(System.out::println);
}
```

**map 和 flatMap 的区别：**

两者的区别就像 list 中的 add 和 addAll 方法。

```java
ArrayList list1 = new ArrayList();
list1.add(1);
list1.add(2);
list1.add(3);
ArrayList list2 = new ArrayList();
list1.add(list2); // list1 中有4个元素，list2 作为一整个元素加入 list1 中
list1.addAll(list2); // list1 中有6个元素，每个元素都被单独取出加入 list1 中
```

 map 中如果有流，就会把整个流当成一个元素

flatMap 中如果有流，会把流打散，取出流中的每一个元素作为一个一个新的元素

**示例：**

```java
@Test
public void test01() {
    List<String> list = Arrays.asList("aa", "bb", "cc", "dd");
    // map 会对集合中的每个元素进行操作,每个元素都调用 fromStingToStream
    // streamStream 就是一个流里面的元素还是流，也就是说 streamStream 中有四个流对象 {a a} {b b} {c c} {d d}
    Stream<Stream<Character>> streamStream = list.stream().map(str -> fromStingToStream(str));
    // 如果想要取出每个元素查看，需要两层 forEach
    streamStream.forEach(stream -> stream.forEach(System.out::println)); // a a b b c c d d

    // 使用 flatMap
    // 这里获得的流对象，每一个元素都是 character
    Stream<Character> characterStream = list.stream().flatMap(str -> fromStingToStream(str));
    characterStream.forEach(System.out::println); // a a b b c c d d
}

// 将字符串中的多个字符构成的集合转换为对应 Stream 的实例
public Stream<Character> fromStingToStream(String str) {
    ArrayList<Character> list = new ArrayList<>();
    for (Character c : str.toCharArray()) {
        // 如 str 为："Tom"，则 list 中的元素有三个，T，o，m
        list.add(c);
    }
    return list.stream();
}
```



#### 3）排序

java 中，和排序相关的接口有两个：Compare 和 Comparator 

- sorted()：自然排序，需要实现 Compare 接口

- sorted(Comparator com)：定制排序

**示例：**

```java
@Test
public void test01() {
    // 自然排序，从小到大
    List<Integer> list = Arrays.asList(12, 43, -1, 34, 99);
    list.stream().sorted().forEach(System.out::println);
    // 定制排序
    List<Employee> employees = Employee.getEmployees();
    // 抛异常，原因：Employee 没有实现 Comparable 接口
    //employees.stream().sorted().forEach(System.out::println);
    employees.stream().sorted((e1, e2) -> {
        // 按照年龄排序，如果年龄相同，按照薪水排序
        int compareValue = Integer.compare(e1.getAge(), e2.getAge());
        if (compareValue != 0) {
            return compareValue;
        } else {
            return Double.compare(e1.getSalary(), e2.getSalary());
        }
    }).forEach(System.out::println);
}
```



### 3. 终止操作

流进行了终止操作，不能再次使用。

终止操作会产生结果，可以是任何不是流的值，如 List，Integer，甚至是 void。



#### 1）匹配与查找

都是一些简单地 API。

- allMatch(Predicate p)：检香是否匹配所有元素

- anyMatch(Predicate p)：检香是否至少匹配一个元素
- noneMatch(Predicate p)：检查是否没有匹配的元素
- findFirst()：返回第一个元素
- findAny()：返回当前流中的任意元素。如果是顺序流（stream），会取第一个元素；如果是并行流（parallelStream），就是任意一个。

- count()：返回流中元素的总个数
- max(Comparator c)：返回流中最大值
- min(Comparator c)：返回流中最小值
- forEach(Consumer c)：内部迭代（使用 iterator 的方式称为外部迭代，就是说我们外部拿到 iterator 这个指针，用 iterator 遍历集合）

**示例：**

```java
List<Employee> employees = Employee.getEmployees();
// allMatch 是否所有员工的年龄都大于 18
boolean b = employees.stream().map(Employee::getAge).allMatch(age -> age > 10);
// 直接拿 Employee 操作也是可以的
boolean allMatch = employees.stream().allMatch(e -> e.getAge() > 18);
// anyMatch 是否存在员工的工资大于 10000
boolean anyMatch = employees.stream().anyMatch(e -> e.getSalary() > 10000);
// noneMatch 查看是否有员工姓”雷“，没有则返回 true
boolean noneMatch = employees.stream().noneMatch(e -> e.getName().startsWith("雷"));
// findFirst
Optional<Employee> first = employees.stream().findFirst();
// findAny
Optional<Employee> any = employees.parallelStream().findAny();
// count
long count = employees.stream().filter(e -> e.getAge() > 18).count();
// max 求最高的工资
Optional<Double> maxSalary = employees.stream().map(Employee::getSalary).max(Double::compareTo);
// min 求最低工资的员工（注意和上面的区别）
Optional<Employee> e = employees.stream().min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));
// forEach 内部迭代
employees.stream().forEach(System.out::println);
// 使用集合的遍历操作
employees.forEach(System.out::println);
```



#### 2）归约

- reduce(T iden, BinaryOperator b)：可以将流中元素反复结合起来，得到一个值，返回 T。**第一个参数相当于初始值，从初始值执行操作**。

- reduce(BinaryOperator b)：可以将流中元素反复结合起来，得到一个值，返回 Optional<T>

备注：map 和 reduce 的连接通常称为 map-reduce 模式，因 Google 用它来进行网络搜索而出名。

**示例：**

```java
// 计算 1-10 自然数的和
Integer sum1 = Stream.iterate(1, t -> t + 1).limit(10).reduce(0, (n1, n2) -> n1 + n2);
// 或者这样写
Integer sum2 = Stream.iterate(1, t -> t + 1).limit(10).reduce(0, Integer::sum);
System.out.println(sum1 + " " + sum2); // 55
// 计算所有员工的工资总和
List<Employee> list = Employee.getEmployees();
Optional<Double> sumSalary1 = list.stream().map(Employee::getSalary).reduce(Double::sum);
// 或者这样写
Optional<Double> sumSalary2 = list.stream().map(Employee::getSalary).reduce((s1, s2) -> Double.sum(s1, s2));
System.out.println(sumSalary1 + " " + sumSalary2); 
```



#### 3）收集

- collect(Collector c)：将流转换为其他形式。用于给 Stream 中的元素做汇总

- Collectors：收集者，提供了很多静态方法，如 toList，toSet，toCollection，可以方便地创建常见收集器实例

**示例：**

```java
// 查询工资大于 30000 的员工，结果返回一个 List 或 Set
List<Employee> list = Employee.getEmployees();
List<Employee> filterList = list.stream().filter(e -> e.getSalary() > 30000).collect(Collectors.toList());
filterList.forEach(System.out::println);
Set<Employee> filterSet = list.stream().filter(e -> e.getSalary() > 30000).collect(Collectors.toSet());
filterSet.forEach(System.out::println);
```



# Optional 类

**Optional 类是为了在程序中避免出现空指针而创建的。**

以前，为了解决空指针异常，Google 公司著名的 Guava 项目引入了 Optional 类，Guava 通过使用检查空值的方式来防止代码污染。受到 Google Guava 的启发，Optional 类已经成为 Java8 类库的一部分。

Optional<T>类（java.util.Optional）是一个容器类，它可以保存类型 T 的值（存储在该类的 value 成员变量中）。

### 常用方法

Optional 提供很多有用的方法，这样我们就不用显式进行空值检测。

#### 创建Optional 类对象

- Optional.of(T t)：创建一个 Optional 实例，**t 必须非空**，用处不大
- Optional.empty()：创建一个空的 Optional 实例
- Optional.ofNullable(T t)：**t 可以为 null** 

**示例：**

```java
Employee employee = null;
Optional<Employee> o1 = Optional.of(employee);
System.out.println(o1);// NullPointerException
Optional<Employee> o2 = Optional.ofNullable(employee);
System.out.println(o2); // Optional.empty
```



### 举例说明 Optional 类处理空指针

- orElse(T ohter)：如果有值则将其返回，否则返回指定的 other 对象，这样就保证了非空（鸡肋）



**使用 if-else 处理空指针：**

```java
@Test
public void test01() {
    Boy boy = null;
    getGirlName(boy);
}

// 没有处理空指针
public String getGirlName(Boy boy) {
    return boy.getGirl().getName();
}

// 简单处理一下空指针
public String getGirlName(Boy boy) {
    if (boy == null) return null;
    if (boy.getGirl() == null) return null;
    return boy.getGirl().getName();
}
```



**使用 Optional 类处理空指针：**

```java
@Test
public void test01() {
    // 正常情况
    Boy boy = new Boy(new Girl("HiBoy"));
    System.out.println(getGirlName(boy));
    // boy 为空
    Boy b1 = null;
    System.out.println(getGirlName(b1));// HiGirl
    // girl 为空
    Boy b2 = new Boy(null);
    System.out.println(getGirlName(b2));// HelloGirl
}

public String getGirlName(Boy boy) {
    Optional<Boy> optionalBoy = Optional.ofNullable(boy);
    // 如果传进来的 boy 为空，b1 就是 orElse 中指定的，否则 b1 就是传进来的 boy
    Boy b1 = optionalBoy.orElse(new Boy(new Girl("HiGirl")));

    Girl girl = b1.getGirl();
    Optional<Girl> optionalGirl = Optional.ofNullable(girl);
    // 如果 boy 不为空 而 girl 为空，g1 就是 orElse 中指定的
    Girl g1 = optionalGirl.orElse(new Girl("HelloGirl"));
    return g1.getName();
}
```



使用 Optional 类中的 get() 方法，调用对象的值：

```java
Optional<Double> sumSalary2 = list.stream().map(Employee::getSalary).reduce((s1, s2) -> Double.sum(s1, s2));
System.out.println(sumSalary2.get());
```

