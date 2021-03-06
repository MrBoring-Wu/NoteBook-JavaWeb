### 一、概念

```
XML是一门可拓展的自定义标记语言。可拓展的意思是标签都是自定义的。
```

### 二、功能与html区别

#### 		1、功能：存储数据

​				（1）存储数据，作配置文件使用。

​				（2）存储数据，在网络中传输.

#### 		2、与HTML区别 

​				（1）XML标签都是自定义，而HTML标签是预定义的。

​				（2）XML用于存储数据，HTML用于展示。

​				（3）XML语法非常严格，而HTML语法松散。

### 三、语法

```
若想判断写的XML文件对不对，只需放在浏览器里，通过浏览器的xml解析引擎解析，不报错就证明写对了。
```

#### 		1、基本语法

```
（1）XML文档后缀名为".xml"。

（2）xml第一行必须要有文档声明<?xml version='1.0'?>，而且必须定义在第一行。如果第一行为空行，会报错。

（3）xml文档中有且仅有一个根标签。

（4）属性值必须使用引号(单双引号两者皆可)引起来。

（5）标签必须正确关闭。

（6）xml标签名称区分大小写。
	如果一个标签写成<user>另一个标签写成<User>，则这是错误的，因为标签名称区分大小写。
```

#### 	2、具体操作

​			（1）定义一个xml文件

![image-20200313092523605](F:\Java集合\Java全面复习\img\image-20200313092523605.png)

​			（2）文档内容

​		![image-20200313092332441](F:\Java集合\Java全面复习\img\image-20200313092332441.png)

### 	四、XML组成部分

#### 		1、文档声明

##### 			 	（1）格式

​						**<?xml 属性列表 ?>**

```
			其中?xml之间不能有空格,不然会报错。属性列表的属性值用单/双引号引起来。						如:<?xml version="1.0" ?>
```

##### 				（2）属性列表

​					①version:版本号，必要的属性，不写会报错。

```
			1.1版本不向下兼容，1.0版本是最常用的。所以用1.0。
```

​					②encoding：编码方式。

```
			告知解析引擎当前文档使用的编码方式，默认值：ISO-8859-1.
```

​					③standalone：判断XML是否独立,两个值 yes/no。

​						若为yes，则这个xml文件是独立的，不依赖其他文件。

​						若为no，依赖其他文件

```
			一般情况下不会设置这个值，即使用了yes，还是可以依赖其他文件。
```

#### 		2、指令：结合css(了解)

#### 		3、标签命名规则

​					（1）数字、标点符号不能开头

​					（2）名称不能包含空格

​					（3）名称不能以XML(不区分大小写)开头。

​					（4）名称可以包含数字、字母及其它字符。

#### 		4、属性

```
	在xml中，如果要想属性值唯一，则只能通过约束来解决。而属性名称为id,它的属性值并非唯一。因为要唯一就要用到约束。
```

#### 		5、文本

​				CDATA区

```
	在该区域中的数据会被原样展示,适合展示代码。若我们在标签里写代码，会出现一些特殊字符，如果不转义则会报错，我们就需要转义一下，但转义后，别人可能就看不懂了，所以就出现了CDATA区，用于原样的展示数据。
```

​		<img src="F:\Java集合\Java全面复习\img\image-20200313095136870.png" alt="image-20200313095136870" style="zoom:50%;" />	

```
	格式:<![CDATA[展示数据]]>	
	先一对尖括号,里面再写一个"！",在后写上中括号,中括号里写上CDATA,后跟[].
```



### 四、约束

#### 		1、为何有约束？

```
	约束规定了xml文档的书写规则。
```

```
	如下图,因为框架是已经写好了的东西，那它怎么识别xml里的内容呢？答案是定义一份约束文档，说明XXX标签可以用来干什么。我们程序员只需要读懂框架提供的约束文档，来写出对应的xml文件即可。
```

![](F:\Java集合\Java全面复习\img\约束.bmp)

​			

```
    程序员只需要：
    	1、引入约束文件
    	2、读懂约束文件
```

#### 		2、约束的分类

​			（1）DTD:一种简单的约束技术

​			（2）Schema:一种复杂的约束技术

#### 			3、DTD约束

##### 			（1）格式

​					①文件后缀名为dtd格式。

​					②DTD编写如图

![image-20200313152150990](..\img\image-20200313145849842.png)

​	

##### 			（2）引入dtd文档到xml文件内

###### 						①内部dtd

```
			将约束规则定义在xml文档中。格式:<!DOCTYPE 写约束文件的根标签名  [约束内容]>
```

![image-20200313151127322](..\img\image-20200313151127322.png)

###### 						**②外部dtd**

```
			约束规则定义在外部的dtd文件中.
```

​						**I、本地****

```
				<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">
```

