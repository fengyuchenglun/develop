## Rest

​	英文是Representational State Transfer的缩写,翻译成中文是"表述性状态转义"。不管是中文还是英文翻译，都让人迷惑不解。

​	首先，之所以迷惑不解是因为前面主语被去掉了，全称是 Resource Representational State  Transfer。

​		Transfer：通俗来讲就是：资源在网络中以某种表现形式进行状态转移。
​		Resource：资源，即数据。比如 用户，角色等，可以类比为本地文件系统，通过文件目录定位文件位置。/document/music/daoxiang.mp3；
​		Representational：某种表现形式，比如用JSON，XML，JPEG等；
​		State Transfer：状态变化。通过HTTP动词实现。

> Rest是Roy Thomas Fielding（HTTP协议（1.0版和1.1版）的主要设计者、Apache服务器软件的作者之一、Apache基金会的第一任主席）博士于2000年在他的博士论文中提出来的一种万维网软件架构风格，目的是便于不同软件/程序在网络（例如互联网）中互相传递信息。

**Rest主要特点：**

1. REST是使用HTTP协议的进程间通信机制。
2. `其关键概念是资源(万物皆资源)`,它通常表示单个业务对象。REST使用HTTP动词操作资源,使用URL引用这些资源。。

**REST成熟度模型**

- LEVEL 0:只是向服务端点发起HTTP POST请求,进行服务调用
- LEVEL 1:引入了资源的概念
- LEVEL 2:使用HTTP动词执行操作
- LEVEL 3:基于HATEOAS原则设计,基本思想是由GET请求返回的资源信息中包含链接,这些链接能够执行该资源允许的操作

