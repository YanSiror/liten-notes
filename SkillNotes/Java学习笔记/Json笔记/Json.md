## 10 JSON 技术

### 10.1 概念

多用于存储和交换文本信息的语法

进行数据的传输

JSON 比 XML 更小、更快、更容易解析

### 10.2 语法

#### 10.2.1 JSON 数据类型

##### 1) 支持的数据类型

- **JSON** 支持以下数据类型

  |        类型        |                     描述                     |
  | :----------------: | :------------------------------------------: |
  |  数字型（Number）  |       JavaScript 中的双精度浮点型格式        |
  | 字符串型（String） |  双引号包裹的 Unicode 字符和反斜杠转义字符   |
  | 布尔型（Boolean）  |                true 或 false                 |
  |   数组（Array）    |                 有序的值序列                 |
  |    值（Value）     | 可以是字符串，数字，true 或 false，null 等等 |
  |   对象（Object）   |              无序的键:值对集合               |
  | 空格（Whitespace） |             可用于任意符号对之间             |
  |        null        |                      空                      |



##### 2) 数字型

- **JavaScript 中的双精度浮点型格式**

  下表展示了数字类型：

  | 类型             | 描述                            |
  | :--------------- | :------------------------------ |
  | 整形（Integer）  | 数字1-9，0和正负数              |
  | 分数（Fraction） | 分数，比如 .3，.9               |
  | 指数（Exponent） | 指数，比如 e，e+，e-，E，E+，E- |

  **语法：**

  ```
  var json-object-name = { string : number_value, .......}
  ```

  **示例：**

  下面的示例展示了数字类型，其值不应该使用引号包裹：

  ```javascript
  var obj = {marks: 97}
  ```



##### 3) 字符串型

- **零个或多个双引号包裹的字符以及反斜杠转义序列**

  **语法：**

  ```
  var json-object-name = { string : "string value", .......}
  ```

  **示例：**

  下面的示例展示了字符串数据类型：

  ```
  var obj = {name: 'Amit'}
  ```



##### 4) 布尔型

- 包含 true 和 false 两种类型

  **语法：**

  ```
  var json-object-name = { string : true/false, .......}
  ```

  **示例：**

  ```
  var obj = {name: 'Amit', marks: 97, distinction: true}
  ```



##### 5) 数组

- **有序的值集合**

  **语法：**

  ```
  [ value, .......]
  ```

  **示例：**

  下面的示例展示了一个包含多个对象的数组：

  ```
  {
      "books": [
          { "language":"Java" , "edition":"second" },
          { "language":"C++" , "lastName":"fifth" },
          { "language":"C" , "lastName":"third" }
      ]
  }
  ```



##### 6) 对象

- **无序的名/值对集合**

  **语法：**

  ```
  { string : value, .......}
  ```

  **示例：**

  下面的例子展示了对象：

  ```
  {
      "id": "011A",
      "language": "JAVA",
      "price": 500,
  }
  ```

  

#### 10.2.2 JSON 数据定义与获取

##### 1) 定义基本格式

```javascript
var person = {"name": "张三", age: 23, 'gender': true};
//数据获取
var name = person.name;
var name = person["name"];
alert(name);
```



##### 2) 嵌套格式

**格式1 : {} + []**

```javascript
var persons = {
    "persons": [
        {"name": "张三", "age": 23, "gender": true},
        {"name": "李四", "age": 24, "gender": true},
        {"name": "王五", "age": 25, "gender": false}
        ]
};
//数据获取
alert(persons);
var name1 = persons.persons[2].name;
```

**格式2 : [] + {}**

```javascript
var ps = [
    {"name": "张三", "age": 23, "gender": true},
    {"name": "李四", "age": 24, "gender": true},
    {"name": "王五", "age": 25, "gender": false}
];
//数据获取
ps[1].name
```



##### 3) 使用 for 循环遍历

```javascript
var ps = [
    {"name": "张三", "age": 23, "gender": true},
    {"name": "李四", "age": 24, "gender": true},
    {"name": "王五", "age": 25, "gender": false}
];
//获取ps中的所有值 外层对象长度3 内层键值参数 3 共打印9次
 for (var i = 0; i < ps.length; i++) {
     var p = ps[i];
     for(var key in p){
         alert(key+":"+p[key]);
     }
 }
```



#### 10.2.3 Java 与 JSON 数据转换

##### 1)  Java 对象转 JSON

- **使用步骤**

  > 1、导入jackson的相关jar包
  > 2、创建Jackson核心对象 ObjectMapper
  > 3、调用ObjectMapper的相关方法进行转换
  > 				writeValue(json字符串数据,Class)

  

