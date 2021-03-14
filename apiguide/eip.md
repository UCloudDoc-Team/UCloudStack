# 7 外网 IP

## 7.1 申请外网IP-AllocateEIP

申请外网IP

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| OperatorName   | string | 线路。目前支持Bgp                                            | **Yes**  |
| Bandwidth      | int    | 带宽，默认值1，默认范围1\~100                                | **Yes**  |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Name           | string | 名称                                                         | **Yes**  |
| Quantity       | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |
| IP             | string | 指定IP                                                       | No       |
| IPVersion      | string | IP版本，默认值IPv4，支持值：IPv4\IPv6                        | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |
| EIPID          | string | 申请的EIP的ID  | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=AllocateEIP
&Region=cn
&Zone=zone-01
&OperatorName=Bgp
&Bandwidth=5
&ChargeType=Dynamic;Month;Year
&Name=yJPiBFDG
&Quantity=3
&IP=UsRyyWpW
&IPVersion=rSkZkkBe
```

**Response Example**

```
{
    "Action": "AllocateEIPResponse", 
    "Message": "KzWgPkfl", 
    "EIPID": "TzvqQnVs", 
    "RetCode": 0
}
```

## 7.2 获取外网IP信息-DescribeEIP

获取外网IP的信息

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| Offset         | string | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | string | 返回数据长度，默认为20，最大100。                            | No       |
| EIPIDs.N       | string | 【数组】外网的 ID。输入有效的 ID。调用方式举例：EIPIDs.0=“one-id”、EIPIDs.1=“two-id” | No       |
| IPVersion      | string | 版本，支持IPv4、IPv6                                         | No       |
| BindResourceID | string | 绑定资源ID，查询该资源绑定的所有 EIP                         | No       |

**Response Elements**

| Parameter name | Type   | Description        | Required |
| -------------- | ------ | ------------------ | -------- |
| RetCode        | int    | 返回码             | **Yes**  |
| Action         | string | 操作名称           | **Yes**  |
| Message        | string | 返回信息描述       | **Yes**  |
| Infos          | array  | 外网IP数组         | **Yes**  |
| Totalcount     | int    | 返回现有外网IP总数 | No       |

**EIPInfo**

| Parameter name   | Type   | Description                                                  | Required |
| ---------------- | ------ | ------------------------------------------------------------ | -------- |
| Region           | string | 地域                                                         | No       |
| Zone             | string | 可用区                                                       | No       |
| EIPID            | string | ID                                                           | No       |
| Bandwidth        | int    | 带宽大小                                                     | No       |
| ChargeType       | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | No       |
| Name             | string | 名称                                                         | No       |
| Remark           | string | 备注                                                         | No       |
| Status           | string | 状态。Allocating：申请中,Free：未绑定,Bounding：绑定中,Bound：已绑定,Unbounding：解绑中,Deleted：已删除,Releasing：销毁中,Released：已销毁,BandwidthChanging：带宽修改中 | No       |
| CreateTime       | int    | 创建时间。时间戳                                             | No       |
| ExpireTime       | int    | 过期时间。时间戳                                             | No       |
| IP               | string | 外网IP                                                       | No       |
| OperatorName     | string | EIP的所属外网网段                                            | No       |
| IPVersion        | string | IP版本,支持值：IPv4\IPv6                                     | No       |
| BindResourceID   | string | 绑定资源ID                                                   | No       |
| BindResourceType | string | 绑定资源类型                                                 | No       |
| ISDefaultGW      | int    | 是否为默认出口，1代表该IP地址为默认出口                      | No       |
| CanDefaultGW     | int    | 所属网段是否为默认路由，1代表所属网段是默认路由；默认路由的网段IP可以设置为默认网络出口 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeEIP
&Region=cn
&Zone=zone-01
&Offset=usWSWEzC
&Limit=uNcfsWHs
&EIPIDs.N=rnvrHmtk
&IPVersion=WwfmTJEk
&BindResourceID=WhCMTSpo
```

**Response Example**

```
{
    "Action": "DescribeEIPResponse", 
    "Totalcount": 8, 
    "Message": "rUXDEXds", 
    "Infos": [
        {
            "Status": "CVyEVZfF", 
            "IPVersion": "uBayIPHI", 
            "Remark": "dUQqriSU", 
            "Name": "ltcadyox", 
            "Zone": "UsWASpTe", 
            "IP": "NkFrtbzc", 
            "Region": "mhLaeNue", 
            "EIPID": "HuCsJPWq", 
            "OperatorName": "AfuwmvUN", 
            "ExpireTime": 9, 
            "Bandwidth": 7, 
            "CanDefaultGW": 5, 
            "BindResourceID": "RyPCuymq", 
            "ChargeType": "OftxMjUM", 
            "BindResourceType": "XIOdjfwJ", 
            "ISDefaultGW": 1, 
            "CreateTime": 9
        }
    ], 
    "RetCode": 0
}
```

## 7.3 获取外网 IP 信息-DescribeEIP

获取 UCloudStack 外网 IP 地址的信息。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| Offset         | string | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | string | 返回数据长度，默认为20，最大100。                            | No       |
| EIPIDs.N       | string | 【数组】外网的 ID。输入有效的 ID。调用方式举例：EIPIDs.0=“one-id”、EIPIDs.1=“two-id” | No       |

