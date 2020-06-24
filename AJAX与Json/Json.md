### 一、定义

```
　JSON（JavaScript Object Notation），JavaScript对象表示法，即JSON 语法是 javascript对象表示的语法的子集。JSON是一种独立于语言用来描述前后端数据交互的内容格式，即可以存储数据并进行数据的传输。有了JSON这样一套统一的描述规则，前后端解析数据成本变低。虽然JSON使用了JavaScript的语法，但是它仍然独立于语言和平台且语法比JS严格。由于JSON语法和JS创建对象的语法相似，所以无需解析器，JS就能将JSON数据转成JS对象。JSON比XML更小、更快,更易解析。
```

### 二、JSON语法

#### 	1、语法

​			（1）JSON的两种结构

​					① "名称/值"对的集合 即{}表示 ，通常被称为对象。即JSON对象.

```
			不同的语言中，它被理解为对象（object），记录（record）...			
```

​					② 值的有序列表 即[]。通常被称为数组。

```
			在大部分语言中，它被理解为数组（array）。
```

​			（2）语法

​						[1]	数据在名称/值对中 

​						[2]	多对键值对由逗号分隔 

​						[3]	键用引号(单双都行)引起来，也可以不使用引号

​						[4]	值的取值类型：

```
            ①	数字（整数或浮点数）
            ②	字符串（在双引号中）
            ③	boolean值（true 或 false）
            ④	数组	{"persons":[{},{}]}
            ⑤	对象  {"address":{"province"："陕西"....}}
            ⑥	null
```

#### 	2、演示

```
	由于JSON语法使用了JS创建对象的语法，所以在JS中演示。在JS中，称JS对象做JSON对象是因为它符合JSON对象的语法。JSON字符串，即符合JSON格式的字符串。
```

​			图中1、4是基本格式，2、3是嵌套格式.

![image-20200316223218112](F:\Java集合\Java全面复习\img\image-20200316223218112.png)

### 三、获取数据

#### 		1、在JS中获取JSON数据方式

​				① json对象.键名

​				② json对象["键名"]

​				③ 数组对象[索引]

![image-20200316225144900](F:\Java集合\Java全面复习\img\image-20200316225144900.png)

![image-20200316225237596](F:\Java集合\Java全面复习\img\image-20200316225237596.png)

#### 		2、遍历

![image-20200316230218106](F:\Java集合\Java全面复习\img\image-20200316230218106.png)



### 四、Json和Java对象的相互转换

​		

```
	将来要把Json作为数据的载体在网络中传输，传输就要涉及到客户端和服务器端，那么客户端和服务器通信时，可以通过Json来作为传输数据的载体。服务器端若使用的是java语言，那我们需要将Json数据转成java对象。从而拿到了数据，在程序中使用。
```



```
常见解析器：Jsonlib，Gson，fastjson，jackson
```



### 五、JackSon解析器

#### 	1、JSON字符串转为Java对象

##### 			（1）步骤

```
	1. 导入jackson的相关jar包
	2. 创建Jackson核心对象 ObjectMapper
	3. 调用ObjectMapper的相关方法进行转换
		readValue(json字符串数据,Class)：Json字符串想要转成什么样的对象，传入一个Class类型.
```

##### 			（2）演示

![image-20200317001448425](..\img\image-20200317001448425.png)

#### 	2、Java对象转为JSON字符串

##### 			（1）步骤

		① 导入jackson的相关jar包
		② 创建Jackson核心对象 ObjectMapper
		③ 调用ObjectMapper的转换方法

##### 			（2）转换方法

```
 	①	writeValue(参数1，obj):
		参数1：
			File：将obj对象转换为JSON字符串，并保存到指定的文件中。
			Writer：将obj对象转换为JSON字符串，并将json数据填充到字符输出流中。
			OutputStream：将obj对象转换为JSON字符串，并将json数据填充到字节输出流中。
			
 	② writeValueAsString(obj):将对象转为json格式的字符串。
			
```

