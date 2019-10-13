### Propertise

```java
//类加载资源/配置文件
Propertise pro =new Propertise();
ClassLoader cla = 当前类名.class.getClassLoader();
InputStream is = cla.getResourceAsStream("配置文件名");
pro.load(is);
//得到参数值
String value = pro.getProperty("参数名");
//加载进内存
Class cls = Class.froName("全类名");
```

### JDBCUtils

```java
public class JDBCUtils {
    //1.定义成员变量 DataSource
    private static DataSource ds ;
    static{
        try {
            //1.加载配置文件
            Properties pro = new Properties();           pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            //2.获取DataSource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //获取连接
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }
    //获取连接池方法	
    public static DataSource getDataSource(){
        return  ds;
    }
    //释放资源,重载方法
    public static void close(Statement stmt,Connection conn){
       close(null,stmt,conn);
    }
    //释放资源
     public static void close(ResultSet rs , Statement stmt, Connection conn){
         if(rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
                if(stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(conn != null){
            try {
                conn.close();//归还连接
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }		
}
```

### Spring JDBC

```java
//导包:drudi一个包,spring jdbc 5个包,连接mysql一个包
//1. 获取JDBCTemplate对象
private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
/**
 * 1. 修改1号数据的 salary 为 10000
 */
@Test
public void test1(){

    //2. 定义sql
    String sql = "update emp set salary = 10000 where id = 1001";
    //3. 执行sql
    int count = template.update(sql);
    System.out.println(count);
}

/**
 * 2. 添加一条记录
 */
@Test
public void test2(){
    String sql = "insert into emp(id,ename,dept_id) values(?,?,?)";
    int count = template.update(sql, 1015, "郭靖", 10);
    System.out.println(count);

}

/**
 * 3.删除刚才添加的记录
 */
@Test
public void test3(){
    String sql = "delete from emp where id = ?";
    int count = template.update(sql, 1015);
    System.out.println(count);
}

/**
 * 4.查询id为1001的记录，将其封装为Map集合
 * 注意：这个方法查询的结果集长度只能是1
 */
@Test
public void test4(){
    String sql = "select * from emp where id = ? or id = ?";
    Map<String, Object> map = template.queryForMap(sql, 1001,1002);
    System.out.println(map);
    //{id=1001, ename=孙悟空, job_id=4, mgr=1004, joindate=2000-12-17, salary=10000.00, bonus=null, dept_id=20}

}

/**
 * 5. 查询所有记录，将其封装为List
 */
@Test
public void test5(){
    String sql = "select * from emp";
    List<Map<String, Object>> list = template.queryForList(sql);

    for (Map<String, Object> stringObjectMap : list) {
        System.out.println(stringObjectMap);
    }
}

/**
 * 6. 查询所有记录，将其封装为Emp对象的List集合
 */

@Test
public void test6(){
    String sql = "select * from emp";
    List<Emp> list = template.query(sql, new RowMapper<Emp>() {

        @Override
        public Emp mapRow(ResultSet rs, int i) throws SQLException {
            Emp emp = new Emp();
            int id = rs.getInt("id");
            String ename = rs.getString("ename");
            int job_id = rs.getInt("job_id");
            int mgr = rs.getInt("mgr");
            Date joindate = rs.getDate("joindate");
            double salary = rs.getDouble("salary");
            double bonus = rs.getDouble("bonus");
            int dept_id = rs.getInt("dept_id");

            emp.setId(id);
            emp.setEname(ename);
            emp.setJob_id(job_id);
            emp.setMgr(mgr);
            emp.setJoindate(joindate);
            emp.setSalary(salary);
            emp.setBonus(bonus);
            emp.setDept_id(dept_id);

            return emp;
        }
    });
        for (Emp emp : list) {
        System.out.println(emp);
    }
}

/**
 * 6. 查询所有记录，将其封装为Emp对象的List集合
 */

@Test
public void test6_2(){
    String sql = "select * from emp";
    List<Emp> list = template.query(sql, new BeanPropertyRowMapper<Emp>(Emp.class));
    for (Emp emp : list) {
        System.out.println(emp);
    }
}

/**
 * 7. 查询总记录数
 */

@Test
public void test7(){
    String sql = "select count(id) from emp";
    Long total = template.queryForObject(sql, Long.class);
    System.out.println(total);
}

```

