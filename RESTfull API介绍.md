# RESTfull API介绍


REST 代表表现层状态转移（REpresentational State Transfer），由 Roy Fielding 在他的 论文 中提出。REST 是一种软件架构风格，不是技术框架，REST 有一系列规范，满足这些规范的 API 均可称为 RESTful API。REST 规范中有如下几个核心：

* REST 中一切实体都被抽象成资源，每个资源有一个唯一的标识 —— URI，所有的行为都应该是在资源上的 CRUD 操作；
* 使用标准的方法来更改资源的状态，常见的操作有：资源的增删改查操作；
* 无状态：这里的无状态是指每个 RESTful API 请求都包含了所有足够完成本次操作的信息，服务器端无须保持 Session。

REST 风格虽然适用于很多传输协议，但在实际开发中，REST 由于天生和 HTTP 协议相辅相成，因此 HTTP 协议已经成了实现 RESTful API 事实上的标准。在 HTTP 协议中通过 POST、DELETE、PUT、GET 方法来对应 REST 资源的增、删、改、查操作，具体的对应关系如下：


| HTTP方法   |      行为      |  URI | 示例说明 |
|----------|-------------|------|------|
|  GET | 获取资源列表  |  /users |  获取用户列表 |
|  GET | 获取一个具体的资源	  |  /users/admin |  获取 admin 用户的详细信息 |
|  POST | 创建一个新的资源  |  /users |  创建一个新用户 |
|  PUT | 以整体的方式更新一个资源  |  /users/1 |  更新 id 为 1 的用户 |
|  DELETE | 删除服务器上的一个资源  |  /users/1 |  删除 id 为 1 的用户 |


