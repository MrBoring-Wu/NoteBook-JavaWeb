### 一、关系型数据库与非关系型数据库

​	

```
	关系型数据库和非关系型数据库(NOSQL)并非对立的关系，而是互补的关系。NOSQL将数据存储于缓存之中，而关系型数据库将数据存储在硬盘之中 。NOSQL数据库的数据之间没有耦合，即没有关系。当关系型数据库数据量非常大时，操作关系型数据库非常耗时。如果我们要经常查询一些不太经常发生变化的数据，那查询起来一定会非常慢，且影响用户体验。所以有一种思想来解决上面的问题。缓存思想，即在内存中开辟一块区域，这块区域称为缓存。接下来，请求数据，从缓存中获取数据，缓存有对应的数据，则返回数据。若没有数据，则从数据库查询，将数据放入缓存，返回数据。这样再去请求同样的数据时，缓存就不用再去查询数据库，而是从自身里面拿数据返回，也就提高了性能。

	一般会将数据存储在关系型数据库中，在nosql数据库备份存储关系型数据库中需要的数据。
```

### 二、定义

```
	redis是一款高性能的NOSQL系列的非关系型数据库.存储是key:value的形式。数据之间没有关系，且数据存储在内存之中。
```

### 三、操作

```
	命令操作：数据的存储、获取和删除.
```

### 四、redis的数据结构

#### 		1、redis的key:value

```
	redis存储的是：key，value格式的数据，其中key都是字符串，value值有五种不同的数据结构.
```

```
		（1）字符串类型：string		

		（2）哈希类型 hash	理解为map格式

		（3）列表类型 list	理解为linkedlist格式

		（4）集合类型 set 	理解为HashSet格式

		（5）有序集合类型 sortedset
```

​	![](..\img\2.redis数据结构.bmp)

#### 		2、字符串类型 String

##### 			（1）存储

```
	set key value：存储键值
```

##### 			（2）获取

```
	get key ：获取值
```

##### 			（3）删除

```
	del key：删除键值
```

![image-20200318221551317](..\img\image-20200318221551317.png)

#### 		3、哈希类型 hash	

##### 				（1）存储

```
	hset key field value：存储值为键值对数据
```

##### 				（2）获取

```
	hget key field：获取指定的field对应的值
	hgetall key:获取所有的field和value
```

##### 				（3）删除

```
	hdel key field删除值里指定的键
```

![image-20200318222102580](..\img\image-20200318222102580.png)

#### 			4、列表类型 list

```
	可以添加元素到列表的头部（左边）或者尾部（右边）
```

##### 						（1）添加

		① lpush key value: 将元素加入列表左表
		② rpush key value：将元素加入列表右边
##### 						（2）获取

		lrange key start end ：范围获取	（0 -1获取所有值）
##### 						（3）删除

```
	①	lpop key： 删除列表最左边的元素，并将元素返回
	②	rpop key： 删除列表最右边的元素，并将元素返回
```

![image-20200319102233627](..\img\image-20200319102233627.png)

#### 	5、集合类型set

```
	不允许重复元素。
```

##### 		（1）存储

```
	sadd key value
```

##### 		（2）获取

```
	smembers key:获取set集合中所有元素
```

##### 		（3）删除

		srem key value:删除set集合中的某个元素	
![image-20200319102535341](..\img\image-20200319102535341.png)

#### 6、有序集合类型 sortedset

```
	不允许重复元素，且元素有排序.每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。
```

##### 	（1）存储

```
	存储：zadd key score value
```

##### 	（2）获取

```
    zrange key start end （获取所有值 不包括分数）
    zrange key start end withscores （获取所有值包括分数）
```



##### 	（3）删除

```
	zrem key value
```

![image-20200319103008045](..\img\image-20200319103008045.png)

#### 7、通用命令

##### 		（1）keys * 

```
	 查询所有的键
```

##### 		（2）type key 

```
	 获取键对应的value的类型
```

##### 		（3）del	key

```
	删除指定的key,删除键之后value自然就没了
```

![image-20200319103347987](..\img\image-20200319103347987.png)

### 三、redis持久化

#### 		1、持久化

```
	redis是一个内存数据库，内存的特点就是数据是临时的，不能被持久化的写入。当redis服务器重启，或者电脑重启，数据会丢失，我们 可以将redis内存的数据持久化保存到硬盘的文件中。
```

#### 		2、redis持久化机制

