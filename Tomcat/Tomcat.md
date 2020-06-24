## 一、定义

#### 1、web相关概念回顾

##### 		 （1）软件架构

​			

```
	1、C/S：客户端/服务器端，也就是说C/S架构需要有一个客户端安装在电脑上，还有一个服务器端，两个同时作用，才能相互通信和访问

	2、	B/S：浏览器/服务器端，通过浏览器域名可以访问到不同服务器的内容，JavaEE开发都注重于B/S架构，它的主要优点就是可移植性强，在任何浏览器上进行访问而不用安装特定软件,维护升级简单.
```

​			客户端：凡是对服务器发出请求的都被称为客户端.

##### 		（2）资源分类

​				

```
	1、静态资源：每一次用户访问的资源都是一样的.如html,css,javascript,jpg...，浏览器内置了解析引擎，通过访问获取到的资源直接被浏览器解析.

	2、动态资源：可以进行逻辑处理或从数据库读取数据，根据不同条件进行显示，如用户访问相同资源后，获取的结果可能不一样.Servlet，JSP就属于动态资源，它把动态资源转成静态资源，再返回给浏览器.返回的过程就叫做响应.
```

![](..\img\资源分类.bmp)

```
	浏览器里有一个静态资源解析引擎，专门用于解析静态资源，我们向服务器请求静态资源的时候，服务器返回给我们一个静态资源，然后被浏览器解析，最后就变成我们能看到的页面.而浏览器请求动态资源时必须先转换成静态资源,再由静态资源返回给浏览器.返回的过程叫做响应.
```

##### 		（3）网络通信三要素

​			1、IP：电子设备在网络中的唯一标识.

​			2、端口：应用程序在计算机中的唯一标识.0-65536

```
	端口的理解：一个端口号可以被多个应用程序占用，但是如果我们想同时打开两个相同端口号的应用程序的时候，它是不行的，必须只能选其中一个打开.打开一个后，还想要打开另一个相同端口号的程序，则会提示端口被占用.
```

​			3、传输协议：规定了数据传输的规则.为什么有传输协议？服务器和浏览器相互之间的请求响应能成功的原因就是统一了传输的规则.如果没有统一，那可能就是鸡同鸭讲.

```
		1、TCP：安全协议,传输之前经过三次握手，确定对方在线才会传送数据，速度慢.

		2、UDP：不安全协议，即使对方不在也能传输数据，速度快.
```



#### 	2、web服务器软件

##### 			（1）服务器

```
	服务器就是安装了服务器软件的计算机.本质上还是一台计算机，并且性能高.我们之前安装过mysql的服务器软件在电脑上，那我们的电脑可以称为mysql服务器，可以接收用户的请求，处理请求，做出响应。那安装了web服务器软件，那电脑就可以称为web服务器.
	台式机还是笔记本、游戏本，工作站或是服务器，都可以统称为计算机，或者电脑。
```

##### 			（2）服务器软件

​					

```
	接收用户的请求，处理请求，做出响应.
```

##### 			（3）web服务器软件

​					

```
接收用户的请求，处理请求，做出响应.
```

​				

```
	①在web服务器软件中，可以部署web项目(运行在服务器的项目)，让用户通过浏览器来访问这些项目，如html，css这些资源，放到web服务器软件中，让别人去访问。

	②动态资源依赖于web服务器软件，即动态资源只能靠web服务器软件运行，所以web服务器软件有时被称为web容器.
```



#### 		3、常见的java相关web服务器软件



```
	JavaEE：java语言在企业级开发中使用的技术规范总和.javaEE一共规定了13项大的规范.
```



​			Weblogic：归属于Orcale公司，大型javaEE服务器，支持所有的JavaEE规范，收费.

​			Tomcat:Apache慈善组织，中小型javaEE服务器，支持少量规范，如：JSP，Servlet

​			

## 二、Tomcat操作

```
Tomcat服务器是由java编写的，它的运行依赖于电脑的JDK.
```

#### 	1、 Tomcat：Web服务器软件

​			1、下载

​			2、安装

​			3、卸载

​			4、启动

​				

```
	去bin目录下找startup文件，即可开启tomcat.开启了tomcat后，我们就可以通过浏览器访问项目了.因为tomcat自带一些项目，所以可以访问到.浏览器访问时，高级浏览器可以省略不用写http.写上电脑的IP地址和Tomcat的端口号即可.127.0.0.1/localhost:8080前者是本机IP地址的意思，后者是Tomcat的默认端口。访问自己：看到下面页面，证明tomcat启动成功.	访问别人：http://别人ip：8080
```

![image-20200212203705662](..\img\122.png)

​		

​			5、关闭

