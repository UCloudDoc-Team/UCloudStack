# 13 回收站

## 13.1 查询回收站资源-DescribeRecycledResource

查询回收站资源

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：如 cn,表示中国。                               | **Yes**  |
| Zone           | string | 可用区。枚举值：如 zone-01，表示可用区1。                    | **Yes**  |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| ResourceIDs.N  | string | 【数组】资源ID，输入“有效”的ID。调用方式举例：ResourceIDs.0=“one-id”、ResourceIDs.1=“two-id”。 | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Infos          | array  | 【数组】返回资源对象数组 | **Yes**  |
| TotalCount     | int    | 返回回收站资源的总个数   | **Yes**  |

**RecycledResourceInfo**

| Parameter name    | Type   | Description                                                  | Required |
| ----------------- | ------ | ------------------------------------------------------------ | -------- |
| Region            | string | 地域                                                         | **Yes**  |
| Zone              | string | 可用区                                                       | **Yes**  |
| Name              | string | 名称                                                         | **Yes**  |
| Description       | string | 描述                                                         | **Yes**  |
| ResourceType      | string | 资源类型：VM:虚拟机，Disk:硬盘，EIP:外网IP，PIP:物理IP，MySQL:数据库，Redis:缓存 | **Yes**  |
| ResourceID        | string | 资源ID                                                       | **Yes**  |
| CreateTime        | int    | 创建时间                                                     | **Yes**  |
| DeleteTime        | int    | 删除时间                                                     | **Yes**  |
| WillTerminateTime | int    | 销毁时间                                                     | **Yes**  |
| ExpireTime        | int    | 过期时间                                                     | **Yes**  |
| IsAutoTerminated  | bool   | 是否自动销户                                                 | **Yes**  |
| Status            | string | 资源状态                                                     | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeRecycledResource
&Region=cn-zj
&Zone=cn-zj-01
&ProjectId=eDIgcGNy
&Keyword=xOrCLJUy
&Limit=3
&Offset=4
&ResourceIDs.N=lPTfzPKq
&ResourceIDs.N=MhgeZCON
&Keyword=OcJIeIxH
```

**Response Example**

```
{
    "Action": "DescribeRecycledResourceResponse", 
    "TotalCount": 1, 
    "Infos": [
        "XDckqsFJ"
    ], 
    "RetCode": 0
}
```

## 13.2 恢复回收站资源-RollbackResource

恢复回收站资源

**Request Parameters**

| Parameter name | Type   | Description                               | Required |
| -------------- | ------ | ----------------------------------------- | -------- |
| Region         | string | 地域。枚举值：如 cn,表示中国。            | **Yes**  |
| Zone           | string | 可用区。枚举值：如 zone-01，表示可用区1。 | **Yes**  |
| ResourceID     | string | 待恢复的资源ID                            | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回描述信息 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=RollbackResource
&Region=cn-zj
&Zone=cn-zj-01
&ResourceID=euCJVxlb
```

**Response Example**

```
{
    "Action": "RollbackResourceResponse", 
    "Message": "FpJhwMSB", 
    "RetCode": 0
}
```

## 13.3 续费回收站资源-RenewResource

续费回收站资源 

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：如 cn,表示中国。                               | **Yes**  |
| Zone           | string | 可用区。枚举值：如 zone-01，表示可用区1。                    | **Yes**  |
| ResourceID     | string | 待续续的资源ID                                               | **Yes**  |
| Quantity       | int    | 购买时长，默认为 1。按小时(Dynamic)付费的资源无需此参数，按月付费的资源传 0 时，代表购买至月末。 | No       |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回描述信息 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=RenewResource
&Region=cn-zj
&Zone=cn-zj-01
&ProjectId=bMLyxEto
&ResourceID=LhGyiWLf
&Quantity=6
```

**Response Example**

```
{
    "Action": "RenewResourceResponse", 
    "Message": "sMNHWSdZ", 
    "RetCode": 0
}
```

## 13.4 销毁资源-TerminateResource

销毁资源

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域。      | **Yes**  |
| Zone           | string | 可用区。    | **Yes**  |
| ResourceID     | string | 资源id      | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回描述信息 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=TerminateResource
&Region=cn-zj
&Zone=cn-zj-01
&ProjectId=gMdvxLJA
&ResourceID=QYDCJKrv
&ResourceType=PfyNGwVr
&ResourceID=hsHmIrGa
```

**Response Example**

```
{
    "Action": "TerminateResourceResponse", 
    "Message": "mHuHgOve", 
    "RetCode": 0
}
```

