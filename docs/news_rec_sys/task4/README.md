# 前后端基础及交互

![image-20211227174438786](README.assets/image-20211227174438786.png)

# 1.前后端交互基本流程

本项目的前端采用基于Vue框架的Vant UI组件库，完成用户注册页、用户退出页、用户热门列表页、用户推荐列表页、新闻详情页等功能

后端采用Flask框架，使用MySQL、MongoDB和Redis作为数据存储，根据新闻推荐系统的整体功能，提供用户注册、用户登录、用户推荐页列表、用户热门页列表、新闻详情、用户行为等服务请求，完成用户从注册到新闻浏览、点赞和收藏的全流程

# 2.前端框架

## 2.1win10环境开发配置

因为之前玩过django所以历史遗留了一个node v12.8.0 的环境，可是那个项目又没做完

而这个项目是在node v16.13.1 下配置的

所以需要一个node版本管理的软件

请教MoMo

在Mac或者linux下

```bash
#安装n模块
npm install -g n 
#升级到最新稳定版
n stable
#安装指定版本
n v6.11.5 
```

我自己是win10，在同事的请教下发现win10也有node的版本管理工具nvm-windows

https://github.com/coreybutler/nvm-windows/releases

下载Assets中的nvm-setup.zip

我当前是1.1.9版本，用chrome下载会提示该文件会提示“******文件 存在危险，因此Chrome已将其拦截”，查看全部然后仍要保留就可以了

下完之后要解压是一个exe，双击安装

安装路径设置：nvm路径我是换到了D盘D:\nvm，nodejs软连接（symlink）设置成D:\nvm\nodejs，文件夹名不能带空格，不要带中文！！安装后，电脑会自动配置好系统环境。

然后这个安装过程是不会自动给你生产这个D:\nvm\nodejs文件夹的，我们在nvm文件夹下自己新建一个nodejs文件夹

然后修改settings.txt

