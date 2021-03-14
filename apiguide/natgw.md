# 11 NAT 网关

## 11.1 创建NAT网关-CreateNATGW

创建NAT网关

**Request Parameters**

| Parameter name | Type   | Description     | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| Name           | string | 名称。                                                       | **Yes**  |
| VMType         | string | 运行NAT网关实例的主机机型。枚举值：如 Normal ，表示普通机型； SSD，表示 SSD 机型。（机型由平台管理员修改和指定，可参考获取主机机型接口） | **Yes**  |
| VPCID          | string | NAT网关实例所在的 VPC ID                                     | **Yes**  |
| SubnetID       | string | NAT网关实例所在的子网 ID                                     | **Yes**  |
| EIPID          | string | 外网IP的ID                                                   | **Yes**  |
| SGID           | string | 安全组ID                                                     | **Yes**  |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Quantity       | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |
| Remark         | string | 描述                                                         | No       |

**Response Elements**

| Parameter name | Type   | Description         | Required |
| -------------- | ------ | ------------------- | -------- |
| RetCode        | int    | 返回码              | **Yes**  |
| Action         | string | 操作名称            | **Yes**  |
| Message        | string | 返回信息描述。      | No       |
| NATGWID        | string | 返回创建的NAT网关ID | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateNATGW
&Region=cn
&Zone=zone-01
&Name=mbcmxKyM
&VMType=Normal
&VPCID=IWIfWzod
&SubnetID=kDnGgjdk
&EIPID=HKbzEhjO
&SGID=JllyMtTF
&ChargeType=Dynamic
&Quantity=9
&Remark=gysHBfQx
```

**Response Example**

```
{
    "Action": "CreateNATGWResponse", 
    "Message": "UxMlMOBh", 
    "RetCode": 0, 
    "NATGWID": "jrLxSMfw"
}
```

## 11.2 获取NAT网关信息 -DescribeNATGW

获取NAT网关信息

**Request Parameters**

| Parameter name | Type   | Description                 | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| NATGWIDs.N     | string | 【数组】NAT网关的 ID。调用方式举例：NATGWIDs.0=“one-id”、NATGWIDs.1=“two-id”。 | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description                 | Required |
| -------------- | ------ | --------------------------- | -------- |
| RetCode        | int    | 返回码                      | **Yes**  |
| Action         | string | 操作名称                    | **Yes**  |
| Message        | string | 返回信息描述。              | **Yes**  |
| TotalCount     | int    | 返回NAT网关总个数           | **Yes**  |
| Infos          | array  | 【数组】返回nat网关对象数组 | **Yes**  |

**NATGWInfo**

| Parameter name  | Type   | Description         | Required |
| --------------- | ------ | ------------------------------------------------------------ | -------- |
| Region          | string | 地域                                                         | **Yes**  |
| Zone            | string | 可用区                                                       | **Yes**  |
| NATGWID         | string | NAT网关ID                                                    | **Yes**  |
| Name            | string | 名称                                                         | **Yes**  |
| Remark          | string | 备注                                                         | **Yes**  |
| NATGWStatus     | string | 状态。Creating:创建中, Running:运行中, Deleting:删除中, Deleted:已删除 | **Yes**  |
| EIP             | string | 虚拟IP                                                       | **Yes**  |
| VPCID           | string | NAT网关实例所在的 VPC ID                                     | **Yes**  |
| SubnetID        | string | NAT网关实例所在的子网 ID                                     | **Yes**  |
| SGID            | string | NAT网关绑定的安全组ID                                        | **Yes**  |
| ChargeType      | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| AlarmTemplateID | string | 告警模板ID                                                   | **Yes**  |
| CreateTime      | int    | 创建时间，时间戳                                             | **Yes**  |
| ExpireTime      | int    | 过期时间，时间戳                                             | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeNATGW
&Region=cUunchUE
&Zone=AFIgfjMT
&NATGWIDs.N=dGsexTRj
&Offset=4
&Limit=3
```

**Response Example**

```
{
    "Action": "DescribeNATGWResponse", 
    "TotalCount": 9, 
    "Message": "jEOiNHjg", 
    "Infos": [
        {
            "EIP": "AotWJgcy", 
            "Remark": "PgAEytTI", 
            "VPCID": "XCPWQsBC", 
            "Name": "EgbkzSdP", 
            "Zone": "aWZtSwdh", 
            "Region": "PepeRTLD", 
            "ChargeType": "Dynamic", 
            "ExpireTime": 5, 
            "AlarmTemplateID": "VuVEDDgn", 
            "NATGWStatus": "Creating", 
            "SubnetID": "TVwYtNVL", 
            "SGID": "mJzcGpdi", 
            "CreateTime": 9, 
            "NATGWID": "UNZeioPI"
        }
    ], 
    "RetCode": 0
}
```

## 11.3 删除NAT网关-DeleteNATGW

