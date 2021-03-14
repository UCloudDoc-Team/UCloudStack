# 14 资源通用操作

## 14.1 获取资源监控信息-DescribeMetric

获取资源监控信息，资源通用 API ，根据请求的资源类型及资源 ID 返回相关监控信息。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn，表示中国；                                 | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，中国；                              | **Yes**  |
| ResourceID     | string | 资源ID                                                       | **Yes**  |
| ResourceType   | string | 资源类型。VM：虚拟机；EIP：弹性IP                            | **Yes**  |
| BeginTime      | string | 开始时间。使用unix时间戳                                     | **Yes**  |
| EndTime        | string | 结束时间。使用Unix时间戳                                     | **Yes**  |
| MetricName.N   | string | 监控指标。1. 获取虚拟机监控信息调用举例，MetricName.0="CPUUtilization"、MetricName.0="MemUsage"。虚拟机监控指标枚举值：BlockProcessCount，表示阻塞进程数；CPUUtilization，表示CPU使用率；DiskReadOps，表示磁盘读次数；DiskWriteOps，表示磁盘写次数；IORead，表示磁盘读吞吐；IOWrite，表示磁盘写吞吐；LoadAvg，表示平均负载1分钟；MemUsage，表示内存使用率；NetPacketIn，表示网卡入包量；NetPacketOut，表示网卡出包量；NICIn，表示网卡入带宽；NICOut，表示网卡出带宽；SpaceUsage，表示空间使用率；TCPConnectCount，表示TCP连接数；2. EIP监控指标：NetPacketIn：入包量；NetPacketOut：出包量；NICIn：入带宽；NICOut：出带宽；NICOutUsage：出带宽使用率； | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description      | Required |
| -------------- | ------ | ---------------- | -------- |
| RetCode        | int    | 返回码           | **Yes**  |
| Action         | string | 操作名称         | **Yes**  |
| Message        | string | 返回信息描述     | No       |
| TotalCount     | int    | 返回监控信息条数 | No       |
| Infos          | array  | 返回信息列表     | No       |

**MetricInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| MetricName     | string | 监控指标。虚拟机的监控指标枚举值为：BlockProcessCount，表示阻塞进程数；CPUUtilization，表示CPU使用率；DiskReadOps，表示磁盘读次数；DiskWriteOps，表示磁盘写次数；IORead，表示磁盘读吞吐；IOWrite，表示磁盘写吞吐；LoadAvg，表示平均负载1分钟；MemUsage，表示内存使用率；NetPacketIn，表示网卡入包量；NetPacketOut，表示网卡出包量；NICIn，表示网卡入带宽；NICOut，表示网卡出带宽；SpaceUsage，表示空间使用率；TCPConnectCount，表示TCP连接数； | No       |
| Infos          | array  | 监控值信息                                                   | No       |

**MetricSet**

| Parameter name | Type  | Description | Required |
| -------------- | ----- | ----------- | -------- |
| Timestamp      | int   | 监控时间    | No       |
| Value          | float | 监控值      | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeMetric
&Region=cn
&Zone=zone-01
&ResourceID=YzWpuDIg
&ResourceType=iKtlCQsF
&BeginTime=iXIsOyVU
&EndTime=dwNekaqm
&MetricName.N=BlockProcessCount
```

**Response Example**

```
{
    "Action": "DescribeMetricResponse", 
    "TotalCount": 1, 
    "Message": "WqIXyTEa", 
    "Infos": [
        {
            "Infos": [
                {
                    "Timestamp": 6, 
                    "Value": 5.34491
                }
            ], 
            "MetricName": "ElcccnVk"
        }
    ], 
    "RetCode": 0
}
```

## 14.2 修改资源名称和备注-ModifyNameAndRemark

修改资源名称和备注，资源通用 API 。

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；       | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| ResourceID     | string | 资源ID;                             | **Yes**  |
| Name           | string | 名称;                               | **Yes**  |
| Remark         | string | 描述;                               | No       |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=ModifyNameAndRemark
&Region=vLVQeDcO
&Zone=yFiEeVPX
&ResourceID=MsctEfZF
&Name=SgvkiVqj
&Remark=ZPSSUwOV
```

**Response Example**

```
{
    "Action": "ModifyNameAndRemarkResponse", 
    "Message": "wUxBPBtD", 
    "RetCode": 0
}
```

## 14.3 绑定告警模板-BindAlarmTemplate

绑定告警模板

