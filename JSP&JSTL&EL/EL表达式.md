### 一、定义

```
Expression Language 表达式语言
```

### 二、作用

```
替换和简化jsp页面中java代码的编写。可以通过执行运算或获取存储在域中的值输出数据到界面，也可以用于路径编写。
```

### 三、语法

```
${表达式}
```

### 四、注意

```
		1、JSP默认支持EL表达式。如果想要忽略，则可以使用page指令中的isELIgnored="true"，忽略当前JSP页面中的所有EL表达式。
		2、使用\可以忽略当前EL表达式，如\${}.
		3、EL表达式不会抛出空指针异常和数组越界异常，只会显示空字符串.
```

```
忽略全部EL表达式演示
```

![image-20200222193124535](..\img\image-20200222193124535.png)

​	

```
未忽略前
```

![image-20200222193215854](..\img\image-20200222193215854.png)

​	

```
忽略后
```

![image-20200222193156218](..\img\image-20200222193156218.png)

```
忽略单个EL表达式：使用\
```



![image-20200223232920246](..\img\image-20200223232920246.png)



### 五、使用

#### 	1、运算符(执行运算)

#### 		（1）算数运算符

```
		+,-, *, /(div表示除), %(mod标出取余)
```

#### 		（2）比较运算符 

```
		< >= <= == !=
```

#### 		（3）逻辑运算符

```
		&&(and), ||(or), !(not)
```

#### 		（4）空运算符empty

​			

```
empty运算符除外的演示
```

​			![image-20200222193810699](..\img\image-20200222193810699.png)

![image-20200222193822598](..\img\image-20200222193822598.png)

```
empty运算符演示
```

​	![image-20200223142011228](..\img\image-20200223142011228.png)

![image-20200223142026419](..\img\image-20200223142026419.png)

#### 	2、获取值

#### 	（1）注意

```
		EL表达式只能从域对象中获取到存储的值
```

#### 	（2）EL域名称

		 	pageScope		  对应   pageContext域对象
		 	requestScope 	  对应   request域对象
		 	sessionScope 	  对应   session域对象
		 	applicationScope  对应   application域对象
#### 	（3）获取普通值

```
		①${域名称.键名}：从指定域中获取指定键的值 
		②${键名}：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。
		注意：在EL表达式中，如果没有对应的键值对，不会返回成null，而会返回成空字符串。
```

![image-20200223130609329](..\img\image-20200223130502317.png)

![image-20200223130650309](..\img\image-20200223130650309.png)

#### （4）获对象、List和Map值

##### 				①对象

```
		${域名称.键名.属性名}：获取对象属性值
		* 本质上会去调用对象的getter方法
```

![image-20200223132521735](..\img\image-20200223132521735.png)

![image-20200223132542074](..\img\image-20200223132542074.png)

##### 				②List集合

			${域名称.键名[索引]}:中括号索引的意思是获取list集合里的第索引+1个元素.
			如果索引越界，则默认处理为空字符串。
![image-20200223133011526](..\img\image-20200223133011526.png)

![image-20200223133023433](..\img\image-20200223133023433.png)



##### 				③Map集合（两种写法）

```
		I、  ${域名称.键名.key名称}
		II、 ${域名称.键名["key名称"]}
```



![image-20200223141154808](..\img\image-20200223141154808.png)

![image-20200223141216029](..\img\image-20200223141216029.png)

#### 3、EL表达式获取值与运算符可以相互配合

```
	${number%2!=0}意思是获取number键所对应的值如果是奇数就为true，否则为false.

	注意：运算符使用时，除了逻辑运算符和空运算符外，使用其他运算符必须要和数字配合，数字可以是字符串类型如"2"，不然就会报错。
```

![image-20200223145614505](..\img\image-20200223145614505.png)

![image-20200223145627540](..\img\image-20200223145627540.png)

### 六、隐式对象

#### 1、定义	

```
EL中不用直接创建就可以拿来用的对象就称为隐式对象，类似于JSP内置对象，但不等价.EL表达式中有11个隐式对象。
```

#### 2、pageContext对象

	功能
			① 获取jsp其他八个内置对象
			②${pageContext.request.contextPath}：动态获取虚拟目录☆，用于路径编写
​			如：<form action="${pageContext.request.contextPath}/xxx">

### 七、EL可以直接输出常量

| boolean      | true 和 false                                                |
| ------------ | ------------------------------------------------------------ |
| int          | 与 Java 类似。可以包含任何正数或负数，例如 24、-45、567      |
| String       | 任何由单引号或双引号限定的字符串。对于单引号、双引号和反斜杠，使用反斜杠字符作为转义序列。必须注意，如果在字符串两端使用双引号，则单引号不需要转义。 |
| float double | 可以包含任何正的或负的浮点数，例如 -1.8E-45、4.567           |

![image-20200223161144290](..\img\image-20200223161144290.png)

![image-20200223161157229](..\img\image-20200223161157229.png)

```
EL不可以获取键名为数字的值，且el可以直接输出数字.
```


​		

![image-20200223153009345](..\img\image-20200223153009345.png)

```
直接输出数字，而忽略request存储的值。
```

![image-20200223153031701](..\img\image-20200223153031701.png)



![image-20200223153040099](..\img\image-20200223153040099.png)