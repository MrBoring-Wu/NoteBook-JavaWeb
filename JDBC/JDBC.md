### 一、定义

​		

```
	JDBC(Java DataBase Connectivity)是Sum公司提供的一组接口，定义了一套操作和连接所有关系型数据库的规则(很多接口).接口的实现由各大数据库厂商提供.对JDBC的实现，就叫做此数据库的驱动.
也就是驱动就是各大厂商对JDBC接口的实现.JDBC的功能主要是可以对数据库进行连接，以及对其进行一系列的增删查改等等.
```



### 二、JDBC操作过程

​      

```
  介绍：操作分成两类,到下面7的时候注意了，如果你要对数据库是DML操作，既增删改操作，则只需要简单的用PrepareStatement的executeUpdate()的方法执行即可.但如果你想要查询数据库数据且想要到其中的数据的时候，操作就很复杂了.参考应用场景.
```



#### 		1、导入Jar包 

```
mysql-connector-java-5.1.37-bin.jar
```

#### 		2、注册驱动

```
即告诉JAVA连接哪个数据库
```



#### 		3、获取连接

#### 		4、定义sql

```
注意：sql参数使用？作为占位符如：select * from user where username = ? and password = ?;
```

#### 		5、获取执行sql对象Preparedment对象 

#### 		6、必须给?赋值

```
既需要先调用Preparedment的方法进行赋值：setObject(参数1,参数2)：

​	  参数1：?的位置编号 从1开始               

​	  参数2：？的值
```

#### 		7、用PreparedStatement执行sql

#### 		8、释放资源.

#### 9、代码演示

```
  	//把conn和statement提到try外，防止try语句内没执行到,让finally找不到相应的对象.
  	Connection conn;
  	PreparedStatement statement;
  	try{
  		//注册驱动
    	Class.forName("com.mysql.jdbc.Driver");
    	//获取连接
 		conn = DriverManager.getConnection("jdbc:mysql:///day23","root","123456789");
      	//定义sql
     	String sql="update province set Name=? where id = ?";
     	//PareparedStatement是Statement的子类。获取对象
        statement = conn.prepareStatement(sql);
        //赋参
        statement.setObject(1,"湖南");
        statement.setObject(2,1);
        //执行并返回成功执行多少条。
        int i = statement.executeUpdate();
        System.out.println(i);
        }catch(Exception exception){
        	e.printStackTrace();
        }finally{
        	//如果statement=null 证明try语句内没执行到
        	if(statement!=null){
        	 	statement.close();
        	}if(conn!=null){
        		conn.close();
        	}
        }
       
```



### 三、六种核心

#### 		1.DriverManager：驱动管理类

​	功能:

##### 						  	①注册驱动

​		

```
   既装载数据库厂商提供的驱动.

​  怎么注册mysql驱动?将mysql提供驱动里的Driver类加载进内存，类完成加载后，会执行静态代码块.
	Class.forName("com.mysql.jdbc.Driver");		
```

​			   

```
static {
		try {
//java自带的DriverManager调用注册驱动功能，并传入Driver类，接下来个人理解，估计在里面读取
Driver类信息，接着完成驱动.确定是哪一个数据库.
		java.sql.DriverManager.registerDriver(new Driver());

		} catch (SQLException E) {
		throw new RuntimeException("Can't register driver!");
		}
	}		   
注意：mysql5之后的驱动jar包可以省略注册驱动的步骤。既省略掉Class.forName（“XXX”）这一步骤.
```

##### 								②获取数据库连接

```
方法：static Connection getConnection(String src,String user,String password);

​	{1}、url：指定连接的路径

​		[1]、语法：jdbc:mysql://本机地址:端口号(3306)/数据库

​		[2]、例子：jdbc:mysql://localhost:3306/db3

​		[3]、细节：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称

​	{2}、user：本机数据库用户名

​	{3}、password：密码
```



#### 		2.Connection:数据库连接接口

​	功能：

##### 									  ①获取执行sql的对象

​			

