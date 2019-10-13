## 选择器

### 基本选择器

```javascript
1. 标签选择器（元素选择器）
    * 语法： $("html标签名") 获得所有匹配标签名称的元素
2. id选择器 
    * 语法： $("#id的属性值") 获得与指定id属性值匹配的元素
    var div1 = $("#div1");
3. 类选择器
    * 语法： $(".class的属性值") 获得与指定的class属性值匹配的元素
4. 并集选择器：
    * 语法： $("选择器1,选择器2....") 获取多个选择器选中的所有元素
```

### 层级选择器

```javascript
1. 后代选择器
    * 语法： $("A B ") 选择A元素内部的所有B元素		
2. 子选择器
    * 语法： $("A > B") 选择A元素内部的所有B子元素
```

### 属性选择器

```javascript
1. 属性名称选择器 
    * 语法： $("A[属性名]") 包含指定属性的选择器
    $("div[title]").css("backgroundColor","pink");
2. 属性选择器
    * 语法： $("A[属性名='值']") 包含指定属性等于指定值的选择器                   $("div[title!='test']").css("backgroundColor","pink");
$("div[title^='te']").css("backgroundColor","pink");
$("div[title$='te']").css("backgroundColor","pink");
$("div[title*='te']").css("backgroundColor","pink");
/*
	*=表示包含 te 的值
	^=以te开头
	$=以te结尾
*/  
3. 复合属性选择器
    * 语法： $("A[属性名='值'][]...") 包含多个属性条件的选择器
    $("div[id][title*='es']").css("backgroundColor","pink");
```

### 过滤选择器

```javascript
1. 首元素选择器 
    * 语法： :first 获得选择的元素中的第一个元素     		$("div:first").css("backgroundColor","pink");
2. 尾元素选择器 
    * 语法： :last 获得选择的元素中的最后一个元素
    $("div:last").css("backgroundColor","pink");
3. 非元素选择器
    * 语法： :not(selector) 不包括指定内容的元素
   $("div:not(.one)").css("backgroundColor","pink");
4. 偶数选择器
    * 语法： :even 偶数，从 0 开始计数
    $("div:even").css("backgroundColor","pink");
5. 奇数选择器
    * 语法： :odd 奇数，从 0 开始计数
    $("div:odd").css("backgroundColor","pink");
6. 等于索引选择器
    * 语法： :eq(index) 指定索引元素
    $("div:eq(3)").css("backgroundColor","pink");
7. 大于索引选择器 
    * 语法： :gt(index) 大于指定索引元素
    $("div:gt(3)").css("backgroundColor","pink");
8. 小于索引选择器 
    * 语法： :lt(index) 小于指定索引元素
    $("div:lt(3)").css("backgroundColor","pink");
9. 标题选择器
    * 语法： :header 获得标题（h1~h6）元素，固定写法
    $(":header").css("backgroundColor","pink");
```

### 表单过滤选择器

```javascript
1. 可用元素选择器 
    * 语法： :enabled 获得可用元素
    $("input[type='text']:enabled").val("aaa");
	//val 可以改变值
2. 不可用元素选择器 
    * 语法： :disabled 获得不可用元素
    $("input[type='text']:disabled").val("aaa");
3. 选中选择器 
    * 语法： :checked 获得单选/复选框选中的元素
	alert($("input[type='checkbox']:checked").length);
4. 选中选择器 
    * 语法： :selected 获得下拉框选中的元素
    alert($("#job > option:selected").length);
```

## DOM操作

### 内容操作

```javascript
1. html(): 获取/设置元素的标签体内容   
    var html = $("#mydiv").html();

2. text(): 获取/设置元素的标签体纯文本内容   
   	$("#mydiv").text("bbb");

3. val()： 获取/设置元素的value属性值
    var value = $("#myinput").val();
    $("#myinput").val("李四");
```

### 属性操作

```javascript
1. 通用属性操作
    1. attr(): 获取/设置元素的属性
	var name = $("#bj").attr("name");
    $("#bj").attr("name","dabeijing");
    2. removeAttr():删除属性
    $("#bj").removeAttr("name");
    3. prop():获取/设置元素的属性
    var checked = $("#hobby").prop("checked");
    4. removeProp():删除属性

    * attr和prop区别？
        1. 元素的固有属性，则建议使用prop
        2. 元素自定义的属性，则建议使用attr
        
2. 对class属性操作
    1. addClass():添加class属性值
    $("#one").addClass("second");

    2. removeClass():删除class属性值
    $("#one").removeClass("second");

    // 判断如果元素对象上存在class="one"，则将属性值one删除掉。  如果元素对象上不存在class="one"，则添加
    3. toggleClass():切换class属性
    $("#one").toggleClass("second");
   
    4. css():
    var backgroundColor = $("#one").css("backgroundColor");
    $("#one").css("backgroundColor","green");
```

## CRUD操作

```javascript
1. append():父元素将子元素追加到末尾
    * 对象1.append(对象2): 将对象2添加到对象1元素内部，并且在末尾
	//将反恐放置到city的最前面
    $("#city").append($("#fk"));
    $("#fk").appendTo($("#city"));
2. prepend():父元素将子元素追加到开头
    * 对象1.prepend(对象2):将对象2添加到对象1元素内部，并且在开头
	$("#city").prepend($("#fk"));

3. appendTo():
    * 对象1.appendTo(对象2):将对象1添加到对象2内部，并且在末尾
    $("#fk").appendTo($("#city"));
4. prependTo()：
    * 对象1.prependTo(对象2):将对象1添加到对象2内部，并且在开头
	$("#fk").prependTo($("#city"));

5. after():添加元素到元素后边
    * 对象1.after(对象2)： 将对象2添加到对象1后边。对象1和对象2是兄弟关系
	//将反恐插入到天津后面
	$("#tj").after($("#fk"));
	$("#fk").insertAfter($("#tj"));
6. before():添加元素到元素前边
    * 对象1.before(对象2)： 将对象2添加到对象1前边。对象1和对象2是兄弟关系

    //将反恐插入到天津前面
	$("#tj").before($("#fk"));
	$("#fk").insertBefore($("#tj"));

7. insertAfter()
    * 对象1.insertAfter(对象2)：将对象2添加到对象1后边。对象1和对象2是兄弟关系
8. insertBefore()
    * 对象1.insertBefore(对象2)： 将对象2添加到对象1前边。对象1和对象2是兄弟关系

9. remove():移除元素
    * 对象.remove():将对象删除掉
    $("#bj").remove();
10. empty():清空元素的所有后代元素。
    * 对象.empty():将对象的后代元素全部清空，但是保留当前对象以及其属性节点
	$("#city").empty();
```

### JQuery对象和JS对象转换


```javascript
1. JQuery对象在操作时，更加方便。
2. JQuery对象和js对象方法不通用的.
3. 两者相互转换
    * jq -- > js : jq对象[索引] 或者 jq对象.get(索引)
    $divs[0].innerHTML = "ddd";
    $divs.get(1).innerHTML = "eee";
    * js -- > jq : $(js对象)
    
```