**Response Elements**

| Parameter name | Type   | Description        | Required |
| -------------- | ------ | ------------------ | -------- |
| RetCode        | int    | 返回码             | **Yes**  |
| Action         | string | 操作名称           | **Yes**  |
| Message        | string | 返回信息描述       | **Yes**  |
| Infos          | array  | 外网IP数组         | **Yes**  |
| Totalcount     | int    | 返回现有外网IP总数 | No       |

**EIPInfo**

| Parameter name   | Type   | Description                                                  | Required |
| ---------------- | ------ | ------------------------------------------------------------ | -------- |
| Region           | string | 地域                                                         | No       |
| Zone             | string | 可用区                                                       | No       |
| EIPID            | string | ID                                                           | No       |
| Bandwidth        | int    | 带宽大小                                                     | No       |
| ChargeType       | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | No       |
| Name             | string | 名称                                                         | No       |
| Remark           | string | 备注                                                         | No       |
| Status           | string | 状态                                                         | No       |
| CreateTime       | int    | 创建时间。时间戳                                             | No       |
| ExpireTime       | int    | 过期时间。时间戳                                             | No       |
| IP               | string | 外网IP                                                       | No       |
| OperatorName     | string | 线路                                                         | No       |
| BindResourceID   | string | 绑定资源ID                                                   | No       |
| BindResourceType | string | 绑定资源类型                                                 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeEIP
&Region=cn-zj
&Offset=UwNHJIzk
&Limit=tXCYBKpV
&Keyword=BPSkmjMw
&EIPIDs=GsKguokU
&Zone=zone-01
```

**Response Example**

```
{
    "RetCode": 0, 
    "TotalBandwidth": "jVegtYxl", 
    "TotalCount": "bLlZlwih", 
    "Action": "DescribeEIPResponse", 
    "Message": "qoTDfXoV", 
    "Infos": [
        "mUHWcktV"
    ], 
    "Totalcount": 9
}
```

## 7.4 绑定外网 IP-BindEIP

绑定 UCoudStack 外网 IP 地址到虚拟机、负载均衡、NAT 网关资源。

**Request Parameters**

| Parameter name | Type   | Description                                       | Required |
| -------------- | ------ | ------------------------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；中国                   | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；               | **Yes**  |
| ResourceType   | string | 资源类型。VM：虚拟机, LB:负载均衡, NATGW：nat网关 | **Yes**  |
| ResourceID     | string | 资源ID                                            | **Yes**  |
| EIPID          | string | 外网IP的ID                                        | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| RetCode        | int    | 返回码      | **Yes**  |
| Action         | string | 操作名称    | **Yes**  |
| Message        | string | 返回描述    | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=BindEIP
&Region=cn-zj
&ResourceType=uhost;ulb;natgw
&ResourceID=NxXrbAWS
&EIPID=cyJrXdCO
&Zone=zone-01
```

**Response Example**

```
{
    "Action": "BindEIPResponse", 
    "Message": "GCJYfpaY", 
    "RetCode": 0
} 
```

## 7.5 解绑外网 IP-UnBindEIP

将外网 IP 地址从虚拟机、负载均衡、NAT网关资源上解绑，解绑后的外网 IP 地址可重新绑定至其它资源。

**Request Parameters**

| Parameter name | Type   | Description                                       | Required |
| -------------- | ------ | ------------------------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；中国                   | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；               | **Yes**  |
| ResourceType   | string | 资源类型。VM：虚拟机, LB:负载均衡, NATGW：nat网关 | **Yes**  |
| ResourceID     | string | 资源ID                                            | **Yes**  |
| EIPID          | string | 外网IP的ID                                        | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UnBindEIP
&Region=cn-zj
&ResourceType=CJFBgSXZ
&ResourceID=dRaxJjDC
&EIPID=KeLgjcGL
&Zone=zone-01
&Zone=zone-01
```

**Response Example**

```
{
    "Action": "UnBindEIPResponse", 
    "Message": "oDveOBgm", 
    "RetCode": 0
}
```

## 7.6 调整外网IP带宽-ModifyEIPBandwidth

调整外网 IP 地址的带宽

**Request Parameters**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| Region         | string | 地域。       | **Yes**  |
| Zone           | string | 可用区。     | **Yes**  |
| EIPID          | string | 外网IP的ID   | **Yes**  |
| Bandwidth      | int    | 调整后的带宽 | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=ModifyEIPBandwidth
&Region=iStEesas
&Zone=xYqxFUiT
&EIPID=kBtjSIwm
&Bandwidth=8
```

**Response Example**

```
{
    "Action": "ModifyEIPBandwidthResponse", 
    "Message": "XyfqWyHO", 
    "RetCode": 0
}
```

## 7.7 删除外网 IP-ReleaseEIP

删除外网 IP 地址，删除后 IP 地址会进入回收站。

**Request Parameters**

| Parameter name | Type   | Description                       | Required |
| -------------- | ------ | --------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；中国   | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国 | **Yes**  |
| EIPID          | string | 外网IP的ID                        | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回状态描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=ReleaseEIP
&Region=cn-zj
&EIPID=LuYtGRas
&Zone=zone-01
```

**Response Example**

```
{
    "Action": "ReleaseEIPResponse", 
    "Message": "TqrWXuuj", 
    "RetCode": 0
}
```