##### 			（3）演示

​				①Person对象，用于将Person对象来转为JSON

![image-20200316232618877](..\img\image-20200316232618877.png)

​				②

​						I、使用writeValueAsString方法

![image-20200316232557634](..\img\image-20200316232557634.png)

​									结果

![image-20200316232632054](..\img\image-20200316232632054.png)

​						II、writeValue展示

![image-20200316232932866](..\img\image-20200316232932866.png)

##### 			（4）注解

	    ①	@JsonIgnore：排除属性。
	    ②	@JsonFormat：属性值的格式化
	        如：@JsonFormat(pattern = "XXX")：pattern和simpleDateformat格式化1格式一样。
###### 					I、演示	

​				 		①Perosn对象

![image-20200316234248452](..\img\image-20200316234248452.png)

​				 		②转换类

![image-20200316234305703](..\img\image-20200316234305703.png)

```
	结果：date的值是毫秒值，我们不想要毫秒值怎么办。第一种方式，忽略掉，即不要生成这个键值对。第二种方式，将其格式化。
```

![image-20200316234336390](..\img\image-20200316234336390.png)

###### 					II、接I演示

​						方式一：忽略掉属性

```
	加了@JsonIgnore后，忽略掉了date属性.
```

![image-20200316234537909](..\img\image-20200316234537909.png)

```
	运行结果，忽略掉了date属性值.
```

![image-20200316234641991](..\img\image-20200316234641991.png)



​						方式二：将其格式化

![image-20200316234901072](..\img\image-20200316234901072.png)

```
	结果：将date的值格式化了，即将毫秒值转换为格式化好了的。
```

![image-20200316234911847](..\img\image-20200316234911847.png)

##### （5）复杂java对象转换为JSON字符串

###### 					①List转为JSON字符串

![image-20200317000022731](..\img\image-20200317000022731.png)

```
	List对象转换Json字符串的格式。
```

![image-20200317000203312](..\img\image-20200317000203312.png)

###### 					②Map转JSON字符串

![image-20200317000328027](..\img\image-20200317000328027.png)

```
		结果：map转JSON字符串的格式
```

![image-20200317000344157](..\img\image-20200317000344157.png)

#### 3、Json格式的例外，基本类型转为JSON字符串

```
	如果将基本数据类型转为JSON字符串，则有值但没有键也没有{}圈着。然后传给前端时，前端对这个JSON字符串进行解析，转换成基本数据类型。这个是不是JSON字符串我不太清楚。但只要知道基本数据类型可以传到前端就够了。
```



![image-20200328182117029](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200328182117029.png)

![image-20200328182047998](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200328182047998.png)

### 五、案例

	* 校验用户名是否存在
		1. 服务器响应的数据，在客户端使用时，要想将JSON字符串当做JSON数据格式来使用。有两种解决方案：
			[1]客户端将接受到的数据，用dataType:"json"，客户端则会作为JS对象来解析.
				（1）$.get(type):将最后一个参数type指定为"json"
				（2）$.post(type):跟(1)同理
				（3）$.ajax():把dataType键值设置为"json"（"json"不区分大小写）
			[2]客户端不设置的情况下，在服务器端设置MIME类型。告诉客户端以JSON格式来解析。
				response.setContentType("application/json;charset=utf-8");
#### 	1、服务器端的代码

![image-20200317115340153](..\img\image-20200317115340153.png)

##### 		（1）响应JSON字符串

###### 						①写法一：

![image-20200317120446371](..\img\image-20200317115501883.png)

###### 						②写法二：

![image-20200317115842451](..\img\image-20200317115842451.png)



#### 	2、客户端代码

![image-20200317121319602](..\img\image-20200317120628367.png)



![image-20200317121404882](..\img\image-20200317121404882.png)

#### 	3、结果

![image-20200317121423815](..\img\image-20200317121423815.png)

![image-20200317121436429](..\img\image-20200317121436429.png)