```
   {1}、Statement CreateStatement();

​  {2}、PreparedStatement prepareStatement(String sql);
```



##### 									   ②管理事务

​			

```
  {1}、开启事务：setAutoCommit(boolean autoCommit):调用该方法设置参数为false,关闭自动提交即 手动开启事务.

  {2}、提交事务:commit()

  {3}、回滚事务：rollback()
```



#### 		3.Statement:执行sql接口

##### 						   ①功能：

​	    

```
  用于执行静态sql语句，并返回其生成其结果的语句.静态sql：给定值的  动态sql：预编译语句
```



##### 			    		    ②sql注入问题		

##### ![](..\img\image-20200209192711188.png)

​			

```
	参照上面图片,用户随便输入一个用户名，然后在密码上做一些手脚，数据库就被破解了

	密码 a' or "a"="a"

	为了以防万一，预编译应运而生.
```

​					

#### 		4.PreparedStatement:执行sql接口

##### 										①解决sql注入问题

​		 	

```
	预编译的sql：使用参数?作为占位符
```



##### 										②方法

​		

```
	SetXxx(int index,String value):设置值，索引从1开始 

​	boolean execute():了解下即可

​	int executeUpdate():执行DML(增删改)语句（DDL：create,alter,drop增删改表语

句）：返回值为执行成功后的行计数，可以通过这个方法判断DML语句是否执行成功，返回值＞0则

执行成功，反之，失败。

​	ResultSet executeQuery():执行DQL(select)查询语句,
```



##### 				 					 	③练习

​			

```
	account表添加一条记录，如果添加记录，则控制台输出失败

	account表删除一条记录

	account表修改记录
```



#### 	    5.ResultSet:结果集接口

​		功能:主要用于获取结果

##### 					①获取表数据值

##### 					<img src="..\img\image-1.png" style="zoom:50%;" />

​				{1}、上图讲解：

```
	游标先从行头开始，然后要取数据，这时候就需要一个方法把它移到下一行,获取到ResultSet对象后如果想获取数据必须要先next，不然游标停在行头，获取不到值，会报错.
```

​				{2}方法介绍

```
						
    [1]next()：游标向下移动一行,如果到最后一行末尾了还向下移动，则报错true有数据，false则无数据了。

	[2]、getXxx（参数）：获取列数据,习惯用getObject方法.

		I、Xxx代表数据类型 如getInt() 既获取int返回值

		II、参数有两种类型：

			(1)、int型：代表列的编号从1开始，如getString(1) 获取第一列的值

			(2)、string型：代表列名称，输入名字获取值。    getString("id");
```



遍历:	

```
   while (result.next()){
            Object object = result.getObject(1);
            Object object1 = result.getObject(2);

            System.out.println(object);
            System.out.println(object1);

        }
```

##### 			 ②用于获取元数据对象

​			

```
	 ResultSetMetaData getMetaData()；
```



#### 6、ResultSetMetadata:元数据接口

​	  功能：

##### 			①获取数据库表的列名.

​			

```
	String getColumnLabel(int column):column即索引，也是从1开始。
```



##### 			②获取数据库表的列数.

​			

```
	int getColumnCount();
```



### 四、应用场景

#### 	1.练习：

​	

```
	{1}、定义一个方法，查询user(其他也可)的数据将其封装成对象，然后装载集合，返回. 

		步骤：[1]、定义User类
				
			 [2]、定义方法 public List<User> findAll(){}

			 [3]、实现方法 select  * from User；

	 {2}、案例：通过键盘录入用户名和密码,判断用户是否登录成功
```



#### 	2、JDBC工具类

​			{1}、定义：

```
	无论是连接操作还是关闭连接操作，每一次都要写，而且很长，所以写一个工具类简化代码.
```

​			{2}、工具类：JDBCUtils



​			{3}、工具类内容：		

​					

```
 	 [1]注册驱动

	 [2]写一个方法获取连接对象

	 [3]分别写两个close方法，用于重载.
```



​			 {4}、需求