```
root: D:\nvm
path: D:\nvm\nodejs
arch: 64
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

然后！！！

用管理员权限打开cmd

###### 安装node

- nvm install 12.8.0  

> 下载成功的时候，会提示你同时下载安装npm对应的版本

- 根据提示，切换到对应的node版本   nvm use 12.8.0
- nvm list 查看当前已安装和使用的node版本，正在使用中的node版本号前面会使用*标识
- node -v 查看node版本
- npm -v 查看npm版本

然后安装python27

https://www.python.org/downloads/release/python-2718/

选[Windows x86-64 MSI installer](https://www.python.org/ftp/python/2.7.18/python-2.7.18.amd64.msi)

装完之后记得配环境变量

1. 跳转到前端项目文件目录：`cd Vue-newsinfo`

2. 在本地跑的话，记得打开文件`package.json`，修改第49行的IP和端口，修改内容如下：  

   ```javascript
   "scripts": {
     "test": "echo \"Error: no test specified\" && exit 1",
     "dev": "webpack-dev-server --open --port 8686 --contentBase src --hot --host 127.0.0.1",
     "start": "nodemon src/main.js"
   },
   ```

   127.0.0.1表示游览器的访问IP（也称为本地IP），8686表示访问端口

3. 修改访问后端API接口的IP和端口，打开文件`main.js`，文件路径：`src/main.js`，修改第23行的IP和端口，修改内容如下：  

   ```javascript
   // Vue.prototype.$http = axios
   Vue.use(VueAxios, axios);
   // axios公共基路径，以后所有的请求都会在前面加上这个路径
   axios.defaults.baseURL = "http://39.108.138.91:3000"
   //改成后端api的url
   ```

   127.0.0.1表示后端项目的访问IP（也称为本地IP），5000表示访问端口。

4. 设置npm使用py27

   ```javascript
   npm install --python=python2.7
   ```

   或将其设置为始终用于：

   ```javascript
   npm config set python python2.7
   (我用的这个，管用！)
   ```

5. 本地安装node环境，在项目根目录命令行输入命令`npm install`安装依赖包

   如果因为版本或者网络问题下载失败请执行`npm install -g cnpm -registry=https://registry.npm.taobao.org/
   `和`cnpm install`

6. 启动前端服务：`npm run dev`

## 2.2目录结构

```
Vue-newsinfo
+---src---------------------------------项目主文件夹
|   +---assets--------------------------静态资源文件，包括img、css、js
|   |   +---css-------------------------样式文件
|   |   |   +---sign.css----------------登录注册页的样式
|   |   |   +---test.css----------------顶部导航样式
|   |   +---js--------------------------前端功能
|   |   |   +---cookie.js---------------定义cookie的相关操作，用于用户登录注册、退出时的cookie操作
|   +---components----------------------组件
|   |   +---bottomBar.vue---------------底部导航
|   |   +---hotLists.vue----------------热门页
|   |   +---Myself.vue------------------个人中心“我的”
|   |   +---NewsInfo.vue----------------新闻详情页
|   |   +---recLists.vue----------------推荐页
|   |   +---signIn.vue------------------登录页
|   |   +---signUp.vue------------------注册页
|   +---images--------------------------网站Logo图标，显示在浏览器地址栏或网页标签上
|   |   +---datawhale.png---------------Datawhale头像，用于“我的”页面
|   |   +---dw.png----------------------Datawhale二维码，用于“我的”页面
|   +---App.vue-------------------------根组件
|   +---index.html----------------------项目主页面
|   +---main.js-------------------------主脚本文件，用于定义全局变量、引入插件
|   +---router.js-----------------------路由脚本文件，用于配置路由url链接与页面组件的映射关系
|   +---store.js------------------------状态管理
+---.babelrc----------------------------ES6转码的配置文件
+---favicon.ico-------------------------浏览器小图标
+---package.json------------------------依赖包的配置文件，配置前端项目运行脚本
+---vue.config.js-----------------------vue项目的配置文件
+---webpack.config.js-------------------webpack的配置文件，用于项目打包
```

## 2.3 创建Vue项目的流程

1. 安装Vue CLI：提供基于Vue.js快速开发的工具，可实现交互式的项目脚手架搭建
2. 创建Vue项目：使用`vue create`命令，创建项目
3. 路由配置：使用`vue-router`库，配置`router.js`
4. 数据请求：使用`axios`封装数据请求
5. 选择UI组件库：选择UI设计组件，用于保证界面一致、用户交互、设计简洁的操作流程

## 2.4 页面功能

### 2.4.1 登录/注册页（signIn.vue/signUp.vue）

- 代码位于`src/components/`中的`signIn.vue`和`signUp.vue`
- 用户登录：输入用户名和密码，登录系统，勾选“记住我”，可以暂存7天的登录信息
- 用户注册：输入用户名、密码、验证密码、年龄，勾选性别和城市，进行用户注册，注册成功之后，将跳转到推荐页

### 2.4.2 推荐/热门页（recLists.vue/hotLists.vue）

- 代码位于`src/components/`中的`recLists.vue`和`hotLists.vue`
- 推荐页列表：展示用户推荐页新闻列表，一次展示10条数据
- 热门页列表：展示用户热门页新闻列表，一次展示10条数据

### 2.4.3 新闻详情页（NewsInfo.vue）

- 代码位于`src/components/`中的`NewsInfo.vue`
- 新闻详情：展示当前新闻内容，并提供点赞（喜欢）和收藏功能

### 2.4.4 个人中心页（Myself.vue）

- 代码位于`src/components/`中的`Myself.vue`
- 个人中心：展示用户头像和用户名，提供用户退出功能



# 3.后端框架

## 3.1目录结构

```
news_rec_sys
+---conf----------------------------------------项目配置文件
+---controller----------------------------------控制层，用于操作数据库接口
+---dao-----------------------------------------DAO层，定义数据对象以及数据库连接对象
+---materials-----------------------------------离线物料构建
|   +---news_scrapy-----------------------------爬取新闻
|   +---user_proccess---------------------------用户画像处理
|   +---material_proccess-----------------------物料画像处理
+---recpocess-----------------------------------推荐流程
|   +---cold_start------------------------------冷启动生成热门/推荐页列表
|   +---recall----------------------------------计算热度值
|   +---online.py-------------------------------在线构建热门/推荐页列表
|   +---offline.py------------------------------离线构建热门/推荐页列表
+---scheduler-----------------------------------定时任务脚本
+--server.py------------------------------------后端项目启动主程序
```

## 3.2后端API接口

### 3.2.1 用户注册请求

- 注册流程：通过前端接收JSON数据（包括用户名、密码、年龄、性别、所在城市），并使用Flask提供用户注册请求服务`/recsys/register`
- 代码逻辑：位于`server.py`中的`register()`方法，接收用户名、密码、年龄、性别、所在城市，根据用户名判断是否已经存在（调用`user_action_controller`中的`user_is_exist()`方法），如果有记录，则返回1，表示注册正常，否则返回0，表示注册失败；再将数据存入MySQL的`userinfo`库的`register_user`表中

```python
@app.route('/recsys/register', methods=["POST"])
def register():
    """用户注册
    """
    request_str = request.get_data()
    request_dict = json.loads(request_str)
    # print(request_dict)
    
    user = RegisterUser()
    user.username = request_dict["username"]
    user.passwd = request_dict["passwd"]

    # 查询当前用户名是否已经被用过了
    result = UserAction().user_is_exist(user, "register")

    if result != 0:
        return jsonify({"code": 500, "mgs": "this username is exists"})

    user.userid = snowflake.client.get_guid() # 雪花算法
    
    user.age = request_dict["age"]
    user.gender = request_dict["gender"]
    user.city = request_dict["city"]

    # 检验年龄格式的合法性
    try:
        age = int(user.age)
    except:
        return jsonify({"code": 500, "mgs": "age is not valid."})

    # 添加注册用户
    save_res = UserAction().save_user(user)
    if not save_res:
        return jsonify({"code": 500, "mgs": "register fail."})

    return jsonify({"code": 200, "msg": "register success."})
