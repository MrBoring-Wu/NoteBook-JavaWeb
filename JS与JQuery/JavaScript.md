### 一、定义

### 二、Bom

### 三、Dom

#### 	1、概念

```
	Document Object Model 文档对象模型，定义了访问HTML和XML文档的标准.将标记语言文档的各个组成部分，封装成对象。可以使用这些对象，对标记语言文档进行增删改查的动态操作。
```

#### 	2、DOM标准

​		W3C DOM标准被分为3个不同的部分：

​		（1）DOM-针对任何结构化文档的标准模型：核心，最基本的DOM模型

​		（2）XML DOM-针对XML文档的标准模型

​		（3）HTML DOM-针对HTML文档的标准模型

#### 	3、DOM				

​		[1]、核心DOM的对象模型：				

​			（1）Document：文档对象，将这个文档封装为文档对象

​			（2）Element：元素对象

​			（3）Attribute：属性对象

​			（4）Text：文本对象

​			（5）Comment：注释对象	

​			（6）Node：节点对象，其他5个的父对象。因为其他5个对象相当于继承了Node对象。

​		[2]、核心对象模型（Document、Element、Attribute）

​				

​			

​		

#### 4、Dom树

![](F:\Java集合\Java全面复习\img\DOM树.bmp)

```
html文档加载进了浏览器内存之后，将文档转换成树形结构，即dom树。整个文档被封装为文档对象，文档对象下面有根元素对象<html>，html元素对象下面又有两个元素对象.head和body.以此类推。
```

#### 5、Document文档对象

​		（1）创建（获取）：我们是基于Html dom来讲解的，在html dom模型中可以使用window对象来获 取.

​				1、window.document	

​				2、document

​		若学习XML Dom则不是按照上面两种方式获取，因为我们现在是基于HTML DOM讲解，所以是以HTML dom方式。

​	（2）我们要想学习核心Dom 模型，要查看XML Dom，而不查看HTML DOM,因为html dom做的修改比较多，XML Dom修改的比较少。

​	（3）方法：

​			1、获取Element对象：

​				（1）getElementById（）：根据id属性值获取元素对象。id属性值唯一。

​				（2）getElementsByTagName（）：根据元素名称获取元素对象们，返回的是数组。

​				（3）getElementsByClassName():根据Class属性值获取元素对象们，返回值是一个数组。

​				（4）getElementsByName：根据name属性值获取元素对象们，返回的是一个数组。

![image-20200305152004771](F:\Java集合\Java全面复习\img\image-20200305152004771.png)

​			2、创建其他DOM对象：

​		createAttribute(name)

​		createComment()

​		createElement():创建一个Element对象，用的比较多.

![image-20200305152343882](F:\Java集合\Java全面复习\img\image-20200305152343882.png)

![image-20200305152407215](F:\Java集合\Java全面复习\img\image-20200305152407215.png)

​		现在创建了，相当于内存里面弄了个table标签，但跟html整个文档没有任何都行。

​		

​	createTextNode()



6、Element：元素对象

​		1、	获取创建：通过document来获取和创建

​		2、方法

​			（1）removeAttribute（String key）：删除属性

​			（2）setAttribute（String key，String value）：给元素设置属性		

![image-20200305152857031](F:\Java集合\Java全面复习\img\image-20200305152857031.png)

7、节点对象

​		节点对象是其他五大对象的父，所以它的方法其他五个对象都能用。

​		特点：所有dom对象都可以被认为是一个节点。文档内容被称为文本节点，元素对象被称为元素节点。



​	方法：

​		*CRUD的DOM树方法

​			 appendChild():向节点的子节点列表的结尾添加新的子节点。

​			removeChild():删除并返回当前节点的指定子节点。

​			replaceChild():用新节点替换一个子节点。

​	属性：parentNode,可以获取父节点对象





操作：

![image-20200306132903816](F:\Java集合\Java全面复习\img\image-20200306132903816.png)

![image-20200306132915772](F:\Java集合\Java全面复习\img\image-20200306132915772.png)

![image-20200306132927426](F:\Java集合\Java全面复习\img\image-20200306132927426.png)

![image-20200306132937973](F:\Java集合\Java全面复习\img\image-20200306132937973.png)



![image-20200306132953195](F:\Java集合\Java全面复习\img\image-20200306132953195.png)

![image-20200306133001915](F:\Java集合\Java全面复习\img\image-20200306133001915.png)





动态表格

![image-20200306142125482](F:\Java集合\Java全面复习\img\image-20200306142125482.png)

![image-20200306142141134](F:\Java集合\Java全面复习\img\image-20200306142141134.png)

