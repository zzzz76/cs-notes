# Jackson

## 目录

1. 序列化至xml
2. 反序列化为对象
3. 注解配置



## 一、序列化至xml

Java对象序列化为xml字符串

```java
public String parseToString(Person person) {
    XmlMapper xmlMapper = new XmlMapper();
    String xml = xmlMapper.writeValueAsString(person);
    return xml;
}
```



Java对象序列化为xml文件

```java
public void parseToFile(Person person, File file) {
    XmlMapper xmlMapper = new XmlMapper();
    xmlMapper.writeValue(file, person);
}
```



## 二、反序列化为对象

xml字符串反序列化为Java对象

```java
public Person generalObject(String xml) {
    XmlMapper xmlMapper = new XmlMapper();
    Person person = xmlMapper.readValue(xml, Person.class);
    return person;
}
```



xml文件反序列化为Java对象

```java
public Person generalObject(File file) {
    XmlMapper xmlMapper = new XmlMapper();
    String xml = fileToString(file);
    Person person = xmlMapper.readValue(xml, Person.class);
    return person;
}
```



## 三、注解配置

* @JsonProperty：用于属性，把属性的名称序列化为另外一个名称
* @JsonIgnore：用于属性或方法，使相应字段不参与序列化与反序列化
* @JsonFormat：用于属性或方法，把属性的格式序列化为另一个格式



## 参考链接

1. [Jackson框架使用教程](https://blog.csdn.net/weixin_44747933/article/details/108301626)
2. [https://github.com/FasterXML/jackson-docs](https://github.com/FasterXML/jackson-docs)
3. [Jackson实现xml序列化和反序列化](https://blog.csdn.net/neweastsun/article/details/100044167)
4. [Jackson 注解大全](https://blog.csdn.net/blwinner/article/details/98532847)