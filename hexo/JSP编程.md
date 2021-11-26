---
title: JSP编程
date: 2021-11-22 16:55:40
categories:
- JSP
tags:
- 语言
- 网站
---

### JSP基本语法

#### 第一个JSP页面

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			out.print("欢迎来到JSP!");
		%>
        <br />
	</body>
</html>
```

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<script type="text/javascript">
			document.write("欢迎来到JSP!");
		</script>
        <br />
	</body>
</html>
```

#### 注释

分为两类，一类可以发送给客户端，能在源代码文件中显示内容，主要以HTML注释语法出现，如下，可以在里面加入JSP表达式，，动态生成注释内容

`<!-- 注释内容 -->`

另一类不能发送给客户端，仅提供给程序员阅读

①JSP注释语法`<%-- 注释内容 --%>` 

②Java代码注释

```jsp
//注释内容
/* 注释内容 */
```

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			out.print("欢迎来到JSP!");
		%>
        <br />
        <!-- HTML风格注释，它会发送到客户端 -->
	</body>
</html>
```

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			out.print("欢迎来到JSP!");
		%>
        <br />
        <%-- JSP风格注释，它不会发送到客户端 --%>
	</body>
</html>
```

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			out.print("欢迎来到JSP!");	//Java注释
		%>
        <br />
	</body>
</html>
```

#### JSP表达式

基本语法`<%= 变量/返回值/表达式 %>`作用是将表达式的内容运算的结果输出到客户端，例`<%= msg %>`表示将msg内容输出给客户端，等价于`<% out.print(msg); %>`

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			String name="Jack";
			String msg="欢迎来到JSP!";
		%>
        <br />
        <%= name + "," + msg %>
	</body>
</html>
```

**注意:** ⑴ JSP表达式中不能用`;`结束。⑵ JSP表达式中不能出现多条语句。⑶ 表达式中的内容一定是字符串类型，或者能通过`toString()`函数转换成字符串的形式

#### JSP程序段

在程序段中可以加入任意数量的代码

`<% Java代码 %>`

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			for(int i=1;i<=10;i++) {
				out.println("欢迎来到JSP<br />");
			}
		%>
	</body>
</html>
```

**不能在JSP程序段中定义函数**

在JSP程序中既可以放入HTML，也可以放JSP程序段和JSP表达式

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			for(int i=1;i<=10;i++)
		%>
		<%= i %>:欢迎来到JSP<br />
        <%
        	}
        %>
	</body>
</html>
```

凡是没写`<% %>`均被解释为HTML

#### JSP声明

变量必须先定义后使用，以下代码会报错

```jsp
<%
	out.println(str);
	String str="欢迎";
%>
```

在JSP声明中可以定义网页中的全局变量，这些变量在JSP页面中的任何地方都可使用。`<%! 代码 %>`

JSP程序段中的变量只能先声明后使用，而在JSP声明中的定义的变量是网页级别的，系统会优先执行，使用JSP声明可以在任何地方定义变量

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			out.println(str);
		%>
        <%!
        	String str="欢迎";
        %>
	</body>
</html>
```

**注意:** 在JSP声明中只能作定义，不可实现控制逻辑，下面代码会报错

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%!
			out.print("欢迎来到JSP");
		%>
	</body>
</html>
```

#### URL传值

HTTP是无状态协议。URL通俗说就是网址

例`http://localhost:8080/Prj04/page.jsp?m=3&n=5`表示访问`http://localhost:8080/Prj04/page.jsp`，并给其传送参数m=3,n=5，实现方法如下

```jsp
<%
	//获取参数m，赋值给str
	String str=request.getParameter("m");
%>
```

如果m没有传过来或者参数名写错，str为null

```jsp
<% @ page laguage="java" import="java.util.*" charset=gb2312" %>
<%
	//定义一个变量
	String str="12";
	int number=Integer.parseInt(str);
%>
该数字的平方为: <%= number*number %><hr>
<a href="urlP2.jsp?number=<%= number %>">到达p2</a>
```

链接内容`http://localhost:8080/Prj04/urlP2.jsp?number=12`

