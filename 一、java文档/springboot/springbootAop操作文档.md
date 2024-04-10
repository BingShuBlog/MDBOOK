# SpringBoot Aop操作文档

简介：aop是spring的两大功能模块之一，功能非常强大，为解耦提供了非常优秀的解决方案。 



#### AOP常用术语：

##### 1.连接点(Joinpoint)

  增强程序执行的某个特定位置(要在哪个地方做增强操作)。Spring仅支持方法的连接点，既仅能在方法调用前，方法调用后，方法抛出异常时等这些程序执行点进行织入增强。

##### 2.切点（Pointcut）

  切点是一组连接点的集合。AOP通过“切点”定位特定的连接点。通过数据库查询的概念来理解切点和连接点的关系再适合不过了：连接点相当于数据库中的记录，而切点相当于查询条件。

##### 3.增强（Advice）

  增强是织入到目标类连接点上的一段程序代码。表示要在连接点上做的操作。

##### 4.切面（Aspect）

  切面由切点和增强（引介）组成(可以包含多个切点和多个增强)，它既包括了横切逻辑的定义，也包括了连接点的定义，SpringAOP就是负责实施切面的框架，它将切面所定义的横切逻辑织入到切面所指定的链接点中。





### 一：使用aop来完成全局请求日志处理

1.创建springBoot项目添加如下引用

```
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```



2.创建一个Controller

```
/**
 * create by hsj on 2018/5/18
 */
@RestController
@RequestMapping("/")
public class FirstController {

    @RequestMapping("/first")
    public String first() {
        return "first controller";
    }
}
```



3.创建切面类

```
/**
 * @Pointcut("execution(public * com.example.controller.*.*(..))")
 * create by hsj on 2018/5/18
 */
@Aspect
@Component
@Slf4j
public class LogAspect {

    //
    @Pointcut("execution(public * com.nbkj.aspect.controller.*.*(..))")
    public void webLog() {
        log.info("调用 weblog");
    }


    @Before("webLog()")
    public void deBefore(JoinPoint joinPoint) throws Throwable {
        // 接收到请求，记录请求内容
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        // 记录下请求内容
        log.info("URL : " + request.getRequestURL().toString());
        log.info("HTTP_METHOD : " + request.getMethod());
        log.info("IP : " + request.getRemoteAddr());
        log.info("CLASS_METHOD : " + joinPoint.getSignature().getDeclaringTypeName() + "." + joinPoint.getSignature().getName());
        log.info("ARGS : " + Arrays.toString(joinPoint.getArgs()));
    }

    @AfterReturning(returning = "ret", pointcut = "webLog()")
    public void doAfterReturning(Object ret) throws Throwable {
        // 处理完请求，返回内容
        System.out.println("方法的返回值 : " + ret);
    }

    //后置最终通知,final增强，不管是抛出异常或者正常退出都会执行
    @After("webLog()")
    public void after(JoinPoint jp){
        log.info("方法最后执行.....");
    }

    //后置异常通知
    @AfterThrowing("webLog()")
    public void throwss(JoinPoint jp){
        System.out.println("方法异常时执行.....");
    }


    //环绕通知,环绕增强，相当于MethodInterceptor
    @Around("webLog()")
    public Object arround(ProceedingJoinPoint pjp) {
        System.out.println("方法环绕start.....");
        try {
            Object o =  pjp.proceed();
            System.out.println("方法环绕proceed，结果是 :" + o);
            return o;
        } catch (Throwable e) {
            e.printStackTrace();
            return null;
        }
    }
}
```



### 二： 切面方法说明

@Aspect

作用是把当前类标识为一个切面供容器读取

@Before
标识一个前置增强方法，相当于BeforeAdvice的功能

@AfterReturning

后置增强，相当于AfterReturningAdvice，方法退出时执行

@AfterThrowing

异常抛出增强，相当于ThrowsAdvice

@After

final增强，不管是抛出异常或者正常退出都会执行

@Around

环绕增强，相当于MethodInterceptor



### 四：关于切面PointCut的切入点

 

##### 1.execution切点函数

（1） execution函数用于匹配方法执行的连接点，语法为：

​                       

```
 execution( 方法修饰符(可选)  返回类型  方法名  参数  异常模式(可选) ) 
```



（2）参数部分允许使用通配符：

| *    | 匹配任意字符，但只能匹配一个元素                             |
| ---- | ------------------------------------------------------------ |
| ..   | 匹配任意字符，可以匹配任意多个元素，表示类时，必须和*联合使用 |
| +    | 必须跟在类名后面，表示类本身和继承或扩展指定类的所有         |

参考：http://blog.csdn.net/autfish/article/details/51184405

 

（3）除了execution()，Spring中还支持其他多个函数，这里列出名称和简单介绍，以方便根据需要进行更详细的查询

| @annotation() | 表示标注了指定注解的目标类方法例如 @annotation(org.springframework.transaction.annotation.Transactional) 表示标注了@Transactional的方法 |
| ------------- | ------------------------------------------------------------ |
| args()        | 通过目标类方法的参数类型指定切点 例如 args(String) 表示有且仅有一个String型参数的方法 |
| @args()       | 通过目标类参数的对象类型是否标注了指定注解指定切点 如 @args(org.springframework.stereotype.Service) 表示有且仅有一个标注了@Service的类参数的方法 |
| within()      | 通过类名指定切点如 with(examples.chap03.Horseman) 表示Horseman的所有方法 |
| target()      | 通过类名指定，同时包含所有子类 如 target(examples.chap03.Horseman)  且Elephantman extends Horseman，则两个类的所有方法都匹配 |
| @within()     | 匹配标注了指定注解的类及其所有子类如 @within(org.springframework.stereotype.Service) 给Horseman加上@Service标注，则Horseman和Elephantman 的所有方法都匹配 |
| @target()     | 所有标注了指定注解的类如 @target(org.springframework.stereotype.Service) 表示所有标注了@Service的类的所有方法 |
| this()        | 大部分时候和target()相同，区别是this是在运行时生成代理类后，才判断代理类与指定的对象类型是否匹配 |





**逻辑运算符**

表达式可由多个切点函数通过逻辑运算组成

 &&

与操作，求交集，也可以写成and

例如 execution(* chop(..)) && target(Horseman)  表示Horseman及其子类的chop方法

 ||

或操作，求并集，也可以写成or

例如 execution(* chop(..)) || args(String)  表示名称为chop的方法或者有一个String型参数的方法

!

非操作，求反集，也可以写成not

例如 execution(* chop(..)) and !args(String)  表示名称为chop的方法但是不能是只有一个String型参数的方法

 

execution常用于匹配特定的方法，如update时怎么处理，或者匹配某些类，如所有的controller类，是一种范围较大的切面方式，多用于日志或者事务处理等。

其他的几个用法各有千秋，视情况而选择。

以上标红的比较常用。下面来看annotation的。