- **writeValue(参数1，obj) 方法**

  将 Java 对象通过 File Writer 等路径进行保存

  - **参数1**

    > File：将obj对象转换为JSON字符串，并保存到指定的文件中
    > Writer：将obj对象转换为JSON字符串，并将json数据填充到字符输出流中
    > OutputStream：将obj对象转换为JSON字符串，并将json数据填充到字节输出流中

  - **代码**

    ```java
    //1.创建Person对象
    Person p = new Person();
    p.setName("张三");
    p.setAge(23);
    p.setGender("男");
    p.setBirthday(new Date());
    
    //2.转换
    ObjectMapper mapper = new ObjectMapper();
    //3.将数据写到d://a.txt文件中
    mapper.writeValue(new File("d://a.txt"),p);
    ```

    

- **writeValueAsString(obj) 方法**

  将对象转为json字符串

  - **代码**

    ```java
    //1.创建Person对象
    Person p = new Person();
    p.setName("张三");
    p.setAge(23);
    p.setGender("男");
    p.setBirthday(new Date());
    
    //2.转换
    ObjectMapper mapper = new ObjectMapper();
    String json = mapper.writeValueAsString(p);
    System.out.println(json);
    ```

    

- **注解**

  ```java
  @JsonIgnore：排除属性。
  ​		@JsonIgnore
  ​		Date birthday;      //忽略birthday属性转换
  @JsonFormat：属性值的格式化  
  ​		@JsonFormat(pattern = "yyyy-MM-dd")
  ```

  

- **复杂 Java 对象转换**

  List：数组类型
  Map：与对象格式一致

  

##### 2)  JSON 对象转 Java

- **使用步骤**

  > 1、导入jackson的相关jar包
  > 2、创建Jackson核心对象 ObjectMapper
  > 3、调用ObjectMapper的相关方法进行转换
  > 				readValue(json字符串数据,Class)

- **代码**

  ```java
  //1.初始化JSON字符串
  String json = "{\"gender\":\"男\",\"name\":\"张三\",\"age\":23}";
  
  //2.创建ObjectMapper对象
  ObjectMapper mapper = new ObjectMapper();
  //3.转换为Java对象 Person对象
  Person person = mapper.readValue(json, Person.class);
  ```

  

#### 10.2.4 校验新用户名是否存在

##### 1) 代码

- **register.html** 

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>注册页面</title>
      <script src="js/jquery-3.3.1.min.js"></script>
      <script>
          //在页面加载完成后
          $(function () {
             //给username绑定blur事件
             $("#username").blur(function () {
                 //获取username文本输入框的值
                 var username = $(this).val();
                 //发送ajax请求
                 //期望服务器响应的数据格式：{"userExsit":true,"msg":"用户名太受欢迎,请更换一个"}
                 //  {"userExsit":false,"msg":"用户名可用"}
                 $.get("findUserServlet",{username:username},function (data) {
                     //判断userExsit键的值是否是true
                     // alert(data);
                     var span = $("#s_username");
                     if(data.userExsit){
                         //用户名存在
                         span.css("color","red");
                         span.html(data.msg);
                     }else{
                         //用户名不存在
                         span.css("color","green");
                         span.html(data.msg);
                     }
                 });
             }); 
          });
      </script>
  </head>
  <body>
      <form>
          <input type="text" id="username" name="username" placeholder="请输入用户名">
          <span id="s_username"></span>
          <br>
          <input type="password" name="password" placeholder="请输入密码"><br>
          <input type="submit" value="注册"><br>
      </form>
  </body>
  </html>
  ```

- **FindUserServlet.java**

  ```java
  @WebServlet("/findUserServlet")
  public class FindUserServlet extends HttpServlet {
      protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          //1.获取用户名
          String username = request.getParameter("username");
  
          //2.调用service层判断用户名是否存在
  
          //期望服务器响应回的数据格式：{"userExsit":true,"msg":"此用户名太受欢迎,请更换一个"}
          //                         {"userExsit":false,"msg":"用户名可用"}
  
          //设置响应的数据格式为json
          response.setContentType("application/json;charset=utf-8");
          Map<String,Object> map = new HashMap<String,Object>();
  
          if("tom".equals(username)){
              //存在
              map.put("userExsit",true);
              map.put("msg","此用户名太受欢迎,请更换一个");
          }else{
              //不存在
              map.put("userExsit",false);
              map.put("msg","用户名可用");
          }
  
          //将map转为json，并且传递给客户端
          //将map转为json
          ObjectMapper mapper = new ObjectMapper();
          //并且传递给客户端
          mapper.writeValue(response.getWriter(),map);
      }
  
      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          this.doPost(request, response);
      }
  }
  ```

  启动 Tomcat 进行部署, 然后获取结果

##### 2) 总结

服务器响应的数据，在客户端使用时，要想当做json数据格式使用。有两种解决方案：

> $.get(type):将最后一个参数type指定为"json"
>
> 在服务器端设置MIME类型 
>
> ​		response.setContentType("application/json;charset=utf-8");
