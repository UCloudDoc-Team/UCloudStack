# 6 VPC 私有网络

## 6.1 创建 VPC-CreateVPC

创建 VPC 私有网络

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；       | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| Network        | string | 网段。例如：10.0.0.0/16；           | **Yes**  |
| Name           | string | 名称;                               | **Yes**  |
| Remark         | string | 描述;                               | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateVPC
&Region=hkwuGGeK
&Zone=TrqbxhMm
&Network=hgjixKha
&Name=qrGmEsqI
&Remark=WdLzycHb
```

**Response Example**

```
{
    "Action": "CreateVPCResponse", 
    "Message": "ocmqsqTr", 
    "VPCID": "CgFIwdya", 
    "RetCode": 0
}
```

## 6.2 查询 VPC 信息-DescribeVPC

查询 VPC 的详细信息

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。                                                       | **Yes**  |
| Zone           | string | 可用区。                                                     | **Yes**  |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| VPCIDs.N       | string | 【数组】VPC的 ID。调用方式举例：VPCIDs.0=“one-id”、VPCIDs.1=“two-id” | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**VPCInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。                                                       | No       |
| Zone           | string | 可用区                                                       | No       |
| VPCID          | string | VPC的ID                                                      | No       |
| Name           | string | 名称                                                         | No       |
| State          | string | 状态；Allocating：申请中,Available：有效,Terminating：销毁中,Terminated：已销毁 | No       |
| Remark         | string | 描述                                                         | No       |
| SubnetCount    | int    | 该VPC下拥有的子网数目                                        | No       |
| CreateTime     | int    | 创建时间，时间戳                                             | No       |
| UpdateTime     | int    | 修改时间，时间戳                                             | No       |
| SubnetInfos    | array  | 该VPC下子网信息。                                            | No       |

**SubnetInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域                                                         | No       |
| Zone           | string | 可用区                                                       | No       |
| SubnetID       | string | ID                                                           | No       |
| Name           | string | 名称                                                         | No       |
| Remark         | string | 描述                                                         | No       |
| Network        | string | 网段                                                         | No       |
| State          | string | 状态；Allocating：申请中,Available：有效,Deleting：删除中,Deleted：已删除 | No       |
| CreateTime     | int    | 创建时间，时间戳                                             | No       |
| UpdateTime     | int    | 更新时间，时间戳                                             | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeVPC
&Region=HyULLTdr
&Zone=buMkFyfh
&Limit=7
&Offset=6
&VPCIDs.N=RJhKKVfw
```

**Response Example**

```
{
    "Action": "DescribeVPCResponse", 
    "TotalCount": 5, 
    "Message": "vUCVfnxP", 
    "Infos": [
        {
            "Remark": "mkQaoTCy", 
            "VPCID": "ZktlYqve", 
            "Name": "lCCMLVpR", 
            "Zone": "CNKSjUeA", 
            "Region": "SpTgLKzY", 
            "UpdateTime": 9, 
            "SubnetInfos": [
                {
                    "Remark": "lwrvCRKE", 
                    "Name": "wxlsonLl", 
                    "Zone": "HLsNhGVl", 
                    "Region": "eYfQLSBu", 
                    "UpdateTime": 1, 
                    "State": "gMBPGFlB", 
                    "SubnetID": "UZMvAgvV", 
                    "CreateTime": 5, 
                    "Network": "XSCkIKGO"
                }
            ], 
            "State": "GqlIfDzb", 
            "SubnetCount": 8, 
            "CreateTime": 1
        }
    ], 
    "RetCode": 0
}
```

## 6.3 删除 VPC-DeleteVPC

删除 VPC

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域。      | **Yes**  |
| Zone           | string | 可用区。    | **Yes**  |
| VPCID          | string | ID          | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteVPC
&Region=zHzJqehB
&Zone=OAKDJkjF
&VPCID=KsMMLmqN
```

**Response Example**

```
{
    "Action": "DeleteVPCResponse", 
    "Message": "bFBVAkyE", 
    "RetCode": 0
}
```

## 6.4 创建子网-CreateSubnet

创建子网

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；       | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| Name           | string | 名称;                               | **Yes**  |
| Network        | string | 网段。列如：10.0.0.0/16；           | **Yes**  |
| VPCID          | string | 所属VPCID                           | **Yes**  |
| Remark         | string | 描述;                               | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateSubnet
&Region=noJZACFA
&Zone=MzSceHgt
&Name=mqGrkIaV
&Network=mkVMDUme
&VPCID=pTiEqMOw
&Remark=RjmXOUgb
```

**Response Example**

```
{
    "SubnetID": "kzHxSXDm", 
    "Action": "CreateSubnetResponse", 
    "Message": "SIpUxcfM", 
    "RetCode": 0
}
```

## 6.5 查询子网信息-DescribeSubnet

查询子网信息

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。                                                       | **Yes**  |
| Zone           | string | 可用区。                                                     | **Yes**  |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| VPCID          | string | VPCID                                                        | No       |
| SubnetIDs.N    | string | 【数组】子网 ID。调用方式举例：SubnetIDs.0=“one-id”、SubnetIDs.1=“two-id” | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**SubnetInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域                                                         | No       |
| Zone           | string | 可用区                                                       | No       |
| SubnetID       | string | ID                                                           | No       |
| Name           | string | 名称                                                         | No       |
| Remark         | string | 描述                                                         | No       |
| Network        | string | 网段                                                         | No       |
| State          | string | 状态；Allocating：申请中,Available：有效,Deleting：删除中,Deleted：已删除 | No       |
| CreateTime     | int    | 创建时间，时间戳                                             | No       |
| UpdateTime     | int    | 更新时间，时间戳                                             | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeSubnet
&Region=JTuvMryP
&Zone=uwiUrrky
&Limit=4
&Offset=1
&VPCID=JgNOwXhC
&SubnetIDs.N=KxeaNVzw
```

**Response Example**

```
{
    "Action": "DescribeSubnetResponse", 
    "TotalCount": 2, 
    "Message": "ZSQbBKCA", 
    "Infos": [
        {
            "Remark": "wLHWgCop", 
            "Name": "tdyaAAPt", 
            "Zone": "kuembULR", 
            "Region": "DFzecatu", 
            "UpdateTime": 5, 
            "State": "dCADFfVR", 
            "SubnetID": "scKXBeza", 
            "CreateTime": 7, 
            "Network": "iriarhtk"
        }
    ], 
    "RetCode": 0
}
```

## 6.6 删除子网-DeleteSubnet

删除子网

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域。      | **Yes**  |
| Zone           | string | 可用区。    | **Yes**  |
| SubnetID       | string | SubnetID    | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteSubnet
&Region=LsWOjCHu
&Zone=ytouuIJK
&SubnetID=RMzmbwMh
```

**Response Example**

```
{
    "Action": "DeleteSubnetResponse", 
    "Message": "ZddTylIQ", 
    "RetCode": 0
}
```

