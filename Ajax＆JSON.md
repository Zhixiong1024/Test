# AJAX

## JS实现方式code

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        //原生的JS实现方式（了解）
        //定义方法
        function  fun() {
            //发送异步请求
            //1.创建核心对象
            var xmlhttp;
            if (window.XMLHttpRequest)
            {// code for IE7+, Firefox, Chrome, Opera, Safari
                xmlhttp=new XMLHttpRequest();
            }
            else
            {// code for IE6, IE5
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

        }
        
    </script>
    
    
</head>
<body>

    <input type="button" value="发送异步请求" onclick="fun();">

    <input>
</body>
</html>
```



## $.ajax()



```javascript
/*
$.ajax()
			* 语法：$.ajax({键值对});
			 //使用$.ajax()发送异步请求
	            $.ajax({
	                url:"ajaxServlet1111" , // 请求路径
	                type:"POST" , //请求方式
	                //data: "username=jack&age=23",//请求参数
	                data:{"username":"jack","age":23},
	                success:function (data) {
	                    alert(data);
	                },//响应成功后的回调函数
	                error:function () {
	                    alert("出错啦...")
	                },//表示如果请求响应出现错误，会执行的回调函数
	
	                dataType:"text"//设置接受到的响应数据的格式
	            });
*/


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-3.3.1.min.js"></script>
    <script>
        
        //定义方法
        function  fun() {
            //使用$.ajax()发送异步请求

            $.ajax({
                url:"ajaxServlet1111" , // 请求路径
                type:"POST" , //请求方式
                //data: "username=jack&age=23",//请求参数
                data:{"username":"jack","age":23},
                success:function (data) {//data会接受服务器响应的值
                    alert(data);
                },//响应成功后的回调函数
                error:function () {
                    alert("出错啦...")
                },//表示如果请求响应出现错误，会执行的回调函数

                dataType:"text"//设置接受到的响应数据的格式
            });
        }
        
    </script>
    
    
</head>
<body>

    <input type="button" value="发送异步请求" onclick="fun();">

    <input>
</body>
</html>
```



##  $.get()

```javascript
/*
$.get()：发送get请求
			* 语法：$.get(url, [data], [callback], [type])
				* 参数：
					* url：请求路径
					* data：请求参数
					* callback：回调函数
					* type：响应结果的类型
*/

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-3.3.1.min.js"></script>
    <script>
        
        //定义方法
        function  fun() {

            $.get("ajaxServlet",{username:"rose"},function (data) {
                alert(data);
            },"text");

        }
        
    </script>
    
    
</head>
<body>

    <input type="button" value="发送异步请求" onclick="fun();">

    <input>
</body>
</html>
```



## $.post()

```javascript
/*
$.post()：发送post请求
			* 语法：$.get(url, [data], [callback], [type])
				* 参数：
					* url：请求路径
					* data：请求参数
					* callback：回调函数
					* type：响应结果的类型
*/

<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-3.3.1.min.js"></script>
    <script>
        
        //定义方法
        function  fun() {

            $.post("ajaxServlet",{username:"rose"},function (data) {
                alert(data);
            },"text");

        }
        
    </script>
    
    
</head>
<body>

    <input type="button" value="发送异步请求" onclick="fun();">

    <input>
</body>
</html>
```



# JSON

```
取值类型：
1. 数字（整数或浮点数）
2. 字符串（在双引号中）
3. 逻辑值（true 或 false）
4. 数组（在方括号中） {"persons":[{},{}]}
5. 对象（在花括号中） {"address":{"province"："陕西"....}}
6. null

获取数据:
1. json对象.键名
2. json对象["键名"]
3. 数组对象[索引]
4. 遍历

//1.定义基本格式
var person = {"name": "张三", age: 23, 'gender': true};

var ps = [
	{"name": "张三", "age": 23, "gender": true},
    {"name": "李四", "age": 24, "gender": true},
    {"name": "王五", "age": 25, "gender": false}
    ];
```



```javascript
	        //获取person对象中所有的键和值
	        //for in 循环
	        for(var key in person){
	            //这样的方式获取不行。因为相当于  person."name"
	            //alert(key + ":" + person.key);
	            alert(key+":"+person[key]);
	        }
	
	       //获取ps中的所有值
	        for (var i = 0; i < ps.length; i++) {
	            var p = ps[i];
	            for(var key in p){
	                alert(key+":"+p[key]);
	            }
	        }
```


## JSON数据转换

```
1. JSON转为Java对象
    1. 导入jackson的相关jar包
    2. 创建Jackson核心对象 ObjectMapper
    3. 调用ObjectMapper的相关方法进行转换
        1. readValue(json字符串数据,Class)
2. Java对象转换JSON
    1. 使用步骤：
        1. 导入jackson的相关jar包
        2. 创建Jackson核心对象 ObjectMapper
        3. 调用ObjectMapper的相关方法进行转换
            
1. 转换方法：
    * writeValue(参数1，obj):
        参数1：
            File：将obj对象转换为JSON字符串，并保存到指定的文件中
            Writer：将obj对象转换为JSON字符串，并将json数据填充到字符输出流中
            OutputStream：将obj对象转换为JSON字符串，并将json数据填充到字节输出流中
    * writeValueAsString(obj):将对象转为json字符串

2. 注解：
    1. @JsonIgnore：排除属性。
    2. @JsonFormat：属性值得格式化
        * @JsonFormat(pattern = "yyyy-MM-dd")

3. 复杂java对象转换
    1. List：数组
    2. Map：对象格式一致
```







# ...