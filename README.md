#学习日志[2016/6/27]

### nodejs 文件基本结构

    /bin 
    ...www  
    /node_modules
    /public
    ... images
    ... javascripts
    ... stylesheets
    /routes
    ... index
    /views
    ... /error
    ... /layouts
    ... /partials
    app.js
    package.json

### 如何配置hbs

    var hbs = exphbs.create({
        partialsDir: 'views/partials',
        layoutsDir: "views/layouts/",
        defaultLayout: 'main',
        extname: '.hbs'
    });
    app.engine('hbs', hbs.engine);

### 如何利用hbs

    /layouts
    ... lg.hbs
    ... main.hbs
    /partials
    ... head.hbs
    ... header.hbs

    1.可以使用layout来调用模块代码进行渲染
    2.利用    {{>head}}  {{>body}}调用模块化代码

### 如何映射静态文件目录

    利用
    router.get('/blog', function(req, res, next) {
     res.render('blog', { title: 'Express', layout: 'main' });
    });
    localhost:3000/blog将跳转到blog.hbs,并利用main.hbs进行填充代码渲染

### 如何配置路由

    app.use(logger('dev'));
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({ extended: false }));
    app.use(cookieParser());
    app.use(express.static(path.join(__dirname, 'public')));

    app.use('/', routes);
    app.use('/admin', admin);

    '/'将会利用路由跳转到相关静态文件

#学习日志[2016/6/28]

### markdown语法