```

### 3.2.2 用户登录请求

- 登录流程：通过前端接收JSON数据（包括用户名、密码），并使用Flask提供用户登录请求服务`/recsys/login`
- 代码逻辑：位于`server.py`中的`login()`方法，接收用户名、密码，根据用户名和密码判断是否有记录，如果有记录，则返回1，表示登录正常；如果仅用户名存在，返回2，表示密码错误；否则返回0，表示用户不存在

```python
@app.route('/recsys/login', methods=["POST"])
def login():
    """用户登录
    """
    request_str = request.get_data()
    request_dict = json.loads(request_str)
    
    user = RegisterUser()
    user.username = request_dict["username"]
    user.passwd = request_dict["passwd"]

    # 查询数据库中的用户名或者密码是否存在
    try:
        result = UserAction().user_is_exist(user, "login")
        # print(result,"login")
        if result == 1:
            return jsonify({"code": 200, "msg": "login success"})
        elif result == 2:
            # 密码错误
            return jsonify({"code": 501, "msg": "passwd is error"})
        else:
            return jsonify({"code": 502, "msg": "this username is not exist!"})
    except Exception as e:
        return jsonify({"code": 500, "mgs": "login fail."})
```

### 3.2.3 用户推荐页请求

- 登录流程：通过前端接收json数据（包括用户名、年龄、性别），并使用Flask提供用户推荐页请求服务`/recsys/rec_list`
- 代码逻辑：位于`server.py`中的`rec_list()`方法，接收用户名、年龄、性别，根据用户名得到用户ID，根据用户ID调用冷启动的推荐页列表方法（`recprocess/online.py`中的`get_cold_start_rec_list_v2()`方法），返回10条推荐新闻

```python
@app.route('/recsys/rec_list', methods=["GET"])
def rec_list():
    """推荐页
    """
    user_name = request.args.get('user_id')
    age = request.args.get('age')
    gender = request.args.get('gender')
    
    # 如果年龄无法转int说明是老用户，不需要传age 和 gender 
    try:
        age = int(age)
    except:
        age = None
        gender = None

    # 查询用户的id
    user_id = UserAction().get_user_id_by_name(user_name)  
    if not user_id:
        return False

    if user_id is None:
        return jsonify({"code": 2000, "msg": "user_id is none!"}) 

    try:
        rec_news_list = recsys_server.get_cold_start_rec_list_v2(user_id, age, gender)
        # 冷启动策略
        # rec_news_list = recsys_server.get_cold_start_rec_list(user_id)
        if len(rec_news_list) == 0:
            jsonify({"code": 500, "msg": "rec_news_list is empty."}) 
        return jsonify({"code": 200, "msg": "request rec_list success.", "data": rec_news_list, "user_id": user_id})
    except Exception as e:
        print(str(e))
        return jsonify({"code": 500, "msg": "redis fail."}) 
```

### 3.2.4 用户热门页请求

- 登录流程：通过前端接收json数据（包括用户名），并使用Flask提供用户热门页请求服务`/recsys/hot_list`
- 代码逻辑：位于`server.py`中的`hot_list()`方法，接收用户名，根据用户名得到用户ID，根据用户ID调用热门页列表方法（`recprocess/online.py`中的`get_hot_list_v2()`方法），返回10条热门新闻

```python
@app.route('/recsys/hot_list', methods=["GET"])
def hot_list():
    """热门页面
    """
    user_name = request.args.get('user_id')

    if user_name is None:
        return jsonify({"code": 2000, "msg": "user_name none!"}) 

    # 查询用户的id
    user_id = UserAction().get_user_id_by_name(user_name)  
    if not user_id:
        return jsonify({"code": 2000, "msg": "user_id is not exits!."})

    try:
        # 这里需要改成get_hot_list, 当前get_hot_list方法还没有实现
        # rec_news_list = recsys_server.get_hot_list(user_id)
        rec_news_list = recsys_server.get_hot_list_v2(user_id)
        if len(rec_news_list) == 0:
            return jsonify({"code": 500, "msg": "request redis data fail."})
        return jsonify({"code": 200, "msg": "request hot_list success.", "data": rec_news_list, "user_id": user_id})
    except Exception as e:
        print(str(e))
        return jsonify({"code": 2000, "msg": "request hot_list fail."}) 
