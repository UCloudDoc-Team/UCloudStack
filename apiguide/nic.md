# 8 弹性网卡

## 8.1 创建网卡-CreateNIC

创建网卡

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| VPCID          | string | 弹性网卡所属 VPC 的 ID              | **Yes**  |
| SubnetID       | string | 弹性网卡所属子网的 ID               | **Yes**  |
| Name           | string | 弹性网卡名称                        | **Yes**  |
| SGID           | string | 弹性网卡绑定的安全组 ID             | No       |
| IP             | string | 手动指定IP弹性网卡的 IP 地址        | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |
| NICID          | string | 创建的网卡 ID  | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateNIC
&Region=cn
&Zone=zone-01
&VPCID=eGLzpjdp
&SubnetID=lrUQEbLr
&Name=QDcnaDZM
&SGID=EiclDNmq
&IP=djLTzEfg
```

**Response Example**

```
{
    "Action": "CreateNICResponse", 
    "Message": "WEgwGtYY", 
    "NICID": "tJJHJAVS", 
    "RetCode": 0
}
```

## 8.2 获取网卡信息-DescribeNIC

获取网卡信息

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| NICIDs.N       | string | 【数组】网卡的 ID。输入有效的 ID。调用方式举例：NICIDs.0=“one-id”、NICIDs.1=“two-id”。 | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Message        | string | 返回信息描述。           | **Yes**  |
| TotalCount     | int    | 返回网卡总个数。         | **Yes**  |
| Infos          | array  | 【数组】返回网卡对象数组 | **Yes**  |

**NICInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域                                                         | **Yes**  |
| Zone           | string | 可用区                                                       | **Yes**  |
| VPCID          | string | 弹性网卡所属 VPC ID                                          | **Yes**  |
| SubnetID       | string | 弹性网卡所属子网 ID                                          | **Yes**  |
| MAC            | string | 弹性网卡的 MAC 地址                                          | **Yes**  |
| IP             | string | 弹性网卡的 IP 地址                                           | **Yes**  |
| SGID           | string | 安全组ID ，未绑定时为空                                      | **Yes**  |
| BindResourceID | string | 绑定资源ID，未绑定资源时为空                                 | **Yes**  |
| NICID          | string | 弹性网卡的 ID                                                | **Yes**  |
| Name           | string | 弹性网卡的名称                                               | **Yes**  |
| Remark         | string | 弹性网卡的备注                                               | **Yes**  |
| NICStatus      | string | 网卡状态。枚举值。Creating：创建中,Free：未绑定,Unbounding：解绑中,Bounding：绑定中,Bound：已绑定,BindSGing：绑定安全组中,UnbindSGing：解绑安全组中,UpdateSGing：更新安全组中,Deleting：删除中,Deleted：已删除,Releasing：销毁中,Released：已销毁 | **Yes**  |
| CreateTime     | int    | 创建时间。时间戳                                             | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeNIC
&Region=vydmIece
&Zone=jgsHLTZJ
&NICIDs.N=rhJPWxxc
&Offset=6
&Limit=9
```

**Response Example**

```
{
    "Action": "DescribeNICResponse", 
    "TotalCount": 1, 
    "Message": "xSoVyfgt", 
    "Infos": [
        {
            "Remark": "kqKEgRaf", 
            "VPCID": "ludsPYeb", 
            "NICID": "VMsqBJvZ", 
            "Name": "gLRMoJkF", 
            "Zone": "mKLvAcCG", 
            "IP": "vseXAZnD", 
            "Region": "yzkQCjpV", 
            "MAC": "CRQIjiTy", 
            "BindResourceID": "YkfGbjrn", 
            "NICStatus": "mTrfquCN", 
            "SubnetID": "NQUhbxaN", 
            "SGID": "OPHcCvKJ", 
            "CreateTime": 6
        }
    ], 
    "RetCode": 0
}
```

## 8.3 绑定弹性网卡-AttachNIC

绑定弹性网卡

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| NICID          | string | 网卡ID                              | **Yes**  |
| ResourceID     | string | 绑定的资源ID                        | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=AttachNIC
&Region=gLNTtZoP
&Zone=AOqlJokz
&NICID=nadzvDrO
&ResourceID=rAPpCitk
```

**Response Example**

```
{
    "Action": "AttachNICResponse", 
    "Message": "xhOyeeuB", 
    "RetCode": 0
}
```

## 8.4 解绑网卡-DetachNIC

解绑网卡

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| NICID          | string | 网卡ID                              | **Yes**  |
| ResourceID     | string | 绑定的资源ID                        | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DetachNIC
&Region=CuhLfpSN
&Zone=aXSsCJxc
&NICID=RfSMuUbr
&ResourceID=tVmqsYiA
```

**Response Example**

```
{
    "Action": "DetachNICResponse", 
    "Message": "gwrWTPtt", 
    "RetCode": 0
}
```

## 8.5 删除网卡-DeleteNIC

删除网卡

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| NICID          | string | 被删除的网卡 ID                     | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteNIC
&Region=NwJREXaT
&Zone=NWgkPWvC
&NICID=ZJABEbcC
```

**Response Example**

```
{
    "Action": "DeleteNICResponse", 
    "Message": "XZaFMquJ", 
    "RetCode": 0
}
```

