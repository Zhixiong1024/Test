### 数据类型

```
基本数据类型
    number：数字。 整数/小数/NaN(not a number 一个不是数字的数字类型)
    string：字符串。 字符串  "abc" "a" 'abc'
    boolean: true和false
    null：一个对象为空的占位符
    undefined：未定义。如果一个变量没有给初始化值，则会被默认赋值为undefined
引用数据类型：对象变量
```

### 运算符

```
typeof运算符：获取变量的类型。
typeof(a)
注：null运算后得到的是object运算符(+)

string转number：+"123"转为123,+"123abc"转为NaN
boolean转number：+true转为1，+false转为0
```

### 比较运算符

```
"abc"<"acc" 结果true  逐一比较,直到分出大小
"123"==123 结果true  字符串转为number,在比较
"123"===123 结果true  先判断类型是否一样,在比较
```

### 逻辑运算符

```
其他类型转boolean：
    number：0或NaN为假，其他为真
    string：除了空字符串("")，其他都是true
    null&undefined:都是false
    对象：所有对象都为true
```

### Function函数对象

```
function 方法名称(形式参数列表){
    方法体
}

var 方法名 = function(形式参数列表){
    方法体
}

方法名.length:代表形参的个数

特点：
方法定义是，形参的类型不用写,返回值类型也不写。
在JS中，方法的调用只与方法的名称有关，和参数列表无关
在方法中有一个隐藏的内置对象arguments,封装所有的实际参数arguments[i]

```

### 	Array:数组对象

```js
1. 创建：
    1. var arr = new Array(元素列表);
    2. var arr = new Array(默认长度);
    3. var arr = [元素列表];
2. 方法
    //join(参数):将数组中的元素按照指定的分隔符拼接为字符串
	//打印数组默认使用arr.join(",")
    document.write(arr.join("--")+"<br>");
    //push()	向数组的末尾添加一个或更多元素，并返回新的长度。
	arr.push(11);

3. 属性
    length:数组的长度
    alert(arr.length);
4. 特点：
    1. JS中，数组元素的类型可变的。
    2. JS中，数组长度可变的。
```

### Date：日期对象

```javascript
1. 创建：
    var date = new Date();

2. 方法：
    //toLocaleString()：返回当前date对象对应的时间本地字符串格式
	document.write(date.toLocaleString()+"<br>");
    //getTime():获取毫秒值。返回当前如期对象描述的时间到1970年1月1日零点
```

### Math：数学对象

```javascript
1. 创建：
    * 特点：Math对象不用创建，直接使用。  Math.方法名();
2. 方法：
    //random():返回 0 ~ 1 之间的随机数。 含0不含1
	document.writh(Math.random()+"<br>");
    ceil(x)：对数进行上舍入。
    floor(x)：对数进行下舍入。
    round(x)：把数四舍五入为最接近的整数。
3. 属性：
    PI
```

### RegExp：正则表达式对象

```javascript
1. 正则表达式：定义字符串的组成规则。
    1. 单个字符:[]
        * 特殊符号代表特殊含义的单个字符:
            \d:单个数字字符 [0-9]
            \w:单个单词字符[a-zA-Z0-9_]
    2. 量词符号：
        ?：表示出现0次或1次
        *：表示出现0次或多次
      	
        +：出现1次或多次
        {m,n}:表示 m<= 数量 <= n
            * m如果缺省： {,n}:最多n次
            * n如果缺省：{m,} 最少m次
    3. 开始结束符号
        * ^:开始
        * $:结束
  
2. 正则对象：
    1. 创建
        1. var reg = new RegExp("^\\w{6,12}$");
        2. var reg = /正则表达式/;
        var reg2 = /^\w{6,12}$/;//以单词开头和结尾,最少6个最多12个

    2. 方法	
        1. test(参数):验证指定的字符串是否符合正则定义的规范	
        var flag = reg2.test(username);
```

### Global

```javascript

1. 特点：全局对象，这个Global中封装的方法不需要对象就可以直接调用。  方 法名();
2. 方法：
    //encodeURI():url编码
    var str = "http://www.baidu.com?wd=传智播客";
    var encode = encodeURI(str);
    //decodeURI():url解码

    encodeURIComponent():url编码,':' '/' '/'  这些字符也编码
    decodeURIComponent():url解码

   // parseInt():将字符串转为数字
        * 逐一判断每一个字符是否是数字，直到不是数字为止，将前边数字部分转为number
		var str = "123abc";
		var num = parseInt(str);//num=123

		var str = "a123abc";
		var num = parseInt(str);//num=NaN

    //isNaN():判断一个值是否是NaN
        * NaN六亲不认，连自己都不认。NaN参与的==比较全部问false
		var flag = isNan(num);

    //eval():讲 JavaScript 字符串，并把它作为脚本代码来执行。
		var jscode = "alert(123)";
		eval(jscode);
```





​	