### JS隔行换色

```javascript
<script>
    //需求：将数据行的奇数行背景色设置为 pink，偶数行背景色设置为 yellow
    $(function () {
        //1. 获取数据行的奇数行的tr，设置背景色为pink
        $("tr:gt(1):odd").css("backgroundColor","pink");
        //2. 获取数据行的偶数行的tr,设置背景色为yellow
     $("tr:gt(1):even").css("backgroundColor","yellow");
    });
</script>
```

### JS全选全不选

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title></title>
<script  src="../../js/jquery-3.3.1.min.js"></script>
<script>
//分析：需要保证下边的选中状态和第一个复选框的选中状态一致即可

function selectAll(obj){
    //获取下边的复选框
    $(".itemSelect").prop("checked",obj.checked);
}

</script>
</head>
<body>
<table id="tab1" border="1" width="800" align="center" >
<tr>
    <td colspan="5"><input type="button" value="删除"></td>
</tr>
<tr>
    <th><input type="checkbox" onclick="selectAll(this)" ></th>
    <th>分类ID</th>
    <th>分类名称</th>
    <th>分类描述</th>
    <th>操作</th>
</tr>
<tr>
    <td><input type="checkbox" class="itemSelect"></td>
    <td>1</td>
    <td>手机数码</td>
    <td>手机数码类商品</td>
    <td><a href="">修改</a>|<a href="">删除</a></td>
</tr>
<tr>
    <td><input type="checkbox" class="itemSelect"></td>
    <td>2</td>
    <td>电脑办公</td>
    <td>电脑办公类商品</td>
    <td><a href="">修改</a>|<a href="">删除</a></td>
</tr>
<tr>
    <td><input type="checkbox" class="itemSelect"></td>
    <td>3</td>
    <td>鞋靴箱包</td>
    <td>鞋靴箱包类商品</td>
    <td><a href="">修改</a>|<a href="">删除</a></td>
</tr>
<tr>
    <td><input type="checkbox" class="itemSelect"></td>
    <td>4</td>
    <td>家居饰品</td>
    <td>家居饰品类商品</td>
    <td><a href="">修改</a>|<a href="">删除</a></td>
</tr>
</table>
</body>
</html>
```

### JSqq表情选择

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>QQ表情选择</title>
<script  src="../../js/jquery-3.3.1.min.js"></script>
<style type="text/css">
*{margin: 0;padding: 0;list-style: none;}

.emoji{margin:50px;}
ul{overflow: hidden;}
li{float: left;width: 48px;height: 48px;cursor: pointer;}
.emoji img{ cursor: pointer; }
</style>
<script>
//需求：点击qq表情，将其追加到发言框中
$(function () {
//1.给img图片添加onclick事件
$("ul img").click(function(){
    //2.追加到p标签中即可。
    $(".word").append($(this).clone());
});

});


</script>

</head>
<body>
<div class="emoji">
<ul>
<li><img src="img/01.gif" height="22" width="22" alt="" /></li>
<li><img src="img/02.gif" height="22" width="22" alt="" /></li>
<li><img src="img/03.gif" height="22" width="22" alt="" /></li>
<li><img src="img/04.gif" height="22" width="22" alt="" /></li>
<li><img src="img/05.gif" height="22" width="22" alt="" /></li>
<li><img src="img/06.gif" height="22" width="22" alt="" /></li>
<li><img src="img/07.gif" height="22" width="22" alt="" /></li>
<li><img src="img/08.gif" height="22" width="22" alt="" /></li>
<li><img src="img/09.gif" height="22" width="22" alt="" /></li>
<li><img src="img/10.gif" height="22" width="22" alt="" /></li>
<li><img src="img/11.gif" height="22" width="22" alt="" /></li>
<li><img src="img/12.gif" height="22" width="22" alt="" /></li>
</ul>
<p class="word">
<strong>请发言：</strong>
<img src="img/12.gif" height="22" width="22" alt="" />
</p>
</div>
</body>
</html>
```

### JS左右移动

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title></title>
<script  src="../../js/jquery-3.3.1.min.js"></script>


