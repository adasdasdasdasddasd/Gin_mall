# gin-mall

**基于 gin+gorm+mysql读写分离 的一个电子商场**

原项目地址：https://github.com/CocaineCong/gin-mall，本人抱着学习的目的对项目进行的改编及完善，感谢原作者的开源！！
# 项目的主要功能介绍

- 用户注册登录(JWT-Go鉴权)
- 用户基本信息修改，解绑定邮箱，修改密码
- 商品的发布，浏览等
- 购物车的加入，删除，浏览等
- 订单的创建，删除，支付等
- 地址的增加，删除，修改等
- 各个商品的浏览次数，以及部分种类商品的排行
- 设置了支付密码，对用户的金额进行了对称加密
- 支持事务，支付过程发送错误进行回退处理
- 可以将图片上传到对象存储，也可以切换分支上传到本地static目录下
- 添加ELK体系，方便日志查看和管理
# 项目的主要依赖：
Golang V1.16
- gin
- gorm
- mysql
- redis
- ini
- jwt-go
- crypto
- logrus
- qiniu-go-sdk
- dbresolver

# 项目结构
```
gin-mall/
├── api
├── cmd
├── conf
├── doc
├── middleware
├── model
├── pkg
│  ├── e
│  └── util
├── repository
│  ├── cache
│  ├── db
│  ├── es
│  ├── mq
│  └── redis
├── routes
├── serializer
├── service
└── static
```
- api : 用于定义接口函数，也就是controller的作用
- conf : 用于存储配置文件
- dao : 对持久层进行操作
- doc : 存放接口文档
- loading : 需要加载的应用
- middleware : 应用中间件
- model : 应用数据库模型
- pkg/e : 封装错误码
- pkg/util : 工具函数
- repository : 存放存储仓库
- repository/cache : 放置redis缓存
- repository/db : 放置持久层的mysql
- repository/db/dao : dao层，对db进行操作
- repository/db/model : 定义mysql的模型
- repository/es : 放置es，形成elk体系
- repository/mq : 放置各种mq，kafka，rabbitmq等等...
- routes : 路由逻辑处理
- serializer : 将数据序列化为 json 的函数，便于返回给前端
- service : 接口函数的实现
- static : 存放静态文件
## 简要说明
1. `mysql` 是存储主要数据。
2. `redis` 用来存储商品的浏览次数。
3. 由于使用的是AES对称加密算法，这个算法并不保存在数据库或是文件中，是第一次登录的时候需要给的值，因为第一次登录系统会送1w作为初始金额进行购物，所以对其的加密，后续支付必须要再次输入，否则无法进行购物。
4. 本项目运用了gorm的读写分离，所以要保证mysql的数据一致性。
5. 引入了ELK体系，可以通过docker-compose全部up起来，也可以本地跑(确保ES和Kibana都开启)
6. 用户创建默认金额为 **1w** ，默认头像为 `static/imgs/avatar/avatar.JPG`
# 导入接口文档

打开postman，点击导入

![postman导入](doc/1.点击import导入.png)

选择导入文件
![选择导入接口文件](doc/2.选择文件.png)

![导入](doc/3.导入.png)

效果

![展示](doc/4.效果.png)


这里是用postman查询es，Kibana也可以查看es！

![postman-es](doc/5.postman-es.png)

# 项目运行
**本项目采用Go Mod管理依赖**

**下载依赖**
```go
go mod tidy
```
**下载依赖**
```go
go run main.go
```