##### 			（1）RDB

###### 					①概念

```
	默认方式，不需要进行配置，默认就使用这种机制，在一定的间隔时间中，检测key的变化情况。
```

###### 					②具体操作

			1. 编辑redis.windwos.conf文件
				#   after 900 sec (15 min) if at least 1 key changed
				save 900 1
				#   after 300 sec (5 min) if at least 10 keys changed
				save 300 10
				#   after 60 sec if at least 10000 keys changed
				save 60 10000
			2. 重新启动redis服务器，并指定配置文件名称
				并在命令行上找到redis安装命令，并执行cd redis-server.exe redis.windows.conf命令。	
##### 			（2）AOF

###### 					①概念

```
	日志记录的方式，可以记录每一条命令的操作，可以每一次命令操作后来持久化数据。即一保存就写到日志上。
```

###### 					②操作

```
			1. 编辑redis.windwos.conf文件
				appendonly no（关闭aof） --> appendonly yes （开启aof）
				
				# appendfsync always ： 每一次操作都进行持久化
				appendfsync everysec ： 每隔一秒进行一次持久化
				# appendfsync no	 ： 不进行持久化
```

### 四、Java客户端  Jedis

```
Jedis: 一款java操作redis数据库的工具.
redis的默认端口为6379。
```

#### 	1、使用步骤

```
（1）下载jedis的jar包
（2）使用
	//1. 获取连接
    Jedis jedis = new Jedis("localhost",6379);
   	//2. 操作
   	jedis.set("username","zhangsan");
    //3. 关闭连接
    jedis.close();
```

![image-20200319112614585](..\img\image-20200319112614585.png)

#### 	2、Jedis操作各种redis中的数据结构

#### 				（1）操作字符串类型

```
	      存储:set(String key,String value)
			  setex(String key,int seconds,String value)方法存储可以指定过期时间的 key value
	        jedis.setex("activecode",20,"hehe");//将activecode:hehe键值对存入redis，并且20秒后自动删除该键值对

```

![image-20200320215617536](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320215617536.png)

​	![image-20200320215536353](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320215536353.png)

#### 		（2) 哈希类型hash ： map格式  


			hset
			hget
			hgetAll
			hdel
![image-20200320220814566](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320220814566.png)

```
   // 获取hash的所有map中的数据
```

![image-20200320221055392](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320221055392.png)

#### 	（3）列表类型list

```
linkedlist格式。支持重复元素
	方法	
         lpush / rpush：添加到最左或最右
		lpop / rpop：删除最左或最右
		lrange start end : 范围获取
```

![image-20200320221925653](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320221925653.png)

#### （4）集合类型set

```
			不允许重复元素
			sadd
			smembers:获取所有元素
			srem:删除元素
```

![image-20200320224509920](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320224509920.png)

#### （5）sortedSet

```
	不允许重复元素，且元素有排序
		zadd
		zrange
		zrem
```

![image-20200320225123166](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320225123166.png)

### 五、JedisPool连接池

#### 	1、连接池使用

```
		这个连接池是Jedis自带的.
	（1） 创建JedisPool连接池对象
	（2）调用方法 getResource()方法获取Jedis连接
			//0.创建一个配置对象
		     JedisPoolConfig config = new JedisPoolConfig();
		     config.setMaxTotal(50);
		     config.setMaxIdle(10);
		
		     //1.创建Jedis连接池对象
		     JedisPool jedisPool = new JedisPool(config,"localhost",6379);
		
		     //2.获取连接
		     Jedis jedis = jedisPool.getResource();
		     //3. 使用
		     jedis.set("hehe","heihei");
		     //4. 关闭 归还到连接池中
		     jedis.close();
```

没有配置对象的

![image-20200320231042935](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320231042935.png)

有对象的配置

​	![image-20200320231233579](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320231233579.png)

#### 2、连接池工具类

![image-20200320234222977](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320234222977.png)

![image-20200320234233685](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320234233685.png)

​				测试

![image-20200320234249466](F:\Java集合\吴某人自写JavaWeb笔记(version1.0)\img\image-20200320234249466.png)

### 六、Redis使用注意

```
	注意：使用redis缓存一些不经常发生变化的数据。数据库的数据一旦发生改变，则需要更新缓存。数据库的表执行增删改的相关操作，需要将redis缓存数据，再次存入。具体做法就是在service对应的增删改方法中，将redis数据删除。
```