<style>
    #leftName , #btn,#rightName{
        float: left;
        width: 100px;
        height: 300px;
    }
    #toRight,#toLeft{
        margin-top:100px ;
        margin-left:30px;
        width: 50px;
    }

    .border{
        height: 500px;
        padding: 100px;
    }
</style>

<script>

    //需求：实现下拉列表选中条目左右选择功能

    $(function () {
        //toRight
        $("#toRight").click(function () {
            //获取右边的下拉列表对象，append(左边下拉列表选中的option)
            $("#rightName").append($("#leftName > option:selected"));
        });

        //toLeft
        $("#toLeft").click(function () {
            //appendTo   获取右边选中的option，将其移动到左边下拉列表中
            $("#rightName > option:selected").appendTo($("#leftName"));

        });
    });


</script>



</head>
<body>
<div class="border">
    <select id="leftName" multiple="multiple">
        <option>张三</option>
        <option>李四</option>
        <option>王五</option>
        <option>赵六</option>
    </select>
    <div id="btn">
        <input type="button" id="toRight" value="-->"><br>
        <input type="button" id="toLeft" value="<--">

    </div>

    <select id="rightName" multiple="multiple">
        <option>钱七</option>
    </select>

</div>


</body>
</html>

```



### JS广告显示与隐藏

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>广告的自动显示与隐藏</title>
    <style>
        #content{width:100%;height:500px;background:#999}
    </style>

    <!--引入jquery-->
    <script type="text/javascript" src="../js/jquery-3.3.1.min.js"></script>
    <script>
        /*
            需求：
                1. 当页面加载完，3秒后。自动显示广告
                2. 广告显示5秒后，自动消失。

            分析：
                1. 使用定时器来完成。setTimeout (执行一次定时器)
                2. 分析发现JQuery的显示和隐藏动画效果其实就是控制display
                3. 使用  show/hide方法来完成广告的显示
         */

        //入口函数，在页面加载完成之后，定义定时器，调用这两个方法
        $(function () {
           //定义定时器，调用adShow方法 3秒后执行一次
           setTimeout(adShow,3000);
           //定义定时器，调用adHide方法，8秒后执行一次
            setTimeout(adHide,8000);
        });
        //显示广告
        function adShow() {
            //获取广告div，调用显示方法
            $("#ad").show("slow");
        }
        //隐藏广告
        function adHide() {
            //获取广告div，调用隐藏方法
            $("#ad").hide("slow");
        }



    </script>
</head>
<body>
<!-- 整体的DIV -->
<div>
    <!-- 广告DIV -->
    <div id="ad" style="display: none;">
        <img style="width:100%" src="../img/adv.jpg" />
    </div>

    <!-- 下方正文部分 -->
    <div id="content">
        正文部分
    </div>
</div>
</body>
</html>
```



### JS抽奖

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>jquery案例之抽奖</title>
    <script type="text/javascript" src="../js/jquery-3.3.1.min.js"></script>

    <script language='javascript' type='text/javascript'>

        /*
            分析：
                1. 给开始按钮绑定单击事件
                    1.1 定义循环定时器
                    1.2 切换小相框的src属性
                        * 定义数组，存放图片资源路径
                        * 生成随机数。数组索引


                2. 给结束按钮绑定单击事件
                    1.1 停止定时器
                    1.2 给大相框设置src属性

         */
        var imgs = ["../img/man00.jpg",
                    "../img/man01.jpg",
                    "../img/man02.jpg",
                    "../img/man03.jpg",
                    "../img/man04.jpg",
                    "../img/man05.jpg",
                    "../img/man06.jpg",
                    ];
        var startId;//开始定时器的id
        var index;//随机角标
        $(function () {
            //处理按钮是否可以使用的效果
            $("#startID").prop("disabled",false);
            $("#stopID").prop("disabled",true);


           //1. 给开始按钮绑定单击事件
            $("#startID").click(function () {
                // 1.1 定义循环定时器 20毫秒执行一次
                startId = setInterval(function () {
                    //处理按钮是否可以使用的效果
                    $("#startID").prop("disabled",true);
                    $("#stopID").prop("disabled",false);


                    //1.2生成随机角标 0-6
                    index = Math.floor(Math.random() * 7);//0.000--0.999 --> * 7 --> 0.0-----6.9999
                    //1.3设置小相框的src属性
                    $("#img1ID").prop("src",imgs[index]);

                },20);
            });


            //2. 给结束按钮绑定单击事件
            $("#stopID").click(function () {
                //处理按钮是否可以使用的效果
                $("#startID").prop("disabled",false);
                $("#stopID").prop("disabled",true);


               // 1.1 停止定时器
                clearInterval(startId);
               // 1.2 给大相框设置src属性
                $("#img2ID").prop("src",imgs[index]).hide();
                //显示1秒之后
                $("#img2ID").show(1000);
            });
        });




    </script>