删除NAT网关

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| NATGWID        | string | NAT网关ID                           | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteNATGW
&Region=FeoiUHCx
&Zone=MjRhTqDP
&NATGWID=OrTaPAif
```

**Response Example**

```
{
    "Action": "DeleteNATGWResponse", 
    "Message": "JsgHoqQq", 
    "RetCode": 0
}
```

## 11.4 添加NAT网关白名单-CreateNATGWRule

添加NAT网关白名单

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| NATGWID        | string | NAT网关ID                           | **Yes**  |
| BindResourceID | string | 绑定的虚拟机资源ID                  | **Yes**  |
| NATGWType      | string | NAT的类型。枚举值：SNAT，DNAT       | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |
| RuleID         | string | 白名单ID       | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateNATGWRule
&Region=cn
&Zone=zone-01
&NATGWID=qclfZMlh
&BindResourceID=UvZerHdr
&NATGWType=ucVWIVkE
```

**Response Example**

```
{
    "Action": "CreateNATGWRuleResponse", 
    "Message": "qvtqXrKv", 
    "RetCode": 0, 
    "RuleID": "RLHBbqGt"
}
```

## 11.5 获取NAT网关白名单信息 -DescribeNATGWRule

获取NAT网关白名单信息 

**Request Parameters**

| Parameter name    | Type   | Description          | Required |
| ----------------- | ------ | ------------------------------------------------------------ | -------- |
| Region            | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone              | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| NATGWID           | string | NAT网关ID                                                    | **Yes**  |
| NATGWType         | string | NAT类型。枚举值：SNAT，DNAT                                  | **Yes**  |
| Offset            | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit             | int    | 返回数据长度，默认为20，最大100。                            | No       |
| RuleIDs.N         | string | 【数组】NAT网关白名单ID。调用方式举例：NATGWRules.0=“one-id”、NATGWRules.1=“two-id”。 | No       |
| BindResourceIDs.N | string | 【数组】NAT网关白名单资源ID。调用方式举例：NATGWRules.0=“one-id”、NATGWRules.1=“two-id”。 | No       |

**Response Elements**

| Parameter name | Type   | Description                       | Required |
| -------------- | ------ | --------------------------------- | -------- |
| RetCode        | int    | 返回码                            | **Yes**  |
| Action         | string | 操作名称                          | **Yes**  |
| Message        | string | 返回信息描述。                    | **Yes**  |
| TotalCount     | int    | 返回NAT网关白名单资源总个数。     | **Yes**  |
| Infos          | array  | 【数组】返回nat网关白名单对象数组 | **Yes**  |

**NATGWRuleInfo**

| Parameter name   | Type   | Description              | Required |
| ---------------- | ------ | ------------------------------------------------------------ | -------- |
| NATGWID          | string | NAT网关ID                                                    | **Yes**  |
| RuleID           | string | 白名单ID                                                     | **Yes**  |
| NATGWType        | string | nat网关类型                                                  | **Yes**  |
| BindResourceID   | string | 绑定的资源ID                                                 | **Yes**  |
| BindResourceType | string | 绑定资源的类型                                               | **Yes**  |
| Name             | string | 添加的白名单资源名称                                         | **Yes**  |
| IP               | string | 白名单资源的内网IP地址                                       | **Yes**  |
| RuleStatus       | string | 状态。Bounding:绑定中,Bound:已绑定,Unbounding:解绑中,Unbound：已解绑 | **Yes**  |
| CreateTime       | int    | 创建时间，时间戳。                                           | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeNATGWRule
&Region=CfTJmEbG
&Zone=CAdxFeXE
&NATGWID=CmUleLgr
&NATGWType=JgpuIWql
&Offset=5
&Limit=4
&RuleIDs.N=hYipGbJC
&BindResourceIDs.N=TzDGkalX
```

**Response Example**

```
{
    "Action": "DescribeNATGWRuleResponse", 
    "TotalCount": 7, 
    "Message": "LzgkxToj", 
    "Infos": [
        {
            "Name": "DLudPSlB", 
            "RuleID": "IYpfHIaJ", 
            "IP": "SDiEqnJh", 
            "NATGWType": "cKAdJsGu", 
            "BindResourceID": "OmvUfONg", 
            "BindResourceType": "zkpONocv", 
            "RuleStatus": "Bounding", 
            "CreateTime": 5, 
            "NATGWID": "QpgmJLwP"
        }
    ], 
    "RetCode": 0
}
```

## 11.6 删除NAT网关白名单-DeleteNATGWRule

删除NAT网关白名单

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| RuleID         | string | 白名单ID                            | **Yes**  |
| NATGWID        | string | nat网关ID                           | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteNATGWRule
&Region=bJuURVft
&Zone=wyhGJrbN
&RuleID=dDeDmUNE
&NATGWID=bdrqFOev
```

**Response Example**

```
{
    "Action": "DeleteNATGWRuleResponse", 
    "Message": "hPWMedNA", 
    "RetCode": 0
}
```

