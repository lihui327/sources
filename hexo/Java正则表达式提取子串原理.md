---
title: Java正则表达式提取子串原理
date: 2021-11-21 10:35:25
categories:
- Java
tags:
- String 
- 正则表达式
- Pattern
- Matcher
---

#### java提取String字符串中的中文

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

String reg="[\u4e00-\u9fa5]";
str_count="hello world,你好，世界";
p=Pattern.compile(reg);
m=p.matcher(str_count);
str=null;
while(m.find()) {
    str=m.group(0);
}
System.out.println(str);
```

#### 参考文献

1. CSDN. 【java 取中文_java怎么把字符串中的的汉字取出来?】(2021-02-12)[2021-11-21]. <https://blog.csdn.net/weixin_30758169/article/details/114047615>.
2. CSDN. 【java提取String中 中文】(2020-12-30)[2021-11-21]. <https://blog.csdn.net/SuperChen12356/article/details/111942599>.

#### java提取String字符串中的数字

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

String reg_tel="\\d{11}";
p=Pattern.compile(reg_tel);
m=p.matcher(str_count);
str=null;
while(m.find()) {
    str=m.group(0);
}
System.out.println(str);
```

#### 参考文献

1. 博客园. 【java从字符串中提取数字】(2018-03-10)[2021-11-21]. <https://www.cnblogs.com/lxqiaoyixuan/p/8541530.html>.