</head>
<body>

<!-- 小像框 -->
<div style="border-style:dotted;width:160px;height:100px">
    <img id="img1ID" src="../img/man00.jpg" style="width:160px;height:100px"/>
</div>

<!-- 大像框 -->
<div
        style="border-style:double;width:800px;height:500px;position:absolute;left:500px;top:10px">
    <img id="img2ID" src="../img/man00.jpg" width="800px" height="500px"/>
</div>

<!-- 开始按钮 -->
<input
        id="startID"
        type="button"
        value="点击开始"
        style="width:150px;height:150px;font-size:22px">

<!-- 停止按钮 -->
<input
        id="stopID"
        type="button"
        value="点击停止"
        style="width:150px;height:150px;font-size:22px">


</body>
</html>
```



### JQ全选全不选

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>01-jQuery对象进行方法扩展</title>
    <script src="../js/jquery-3.3.1.min.js" type="text/javascript" charset="utf-8"></script>
    <script type="text/javascript">
       //使用jquery插件 给jq对象添加2个方法 check()选中所有复选框，uncheck()取消选中所有复选框
        
        
        //1.定义jqeury的对象插件
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


    </script>
</head>
<body>
<input id="btn-check" type="button" value="点击选中复选框" onclick="checkFn()">
<input id="btn-uncheck" type="button" value="点击取消复选框选中" onclick="uncheckFn()">
<br/>
<input type="checkbox" value="football">足球
<input type="checkbox" value="basketball">篮球
<input type="checkbox" value="volleyball">排球

</body>
</html>


```



### Jedis连接池工具类

```java
package cn.itcast.jedis.util;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

/**
 JedisPool工具类
    加载配置文件，配置连接池的参数
    提供获取连接的方法

 */
public class JedisPoolUtils {

    private static JedisPool jedisPool;

    static{
        //读取配置文件
        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        //创建Properties对象
        Properties pro = new Properties();
        //关联文件
        try {
            pro.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
        //获取数据，设置到JedisPoolConfig中
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));

        //初始化JedisPool
        jedisPool = new JedisPool(config,pro.getProperty("host"),Integer.parseInt(pro.getProperty("port")));



    }


    /**
     * 获取连接方法
     */
    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}

```



### 动态代理(设计模式)

```java
package cn.itcast.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyTest {

    public static void main(String[] args) {
        //1.创建真实对象
        Lenovo lenovo = new Lenovo();
        
        //2.动态代理增强lenovo对象
        /*
            三个参数：
                1. 类加载器：真实对象.getClass().getClassLoader()
                2. 接口数组：真实对象.getClass().getInterfaces()
                3. 处理器：new InvocationHandler()
         */
        SaleComputer proxy_lenovo = (SaleComputer) Proxy.newProxyInstance(lenovo.getClass().getClassLoader(), lenovo.getClass().getInterfaces(), new InvocationHandler() {

            /*
                代理逻辑编写的方法：代理对象调用的所有方法都会触发该方法执行
                    参数：
                        1. proxy:代理对象
                        2. method：代理对象调用的方法，被封装为的对象
                        3. args:代理对象调用的方法时，传递的实际参数
             */
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                /*System.out.println("该方法执行了....");
                System.out.println(method.getName());
                System.out.println(args[0]);




*/
                //判断是否是sale方法
                if(method.getName().equals("sale")){
                    //1.增强参数
                    double money = (double) args[0];
                    money = money * 0.85;
                    System.out.println("专车接你....");
                    //使用真实对象调用该方法
                    String obj = (String) method.invoke(lenovo, money);
                    System.out.println("免费送货...");
                    //2.增强返回值
                    return obj+"_鼠标垫";
                }else{
                    Object obj = method.invoke(lenovo, args);
                    return obj;
                }



            }
        });

        //3.调用方法

       /* String computer = proxy_lenovo.sale(8000);
        System.out.println(computer);*/

        proxy_lenovo.show();
    }
}

```