```
	不想传参，保证工具类通用性，即换数据库的时候不需要修改相应的代码.
```

​			 {5}、解决需求：配置文件properties.

​					

```
  定义一个jdbc.properties配置文件.里面的值是键值对形式 ，这样做的好处是

参数有变化时，只需要改配置文件就可以，这样就不用去修改代码了,保证了通用性.
```

![](..\img\Snipaste_2020-02-10_22-45-13.png)

​						

```
  driver是注册驱动，为了保证通用性，因为驱动也会有变化，将来想换数据库的时候，直接在配置文件改就行了.
```



----------------------------------------------

​		  {6}、获取连接时遇到的问题					

 		  	[1]、问题：

​	   

```
	  	下面这样写获取getConnection，将来调用的时候获取的连接对象总是同一个，既day18数据库.将来会有很多数据库想换数据库连接对象又要重新修改代码，这使得此工具类不通用。
```

​	![](..\img\3.png)

​		  	 [2]、不可取的解决方法：

​					

```
		传参，传参虽可，但却没有任何优化的操作，那还不如不写getConnection方法
```

![](..\img\Snipaste_2020-02-10_10-22-12.png)

 ![](..\img\Snipaste_2020-02-10_10-21-36.png)

   		 [3]、标准方法：配置文件方式Properties

​		 	

```
		(1)、问题：使用下面这种读取，不能读取到文件，换成绝对路径就一定能找到，但写绝对路径不好，将来如果位置变了，又读取不到了，那怎么办
```

![](..\img\Snipaste_2020-02-10_10-48-53.png)

![](..\img\Snipaste_2020-02-10_10-49-07.png)

​		    

```
        (2)、解决方法：获取src路径下的文件方式--->ClassLoader 类加载器 可以加载字节码文件进内存，也可以获取src下的资源文件路径，要获取ClassLoader，随便一个字节码文件都可以，比如JDBCUtils字节码文件
```



----

#####  标准写法