**Request Parameters**

| Parameter name  | Type   | Description                                                  | Required |
| --------------- | ------ | ------------------------------------------------------------ | -------- |
| Region          | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone            | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| ResourceIDs.N   | string | 【数组】告警模板ID。调用方式举例：ResourceIDs.0=“one-id”、ResourceIDs.1=“two-id”。 | **Yes**  |
| ResourceType    | string | 资源类型。VM：虚拟机, LB:负载均衡, NATGW：nat网关;EIP:弹性IP | **Yes**  |
| AlarmTemplateID | string | 告警模板ID                                                   | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=BindAlarmTemplate
&Region=OqBEgXxU
&Zone=QOdfROCD
&ResourceIDs.N=gSpvjvpB
&ResourceType=byYRAtPl
&AlarmTemplateID=UGuKLARa
```

**Response Example**

```
{
    "Action": "BindAlarmTemplateResponse", 
    "Message": "rpORjgBO", 
    "RetCode": 0
}
```

## 14.4 解绑告警模板-UnbindAlarmTemplate

解绑告警模板

**Request Parameters**

| Parameter name  | Type   | Description                                                  | Required |
| --------------- | ------ | ------------------------------------------------------------ | -------- |
| Region          | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone            | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| ResourceIDs.N   | string | 【数组】资源的 ID。调用方式举例：ResourceIDs.0=“one-id”、ResourceIDs.1=“two-id”。 | **Yes**  |
| ResourceType    | string | 资源类型。VM：虚拟机, LB:负载均衡, NATGW：nat网关;EIP:弹性网卡 | **Yes**  |
| AlarmTemplateID | string | 告警模板ID                                                   | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UnbindAlarmTemplate
&Region=uAdynArQ
&Zone=wjehVKoU
&ResourceIDs.N=hPHosUtB
&ResourceType=viBQGqWY
&AlarmTemplateID=cCBssjyK
```

**Response Example**

```
{
    "Action": "UnbindAlarmTemplateResponse", 
    "Message": "toGFaOmr", 
    "RetCode": 0
}
```

## 14.5 查询主机机型-DescribeVMType

查询主机机型

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域。      | **Yes**  |
| Zone           | string | 可用区。    | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**VMTypeInfo**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域        | **Yes**  |
| ZoneID         | string | 可用区      | **Yes**  |
| VMType         | string | 机型        | **Yes**  |
| VMTypeAlias    | string | 机型别名    | **Yes**  |
| SetArch        | string | 架构        | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeVMType
&Region=PbCQbrVT
&Zone=LpYzjJKN
```

**Response Example**

```
{
    "Action": "DescribeVMTypeResponse", 
    "TotalCount": 8, 
    "Message": "GISkIJer", 
    "Infos": [
        {
            "VMType": "fXZZhjAF", 
            "Region": "kYivTASD", 
            "SetArch": "BkluUXfw", 
            "VMTypeAlias": "rCVZejTi", 
            "ZoneID": "emDpiIyl"
        }
    ], 
    "RetCode": 0
}
```

## 14.6 查询存储类型-DescribeStorageType

查询存储类型

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域。      | **Yes**  |
| Zone           | string | 可用区。    | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**StorageTypeInfo**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域        | **Yes**  |
| Zone           | string | 可用区      | **Yes**  |
| StorageType    | string | 存储类型    | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeStorageType
&Region=xybVbRgR
&Zone=mcAMGUqD
```

**Response Example**

```
{
    "Action": "DescribeStorageTypeResponse", 
    "TotalCount": 8, 
    "Message": "wGCixRsi", 
    "Infos": [
        {
            "StorageType": "wWLDEBqf", 
            "Region": "gJvDjJly", 
            "Zone": "dRuTGjoe"
        }
    ], 
    "RetCode": 0
}
```

## 14.7 查询操作日志-DescribeOPLogs

查询操作日志

**Request Parameters**

| Parameter name | Type   | Description                       | Required |
| -------------- | ------ | --------------------------------- | -------- |
| Region         | string | 地域。                            | **Yes**  |
| Zone           | string | 可用区。                          | **Yes**  |
| BeginTime      | int    | 开始时间                          | **Yes**  |
| EndTime        | int    | 结束时间                          | **Yes**  |
| ResourceID     | string | 资源ID                            | No       |
| ResourceType   | string | 资源类型                          | No       |
| IsSuccess      | string | 是否操作成功                      | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。     | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。 | No       |