### 敏感词过滤

```java
package cn.itcast.web.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.ArrayList;
import java.util.List;

/**
 * 敏感词汇过滤器
 接收到浏览器发来请求,通关过滤器拦截请求,判断请求是否包含敏感词,然后返回正确的字符串.
 		String name = request.getParameter("name");
        String msg = request.getParameter("msg");
 */
@WebFilter("/*")
public class SensitiveWordsFilter implements Filter {


    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        //1.创建代理对象，增强getParameter方法

        ServletRequest proxy_req = (ServletRequest) Proxy.newProxyInstance(req.getClass().getClassLoader(), req.getClass().getInterfaces(), new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                //增强getParameter方法
                //判断是否是getParameter方法
                if(method.getName().equals("getParameter")){
                    //增强返回值
                    //获取返回值
                    String value = (String) method.invoke(req,args);
                    if(value != null){
                        for (String str : list) {
                            if(value.contains(str)){
                                value = value.replaceAll(str,"***");
                            }
                        }
                    }
                    
                    return  value;
                }


                return method.invoke(req,args);
            }
        });

        //2.放行
        chain.doFilter(proxy_req, resp);
    }
    private List<String> list = new ArrayList<String>();//敏感词汇集合
    public void init(FilterConfig config) throws ServletException {

        try{
            //1.获取文件真实路径
            ServletContext servletContext = config.getServletContext();
            String realPath = servletContext.getRealPath("/WEB-INF/classes/敏感词汇.txt");
            //2.读取文件
            BufferedReader br = new BufferedReader(new FileReader(realPath));
            //3.将文件的每一行数据添加到list中
            String line = null;
            while((line = br.readLine())!=null){
                list.add(line);
            }

            br.close();

            System.out.println(list);

        }catch (Exception e){
            e.printStackTrace();
        }

    }

    public void destroy() {
    }

}

```



### 登陆验证Fileter

```java
package cn.itcast.web.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

/**
 * 登录验证的过滤器
 */
@WebFilter("/*")
public class LoginFilter implements Filter {


    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        System.out.println(req);
        //0.强制转换
        HttpServletRequest request = (HttpServletRequest) req;

        //1.获取资源请求路径
        String uri = request.getRequestURI();
        //2.判断是否包含登录相关资源路径,要注意排除掉 css/js/图片/验证码等资源
        if(uri.contains("/login.jsp") || uri.contains("/loginServlet") || uri.contains("/css/") || uri.contains("/js/") || uri.contains("/fonts/") || uri.contains("/checkCodeServlet")  ){
            //包含，用户就是想登录。放行
            chain.doFilter(req, resp);
        }else{
            //不包含，需要验证用户是否登录
            //3.从获取session中获取user
            Object user = request.getSession().getAttribute("user");
            if(user != null){
                //登录了。放行
                chain.doFilter(req, resp);
            }else{
                //没有登录。跳转登录页面

                request.setAttribute("login_msg","您尚未登录，请登录");
                request.getRequestDispatcher("/login.jsp").forward(request,resp);
            }
        }


        // chain.doFilter(req, resp);
    }

    public void init(FilterConfig config) throws ServletException {

    }

    public void destroy() {
    }

}

```



### 验证码