​							*基于(1)格式里的图，本地导入演示：*

​							定义文件

![image-20200313150056621](..\img\image-20200313150056621.png)

​							写入外部约束  

![image-20200313150036609](..\img\image-20200313150036609.png)

```
	必须要写全约束文档规定的标签，约束文件规定student下必须出现三个标签，不然会报错。因为student标签里还差一个sex标签，所以报错。
```

![image-20200313150134533](..\img\image-20200313150134533.png)

```
	因为约束文档规定了number的属性值是唯一的，所以若存在和number属性值相同的，则会报错。
```

![image-20200313150222604](..\img\image-20200313150222604.png)

​						**II、网络引入**

			<!DOCTYPE 根标签名 PUBLIC "dtd文件名字" "dtd文件的位置URL">
#### 			4、Schema约束

```
	Schema的出现是为了弥补DTD不足的地方，在DTD约束中，不能规定标签数据里的取值范围，而Schema可以。		
```

##### 		使用

​			①读懂![image-20200313162733530](..\img\image-20200313162733530.png)![image-20200313162418070](..\img\image-20200313161441792.png)![image-20200313162538060](..\img\image-20200313162509857.png)

​			②引入

			1.填写xml文档的根元素
	
			2.引入xsi前缀（一个固定的格式). 
				xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	
			3.引入xsd文件命名空间. 通过xsi来引入文件对应的路径
			（schemaLocation：schema文档路径）
	xsi:schemaLocation="(http://www.itcast.cn/xml)(给后面的路径起一个名字，被称为命名空间)  student.xsd(文件具体路径)"
	
			4.给每一个xsd约束声明(即命名空间)加一个前缀，接着把这个前缀弄在标签前。如<前缀:标签><前缀:标签/> 
![image-20200313171925462](F:\Java集合\Java全面复习\img\image-20200313171925462.png)



### 五、解析（读取）

#### 	1、解析

```
	将文档中的数据读取到内存中。
```

#### 	2、写入

```
	将内存中的数据保存到xml文档中。持久化存储。（后期用不多，不详细讲解）
```

#### 	3、XML解析思想

##### 		（1）Dom解析思想

```
		将标记语言文档一次性加载进内存，在内存中形成一颗DOM树。
```

##### 		（2）SAX思想

```
		逐行读取，读一行释放一行。基于事件驱动来完成数据获取。
```

![image-20200313173645864](F:\Java集合\Java全面复习\img\image-20200313173645864.png)

		<title>文档标题</title>相当于读三行。左右半标签各为一行，内容为另一行。

```
	逐行读会有一个严重的问题。用<title>文档标题</title>举例，因为title读完就被释放掉了，则在逐行读的时候，怎么会知道"文档标题"是属于title标签里的内容，而非其他标签的？所以要基于事件的驱动，来完成数据的获取。也就是说判断是否是开始标签，若是开始的标签就触发监听器代码。如：若是title就怎么怎么样，若是a标签就怎么怎么样。 
```

##### 		（3）两种思想优缺点

###### 			 	①优点

​							Dom思想操作方便，可以对文档进行CRUD的所有操作。

​							SAX思想不占内存（主流手机都采用SAX思想）.

###### 			 	②缺点

​							Dom思想占内存，因为一次性加载进内存，若DOM树过大，则会占内存，导致卡顿。

​							SAX思想只能读取，不能增删查改。

```
			服务端开发用DOM思想，移动端用SAX思想。
```



#### 	4、 XML解析器

##### 				（1）JAXP

```
		sun公司提供的解析器，支持dom和sax两种思想。性能低，写起来麻烦，所以基本不使用。
```

##### 				（2）DOM4J

```
		一款非常优秀的解析器，基于DOM思想来解析XML.
```

##### 				（3）Jsoup

```
		Jsoup 是一款Java 的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据。它是基于DOM思想，也可以解析XML.
```

##### 				（4）PULL

			PULL：Android操作系统内置的解析器，sax方式的。
### 六、解析器Jsoup

#### 	1、定义

```
	基于DOM思想，在JAVA中解析XML或HTML文档。
```

#### 	2、操作

##### 			（1）导入jar包

```
		jsoup-1.11.2.jar.
```

##### 			（2）获取Document对象，代表整个DOM的文档结构。

```
		通过加载文件进内存，获取Document对象。
```

##### 			（3）获取对应的标签，其实就是获取Element对象

##### 			（4）通过Element对象获取数据

##### 			（5）演示

![image-20200313183825524](F:\Java集合\Java全面复习\img\image-20200313183519921.png)

![image-20200313183537146](F:\Java集合\Java全面复习\img\image-20200313183537146.png)

#### 	3、对象使用	

##### 		（1）Jsoup

```
	工具类，可以解析html或xml文档，返回Document.
```

