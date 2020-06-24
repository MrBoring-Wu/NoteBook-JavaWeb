### 一、JSP演变历史

​	1、早期只有servlet，即想要输出动态数据，又想要输出静态标签数据，只能使用response输出标签数据，非常麻烦

​	2、后来有了jsp，简化了Servlet的开发，如果过度使用jsp，在jsp里既写大量的java代码，又写html标签，导致除了自己谁都看不懂，就会造成难于维护，难于分工协作。

​	3、再后来，java的web开发，借鉴mvc开发模式，使得程序的设计更加合理性

### 二、定义

```
浏览器请求资源时，首先要经过控制器，接着调用模型，模型返回数据给控制器，接着控制器将数据交给视图进行展示.


1.M：Model，模型。
* 完成具体的业务操作，如：查询数据库，封装对象
2. V：View，视图。
   * 展示数据
3. C：Controller，控制器。
   * 获取用户的输入
   * 调用模型
   * 将数据交给视图进行展示
   
 在Web开发中，Servlet充当Controller的角色.JSP充当了View，JavaBean充当了模型.以后JSP仅仅进行数据展示，那么不写java代码了，那将来怎么展示数据，用JSTL和EL表达式。
```

```
举个栗子，登录时：
1、用户在View的Input上输入手机号后，点击View上的Button，触发Controller调用Model的verifyMobile(mobile)接口。
2、Model.verifyMobile(mobile) 校验后不符合手机号格式，返回出错提示msg: {passed: false, message:"长度不对"}。
3、Controller将msg显示在View上Input上。你就看到 Duang！ 在手机输入框后边出现一个红色提示框提示长度不对。
4、用户修改成正确的手机号后，重复1、2，校验通过。Controller再调用login(mobile, password)方法来请求登录。
所以，MVC就是这么个东西。
```

### 三、优缺点

#### 	1、优点

```
（1）耦合性低，因为我们将来的代码分为三部分，互不干扰，所以耦合性低，也就方便维护，所以三部分可以单独找人写，也就利于分工协作.
（2）重用性高.
```

#### 	2、缺点：

```
使得项目架构变得复杂，对开发人员要求高
```

