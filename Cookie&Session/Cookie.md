### 一、会话技术

#### 1、一次会话

```
	定义：一次会话中包含多次请求和响应。
		*一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止.断开即浏览器关闭或服务器关闭.
```

#### 2、为什么会有会话技术？

```
原因就是Http协议是无状态的，即每次请求都是相互独立，不交互数据的.但服务器的业务必须是要有状态的，即不同请求间要交互数据，那怎么办？于是就有会话技术出现.
```

#### 3、功能

```
	在一次会话的范围内的多次请求间，共享数据.
```

#### 4、方式

```
	客户端会话技术：Cookie

	服务器端会话技术：Session
```

### 二、Cookie

#### 1、定义

```
Cookie是一门客户端会话技术，用于将数据存储到客户端.Cookie的最初目的是为了存储web中的状态信息，以方便服务器端使用.
```

#### 2、Cookie方法

		public String getName()：获取cookie的键名，即属性名
		public String getValue() ：获取cookie信息的值
		public void setMaxAge(int expiry)：设置Cookie存活时间
		public void setPath(String uri)：设置cookie在同一个服务器内的获取范围
		public void setDomain(String pattern):设置这个方法可以跨服务器共享cookie
#### 3、实现

#### 	（1）使用步骤

```
	[1]如何发送cookie？
		① 创建Cookie对象，绑定数据
			* new Cookie(String name, String value) 
		②发送Cookie对象
			* response.addCookie(Cookie cookie) 	
	[2]服务器端如何获取cookie？	
		① 获取Cookie，拿到数据
			* Cookie[]  request.getCookies()
        ②使用Cookie的getName方法和getValue方法获取键值
    		*public String getName()
    		*public String getValue() 
```

#### 	（2）代码实现

​			

```
	①响应Cookie给浏览器
```

​					![image-20200219104413858](..\img\image-20200219104413858.png)

​		

```
	②从浏览器中获取Cookie
```

![image-20200219200528850](..\img\image-20200219104516034.png)

​			

```
	③	访问
```

​													![image-20200219110846443](..\img\image-20200219110846443.png)

![image-20200219110916405](..\img\image-20200219110916405.png)

​			

```
	④效果
```

<img src="..\img\image-20200219111212607.png" alt="image-20200219111212607" style="zoom:50%;" />

#### 4、Cookie原理

```
	基于响应头set-cookie和请求头cookie实现
```

![](..\img\Cookie原理.png)

​	

```
	客户端请求服务器，服务器响应一个Cookie给客户端时，其实就是将set-cookie响应头传给客户端，当客户端接收到set-cookie值时就赋值在set-cookie响应头里.当客户端再次请求服务器时，会将请求头里的cookie附带给服务器.cookie和set-cookie的值都是键值对形式XXX=XXX。
```



#### 5、Cookie的细节

#### 	（1） 一次可不可以发送多个cookie?

		可以创建多个Cookie对象，使用response调用多次addCookie方法发送cookie即可，最终会发送多个set-cookie头给浏览器. 
![image-20200219124316801](..\img\image-20200219124316801.png)

#### 	（2）cookie在浏览器中保存多长时间？

		1. 默认情况下，当浏览器关闭后，Cookie数据被销毁
		2. 持久化存储：setMaxAge(int seconds)				//持久化存储就是将数据存储到硬盘
			（1） 正数：将Cookie数据写到硬盘的文件中。持久化存储。并指定cookie存活时间，时间到后，cookie文件自动失效。也就是说客户端关闭后，除非cookie过了存活时间，否则客户端请求服务器时，服务器还能获取到cookie信息。
			（2）负数：默认值
			（3）零：删除cookie信息
```
	正数
```

![image-20200219125440175](..\img\image-20200219125440175.png)

--------

​	

```
零
```

![image-20200219130041551](..\img\image-20200219130041551.png)

①访问：								![image-20200219130748852](..\img\image-20200219130748852.png)

②抓包看到的响应头信息：![image-20200219130729592](..\img\image-20200219130729592.png)



③访问：								![image-20200219131112273](..\img\image-20200219131112273.png)

④通过CookieDemo2获取不到cookie信息，得证![image-20200219130825777](..\img\image-20200219130825777.png)

-----



```
	Cookie持久化保存的问题，客户端请求服务器一个资源，里面的内容有设置cookie的持久化时间，如果再一次请求那个资源，cookie的持久化时间是否为叠加？
	不会的，如果再一次访问那个资源，会对set-cookie头进行重新赋值，并且重新设置持久化时间.
```



#### 	（3）cookie能不能存中文？

		（1）在tomcat 8 之前 cookie中不能直接存储中文数据。
			 需要将中文数据转码---一般采用URL编码(%E3)
		（2）在tomcat 8 之后，cookie支持中文数据，即可以直接设置和获取中文。但特殊字符还是不支持，建议使用URL编码存储，URL解码解析
#### 	（4）cookie共享问题？

​			①同个服务器下，不同Web项目能否共享Cookie?

			* 默认情况下cookie不能共享
	
			* setPath(String path):设置cookie的获取范围。在默认情况下，也就是说不去设置这个方法的情况下，就默认设置为当前的虚拟目录，则cookie的共享范围就是这个虚拟目录下的资源.
			* 如果要共享，则可以将path设置为"/"，/代表当前项目的根路径（文件目录最上层）,相当与http://localhost下的项目资源.
​				

```
在同一个服务器下部署两个项目.
```

![image-20200219133926266](..\img\image-20200219133926266.png)

```
项目结构
```

![image-20200219141321153](..\img\image-20200219141321153.png)

![image-20200219141334436](..\img\image-20200219141334436.png)

```
访问虚拟路径为Wu的资源，设置cookie.
```

![image-20200219134144934](..\img\image-20200219134144934.png)

```
访问虚拟路径为Jia的资源，获取cookie
```

![image-20200219134250574](..\img\image-20200219134250574.png)

```
结果
```

​			![image-20200219141236821](..\img\image-20200219141236821.png)



​		②不同Tomcat服务器是否能共享cookie？

```
	（1） setDomain(String path):如果设置一级域名相同，那么多个服务器之间cookie可以共享
	 例子：setDomain(".baidu.com"),那么tieba.baidu.com和news.baidu.com中cookie可以共享
		一级域名和二级域名
	 例子：tieba.baidu.com    news.baidu.com   tieba和news为二级域名 一级域名为.baidu.com
```

#### 6、Cookie的特点和作用

#### 	（1）特点

​	

```
	1、cookie存储数据在客户端浏览器

	2、浏览器对于单个cookie 的大小有限制(4kb) 以及 对同一个域名下的总cookie数量也有限制(20个)
```

#### 	（2）作用

```
	1、cookie一般用于存出少量的不敏感的数据

	2、 在浏览器不登录的情况下，完成服务器对客户端的身份识别，即不登陆浏览器的情况下，对浏览器进行偏好的设置，设置完成后就将cookie持久化存储到浏览器内部中，等到下一次打开时，就识别cookie.看看设置了哪些偏好.
```

#### 7、Cookie的应用

```
案例：记住上次访问的时间
需求：

	（1）访问一个Servlet，如果是第一次访问，则提示：您好，欢迎你首次访问.

	（2）如果不是第一次访问，则提示：欢迎回来，您上次访问时间为：显示时间字符串.
```


