Generate java class from xsd for amzn-base.xsd
====

Fung.Xu @ 2016.10

解决 amazon 枚举值不能生成 java 文件的方法:

1. 新建一个 binding.xjb 文件，内容如下 ：

```
<?xml version="1.0" ?>
<jaxb:bindings version="1.0" xmlns:jaxb="http://java.sun.com/xml/ns/jaxb">
    <jaxb:globalBindings typesafeEnumMemberName="generateName" typesafeEnumMaxMembers="2048"/>
</jaxb:bindings>
```

2. 在xjc 生成 java 文件时，加上-b 参数:

例如:  

```
xjc -d src -p com.gm.dropship.amazon.stub -b xsd/binding.xjb xsd/amzn-base.xsd
```