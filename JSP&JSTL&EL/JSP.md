### 一、定义

```
	JSP是一个特殊的页面，它简化了书写.里面既可以定义HTML标签，又可以定义Java代码。解决了html页面不能动态获取数据和用Servlet写html页面的麻烦.
```

### 二、原理

![](..\img\JSP原理.png)

​	

```
	JSP本质上是一个Servlet。原理是找JSP资源，如果找到了，将jsp转换成.java文件，编译.java文件，生成.class字节码文件，由字节码文件响应请求。那什么样的字节码可以被外界访问到？那就是实现Servlet接口的类，所以它本质是一个Servlet。所以，JSP的html页面其实就是Servlet通过在service方法中调用输出流对象将页面标签输出到页面.
```

### 三、JSP的脚本

#### 	1、定义

```
	JSP脚本定义了写Java代码的方式,即可以在脚本中运行Java代码.JSP脚本可以写在页面的任意位置。即body标签里、head标签里还有<head/>和<body>之间.
```

#### 	2、脚本内容

		（1）<% 代码 %>：定义的java代码最终经过转换成字节码文件，会在Service方法中，Service方法能写什么，它就能写什么.
		
		（2）<%! 代码 %>：只能定义成员变量和成员方法。jsp转换成java类后，将其放在Java类的成员位置。（用的比较少）
		
		（3）<%= 代码 %>：定义的java代码，会输出到页面上。输出语句中可以定义什么，该脚本中就可以定义什么。即可以输出字符串也可以输出定义在<%%>上的变量.但输出字符串要加上双引号，输出变量<%%>要写在<%=%>上面。
#### 	3、脚本演示

```
	演示（1）（3）：可以定义多个<%%>和 <%=%>，里面写的代码都是在同一个Service方法中。并且定义的多个JSP脚本是按顺序执行代码的.
```

  ![image-20200220105157732](..\img\image-20200220105157732.png)

![image-20200220105242602](..\img\image-20200220105242602.png)

![image-20200220105212745](..\img\image-20200220105212745.png)

### 四、JSP内置对象

#### 	1、定义

```
	在jsp页面中不需要创建，直接使用的对象,因为在JSP转换成java文件的Service方法中，这些对象已经声明好了.
```

#### 	2、内置对象（九个）

```
		变量名						真实类型						作用
	* pageContext				PageContext				当前页面共享数据，除了这个页面，不能共享到其他页面.还可以获取其他八个内置对象
	* request					HttpServletRequest		一次请求访问的多个资源(转发)
	* session					HttpSession				一次会话的多个请求间
	* application				ServletContext			所有用户间共享数据
	* response					HttpServletResponse		响应对象
	* page						Object					当前页面(Servlet)的对象  this
	* out						JspWriter				输出对象，数据输出到页面上
	* config					ServletConfig			Servlet的配置对象
	* exception					Throwable				异常对象
```

```
pageContext演示
```

![image-20200222165441111](..\img\image-20200222165441111.png)

```
结果：输出a
```



```
	* out：字符输出流对象。可以将数据输出到页面上。和response.getWriter()类似
	 * response.getWriter()和out.write()的区别：
	 * 在tomcat服务器真正给客户端做出响应之前，会先找response缓冲区数据，再找out缓冲区数据。
		* response.getWriter()数据输出永远在out.write()之前
```

```
		JSP脚本按照顺序输出有一个特例，即response.getWriter输出对象总比out输出流对象先输出,我们尽量不要写getWriter往页面写数据.
```

![image-20200220111111473](..\img\image-20200220111111473.png)

![image-20200220111122290](..\img\image-20200220111122290.png)

![image-20200220111053599](..\img\image-20200220111053599.png)

### 五、指令

#### 		1、作用

```
		用于配置JSP页面，导入资源文件
```

#### 		2、格式

		<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>
#### 		3、指令分类

#### 			（1）page

```
		①作用：配置JSP页面的
		②属性
			[1]、contentType：等同于response.setContentType().
				I、 设置响应体的mime类型以及字符集(字符编码）
				II、设置当前jsp页面的编码（只能是高级的IDE才能生效，如果使用低级工具，则需要设置pageEncoding属性设置当前页面的字符集）
			[2]、import：导java类的包
			[3]、errorPage：当前页面发生异常后，会自动跳转到指定的错误页面
			[4]、isErrorPage：标识当前也是是否是错误页面。
				* true：是，可以使用内置对象exception
				* false：否。默认值。不可以使用内置对象exception
			errorPage配合isErrorPage使用.
```

​	

```
	使用Page指令的import导包属性时，导包应该写成多行.
```

![image-20200222155122308](..\img\image-20200222155122308.png)

​	

```
使用Page指令的errorPage属性，如果当前页面发生异常，则转发到500.jsp中.
```

![image-20200222155734459](..\img\image-20200222155734459.png)

![image-20200222165057847](..\img\image-20200222165057847.png)

![image-20200222165017456](..\img\image-20200222165017456.png)

#### 			（2）include(了解即可)		

			①作用：页面包含的。导入页面的资源文件,用于在JSP中插入一个包含文件或代码的文件.
				* <%@include file="top.jsp"%>
#### 			（3）taglib	

			①作用：此指令是导入一个标签库以及其自定义标签的前缀.
			* <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
			* prefix：前缀，自定义的,一般命名为c.定义为c后，用JSTL标签都要加前缀<c:XXX>
![image-20200222161806208](..\img\image-20200222161806208.png)

### 六、注释

#### 	1、html注释：

​		<!-- -->:只能注释html代码片段

#### 	2、jsp注释：用于注释JSP脚本或html标签.

​		<%-- --%>：可以注释所有

![image-20200222162237704](..\img\image-20200222162237704.png)

### 七、操作

```
截断操作
```

![image-20200220112530659](..\img\image-20200220112530659.png)

![image-20200220112631133](..\img\image-20200220112631133.png)

![image-20200220112713787](..\img\image-20200220112713787.png)

![image-20200220112631133](..\img\image-20200220112631133.png)

### 八、JSP四大域对象

```
cookie不是域对象是因为它存储于客户端，在服务器内传递存储数据的就是域对象.域对象主要用于传递数据。
```



```
	真实类型			对象名 		作用范围
	PageContext        pageContext 	JSP当前页面之中

	HttpServletRequet  request 		一次请求

	HttpSession        session   	一次会话多次请求之间

	ServletContext     application 	同一个web应用下

```

​			

```
可以叫（类型+域），也可以（对象名+域），如PageContext域或page域.	
```

