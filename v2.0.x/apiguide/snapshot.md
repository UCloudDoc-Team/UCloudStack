# 5 硬盘快照

## 5.1 创建硬盘快照-CreateSnapshot

创建硬盘快照

!> 为保证数据及磁盘的安全，仅支持未绑定及已绑定的硬盘进行快照操作，若硬盘在升级或快照中，无法进行创建快照操作。

**Request Parameters**

| Parameter name | Type   | Description                               | Required |
| -------------- | ------ | ----------------------------------------- | -------- |
| Region         | string | 地域。枚举值：如 cn,表示中国。            | **Yes**  |
| Zone           | string | 可用区。枚举值：如 zone-01，表示可用区1。 | No       |
| Name           | string | 快照名称，限制字符长度30                  | **Yes**  |
| DiskID         | string | 硬盘ID，输入“有效”状态的ID                | **Yes**  |
| Remark         | string | 描述，限制字符长度100                     | No       |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |
| SnapshotID     | string | 创建的快照ID | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateSnapshot
&Region=cn-zj
&Zone=cn-zj-01
&Name=RsvdhPcb
&DiskID=FxxPJPmq
&Remark=CBIehGJR
```

**Response Example**

```
{
    "Action": "CreateSnapshotResponse", 
    "SnapshotID": "OVTzNNtF", 
    "Message": "BWAZQDhS", 
    "RetCode": 0
}
```

## 5.2 查询硬盘快照信息-DescribeSnapshot

查询硬盘快照信息

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                           | No       |
| SnapshotIDs.N  | string | 【数组】快照ID，输入“有效”的ID。调用方式举例：SnapshotIDs.0=“one-id”、SnapshotIDs.1=“two-id”。 | No       |
| DiskID         | string | 硬盘ID，输入“有效”状态的ID                                   | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Message        | string | 返回信息描述             | **Yes**  |
| TotalCount     | int    | 返回快照总个数           | **Yes**  |
| Infos          | array  | 【数组】返回快照对象数组 | **Yes**  |

**SnapshotInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | No       |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | No       |
| Name           | string | 快照名称                                                     | No       |
| Remark         | string | 描述                                                         | No       |
| SnapshotID     | string | 快照ID                                                       | No       |
| SnapshotStatus | string | 快照状态。枚举值：Createing，表示制作中；Normal，表示正常；RollBacking，表示回滚中；Deleting，表示删除中；Deleted，表示已删除； | No       |
| DiskID         | string | 快照对应的硬盘 ID                                            | No       |
| DiskType       | string | 硬盘类型。枚举值：Boot，表示系统盘；Data，表示数据盘；       | No       |
| CreateTime     | int    | 快照创建时间                                                 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeSnapshot
&Region=cn-zj
&Zone=cn-zj-01
&SnapshotIDs=rKAmGutD
&DiskID=yiPZorAe
&Offset=1
&Limit=6
```

**Response Example**

```
{
    "Action": "DescribeSnapshotResponse", 
    "TotalCount": 6, 
    "Message": "xxvzrmqS", 
    "Infos": [
        {
            "Zone": "sHpsasrk", 
            "SnapshotName": "EmRKPMfA", 
            "Region": "nLOwuJMy", 
            "DiskType": "FSqSMNcx", 
            "SnapshotStatus": "chwKtEAU", 
            "SnapshotID": "MxRoqprG", 
            "CreateTime": 6, 
            "DiskID": "PDekJzuV"
        }
    ], 
    "RetCode": 0
}
```

## 5.3 回滚快照-RollbackSnapshot

将某个快照内的数据回滚到原云硬盘，仅支持正常状态的快照进行回滚操作，回滚时硬盘必须处于未绑定或其挂载的主机为关机状态。

**Request Parameters**

| Parameter name | Type   | Description                               | Required |
| -------------- | ------ | ----------------------------------------- | -------- |
| Region         | string | 地域。枚举值：如 cn,表示中国。            | **Yes**  |
| Zone           | string | 可用区。枚举值：如 zone-01，表示可用区1。  | No       |
| DiskID         | string | 对应的云硬盘 ID；                         | **Yes**  |
| SnapshotID     | string | 快照ID                                    | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=RollbackSnapshot
&Region=cn-zj
&Zone=cn-zj-01
&DiskID=icJxMctU
&SnapshotID=oGGQpgjB
```

**Response Example**

```
{
    "Action": "RollbackSnapshotResponse", 
    "Message": "NAyAmJTS", 
    "RetCode": 0
}
```

## 5.4 删除快照-DeleteSnapshot

删除快照，仅支持状态为正常的快照进行删除操作。

**Request Parameters**

| Parameter name | Type   | Description                               | Required |
| -------------- | ------ | ----------------------------------------- | -------- |
| Region         | string | 地域。枚举值：如 cn,表示中国。            | **Yes**  |
| Zone           | string | 可用区。枚举值：如 zone-01，表示可用区1。  | No       |
| SnapshotID     | string | 快照ID                                    | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteSnapshot
&Region=cn-zj
&Zone=cn-zj-01
&SnapshotID=lWHdWLAn
```

**Response Example**

```
{
    "Action": "DeleteSnapshotResponse", 
    "Message": "NgzHbJOY", 
    "RetCode": 0
}
```