[Markdown语法说明](http://www.appinn.com/markdown/#link)  
  1.一个 Markdown段落是由一个或多个连续的文本行组成，它的前后要有一个以上的空行被视为空行  
  2.标题，类 Atx 形式则是在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶，例如：

    # 这是 H1

    ## 这是 H2

    ###### 这是 H6
  3.代码区段：tab4个空格开始  
  4.要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：

    This is [an example](http://example.com/ "Title") inline link.

    [This link](http://example.net/) has no title attribute.

### 配置mongodb

  1.从官网下载mongodb-win32-x86_64-2008plus-3.6.7.zip  
  2.创建数据库文件的存放位置，比如d:/mongodb/data/db。启动mongodb服务之前需要必须创建数据库文件的存放文件夹，否则命令不会自动创建，而且不能启动成功。  
  3.打开cmd（windows键+r输入cmd）命令行，进入D:\mongodb\bin目录（如图先输入d:进入d盘然后输入cd d:\mongodb\bin，
    输入如下的命令启动mongodb服务：
        D:/mongodb/bin>mongod --dbpath D:\mongodb\data\db  
  4.mongodb默认连接端口27017,如果出现"it looks like you are trying to acccess mongoDB over HTTP on the native driver port"说明成功  
   5.可以将MongoDB设置成Windows服务，在F:\mongodb下创建mongodb.cfg,内容为
        ##数据文件
        dbpath=F:\mongodb\data\db 
        ##日志文件
        logpath=F:\mongodb\log\mongodb.log  
   6.在F:\mongodb中创建log日志文件夹,新建mongodb.log日志文件
        用【管理员】身份打开cmd命令行，进入D:\mongodb\bin目录，输入如下的命令：
        D:\mongodb\bin>mongod --config D:\mongodb\mongo.config  
   7.最后services.msc查看mongoDB服务并启动  

### 通过CMD生成文本目录

    1.通过dir命令
    dir /o /n /B
    输出到同目录下的file.txt
    dir /o /n /B >file.txt
    2.通过tree命令
    tree /f /A
    tree /A >file.txt

### webstorm中mongoose 的使用

    1. 打开webstorm中右下角的terminal 输入$ npm install --save-mongoose
    2. run当前的项目，就会发现node_modules中就会有mongoose这个文件夹

### 如何调用数据
    
    router.get('/', function(req, res, next) {
    // res.render('index', { title: 'Express' });  //使用数据库的时候不能渲染网页   

      mongoose.connect('mongodb://127.0.0.1:27017/test');  //连接到数据库  自动会新建一个test的数据表    

      var Schema = mongoose.Schema;    //获取schema

      var blogSchema = new Schema({    //用schema创建blogSchema
        title:  String,
        author: String,
        body:   String
      });   

      var Blog = mongoose.model('Blog', blogSchema);   //用blogSchema创建Blog  注意大写和匹配 

      blog = new Blog({      //创建一个blog实例
        title: 'aaa',
        author: 'bbb',
        body:   'ccc'
      })    

      blog.save(function(err, doc) {   //保存
        if (err) {
          console.log('error')         //出现错误打印错误信息
        } else {
          console.log(doc)             //成功返回数据，可以在console中看到
        }
      })    
    

    });

### 用mongoose操作MongoDB

#### 获得mongoose模板

    var mongoose = require('mongoose');
    mongoose.connect("mongodb://127.0.0.1:27017/test");
    /*  var Schema = mongoose.Schema;  用Schema代替mongoose.Schema*

#### 存储数据

    var PersonInfo = new mongoose.Schema({
        age       : Number,
        id        : String,
        phone     : String,
        colloge   : String,
        hometown  : String
    }); 

    var Person = mongoose.model('Person',PersonInfo);   

    var person = new Person({
        age     :'20',
        id      :'3326241995123456',
        phone   :'13819630116',
        colloge :'hznu',
        hometown :'xianju'
    })  

    person.save(function (err) {
        if(err){
            console.log('保存失败');
        }
        console.log('success');
    })

#### 增加数据

    var person1 = new Person({
        age     :'15',
        id      :'3326242000123456',
        phone   :'1776542365',
        colloge :'no',
        hometown :'xianju'
    })
    person1.save(function (err) {
        if(err){
            console.log('保存失败');
        }
        console.log('success');
    })

#### 修改数据

    var conditions = {age:20};
    var update={$set:{hometown:'taizhou'}};
    //第一个参数conditions是选择条件，第二个参数update是选择后该如何更改的参数,第三个是回调函数
    Person.update(conditions,update,function (error,data) {
        if(error){
            console.log(error);
        }else{
            console.log(data);
        }
    })

#### 删除数据

    Person.remove({age:20},function (err) {
        if(err){
            console.log(err);
        }else{
            console.log('删除成功');
        }
    })

#### 查询数据

    //$lt表示小于
    Person.find({"age":{"$lt":19}},function (error,docs) {
        if(error){
            console.log(error);
        }else{
            console.log(docs);
        }
    })

# 学习日志【2016/6/29】

### 博客完成进度 

  * 登录功能完成  
  * 整体界面框架搭建完成  

### mongodb 查询语句

    1.db.users.find({"age" : 27}) select * from users where age = 27
    2.db.users.find({}, {"username" : 1, "email" : 1}) select username, email from users
    3.db.users.find({}, {"username" : 1, "_id" : 0}) // no case  // 即时加上了列筛选，_id也会返回；必须显式的阻止_id返回
    4.db.users.find({"age" : {"$gte" : 18, "$lte" : 30}}) select * from users where age >=18 and age <= 30 // $lt(<) $lte(<=) $gt(>) $gte(>=)

### CSS3 如何让背景图片拉伸填充避免重复显示

    使用background-size属性方法：
    1.数值表示 100px
    2.百分比表示方式 30%
    3.等比扩展图片来填满元素，即cover值
    4.等比缩小图片来适应元素的尺寸，即contain值
    5.以图片自身大小来填充元素，即auto值

### 数据的增删改查注意点

    1. 数据的增删改查都是在model也就是当前的User下进行的，而不是在新建的变量，也就是当前的user中进行的；save操作是以实例为对象的。  

    2. 数据的增删改查产生的效果可以在console中看到，在浏览器输入localhost:8080/register，由于不能渲染网页，所以是500错误，console中会出现。  

    3. 数据的操作都是异步式的，因此显示的结果并不是按照程序写的顺序执行。

    4. mongo exployer 只能查看和修改数据  

    5. romomongo 可以查看、修改、删除数据 鼠标右键即可执行操作。  

### 新注册的用户是如何插入mongodb数据库中的

    mongoose.model('User', UserSchema);

    mongoose在内部创建collection时将我们传递的collection名小写化，同时如果小写化的名称后面没有字母——s,则会在其后面添加一s,针对我们刚建的collection,则会命名为：users。  

### 利用Mongodb Nodejs 实现登录功能

  1.新建表User并向表中新增一条记录

    var userSchema = require('../db/schema/user');
    var User = mongoose.model('User', userSchema);

  2.对login页面submit事件监听

    $(init);

    function init() {   

      $("body").on('click', '#loginBtn', doLogin);
    }   

    function doLogin() {    

      $.ajax({
        type: "POST",
        url: "/login",
        contentType: "application/json",
        dataType: "json",
        data: JSON.stringify({
          'usr': $("#usr").val(),
          'pwd': $("#pwd").val()
        }),
        success: function(result) {
          if (result.code == 99) {
            $(".login-box-msg").text(result.msg);
          } else {
            $.cookie('username', result.data.username, {expires:30});
            $.cookie('password', result.data.password, {expires:30});
            $.cookie('id', result.data._id, {expires:30});
            location.href = "/blog";
          }
        }
      })
    }

  3.mongodb findOne()

    > db.blog.findOne({"author.name" : "Jane"})

  find查询出来的是mongoose的doc对象，支持update等操作，不是POJO，用doc.toObject()转换成POJO
    
### Mongoose 模型提供了 find, findOne, 跟 findById 方法用于文档查询

#### Model.find  

    Model.find(query, fields, options, callback)// fields 和 options 都是可选参数
    例如：Model.find({'csser.com':5},function(err, docs){// docs 是查询的结果数组 });
    只查询指定键的结果

#### Model.findOne  

    与 Model.find 相同，但只返回单个文档
    Model.findOne({ age:5},function(err, doc){// doc 是单个文档});

#### Model.findById

  与 findOne 相同，但它接收文档的 _id 作为参数，返回单个文档。_id 可以是字符串或 ObjectId 对象  

    Model.findById(obj._id,function(err, doc){// doc 是单个文档});

### 关于jQuery新的事件绑定机制on()的使用技巧

    on(events,[selector],[data],fn)  

    例子 ：$("body").on('click', '#loginBtn', doLogin);

    events:一个或多个用空格分隔的事件类型和可选的命名空间，如"click"或"keydown.myPlugin" 。  
    selector:一个选择器字符串用于过滤器的触发事件的选择器元素的后代。如果选择器为null或省略，当它到达选定的元素，事件总是触发。  
    data:当一个事件被触发时要传递event.data给事件处理函数。  
    fn:该事件被触发时执行的函数。 false 值也可以做一个函数的简写，返回false。  

### jquery.cookie用法详细解析

    语法：$.cookie(名称,值,[option])
    例如：$.cookie('username', result.data.username, {expires:30});

    (1)读取cookie值：
    $.cookie(cookieName)      cookieName:要读取的cookie名称。
    示例：$.cookie("username"); 读取保存在cookie中名为的username的值。

    (2)写入设置Cookie值：
    $.cookie(cookieName,cookieValue);    cookieName:要设置的cookie名称，cookieValue表示相对应的值。
    示例:$.cookie("username","admin"); 将值"admin"写入cookie名为username的cookie中。
    $.cookie("username",NULL);　　　销毁名称为username的cookie

    (3) [option]参数说明：
    expires:　　有限日期，可以是一个整数或一个日期(单位：天)   这个地方也要注意，如果不设置这个东西，浏览器关闭之后此cookie就失效了
    path:　　　 cookie值保存的路径，默认与创建页路径一致。
    domin: cookie域名属性，默认与创建页域名一样。　　这个地方要相当注意，跨域的概念，如果要主域名二级域名有效则要设置　　".xxx.com"
    secrue:　　 一个布尔值，表示传输cookie值时，是否需要一个安全协议。


### Node.js开发框架Express

#### 1.目录结构

    nodejs-demo/
    ├──app.js  应用核心配置文件

    ├──bin  启动项目的脚本文件

    ├──node_modules  项目依赖库

    ├──package.json  项目依赖配置及开发者信息

    ├──public  静态文件(css,js,img)

    ├──routes  路由文件(MVC中的C,controller)

    └──views  页面文件(Ejs模板)

#### 2.package.json项目配置

  package.json用于项目依赖配置及开发者信息，scripts属性是用于定义操作命令的，可以非常方便的增加启动命令，比如默认的start，用npm start代表执行node ./bin/www命令。
  查看package.json文件。

    {
      "name": "express4-demo",
      "version": "0.0.0",
      "private": true,
      "scripts": {
        "start": "node ./bin/www"
      },
      "dependencies": {
        "body-parser": "~1.10.2",
        "cookie-parser": "~1.3.3",
        "debug": "~2.1.1",
        "ejs": "~2.2.3",
        "express": "~4.11.1",
        "morgan": "~1.5.1",
        "serve-favicon": "~2.2.0"
      }
    }

#### 3.app.js核心文件

    // 加载依赖库，原来这个类库都封装在connect中，现在需地注单独加载
    var express = require('express'); 
    var path = require('path');
    var favicon = require('serve-favicon');
    var logger = require('morgan');
    var cookieParser = require('cookie-parser');
    var bodyParser = require('body-parser');    

    // 加载路由控制
    var routes = require('./routes/index');
    //var users = require('./routes/users');    

    // 创建项目实例
    var app = express();    

    // 定义EJS模板引擎和模板文件位置，也可以使用jade或其他模型引擎
    app.set('views', path.join(__dirname, 'views'));
    app.set('view engine', 'ejs');  

    // 定义icon图标
    app.use(favicon(__dirname + '/public/favicon.ico'));
    // 定义日志和输出级别
    app.use(logger('dev'));
    // 定义数据解析器
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({ extended: false }));
    // 定义cookie解析器
    app.use(cookieParser());
    // 定义静态文件目录
    app.use(express.static(path.join(__dirname, 'public')));    

    // 匹配路径和路由
    app.use('/', routes);
    //app.use('/users', users); 

    // 404错误处理
    app.use(function(req, res, next) {
        var err = new Error('Not Found');
        err.status = 404;
        next(err);
    }); 

    // 开发环境，500错误处理和错误堆栈跟踪
    if (app.get('env') === 'development') {
        app.use(function(err, req, res, next) {
            res.status(err.status || 500);
            res.render('error', {
                message: err.message,
                error: err
            });
        });
    }   

    // 生产环境，500错误处理
    app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('error', {
            message: err.message,
            error: {}
        });
    }); 

    // 输出模型app
    module.exports = app;

#### 4.www文件

项目启动代码也被移到./bin/www的文件，www文件也是一个node的脚本，用于分离配置和启动程序。

    #!/usr/bin/env node     

    /**
     * 依赖加载
     */
    var app = require('../app');
    var debug = require('debug')('nodejs-demo:server');
    var http = require('http'); 

    /**
     * 定义启动端口
     */
    var port = normalizePort(process.env.PORT || '3000');
    app.set('port', port);  

    /**
     * 创建HTTP服务器实例
     */
    var server = http.createServer(app);    

    /**
     * 启动网络服务监听端口
     */
    server.listen(port);
    server.on('error', onError);
    server.on('listening', onListening);    

    /**
     * 端口标准化函数
     */
    function normalizePort(val) {
      var port = parseInt(val, 10);
      if (isNaN(port)) {
        return val;
      }
      if (port >= 0) {
        return port;
      }
      return false;
    }   

    /**
     * HTTP异常事件处理函数
     */
    function onError(error) {
      if (error.syscall !== 'listen') {
        throw error;
      } 

      var bind = typeof port === 'string'
        ? 'Pipe ' + port
        :  'Port ' +  port

      // handle specific listen errors with friendly messages
      switch (error.code) {
        case 'EACCES':
          console.error(bind + ' requires elevated privileges');
          process.exit(1);
          break;
        case 'EADDRINUSE':
          console.error(bind + ' is already in use');
          process.exit(1);
          break;
        default:
          throw error;
      }
    }   

    /**
     * 事件绑定函数
     */
    function onListening() {
      var addr = server.address();
      var bind = typeof addr === 'string'
        ? 'pipe ' + addr
        : 'port ' + addr.port;
      debug('Listening on ' + bind);
    }