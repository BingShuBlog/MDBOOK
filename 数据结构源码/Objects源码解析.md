# Objects类解析

​    JDK7新增Objects类介绍（以下程序以1.8来说明）



## 简介：

​    JDK7里面新增的Objects类，本人学习HashMap源码偶遇此类，所以研究一下，本类将对象常用的操作进行了封装，包括hashCode、equals、deepEquals等方法。



### 源码：

   

``` java
package java.util;

import java.util.function.Supplier;

public final class Objects {
    
    //私有构造函数无法New对象，也无法去继承final
    private Objects() {
        throw new AssertionError("No java.util.Objects instances for you!");
    }
    
    //equals方法比较两个对象是否相等（首先比较内存地址，然后比较a.equals(b)，只要符合其中之一返回true）
    public static boolean equals(Object a, Object b) {
        return (a == b) || (a != null && a.equals(b));
    }

    
    //深度equals深度比较两个对象是否相等(首先比较内存地址,相同返回true;如果传入的是数组，则比较数组内的对应下标值是否相同)
    public static boolean deepEquals(Object a, Object b) {
        if (a == b)
            return true;
        else if (a == null || b == null)
            return false;
        else
            return Arrays.deepEquals0(a, b);
    }
    
    //返回对象的hashCode，若o的为null，返回0
    public static int hashCode(Object o) {
        return o != null ? o.hashCode() : 0;
    }

   //当传入多个参数转成数组，计算HashCode
    public static int hash(Object... values) {
        return Arrays.hashCode(values);
    }

    //返回对象的String表示，若传入null，返回null字符串
    public static String toString(Object o) {
        return String.valueOf(o);
    }
    
    //返回对象的String表示，若传入null，返回默认值nullDefault
    public static String toString(Object o, String nullDefault) {
        return (o != null) ? o.toString() : nullDefault;
    }

    
    //使用指定的比较器c 比较参数a和参数b的大小（相等返回0，a大于b返回整数，a小于b返回负数）
    public static <T> int compare(T a, T b, Comparator<? super T> c) {
        return (a == b) ? 0 :  c.compare(a, b);
    }

   //如果传入的obj为null抛出NullPointerException,否者返回obj
    public static <T> T requireNonNull(T obj) {
        if (obj == null)
            throw new NullPointerException();
        return obj;
    }

   //如果传入的obj为null抛出NullPointerException并可以指定错误信息message,否者返回obj
    public static <T> T requireNonNull(T obj, String message) {
        if (obj == null)
            throw new NullPointerException(message);
        return obj;
    }
   
    
    
    -----------------------------以下是jdk8新增方法---------------------------
    
   //判断传入的obj是否为null，是返回true,否者返回false
    public static boolean isNull(Object obj) {
        return obj == null;
    }

    //判断传入的obj是否不为null，不为空返回true,为空返回false （和isNull()方法相反）
    public static boolean nonNull(Object obj) {
        return obj != null;
    }

   //如果传入的obj为null抛出NullPointerException并且使用参数messageSupplier指定错误信息,否者返回obj
    public static <T> T requireNonNull(T obj, Supplier<String> messageSupplier) {
        if (obj == null)
            throw new NullPointerException(messageSupplier.get());
        return obj;
    }
}

```

