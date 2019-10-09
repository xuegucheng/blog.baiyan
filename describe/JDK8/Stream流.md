## Stream流

**作用：为了简化我们数组与集合的操作**

* **Stream流的生成方式**
  * 生成流
    * 通过数据源（集合，数组等）生成流
  * 中间操作
    * 一个流后面可以跟随多个零个或多个中间操作，其目的是为了打开流，做出某些程度的数据过虑/映射，然后返回一个新的流并交给下一个操作使用
  * 终结操作
    * 一个流只能有一个终结操作，当这个操作执行后，流就被使用"光"了，无法再被操作。所以这必定是流的最后一个操作

* **如何获取Stream流**

  1. Collection体系的集合可以使用默认方法stream()生成流
  2. Map体系的集合间接的生成流
  3. 数据可以通过Stream接口的静态方法`of(T ...values)`生成流

* **如何使用Stream流**

  1. 对此流的每个元素执行操作，属于流的终结操作
  
  ```java
  void  forEach(Consumer<>)
  ```
  
  2. 返回boolean类型函数式接口,用于对流中的数据进行过滤
  
  ```java
  Stream<T> filter(Predicate<>)
  ```
  
  3. 用于类型转换的函数式接口
  
  ```java
  Stream map(Function<String,Integer>)
  ```
  
  4. 用于截取指定参数个数的数据，maxSize代表要截取的数量
  
  ```java
  Stream<T> limit(long maxSize)
  ```
  
  5. 用于跳过指定参数个数的数据，返回由该流的剩余元素组成的流
  
  ```java
  Stream<T> skip(long n)
  ```
  
  6. 求数据总数量，属于流的终结操作
  
  ```java
  long count()
  ```
  
  7. 将Stream流合并
  
  ```java
  static<T> Stream<T> concat(Stream A,Stream B)
  ```
  
  8. 返回由该流的不同元素（根据`Objectequals(Object)`）组成的流
  
  ```java
  Stream<T> distinct()
  ```
  
  9. 1返回由该流的元素组成的流，根据提供的`Comparator`进行排序
  
  ```java
  Stream<T> sorted(Comparator<>)
  //Comparator接口中的方法 int compare(T O1,T O2)
  ```
  
* **如何引用Stream流**

  * 对象的方法引用

  ```java
  ()->{new Person().toString()}
  //这个是方法引用写法,但是前提是person对象已经创建了
  person::toString
  ```

  * 静态方法的方法引用

  ```java
  (i)->{Math.abs(i)}
  //这个是静态方法引用
  Math::abs
  ```

  * super：调用父类的method方法

  ```java
  super::method
  ```

  * this：调用本类中的method方法

  ```java
  this::method
  ```

  * 调用构造方法

  ```java
  (String name)->{return new Person(name);}
  //以下为简写形式，但是要确保该方法只能传递一个参数
  Person::new
  ```

  * 数组的构造方法

  ```java
  (Integer len)->{return new int[len];}
  //以下为简写形式，但是要确保该方法只能传递一个参数
  int[]::new	
  ```

* **Stream流的收集操作**

  对数据使用Stream流的方式操作完毕后，我想把流中的数据收集到集合中，该怎么办呢

  Stream流的收集方式

  ```java
  R collect(Collect<>)
  ```

  > 但是这个收集方法的参数是一个Collector接口

  工具类`Collectors`提供了具体的收集方式

  * `public static <T> Collector toList()`：把元素收集到List集合中
  * `public static <T> Collector toSet()`：把元素收集到Set集合中
  * `public static Collector toMap(Function keyMapper,Function valueMapper)`：把元素收集到Map集合中