```jsp
<% @ page laguage="java" import="java.util.*" charset=gb2312" %>
<%
	//获得number
	String str=request.getParameter("number");
	int number=Integer.parseInt(str);
%>
该数字的立方为: <%= number*number*number %><hr>
```

有缺陷: ⑴ 传输的数据只能是字符串，对数据类型有一定的限制  ⑵ 传输数据的值会在浏览器的地址栏中看到

#### JSP指令和动作

1. **JSP指令**

告诉JSP引擎对JSP页面如何编译，不包含控制逻辑，不产生任何可见的输出

`<% @ 指令类别 属性1="属性值1" ... 属性n="属性值n" %>`

page指令例如`<% @ page laguage="java" contentType="text/html; charset=gb2312" %>`

**注意:** 属性名大小写敏感

JSP包含3个指令，即page、include、taglib，通常，JSP以page指令开头

Ⅰ **导入包**

`<% @ page import="包名.类名" %>`

导入包下面的全部类可用 ``<% @ page import="包名.*" %>``

若要引入包中的多个类，可

```
<% @ page import="包名.类1" %>
<% @ page import="包名.类2" %>
或者
<% @ page import="包名.类1,包名.类2" %>
```

```jsp
<% @ page import="java.util.Date" laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		你的登录时间是<%= new Date() %>
	</body>
</html>
```

Ⅱ **设定字符集**

`pageEncoding`可以设置页面的字符集，pageEncoding属性用来设定JSP文件编码方式，常见有ISO-8859-1、gb2312、GBK等

`<% @ page pageEncoding="编码类型"%>`

例``<% @ page pageEncoding="GBK"%>``

Ⅲ 设定错误页面

有errorPage和isErrorPage属性，errorPage属性指定一个页面，当JSP出现未被捕获的异常时跳转到这个指定的页面。通常情况下，跳转到的页面需要使用isErrorPage属性指明处理其他页面的错误信息

发生异常的页面使用代码`<% @ page errorPage="anErrorPage.jsp"`，在anErrorPage.jsp中需使用`<% @ page isErrorPage="true" %>`进行错误处理

```jsp
<% @ page contentType="text/html; charset=gb2312" errorPage="pageTest2_error.jsp"%>
<html>
	<body>
		<%	//此页面会向pageTest2_error抛出异常，让其来处理
			int num1=10;
			int num2=0;
			int num3=num1/num2;
		%>
	</body>
</html>
```

```jsp
<% @ page contentType="text/html; charset=gb2312" iserrorPage="true"%>
<html>
	<body>
		<%	//此页面会处理pageTest2.jsp抛出的异常
			//友好地显示错误信息
			out.println("网页出现数学运算异常!");
		%>
	</body>
</html>
```

Ⅳ **设定MIME类型和字符编码**

`<% @ page contentType="MIME类型; charset=字符编码" %>`

一般情况下，属性设置为`contentType="text/html; charset=gb2312"`，表示页面是HTML页面，字符集是gb2312。

include指令

`<% @ include file="文件名" %>`可以插入多个外部文件，这些文件可以是jsp、HTML或者java程序，甚至是文本

```jsp
<% @ page contentType="text/html; charset=gb2312" %>
<hr>
<center>
	公司电话号码: 010-89574895,欢迎来电!
</center>
```

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
			out.print("欢迎来到本系统!");
		%>
		<br />
		<% @ include file="info.jsp" %>
	</body>
</html>
```

在使用include指令把另外页面包含进本页面，但被包含的页面与本页面有相同的变量，代码会报错

```jsp
<% @ page contentType="text/html; charset=gb2312" %>
<%
	String msg="欢迎来到本系统!";
%>
```

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<% @ include file="info.jsp" %>
		<%
			String msg="欢迎来到本系统!";
		%>
	</body>
</html>
```

2. **JSP动作**

`<jsp:动作名 属性1="属性值1"...属性n="属性值n" />`或者`<jsp:动作名 属性1="属性值1"...属性n="属性值n" >相关内容</jsp:动作名>`

动作如下：

+ `jsp:include` 当页面被请求时引入一个文件
+ `jsp:forward` 将请求转到另外一个页面
+ `jsp:useBean` 获得javabean的实例
+ `jsp:setProperty` 设置javabean属性
+ `jsp:getProperty` 获得javabean属性
+ `jsp:plugin` 根据浏览器类型为java插件生成OBJECT或EMBED两张标记