**Response Elements**

| Parameter name | Type   | Description                  | Required |
| -------------- | ------ | ---------------------------- | -------- |
| RetCode        | int    | 返回码                       | **Yes**  |
| Action         | string | 操作名称                     | **Yes**  |
| Message        | string | 错误信息                     | **Yes**  |
| TotalCount     | int    | 总数                         | **Yes**  |
| Infos          | array  | 【数组】返回操作日志对象数组 | **Yes**  |

**OPLogInfo**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| Region         | string | 地域或数据中心           | No       |
| Zone           | string | 可用区                   | No       |
| OPLogsID       | string | 操作日志ID               | No       |
| OPName         | string | 操作名称                 | No       |
| IsSuccess      | string | 是否操作成功， Yes, No   | No       |
| OPTime         | int    | 操作时间                 | No       |
| UserEmail      | string | 操作者账号邮箱           | No       |
| ResourceID     | string | 操作的资源ID             | No       |
| RetCode        | int    | 操作返回的状态码         | No       |
| OpMessage      | string | 操作返回的信息描述       | No       |
| ResourceType   | int    | 操作资源的类型，如虚拟机 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeOPLogs
&Region=cn-zj
&Zone=cn-zj-01
&BeginTime=2
&EndTime=2
&ResourceID=BcRktuUX
&ResourceType=OnYQjtVl
&IsSuccess=iyHhkjDB
&Offset=6
&Limit=8
```

**Response Example**

```
{
    "Action": "DescribeOPLogsResponse", 
    "TotalCount": 7, 
    "Message": "tBpstDyP", 
    "Infos": [
        {
            "UserEmail": "yQhrsGvF", 
            "OPLogsID": "MMvYHzju", 
            "OPTime": 5, 
            "RetCode": 7, 
            "Zone": "HOxgbLOt", 
            "ResourceType": 6, 
            "ResourceID": "dZedlNUs", 
            "Region": "ZNgjEPqm", 
            "OpMessage": "GbzPKpLP", 
            "IsSuccess": "QkLloVhX", 
            "OPName": "gzyXadNY"
        }
    ], 
    "RetCode": 0
}
```

## 14.8 查询地域-DescribeRegion

查询地域

**Request Parameters**

| Parameter name | Type   | Description                       | Required |
| -------------- | ------ | --------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；       | No       |
| Limit          | int    | 列表起始位置偏移量，默认为0。     | No       |
| Offset         | int    | 返回数据长度，默认为20，最大100。 | No       |

**Response Elements**

| Parameter name | Type   | Description      | Required |
| -------------- | ------ | ---------------- | -------- |
| RetCode        | int    | 返回码           | **Yes**  |
| Action         | string | 操作名称         | **Yes**  |
| Message        | string | 返回信息描述     | No       |
| Infos          | string | 地域数组         | No       |
| TotalCount     | int    | 返回现有地域总数 | No       |

**RegionInfo**

| Parameter name | Type   | Description                                      | Required |
| -------------- | ------ | ------------------------------------------------ | -------- |
| Region         | string | 地域                                             | **Yes**  |
| RegionAlias    | string | 地域别名                                         | **Yes**  |
| EndPoints      | string | 地域的API访问地址，仅平台管理员有查看权限。      | **Yes**  |
| CPUCount       | int    | 地域中的vCPU总核数，仅平台管理员有权限查看。     | **Yes**  |
| CPUUsed        | int    | 地域中已分配的vCPU核数，仅平台管理员有权限查看。 | **Yes**  |
| MemoryCap      | int    | 地域中的总内存容量，仅平台管理员有权限查看。     | **Yes**  |
| MemoryUsed     | int    | 地域中已分配的内存容量，仅平台管理员有权限查看。 | **Yes**  |
| GPUCount       | int    | 地域中已分配的GPU数量，仅平台管理员有权限查看。  | **Yes**  |
| GPUUsed        | int    | 地域中已分配的GPU数量，仅平台管理员有权限查看。  | **Yes**  |
| CreateTime     | int    | 地域的创建时间                                   | **Yes**  |
| UpdateTime     | int    | 地域的更新时间                                   | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeRegion
&Region=DlLJdPEF
&Limit=2
&Offset=5
```

**Response Example**

```
{
    "Action": "DescribeRegionResponse", 
    "TotalCount": 8, 
    "Message": "JpwVTaAL", 
    "Infos": "DCLducJK", 
    "RetCode": 0
}
```

