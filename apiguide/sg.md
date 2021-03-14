# 9 安全组

## 9.1 创建安全组-CreateSecurityGroup

创建安全组

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| Name           | string | 名称;                                                        | **Yes**  |
| Rule.N         | string | 【数组】安全组规则。输入有效的规则，调用方式举例：Rule.0=“TCP\|23\|0.0.0.0/0\|ACCEPT\|HIGH\|1”、Rule.1=“TCP\|55\|0.0.0.0/0\|ACCEPT\|HIGH\|1” | **Yes**  |
| Remark         | string | 描述;                                                        | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateSecurityGroup
&Region=mBSNafIJ
&Zone=TgCDLQwd
&Name=XemCvdKd
&Rule.N=werodRwL
&Remark=efLwiaBl
```

**Response Example**

```
{
    "Action": "CreateSecurityGroupResponse", 
    "SGID": "QhsLUtSH", 
    "Message": "ggWCgjsx", 
    "RetCode": 0
}
```

## 9.2 查询安全组信息-DescribeSecurityGroup

查询安全组信息

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。                                                       | **Yes**  |
| Zone           | string | 可用区。                                                     | **Yes**  |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| SGIDs.N        | string | 【数组】安全组的 ID。输入有效的 ID。调用方式举例：SGIDs.0=“one-id”、SGIDs.1=“two-id” | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**SGInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域                                                         | No       |
| Zone           | string | 可用区                                                       | No       |
| SGID           | string | 安全组ID                                                     | No       |
| Name           | string | 名称                                                         | No       |
| Status         | string | 状态。Creating：创建中,Updating：更新中,Available：有效,Deleted：已删除,Terminating：销毁中,Terminated：已销毁 | No       |
| Remark         | string | 描述                                                         | No       |
| ResourceCount  | int    | 资源绑定数量                                                 | No       |
| Rule           | array  | 安全组规则。                                                 | No       |
| RuleCount      | int    | 规则数量                                                     | No       |
| CreateTime     | int    | 创建时间，时间戳                                             | No       |
| UpdateTime     | int    | 更新时间，时间戳                                             | No       |

**SGRuleInfo**

| Parameter name | Type   | Description                               | Required |
| -------------- | ------ | ----------------------------------------- | -------- |
| RuleID         | string | 规则ID                                    | No       |
| SrcIP          | string | IP或者掩码/段形式。10.0.0.2,10.0.10.10/16 | No       |
| Priority       | string | 优先级。HIGH:高，MEDIUM:中，LOW:低        | No       |
| ProtocolType   | string | 协议                                      | No       |
| DstPort        | string | 端口号                                    | No       |
| RuleAction     | string | 动作。ACCEPT：接受，DROP：拒绝            | No       |
| IsIn           | string | 方向。1：入，0：出                        | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeSecurityGroup
&Region=VmETiOSi
&Zone=vtTtvTJw
&Limit=7
&Offset=3
&SGIDs.N=xCuqnIYy
```

**Response Example**

```
{
    "Action": "DescribeSecurityGroupResponse", 
    "TotalCount": 2, 
    "Message": "KczyCwYA", 
    "Infos": [
        {
            "Status": "rfkwrebV", 
            "Remark": "DupbLCkC", 
            "ResourceCount": 3, 
            "Name": "MvDpbOFZ", 
            "Zone": "YCtZUCKE", 
            "Region": "zpBVJTmw", 
            "UpdateTime": 6, 
            "Rule": [
                {
                    "RuleID": "uRYfyzOy", 
                    "ProtocolType": "TgAPghqF", 
                    "Priority": "cIcEZnCW", 
                    "RuleAction": "LponthVe", 
                    "SrcIP": "JpbnVeyK", 
                    "IsIn": "sVwKIWWS", 
                    "DstPort": "ThvVrzGF"
                }
            ], 
            "RuleCount": 3, 
            "SGID": "KSXNlhVY", 
            "CreateTime": 5
        }
    ], 
    "RetCode": 0
}
```

## 9.3 删除安全组-DeleteSecurityGroup

删除安全组

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；       | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| SGID           | string | 安全组ID                            | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteSecurityGroup
&Region=yLaoHozN
&Zone=UzWUGlNL
&SGID=pzowroWS
```

**Response Example**

```
{
    "Action": "DeleteSecurityGroupResponse", 
    "Message": "AEvoXnsi", 
    "RetCode": 0
}
```

## 9.4 创建安全组规则-CreateSecurityGroupRule

创建安全组规则

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| SGID           | string | 安全组ID                                                     | **Yes**  |
| Rules.N        | string | 【数组】安全组规则。输入有效的规则，调用方式举例：Rule.0=“TCP\|23\|0.0.0.0/0\|ACCEPT\|HIGH\|1”、Rule.1=“TCP\|55\|0.0.0.0/0\|ACCEPT\|HIGH\|1” | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateSecurityGroupRule
&Region=dKCtOsvv
&Zone=GQwnozYc
&SGID=hzssMniY
&Rules.N=OFrenUjw
```

