# Lambda 表达式

*对匿名内部类的写法进行简化*



## 目录

1. 创建线程
1. 校验函数
1. 聚合函数
1. 类型转换



## 一、创建线程

匿名内部类

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        //todo...
    }
}).start();
```



Lambda表达式

```java
new Thread(()->{
	//todo...
}).start();
```



## 二、校验函数

已知用于校验的工具库如下，要求实现代码判断数列是否均为偶数

```java
/**
 * 校验器
 * @param <T> 待校验的类型
 */
interface Checker<T> {
    boolean check(T value);
}

/**
 * 工具类
 */
class Tool {
    public static <T> boolean arrayCheck(T[] arr, Checker<T> checker) {
        for (T e : arr) {
            if (!checker.check(e)) {
                return false;
            }
        }
        return true;
    }
}
```



匿名内部类

```java
public static void main(String[] args) {
    Integer[] arr = {2, 4, 6, 8, 10};
    boolean res = Tool.arrayCheck(arr, new Checker() {
        @Override
        public boolean check(Integer value) {
            return value % 2 == 0;
        }
    });
    System.out.println(res);
}
```



Lambda表达式

```java
public static void main(String[] args) {
    Integer[] arr = {2, 4, 6, 8, 10};
    boolean res = Tool.arrayCheck(arr, (value) -> {
        return value % 2 == 0;
    });
    System.out.println(res);
}
```



## 三、聚合函数

已知用于聚合的工具库如下，要求实现代码对数列进行求和

```java
/**
 * 聚合器
 * @param <T> 待聚合的类型
 */
interface Aggregator<T> {
    T aggregate(T v1, T v2);
}

/**
 * 工具类
 */
class Tool {
    public static <T> T arrayAggregate(T[] arr, Aggregator<T> aggregator) {
        if (arr.length == 0) return null;
        T res = arr[0];
        for (int i = 1; i < arr.length; i++) {
            res = aggregator.aggregate(res, arr[i]);
        }
        return res;
    }
}
```



匿名内部类

```java
public static void main(String[] args) {
    Integer[] arr = {2, 4, 6, 8, 10};
    Integer res = Tool.arrayAggregate(arr, new Aggregator<Integer>() {
        @Override
        public Integer aggregate(Integer v1, Integer v2) {
            return v1 + v2;
        }
    });
    System.out.println(res);
}
```



Lambda表达式

```java
public static void main(String[] args) {
    Integer[] arr = {2, 4, 6, 8, 10};
    Integer res = Tool.arrayAggregate(arr, (v1, v2) -> {
        return v1 + v2;
    });
    System.out.println(res);
}
```



## 四、类型转换

已知用于类型转换的工具库如下，要求实现代码将字符串转换为整数

```java
/**
 * 类型转换器
 * @param <S> 原始类型
 * @param <T> 目标类型
 */
interface Converter<S, T> {
    T convert(S source);
}

/**
 * 工具类
 */
class Tool {
    public static <S, T> T typeConvert(S source, Converter<S, T> converter) {
        return converter.convert(source);
    }
}
```



* 匿名内部类

```java
public static void main(String[] args) {
    String source = "1234";
    Integer target = Tool.typeConvert(source, new Converter<String, Integer>() {
        @Override
        public Integer convert(String source) {
            return Integer.valueOf(source);
        }
    });
    System.out.println(target);
}
```



* Lambda表达式

```java
public static void main(String[] args) {
    String source = "1234";
    Integer target = Tool.typeConvert(source, (s) -> {
        return Integer.valueOf(s);
    });
    System.out.println(target);
}
```

