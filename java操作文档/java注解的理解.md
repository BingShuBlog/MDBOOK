# java注解的理解

## 一、元注解（meta-annotation）

​    元注解的作用就是负责注解其他注解 

### 1.@Target

表示作用的位置

```
@Target(ElementType.TYPE)   //接口、类、枚举、注解
@Target(ElementType.FIELD) //字段、枚举的常量
@Target(ElementType.METHOD) //方法
@Target(ElementType.PARAMETER) //方法参数
@Target(ElementType.CONSTRUCTOR)  //构造函数
@Target(ElementType.LOCAL_VARIABLE)//局部变量
@Target(ElementType.ANNOTATION_TYPE)//注解
@Target(ElementType.PACKAGE) ///包   
```



### 2.@Retention

RetentionPolicy.SOURCE  : 注解只保留在源文件中

RetentionPolicy.CLASS  : 注解保留在class文件中，在加载到JVM虚拟机时丢弃

RetentionPolicy.RUNTIME  : 注解保留在程序运行期间，此时可以通过反射获得定义在某个类上的所有注解。



### 3.@Documented 

​        被修饰的注解会生成到javadoc中 

​     

```
命令：javadoc a.java
```



### 4.@Inherited 

***

　　@Inherited 元注解是一个标记注解，@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的注解类型被用于一个class，则这个注解将被用于该class的子类。

　　注意：@Inherited 注解类型是被标注过的class的子类所继承。类并不从它所实现的接口继承注解，方法并不从它所重载的方法继承注解。

　　当@Inherited注解类型标注的注解的Retention是RetentionPolicy.RUNTIME，则反射API增强了这种继承性。如果我们使用java.lang.reflect去查询一个@Inherited 注解类型的注解时，反射代码检查将展开工作：检查class和其父类，直到发现指定的注解类型被发现，或者到达类继承结构的顶层。