## 概览

### 设计规范
* [基本设计原则](https://github.com/ZhangBohan/http-api-design-ZH_CN)
* [path 中的单词用中划线（-）隔开](http://blog.restcase.com/5-basic-rest-api-design-guidelines/)
* [最小化路径嵌套](https://github.com/ZhangBohan/http-api-design-ZH_CN#最小化路径嵌套)
  * 方便实现跨资源的整合接口（如全局统计、全局日志）；
  * url 设计规范：
      * **1:1**：`/collection/host_id`
      * **1:N**：`/collection/host_id/item_id`
      * **N:N**：`/collection/item_id`
* [状态码](https://github.com/ZhangBohan/http-api-design-ZH_CN#返回合适的状态码)
* [嵌套外键关系](https://github.com/ZhangBohan/http-api-design-ZH_CN#嵌套外键关系)
* 跨资源的过滤操作单独提供搜索接口

## 获取资源

使用 `GET` 方法获取资源：

* `GET /articles HTTP/1.1` 获取集合资源，返回资源集合数组 `[{}]`；
* `GET /articles/1 HTTP/1.1` 获取单项资源，返回单项资源对象 `{}`；
* 集合资源请求需返回 `X-Total-Count` header，表示此集合的所有条目数量；
* 支持 `HEAD` 请求，可实现获取当前集合资源数量的功能，`HEAD` 请求返回状态 [204](https://httpstatuses.com/204)。
* 对于集合资源，以下参数为特殊用途，其他的默认都认为是过滤行为：
  * `page`、`size`、`start` 用于分页；
  * `sort` 用于排序；
  * `group` 用于分组。

### 排序

使用 `sort` 关键字进行排序：

```http
GET /cars?sort[]=-manufacturer&sort[]=+model
```

### 分页

使用 `page` 关键字进行分页：

```http
GET /cars?page=2
```

### 过滤

支持条件查找：

* `GET /cars?color=red` 颜色是红色的车型；
* `GET /cars?seats>=5` 大于5座的车型；
* `GET /cars?color!=red` 颜色不是红色的车型；

多个过滤条件之间是『与』的关系，同一个过滤条件的多次调用之间是『或』的关系。

* `GET /cars?color[]=red&color[]=blue` 所有红色和蓝色的车型；
* `GET /cars?color=red&seats>=5` 所有红色且大于5座的车型；

## 更新资源

### 创建

* 资源已存在返回状态码 [409](https://httpstatuses.com/409)；
* 同步创建资源接口调用成功后返回状态码 [201](https://httpstatuses.com/201)，且包含 `Location` header；
* 异步创建资源接口调用成功后返回状态码 [202](https://httpstatuses.com/202)，且包含 `X-Task-Id` header；

#### 通过向集合资源发送 `POST` 请求创建资源

```http
POST /articles
 
{
  "title": "My Ariticle"
}
```

#### 通过向目标单向资源地址发送 `PUT` 请求创建资源

```http
PUT /articles/my-article
 
{
  "title": "My Ariticle"
}
```

### 更新

* 同步更新资源接口调用成功后返回状态码 [200](https://httpstatuses.com/200)；
* 异步更新资源接口调用成功后返回状态码 [202](https://httpstatuses.com/202)，且包含 `X-Task-Id` header；
* `PATCH` 功能实际是覆盖 `PUT` 的，实现时可选择不实现 `PUT`；

#### 通过向单项资源发送 `PUT` 请求更新资源

替换整个资源：

```http
PUT /articles/my-article

{
  "title": "My Article"
}
```

#### 通过向单项资源发送 `PATCH` 请求更新资源

更新发送资源所包含的字段：

```http
PATCH /articles/my-article

{
  "title": "My Article"
}
```

### 删除

* 通过向单项资源发送 `DELETE` 请求删除资源；
* 删除后接口应该返回刚刚删除的资源内容；

## 批量操作

* 批量操作可归纳为两种方式：
  1. 基于固定过滤条件的批量操作，如 `DELETE /apps?status=5`；
  2. 基于 id 过滤的批量操作，如 `DELETE /apps?id[]=1&id[]=2&id[]=3`，此方式实现更加灵活，但会遇到 url 长度过长的问题，并不能无限长度的批量；

| /collection | 无参数   | 基于 id 的过滤 | 基于 filter 的过滤 |
| ----------- | ----- | --------- | ------------- |
| DELETE      | ×（危险） | ✓         | ✓             |
| PUT []      | ×（危险） | ✓         | ×（危险）         |
| PUT {}      | ×（危险） | ✓         | ✓             |
| PATCH []    | ×     | ✓         | ×（危险）         |
| PATCH {}    | ×（危险） | ✓         | ✓             |
| POST []     | ✓     | ×         | ×             |
| POST {}     | ✓     | ×         | ✓             |


## 典型场景

### 任务接口

* 异步接口可以通过任务接口来查询异步任务的进程；
* 通过异步接口的 response header：`X-Task-Id` 查询任务状态。

### 文件操作

| 操作     | 示例              | 备注   |
| ------ | --------------- | ---- |
| 上传文件   | POST /files     |      |
| 下载文件   | GET /files/1    |      |
| 获取文件信息 | HEAD /files/1   |      |
| 删除文件   | DELETE /files/1 |      |

### 登录、验权相关接口设计

* 将”登陆“看做资源

| 操作   | 示例             |
| ---- | -------------- |
| 注册   | POST /register |
| 登陆   | POST /login    |
| 退出   | delete /logout |

* 需要 User-Agent header 否则 `403 Forbidden`
* 登陆失败：

```http
401 Unauthorized
{
  "message": "Bad credentials",
  "documentation_url": "https://developer.github.com/v3"
}
```
* 某段时间登陆次数超过极限，暂时屏蔽所有登陆操作：

```http
403 Forbidden:
{
  "message": "Maximum number of login attempts exceeded. Please try again later.",
  "documentation_url": "https://developer.github.com/v3"
}
```

## 参考资料

* http://docs.upyun.com/api/rest_api/#_4
* [https://developer.github.com/v3/#user-agent-required](https://developer.github.com/v3/#user-agent-required)
* [best-practices-for-a-pragmatic-restful-api](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)
