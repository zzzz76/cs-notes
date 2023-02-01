# Stream 流

*对数组或集合进行链状流式的操作*



## 目录

1. 创建流
2. 中间操作
3. 终结操作
4. 注意事项
5. 场景样例



## 一、创建流

#### 1. 数组

```java
Integer[] arr = {1,2,3,4,5};
Stream<Integer> stream1 = Arrays.stream(arr);
Stream<Integer> stream2 = Stream.of(arr);
```



#### 2. List

```java
List<Person> persons = new ArrayList<>();
Stream<Person> stream = persons.stream();
```



#### 3. Set

```java
Set<String> set = new HashSet<>();
Stream<String> stream = set.stream();
```



#### 4. Map

```java
Map<String, Person> map = new HashMap<>();
Stream<Map.Entry<String, Person>> stream = map.entrySet().stream();
```



## 二、中间操作

#### 1. filter

对流中的元素进行条件过滤

```java
List<Person> personList = getPersonList();
personList.stream()
    .filter(person -> person.getId() >= 2)
    .forEach(person -> System.out.println(person.getName()));
```



#### 2. map

把流中的元素映射成另外的元素

```java
Integer[] arr = new Integer[]{1, 2, 3, 4};
Arrays.stream(arr)
    .map(e -> e + 10)
    .forEach(e -> System.out.println(e));
```



#### 3. distinct

去除流中的重复元素，distinct在判断对象是否相同时需要调用Object的equals方法

```java
Integer[] arr = new Integer[]{1, 2, 2, 3};
Arrays.stream(arr)
    .distinct()
    .forEach(e -> System.out.println(e));
```



#### 4. sorted

对流中的元素进行排序

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
Arrays.stream(arr)
    .sorted((o1, o2) -> o2.compareTo(o1))
    .forEach(e -> System.out.println(e));
```



#### 5. limit

对流的最大长度进行限制，超出的部分将被抛弃

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
Arrays.stream(arr)
    .sorted()
    .limit(2)
    .forEach(e -> System.out.println(e));
```



#### 6. skip

跳过流中的前n个元素

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
Arrays.stream(arr)
    .sorted()
    .skip(2)
    .forEach(e -> System.out.println(e));
```



#### 7. flatMap

把流中的元素映射成另外的流

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
Arrays.stream(arr)
    .flatMap(e -> Arrays.stream(new Integer[]{1 + e, 1 + e}))
    .forEach(e -> System.out.println(e));
```



## 三、终结操作

#### 1. forEach

对流中的元素进行遍历操作

```java
List<Person> personList = getPersonList();
personList.stream()
    .forEach(person -> System.out.println(person.getName()));
```



#### 2. count

获取当前流中元素的个数

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
long count = Arrays.stream(arr)
    .skip(2)
    .count();
```



#### 3. max

获取当前流中的最大元素

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
Optional<Integer> max = Arrays.stream(arr)
    .max(((o1, o2) -> o1.compareTo(o2)));
System.out.println(max.get());
```



#### 4. collect

把当前流转换成一个List集合

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
List<Integer> list = Arrays.stream(arr)
    .collect(Collectors.toList());
```



把当前流转换成一个Set集合

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
Set<Integer> set = Arrays.stream(arr)
    .collect(Collectors.toSet());
```



#### 5. match

判断流中的元素是否都符合匹配条件

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
boolean flag = Arrays.stream(arr).allMatch(e -> e >= 1);
```



判断是否都不符合匹配条件

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
boolean flag = Arrays.stream(arr).noneMatch(e -> e < 1);
```



判断流中是否存在元素符合条件

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
boolean flag = Arrays.stream(arr).anyMatch(e -> e == 1);
```



#### 6. find

获取流中的任意一个元素

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
Optional<Integer> optional = Arrays.stream(arr).findAny();
```



获取流中的第一个元素

```java
Integer[] arr = new Integer[]{1, 3, 2, 2};
Optional<Integer> optional = Arrays.stream(arr).findFirst();
```



#### 7. reduce

对流中的所有元素进行聚合

```java
```



## 四、注意事项

1. 如果没有终结操作，中间操作是不会执行的
2. 流对象是一次性的
3. 不会影响原数据



## 五、场景样例

查询未成年作家的评分在70以上的书籍，由于洋流影响所以作家和书籍可能出现重复，需要进行去重

* 传统写法

```java
public List<Book> getBookList(List<Author> authors) {
	List<Book> bookList = new ArrayList<>();
    Set<Book> uniqueBooks = new HashSet<>();
    Set<Author> uniqueAuthors = new HashSet<>();
    for (Author author : authors) {
        if (uniqueAuthors.add(author)) {
            if (author.getAge() < 18) {
                List<Book> books = author.getBooks();
                for (Book book : books) {
                    if (book.getScore() > 70) {
                        if (uniqueBooks.add(book)) {
                            bookList.add(book);
                        }
                    }
                }
            }
        }
    }
    return bookList;
}

```

* Stream写法

```java
List<Book> collect = authors.stream()
    .distinct()
    .filter(author -> author.getAge() < 18)
    .map(author -> author.getBooks())
    .flatMap(Collection::stream)
    .filter(book -> book.getScore() > 70)
    .distinct()
    .collect(Collectors.toList());
```



## 参考链接

1. [三更草堂函数式编程](https://www.bilibili.com/video/BV1Gh41187uR)

