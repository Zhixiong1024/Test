# 动画

## 默认动画

```
show([speed,[easing],[fn])
hide([speed,[easing],[fn])
toggle([speed],[easing],[fn])

1. speed：动画的速度。三个预定义的值("slow","normal", "fast")或表示动画时长的毫秒数值(如：1000)

2. easing：切换效果，默认是"swing"，可用参数"linear"
	* swing：动画执行时效果是 先慢，中间快，最后又慢
	* linear：动画执行时速度是匀速的

3. fn：在动画完成时执行的函数，每个元素执行一次。
```

## 滑动显示和隐藏方式

```
slideDown([speed],[easing],[fn])
slideUp([speed,[easing],[fn])
slideToggle([speed],[easing],[fn])
```

## 淡入淡出显示和隐藏方式

```
fadeIn([speed],[easing],[fn])
fadeOut([speed],[easing],[fn])
fadeToggle([speed,[easing],[fn])
```

## code

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    <script type="text/javascript" src="../js/jquery-3.3.1.min.js"></script>


    <script>
        //隐藏div
        function hideFn(){
            $("#showDiv").hide("slow","swing",function(){
                alert("隐藏了...")
            });

           //默认方式
            $("#showDiv").hide(5000,"swing");

            //滑动方式
            $("#showDiv").slideUp("slow");


            //淡入淡出方式
            $("#showDiv").fadeOut("slow");
        }

        //显示div
        function showFn(){
            $("#showDiv").show("slow","swing",function(){
                alert("显示了...")
            });

            //默认方式
            $("#showDiv").show(5000,"linear");
            //滑动方式
            $("#showDiv").slideDown("slow");

            //淡入淡出方式
            $("#showDiv").fadeIn("slow");
        }


        //切换显示和隐藏div
        function toggleFn(){

            //默认方式
            $("#showDiv").toggle("slow");
            //滑动方式
            $("#showDiv").slideToggle("slow");

            //淡入淡出方式
            $("#showDiv").fadeToggle("slow");
        }




    </script>

</head>
<body>
<input type="button" value="点击按钮隐藏div" onclick="hideFn()">
<input type="button" value="点击按钮显示div" onclick="showFn()">
<input type="button" value="点击按钮切换div显示和隐藏" onclick="toggleFn()">

<div id="showDiv" style="width:300px;height:300px;background:pink">
    div显示和隐藏
</div>
</body>
</html>

```



# 遍历

```
 jq的遍历方式
    1. jq对象.each(callback)
        jquery对象.each(function(index,element){});
            * index:就是元素在集合中的索引
            * element：就是集合中的每一个元素对象
            * this：集合中的每一个元素对象
        回调函数返回值：
            * true:如果当前function返回为false，则结束循环(break)。
            * false:如果当前function返回为true，则(continue)
    2. $.each(object, [callback])
    3. for..of: jquery 3.0 版本之后提供的方式
        for(元素对象 of 容器对象)
```

## jq对象.each

```javascript
citys.each(function (index,element) {
    //3.1 获取li对象 第一种方式 this
    alert(this.innerHTML);
    alert($(this).html());
    //3.2 获取li对象 第二种方式 在回调函数中定义参数   index（索引） element（元素对象）
    alert(index+":"+element.innerHTML);
    alert(index+":"+$(element).html());

    //判断如果是上海，则结束循环
    if("上海" == $(element).html()){
        //如果当前function返回为false，则结束循环(break)。
        //如果返回为true，则结束本次循环，继续下次循环(continue)
        return true;
    }
    alert(index+":"+$(element).html());
});
```

## $.each(object, [callback])

```javascript
//$.each(object, [callback])
$.each(citys,function () {
                    alert($(this).html());
                });
```

## for..of

```javascript
//for..of
for(li of citys){
                    alert($(li).html());
                }
```

# 事件绑定

## jq对象.事件方法(回调函数)

```javascript
注：如果调用事件方法，不传递回调函数，则会触发浏览器默认行为。
			* 表单对象.submit();//让表单提交
			
$("#name").click(function () {
               alert("我被点击了...")
})

//给name绑定鼠标移动到元素之上事件。绑定鼠标移出事件
$("#name").mouseover(function () {
   alert("鼠标来了...")
});

$("#name").mouseout(function () {
    alert("鼠标走了...")
});
```

## on绑定事件off解除绑定

```javascript
//1.使用on给按钮绑定单击事件  click
$("#btn").on("click",function () {
   alert("我被点击了。。。")
}) ;

//2. 使用off解除btn按钮的单击事件
$("#btn2").click(function () {
    //解除btn按钮的单击事件
    $("#btn").off("click");
    //$("#btn").off();将组件上的所有事件全部解绑
});
```

## jq对象.toggle(fn1,fn2...)

```javascript
* 当单击jq对象对应的组件后，会执行fn1.第二次点击会执行fn2.....
注意：1.9版本 .toggle() 方法删除,jQuery Migrate（迁移）插件可以恢复此功能。
$("#btn").toggle(function () {
   //改变div背景色backgroundColor 颜色为 green
   $("#myDiv").css("backgroundColor","green");
},function () {
   //改变div背景色backgroundColor 颜色为 pink
   $("#myDiv").css("backgroundColor","pink");
});
```

# 插件

##  $.fn.extend(object) 

```javascript
//增强通过Jquery获取的对象的功能  $("#id")
$.fn.extend({
    //定义了一个check()方法。所有的jq对象都可以调用该方法
    check:function () {
       //让复选框选中

        //this:调用该方法的jq对象
        this.prop("checked",true);
    },
    uncheck:function () {
        //让复选框不选中

        this.prop("checked",false);
    }

});

$(function () {
   // 获取按钮
    //$("#btn-check").check();
    //复选框对象.check();

    $("#btn-check").click(function () {
        //获取复选框对象
        $("input[type='checkbox']").check();

    });

    $("#btn-uncheck").click(function () {
        //获取复选框对象
        $("input[type='checkbox']").uncheck();

    });
});
```

## $.extend(object)

```javascript
$.extend({
    max:function (a,b) {
        //返回两数中的较大值
        return a >= b ? a:b;
    },
    min:function (a,b) {
        //返回两数中的较小值
        return a <= b ? a:b;
    }


});

//调用全局方法
var max = $.max(4,3);
alert(max);

var min = $.min(1,2);
alert(min);
```

