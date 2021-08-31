# 4 云硬盘

## 4.1 创建云硬盘-CreateDisk

创建 UCloudStack 云硬盘。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | No       |
| SetType        | string | 磁盘类型。例如：Normal,SSD                                   | **Yes**  |
| Name           | string | 磁盘名称                                                     | **Yes**  |
| DiskSpace      | int    | 磁盘大小                                                     | **Yes**  |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Quantity       | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |
| DiskID         | string | 创建的磁盘ID   | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateDisk
&Region=cn
&Zone=zone-01
&StorageType=LocalDisk
&VMType=ECLTjhQn
&Name=tGuwcuXQ
&DiskSpace=4
&ChargeType=JzuxyWSg
&Quantity=9
```

**Response Example**

```
{
    "Action": "CreateDiskResponse", 
    "Message": "CuqjnfHe", 
    "RetCode": 0, 
    "DIskID": "XJoFlhtg"
}
```

## 4.2 获取云硬盘信息-DescribeDisk

获取 UCloudStack 云硬盘信息。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                           | No       |
| DiskIDs.N      | string | 【数组】磁盘的 ID。输入有效的 ID。调用方式举例：DiskIDs.0=“one-id”、DiskIDs.1=“two-id”。 | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Message        | string | 返回信息描述。           | **Yes**  |
| TotalCount     | int    | 返回磁盘总个数。         | **Yes**  |
| Infos          | array  | 【数组】返回磁盘对象数组 | **Yes**  |

**DiskInfo**

| Parameter name   | Type   | Description                                                  | Required |
| ---------------- | ------ | ------------------------------------------------------------ | -------- |
| Region           | string | 地域                                                         | No       |
| Zone             | string | 可用区                                                       | No       |
| DiskID           | string | 硬盘ID                                                       | No       |
| Name             | string | 名称                                                         | No       |
| Remark           | string | 备注                                                         | No       |
| Size             | int    | 大小。单位GB                                                 | No       |
| SetType          | string | 磁盘类型。例如：Normal,SSD                                   | No       |
| DiskStatus       | string | 硬盘状态。Creating：创建中,BeingCloned：克隆中,Unbound：已解绑,Unbounding：解绑中,Bounding：绑定中,Bound：已绑定,Upgrading：升级中,Deleting：删除中,Deleted：已删除,Releasing：销毁中,Released：已销毁；Snapshoting（快照中）；Rollbacking（回滚中） | No       |
| AttachResourceID | string | 绑定资源ID                                                   | No       |
| ChargeType       | string | 硬盘计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | No       |
| CreateTime       | int    | 创建时间。时间戳                                             | No       |
| ExpireTime       | int    | 过期时间。时间戳                                             | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeDisk
&Region=xmwSiUbA
&Zone=NpcNUAvc
&DiskIDs.N=KcSmPZaK
&Offset=jcuTVzLW
&Limit=mfLkRUOh
```

**Response Example**

```
{
    "Action": "DescribeDiskResponse", 
    "TotalCount": "zMkEpWOZ", 
    "Message": "yDPoftjB", 
    "Infos": [
        "tjQctHDP"
    ], 
    "RetCode": 0
}
```

## 4.3 获取云硬盘价格-GetDiskPrice

获取 UCloudStack 云硬盘价格信息。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。                                                       | **Yes**  |
| Zone           | string | 可用区                                                       | No       |
| DiskSpace      | int    | 磁盘大小                                                     | **Yes**  |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| SetType        | string | 磁盘类型                                                     | **Yes**  |
| Quantity       | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |
| Infos          | array  | 价格信息       | No       |

**PriceInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Price          | float  | 价格                                                         | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=GetDiskPrice
&Region=CfXeboqy
&Zone=LkCefQha
&StorageType=LocalDisk
&DiskSpace=2
&ChargeType=rBfxaaVc
&Quantity=4
&DiskType=JSRILwJM
```

**Response Example**

```
{
    "RetCode": 0, 
    "Price": 9, 
    "ChargeType": "vKDNlcwp", 
    "Action": "GetDiskPriceResponse", 
    "Message": "qgCEetLa", 
    "Infos": [
        {
            "Price": 8.54288, 
            "ChargeType": "Dynamic"
        }
    ]
}
```

## 4.4 绑定云硬盘-AttachDisk

绑定 UCloudStack 云硬盘至虚拟机实例。

**Request Parameters**

| Parameter name | Type   | Description                            | Required |
| -------------- | ------ | -------------------------------------- | -------- |
| Region         | string | 地域                                   | **Yes**  |
| Zone           | string | 可用区                                 | No       |
| DiskID         | string | 硬盘ID                                 | **Yes**  |
| ResourceID     | string | 绑定的资源ID                           | **Yes**  |
| ResourceType   | string | 绑定的资源类型，枚举值：VM，标识虚拟机 | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=AttachDisk
&DiskID=zQOlXmuF
&BindResourceID=pzGEJkgm
&BindResourceType=VM
&Zone=NMPwJNpK
&DiskID=AybnmGwA
```

**Response Example**

```
{
    "Action": "AttachDiskResponse", 
    "Message": "CUtzuXmT", 
    "RetCode": 0
}
```

## 4.5 解绑云硬盘-DetachDisk

将云硬盘从虚拟机上卸载，卸载的云硬盘可重新绑定至虚拟机。

**Request Parameters**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| Region         | string | 地域         | **Yes**  |
| Zone           | string | 可用区        | No       |
| DiskID         | string | 硬盘ID       | **Yes**  |
| ResourceID     | string | 绑定的资源ID | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DetachDisk
&DiskID=AQmiaNOn
&BindResourceID=MZzGJyjN
&BindResourceType=VM
&Zone=fbtCbAOG
&DiskID=gWRQdwxm
```

Response Example

```
{
    "Action": "DetachDiskResponse", 
    "Message": "moukTjaJ", 
    "RetCode": 0
}
```

## 4.6 升级硬盘-UpgradeDisk

升级硬盘，为保证数据完整性，容量扩容前建议暂停对当前硬盘的所有文件系统读写操作，并进入操作系统进行 `umount ` 或`脱机` 操作。

**Request Parameters**

| Parameter name | Type   | Description                                         | Required |
| -------------- | ------ | --------------------------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                 | No       |
| DiskID         | string | 硬盘ID                                              | **Yes**  |
| DiskSpace      | int    | 硬盘升级后的容量， 不能小于原硬盘容量，单位为 GB 。 | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UpgradeDisk
&Region=SZBsErbz
&Zone=qENOgXHP
&DiskID=DQCbRBkv
&DiskSpace=2
```

**Response Example**

```
{
    "Action": "UpgradeDiskResponse", 
    "Message": "bcRYqzKZ", 
    "RetCode": 0
}
```

## 4.7 克隆云硬盘-CloneDisk

克隆 UCloudStack 云硬盘。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域                                                         | **Yes**  |
| Zone           | string | 可用区                                                        | No       |
| SrcID          | string | 源硬盘ID                                                     | **Yes**  |
| Name           | string | 名称                                                         | **Yes**  |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Quantity       | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |
| DiskID         | string | 克隆出的硬盘ID | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CloneDisk
&Region=wirBLxoX
&Zone=hdUruBXi
&SrcID=fHBzzdrd
&Name=OXWXXURY
&ChargeType=ThYRjWQF
&Quantity=ZThyUVyK
```

**Response Example**

```
{
    "Action": "CloneDiskResponse", 
    "Message": "APguKuNO", 
    "RetCode": 0, 
    "DiskID": "XvftolRk"
}
```

## 4.8 删除云硬盘-DeleteDisk

删除 UCloudStack 云硬盘资源，云硬盘删除后会进入回收站。

**Request Parameters**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| Region         | string | 地域           | **Yes**  |
| Zone           | string | 可用区          | No       |
| DiskID         | string | 被删除的硬盘ID | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteDisk
&Region=oHvuTwLW
&DiskID=voDtaqZZ
&Zone=JEBFNWps
```

**Response Example**

```
{
    "Action": "DeleteDiskResponse", 
    "Message": "odgMHmEe", 
    "RetCode": 0
}
```

