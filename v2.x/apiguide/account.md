# 12 账户管理

## 12.1 管理员添加账号-CreateUser

管理员添加账号，即创建租户（主账号）。

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| UserEmail      | string | 账号邮箱。  | **Yes**  |
| PassWord       | string | 账号密码。  | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |
| UserID         | int    | 账户ID         | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateUser
&UserEmail=ebvUGGyb
&PassWord=VHPdVijw
```

**Response Example**

```
{
    "Action": "CreateUserResponse", 
    "Message": "QYsSiqSC", 
    "UserID": "qXGiAIgk", 
    "RetCode": 0
}
```

## 12.2 查询租户信息-DescribeUser

查询 UCloudStack 租户信息。

**Request Parameters**

| Parameter name | Type | Description                                                  | Required |
| -------------- | ---- | ------------------------------------------------------------ | -------- |
| Offset         | int  | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int  | 返回数据长度，默认为20，最大100。                            | No       |
| UserIDs.N      | int  | 【数组】租户的 ID。输入有效的 ID。调用方式举例：UserIDs.0=123”、UserIDs.1=456 | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Message        | string | 返回信息描述             | **Yes**  |
| TotalCount     | int    | 返回现有租户总数         | **Yes**  |
| Infos          | array  | 【数组】返回租户对象数组 | **Yes**  |

**UserInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| UserID         | int    | 租户ID.                                                      | No       |
| Email          | string | 租户名称                                                     | No       |
| PublicKey      | string | 公钥                                                         | No       |
| PrivateKey     | string | 私钥                                                         | No       |
| CreateTime     | int    | 账户创建时间。时间戳                                         | No       |
| UpdateTime     | int    | 更新时间。时间戳                                             | No       |
| Amount         | float  | 账户余额                                                     | No       |
| Status         | string | 用户状态。USER_STATUS_AVAILABLE：正常，USER_STATUS_FREEZE：冻结 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeUser
&Offset=mQpwrmYK
&Limit=HLrzzDfb
&UserIDs=QvIQBBgg
```

**Response Example**

```
{
    "Action": "DescribeUserResponse", 
    "TotalCount": 8, 
    "Message": "pnbYmdNw", 
    "Infos": [
        "uEzXfQUU"
    ], 
    "RetCode": 0
}
```

## 12.3 管理员给租户充值-Recharge

UCloudStack 管理员给租户充值金额。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| UserID         | int    | 租户的账户ID。                                               | **Yes**  |
| Amount         | int    | 充值金额。最少100,最大500000                                 | **Yes**  |
| FromType       | string | 充值来源。INPOUR_FROM_ALIPAY：支付宝，INPOUR_FROM_OFFLINE：银行转账，INPOUR_FROM_SINPAY：新浪支付，INPOUR_FROM_WECHAT_PAY：微信转账。 | **Yes**  |
| SerialNo       | string | 充值单号。充值方式为“账户余额”时为必要参数。                 | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=Recharge
&UserID=9
&RechargeType=1
&Amount=4
&FromType=INPOUR_FROM_ALIPAY;INPOUR_FROM_OFFLINE;INPOUR_FROM_SINPAY;INPOUR_FROM_WECHAT_PAY
&SerialNo=xSvXhxgK
```

**Response Example**

```
{
    "Action": "RechargeResponse", 
    "Message": "LvzORptu", 
    "RetCode": 0
}
```

