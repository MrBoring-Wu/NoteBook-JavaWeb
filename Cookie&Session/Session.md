### 一、定义

```
服务器端会话技术，在一次会话中共享数据，将数据保存在服务器端的对象中。
```

### 二、方法

	Object getAttribute(String name)  ：获取值
	void setAttribute(String name, Object value)：设置值
	void removeAttribute(String name)  :移除值
	String getId()：获取HttpSession对象唯一的Id值
### 三、操作

	1. 获取HttpSession对象：
		HttpSession session = request.getSession();
	2. 使用HttpSession对象进行获取、存储或移除数据.
```
设置session的Servlet
```

![image-20200220144404630](..\img\image-20200220144404630.png)

```
获取session的Servlet
```

![image-20200220144418493](..\img\image-20200220144418493.png)

```
访问
```

​	![image-20200220144829228](..\img\image-20200220144829228.png)

![image-20200220144905275](..\img\image-20200220144905275.png)	

```
获取到值
```

![image-20200220144915504](..\img\image-20200220144915504.png)

```
当关闭浏览器后，再去访问GetSession，获取到的是null，说明确实是在一次会话中，才能使用session共享数据.
```

![image-20200220145011336](..\img\image-20200220145011336.png)

### 四、原理

```
Session的实现是依赖于Cookie的。
```

![image-20200220150215658](..\img\image-20200220150215658.png)

```
服务器如何保证在一次会话范围内，多次获取的HttpSession对象是同一个？
```



```
首先，当浏览器访问服务器时，如果是第一次要获取HttpSeesion对象，则会在内存中创建一个新的HttpSession对象，并且这个HttpSession对象有唯一的ID，假如这个对象id为XXX，之后服务器给客户端一个响应头set-cookie：JESSIONID=XXX。当客户端再一次请求服务器时，会附带请求头cookie：JESSIONID=XXX，如果是有资源要获取HTTPSession对象，会通过这个唯一id，去内存中找到这个对象。所以服务器就是用cookie的方式来确保获取的是同一个HttpSession对象。这也就解释了为什么用request获取HttpSession对象的原因，因为要从请求头里拿ID，如果请求头里没有对应的id值，就会在内存中新建。如果有，则会拿到HttpSession对象。
```

```
注意：这个唯一的ID和内存地址是不同的.
```



### 五、细节

#### 1、当客户端关闭后，服务器不关闭，两次获取session是否为同一个？

	（1）默认情况下。不是。
	（2）如果需要相同，由于session依赖于cookie，所以我们只需将cookie持久化保存即可。
			//设置键名为JESSIONID以及ID值
			 Cookie c = new Cookie("JSESSIONID",session.getId());
	         //设置一个小时的存活时间
	         c.setMaxAge(60*60);
	         //发送cookie
	         response.addCookie(c);
```
按照（1）的
```

![image-20200220152140422](C:\Users\86181\Desktop\Java全面复习\img\image-20200220152140422.png)

```
获取到的
```

![image-20200220152155362](C:\Users\86181\Desktop\Java全面复习\img\image-20200220152155362.png)

```
关闭并重新打开客户端进行访问
```

​		![image-20200220152246107](C:\Users\86181\Desktop\Java全面复习\img\image-20200220152246107.png)

```
两个session对象的内存地址不一样，足以说明关闭客户端后，再次获取session不是同一个.
```

-------



```
按照（2）的
```

​		![image-20200220161326314](C:\Users\86181\Desktop\Java全面复习\img\image-20200220161326314.png)

```
	第一次访问此资源时，会把session的Id存在set-cookie响应头里，等关闭客户端，再重新访问时，会附带存放着id的cookie请求头给此资源，此资源通过这个id去内存找session，最终获取出来的是同一个对象。
```

![image-20200220161342710](C:\Users\86181\Desktop\Java全面复习\img\image-20200220161342710.png)

#### 2、客户端不关闭，服务器关闭后，两次获取的session是同一个吗？

	是同一个，为了确保数据不丢失。本地的tomcat自动完成以下工作。
			（1）session的钝化：
				* 在服务器正常关闭之前，将session对象系列化到硬盘上
			（2）session的活化：
				* 在服务器启动后，将session文件转化为内存中的session对象即可。
```
在IDEA里不能自动完成钝化，活化的原因：

因为session对象序列化存放在work目录里，IDEA的服务器一关闭后，就放在work目录里，接着Idea的服务器重新启动后，就会自动删除work目录，所以它没有办法获取数据也就不能活化.那怎么办？没有关系，因为将来部署项目不可能在IDEA本地部署，将来会放在tomcat的web-apps目录下面居多.所以放在tomcat的web-apps目录，下work目录就不会被删除了。
```

#### 3、session什么时候被销毁？

		（1）正常关闭本地服务器不会销毁。而IDEA下关闭服务器就会销毁。
		（2）session对象调用invalidate() 。
		（3）session 默认30分钟不活动，就会被服务器自动删除掉
			在tomcat的conf目录里的web.xml进行选择性配置修改	
			<session-config>
		        <session-timeout>30</session-timeout>
		    </session-config>
		（4）浏览器cookie销毁时.
### 六、特点与区别

#### 	1、特点

```
			（1）session用于存储一次会话的多次请求的数据，存在服务器端

			（2）session可以存储任意类型，在内存足够的前提下，可以存储任意大小的数据。
```

#### 	2、session与Cookie的区别：

​	

```
		（1）session存储数据在服务器端，Cookie在客户端

		（2）session没有数据大小限制，Cookie有	

		（3）session数据安全，Cookie相对于不安全
```

### 七、案例

1. 案例需求：
	
	（1）访问带有验证码的登录页面login.jsp
	
	（2）用户输入用户名，密码以及验证码。
	```
		①如果用户名和密码输入有误，跳转登录页面，提示:用户名或密码错误.
	
		②如果验证码输入有误，跳转登录页面，提示：验证码错误.
	
		③如果全部输入正确，则跳转到主页success.jsp，显示：用户名,欢迎您.
	```
	
	​    已做：ToDoSession的LoginCase里