​					**parse：Jsoup里的方法，解析html或xml文档，返回Document对象.**

​					获取Document对象的方式:

###### 						①	parse(File in,String charseName)

```
			解析xml或html文件的.（常见于解析xml文档）
```

###### 						②	parse(String html)

```
			解析xml或html字符串的，通过在字符串上写xml内容，然后传入到这个方法里，以此来获得数据.不太常用。
```

###### 						③parse(URL url,int timeoutMillis)

```
			通过网络路径获取指定的html和xml文档对象。(解析html最常用，常见用于爬虫)
```

###### 						④演示

第二种方式演示:

![image-20200314134112372](F:\Java集合\Java全面复习\img\image-20200314134112372.png)

![image-20200314134127743](F:\Java集合\Java全面复习\img\image-20200314134127743.png)

第三种方式：

![image-20200314135413264](F:\Java集合\Java全面复习\img\image-20200314135413264.png)

​	打印结果

![image-20200314135438780](F:\Java集合\Java全面复习\img\image-20200314135438780.png)

​		

##### 	（2）Document

```
	文档对象，代表内存中的dom树，Document对象重写了toString方法，所以打印出来的就是整个html或xml文档的内容.
```

###### 			①方法

```
获取Element对象
	getElementById(String id)：根据id属性值获取唯一的element对象(这里的通过id属性值查找，是指查找标签属性名称为id的属性值。)
	
	getElementsByTag(String tagName)：根据标签名称获取元素对象集合
	
	getElementsByAttribute(String key)：根据属性名称获取元素对象集合
	
	getElementsByAttributeValue(String key, String value)：根据对应的属性名和属性值获取元素对象集合
```

###### 			②演示

​			演示一![image-20200314141438528](F:\Java集合\Java全面复习\img\image-20200314141438528.png)

![image-20200314141457012](F:\Java集合\Java全面复习\img\image-20200314141457012.png)

​			演示二

![image-20200314141517111](F:\Java集合\Java全面复习\img\image-20200314141517111.png)

![image-20200314141529471](F:\Java集合\Java全面复习\img\image-20200314141529471.png)

##### 	（3）Elements

```
	元素Element对象集合，可以当做ArrayList<Element>来使用
```

##### 	（4）Element

```
	元素对象，可以获取元素名称，元素文本
```

​			①获取子元素对象，获取该元素对象下的子元素对象

```
	getElementById(String id)：根据id属性名称的属性值获取对应的元素对象
	
	getElementsByTag(String tagName)：根据标签名称获取元素对象集合
	
	getElementsByAttribute(String key)：根据属性名称获取元素对象集合
	
	getElementsByAttributeValue(String key, String value)：根据对应的属性名和属性值获取元素对象集合
```

​			②获取属性值

```
	String attr(String key)：根据属性名称获取属性值
```

​			③获取文本内容

```
	String text():获取文本内容
	String html():获取标签体的所有内容(包括字标签的字符串内容)
```

​			④演示

![image-20200314142655936](F:\Java集合\Java全面复习\img\image-20200314142655936.png)

![image-20200314142608952](F:\Java集合\Java全面复习\img\image-20200314142608952.png)

![image-20200314142637248](F:\Java集合\Java全面复习\img\image-20200314142637248.png)

##### （5）Node

```
	Document和Element的父类
```

#### 4、选择器

```
	用于快速查询文档内容.
```

##### 		（1）选择器分类

```
	两种任性一种使用
```

###### 									①selector

###### 									②XPath

##### 		（2）selector选择器

​				①使用

			通过document获取select方法。
			Elements select(String cssQuery)
​				②语法

```
		参考Selector类中定义的语法,传入的字符串类似于CSS选择器
```

​				③使用

![image-20200314144840588](F:\Java集合\Java全面复习\img\image-20200314144840588.png)

​	![image-20200314144938517](F:\Java集合\Java全面复习\img\image-20200314144938517.png)

![image-20200314144954249](F:\Java集合\Java全面复习\img\image-20200314144954249.png)

##### 		（3）XPath

```
	XPath即为XML路径语言，它是一种用来确定XML文档中某部分位置的语言。使用Jsoup的XPath要额外导包.因为XPath独立于Jsoup.导入的包：JsoupXpath-0.3.2.jar
```

​			①语法	

```
	查询w3cshool参考手册，使用xpath的语法完成查询
```

​			②使用

![image-20200314151125942](F:\Java集合\Java全面复习\img\image-20200314151125942.png)

![image-20200314150927832](F:\Java集合\Java全面复习\img\image-20200314150927832.png)

![image-20200314151039194](F:\Java集合\Java全面复习\img\image-20200314151039194.png)

![image-20200314151110168](F:\Java集合\Java全面复习\img\image-20200314151110168.png)