**Response Example**

```
{
    "Action": "CreateSecurityGroupRuleResponse", 
    "Message": "dczhUUtk", 
    "SGRuleID": "ldgyOEQd", 
    "RetCode": 0
}
```

## 9.5 修改安全组规则-UpdateSecurityGroupRule

修改安全组规则

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| SGID           | string | 安全组ID                                                     | **Yes**  |
| Rules.N        | string | 【数组】规则。输入有效的 规则。调用方式举例：Rules.0=“TCP\|23\|0.0.0.0/0\|ACCEPT\|HIGH\|1\|sg_rule-wefvg34f”、Rules.1=“TCP\|55\|0.0.0.0/0\|ACCEPT\|HIGH\|1\|sg_rule-wefvggf” | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UpdateSecurityGroupRule
&Region=XiHEGOAx
&Zone=BwqGouSr
&SGID=ifFOuBhC
&Rules.N=jeKSrdZU
```

**Response Example**

```
{
    "Action": "UpdateSecurityGroupRuleResponse", 
    "Message": "QrnbhbWs", 
    "RetCode": 0
}
```

## 9.6 删除安全组规则-DeleteSecurityGroupRule

删除安全组规则

**Request Parameters**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| Region         | string | 地域。       | **Yes**  |
| Zone           | string | 可用区。     | **Yes**  |
| SGID           | string | 安全组ID     | **Yes**  |
| SGRuleID       | string | 安全组规则ID | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteSecurityGroupRule
&Region=tFuMChLG
&Zone=AXGoLWxV
&SGID=wZvelWfx
&SGRuleID=lNEQOSap
```

**Response Example**

```
{
    "Action": "DeleteSecurityGroupRuleResponse", 
    "Message": "QvvTQFPp", 
    "RetCode": 0
}
```

## 9.7 绑定安全组-BindSecurityGroup

绑定安全组

**Request Parameters**

| Parameter name | Type   | Description                                       | Required |
| -------------- | ------ | ------------------------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                     | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；               | **Yes**  |
| ResourceID     | string | 绑定的资源ID。调用方式举例：ResourceID=“one-id”。 | **Yes**  |
| SGID           | string | 安全组ID                                          | **Yes**  |
| NICID          | string | 网卡ID                                            | No       |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=BindSecurityGroup
&Region=eKfmhosF
&Zone=SsNeJAEb
&ResourceID=OPJSfLmG
&SGID=gvpUkfhe
&NICID=uLMElxbc
```

**Response Example**

```
{
    "Action": "BindSecurityGroupResponse", 
    "Message": "SQuEqizT", 
    "RetCode": 0
}
```

## 9.8 解绑安全组-UnBindSecurityGroup

解绑安全组

**Request Parameters**

| Parameter name | Type   | Description                                       | Required |
| -------------- | ------ | ------------------------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                     | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；               | **Yes**  |
| ResourceID     | string | 解绑的资源ID。调用方式举例：ResourceID=“one-id”。 | **Yes**  |
| SGID           | string | 安全组ID                                          | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UnBindSecurityGroup
&Region=IQxzgMoG
&Zone=DavYnlLq
&ResourceID=GoWhyrAT
&SGID=nDYeMYbG
```

**Response Example**

```
{
    "Action": "UnBindSecurityGroupResponse", 
    "Message": "HvZpQpFD", 
    "RetCode": 0
}
```

## 9.9 查询安全组绑定的资源信息-DescribeSecurityGroupResource

查询安全组绑定的资源信息

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；       | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| SGID           | string | 安全组ID                            | **Yes**  |
| Offset         | int    | 列表起始位置偏移量，默认为0。       | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。   | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Message        | string | 返回信息描述。           | **Yes**  |
| TotalCount     | int    | 返回资源总个数。         | **Yes**  |
| Infos          | array  | 【数组】返回资源对象数组 | **Yes**  |

**SGResourceInfo**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域        | **Yes**  |
| Zone           | string | 可用区      | **Yes**  |
| Name           | string | 资源名称    | **Yes**  |
| ResourceID     | string | 资源ID      | **Yes**  |
| ResourceType   | string | 资源类型    | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeSecurityGroupResource
&SGID=oUlxQIRz
&Offset=CLuLQeAp
&Limit=qNwYnEFY
&Region=hVMuECVm
&Zone=SaQhBGLf
```

**Response Example**

```
{
    "Action": "DescribeSecurityGroupResourceResponse", 
    "TotalCount": 2, 
    "Message": "aEhGfxpc", 
    "Infos": "SaATFWeK", 
    "RetCode": 0
}
```

