



### 一、定义

​	

```
	当访问服务器资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。
```

```
	过滤器常见字符串：过滤一些敏感字符串、避免中文乱码【在过滤器中规定编码方式，规定Web资源都使用UTF-8编码】、权限验证【规定只有带Session或Cookie的浏览器，才能访问web资源】等等等。过滤器的作用非常强大，只要发挥想象就可以有意想不到的效果。
```



### 二、Filter接口方法

​	

```

	1、init：在服务器启动后，会创建Filter对象，然后调用init方法，只执行一次，用于加载资源。

	2、doFilter：Filter的核心方法，用于执行Filter。每一次要请求被拦截的资源时，就会执行，可以执行多次。

	3、destroy：在服务器关闭后，Filter对象被销毁。如果服务器正常关闭，则会执行destroy方法，只执行一次，用于释放资源.
	
```



### 三、操作步骤

#### 	1、定义一个类实现Filter接口

```
	注意：Filter接口的包为javax.servlet.*
```

#### 	2、复写Filter接口方法

#### 	3、配置拦截路径

```
意思是客户端要访问的资源若是拦截路径里填写的，那过滤器就会生效
```

#### 	4、决定是否放行

#### 	5、具体实现

​		实现Filter的类，通过filterChain的doFilter方法决定是否放行，如果不放行，则客户端访问的资源访问不到。如果放行，则客户端可以访问到资源。

##### 			①未放行

![image-20200302132213226](..\img\image-20200302132213226.png)



![image-20200302132301186](..\img\image-20200302132301186.png)

```
因为FilterDemo1没有放行，所以访问不到index.jsp的资源，结果如下：			
```

​	

![image-20200302132243036](..\img\image-20200302132243036.png)



##### 			②放行后

![image-20200302132447754](..\img\image-20200302132447754.png)

​						因为放行了，所以index.jsp资源被访问到了。

![image-20200302132528147](..\img\image-20200302132528147.png)

![image-20200302132515641](..\img\image-20200302132515641.png)

### 四、Filter配置方式	

#### 	1、拦截路径多种写法

##### 		（1）具体资源路径 

​			

```
	如拦截路径为:/index.jsp,则客户端要访问index.jsp资源时，就要执行过滤器
```



##### 		（2）拦截目录

```
	如拦截路径为：/user/* ，则客户端如果要访问user下的资源时，执行该过滤器。
```

