### 一、定义

```
JSP标准标签库(有很多的标签)，是由Apache组织提供的开源的免费JSP标签.
```

### 二、作用

```
用于简化和替换JSP页面上的Java代码.多数时候，我们都是把数据库查询出来的数据，展示到页面上，要想展示数据，可以利用JSTL的foreach标签展示数据.
```

### 三、使用步骤

```
1、导入JSTL相关jar包

2、引入标签库：taglib指令	<%@taglib %> 属性uri="http://java.sun.com/jsp/jstl/core"高版本的.prefix前缀取为c，取什么都行，但这是约定俗成的。

3、使用标签
```

### 四、常用JSTL标签

#### 	1、if

#### 		（1）定义

```
	相当于Java代码的if语句
```

#### 		（2）使用步骤		

```
	<前缀:if test="">内容</前缀：if>
	test必须要写的属性，接收boolean类型的表达式.
	如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容.内容里也可以写html标签.
	一般情况下，test属性值会结合el表达式一起使用
```

​			

```
和el表达式配合演示.
```

![image-20200223145856861](..\img\image-20200223145856861.png)

​	![image-20200223145917209](..\img\image-20200223145917209.png)

#### 	（3）注意

​			

```
	c：if标签没有else情况，想要else情况，则可以定义一个c：if标签.
```



#### 	2、choose

#### 		（1）介绍

```
相当于java代码的switch语句
```

#### 		（2）使用步骤

​	

```
		①使用choose标签声明

		②使用when标签做判断,when标签必须要用test属性值

		③使用otherwise标签做其他情况的声明
```

#### 		（3）具体操作

​		![image-20200223150621478](..\img\image-20200223150621478.png)

![image-20200223150634735](..\img\image-20200223150634735.png)

#### 	3、foreach

#### 		（1）介绍

​	

```
	相当于java代码的for语句，主要用于完成重复的操作，以及遍历集合.
```

#### 		（2） 属性

##### 					①非遍历集合情况下						

```
			begin：开始值

			end：结束值

			var：临时变量.

			step：步长 

				步长1，相当于 ++

				步长2，相当于+=2

			 varstatus：循环状态对象

				两个属性值 index：容器中元素的索引，从0开始

						 count：循环次数
```

##### 					②遍历集合

```
			items：容器对象

			var：容器元素的临时变量

			varStatus：循环状态对象

				count：循环次数，从1开始

				index：	容器中元素的索引，从0开始
```

#### 		（3）具体操作

​					①遍历非集合

![image-20200223153203447](..\img\image-20200223153203447.png)

![image-20200223153217527](..\img\image-20200223153217527.png)

​					②遍历集合

![image-20200223154422521](..\img\image-20200223154422521.png)

### 五、练习

​	

```
需求：在request域中有一个存有User对象的List集合。需要使用jstl+el将list集合数据展示到jsp页面的表格table中。
```



```
<body>
<%--在request域中有一个存有User对象的List集合。需要使用jstl+el将list集合数据展示到jsp页面的表格table中。--%>

<%
    User user=new User("zhangsan",18);
    User user1=new User("lisi",23);
    User user2=new User("liming",22);
    List<User> list=new ArrayList<User>();
    list.add(user);
    list.add(user1);
    list.add(user2);
    request.setAttribute("list",list);
%>

<table border="1" width="500" align="center" >
    <tr>
        <th>编号</th>
        <th>名字</th>
        <th>年龄</th>
    </tr>
    <c:forEach items="${list}" var="element" varStatus="s">
    <tr>
        <td>${s.count}</td>
        <td>${element.name}</td>
        <td>${element.age}</td>
    </tr>
    </c:forEach>
</table>
</body>
```

