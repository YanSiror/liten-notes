## 9 Ajax 技术

#### 9.1 Ajax 概述

- **什么是 Ajax**

  AJAX = 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。

  简短地说，在不重载整个网页的情况下，AJAX 通过后台加载数据，并在网页上进行显示。



### 9.2 Ajax 方法

#### 9.2.1 load() 方法

​	jQuery load() 方法是简单但强大的 AJAX 方法。

​	load() 方法从服务器加载数据，并把返回的数据放入被选元素中。

- **语法**

  *URL*  加载条件   data 键值对集合  callback 回调函数名

  ```javascript
  $(selector).load(URL,data,callback);
  ```

  

- **实例**

  下面的例子会把文件 "demo_test.txt" 的内容加载到指定的 **div** 元素中

  ```javascript
  $("#div1").load("demo_test.txt");
  ```

  

#### 9.2.2 get() 与 post() 

##### 1)  HTTP请求  GET & POST

两种在客户端和服务器端进行请求-响应的常用方法是：GET 和 POST。

- *GET* - 从指定的资源请求数据
- *POST* - 向指定的资源提交要处理的数据

GET 基本上用于从服务器获得（取回）数据。注释：GET 方法可能返回缓存数据。

POST 也可用于从服务器获取数据。不过，POST 方法不会缓存数据，并且常用于连同请求一起发送数据。



##### 2)  $.get() 请求数据

- **语法**

  *URL*  加载条件  callback 回调函数名

  ```javascript
  $.get(URL,callback);
  ```

- **实例**

  ```javascript
  $("button").click(function(){
    $.get("demo_test.php",function(data,status){
      alert("数据: " + data + "\n状态: " + status);
    });
  });
  ```



##### 3) $.post() 提交数据

- **语法**

  ```
  $.post(URL,data,callback);
  ```

- **实例**

  *URL* 请求参数  *data*  发送数据   *callback* 回调函数

  ```javascript
  $("button").click(function(){
      $.post("/try/ajax/demo_test_post.php",
      {
          name:"菜鸟教程",
          url:"http://www.runoob.com"
      },
      function(data,status){
          alert("数据: \n" + data + "\n状态: " + status);
      });
  });
  ```

  

#### 9.2.3 案例

**ajaxServlet 代码**

```java
@WebServlet("/ajaxServlet")
public class AjaxServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.获取请求参数
        String username = request.getParameter("username");
        //2.打印username
        System.out.println(username);
        //3.响应
        response.getWriter().write("hello : " + username);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

##### 1) ajax() 方法

```
<input type="button" value="发送异步请求" onclick="fun();">
<script>
    //定义方法
    function  fun() {
        //使用$.ajax()发送异步请求
        $.ajax({
            url: "ajaxServlet1111", // 请求路径
            type: "POST", //请求方式
            //data: "username=jack&age=23",//请求参数
            data: {"username": "jack", "age": 23},
            success: function (data) {
                alert(data);
            },//响应成功后的回调函数
            error: function () {
                alert("出错啦...")
            },//表示如果请求响应出现错误，会执行的回调函数

            dataType: "text"//设置接受到的响应数据的格式
        });
    }
</script>
```



##### 2) get方法

```javascript
<input type="button" value="发送异步请求" onclick="fun();">
<script>
    //定义方法
    function  fun() {
        $.get("ajaxServlet",{username:"rose"},function (data) {
            alert(data);
        },"text");
    }
</script>
```



##### 3) post方法

```javascript
<input type="button" value="发送异步请求" onclick="fun();">
<script>
    //定义方法
    function  fun() {
        $.get("ajaxServlet",{username:"rose"},function (data) {
            alert(data);
        },"text");
    }
</script>
```

##### 4) 异步与同步的区别

异步: 服务器和客户端可以同时操作,用户仍然可以继续操作,不用等待服务器响应

同步: 用户必须等待服务器返回数据才可以继续操作