```

### 3.2.5 新闻详情请求

- 登录流程：通过前端接收json数据（包括用户名，新闻ID），并使用Flask提供新闻详情请求服务`/recsys/news_detail`
- 代码逻辑：位于`server.py`中的`news_detail()`方法，接收用户名，新闻ID，根据用户名得到用户ID，根据新闻ID得到新闻内容，通过查询`user_likes`表和`user_collections`表，得到新闻的用户行为（是否点赞、是否收藏），返回前端需要的新闻详情（新闻内容、用户是否点赞、用户是否收藏）

```python
@app.route('/recsys/news_detail', methods=["GET"])
def news_detail():
    """一篇文章的详细信息
    """
    user_name = request.args.get('user_name')
    news_id = request.args.get('news_id')
    
    user_id = UserAction().get_user_id_by_name(user_name)  

    # if news_id is None or user_id is None:
    if news_id is None or user_name is None:
        return jsonify({"code": 2000, "msg": "news_id is none or user_name is none!"}) 
    try:
        news_detail = recsys_server.get_news_detail(news_id)

        # recsys_server.save_user_consume(user_id,news_id)  # 记录用户消费的
  
        if UserAction().get_likes_counts_by_user(user_id,news_id) > 0:
            news_detail["likes"] = True
        else:
            news_detail["likes"] = False

        if UserAction().get_coll_counts_by_user(user_id,news_id) > 0:
            news_detail["collections"] = True
        else:
            news_detail["collections"] = False

        return jsonify({"code": 0, "msg": "request news_detail success.", "data": news_detail})
    except Exception as e:
        print(str(e))
        return jsonify({"code": 2000, "msg": "error"}) 
```

### 3.2.6 用户行为请求

- 登录流程：通过前端接收json数据（包括用户名，新闻ID、用户行为），并使用Flask提供新闻详情请求服务`/recsys/action`
- 代码逻辑：位于`server.py`中的`actions()`方法，接收用户名，新闻ID、用户行为，根据用户名得到用户ID，根据用户行为（是否点赞、收藏），修改`user_likes`表和`user_collections`表中对应的记录

```python
@app.route('/recsys/action', methods=["POST"])
def actions():
    """用户的行为：阅读，点赞，收藏
    """
    request_str = request.get_data()
    request_dict = json.loads(request_str)
    
    username = request_dict.get('user_name')
    newsid = request_dict.get('news_id')
    actiontype = request_dict.get("action_type")
    actiontime = request_dict.get("action_time")

    userid = UserAction().get_user_id_by_name(username)   # 获取用户 id
    if not userid:
        return jsonify({"code": 2000, "msg": "user not register"})

    #TODO 先判断当前的action_type是否是取消的意思，如果是的话，需要将数据库中对应的操作删掉
    action_type_list = actiontype.split(":")
    # print(actiontype)
    if len(action_type_list) == 2:
        _action_type = action_type_list[0]
        if action_type_list[1] == "false": # 如果数据库中这个参数为false的话
            # 删除数据 
            if _action_type=="likes": 
                UserAction().del_likes_by_user(userid,newsid)    # 删除用户喜欢记录
            elif _action_type=="collections":
                UserAction().del_coll_by_user(userid,newsid)     # 删除用户收藏记录 
        else: 
            if _action_type=="likes":
                userlikes = UserLikes()
                userlikes.new(userid,username,newsid)
                UserAction().save_one_action(userlikes)       # 记录用户喜欢记录
            elif _action_type=="collections":
                usercollections = UserCollections()
                usercollections.new(userid,username,newsid)
                UserAction().save_one_action(usercollections)   # 记录用户收藏记录 

    try:
        # 落日志
        logitem = LogItem()
        logitem.new(userid,newsid,action_type_list[0])
        LogController().save_one_log(logitem)

        # 更新redis中的展示数据   新闻侧
        # if action_type_list[0] in ["read","likes","collections"]:
        recsys_server.update_news_dynamic_info(news_id=newsid,action_type=action_type_list)
        return jsonify({"code": 200, "msg": "action success"})

    except Exception as e:
        print(str(e))
        return jsonify({"code": 2000, "msg": "action error"})
```

