# XD

## 定义

​	XSD，XML Schema的作用是定义 XML 文档的合法构建模块，类似DTD。

## 元素类型

### 简易元素

- xs:string
- xs:decimal
-  xs:integer
- xs:boolean
- xs:date
-  xs:time

### 数据类型限定

| **限定**       | **描述**                                                  |
| -------------- | --------------------------------------------------------- |
| enumeration    | 定义可接受值的一个列表                                    |
| fractionDigits | 定义所允许的最大的小数位数。必须大于等于0。               |
| length         | 定义所允许的字符或者列表项目的精确数目。必须大于或等于0。 |
| maxExclusive   | 定义数值的上限。所允许的值必须小于此值。                  |
| maxInclusive   | 定义数值的上限。所允许的值必须小于或等于此值。            |
| maxLength      | 定义所允许的字符或者列表项目的最大数目。必须大于或等于0。 |
| minExclusive   | 定义数值的下限。所允许的值必需大于此值。                  |
| minInclusive   | 定义数值的下限。所允许的值必需大于或等于此值。            |
| minLength      | 定义所允许的字符或者列表项目的最小数目。必须大于或等于0。 |
| pattern        | 定义可接受的字符的精确序列。                              |
| totalDigits    | 定义所允许的阿拉伯数字的精确位数。必须大于0。             |
| whiteSpace     | 定义空白字符（换行、回车、空格以及制表符）的处理方式。    |

###  指示器

- Order 指示器：
	- All
	- Choice
	- Sequence
- Occurrence 指示器：
	- maxOccurs
	- minOccurs
- Group 指示器：
	- Group name
	- attributeGroup name

### 示例

```xml-dtd
<xs:simpleType name="stringtype">
  <xs:restriction base="xs:string"/>
</xs:simpleType>

<xs:simpleType name="inttype">
  <xs:restriction base="xs:positiveInteger"/>
</xs:simpleType>

<xs:simpleType name="dectype">
  <xs:restriction base="xs:decimal"/>
</xs:simpleType>

<xs:simpleType name="orderidtype">
  <xs:restriction base="xs:string">
    <xs:pattern value="[0-9]{6}"/>
  </xs:restriction>
</xs:simpleType>
```