include动作与include指令作用差不多，include动作作用是在页面请求时候引入一个指定的文件

`jsp:include page="文件名" />`或者

```jsp
<jsp:include page="文件名">
	相关标签
</jsp:include>
```

**注意:**  include动作只会把文件中的输出包含进来；include动作自动检查被包含文件的变化

forward动作可以实现跳转

`<jsp:forward page="文件名"`

```jsp
<% @ page laguage="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<jsp:forward page="pageTest1.jsp" />
	</body>
</html>
```

###  表单开发

#### 初识表单

1. 表单的作用

![](https://img01.sogoucdn.com/app/a/100520146/03B16A125B3C1C5F347C1A3167C104D5)

2. 定义表单

⑴ 输入功能由控件提供，叫表单元素

⑵ 按钮负责提交

⑶ 提交内容，表单会将内容提交给服务器端

⑷ 表单元素在`<form></form>`之间

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		欢迎登录本系统
		<form>
			请您输入账号: <input name="account" type="text"><br />
			请您输入密码: <input name="password" type="password"><br />
            <input type="submit" value="登录">
		</form>
	</body>
</html>
```

**表单提交到服务器端，提交给哪一页面?**

用`<form>`中的action属性确定

```jsp
<form action="page.jsp">
	请您输入账号: <input name="account" type="text"><br />
	请您输入密码: <input name="password" type="password"><br />
    <input type="submit" value="登录">
</form>
```

action支持相对路径，例

+ ../page.jsp表示当前页面的上一级目录中的page.jsp
+ jsps/page.jsp表示当前目录jsps目录中的page.jsp

也支持绝对路径，例

/Prj05/page.jsp表示Prj05中的page.jsp

**page.jsp如何获取提交的值?**

用request对象

```jsp
<%
	//获取表单中name=account的表单元素中输入的值，赋值给str
	String str=request.getParameter("account");
%>
```

如果表单中没有name=account的表单元素，str=null；如果在表单元素account中没有输入任何内容就提交，str=""

**`<input type="submit" value="登录">`表示提交按钮，可以用普通按钮吗?**

不可，将该按钮改为`<input type="button" value="登录">`，显示效果一样，但是单击后无提交功能。可以用JavaScript进行提交

#### 单一表单元素数据的获取

1. **获取文本框中的数据**

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<form action="textForm_result.jsp">
			请您输入学生的模糊资料: <br />
			<input name="stuname" type="text">
            <input type="submit" value="查询">
		</form>
	</body>
</html>
```
`<form action="textForm_result.jsp">`表示将页面提交到textForm_result.jsp

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		<%
        	String stuname=request.getParameter("stuname");
        	out.println("输入的查询关键字为:" + stuname);
        %>
	</body>
</html>
```

**注意**: 如果输入的是罗斯，则会显示`????`，即中文无法显示

上述代码提交的内容在浏览器中能看到，这不安全，解决办法为在表单中将method属性设置为post，将textForm.jsp中的表单改为如下格式

```jsp
...
		<form action="textForm_result.jsp" method="post">
			请您输入学生的模糊资料: <br />
			<input name="stuname" type="text">
            <input type="submit" value="查询">
		</form>
...
```

**注意**: 在默认情况下是get方式，get方式和post方式是提交请求的两种方式.

2. **获取密码框中的数据**

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		请您输入自己的信息进行注册
		<form action="passwordForm_result.jsp" method="post">
			请您输入账号: <input name="account" type="text"><br />
			请您输入密码: <input name="password" type="password"><br />
            <input type="submit" value="注册">
		</form>
	</body>
</html>
```

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        <%
        	String password=request.getParameter("password");
        	out.println("密码为:" + password);
        %>
	</body>
</html>
```

3. **获取多行文本框中的数据**

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		请您输入自己的信息进行注册
		<form action="textareaForm_result.jsp" method="post">
			请您输入账号: <input name="account" type="text"><br />
			请您输入密码: <input name="password" type="password"><br />
            请您输入个人信息: <br />
            <textarea name="info" rows="5" cols="30"></textarea>
            <input type="submit" value="注册">
		</form>
	</body>
</html>
```

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        <%
        	String info=request.getParameter("info");
        	out.println("个人信息为:" + info);
        %>
	</body>
</html>
```

4. **获取单选按钮中的数据**

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
		请您输入自己的信息进行注册
		<form action="radioForm_result.jsp" method="post">
			请您输入账号: <input name="account" type="text"><br />
			请您输入密码: <input name="password" type="password"><br />
            请您输入性别:
            <input name="sex" type="radio" value="boy" checked>男
            <input name="sex" type="radio" value="girl" checked>女<br />
            <input type="submit" value="注册">
		</form>
	</body>
</html>
```

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        <%
        	String sex=request.getParameter("sex");
        	out.println("性别为:" + sex);
        %>
	</body>
</html>
```

5. **获取下拉菜单中的数据**

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        请您输入自己的信息进行注册
		<form action="selectForm_result.jsp" method="post">
			请您输入账号: <input name="account" type="text"><br />
			请您输入密码: <input name="password" type="password"><br />
            请您选择家乡:
            <select name="home">
                <option value="beijing">北京</option>
                <option value="shanghai">上海</option>
                <option value="guangdong">广东</option>
            </select>
            <input type="submit" value="注册">
		</form>
	</body>
</html>
```

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        <%
        	String home=request.getParameter("home");
        	out.println("家乡为:" + home);
        %>
	</body>
</html>
```

#### 捆绑表单元素数据的获取

```jsp
<%
	String[] str=request.getParameterValues["pName"];
%>
```

1. **获取复选框中的数据**

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        请您输入自己的信息进行注册
		<form action="checkForm_result.jsp" method="post">
			请您选择您的爱好:
			<input name="fav" type="checkbox" value="sing">唱歌
			<input name="fav" type="checkbox" value="dance">跳舞
			<input name="fav" type="checkbox" value="ball">打球
			<input name="fav" type="checkbox" value="game">打游戏<br />
            <input type="submit" value="注册">
		</form>
	</body>
</html>
```

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        <%
        	String[] fav=request.getParameterValues("fav");
        	out.println("爱好为:");
        	for(int i=0;i<fav.length;i++) {
                out.println(fav[i]);
            }
        %>
	</body>
</html>
```

2. **获取多选列表框中的数据**

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        请您输入自己的信息进行注册
		<form action="listForm_result.jsp" method="post">
			请您选择您的爱好: <br />
            <select name="fav" multiple>
                <option value="sing">唱歌</option>
                <option value="dance">跳舞</option>
                <option value="ball">打球</option>
                <option value="game">打游戏</option>
            </select>
            <input type="submit" value="注册">
		</form>
	</body>
</html>
```
<u>在选择的同时按下`Ctrl`键可以多选</u>

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        <%
        	String[] fav=request.getParameterValues("fav");
        	out.println("爱好为:");
        	for(int i=0;i<fav.length;i++) {
                out.println(fav[i]);
            }
        %>
	</body>
</html>
```

3. **获取其他同名表单元素中的数据**

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        请您输入自己的信息进行注册
		<form action="multiNameForm_result.jsp" method="post">
			请您输入您的电话号码(最多4个): <br />
            <% for(int i=1;i<=4;i++) { %>
            号码<%= i %>: <input name="phone" type="text"><br />
            <% } %>
            <input type="submit" value="注册">
		</form>
	</body>
</html>
```

```jsp
<% @ page language="java" contentType="text/html; charset=gb2312" %>
<html>
	<body>
        <%
        	String[] phone=request.getParameterValues("phone");
        	out.println("号码为:");
        	for(int i=0;i<phone.length;i++) {
                out.println(phone[i]);
            }
        %>
	</body>
</html>
```

#### 隐藏表单



EL/JSTL
https://www.cnblogs.com/mingforyou/p/4468313.html
https://www.runoob.com/jsp/jsp-expression-language.html
https://www.runoob.com/jsp/jsp-jstl.html

作业：

基于前面 java web远程控制服务器的代码，使用EL 或JSTL优化你的项目（只要项目中使用EL或JSTL相关技术，优化数据库访问与数据显示即可）