![image-20200724100846712](https://i.loli.net/2020/07/24/EqStj4xhkHwUfBe.png)

> `Level0和Level1最大的区别，就是Level1拥有了Restful的第一个特征——面向资源`，这对构建可伸缩、分布式的架构是至关重要的。同时，如果把Level0的数据格式换成Xml，那么其实就是SOAP，SOAP的特点是关注行为和处理，和面向资源的RESTful有很大的不同。
>
> Level0和Level1，其实都很挫，他们都只是把HTTP当做一个传输的通道，没有把HTTP当做一种传输协议。
>
> Level2,真正将HTTP作为了一种传输协议，最直观的一点就是Level2使用了HTTP动词，GET/PUT/POST/DELETE/PATCH....,这些都是HTTP的规范，规范的作用自然是重大的，用户看到一个POST请求，就知道它不是幂等的，使用时要小心，看到PUT，就知道他是幂等的，调用多几次都不会造成问题，当然，这些的前提都是API的设计者和开发者也遵循这一套规范，确保自己提供的PUT接口是幂等的。
>
> Level3，关于这一层，有一个古怪的名词，叫HATEOAS（Hypertext As The Engine Of Application State），中文翻译为“将超媒体格式作为应用状态的引擎”，核心思想就是每个资源都有它的状态，不同状态下，可对它进行的操作不一样，返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。当访问api.github.com，返回结果如下：
>
> ```
> {
>   "current_user_url": "https://api.github.com/user",
>   "authorizations_url": "https://api.github.com/authorizations",
>   // ...
> }
> ```
>
> ​	理解了这一层，再来看看REST的全称，Representational State Transfer，中文翻译为“表述性状态转移”，我们会好理解多了？
>
> ​	Level3的Restful API，给使用者带来了很大的便利，使用者只需要知道如何获取资源的入口，之后的每个URI都可以通过请求获得，无法获得就说明无法执行那个请求。

​	现在绝大多数的RESTful接口都做到了Level2的层次，做到Level3的比较少。当然，这个模型并
不是一种规范，只是用来理解Restful的工具。所以，做到了Level2，也就是面向资源和使用Http动词，就已经很Restful了,我们称之为Restful-like。

​	通俗的说:看Url就知道要什么->看http method就知道干什么 ->看http status code就知道结果如何   

​	最后要明确一点：REST 实际上只是一种设计风格，它并不是标准。



## restful

REST，名词，一种面向资源的网络架构规范。
RESTful，形容词，指实现了 REST规范的系统，如实现了REST规范的Web API就叫RESTful API。    

**为什么会出现Restful?**

在使用Restful之前，我们定义的操作接口可能如下：

```
http://127.0.0.1/user/query/1 GET  根据用户id查询用户数据
http://127.0.0.1/user/save POST 新增用户
http://127.0.0.1/user/update POST 修改用户信息
http://127.0.0.1/user/delete GET/POST 删除用户信息
```



```
RESTful用法：
http://127.0.0.1/user/1 GET  根据用户id查询用户数据
http://127.0.0.1/user  POST 新增用户
http://127.0.0.1/user  PUT 修改用户信息
http://127.0.0.1/user  DELETE 删除用户信息
```
​	之前的操作是没有问题的,大神（Roy Thomas Fielding）认为是有问题的。有什么问题呢?你每次请求的接口或者地址,都在做描述,例如查询的时候用了query,新增的时候用了save,其实完全没有这个必要,我使用了get请求,就是查询.使用post请求,就是新增的请求。通过http协议，我的意图很明显,完全没有必要做描述,这就是为什么有了restful.

通俗的来说就是:
传统的接口设计，就是过程式的，每个特定的动作有特定的接口。

​	后来的RESTful，其实就是一个面向对象的接口，接口是对象，这个对象有GET／POST／PUT／DELETE。。。等等成员函数（接口）。

幂等性：对同一个Rest接口的多次访问，得到的资源状态是相同的。

安全性：对盖Rest接口访问，不会使服务器端资源的状态发生改变。

| Http方法 | 资源操作 | 幂等 | 安全 |
| -------- | -------- | ---- | ---- |
| GET      | Select   | 是   | 是   |
| POST     | Insert   | 否   | 否   |
| PUT      | Update   | 是   | 否   |
| DELETE   | Delete   | 是   | 否   |

**REST的好处和弊端**

好处:

- 简单熟悉
- 可使用浏览器扩展或curl来测试API
- 直接支持请求/响应方式通信
- HTTP对防火墙友好
- 不需要中间代理,简化系统架构

弊端:

- 只支持请求/响应方式通信
- 没有代理缓冲消息,可能导致可用性降低
- 客户端必须知道服务实例的位置
- 在单个请求中获取多个资源具有挑战性
- 有时很难将多个更新操作映射到HTTP动词

> TODO：以上介绍Rest以及Restful基本概念写的很戳，需要重新优化。切入点从远程进程间以及服务器间通信演变历史说起。涉及到RPC、RMI、gRPC、SOAP、http、Web Service、GraphQL 、Webhook等概念以及流行的框架。这将是一个很长的故事，敬请期待。

## Restful最佳实践
​	遵循RESTful设计风格，同时控制复杂度及易于使用，仅遵循大部分原则，一个Restful-lile api设计风格。   

`以用户角色作为列子,一个用户对应多个角色,一个角色对应多个用户`   

遵循原则：
1. `使用https协议(建议)`

   使用https安全,如果不使用https,请使用其他安全方案,比如使用token等其他认证方案。

   

2. `版本号放入URL`

   例如：

   ```http
   get /api/v1/users // good
   ```

   版本处理有两种方案：

   - 放入请求头中
   - 放入url中(只是建议,感觉一目了然,在 url 中指定 API 的版本是个很好地做法)。v表示版本 1版本号.不推荐使用1.1带小版本的版本号；版本号一般只有大的项目才会使用，不管项目大小，建议都在url添加版本。
   
   


3. `请求头公共参数`

   每个请求公有的参数,比如token、请求客户端类型、请求来源(前台展示还是后台管理);
   公共参数放在请求header中。
   
   | 请求头参数            | 是否必须 | 说明                                    |
   | --------------------- | -------- | --------------------------------------- |
   | X-App                 | 是       | 客户端类型:*-ios,*-android,*-pc,*-h5    |
   | X-Lang                | 是       | 客户端语言:'en-us','zh-cn'等,默认中文   |
   | X-Orginal             | 是       | 请求来源:0-前台展示调用,默认;1-后台管理 |
   | X-Token/Authorization | 否       | 认证凭证                                |
   
   后续根据实际情况添加.
   
   

4. `只提供json返回格式`

   restful接口规范不限制请求的返回格式，可以返回xml、text、json等格式。

   对于响应返回的格式，JSON 因为它的可读性、紧凑性以及多种语言支持等优点，成为了 HTTP API 最常用的返回格式
   因此，建议采用 JSON 作为返回内容的格式。如果用户需要其他格式，比如xml，应该在请求头部 Accept 中指定。
   对于不支持的格式，服务端需要返回正确的 status code，并给出详细的说明。

   ```http
   Accept:application/json  告诉服务器客户端接受json格式返回
   Content-Type:application/json  告诉服务器请求内容为json格式
   ```

   

5. `使用自定义错误编码作为错误提示`

   对于如何进行错误提示，业界现在有两种做法:

   - 使用http状态码,比如：

     ```http
     200  请求成功接收并处理，一般响应中都会有 body
     404  客户端要访问的资源不存在，链接失效或者客户端伪造 URL 的时候回遇到这个情况
     ```

   - 使用自定义返回的错误码，业务异常http status统一返回200.例如：

     ```http
     1000 系统错误
     2001 用户不存在
     2002 用户名称已经存在
     ```

   建议使用自定义的状态。原因：

   1. http状态码可能不够用(暂时肯定够);http状态码在不同的业务场景下表述的意义可能不同，引起前后端理解歧义。
   2. 使用自定义返回的错误码更加灵活;
   3. 便于前后台代码统一处理;
   
   


6. `Path（路径）尽量使用名词，不使用动词，把每个URL看成一个资源`

   ```http
   get /getUserList   // bad 获得所有用户列表
   get /getActiveUserList //bad 获得所有激活的用户列表
   
   get /api/v1/users // good 获得所有用户列表
   get /api/v1/users?state=active // good 获得所有激活的用户列表
   ```

   rest只是一种接口规范不是标准.尽量遵守即可.有的接口不好用名词描述或者描述会让人很难理解,特殊情况我们还是可以使用动词进行描述.比如:登录|注销接口

   ```http
   post /api/v1/session //登录接口
   delete /api/v1/session //注销接口
   ```

   看上去很难理解的接口.如果不使用session,那需要把session替换成其他名词?特殊情况特殊处理。

   ```http
   post /api/v1/login //登录接口
   delete /api/v1/logout // 注销接口
   ```

   

7. `避免url最后带'/'`

   ```http
   get /api/v1/users/  // bad
   get /api/v1/users  // good
   ```

   最后带'/'与不带意思完全不一样,甚至两个不同的接口



8. `名词使用复试`

   ```http
   get /api/v1/users  // good 查询用户列表
   get /api/v1/users/1 // good 查询用户1的信息
   
   get /api/v1/user  // bad 查询用户列表
   get /api/v1/user/1 // bad 查询用户1的信息
   ```

   事实上，这是个人爱好问题，但复数形式更为常见。
   此外，在资源集合URL上用GET方法，它更直观，特别是get /api/v1/users?state=active。
   **但最重要的是：避免复数和单数名词混合使用，这显得非常混乱且容易出错。**



9. `为关系使用子资源`

   多级资源命名，**资源层级不建议超过3层**；

   ```http
   get /api/v1/users/1/roles  //查看用户1的角色信息
   get /api/v1/roles/admin/users // 查看管理员的角色列表
   ```

   如果资源层级太深，可以使用除了第一级，其他级别都用查询字符串表达。

   ```http
   get /api/v1/roles?users=1  //查看用户1的角色列表
   get /api/v1/users?roles=admin // 查看管理员的角色列表
   ```

10. `使用小驼峰命名法`

    使用小驼峰命名法作为属性标识符。
    { "yearOfBirth": 1982 }
    不要使用下划线（year_of_birth）或大驼峰命名法（YearOfBirth）。
    通常，RESTful Web服务将被JavaScript编写的客户端使用。
    客户端会将JSON响应转换为JavaScript对象（通过调用var person = JSON.parse(response)），然后调用其属性。因此，最好遵循JavaScript代码通用规范(JavaScript代码通用规范规定变量、参数和函数使用小驼峰法命名)。

    ```http
    person.year_of_birth // 不推荐，违反JavaScript代码通用规范
    person.YearOfBirth // 不推荐，JavaScript构造方法命名
    person.yearOfBirth // 推荐
    ```

    

11. `对可选的、复杂的查询参数，使用查询字符串(?或者&)`

    ```http
    get /activeUsers    //bad
    get /disableUsers   //bad
    ```

    为了让你的URL更小、更简洁。
    为资源设置一个基本URL，将可选的、复杂的参数用查询字符串表示。

    ```http
    get /api/v1/users?state=active&sex=male //good
    ```

    

12. `使用HTTP动词（GET,POST,PUT,DELETE）作为action操作URL资源`

    | http请求方法 | 描述                                                  |
    | ------------ | ----------------------------------------------------- |
    | get          | 查询                                                  |
    | post         | 添加                                                  |
    | patch        | 修改                                                  |
    | put          | 修改(PUT 是幂等的，而 PATCH 不是幂等的,一般用put就ok) |
    | delete       | 删除                                                  |

    推荐：

    ```http
    get /api/v1/users  // good 查询用户列表
    post /api/v1/users //good 添加用户
    put  /api/v1/users //good 编辑用户
    delete /api/v1/users/1 //good 删除参数
    ```

    - get请求传参使用url请求参数传参
    - post|put请求使用body传参,json格式
    - delete请求使用url路径传参,如果是批量删除使用body传参,json格式

    ```http
    get /api/v1/users?state=active //查询激活用户列表
    
    post /api/v1/users  //添加用户
    body:
    {
        id:1,
        name:'duanledexianxian',
        state:'active',
        ....
    }
    
    put /api/v1/users/1   //修改用户
    body:
    {
        id:1,
        name:'duanledexianxian',
        state:'active',
        ....
    }
     
    delete /api/v1/users/1 //删除参数 1是用户id
    delete /api/v1/users //批量删除参数
    or 
    delete /api/v1/users/batch //批量删除参数
    {
        ids:[1,2,....]
    }
    ```

    

    > **知识扩展**
>
    > 根据http规范，接口参数传递方式一般有以下几种方式：
>
    > 1. 查询参数
>
    >    ```http
    >    GET /api/v1/users?age=20&name=liuebei
    >    ```
>
    >    age与name即为查询参数。
>
    > 2. 路径参数
>
    >    ```http
    >    GET /api/v1/users/1 // 查询用户1的详情信息
    >    ```
>
    >    1即为路径参数
>
    > 3. 请求体参数
>
    >    请求体参数又分为**form-data**、**raw**、**binary**、**x-www-form-urlencode**几种类型，打开postman可以查看几种请求体设置。
>
    >    ![image-20200723172647499](https://i.loli.net/2020/07/23/6JrevD9hEV8BNUF.png)
>
    >    - form-data-表单提交
>
    >      可以提交**key-value**的键值对 也就是选择如下图的Text
>
    >      可以提交**文件**包括**多个文件** 选择如下图的File
>
    >      ```http
    >      POST  /api/v1/users // 添加用户
    >      
    >      Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
    >      
    >      ----WebKitFormBoundary7MA4YWxkTrZu0gW
    >      Content-Disposition: form-data; name="name"
    >      
    >      liebei
    >      ----WebKitFormBoundary7MA4YWxkTrZu0gW
    >      Content-Disposition: form-data; name="age"
    >      
    >      20
    >      ----WebKitFormBoundary7MA4YWxkTrZu0gW
    >      Content-Disposition: form-data; name="file"; filename="/C:/Users/44902/Desktop/转正任务"
    >      Content-Type: <Content-Type header here>
    >      
    >      (data)
    >      ----WebKitFormBoundary7MA4YWxkTrZu0gW
    >      ```
>
    >    - **x-www-form-urlencoded**
>
    >      同样是表单提交
>
    >      不同的是将表单内的数据转换为**键值对**,不能提交文件。
>
    >      ```http
    >      POST  /api/v1/users // 添加用户
    >      Content-Type: application/x-www-form-urlencoded
    >      
    >      name=liebei&age=20&file=
    >      ```
>
    >    - **raw**
>
    >      可以上传**支持的任意格式**的文本,支持的格式包括：Text、JavaScript、Json、Html、Xml等。
>
    >      ```http
    >      POST  /api/v1/users // 添加用户
    >      Content-Type: application/json
    >      
    >      {
    >          "name":"liebei",
    >          "age":"20"
    >      }
    >      ```
>
    >    - **binary**
>
    >      只可以上传**二进制数据**，只能上传**一个**文件
>
    >      ```http
    >      POST  /api/v1/users // 添加用户
    >      Content-Type: application/octet-stream
    >      
    >      "<file contents here>"
    >      ```



13. `过滤信息`
    
    1. pageNum：第几页,默认1
    2. pageSize：每页条数,默认10
    4. sortField：column1,column2排序字段
    4. sortOrder：排序规则，desc-降序、asc-升序、null/不传-默认排序
    5. key：搜索关键字（uri encode之后的）
    6. 其他
    
    可对查询参数封装成QueryCondition的dto对象,通过filter放入TreadLocal中,用的时候直接从TreadLocal获取.




14. `返回结果`

    1. `GET：返回资源对象`
    2. `POST：返回新生成的资源对象`
    3. `PUT：返回完整的资源对象`
    4. `DELETE：返回一个空文档`

    统一返回格式:

    ```http
    {
        "data": null,// 返回数据内容
        "code": "sys.success",//成功返回'sys.success',否则返回自定义的错误编码
        "message": null,//如果错误,则还有错误描述信息属性字段
        "traceId": "19e54aca08764bbe8548a61c4442a454"// 请求追踪唯一标识
    }
    ```

    分页统一返回格式:

    ```http
    {
        "code":'SUCCESS',//成功返回'sys.success',否则返回自定义的错误编码
        "message": null,//如果错误,则还有错误描述信息属性字段
        "traceId": "19e54aca08764bbe8548a61c4442a454",// 请求追踪唯一标识
        "data":{
            pageNum:0,//分页页面
            pageSize:10,//分页大小
            total:100,//记录总数
            pages:10,//分页页数
            list:[...]//列表内容
        }// 返回数据内容
    }
    ```

    新增操作需要返回新增资源的资源唯一编号,比如:新增用户

    ```http
    {
        "code":'SUCCESS',//成功返回'SUCCESS',否则返回自定义的错误编码
        "message": null,//如果错误,则还有错误描述信息属性字段
        "traceId": "19e54aca08764bbe8548a61c4442a454",// 请求追踪唯一标识
        "data":{
          useriId:1,//用户编号
        }// 返回数据内容
    }
    ```

    编辑/删除资源返回操作是否是否成功状态：比如：删除用户

    ```http
    {
        "code":'SUCCESS',//成功返回'SUCCESS',否则返回自定义的错误编码
        "message": null,//如果错误,则还有错误描述信息属性字段
        "traceId": "19e54aca08764bbe8548a61c4442a454",// 请求追踪唯一标识
        "data":true // 删除用户是否成功
    }
    ```

    

15. `速率限制(暂时不需要)`
    
    1. X-RateLimit-Limit: 每个IP每个时间窗口最大请求数
    2. X-RateLimit-Remaining: 当前时间窗口剩余请求数
    3. X-RateLimit-Reset: 下次更新时间窗口的时间（UNIX时间戳），达到下个时间窗口时，Remaining恢复为Limit
    
    如果对访问的次数不加控制，很可能会造成 API 被滥用，甚至被 DDos 攻击。
    根据使用者不同的身份对其进行限流，可以防止这些情况，减少服务器的压力。
    暂时不需要,以后可能做限流控制、访问控制的时候可能用的上。
    

    
16. `限制返回值的域，fields=id,subject,customerName`

    需要后台支持

    ```http
    比如:/api/v1/users?fields=id,name,....//获取用户列表,包含的字段包括id、名称.
    可以与11一起处理
    ```

    



## 待完善原则

1. Hypermedia API（HATEOAS），通过接口URL获取接口地址及帮助文档地址信息
```http
暂时不需要,如果需要实现前后台都需要支持
```


2. 缓存，使用ETag和Last-Modified
```http
暂时不需要,做性能优化的时候,根据需要添加
```

3. 文件上传,下载

4. 其他未完待续，持续更新中.   




## 总结
对于一个restful api，期望能够做到:

1. 看Url就知道要操作的资源是什么，是用户还是角色
2. 看Http Method就知道操作动作是什么，是添加（post）还是删除（delete）
3. 看Http Status Code就知道操作结果如何，是成功（200）还是内部错误（500）,当然使用自定义错误编码可以实现同样的效果。



## 参考
1. https://developer.github.com/v3/
2. https://fxm5547.com/%E6%8A%80%E6%9C%AF%E8%A7%84%E8%8C%83/2017/11/08/RESTful-API-standard/
3. https://blog.csdn.net/niubity/article/details/64438668
4. http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html
5. http://blog.jobbole.com/41233/
6. https://www.cnblogs.com/jaxu/p/7908111.html
7. https://zhuanlan.zhihu.com/p/60352360
8. https://zhuanlan.zhihu.com/p/28858632