​			6、配置文件

![](..\img\tomcat目录结构.png)

```
bin:放的都是可执行的文件，Tomcat启动程序放在里面.	

lib:放的是依赖jar包，因为tomcat启动的时候，要通过一些jar包才能执行.

logs：logs目录用来存放tomcat在运行过程中产生的日志文件，非常重要的是在控制台输出的日志。

temp：临时文件，存放临时文件（清空不会对tomcat运行带来影响） 

webapps：存放web项目，web服务器软件可以部署web项目，那这些项目放哪里？放webapps下面.webapps目录用来存放应用程序，当tomcat启动时会去加载webapps目录下的应用程序。可以以文件夹、war包、jar包的形式发布应用。 当然，你也可以把应用程序放置在磁盘的指定位置，在配置文件中映射好就行。 

如果我们想把写好的项目部署到服务器，我们只需要放项目放在webapps即可。
```

<img src="..\img\13.png" alt="image-20200212202540999" style="zoom:50%;" />



```
work：work目录用来存放tomcat在运行时的编译后文件，例如JSP编译后的文件。 
清空work目录，然后重启tomcat，可以达到清除缓存的作用
```



#### 	2、问题		

​		(1)	启动报错：原因是端口号被占用

​				解决：最好的方式是第一种.

​					1、暴力：找到占用的端口号，并且找到相应的进程，杀死该进程

​					2、温柔：修改自身的端口号.

​						在conf目录下找到server.xml,找到这里，修改port和redirectPort值

​			<img src="..\img\image-202002122054261848.png" alt="image-20200212205930277" style="zoom:50%;" />

​		![image-20200212205734468](..\img\image-202002122056449326.png)

​          	 		两个redirectPort要一样,port要求全都不一样

​			<img src="..\img\Snipaste_2020-02-12_21-00-18.png" style="zoom: 50%;" />

  					一般会将tomcat的默认端口号修改为80,80端口号是http协议的默认端口号.

​						好处：在访问时，不用输入端口号

#### 3、部署项目方式

​		（1）直接将项目放到webapps目录即可

​				 

```
		把hello文件夹放webapps里，然后hello文件夹夹里有hello的html文件，输入IP地址：(端口号/hello/hello.html )完成访问。/hello：项目的访问路径就是虚拟目录/虚拟路径

		项目路径和项目访问路径两者概念是不一样的.前者是文件放置在磁盘下的位置，后者则是虚拟目录，就是说这个虚拟目录可以改成任意名字.相当与映射的概念，把虚拟目录映射到项目路径.
```



			简化部署：将项目打成一个war包，再将war包放置到webapps目录下。
			war包会自动解压缩
​		（2）第一种缺点部署项目的时候需要把项目拷贝到webapps目录下，那我把项目放在指定磁盘位置可以吗？可以

​				配置conf/server.xml文件

​				在<host>标签体中配置

​				<Context docBase="D:\hello" path="/hehe" />

​						docBase:项目存放位置

​						path：虚拟目录

​				<img src="..\img\image-20200212212735284.png" alt="image-20200212212735284" style="zoom:50%;" />

在server.xml配置文件可能会误删其他内容，又因为Server.xml是整个项目的核心，所以很危险

​		

​		（3）热部署：
​				

```
		在conf\Catalina\localhost创建任意名称的xml文件。在文件中编写
<Context docBase="D:\hello" />* 指定项目存放路径，虚拟目录为xml文件的名称。
```

​	<img src="..\img\M1.png" style="zoom:50%;" />

<img src="..\img\M2.png" style="zoom:50%;" />

#### 4、静态项目和动态项目

​	 静态项目和动态项目，静态项目只能放静态的资源，动态项目可以放静态资源和动态资源

​		

```
动态项目的目录结构：

				根目录----

						--Web-INF--（Web-INF内的资源，浏览器是访问不到的）

									---web.xml：web项目的核心配置文件

									---classes：放字节码文件

									---lib目录：放置依赖jar包
```

![image-20200212214033924](..\img\image-20200212214033924.png)

​	有Web-INF的就是动态项目

![image-20200212214115282](..\img\image-20200212214115282.png)

​	因为Root项目没有动态资源，且不依赖jar包，所以就不需要classes和lib文件夹

#### 5、在Intellij IDEA中使用TomCat	

​	即把Tomcat弄到IDEA中操作

​	https://blog.csdn.net/wsjzzcbq/article/details/89463304

#### 6、IDEA中web项目如何部署到Tomcat？

 https://blog.csdn.net/weixin_39722922/article/details/89084873 

 https://blog.csdn.net/qq_33442160/article/details/81347319?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task 

#### 7、Tomcat创建多实例