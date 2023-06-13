# ChatServer
## 基于muduo网络库的集群聊天服务器

### 该项目是基于muduo网络库的集群聊天服务器，主要实现了登录、注销、注册、添加好友、一对一聊天、群组创建、群聊以及离线消息的接收和存储。

* 基于Muduo网络库实现高并发网络IO服务，解耦网络和业务模块
* 配置nginx实现基于轮询的TCP负载均衡，实现聊天服务器集群，提高后端服务并发能力
* 基于Redis的发布-订阅服务器中间件，实现跨服务器的消息通信
* 基于Mysql实现数据落地存储并使用连接池提高存取性能

### USAGE
需要配置boost + muduo + redis + mysql + nginx + cmake环境
```
$ cd ./ChatServer/build/
$ cmake ..
$ make
```
### 数据库表设计 
#### User
|  <div style="width:100px">Field</div>   | <div style="width:150px">Attribute</div>  | <div style="width:220px">Constraint</div> |
|  ----  | ----  | ---- |
| id  | INT | PRIMARY KEY, AUTO_INCREMENT|
| name  | VARCHAR(50) | NOT NULL, UNIQUE  |
| password  | VARCHAR(50) | NOT NULL  |
| state  | ENUM('online', 'offline') | DEFAULT 'offline'  |

#### Friend
|  <div style="width:100px">Field</div>   | <div style="width:150px">Attribute</div>   | <div style="width:220px">Constraint</div> |
|  ----  | ----  | ---- |
| userid  | INT | NOT NULL, composite primary key|
| friendid  | INT | NOT NULL, composite primary key  |

#### AllGroup
|  <div style="width:100px">Field</div>   | <div style="width:150px">Attribute</div>   | <div style="width:220px">Constraint</div> |
|  ----  | ----  | ---- |
| id  | INT | PRIMARY KEY, AUTO_INCREMENT|
|groupname |VARCHAR(50)|NOT NULL, UNIQUE|
| groupdesc  | VARCHAR(50) | DEFAULT ''|

#### GroupUser
| <div style="width:100px">Field</div>   | <div style="width:150px">Attribute</div>   | <div style="width:220px">Constraint</div> |
|  ----  | ----  | ---- |
| groupid  | INT | NOT NULL, composite primary key|
| userid |INT|NOT NULL, composite primary key|
| grouprole  | ENUM('creator', 'normal')  | DEFAULT 'normal'|

#### OfflineMessage
|  <div style="width:100px">Field</div>   | <div style="width:150px">Attribute</div>   | <div style="width:220px">Constraint</div> |
|  ----  | ----  | ---- |
| userid  | INT | NOT NULL|
|message |VARCHAR(500)|NOT NULL|

