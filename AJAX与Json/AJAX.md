### 一、异步和同步的概念

​		我们建立在客户端和服务器端相互通信的基础上，去说明同步和异步。概念参照下图。

![](..\img\1.同步和异步.bmp)

#### 		1、同步

```
	客户端必须等待服务器端的响应。在等待的期间客户端不能做其他操作。
```

#### 		2、异步

```
	在服务器处理请求的过程中，客户端可以进行其他的操作。
```

#### 		3、区别

```
	所以客户端需不需要等待服务器端的响应，就是同步和异步的区别。
```

### 二、Ajax定义

```
	ASynchronous JavaScript And XML，即异步的JavaScript 和 XML。是一种异步技术，可以在无需重新加载网页的情况下，能够更新部分网页内容的技术。它也可以在请求服务器时，不用等待服务器响应，而可以做其他的操作。传统的网页（不使用 Ajax），如果需要更新内容，必须重载整个网页页面。
```

​	Ajax是一种在无需重新加载网页的情况下，能够更新部分网页内容的技术。在请求服务器时，不用等待服务器响应，而可以做其他的操作。传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。



```
	在验证码案例中，img图片是在服务器生成的，那我们在客户端获取img图片，且修改 img 标签的 src，其实是发起一个异步的 get 请求。“算是异步技术的一种，但不是 AJAX。 
```

### 三、Ajax的好处

```
	提升用户的体验，让用户操作更加连贯。
```

### 四、实现

#### 	1、原生JS实现方式(看下就行了)

```
	//1.创建核心对象
      var xmlhttp;
      if (window.XMLHttpRequest){
      	// code for IE7+, Firefox, Chrome, Opera, Safari
        xmlhttp=new XMLHttpRequest();
      }else{
      	// code for IE6, IE5
        xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
      }
	//2. 建立连接
  		/*
      		参数：
          		1. 请求方式：GET、POST
              		* get方式，请求参数在URL后边拼接。send方法为空参
              		* post方式，请求参数在send方法中定义
          		2. 请求的URL：
          		3. 同步或异步请求：true（异步）或 false（同步）

  		*/
      xmlhttp.open("GET","ajaxServlet?username=tom",true);

	//3.发送请求
      xmlhttp.send();

	//4.接受并处理来自服务器的响应结果
	//获取方式 ：xmlhttp.responseText
	//什么时候获取？当服务器响应成功后再获取

  //当xmlhttp对象的就绪状态改变时，触发事件onreadystatechange。
  xmlhttp.onreadystatechange=function()
  {
      //判断readyState就绪状态是否为4，判断status响应状态码是否为200
      if (xmlhttp.readyState==4 && xmlhttp.status==200)
      {
         //获取服务器的响应结果
          var responseText = xmlhttp.responseText;
          alert(responseText);
      }
  }
```
#### 2、JQeury实现方式

```
	即对Ajax实现的封装。有三种实现方式。在Ajax中，url填写请求路径可以带虚拟路径也可以不带虚拟路径。不带虚拟路径去请求对应的Servlet时，不要带/，直接写访问的Servlet名就行了。
```

##### 	(1)$.ajax()

###### 						①语法					

```
	$.ajax({键值对});	
	基本键:url,type,data,success,errorpage,dataType。(键值对按需求写)多对键值对用逗号隔开。
```

###### 		②参数

​			I、contentType

```
	主要设置你发送给服务器的格式,只有在发送json字符串格式时，才要加这个，表明是json格式的字符串.
```

​			II、传送的数据格式

		①json格式
			{“username”:”chen”,”nickname”:”alien”}
			不用加contentType
		
		②json字符串格式
			"{"username":"chen","nickname":"alien"}"
			用此格式get请求参数传递不过去，不会把json串解析成参数。而且如果是post请求需要添加 
	 		contentType:"json/application".
	 		只有这种方式使用request的getParameter时，获取不到值.
		
		③标准参数模式
			"username=chen&nickname=alien"
			$(“#xxx”).serialize()是将对应表单中的input的name和value进行拼装，最后拼成标准参数模式的字符串。不用加contentType.
###### 					  ③演示

```
	使用$.ajax()发送异步请求
```

![image-20200316120755597](..\img\image-20200316120755597.png)





##### 	(2)$.get()：发送get请求

​			①语法

```
	$.get(url, [data], [callback], [type])：接收四个参数，第一个参数必须，后面其他三个可选(中括号就是可选的意思)
	参数：url：请求路径
		 data：请求参数
		 callback：回调函数
		 type：响应结果的类型
```

​			②实现方式

![image-20200316123354004](..\img\image-20200316123354004.png)

##### 	(3)$.post()：发送post请求

​		①语法

```
	与$.get()方式类似.

	$.post(url, [data], [callback], [type])：中括号表示参数可写可不写.
```

​		②实现

![image-20200316123540645](..\img\image-20200316123540645.png)