```java
package cn.itcast.web.servlet;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

/**
 * 验证码
 */
@WebServlet("/checkCodeServlet")
public class CheckCodeServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)throws ServletException, IOException {
		
		//服务器通知浏览器不要缓存
		response.setHeader("pragma","no-cache");
		response.setHeader("cache-control","no-cache");
		response.setHeader("expires","0");
		
		//在内存中创建一个长80，宽30的图片，默认黑色背景
		//参数一：长
		//参数二：宽
		//参数三：颜色
		int width = 80;
		int height = 30;
		BufferedImage image = new BufferedImage(width,height,BufferedImage.TYPE_INT_RGB);
		
		//获取画笔
		Graphics g = image.getGraphics();
		//设置画笔颜色为灰色
		g.setColor(Color.GRAY);
		//填充图片
		g.fillRect(0,0, width,height);
		
		//产生4个随机验证码，12Ey
		String checkCode = getCheckCode();
		//将验证码放入HttpSession中
		request.getSession().setAttribute("CHECKCODE_SERVER",checkCode);
		
		//设置画笔颜色为黄色
		g.setColor(Color.YELLOW);
		//设置字体的小大
		g.setFont(new Font("黑体",Font.BOLD,24));
		//向图片上写入验证码
		g.drawString(checkCode,15,25);
		
		//将内存中的图片输出到浏览器
		//参数一：图片对象
		//参数二：图片的格式，如PNG,JPG,GIF
		//参数三：图片输出到哪里去
		ImageIO.write(image,"PNG",response.getOutputStream());
	}
	/**
	 * 产生4位随机字符串 
	 */
	private String getCheckCode() {
		String base = "0123456789ABCDEFGabcdefg";
		int size = base.length();
		Random r = new Random();
		StringBuffer sb = new StringBuffer();
		for(int i=1;i<=4;i++){
			//产生0到size-1的随机值
			int index = r.nextInt(size);
			//在base字符串中获取下标为index的字符
			char c = base.charAt(index);
			//将c放入到StringBuffer中去
			sb.append(c);
		}
		return sb.toString();
	}
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		this.doGet(request,response);
	}
}




```



### 登陆Servlet

```java
package cn.itcast.web.servlet;

import cn.itcast.domain.User;
import cn.itcast.service.UserService;
import cn.itcast.service.impl.UserServiceImpl;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println(request);

        //1.设置编码
        request.setCharacterEncoding("utf-8");

        //2.获取数据
        //2.1获取用户填写验证码
        String verifycode = request.getParameter("verifycode");

        //3.验证码校验
        HttpSession session = request.getSession();
        String checkcode_server = (String) session.getAttribute("CHECKCODE_SERVER");
        session.removeAttribute("CHECKCODE_SERVER");//确保验证码一次性
        if(!checkcode_server.equalsIgnoreCase(verifycode)){
            //验证码不正确
            //提示信息
            request.setAttribute("login_msg","验证码错误！");
            //跳转登录页面
            request.getRequestDispatcher("/login.jsp").forward(request,response);

            return;
        }

        Map<String, String[]> map = request.getParameterMap();
        //4.封装User对象
        User user = new User();
        try {
            BeanUtils.populate(user,map);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }


        //5.调用Service查询
        UserService service = new UserServiceImpl();
        User loginUser = service.login(user);
        //6.判断是否登录成功
        if(loginUser != null){
            //登录成功
            //将用户存入session
            session.setAttribute("user",loginUser);
            //跳转页面
            response.sendRedirect(request.getContextPath()+"/index.jsp");
        }else{
            //登录失败
            //提示信息
            request.setAttribute("login_msg","用户名或密码错误！");
            //跳转登录页面
            request.getRequestDispatcher("/login.jsp").forward(request,response);

        }




    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```



### 读取文件

#### ServletContext

```java
package cn.itcast.web.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;
import java.io.FileInputStream;


@WebListener
public class ContextLoaderListener implements ServletContextListener {

    /**
     * 监听ServletContext对象创建的。ServletContext对象服务器启动后自动创建。
     *
     * 在服务器启动后自动调用
     * @param servletContextEvent
     */
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        //加载资源文件
        //1.获取ServletContext对象
        ServletContext servletContext = servletContextEvent.getServletContext();

        //2.加载资源文件
        String contextConfigLocation = servletContext.getInitParameter("contextConfigLocation");

        //3.获取真实路径
        String realPath = servletContext.getRealPath(contextConfigLocation);

        //4.加载进内存
        try{
            FileInputStream fis = new FileInputStream(realPath);
            System.out.println(fis);
        }catch (Exception e){
            e.printStackTrace();
        }
        System.out.println("ServletContext对象被创建了。。。");
    }

    /**
     * 在服务器关闭后，ServletContext对象被销毁。当服务器正常关闭后该方法被调用
     * @param servletContextEvent
     */
    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("ServletContext对象被销毁了。。。");
    }
}

```











# ...