![image-20200306142155180](F:\Java集合\Java全面复习\img\image-20200306142155180.png)

![image-20200306142206162](F:\Java集合\Java全面复习\img\image-20200306142206162.png)

![image-20200306142217633](F:\Java集合\Java全面复习\img\image-20200306142217633.png)

![image-20200306142229466](F:\Java集合\Java全面复习\img\image-20200306142229466.png)













HTML Dom

Dom里的内容HTML DOM都可用

HTML独有的属性

1、标签体的设置和获取：innerHTML

![image-20200306143630694](F:\Java集合\Java全面复习\img\image-20200306143630694.png)



![image-20200306143642474](F:\Java集合\Java全面复习\img\image-20200306143642474.png)

![image-20200306143722263](F:\Java集合\Java全面复习\img\image-20200306143722263.png)

![image-20200306143731799](F:\Java集合\Java全面复习\img\image-20200306143731799.png)

追加内容

![image-20200306143749886](F:\Java集合\Java全面复习\img\image-20200306143749886.png)



![image-20200306143809551](F:\Java集合\Java全面复习\img\image-20200306143809551.png)

2、使用html元素对象的属性

3、控制样式

1、使用元素的style属性来设置

![image-20200306144019562](F:\Java集合\Java全面复习\img\image-20200306144019562.png)

![image-20200306144028561](F:\Java集合\Java全面复习\img\image-20200306144028561.png)

​		若是 font-size类似于这样，则将-去掉，后边单词首字母大写fontSize.

2、提前定义好类选择器的样式，通过元素的className属性来设置class属性值。

![image-20200306144427122](F:\Java集合\Java全面复习\img\image-20200306144427122.png)







四、事件

​	概念：某些组件被执行了某些操作后，触发某些代码执行。

​			事件：某些操作（如：单击操作，双击的操作）

​			事件源：组件。如： 按钮 文本输入框...

​			监听器：代码。

​			注册监听：将事件，事件源，监听器结合在一起。 当事件源上发生了某个事件，则触发执行某个监听器代码。

		 常见的事件：
	1. 点击事件：
		1. onclick：单击事件
		2. ondblclick：双击事件
	2. 焦点事件
		1. onblur：失去焦点
			一般用于表单验证，比如文本框写入后，失去焦点后来判断用户名格式是否正确。只能失去一次.只有再次获取焦点，再离开焦点后，就会失效。
		2. onfocus:元素获得焦点。
	
	3. 加载事件：
		1. onload：一张页面或一幅图像完成加载。
			window.onload：当整个窗口加载完了，再执行js代码。
	4. 鼠标事件：
		1. onmousedown	鼠标按钮被按下。
			定义方法时，定义一个形参，接受event对象
			event对象的button属性可以获取鼠标按钮被点击了。
				0鼠标左键 中键1  右键2
		2. onmouseup	鼠标按键被松开。
		3. onmousemove	鼠标被移动。
		4. onmouseover	鼠标移到某元素之上。
		5. onmouseout	鼠标从某元素移开。
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>常见事件</title>

    <script>
        /*


        常见的事件：
            1. 点击事件：
                1. onclick：单击事件
                2. ondblclick：双击事件
            2. 焦点事件
                1. onblur：失去焦点。
                    * 一般用于表单验证
                2. onfocus:元素获得焦点。

            3. 加载事件：
                1. onload：一张页面或一幅图像完成加载。

            4. 鼠标事件：
                1. onmousedown	鼠标按钮被按下。
                    * 定义方法时，定义一个形参，接受event对象。
                    * event对象的button属性可以获取鼠标按钮键被点击了。
                2. onmouseup	鼠标按键被松开。
                3. onmousemove	鼠标被移动。
                4. onmouseover	鼠标移到某元素之上。
                5. onmouseout	鼠标从某元素移开。


            5. 键盘事件：
                1. onkeydown	某个键盘按键被按下。
                2. onkeyup		某个键盘按键被松开。
                3. onkeypress	某个键盘按键被按下并松开。

            6. 选择和改变
                1. onchange	域的内容被改变。
                2. onselect	文本被选中。

            7. 表单事件：
                1. onsubmit	确认按钮被点击。
                    * 可以阻止表单的提交
                        * 方法返回false则表单被阻止提交。
                        给form表单绑定
                2. onreset	重置按钮被点击。
         */





        //2.加载完成事件  onload
        window.onload = function(){
            /*//1.失去焦点事件
            document.getElementById("username").onblur = function(){
                alert("失去焦点了...");
            }*/
            /*//3.绑定鼠标移动到元素之上事件
            document.getElementById("username").onmouseover = function(){
                alert("鼠标来了....");
            }*/

           /* //3.绑定鼠标点击事件
            document.getElementById("username").onmousedown = function(event){
               // alert("鼠标点击了....");
                alert(event.button);
            }*/

          /*  document.getElementById("username").onkeydown = function(event){
                // alert("鼠标点击了....");
               // alert(event.button);
                if(event.keyCode == 13){
                    alert("提交表单");
                }

            }*/

           /* document.getElementById("username").onchange = function(event){

                alert("改变了...")

            }

            document.getElementById("city").onchange = function(event){

                alert("改变了...")

            }*/

            /*document.getElementById("form").onsubmit = function(){
                //校验用户名格式是否正确
                var flag = false;


                return flag;
            }*/
        }

        function checkForm(){
            return true;
        }


    </script>