```
public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;

    //文件的读取，只需要读取一次即可，不需要每次调用getConnection方法就读文件，所以用静态代码块，随类加载而加载
    static{
        //读取资源文件，获取值
        try {
            //1.创建properties集合类
            Properties prop=new Properties();
            // 获取src路径下的文件方式--->ClassLoader 类加载器 可以加载字节码文件进内存，也可以获取src下的资源文件路径

            //①以src为相对的根路径
            ClassLoader classLoader =JDBCUtils.class.getClassLoader();
            //②因为以src作为相对的根路径，又因为文件在src下，所以只需传一个文件名，就可以来获取资源
            //但如果配置文件弄在src下的文件夹下里，就要:文件夹/配置文件
            URL resource = classLoader.getResource("jdbc.properties");
            //③
            String path=resource.getPath();
            //三句合一句  String path=JDBCUtils.class.getClassLoader().getResource("jdbc.properties");.getPath;
            //load方法可以接受字符读取流和字节读取流，通过上面3行代码，就可以获取一个绝对路径
            prop.load(new FileReader(path));
            //配置文件的getProperty方法里输入配置文件的键名，获取值
            url=prop.getProperty("url");
            user=prop.getProperty("user");
            password=prop.getProperty("password");
            driver=prop.getProperty("driver");
            Class.forName(driver);
        }catch (IOException e) {
            e.printStackTrace();
        }
        catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
        }

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url,user,password);
    }
    public static  void close(Connection conn, Statement statement){
        close(conn,statement,null);
    }
    public static void close(Connection conn, Statement statement, ResultSet result){
        if(conn!=null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(statement!=null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(result!=null) {
            try {
                result.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

<img src="..\img\Snipaste_2020-02-10_19-11-27.png" style="zoom: 50%;" />







-----

#### 3、通用写法之查询操作

​		

```
因为一张表对应一个对象，而我们写的查询方法只针对某一个特定的对象，如果要弄另一个对象，就要修改代码，所以通用写法就出来了.查询操作标准写法可以查询多条数据：
```



#####     	 {1}、步骤： 

//建议T的基本数据类型写出封装数据类，如果数据库表里有数字为null时，会得到null值，接着Field//再将null值赋值给基本数据类型，就会报错,前提，类属性要对准表。

​				①创建一个返回值为List<T>的有参方法，该方法参数为Class <T>clazz,String sql,Object...args. 

​				②创建一个List对象

​				③调用写好的JDBCUtils工具类进行连接数据库操作.

​				④用Connection获取PrepareStatment对象

​				⑤用PrepareStatement对象的executeQuery方法获取ResultSet.

​				⑥用ResultSet获取ResultSetMetadata对象

​				⑦用ResultSetMetadata调用方法获取列数，用于接下来在for循环中弄索引				

​				⑧调用ResultSet的next方法配合while获取每行数据

​				⑨进入while中，首先先把对象弄出来，T t=(T) clazz.newInstance;

​				⑩接下来的操作就要用到⑤步骤中的列数了，用for循环操作

​				⑪进入for循环中 获取对应索引的列值，获取对应索引的列名，用于等下对对象属性进行赋值

​			    ⑫用Class对象得到Field类，进行赋值

​				⑬关闭连接.

​										

##### 		{2}、标准写法

```
//泛型方法在方法名称前面有一个<T>声明，它的作用是告诉编译器编译的时候就识别它的类型
    //字节码文件用于建立对象,args用于传参
 public static <T> List<T> findAll(Class<T> clazz, String sql, Object... args) {
        Connection conn = null;
        PreparedStatement statement = null;
        ResultSet resultSet = null;
        List<T> list = null;
        try {
            conn = JDBCUtils.getConnection();
            statement = conn.prepareStatement(sql);
            //对？赋值 要从0开始，因为args是一个数组
            for(int i=0;i<args.length;i++){
                statement.setObject(i+1,args[i]);
            }
            resultSet = statement.executeQuery();
            //结果集元数据：即行头各列名.
            ResultSetMetaData metaData = resultSet.getMetaData();
            list = new ArrayList<T>();
            //获取列数用于接下来resultSet赋值.
            int columnCount = metaData.getColumnCount();
            while (resultSet.next()) {
                //新建一个对象
                T t = (T) clazz.newInstance();
                //做一个循环操作,对每一列进行赋值
                for (int i = 1; i <=columnCount; i++) {
                    //获取第i列的值
                    Object object = resultSet.getObject(i);
                    //获取数据库表的i列别名
                    String columnLabel = metaData.getColumnLabel(i);
                    //接下来三步，就是获取属性，接着属性对象会对对象里对应的属性进行赋值.
                    Field field = clazz.getDeclaredField(columnLabel);
                    field.setAccessible(true);
                    field.set(t,object);
                }
                list.add(t);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.close(conn, statement, resultSet);
            return list;
        }
    }
```

4、ORM思想

​	ORM（ Object RelationshiP mapping）思想，表结构跟类对应；表中字段和类的属性对应；表中数据和对象对应；一条记录对应一个对象，将这些查询到的对象放到容器中（List,Set,Map）；

### 五、番外篇

​	1、Field类：

​		[1]、方法

​			public Field getDeclaredField(String name)：name是类的属性名,此方法可以获取任何类型的属性.

​			public Field getField(String name)：只能获取带有public修饰的属性.

​			public void setAccessible(boolean flag)：如果为true，则可以设置被private修饰的变量.

​			public void set(Object obj, Object value)：给对象的属性赋值,此方法通过getField方法可以知道所赋值的属性是什么类型，然后对value向下转型.

<img src="..\img\Snipaste_2020-02-10_21-00-11.png" style="zoom: 80%;" />

-----

### 六、事务

#### 	1、事务

```
	一个包含多个步骤的业务操作，如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。
```

#### 	2、操作：

​		   ①开启事务

​		   ②提交事务

​		   ③回滚事务

#### 	3、用Connection对象管理事务.

​			（1）、开启事务

```
	setAutoCommit(boolean flag):使用该方法设置参数为false，则成功开启事务.如果为true，则是自动提交，自动提交的意思就是说执行到哪就更新到哪。事务在哪里开启？在执行sql语句前开启事务.
```

​			（2）、提交事务

```
	commit();提交事务在哪提交？在使用sql语句执行完后提交事务.
```

​			（3）、回滚事务

```
	在哪回滚事务？在异常catch中设置回滚事务，因为程序发生异常后都会到catch语句里，所以回滚事务设置在里面就好了.
```

#### 	4、开启事务有什么好处？	

```
	举一个转账的例子吧，如果我们没开启事务，在转账的过程中，如果我们要转给另一个人，如果银行转账出现异常，那钱就人间蒸发了，谁都收不到.但要是开启事务，就算银行转账的过程中出现了异常，它就会进行重置操作，也就是回滚了，即把发出去的钱还给你.
```

​			

			  4. 代码：
	public class JDBCDemo10 {
	
	    public static void main(String[] args) {
	        Connection conn = null;
	        PreparedStatement pstmt1 = null;
	        PreparedStatement pstmt2 = null;
	
	        try {
	            //1.获取连接
	            conn = JDBCUtils.getConnection();
	            //开启事务
	            conn.setAutoCommit(false);
	
	            //2.定义sql
	            //2.1 张三 - 500
	            String sql1 = "update account set balance = balance - ? where id = ?";
	            //2.2 李四 + 500
	            String sql2 = "update account set balance = balance + ? where id = ?";
	            //3.获取执行sql对象
	            pstmt1 = conn.prepareStatement(sql1);
	            pstmt2 = conn.prepareStatement(sql2);
	            //4. 设置参数
	            pstmt1.setDouble(1,500);
	            pstmt1.setInt(2,1);
	
	            pstmt2.setDouble(1,500);
	            pstmt2.setInt(2,2);
	            //5.执行sql
	            pstmt1.executeUpdate();
	            // 手动制造异常
	            int i = 3/0;
	
	            pstmt2.executeUpdate();
	            //提交事务
	            conn.commit();
	        } catch (Exception e) {
	            //事务回滚
	            try {
	            //防止空指针异常
	                if(conn != null) {
	                    conn.rollback();
	                }
	            } catch (SQLException e1) {
	                e1.printStackTrace();
	            }
	            e.printStackTrace();
	        }finally {
	            JDBCUtils.close(pstmt1,conn);
	            JDBCUtils.close(pstmt2,null);
	        }
------

### 七、数据库连接池

####  1、为什么引进数据库连接池？	 

```
   我们使用数据库连接对象时，它是一访问数据库完，就关闭连接的.从多用户同时连接的层面上看，每一个用户去访问数据库时，都要去申请连接，最后再关闭.这个逻辑在生活中的例子相当于一家餐馆有客人时，就去招员工，员工服务完顾客后，就被解雇，有新客人来，就又招员工，这显然是不合理的。所以，引进了数据库连接池,数据库连接池存放了一些半成品数据库连接对象,每当用户去访问数据库时，数据库连接池会提供数据库连接对象给用户去用，用户用完后，就归还给数据库连接池.所以数据库连接池复用性高，且高效.
```

#### 2、定义

```
	本质上是一个容器(集合),存放数据库连接的容器.系统初始化后，容器被创建，容器中会申请一些数据库连接对象，当用户来访问数据库时,从容器中获取连接对象，当用户访问后，归还连接对象.且sum公司只提供了数据库连接池的接口，具体实现由各大数据库厂商提供.
```

#### 3、好处

​	

```
	①节约资源	

	②用户访问高效
```



#### 4、DataSource接口：数据库连接池接口

​	

```
	创建数据库连接池
```



#### 5、连接池分类

数据库连接池会自动注册驱动.所以不用写注册驱动.

​		（1）C3P0：C3P0是自动找配置文件的，因此配置文件要在src下,名称要指定名称.

	* 步骤：
		1. 导入jar包 (两个) c3p0-0.9.5.2.jar mchange-commons-java-0.2.12.jar ，
			* 不要忘记导入数据库驱动jar包
		2. 定义配置文件：
			* 名称： c3p0.properties 或者 c3p0-config.xml
			* 路径：直接将文件放在src目录下即可。
					配置文件中如果超过规定的最大值就会报错，那么如果同时超过规定的最大值怎么办，一般根据自身数据库设置大小，如果设置的足够好，到时候其实同时也不同时，因为它精确到毫秒。
		3. 创建核心对象 数据库连接池对象 ComboPooledDataSource
		4. 获取连接： getConnection
​		（2）Druid：阿里巴巴开放，性能高效，Druid可以随便放配置文件，起任意名称，这就意味着要不会自动加载，要手动加载路径.

```
	1. 步骤：
		1. 导入jar包 druid-1.0.9.jar
		2. 定义配置文件：
			* 是properties形式的
			* 可以叫任意名称，可以放在任意目录下
		3. 加载配置文件。Properties
		4. 获取数据库连接池对象：通过工厂来来获取  				 
			DruidDataSourceFactory.createDateSource(Properties prop)
		5. 获取连接：getConnection
```

####  6、开源数据库操作 

​		数据库连接池都必须要用相应的jar包以及驱动jar包和配置文件

​	①C3P0

```
package JDBCPool.C3P0;

import com.mchange.v2.c3p0.ComboPooledDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

/**
 * c3po演示
 */

public class C3P0Demo1 {
    public static void main(String[] args) throws SQLException {
        DataSource ds=new ComboPooledDataSource();
        Connection conn = ds.getConnection();
        System.out.println(conn);
    }
}

```

 

​	②Druid

```
	 //加载配置文件(配置文件放在src下了，所以就找的到文件)
        Properties pro = new Properties();
        InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
        pro.load(is);
        //获取连接池对象
        DataSource ds = DruidDataSourceFactory.createDataSource(pro);
        //获取连接
        Connection conn = ds.getConnection();
```

#### 7、工具类最终版本（日后写版本）

```
定义工具类
		1. 定义一个类 JDBCUtils
		2. 提供静态代码块加载配置文件，初始化连接池对象
		3. 提供方法
			1. 获取连接方法：通过数据库连接池获取连接
			2. 释放资源
			3. 获取连接池的方法//有的小框架只需要连接池就能获得连接对象，所以要定义下
```




```
  *代码：
   public class JDBCUtils {
    //1.定义成员变量 DataSource
    private static DataSource ds ;

    static{
        try {
            //1.加载配置文件
            Properties pro = new Properties();
            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            //2.获取DataSource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接
     */
    public static Connection getConnection() throws SQLException {
       if(ds!=null){
       return ds.getConnection();
    	}else{
    	return null;
    	}
    }
//获取连接池
    public static DataSource getDataSource(){
        return ds;
    }
 //关闭连接
    public static void close(Connection conn, Statement statement, ResultSet set){
         if(conn!=null){
             try {
                 conn.close();
             } catch (SQLException e) {
                 e.printStackTrace();
             }
         }
        if(statement!=null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(set!=null){
            try {
                set.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
     //关闭连接
    public static void close(Connection conn, Statement statement){
        close(conn,statement,null);
    }
```
### 八、Spring JDBC

#### 	1、操作步骤

​	Spring框架对JDBC的简单封装.提供了一个JDBCTemplate对象简化JDBC的开发

​	主要就是创建JdbcTemplate对象，并对此对象传入一个数据库连接池对象.

```
步骤：

​		1、导入jar包

​		2、创建jdbcTemplate对象，依赖于数据源DataSource

​				JdbcTemplate template=new JdbcTemplate(ds);

​		3、调用JdbcTemplate的方法来完成CRUD的操作
```

#### 	2、主要方法

		* update(String sql,Object...args):执行DML语句。增、删、改语句,返回值int类型
		
		* queryForMap(String sql,Object...args):查询结果将结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合
				* 注意：这个方法查询的结果集长度只能是1
				
		* queryForList(String sql,Object...args):查询结果将结果集封装为list集合
				* 注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中


​				
​		* query(String sql,RowMapper<T>rowMapper,Object...args):返回一个装有T类的List对象，如果没有查询到，则会返回一个长度为0的List<T>对象.
​		  * query的参数：RowMapper
​			  * 一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
​			  * new BeanPropertyRowMapper<类型>(类型.class)
​				(注意：使用这个方法时，类的基本数据属性要弄成数据类型封装类，如int--Integer，因为用这个方法时，BeanPropertyRowMapper会赋值给类属性，但如果对数据库查询时该属性对应的字段若为null，则返回null值时，会赋值给给类属性，就导致了报错，因为此时类属性是基本数据类型的，相当于把null赋值给基本数据类型。这个方法就算T类的属性跟数据库表格数量不一样，也没有关系)


​	
​	
​	
​		* queryForObject(String sql,RowMapper<T>rowMapper,Object...args)：查询结果，将结果封装为对象，如果没查询到，则报错
​			* 一般用于聚合函数的查询，即中间的位置一般传入基本数据类型封装类的字节码。


​			
​		注意：查询可以用取别名，即javaBean类可以取别名，到时候查询的时候将字段设置为别名，到时候就会把对应的值赋给javaBean定义的属性.
#### 3、练习：

* ```
		需求：
	
	​	1、修改数据
	
	​	2、添加一条记录
	
	​	3、删除刚才添加的记录
	
	​	4、查询id为1的记录，将其封装为Map集合
	
	​	5、查询所有记录，将其封装为List
	
	​	6、查询所有记录，将其封装为Emp对象的List集合
	
	​	7、查询总记录数
	```
	
	

​			

#### 4、Select查询操作最终版本

```
package JDBCPool.SpringJDBC;

import JDBCPool.Druid.JDBCUtils;
import JDBCPool.SpringJDBC.domain.User;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;

import javax.sql.DataSource;
import java.util.List;

//6、查询所有记录，将其封装为Emp对象的List集合
public class Pratice5 {
    public static void main(String[] args) {
        DataSource ds = JDBCUtils.getDataSource();
         JdbcTemplate template=new JdbcTemplate(ds);
         String sql="select * from user";
        List<User> query = template.query(sql, new BeanPropertyRowMapper<User>(User.class));
        for(User user:query){
            System.out.println(user);
        }
    }
}

```

```
    /**
			     * 7. 查询总记录数
			     */
			
			    @Test
			    public void test7(){
			    	DataSource ds=JDBCUtils.getDataSource();
			    	JdbcTemplate template=new JdbcTemplate();
			        String sql = "select count(id) from emp";
			        Long total = template.queryForObject(sql, Long.class);
			        System.out.println(total);
			    }
			
			}
```

### 九、最终写法

​	工具类最终写法

​	对数据库操作最后写法

### 十、BeanUtils工具类

#### 	 1、定义

###### 		

```
	用于简化数据封装，用于封装JavaBean.
```

#### 	2、JavaBean

​				

```
	（1）要求

		①必须是一个被public修饰的类

		②必须要有空参构造方法

		③成员变量被private所修饰

		④提供公共getXxx、setXxx方法

	（2）功能：用于封装数据.
```

#### 	3、方法

```
	1、setProperty(Object,String name,String value )：设置属性,对set方法进行赋值.

	2、getProperty(Object,String name,String value)：获取属性,调用get方法.

	3、populate(Object obj,Map map):把map集合的键值对信息，封装到对应的JavaBean中.(☆)
```

#### 	4、属性和成员变量辨析

 			

```
	属性就是 get和set方法 .它的定义规则是：set/get方法名，去掉set/get后，将剩余部分首字母小写得到的字符串就是这个类的属性。 如getXxx和SetXxx变成属性就是xxx.属性赋值给成员变量.
```

#### 	5、用法

​			

```
	参考笔记Servlet的登录案例.
```

​	 	

​										