​			①定义一个Filter.拦截路径为/User/*，意为拦截此路径下的所有资源。

![image-20200302142022306](..\img\image-20200302142022306.png)

​			②创建两个User目录下的Servlet.![image-20200302142036885](..\img\image-20200302142036885.png)

![image-20200302142049804](..\img\image-20200302142049804.png)

​			③结果

![image-20200302142106090](..\img\image-20200302142106090.png)

![image-20200302142125452](..\img\image-20200302142125452.png)

![image-20200302142142559](..\img\image-20200302142142559.png)

![image-20200302142153085](..\img\image-20200302142153085.png)



##### 		（3）后缀名拦截

​				

```
	如拦截路径为： *.jsp:如果客户端要访问后缀名为jsp的资源时，则执行过滤器
```

##### 		（4）全资源拦截

​				

```
	如：/*: 客户端访问资源时，就会执行该过滤器。
```



```
	配置方式有两种：XML配置法和web.xml配置法
```



#### 	2、XML配置法

​			

```
在web.xml文件中配置，需要填写的参数暂时用XXX代替。
```



```
    <filter>
        <!--用于为过滤器指定一个名字，该元素的内容不能为空。-->
        <filter-name>XXX</filter-name>
        
        <!--元素用于指定过滤器的完整的全类名-->
        <filter-class>XXX</filter-class>
        
    </filter>
    
    
    <filter-mapping>
    
        <!--设置filter的名称，和上面一样的名字-->
        <filter-name>XXX</filter-name>
        
        <!--设置 filter 所拦截的请求路径。-->
        <url-pattern>XXX</url-pattern>
        
    </filter-mapping>
```

​	

##### 	（1）具体演示

###### 			①web.xml内

![image-20200302134031219](..\img\image-20200302134031219.png)

###### 			②Filter内

![image-20200302134108552](..\img\image-20200302134108552.png)

###### 			③结果

​	![image-20200302134121245](..\img\image-20200302134121245.png)

![image-20200302134134476](..\img\image-20200302134134476.png)



#### 	3、注解配置法

```
	即在实现Filter接口的类上添加一个@WebFilter(“拦截路径”)。
```

#### 	4、过滤器拦截方式配置

#### 		（1）四种拦截方式

###### 				①REQUEST

```
		默认值，客户端直接访问资源时，过滤器生效。
```



###### 				②FORWARD

```
		当客户端直接访问资源时，过滤器不生效，只有该资源转发给另一个资源时，过滤器才生效。
```



###### 				③INCLUDE

```
		包含访问资源（了解）
```



###### 				④ERROR

```
		跳转错误页面，执行过滤器（了解）
```

#### 		（2）注解设置拦截方式

​			

```
	通过dispatcherTypes设置属性
```

​			演示

![image-20200302145914908](..\img\image-20200302145914908.png)

​						

![image-20200302145852279](..\img\image-20200302145852279.png)

![image-20200302145838026](..\img\image-20200302145838026.png)

​		![image-20200302145815983](..\img\image-20200302145815983.png)

![image-20200302145941623](..\img\image-20200302145941623.png)

​				

```
	未经过过滤器情况,直接访问ForwardServlet，而不是转发。
```

​		![image-20200302150111634](..\img\image-20200302150111634.png)

![image-20200302150126477](..\img\image-20200302150126477.png)

​					

##### 思考题

```
如果FilterDemo1注解的拦截路径设置为/*,且拦截方式为FORWARD和REQUEST,那么Filter会被执行几次？答:两次
```

![image-20200302150338076](..\img\image-20200302150338076.png)

![image-20200302150726885](..\img\image-20200302150726885.png)

![image-20200302150738466](..\img\image-20200302150738466.png)

​		![image-20200302150757408](..\img\image-20200302150757408.png)

![image-20200302150809339](..\img\image-20200302150809339.png)

#### 		（3）web.xml拦截方式

```
	配置在<filter-mapping>的<url-pattern>下，把属性配置在<dispatcher>标签里即可
```

```
	<filter-mapping>
        <filter-name>myfilter</filter-name>
        <url-pattern>/test.jsp</url-pattern>
        <dispatcher>REQUEST</dispatcher>
        <dispatcher>FORWARD</dispatcher>
	</filter-mapping>
```



### 五、执行流程

#### 	1、执行过滤器

#### 	2、执行放行后的资源

#### 	3、资源执行完成后，回到过滤器执行放行代码下边的代码

```
	完整执行流程：当服务器收到客户端请求后，如果有过滤器，则会通过过滤器过滤掉一些东西或增强请求，然后放行后，如果还有过滤器拦截，则进入过滤器。如果没有，则Servlet执行，等Servlet执行完后，还需要经过过滤器处理，处理完响应给客户端。
```

![image-20200302135112304](..\img\image-20200302135112304.png)



![image-20200302135347211](..\img\image-20200302135347211.png)

![image-20200302135359636](..\img\image-20200302135359636.png)

![image-20200302135426382](..\img\image-20200302135426382.png)

### 六、多个Filter(Filter链)执行顺序	

#### 		1、执行顺序

​				若有两过滤器，过滤器1和过滤器2				

​			（1）执行过滤器1

​			（2）执行过滤器2

​			（3）执行资源

​			（4）回到过滤器2

​			（5）回到过滤器1

​		

#### 		2、过滤器先后顺序问题

#### 			（1）注解配置

```
		按照类名的字符串比较规则去比较，值小的先执行。
```

```
		例子：如AFilter和BFilter，过滤器类名，由字符串比较规则比，第一个字符和第二个字符进行比较A比B小，所以AFilter就先执行了。
```

![image-20200302140007027](..\img\image-20200302140007027.png)





![image-20200302140019186](..\img\image-20200302140019186.png)

![image-20200302140032118](..\img\image-20200302140032118.png)

![image-20200302140044109](..\img\image-20200302140044109.png)

![image-20200302140057834](..\img\image-20200302140057834.png)



#### 			（2）web.xml配置

```
			<Filter-mapping>标签谁定义在上边，谁先执行
```

![image-20200302144452681](..\img\image-20200302144452681.png)

​		FilterDemo2的filter-mapping标签在FilterDemo1上，所以先执行

![image-20200302144517379](..\img\image-20200302144517379.png)

​		FilterDemo1：

![image-20200302144536675](..\img\image-20200302144536675.png)

![image-20200302144624724](..\img\image-20200302144624724.png)







### 七、过滤器的应用

#### 	1、登录验证

​		

```
		需求

			（1）对之前做过的登录查询案例进行改造，访问时，验证其是否登录

			（2）如果登录了，则直接放行

			（3）如果没有登录，则跳转到登录界面，提示"你尚未登录，请先登录."
```

![image-20200302161817040](..\img\image-20200302161817040.png)

#### 	2、用来设置整个应用的字符编码 

#### 3、敏感词汇过滤

```
import jdk.nashorn.internal.ir.WhileNode;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.*;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.Set;

@WebFilter("/*")
public class SensitiveStringFilter implements Filter {
    public void destroy() {
    }

    //定义一个List集合存储IO流读取到的字符串.
    private List<String> list = new ArrayList();

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        //生成代理对象
        ServletRequest request = (ServletRequest) Proxy.newProxyInstance(req.getClass().getClassLoader(), req.getClass().getInterfaces(), new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                //判断是否是getParameter方法，是则进入
                if (method.getName().equals("getParameter")) {
                    String value = (String) method.invoke(req, args);
                    //替换字符串.
                    if (list != null) {
                        for (String str : list) {
                            if (value.contains(str)) {
                                value = value.replace(str, "****");
                            }
                        }
                    }
                    return value;
                }
              //判断是否是getParameterMap，由于getParameterMap只能读不能写的原因，不知道怎么过滤
              //判断是否是getParameterValues.  
                return method.invoke(req, args);
            }
        });

        //将代理对象传到Servlet去
        chain.doFilter(request, resp);
    }

    public void init(FilterConfig config) throws ServletException {
        ServletContext context = config.getServletContext();
        //获取真实路径
        String realPath = context.getRealPath("/WEB-INF/classes/敏感词汇.txt");
        try {
            //对字符流进行编码设置
            InputStreamReader is = new InputStreamReader(new FileInputStream(realPath), "UTF-8");
            BufferedReader br = new BufferedReader(is);
            String str = null;
            while ((str = br.readLine()) != null) {
                list.add(str);
            }

            br.close();
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

}

```

