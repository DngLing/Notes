## Lambda表达式  
  lambda表达式的意义是为了快速方便的实现接口，例如我们在创建Runnable 接口时
  ```java
  Runnable runnable = new Runnable() {
        @Override    
        public void run(){
            System.out.println("I am running!");
            }
       };
  ```
  或者我们会使用匿名类。
  对于这种接口中只有单一方法的，我们都可以用Lamda表达式来快速创建，

  ```java
  Runnable runnable = () -> System.out.println("I am Running!");
  ```

  ### 一、Lambda 表达式的语法  
  Lambda 表的式的基本语法为 `Interface interface =  (AnyType ... param) -> { code block };` 其中:
  * 圆括号()中输入参数，该参数的数量与实现单一方法接口中的方法数量一样，参数名一样，但是可以省略参数类型
  * 花括号中{}像正常的实现方法一样
  * 两个括号的内容用 **->** 连接

  ### 二、Lambda 表达式的精简
  #### 1. 单行代码可以省略 { } 花括号
  ``` java
  Runnable run = () ->{ System.out.println("nihao"); } 
  ```

  ```java
  Runnable runnable = () -> System.out.println("I am Running!");
  ```
  ####  2. 单个参数可以省略()圆括号
  ```java
  Interface inter = i -> System.out.println("I am Running!");
  ```

  #### 3. 需要返回值单只有一行代码时，可以省略return 关键字
  ```java
  Interface inter = i -> i;
  ```

  ### 三、Lambda 的语法进阶

  #### 1.方法引用
  当需要的实现的接口方法已经实现，或者同时有几个方法需要实现同一个方法时，可以使用方法引用来简化操作：
  ```java
  public static void main(String[] args) {  
    //注意这里时静态方法引用静态方法   
    SingleParamReturn  singleParamReturn1 = a -> test1(a);
  }
  public static int test1(int a){
    return a;
 }
  ```
或者使用更简洁的语法：
方法隶属者(静态方法-类，非静态方法-this对象) :: 方法名
```java
  public static void main(String[] args) {  
    //注意这里时静态方法引用静态方法   
    SingleParamReturn  singleParamReturn1 = LamdaTest :: test1;
  }
  public static int test1(int a){
    return a;
 }
```
 ***注意：参数类型，数量，顺序要一样，返回值的类型也要一样***

 #### 2.构造方法的引用
 当接口中的方法需要新建返回某个对象的实例时，我们可以进行构造方法引用来完成：
 语法： ```Interface in = ClassName :: new;```  这时 Interface中接口方法的参数数量，类型，顺序都要与该类中指定的构造构造方法保持一致，否则就会报错；



### 三、综合案例

#### 集合排序 ：ArrayList的排序方法

**需求: 使用ArrayList的sort(Compare<? extends T >)将Person对象按照年龄排序**

```java
public class Person {
    public String name;
    public int age;
    

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "name = "+name+" age = "+age;
    }
}
```

```java
public static void main(String[] args) {
    //需求: 按照年龄排序
    ArrayList<Person> people = new ArrayList<>();
    people.add(new Person("xiaopeng",11));
    people.add(new Person("xiaoHua",12));
    people.add(new Person("xiaolili",13));
    people.add(new Person("tang",14));
    people.add(new Person("fangfnag",16));
    people.add(new Person("donggua",17));
    people.add(new Person("fanqie",15));
    people.add(new Person("magua",18));
    //通过Lambda表达式 来实现Compare接口
    people.sort((o1, o2) -> o2.age - o1.age);
    System.out.println(people);
}
```

由于sort 方法中需要传入一个Compare接口的实现，而Compare接口是一个被声明为 `@FunctionalInterface`的接口，我们只需要去实现他的 `int compare(T o1, T o2);`方法即可。



### 四、系统内置的函数式接口

#### 1. Predicate<T> 

参数： T 泛型

返回值：boolean

需完成方法： ` boolean test(T t)`

适用于：返回指定类型的值



#### 2.Consumer<T> 

参数：T 泛型

返回值：void

需完成方法：`void accept(T t)`

适用于：对数据进行一些操作



#### 3. Function<T>

参数：T 泛型

返回值：R 泛型

需完成方法：`R accept(T t)`

适用于：处理数据T 并返回数据R



#### 4. UnaryOperator<T> extends Function<T>

参数：T 泛型

返回值：T 泛型

需完成方法：` identity()`

适用于：继承于Function<T> 处理T并返回T



#### 5. BiFunction<T, U, R> 

参数：T ,U泛型

返回值：R 泛型

需完成方法：`R apply(T t, U u);`

适用于：处理 T,U 数据并返回 R数据



#### 6. BinaryOperator<T>  extends  BiFunction<T, T ,T>

参数：T ,T泛型

返回值：T 泛型

需完成方法：`T apply(T t1, T t2);`

适用于：处理 T, T  数据并返回 T 数据



#### 7. BiPredicate<T, U>

 参数：T ,U泛型

返回值：boolean 泛型

需完成方法：`boolean test(T t, U u);`

适用于：处理T,U 返回boolean 类型



 ### 五、闭包问题

#### 1.  当闭包中的局部变量需要被多次调用式，该变量的作用域会被提升，是的我们可以在其它地方调用它。

实例：

```java
public class CloserDemo1 {

    public static void main(String[] args) {

        int n = getSupplier() //执行完getSupplier() 方法后方法中的局部变量i，没有被销毁
                .get();       //获得这个局部变量
        
        System.out.println(n);
    }

    //得到一个Supplier<Integer> 的实例对象
    public static Supplier<Integer> getSupplier(){
        // 局部变量
        int i = 10;
        //使用lanbda 实现Supplier接口，并返回这个实例
        return ()-> i;
    }
}
```



#### 2.这里在闭包情况下使用的了一个局部变量，那么这个局部变量必须是一个常量

```java
public class CloserDemo2 {
    public static void main(String[] args) {
        int i = 10;
        //这里的i 已经是一个被系统编译修饰成final i不能被修改，例如i++不会通过编译
        Consumer<Integer> consumer = integer -> System.out.println(i);

        consumer.accept(1200);
    }
}
```