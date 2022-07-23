# Optional

*优雅的非空判断*



## 目录

1. 创建对象
2. 中间操作
3. 非空访问
4. 场景样例



## 一、创建对象

创建值为null的Optional

```java
Optional<User> userOptional = Optional.empty();
```



创建值非null的Optional

```java
User user = new User();
Optional<User> userOptional = Optional.of(user);
```



创建值可能为null、可能非null的Optional

```java
User user = getUser();
Optional<User> userOptional = Optional.ofNullable(user);
```



## 二、中间操作

1. filter

如果未通过过滤，则返回空的Optional

```java
Optional<User> userOptional = Optional.ofNullable(getUser());
userOptional.filter(user -> user.getAge() > 18).ifPresent(user -> System.out.println(user.getName()));
```



2. map

把Optional中的元素映射成另外的元素

```java
Optional<User> userOptional = Optional.ofNullable(getUser());
String name = userOptional.map(u -> u.getName()).orElseGet(() -> "default");
```



3. flatMap

把Optional中的元素映射成另外的Optional

```java
Optional<User> userOptional = Optional.ofNullable(getUser());
String name = userOptional.flatMap(u -> Optional.ofNullable(u.getName())).orElseGet(() -> "default");
```



## 三、非空访问

1. orElseGet

如果数据不为空，则获取该数据；如果数据为空，则获取设置的默认值

```java
Optional<User> userOptional = Optional.ofNullable(getUser());
User user = userOptional.orElseGet(() -> new User());
```



2. orElseThrow

如果数据不为空，则获取该数据；如果数据为空，则抛出设置的默认异常

```java
Optional<User> userOptional = Optional.ofNullable(getUser());
User user = userOptional.orElseThrow(() -> new IllegalArgumentException());
```



3. ifPresent

如果数据不为空，则执行函数操作；如果数据为空，则不执行函数操作

```java
Optional<User> userOptional = Optional.ofNullable(getUser());
userOptional.ifPresent(user -> System.out.println(user.getName()));
```



## 四、场景样例

访问内部属性

* 传统写法

```java
public String getIsocode(User user) {
    if(user != null) {
        Address address = user.getAddress();
        if (address != null) {
            Country country = address.getCountry();
            if (country != null) {
                String isocode = country.getIsocode();
                if (isocode != null) {
                    return isocode.toUpperCase()
                }
            }
        }
    }
    return "DEFAULT";
}
```

* Optional写法

```java
public String getIsocode(User user) {
    Optional<User> optionalUser = Optional.ofNullable(user);
    optionalUser.map(u -> u.getAddress())
        .map(a -> a.getCountry())
        .map(c -> c.getIsocode())
        .map(s -> s.toUpperCase())
        .orElseGet(() -> "DEFAULT");
}
```



## 参考链接

1. [Java8 Optional 详细用法](https://blog.csdn.net/y_k_y/article/details/84633143)
2. [理解、学习与使用 Java 中的 Optional](https://www.cnblogs.com/zhangboyu/p/7580262.html)