</head>
<body>

<!--
    function fun(){
       return  checkForm();
    }

 -->



<form action="#" id="form" onclick="return  checkForm();">
<input name="username" id="username">

<select id="city">
    <option>--请选择--</option>
    <option>北京</option>
    <option>上海</option>
    <option>西安</option>
</select>
<input type="submit" value="提交">
</form>
</body>
</html>
```

6、表格全选

​	按钮全选思路

​	给每个单选框添相同的name，当点击按钮后，进入方法.由document.getElementsByName获取单选框组。遍历单选框组，给每个单选框的checked设置为true.

​	按钮全不选思路

​	跟上面差不多，把checked改成false即可

​	按钮反选

​	跟上面差不多，把checked改成!checked	



​	

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    #table {
        border: 1px solid black;
        width: 500px;
        margin: auto;
    }

    td, th {
        border: 1px solid black;
    }

    .out {
        background-color: white;
    }

    .in {
        background-color: blue;
    }

    #buttonGroup {
        padding-top: 10px;
        padding-left: 360px;
    }
</style>
<script>
    window.onload = function () {
        var selectAll = document.getElementById("selectAll");
        selectAll.onclick = function () {
            var checkboxes = document.getElementsByName("checkbox");
            for (var i = 0; i < checkboxes.length; i++) {
                checkboxes[i].checked = true;
            }
        }

        var selectNotAll = document.getElementById("selectNotAll");
        selectNotAll.onclick = function () {
            var checkboxes = document.getElementsByName("checkbox");
            for (var i = 0; i < checkboxes.length; i++) {
                checkboxes[i].checked = false;
            }
        }

        var converseSelect = document.getElementById("converseSelect");
        converseSelect.onclick = function () {
            var checkboxes = document.getElementsByName("checkbox");
            for (var i = 0; i < checkboxes.length; i++) {
                checkboxes[i].checked = !checkboxes[i].checked;
            }
        }
        var firstcb = document.getElementById("firstcb");
        firstcb.onclick = function () {
            var checkboxes = document.getElementsByName("checkbox");
            for (var i = 0; i < checkboxes.length; i++) {
                checkboxes[i].checked = firstcb.checked;

            }
        }
        var trs = document.getElementsByTagName("tr");

        for (var i = 0; i < trs.length; i++) {
            trs[i].onmouseover = function () {
                this.className = "in";
            }
            trs[i].onmouseout = function () {
                this.className = "out";
            }
        }

    }
</script>
<body>
<table id="table">
    <caption>学生信息表</caption>
    <tr>
        <th><input type="checkbox" name="checkbox" id="firstcb"></th>
        <th>编号</th>
        <th>姓名</th>
        <th>性别</th>
        <th>操作</th>
    </tr>
    <tr>
        <td><input type="checkbox" name="checkbox"></td>
        <td>1</td>
        <td>令狐冲</td>
        <td>男</td>
        <td><a href="javascript:void(0)" onclick="delTr()">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox" name="checkbox"></td>
        <td>1</td>
        <td>令狐冲</td>
        <td>男</td>
        <td><a href="javascript:void(0)" onclick="delTr()">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox" name="checkbox"></td>
        <td>1</td>
        <td>令狐冲</td>
        <td>男</td>
        <td><a href="javascript:void(0)" onclick="delTr()">删除</a></td>
    </tr>
</table>
<div id="buttonGroup">
    <input type="button" id="selectAll" value="全选">
    <input type="button" id="selectNotAll" value="全不选">
    <input type="button" id="converseSelect" value="反选">
</div>
</body>
</html>
```

![image-20200306155326030](F:\Java集合\Java全面复习\img\image-20200306155326030.png)