# 。好医生项目 API 设计说明
> **修订记录**
>
> | 版本   | 时间        | 人员   | 内容   |
> | ---- | --------- | ---- | ---- |
> | V1.0 | 2017.2.15 | 王凤鸣  | 创建文档 |

### 内容目录

- [1. 设计说明](#1-设计说明)
  - [1.1. 协议格式](#11-协议格式)
  - [1.2. URL格式](#12-url格式)
  - [1.3. 验证授权](#13-验证授权)
    - [1.3.1. `access_token`获取](#131-accesstoken获取)
    - [1.3.2. `access_token`更新](#132-accesstoken更新)
    - [1.3.3. `access_token`使用](#133-accesstoken使用)
  - [1.4. HTTP动词](#14-http动词)
  - [1.5. 响应状态码](#15-响应状态码)
  - [1.6. 错误处理](#16-错误处理)
  - [1.7. 功能接口](#17-功能接口)
    - [1.7.1. 首页](#171-首页)
      - [1.7.1.1. 获取名医问诊首页列表](#1711-获取名医问诊首页列表)
      - [1.7.1.2. 获取海外好医药首页列表](#1712-获取海外好医药首页列表)
    - [1.7.2. 海外医疗](#172-海外医疗)
      - [1.7.2.1. 获取医院列表](#1721-获取医院列表)
      - [1.7.2.1. 获取医院详情](#1722-获取医院详情)
    - [1.7.3. 名医问诊](#173-名医问诊)
      - [1.7.3.1. 名医问诊首页](#1731-名医问诊首页)
        - [1.7.3.1.1. 名医问诊首页列表](#17311-名医问诊首页列表)
        - [1.7.3.1.2. 名医问诊医生首页列表查询](#17312-名医问诊医生首页列表查询)
      - [1.7.3.2. 名医问诊医生列表](#1732-名医问诊医生列表)
      - [1.7.3.3. 名医问诊医生详情](#1733-名医问诊医生详情)
        - [1.7.3.3.1. 名医问诊医生基本信息](#17331-名医问诊医生基本信息)
        - [1.7.3.3.2. 名医问诊医生评价列表](#17332-名医问诊医生评价列表)
        - [1.7.3.3.3. 名医问诊医生评价统计](#17333-名医问诊医生评价统计)
      - [1.7.3.4. 预约问诊](#1734-预约问诊)
        - [1.7.3.4.1. 查询预约问诊的时间段](#17341-查询预约问诊的时间段)
        - [1.7.3.4.2. 提交订单](#17342-提交订单)
      - [1.7.3.5. 订单支付](#1735-订单支付)
      - [1.7.3.6. 上传病例](#1736-上传病例)
    - [1.7.4. 海外好医药](#174-海外好医药)
      - [1.7.4.1. 海外好医药栏目首页](#1741-海外好医药栏目首页) 
      - [1.7.4.2. 药品详情](#1742-药品详情) 
      - [1.7.4.3. 确认订单](#1743-确认订单) 
      - [1.7.4.1. 付款](#1741-付款) 
    - [1.7.5. 健康体验站](#175-健康体验站)
      - [1.7.5.1. 获取附近健康体验站数据](#1751-获取附近健康体验站数据)
      - [1.7.5.2. 会所详情](#1752-会所详情)
      - [1.7.5.3. 过往评价](#1753-过往评价)
        - [1.7.5.3.1. 过往评价列表](#17531-过往评价列表)
        - [1.7.5.3.2. 过往评价统计](#17532-过往评价统计)
      - [1.7.5.4. 在线预约页面](#1754-在线预约页面)
        - [1.7.5.4.1. 在线预约页面](#17541-在线预约页面)
        - [1.7.5.4.2. 提交预约](#17542-提交预约)
    - [1.7.6. 个人中心](#176-个人中心)
      - [1.7.6.1. 个人中心首页](#1761-个人中心首页)
      - [1.7.6.2. 消息中心](#1762-消息中心)
      - [1.7.6.3. 消息详情](#1763-消息详情)
      - [1.7.6.4. 基础资料](#1764-基础资料)
        - [1.7.6.4.1. 基础资料修改](#17641-基础资料修改)
      - [1.7.6.5. 我的预约](#1765-我的预约)
        - [1.7.6.5.1. 名医就诊预约](#17651-名医就诊预约)
        - [1.7.6.5.2. 体验店预约](#17652-体验店预约)
      - [1.7.6.6. 我的预约历史记录](#1766-我的预约历史记录)
      - [1.7.6.7. 我的订单](#1767-我的订单)
      - [1.7.6.8. 健康履历](#1768-健康履历)
      - [1.7.6.9. 我的病例](#1769-我的病例)
        - [1.7.6.9.1. 健康档案](#17691-健康档案)
        - [1.7.6.9.2. 诊断记录](#17692-诊断记录)
      - [1.7.6.10. 意见反馈](#17610-意见反馈)
    - [1.7.7. 注册](#177-注册)
    - [1.7.8. 登录](#178-登录)

      ​

<a name="1-设计说明"></a>
## 1. 设计说明

<a name="11-协议格式"></a>
### 1.1. 协议格式

`好医生项目 API`使用`HTTP`或`HTTPS`协议，消息内容使用`JSON`格式。`HTTP`头中应包含：

```
Content-Type: "application/json"
```

<a name="12-url格式"></a>
### 1.2. URL格式
`好医生项目 API`的通用URL格式为：

    http(s)://<server_address>:<server_port>/[engine|front|console]/api/...

例如：

```
http://xxx.xxxxx.xxx/engine/api/direct_tasks
https://xxx.xxxx.xxx:80/console/api/users
```

<a name="13-验证授权"></a>
### 1.3. 验证授权

`Ftrans REST API`采用`OAuth 2.0`的“密码模式” (resource owner password credentials) 对第三方应用进行授权。

```
 +----------+
 | Resource |
 |  Owner   |
 |          |
 +----------+
      v
      |    Resource Owner
     (A) Password Credentials
      |
      v
 +---------+                                  +---------------+
 |         |>--(B)---- Resource Owner ------->|               |
 |         |         Password Credentials     | Authorization |
 | Client  |                                  |     Server    |
 |         |<--(C)---- Access Token ---------<|               |
 |         |    (w/ Optional Refresh Token)   |               |
 +---------+                                  +---------------+
```

<a name="131-accesstoken获取"></a>
#### 1.3.1. `access_token`获取

客户端发出的`HTTP`请求，请求类型为`POST`，请求头中包含：

```
Content-Type: application/x-www-form-urlencoded
```

消息体中包含以下参数：

* `grant_type`：表示授权类型，此处的值固定为"`password`"，必选项。
* `username`：表示用户名，必选项。
* `password`：表示用户的密码，必选项。
* `scope`：表示权限范围，可选项。

例如：
```http
POST /token HTTP/1.1
Host: server.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=password&username=johndoe&password=A3ddj3w
```

服务器返回的报文，例如：

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "token_type": "bearer",
  "expires_in": 3600,
  "refresh_token": "tGzv3JOkF0XG5Qx2TlKWIA"
}
```
<a name="132-accesstoken更新"></a>
#### 1.3.2. `access_token`更新

客户端发出的`HTTP`请求，请求类型为`POST`，请求头中包含：

```
Content-Type: application/x-www-form-urlencoded
```

消息体中包含以下参数：

* `grant_type`：表示授权类型，此处的值固定为"`refresh_token`"，必选项。
* `refresh_token`：表示之前获取`access_token`时获取到的`refresh_token`，必选项。

例如：
```http
POST /token HTTP/1.1
Host: server.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA
```

服务器返回的报文，例如：

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "token_type": "bearer",
  "expires_in": 3600,
  "refresh_token": "tGzv3JOkF0XG5Qx2TlKWIA"
}
```

<a name="133-accesstoken使用"></a>
#### 1.3.3. `access_token`使用

在请求需要授权验证的API时，在请求头加入：

```
Authorization: Bearer [access_token]
```

<a name="14-http动词"></a>
### 1.4. HTTP动词

* 考虑到调用`Ftrans REST API`的程序编写的方便性，只选取采用`GET`和`POST`两种HTTP访问方式。

>常用的HTTP动词有下面五个（括号里是对应的SQL命令）。
>
* `GET`（SELECT）：从服务器取出资源（一项或多项）。
* `POST`（CREATE）：在服务器新建一个资源。
* `PUT`（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
* `PATCH`（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
* `DELETE`（DELETE）：从服务器删除资源。

<a name="15-响应状态码"></a>
### 1.5. 响应状态码

* 最终决定只要HTTP请求能够访问通过，无论是增删改查操作还是应用程序处理错误都返回状态码`200`。

>
* `2xx` - 成功
    - `200` OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
    - `201` CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
    - `204` NO CONTENT - [DELETE]：用户删除数据成功。
* `3xx`  重定向
    - `301` 已移动 — 请求的数据具有新的位置且更改是永久的。 
* `4xx` - 客户机中出现的错误
    - `400` INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
    - `401` Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
    - `403` Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
    - `404` NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
    - `406` Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
    - `410` Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
    - `422` Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
* `5xx` - 服务器中出现的错误
    - `500` INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

<a name="16-错误处理"></a>
### 1.6. 错误处理

如果`HTTP`请求错误时，就应该向客户端返回错误信息。返回的信息中包含错误码和错误信息。
例如：

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
  "error_code" : 50000,
  "error_msg" : "请求处理的业务逻辑发生错误"
}
```

调用接口发生错误时，开发者可以根据错误码信息排查错误。

`好医生项目 API`全局错误码如下：

| 错误码     | 说明                      |
| ------- | ----------------------- |
| `-1`    | 系统错误                    |
| `40001` | `access_token`无效        |
| `40002` | `transfer_token`无效（暂不用） |
| `40003` | 请求对象不存在                 |
| `40004` | 数据创建或修改失败               |
| `40005` | 数据删除失败                  |
| `40007` | 用户名密码验证错误               |
| `41000` | 请求的应用级输入参数错误            |
| `50000` | 请求处理的业务逻辑发生错误           |

<a name="17-功能接口"></a>
### 1.7. 功能接口

<a name="171-首页"></a>
#### 1.7.1. 首页

<a name="1711-获取名医问诊列表"></a>
#####1.7.1.1. 获取名医问诊首页列表

* 请求头：

```http
GET /api/Doctor/IndexList
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"DoctorName": "张三",
	"Department": "脑科",
	"Position": "主任医师",
	"Hospital": "XXXXXXXX医院",
	"Photo": "xxxx/xxxxx/xxxx.jpg",
  	"Cost":300,
	"Tags":[{
      "Tag": "头痛"
	},
	{
      "Tag": "心绞痛"
	}]
},...
]
```

<a name="1712-获取海外好医药首页列表"></a>
#####1.7.1.2. 获取海外好医药首页列表

* 请求头：

```http
GET /api/Medicine/IndexList
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"MedicineName": "吗丁啉",
	"Description": "适用人群：XXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
	"Picture": "xxxx/xxxxx/xxxx.jpg",
  	"Price":300
},...
]
```

<a name="172-海外医疗"></a>
#### 1.7.2. 海外医疗 

<a name="1721-获取医院列表"></a>
#####1.7.2.1. 获取医院列表

* 请求头：

```http
GET /api/Hospital/List
Accept: application/json
```

* 请求体：
```json
{
	"Category":"治疗"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"HospitalName": "XXXX医院",
	"Picture": "xxxx/xxxxx/xxxx.jpg"
},...]
```

<a name="1722-获取医院详情"></a>
#####1.7.2.2. 获取医院详情

* 请求头：

```http
GET /api/Hospital/Get/<id>
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"HospitalName": "XXXX医院",
	"Route": "地点：xxxxxxx，乘坐x路公交车至xxxx站下。",
	"Notice": "用户须知",
	"Picture": "xxxx/xxxxx/xxxx.jpg",
	"Telephone":"025888888888",
	"MedicalAdvisorId":"5a77720b-12ef-c32c-1a0b-39d5b1adc577",
	"MedicalAdvisorUserName":"18912341524"
}
```

<a name="173-名医问诊"></a>
#### 1.7.3. 名医问诊 

<a name="1731-名医问诊首页"></a>
#####1.7.3.1. 名医问诊首页

<a name="17311-名医问诊首页列表"></a>
######1.7.3.1.1. 名医问诊首页列表
* 请求头：

```http
GET /api/Department/List
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"DepartmentName": "儿科",
	"Icon": "xxx/xxxx/xxxx.png"
},...]
```
<a name="17312-名医问诊医生首页列表查询"></a>
######1.7.3.1.2. 名医问诊医生首页列表查询
* 请求头：

```http
GET /api/Doctor/Search
Accept: application/json
```

* 请求体：

```json
{
  "KeyWord":"张三"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"DoctorName": "张三",
	"Department": "脑科",
	"Position": "主任医师",
	"Hospital": "XXXXXXXX医院",
	"Tags":[{
      "Tag": "头痛"
	},
	{
      "Tag": "心绞痛"
	}]
},...
]
```
<a name="1732-名医问诊医生列表"></a>
#####1.7.3.2. 名医问诊医生列表
* 请求头：

```http
GET /api/Doctor/List
Accept: application/json
```

* 请求体：

```json
{
  "Category":"国内专家"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"DoctorName": "张三",
	"Department": "脑科",
	"Position": "主任医师",
	"Hospital": "XXXXXXXX医院",
	"Photo": "xxxx/xxxxx/xxxx.jpg",
	"Tags":[{
      "Tag": "头痛"
	},
	{
      "Tag": "心绞痛"
	}]
},...
]
```

<a name="1733-名医问诊医生详情"></a>
#####1.7.3.3. 名医问诊医生详情

<a name="17331-名医问诊医生基本信息"></a>
######1.7.3.3.1. 名医问诊医生基本信息
* 请求头：

```http
GET /api/Doctor/Get/<id>
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"DoctorName": "张三",
	"Department": "脑科",
	"Position": "主任医师",
	"Hospital": "XXXXXXXX医院",
	"Photo": "xxxx/xxxxx/xxxx.jpg",
	"Introduce":"xxxxxxxxxxxx",
	"Cost":300,
	"Tags":[{
      "Tag": "头痛"
	},
	{
      "Tag": "心绞痛"
	}]
}
```

<a name="17332-名医问诊医生评价列表"></a>
######1.7.3.3.2. 名医问诊医生评价列表

* 请求头：

```http
GET /api/DoctorAppraise/Page
Accept: application/json
```

* 请求体：
```json
{
	"DoctorId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"PageSize": 5,
	"PageIndex": 1
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"UserName": "189*****234",
	"AppraiseTime": "2017年1月2日",
	"Content": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
	"Satisfaction":3
},...]
```

<a name="17333-名医问诊医生评价统计"></a>
######1.7.3.3.3. 名医问诊医生评价统计

* 请求头：

```http
GET /api/DoctorAppraise/statistic?doctorId=<doctorId>
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"Total":100,
	"VerySatisfied": 50,
	"Satisfied": 30,
	"Normal": 20
}
```

<a name="1734-预约问诊"></a>
#####1.7.3.4. 预约问诊

<a name="17341-查询预约问诊的时间段"></a>
######1.7.3.4.1. 查询预约问诊的时间段

* 请求头：

```http
GET /api/DoctorOrder/GetOrderDate?doctorId=<doctorId>
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"AvailableDate":["2017-01-02","2017-01-03","2017-01-04","2017-01-05","2017-01-06","2017-01-07","2017-01-08"....],
	"AvailableTime":["09:00","09:30","10:00","10:30","11:00","11:30"....]
}
```


<a name="17342-提交订单"></a>
######1.7.3.4.2. 提交订单
* 请求头：

```http
POST /api/DoctorOrder/Create
Authorization: Bearer [access_token]
Content-Type: application/json
```

* 请求体：
```json
{
	"UserId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"DoctorId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"ReserveData":"2017-01-15",
	"StarTime":"13:00",
	"EndTime":"13:30",
	"Cost":300
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```

* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"预约问诊订单提交成功。"
}
```

<a name="1735-订单支付"></a>
#####1.7.3.5. 订单支付
* 请求头：

```http
PATCH /api/DoctorOrder/Update
Authorization: Bearer [access_token]
Content-Type: application/json
```

* 请求体：
```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"OrderState": 0
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```

* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"预约问诊订单状态修改成功。"
}
```

<a name="1736-上传病例"></a>
#####1.7.3.6. 上传病例
* 请求头：

```http
PATCH /api/DoctorOrder/Update
Authorization: Bearer [access_token]
Content-Type: application/json
```

* 请求体：
```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"Pictures":["xxx/xxx.jpg","xxx/xxx.jpg","xxx/xxx.jpg"],
	"IllnessDescription":"头痛，偏头痛"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```

* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"病例资料上传成功。"
}
```

<a name="174-海外好医药"></a>
####1.7.4. 海外好医药

<a name="1741-海外好医药栏目首页"></a>
#####1.7.4.1. 海外好医药栏目首页

* 请求头：

```http
GET /api/Medicine/List
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"MedicineName": "吗丁啉",
	"Picture": "xxxx/xxxxx/xxxx.jpg"
},...
]
```

<a name="1742-药品详情"></a>
#####1.7.4.2. 药品详情

* 请求头：

```http
GET /api/Medicine/Get/<id>
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"MedicineName": "吗丁啉",
	"Picture": "xxxx/xxxxx/xxxx.jpg",
	"Price":300,
	"Description":"xxxxxxxxxxxxxxxxxxxxx",
	"DispatchPlace":"日本",
	"Details":"xxxx/xxxxx/xxxx.jpg",
	"Telephone":"02512345678"
}
```

<a name="1743-确认订单"></a>
#####1.7.4.3. 确认订单

* 请求头：

```http
POST /api/MedicineOrder/Create
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：
```json
{
	"UserId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"MedicineId":"5648C3AE-E144-45EB-9A76-D0D6DF4F5598",
  	"OrderNum":"102351257452221"
	"Consignee": "李四",
	"Phone": "13812457894",
	"Address":"xxx市xxx区xxx号",
	"Count":2,
	"Message":"",
	"Details":"xxxx/xxxxx/xxxx.jpg",
	"Telephone":"02512345678",
	"UnitPrice":300,
	"TotalPrice":600
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"药品订单提交成功。"
}
```

<a name="1744-付款"></a>
#####1.7.4.4. 付款
* 请求头：

```http
PATCH /api/MedicineOrder/Update
Authorization: Bearer [access_token]
Content-Type: application/json
```

* 请求体：
```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"OrderState": 0
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```

* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"药品订单状态修改成功。"
}
```

<a name="175-健康体验站"></a>
####1.7.5 健康体验站

<a name="1751-获取附近健康体验站数据"></a>
#####1.7.5.1. 获取附近健康体验站数据

* 请求头：

```http
GET /api/Club/List
Accept: application/json
```

* 请求体：
```json
{
	"Distance": 2000,
	"Longitude": 36.5,
	"Latitude": 43.7,
	"ClubName": "xxx"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"ClubName": "xxxxx会所",
	"Picture": "xxxx/xxxxx/xxxx.jpg",
	"Address": "xxxx市xxxx区xxxx号",
	"Telephone": "02512345786",
	"Longitude": 56.7,
	"Latitude": 78.5
},...]
```

<a name="1752-会所详情"></a>
#####1.7.5.2. 会所详情

* 请求头：

```http
GET /api/Club/Get/<id>
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"ClubName": "xxxxx会所",
	"Picture": "xxxx/xxxxx/xxxx.jpg",
	"Address": "xxxx市xxxx区xxxx号",
	"Telephone": "02512345786",
	"ClubService":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
	"Project":["血压测试","康复治疗"]
}
```

<a name="1753-过往评价"></a>
#####1.7.5.3. 过往评价

<a name="17531-过往评价列表"></a>
######1.7.5.3.1. 过往评价列表

* 请求头：

```http
GET /api/ClubAppraise/Page
Accept: application/json
```

* 请求体：
```json
{
	"ClubId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"PageSize": 5,
	"PageIndex": 1
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"UserName": "189*****234",
	"AppraiseTime": "2017年1月2日",
	"Content": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
	"Satisfaction":3
},...]
```

<a name="17532-过往评价统计"></a>
######1.7.5.3.2. 过往评价统计


* 请求头：

```http
GET /api/ClubAppraise/statistic?ClubId=<clubId>
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"Total":100,
	"VerySatisfied": 50,
	"Satisfied": 30,
	"Normal": 20
}
```

<a name="1754-在线预约页面"></a>
#####1.7.5.4. 在线预约页面

<a name="17542-提交预约"></a>
######1.7.5.4.2. 提交预约

* 请求头：

```http
POST /api/ClubOrder/Create
Authorization: Bearer [access_token]
Content-Type: application/json
```

* 请求体：
```json
{
	"UserId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"ClubId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"ReserveData":"2017-01-15",
	"ReserveProject":"血压测试",
	"ReserveCount":3
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```

* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"预约问诊订单提交成功。"
}
```

<a name="176-个人中心"></a>
####1.7.6. 个人中心

<a name="1761-个人中心首页"></a>
#####1.7.6.1. 个人中心首页

* 请求头：

```http
GET /api/User/Info?userId=<userId>
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"Name":"张三",
	"UserName": "13512345678",
	"Gender":"男",
	"BloodType":"AB",
	"Stature":178
}
```

<a name="1762-消息中心"></a>
#####1.7.6.2. 消息中心

* 请求头：

```http
GET /api/User/InfoList?userId=<userId>
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"Title":"注册成功",
	"Type": "系统通知",
	"IsNew":True
},...]
```

<a name="1763-消息详情"></a>
#####1.7.6.3. 消息详情


* 请求头：

```http
GET /api/User/Info/<id>
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"Title":"注册成功",
	"Type": "系统通知",
	"Content":"XXXXXXXXXXXXXXXXXXXX",
	"CreateTime":"2017-02-02 12:12:58"
	"IsNew":True
}
```


<a name="1764-基础资料"></a>
#####1.7.6.4. 基础资料

<a name="17641-基础资料修改"></a>
######1.7.6.4.1. 基础资料修改
* 请求头：

```http
PATCH /api/User/Update
Authorization: Bearer [access_token]
Content-Type: application/json
```

* 请求体：
```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"Name":"张三",
	"Gender":"男",
	"BloodType":"AB",
	"Stature":178
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```

* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"基础信息修改成功。"
}
```

<a name="1765-我的预约"></a>
#####1.7.6.5. 我的预约

<a name="17651-名医就诊预约"></a>
######1.7.6.5.1. 名医就诊预约

* 请求头：

```http
GET /api/DoctorOrder/UserOrderList?userId=<id>
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"DoctorName": "张三",
	"Department": "脑科",
	"Position": "主任医师",
	"Hospital": "XXXXXXXX医院",
	"Photo": "xxxx/xxxxx/xxxx.jpg",
	"OrderState": 0
},...]
```

<a name="17652-体验店预约"></a>
######1.7.6.5.2. 体验店预约

* 请求头：

```http
GET /api/ClubOrder/UserOrderList?userId=<id>
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"ClubName": "xxxxx会所",
	"Picture": "xxxx/xxxxx/xxxx.jpg",
	"Address": "xxxx市xxxx区xxxx号",
	"Telephone": "02512345786",
	"OrderState": 0
},...]
```

<a name="1766-我的预约历史记录"></a>
#####1.7.6.6. 我的预约历史记录


* 请求头：
  要调用两个接口
```http
GET /api/DoctorOrder/UserHistroyOrderList
Authorization: Bearer [access_token]
Accept: application/json
```

```http
GET /api/ClubOrder/UserHistroyOrderList
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：
```json
{
	"UserId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"OrderState": 0 // 已结束状态
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：
  同`1.7.6.5. 我的预约`接口的响应体


<a name="1767-我的订单"></a>
#####1.7.6.7. 我的订单

* 请求头：

```http
GET /api/MedicineOrder/UserOrderList?userId=<id>
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"MedicineName": "吗丁啉",
	"Picture": "xxxx/xxxxx/xxxx.jpg",	
  	"OrderNum":"102351257452221",
	"UnitPrice":300,
	"TotalPrice":600
	"Count":2,
	"OrderState": 0
},...]
```

<a name="1768-健康履历"></a>
#####1.7.6.8. 健康履历

* 请求头：

```http
GET /api/User/HealthRecordR?userId=<UserId>
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"ClubName": "xxxxx会所",
	"CreateTime": "2017-02-05 12:23:47",	
  	"Content":"XXXXXXXXXXXXXXXXXXXXXXXX"
},...]
```

<a name="1769-我的病例"></a>
#####1.7.6.9. 我的病例
<a name="17691-健康档案"></a>
######1.7.6.9.1. 健康档案
* 请求头：

```http
PATCH /api/User/Update
Authorization: Bearer [access_token]
Content-Type: application/json
```

* 请求体：
```json
{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"BloodPressure":"120",
	"Age":45,
	"MedicalHistory":"XXXXXXXXXXXXXXXXXXXXXX",
	"GeneticMedicalHistory":"XXXXXXXXXXXXXXXXXXXXXX"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```

* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"基础信息修改成功。"
}
```


<a name="17692-诊断记录"></a>
######1.7.6.9.2. 诊断记录

* 请求头：

```http
GET /api/User/DiagnoseRecordR?userId=<UserId>
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：无

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
[{
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"DoctorName": "张三",
	"CreateTime": "2017-02-05 12:23:47",	
  	"DiagnoseContent":"XXXXXXXXXXXXXXXXXXXXXXXX"
},...]
```

<a name="17610-意见反馈"></a>
#####1.7.6.10. 意见反馈


* 请求头：

```http
POST /api/User/Advice
Authorization: Bearer [access_token]
Accept: application/json
```

* 请求体：
```json
{
	"UserId":"099b018e-9e1b-226b-108c-39d5ae517356",
	"AdviceContent":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"意见提交成功。"
}
```

<a name="177-注册"></a>
####1.7.7. 注册

```http
POST /api/User/Register
Accept: application/json
```

* 请求体：
```json
{
	"UserName":"18945781268",
	"Name":"18945781268",
	"Password":"5578"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"注册成功。"
}
```

<a name="177-登录"></a>
####1.7.7. 登录

```http
POST /api/User/Login
Accept: application/json
```

* 请求体：
```json
{
	"UserName":"18945781268",
	"Password":"5578"
}
```

* 响应头：

```http
200 OK
Content-Type: application/json
```
* 响应体：

```json
{
	"IsSuccess":True,
	"Message":"登录成功。",
	"Id":"099b018e-9e1b-226b-108c-39d5ae517356",
	"UserName":"18945781268",
	"Name":"18945781268",
  	"access_token": "2YotnFZFEjr1zCsicMWpAA",
  	"token_type": "bearer",
  	"expires_in": 3600
}
```
