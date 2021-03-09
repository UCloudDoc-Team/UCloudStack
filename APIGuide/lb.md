# 10 负载均衡

## 10.1 创建负载均衡-CreateLB

创建负载均衡

**Request Parameters**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| Name           | string | 名称。                                                       | **Yes**  |
| VMType         | string | 运行负载均衡实例的主机机型。枚举值：如 Normal ，表示普通机型； SSD，表示 SSD 机型。（机型由平台管理员修改和指定，可参考获取主机机型接口） | **Yes**  |
| VPCID          | string | LB实例所在的 VPC ID 。                                       | **Yes**  |
| SubnetID       | string | LB 实例所在的子网 ID 。                                      | **Yes**  |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| LBType         | string | 枚举值。LAN：内网，WAN:外网                                  | **Yes**  |
| Quantity       | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |
| Remark         | string | 描述。                                                       | No       |
| SGID           | string | 安全组ID，创建外网LB时为必需                                 | No       |
| EIPID          | string | 外网IP的ID，创建外网LB时为必需                               | No       |

**Response Elements**

| Parameter name | Type   | Description          | Required |
| -------------- | ------ | -------------------- | -------- |
| RetCode        | int    | 返回码               | **Yes**  |
| Action         | string | 操作名称             | **Yes**  |
| Message        | string | 返回信息描述。       | No       |
| LBID           | string | 返回创建的负载均衡ID | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateLB
&Region=cn
&Zone=zone-01
&Name=HNMpmxzF
&VMType=Normal
&VPCID=NyxxuNFk
&SubnetID=cMCdTjaR
&ChargeType=Dynamic
&LBType=LAN
&Quantity=1
&Remark=HZeaXlUU
&SGID=hbCadPDZ
&EIPID=rARpdAMv
```

**Response Example**

```
{
    "Action": "CreateLBResponse", 
    "LBID": "gDuhSoaK", 
    "Message": "yYKVZMWH", 
    "RetCode": 0
}
```

## 10.2 获取负载均衡信息-DescribeLB

获取负载均衡信息

**Request Parameters**

| Parameter name | Type   | Description     | Required |
| -------------- | ------ | --------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| VPCID          | string | VPCID                                                        | No       |
| SubnetID       | string | 子网ID                                                       | No       |
| LBIDs.N        | string | 【数组】负载均衡的 ID。调用方式举例：LBIDs.0=“one-id”、LBIDs.1=“two-id”。 | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description                  | Required |
| -------------- | ------ | ---------------------------- | -------- |
| RetCode        | int    | 返回码                       | **Yes**  |
| Action         | string | 操作名称                     | **Yes**  |
| Message        | string | 返回信息描述。               | **Yes**  |
| TotalCount     | int    | 返回负载均衡总个数。         | **Yes**  |
| Infos          | array  | 【数组】返回负载均衡对象数组 | **Yes**  |

**LBInfo**

| Parameter name  | Type   | Description   | Required |
| --------------- | ------ | ------------------------------------------------------------ | -------- |
| Region          | string | 地域                                                         | **Yes**  |
| Zone            | string | 可用区                                                       | **Yes**  |
| LBID            | string | 负载均衡ID                                                   | **Yes**  |
| Name            | string | 名称                                                         | **Yes**  |
| LBType          | string | 负载均衡类型，枚举值，WAN:外网负载均衡，LAN:内网负载均衡。   | **Yes**  |
| VPCID           | string | VPCID                                                        | **Yes**  |
| SubnetID        | string | 子网ID                                                       | **Yes**  |
| VSCount         | int    | VServer的数量                                                | **Yes**  |
| LBStatus        | string | 状态。Creating:创建中,Running:运行中,Deleting:删除中,Deleted:已删除 | **Yes**  |
| ChargeType      | string | 虚拟机计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| AlarmTemplateID | string | 告警模板ID                                                   | **Yes**  |
| CreateTime      | int    | 创建时间，时间戳                                             | **Yes**  |
| ExpireTime      | int    | 过期时间，时间戳                                             | **Yes**  |
| Remark          | string | 描述                                                         | No       |
| SGID            | string | 安全组 ID ，当LB为内网类型时，该值为空。                     | No       |
| PrivateIP       | string | 负载均衡的内网 IP 地址，当LB为外网类型时，该值为空。         | No       |
| PublicIP        | string | 负载均衡的外网 IP 地址，当LB为内网类型时，该值为空。         | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeLB
&Region=dICiTNUd
&Zone=yucpCJXf
&VPCID=KSHgrcOO
&SubnetID=xJMXkVyK
&LBIDs.N=aZiyGgQJ
&Offset=1
&Limit=5
```

**Response Example**

```
{
    "Action": "DescribeLBResponse", 
    "TotalCount": 3, 
    "Message": "eYpWSjcJ", 
    "Infos": [
        {
            "ExpireTime": 2, 
            "Remark": "YzRxTMHb", 
            "VPCID": "UzAngsAa", 
            "Name": "kJbNmhmx", 
            "Zone": "cLNPTaRV", 
            "VSCount": 8, 
            "Region": "vMADJRaJ", 
            "LBStatus": "Creating", 
            "PrivateIP": "oWKsDJpe", 
            "LBType": "WAN", 
            "LBID": "pikYVEPO", 
            "AlarmTemplateID": "cldVFmoz", 
            "ChargeType": "Dynamic", 
            "SubnetID": "LMmjitUd", 
            "SGID": "PosfeJFB", 
            "CreateTime": 2, 
            "PublicIP": "lZuJbyhz"
        }
    ], 
    "RetCode": 0
}
```

## 10.3 删除负载均衡-DeleteLB

删除负载均衡

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| LBID           | string | 负载均衡ID                          | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteLB
&Region=bkxitZzD
&Zone=JyqTWEDD
&LBID=vnXpkQoP
```

**Response Example**

```
{
    "Action": "DeleteLBResponse", 
    "Message": "uVolzKjE", 
    "RetCode": 0
}
```

## 10.4 创建负载均衡VServer-CreateVS

创建负载均衡VServer

**Request Parameters**

| Parameter name      | Type   | Description                                                  | Required |
| ------------------- | ------ | ------------------------------------------------------------ | -------- |
| Region              | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone                | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| LBID                | string | 负载均衡ID                                                   | **Yes**  |
| Protocol            | string | VServer 的监听协议。枚举值：支持 TCP、UDP、HTTP、HTTPS 四种协议转发。 | **Yes**  |
| Port                | int    | VServer 的监听端口。端口范围为 1\~65535 ，其中 323、9102、9103、9104、9105、60909、60910 被系统占用。 | **Yes**  |
| Scheduler           | string | 负载均衡的调度算法。枚举值：wrr:加权轮训；least_conn:最小连接数；hash:原地址,四层lb使用。ip_hash:七层lb使用 | **Yes**  |
| HealthcheckType     | string | 健康检查类型，枚举值，Port:端口,Path:域名。TCP和UDP协议只支持Port类型。 | **Yes**  |
| PersistenceType     | string | 会话保持类型。枚举值：None:关闭；Auto:自动生成；Manual:手动生成 。当协议为 TCP 时，该值不生效，会话保持和选择的调度算法相关；当协议为 UDP 时 Auto 表示开启会话保持 。 | No       |
| PersistenceKey      | string | 会话保持KEY，会话保持类型为Manual时为必填项，仅当 VServer 协议为 HTTP 时有效。 | No       |
| KeepaliveTimeout    | int    | 负载均衡的连接空闲超时时间，单位为秒，默认值为 60s 。        | No       |
| Path                | string | HTTP 健康检查的路径，健康检查类型为 HTTP 检查时为必填项。当健康检查类型为端口检查时，该值为空。 | No       |
| Domain              | string | HTTP 健康检查时校验请求的 HOST 字段中的域名。当健康检查类型为端口检查时，该值为空。 | No       |
| ServerCertificateID | string | 服务器证书ID，用于证明服务器的身份，仅当 VServer监听协议为 HTTPS时有效。 | No       |
| CACertificateID     | string | CA证书ID，用于验证客户端证书的签名，仅当VServer监听协议为 HTTPS 且 SSLMode 为双向认证时有效。 | No       |
| SSLMode             | string | SSL认证模式,HTTPS协议下必传,取值范围["simplex","duplex"]分别表示单向认证和双向认证。 | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | No       |
| VSID           | string | 返回创建的VSID | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateVS
&Region=cn
&Zone=zone-01
&LBID=eSeMrMzu
&Protocol=TCP|UDP|HTTP
&Port=6
&Scheduler=wrr
&HealthcheckType=Port
&PersistenceType=None
&PersistenceKey=CGZYdHEW
&KeepaliveTimeout=9
&Path=rLyGsgWv
&Domain=iShXSkya
&ServerCertificateID=hhYiLUcm
&CACertificateID=ajznmSxy
&SSLMode=fmKPGstD
```

**Response Example**

```
{
    "Action": "CreateVSResponse", 
    "Message": "yspqZwgH", 
    "RetCode": 0, 
    "VSID": "EfjgRIrn"
}
```

## 10.5 获取负载均衡VServer-DescribeVS

获取负载均衡 VServer 信息

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| LBID           | string | 负载均衡ID                                                   | **Yes**  |
| VSIDs.N        | string | 【数组】VServer的 ID。调用方式举例：VSIDs.0=“one-id”、VSIDs.1=“two-id”。 | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description                       | Required |
| -------------- | ------ | --------------------------------- | -------- |
| RetCode        | int    | 返回码                            | **Yes**  |
| Action         | string | 操作名称                          | **Yes**  |
| Message        | string | 返回信息描述。                    | **Yes**  |
| TotalCount     | int    | 返回当前负载均衡 VServer 总个数。 | **Yes**  |
| Infos          | array  | 【数组】返回VServer对象数组       | **Yes**  |

**VSInfo**

| Parameter name      | Type   | Description                                                  | Required |
| ------------------- | ------ | ------------------------------------------------------------ | -------- |
| VSID                | string | VServer的ID                                                  | **Yes**  |
| LBID                | string | VServer 所属的负载均衡 ID                                    | **Yes**  |
| Protocol            | string | 协议                                                         | **Yes**  |
| Port                | int    | 端口                                                         | **Yes**  |
| Scheduler           | string | 负载均衡的调度算法。枚举值：wrr:加权轮训；least_conn:最小连接数；hash:原地址,四层lb使用。ip_hash:七层lb使用 | **Yes**  |
| PersistenceType     | string | 会话保持类型。枚举值：None:关闭；Auto:自动生成；Manual:手动生成 。当协议为 TCP 时，该值为空；当协议为 UDP 时 Auto 表示开启会话保持 。 | **Yes**  |
| HealthcheckType     | string | 负载均衡的健康检查类型。枚举值：Port:端口检查；Path: HTTP检查 。 | **Yes**  |
| KeepaliveTimeout    | int    | 负载均衡的连接空闲超时时间，单位为秒，默认值为 60s 。当 VServer 协议为 UDP 时，该值为空。 | **Yes**  |
| VSStatus            | string | VServer 的资源状态。枚举值，Available:可用,Updating:更新中,Deleted:已删除 。 | **Yes**  |
| RSHealthStatus      | string | 健康检查状态，枚举值，Empty:全部异常,Parts:部分异常,All:正常 | **Yes**  |
| CreateTime          | int    | 创建时间，时间戳                                             | **Yes**  |
| UpdateTime          | int    | 更新时间，时间戳                                             | **Yes**  |
| AlarmTemplateID     | string | 告警模板ID                                                   | **Yes**  |
| SSLMode             | string | SSL认证模式,取值范围["simplex","duplex"]分别表示单向认证和双向认证。 | No       |
| PersistenceKey      | string | 会话保持KEY，仅当 VServer 协议为 HTTP 且会话保持为手动时有效。 | No       |
| Path                | string | HTTP 健康检查的路径。当健康检查类型为端口检查时，该值为空。  | No       |
| Domain              | string | HTTP 健康检查时校验请求的 HOST 字段中的域名。当健康检查类型为端口检查时，该值为空。 | No       |
| RSInfos             | array  | 前 VServer 中已添加的服务节点信息。                          | No       |
| VSPolicyInfos       | array  | 转发规则 Policy 的规则信息                                   | No       |
| ServerCertificateID | string | 服务器证书ID，用于证明服务器的身份。仅当 VServer监听协议为 HTTPS 时有效。 | No       |
| CACertificateID     | string | CA证书ID，用于验证客户端证书的签名。仅当VServer监听协议为 HTTPS 且 SSLMode 为双向认证时有效。 | No       |

**RSInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| LBID           | string | 服务节点所属的负载均衡 ID                                    | **Yes**  |
| RSID           | string | 服务节点的 ID                                                | **Yes**  |
| VSID           | string | 服务节点所属的 VServer ID                                    | **Yes**  |
| Name           | string | 服务节点的资源名称                                           | **Yes**  |
| BindResourceID | string | 绑定的资源ID                                                 | **Yes**  |
| IP             | string | 服务节点的内网 IP 地址                                       | **Yes**  |
| Port           | int    | 服务节点暴露的服务端口号                                     | **Yes**  |
| Weight         | int    | 服务节点的权重                                               | **Yes**  |
| RSMode         | string | 节点模式。枚举值，Enabling:开启中,Enable:已启用,Disabling:禁用中,Disable:已禁用 | **Yes**  |
| RSStatus       | string | RSStatus 的描述修改为：状态，枚举值，Creating:创建中,Inactive:无效,Active:有效,Updating:更新中,Deleting:删除中,Deleted:已删除。其中有效代表节点服务健康，无效代表节点服务异常。 | **Yes**  |
| CreateTime     | int    | 创建时间，时间戳                                             | **Yes**  |
| UpdateTime     | int    | 更新时间，时间戳                                             | **Yes**  |

**VSPolicyInfo**

| Parameter name | Type   | Description                                              | Required |
| -------------- | ------ | -------------------------------------------------------- | -------- |
| LBID           | string | 负载均衡ID                                               | **Yes**  |
| VSID           | string | VServerID                                                | **Yes**  |
| PolicyID       | string | 内容转发规则ID                                           | **Yes**  |
| Domain         | string | 内容转发规则关联的请求域名，值可为空，即代表仅匹配路径。 | **Yes**  |
| Path           | string | 内容转发规则关联的请求访问路径，如 "/" 。                | **Yes**  |
| PolicyStatus   | string | 状态，枚举值，Available:有效,Deleted:已删除              | **Yes**  |
| CreateTime     | int    | 创建时间，时间戳                                         | **Yes**  |
| UpdateTime     | int    | 更新时间，时间戳                                         | **Yes**  |
| RSInfos        | array  | 转发规则关联的服务节点信息                               | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeVS
&Region=CPOjKYJj
&Zone=WBJeDVfo
&LBID=jTtCTJTh
&VSIDs.N=dCmbyxHJ
&Offset=1
&Limit=7
```

**Response Example**

```
{
    "Action": "DescribeVSResponse", 
    "TotalCount": 7, 
    "Message": "kxxUeJfM", 
    "Infos": [
        {
            "CACertificateID": "lNruhpqA", 
            "PersistenceType": "None", 
            "Domain": "pvNxBnQY", 
            "Protocol": "axRwVusJ", 
            "VSPolicyInfos": [
                {
                    "Domain": "IDMoHeoG", 
                    "UpdateTime": 9, 
                    "VSID": "GVFbTmIT", 
                    "PolicyStatus": "Available", 
                    "LBID": "zHQKrFXH", 
                    "PolicyID": "zhhfvipY", 
                    "Path": "TZuliwTB", 
                    "CreateTime": 4, 
                    "RSInfos": [
                        {
                            "UpdateTime": 1, 
                            "Name": "migqkpKK", 
                            "Weight": 8, 
                            "IP": "oAUJRzRL", 
                            "RSStatus": "Creating", 
                            "VSID": "sONSWMOj", 
                            "CreateTime": 1, 
                            "LBID": "iAYAcAGG", 
                            "RSID": "iEtcrhIe", 
                            "BindResourceID": "DlPhQxWL", 
                            "Port": 6, 
                            "RSMode": "Enabling"
                        }
                    ]
                }
            ], 
            "RSHealthStatus": "Empty", 
            "UpdateTime": 1, 
            "VSID": "FuJXOwaQ", 
            "HealthcheckType": "Port", 
            "LBID": "XenNKVkP", 
            "CreateTime": 3, 
            "AlarmTemplateID": "hOqvPKXw", 
            "Scheduler": "eVgWEVhB", 
            "KeepaliveTimeout": 4, 
            "SSLMode": "uMRyDKFo", 
            "Path": "bgyqHVaU", 
            "VSStatus": "Available", 
            "PersistenceKey": "hCCVtRaj", 
            "Port": 6, 
            "ServerCertificateID": "qLuTYvCS", 
            "RSInfos": [
                {
                    "UpdateTime": 9, 
                    "Name": "tysEPtls", 
                    "Weight": 5, 
                    "IP": "xboMKoFS", 
                    "RSStatus": "Creating", 
                    "VSID": "UlbYhLzh", 
                    "CreateTime": 2, 
                    "LBID": "WkkSzlEj", 
                    "RSID": "tElbFTxw", 
                    "BindResourceID": "VAaPHfHk", 
                    "Port": 9, 
                    "RSMode": "Enabling"
                }
            ]
        }
    ], 
    "RetCode": 0
}
```

## 10.6 修改负载均衡VServer-UpdateVS

修改负载均衡VServer

**Request Parameters**

| Parameter name      | Type   | Description                                                  | Required |
| ------------------- | ------ | ------------------------------------------------------------ | -------- |
| Region              | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone                | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| VSID                | string | 需要更新的VSID                                               | **Yes**  |
| LBID                | string | VServer 监听器所属的负载均衡 ID                              | **Yes**  |
| Scheduler           | string | 负载均衡的调度算法。枚举值：wrr:加权轮训；least_conn:最小连接数；hash:原地址,四层lb使用。ip_hash:七层lb使用 | No       |
| KeepaliveTimeout    | int    | 负载均衡的连接空闲超时时间，单位为秒，默认值为 60s 。当 VServer 协议为 UDP 时，该值为空。 | No       |
| HealthcheckType     | string | 负载均衡的健康检查类型。枚举值：Port:端口检查；Path: HTTP检查 。仅当 VServer 协议类型为 HTTP 时，才可进行 HTTP 检查。 | No       |
| Path                | string | HTTP 健康检查的路径，健康检查类型为 HTTP 检查时为必填项。当健康检查类型为端口检查时，该值为空。 | No       |
| Domain              | string | HTTP 健康检查时校验请求的 HOST 字段中的域名。当健康检查类型为端口检查时，该值为空。 | No       |
| Port                | int    | VServer 监听端口                                             | No       |
| PersistenceType     | string | 会话保持类型。枚举值：None:关闭；Auto:自动生成；Manual:手动生成 。当协议为 TCP 时，该值不生效，会话保持和选择的调度算法相关；当协议为 UDP 时 Auto 表示开启会话保持 。 | No       |
| PersistenceKey      | string | 会话保持KEY，会话保持类型为Manual时为必填项，仅当 VServer 协议为 HTTP 时有效。 | No       |
| ServerCertificateID | string | 服务器证书ID，用于证明服务器的身份，仅当 VServer监听协议为 HTTPS 时有效。 | No       |
| CACertificateID     | string | CA证书ID，用于验证客户端证书的签名，仅当VServer监听协议为 HTTPS 且 SSLMode 为双向认证时有效。 | No       |
| SSLMode             | string | SSL认证模式,HTTPS协议下必传,取值范围["simplex","duplex"]分别表示单向认证和双向认证。 | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UpdateVS
&Region=cn
&Zone=zone-01
&VSID=drNhHRuk
&LBID=HlYbzVoO
&Scheduler=wrr
&KeepaliveTimeout=1
&HealthcheckType=Port
&Path=qLReLofN
&Domain=wDkfxuMq
&Port=4
&PersistenceType=None
&PersistenceKey=EOkGXtOS
&ServerCertificateID=lfcDlWhs
&CACertificateID=gTlaOszR
&SSLMode=fFGxzLCr
```

**Response Example**

```
{
    "Action": "UpdateVSResponse", 
    "Message": "UDgQbPVN", 
    "RetCode": 0
}
```

## 10.7 删除VServer-DeleteVS

删除VServer

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| LBID           | string | VServer 监听器所属的负载均衡 ID     | **Yes**  |
| VSID           | string | 负载均衡VServer监听器ID             | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteVS
&Region=WweqaCeP
&Zone=kOiSpcOM
&LBID=IIXTVenT
&VSID=IIvqJkRZ
```

**Response Example**

```
{
    "Action": "DeleteVSResponse", 
    "Message": "aQzOhBjG", 
    "RetCode": 0
}
```

## 10.8 添加服务节点-CreateRS

为负载均衡的 VServer 添加后端服务节点。

**Request Parameters**

| Parameter name | Type   | Description                                               | Required |
| -------------- | ------ | --------------------------------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                               | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                       | **Yes**  |
| LBID           | string | 负载均衡ID                                                | **Yes**  |
| VSID           | string | VServer的ID                                               | **Yes**  |
| BindResourceID | string | 服务节点的资源 ID ，仅支持添加与 LB 相同 VPC 的虚拟机资源 | **Yes**  |
| Port           | int    | 服务节点暴露的服务端口号                                  | **Yes**  |
| Weight         | int    | 服务节点的权重                                            | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | No       |
| RSID           | string | 返回创建的RSID | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateRS
&Region=cn
&Zone=zone-01
&LBID=MCZXKjpf
&VSID=rQvVbWwO
&BindResourceID=TCP|UDP|HTTP
&Port=6
&Weight=NaN
```

**Response Example**

```
{
    "Action": "CreateRSResponse", 
    "Message": "XlHRYxho", 
    "RSID": "cnpbAnET", 
    "RetCode": 0
}
```

## 10.9 获取服务节点信息-DescribeRS

获取负载均衡服务的服务节点信息

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| LBID           | string | 负载均衡ID                                                   | **Yes**  |
| VSID           | string | VServer的ID                                                  | No       |
| RSIDs.N        | string | 【数组】RServer的 ID。调用方式举例：RSIDs.0=“one-id”、RSIDs.1=“two-id”。 | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description                       | Required |
| -------------- | ------ | --------------------------------- | -------- |
| RetCode        | int    | 返回码                            | **Yes**  |
| Action         | string | 操作名称                          | **Yes**  |
| Message        | string | 返回信息描述。                    | **Yes**  |
| TotalCount     | int    | 返回该负载均衡下VServer的总个数。 | **Yes**  |
| Infos          | array  | 【数组】返回VServer对象数组       | **Yes**  |

**RSInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| LBID           | string | 服务节点所属的负载均衡 ID                                    | **Yes**  |
| RSID           | string | 服务节点的 ID                                                | **Yes**  |
| VSID           | string | 服务节点所属的 VServer ID                                    | **Yes**  |
| Name           | string | 服务节点的资源名称                                           | **Yes**  |
| BindResourceID | string | 绑定的资源ID                                                 | **Yes**  |
| IP             | string | 服务节点的内网 IP 地址                                       | **Yes**  |
| Port           | int    | 服务节点暴露的服务端口号                                     | **Yes**  |
| Weight         | int    | 服务节点的权重                                               | **Yes**  |
| RSMode         | string | 节点模式。枚举值，Enabling:开启中,Enable:已启用,Disabling:禁用中,Disable:已禁用 | **Yes**  |
| RSStatus       | string | RSStatus 的描述修改为：状态，枚举值，Creating:创建中,Inactive:无效,Active:有效,Updating:更新中,Deleting:删除中,Deleted:已删除。其中有效代表节点服务健康，无效代表节点服务异常。 | **Yes**  |
| CreateTime     | int    | 创建时间，时间戳                                             | **Yes**  |
| UpdateTime     | int    | 更新时间，时间戳                                             | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeRS
&Region=wFHWqfdx
&Zone=fnxxGzty
&LBID=wkVavBTx
&VSID=npdQKYdS
&RSIDs.N=OyAGDYUP
&Offset=6
&Limit=5
```

**Response Example**

```
{
    "Action": "DescribeRSResponse", 
    "TotalCount": 5, 
    "Message": "KYpVZQwu", 
    "Infos": [
        {
            "UpdateTime": 2, 
            "Name": "DVVbVNzZ", 
            "Weight": 6, 
            "IP": "bIYvNbXN", 
            "RSStatus": "Creating", 
            "VSID": "XGTFrjPy", 
            "CreateTime": 5, 
            "LBID": "UCQgivkx", 
            "RSID": "MfClkxGJ", 
            "BindResourceID": "ILpBLIjX", 
            "Port": 2, 
            "RSMode": "Enabling"
        }
    ], 
    "RetCode": 0
}
```

## 10.10 启用服务节点-EnableRS

启用负载均衡的单个服务节点

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| RSID           | string | RServer的ID                         | **Yes**  |
| VSID           | string | VServer的ID                         | **Yes**  |
| LBID           | string | 负载均衡ID                          | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=EnableRS
&Region=HcHWTbvj
&Zone=hZEizudl
&RSID=YwrgqtLy
&VSID=YsHCfqbY
&LBID=wZbSQcfB
```

**Response Example**

```
{
    "Action": "EnableRSResponse", 
    "Message": "QyUeFjvH", 
    "RetCode": 0
}
```

## 10.11 禁用服务节点-DisableRS

禁用负载均衡的单个服务节点

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| RSID           | string | RServer的ID                         | **Yes**  |
| VSID           | string | VServer的ID                         | **Yes**  |
| LBID           | string | 负载均衡ID                          | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DisableRS
&Region=eXszliox
&Zone=iBJYlWFl
&NATGWID=fqkyhbWT
&VSID=CXbclGlO
&LBID=LybVDFIf
```

**Response Example**

```
{
    "Action": "DisableRSResponse", 
    "Message": "LbJgakZp", 
    "RetCode": 0
}
```

## 10.12 修改服务节点-UpdateRS

修改负载均衡的服务节点

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| LBID           | string | VServer 监听器所属的负载均衡 ID     | **Yes**  |
| VSID           | string | RServer所属的VServer的ID            | **Yes**  |
| RSID           | string | RServer的ID                         | **Yes**  |
| Port           | int    | 端口号                              | No       |
| Weight         | int    | 权重                                | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UpdateRS
&Region=cn
&Zone=zone-01
&LBID=RarrsdHr
&VSID=VrJWKImc
&RSID=sGIACOne
&Port=7
&Weight=NaN
```

**Response Example**

```
{
    "Action": "UpdateRSResponse", 
    "Message": "rxVNLrRV", 
    "RetCode": 0
}
```

## 10.13 移除服务节点-DeleteRS

移除负载均衡的单个服务节点

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| RSID           | string | RServer的ID                         | **Yes**  |
| VSID           | string | VServer的ID                         | **Yes**  |
| LBID           | string | 负载均衡ID                          | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteRS
&Region=fcCRslRm
&Zone=sbiBevID
&RSID=UrudIGhy
&VSID=QKBPGTdb
&LBID=lLDdStol
```

**Response Example**

```
{
    "Action": "DeleteRSResponse", 
    "Message": "hFFPIKrV", 
    "RetCode": 0
}
```

## 10.14 创建内容转发规则-CreateVSPolicy

创建七层负载均衡内容转发规则，仅当 VServer 的监听协议为 HTTP 和 HTTPS 时有效。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| LBID           | string | 负载均衡ID                                                   | **Yes**  |
| VSID           | string | VServer的ID                                                  | **Yes**  |
| RSIDs.N        | string | 【数组】内容转发规则应用的服务节点的 ID，来源于 VServer 中添加的服务节点。调用方式举例：RSIDs.0=“one-id”、RSIDs.1=“two-id”。 | **Yes**  |
| Path           | string | 内容转发规则关联的请求访问路径，如 "/" 。域名和路径至少需要指定一项，且域名和路径的组合在一个 VServer 中必须唯一。 | No       |
| Domain         | string | 内容转发规则关联的请求域名，值可为空，即代表仅匹配路径。域名和路径至少需要指定一项，且域名和路径的组合在一个 VServer 中必须唯一。 | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Message        | string | 返回信息描述。           | No       |
| PolicyID       | string | 返回创建的内容转发规则ID | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateVSPolicy
&Region=cn
&Zone=zone-01
&LBID=vfWfhpBM
&VSID=aLLthkrS
&RSIDs.N=IdKTDMSo
&Path=pWJqqfba
&Domain=cZdfEWho
```

**Response Example**

```
{
    "Action": "CreateVSPolicyResponse", 
    "Message": "xzMjngvL", 
    "PolicyID": "xXfvlTgK", 
    "RetCode": 0
}
```

## 10.15 获取内容转发规则信息-DescribeVSPolicy

获取七层负载均衡内容转发规则信息，仅当 VServer 的监听协议为 HTTP 和 HTTPS 时有效。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| LBID           | string | 负载均衡ID                                                   | **Yes**  |
| VSID           | string | VServerID                                                    | No       |
| PolicyIDs.N    | string | 【数组】七层负载均衡内容转发规则的 ID。调用方式举例：PolicyIDs.0=“one-id”、PolicyIDs.1=“two-id” | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description                        | Required |
| -------------- | ------ | ---------------------------------- | -------- |
| RetCode        | int    | 返回码                             | **Yes**  |
| Action         | string | 操作名称                           | **Yes**  |
| Message        | string | 返回信息描述。                     | **Yes**  |
| TotalCount     | int    | 返回内容转发规则的总个数。         | **Yes**  |
| Infos          | array  | 【数组】返回内容分转发规则对象数组 | **Yes**  |

**VSPolicyInfo**

| Parameter name | Type   | Description                                              | Required |
| -------------- | ------ | -------------------------------------------------------- | -------- |
| LBID           | string | 负载均衡ID                                               | **Yes**  |
| VSID           | string | VServerID                                                | **Yes**  |
| PolicyID       | string | 内容转发规则ID                                           | **Yes**  |
| Domain         | string | 内容转发规则关联的请求域名，值可为空，即代表仅匹配路径。 | **Yes**  |
| Path           | string | 内容转发规则关联的请求访问路径，如 "/" 。                | **Yes**  |
| PolicyStatus   | string | 状态，枚举值，Available:有效,Deleted:已删除              | **Yes**  |
| CreateTime     | int    | 创建时间，时间戳                                         | **Yes**  |
| UpdateTime     | int    | 更新时间，时间戳                                         | **Yes**  |
| RSInfos        | array  | 转发规则关联的服务节点信息                               | **Yes**  |

**RSInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| LBID           | string | 服务节点所属的负载均衡 ID                                    | **Yes**  |
| RSID           | string | 服务节点的 ID                                                | **Yes**  |
| VSID           | string | 服务节点所属的 VServer ID                                    | **Yes**  |
| Name           | string | 服务节点的资源名称                                           | **Yes**  |
| BindResourceID | string | 绑定的资源ID                                                 | **Yes**  |
| IP             | string | 服务节点的内网 IP 地址                                       | **Yes**  |
| Port           | int    | 服务节点暴露的服务端口号                                     | **Yes**  |
| Weight         | int    | 服务节点的权重                                               | **Yes**  |
| RSMode         | string | 节点模式。枚举值，Enabling:开启中,Enable:已启用,Disabling:禁用中,Disable:已禁用 | **Yes**  |
| RSStatus       | string | RSStatus 的描述修改为：状态，枚举值，Creating:创建中,Inactive:无效,Active:有效,Updating:更新中,Deleting:删除中,Deleted:已删除。其中有效代表节点服务健康，无效代表节点服务异常。 | **Yes**  |
| CreateTime     | int    | 创建时间，时间戳                                             | **Yes**  |
| UpdateTime     | int    | 更新时间，时间戳                                             | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeVSPolicy
&Region=GoBCJRIr
&Zone=tlPJijjU
&LBID=JvahrZut
&VSID=bQUloIIJ
&PolicyIDs.N=bBqfVPgh
&Offset=2
&Limit=5
```

**Response Example**

```
{
    "Action": "DescribeVSPolicyResponse", 
    "TotalCount": 3, 
    "Message": "hJyXRpbp", 
    "Infos": [
        {
            "Domain": "KtROtIQz", 
            "UpdateTime": 7, 
            "VSID": "ItTEcZBA", 
            "PolicyStatus": "Available", 
            "LBID": "jNtxJpUX", 
            "PolicyID": "fKGcHwJg", 
            "Path": "wDwBCGud", 
            "CreateTime": 2, 
            "RSInfos": [
                {
                    "UpdateTime": 2, 
                    "Name": "UFLzBytK", 
                    "Weight": 4, 
                    "IP": "jfnMUKDg", 
                    "RSStatus": "Creating", 
                    "VSID": "OGUiRuKY", 
                    "CreateTime": 7, 
                    "LBID": "nNZSWzXU", 
                    "RSID": "dvyIwlSt", 
                    "BindResourceID": "wMibyaZt", 
                    "Port": 3, 
                    "RSMode": "Enabling"
                }
            ]
        }
    ], 
    "RetCode": 0
}
```

## 10.16 更新内容转发规则-UpdateVSPolicy

更新七层负载均衡内容转发规则，仅当 VServer 的监听协议为 HTTP 和 HTTPS 时有效。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| LBID           | string | 负载均衡ID                                                   | **Yes**  |
| VSID           | string | VServer的ID                                                  | **Yes**  |
| PolicyID       | string | 内容转发规则ID                                               | **Yes**  |
| Domain         | string | 内容转发规则关联的请求域名，值可为空，即代表仅匹配路径。     | No       |
| Path           | string | 内容转发规则关联的请求访问路径，如 "/" 。                    | No       |
| RSIDs.N        | string | 【数组】RServer的 ID。调用方式举例：RSIDs.0=“one-id”、RSIDs.1=“two-id”。 | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UpdateVSPolicy
&Region=cn
&Zone=zone-01
&LBID=xQNYTbBu
&VSID=JXPvzOnx
&PolicyID=zeRsqxix
&Domain=UQeDdZSp
&Path=LDXujumd
&RSIDs.N=vaqpUSrR
```

**Response Example**

```
{
    "Action": "UpdateVSPolicyResponse", 
    "Message": "svbsDztL", 
    "RetCode": 0
}
```

## 10.17 删除内容转发规则-DeleteVSPolicy

删除七层负载均衡内容转发规则，仅当 VServer 的监听协议为 HTTP 和 HTTPS 时有效。

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| PolicyID       | string | 内容转发规则ID                      | **Yes**  |
| LBID           | string | 负载均衡ID                          | **Yes**  |
| VSID           | string | VServer的ID                         | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteVSPolicy
&Region=cn
&Zone=zone-01
&PolicyID=atuSysrz
&LBID=TeyqNQZH
&VSID=DrUyYqyb
```

**Response Example**

```
{
    "Action": "DeleteVSPolicyResponse", 
    "Message": "IBRYMiwh", 
    "RetCode": 0
}
```

## 10.18 创建证书-CreateCertificate

创建证书

**Request Parameters**

| Parameter name  | Type   | Description                                                  | Required |
| --------------- | ------ | ------------------------------------------------------------ | -------- |
| Region          | string | 地域。                                                       | **Yes**  |
| Zone            | string | 可用区。                                                     | **Yes**  |
| Certificate     | string | 证书内容                                                     | **Yes**  |
| Name            | string | 证书名称                                                     | **Yes**  |
| CertificateType | string | 证书类型，枚举值["ServerCrt","CACrt"]。分别表示服务器证书和CA证书，CA证书仅在双向认证的时有效。 | **Yes**  |
| PrivateKey      | string | 私钥内容,服务器证书必传,证书类型为CA证书时无效。             | No       |
| Remark          | string | 证书描述                                                     | No       |

**Response Elements**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| RetCode        | int    | 返回码      | **Yes**  |
| Action         | string | 操作名称    | **Yes**  |
| Message        | string | 错误描述    | **Yes**  |
| CertificateID  | string | 证书ID      | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateCertificate
&Region=cn-zj
&Zone=cn-zj-01
&ProjectId=htENHVIj
&Certificate=kUbusDVn
&PrivateKey=cwpajKCz
&CertificateName=LdPvXaLL
&CertificateType=zzBMnjIH
&Remark=bLtpxlJV
&Certificate=HJlGtXLl
&PrivateKey=ULqfPvaz
&CertificateName=YPcrjwMM
&CertificateType=BZplOOit
&Remark=jAJpfciQ
```

**Response Example**

```
{
    "Action": "CreateCertificateResponse", 
    "Message": "yiImQPEa", 
    "CertificateID": "yvklxOAN", 
    "RetCode": 0
}
```

## 10.19 查询证书-DescribeCertificate

查询证书

**Request Parameters**

| Parameter name   | Type   | Description                                                  | Required |
| ---------------- | ------ | ------------------------------------------------------------ | -------- |
| Region           | string | 地域。                                                       | **Yes**  |
| Zone             | string | 可用区。                                                     | **Yes**  |
| CertificateType  | string | 证书类型，枚举值["ServerCrt","CACrt"]，分别表示服务器证书和CA证书。 | No       |
| CertificateIDs.N | string | 证书ID列表                                                   | No       |
| Limit            | int    | 返回数据长度，默认为20，最大100                              | No       |
| Offset           | int    | 列表起始位置偏移量，默认为0                                  | No       |

**Response Elements**

| Parameter name | Type   | Description        | Required |
| -------------- | ------ | ------------------ | -------- |
| RetCode        | int    | 返回码             | **Yes**  |
| Action         | string | 操作名称           | **Yes**  |
| Message        | string | 返回信息描述       | **Yes**  |
| TotalCount     | int    | 证书总个数         | **Yes**  |
| Infos          | array  | [数组]证书对象数组 | No       |

**CertificateInfo**

| Parameter name          | Type   | Description                           | Required |
| ----------------------- | ------ | ------------------------------------- | -------- |
| Region                  | string | 地域                                  | No       |
| Zone                    | string | 可用区                                | No       |
| VSInfos                 | array  | 关联的vs的信息                        | **Yes**  |
| CertificateID           | string | 证书ID                                | No       |
| CertificateType         | string | 证书类型，枚举值["ServerCrt","CACrt"] | No       |
| Name                    | string | 证书名                                | No       |
| Remark                  | string | 证书描述                              | No       |
| CertificateContent      | string | 证书内容                              | No       |
| Privatekey              | string | 私钥内容                              | No       |
| CommonName              | string | 主域名                                | No       |
| SubjectAlternativeNames | array  | 备域名                                | No       |
| CreateTime              | int    | 创建时间（平台创建时间）              | No       |
| ExpireTime              | int    | 证书内容的过期时间                    | No       |
| Fingerprint             | string | 证书指纹                              | No       |

**BindVSInfo**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| LBID           | string | LB ID       | No       |
| LBName         | string | LB名称      | No       |
| VSID           | string | VS ID       | No       |
| Protocol       | string | VS的协议    | No       |
| Port           | int    | VS的端口    | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeCertificate
&Region=cn-zj
&Zone=cn-zj-01
&CertificateType=VRdVNQQr
&CertificateIDs.N=vzaHTLGq
&Limit=9
&Offset=5
```

**Response Example**

```
{
    "Action": "DescribeCertificateResponse", 
    "TotalCount": 6, 
    "Message": "EdFnAQne", 
    "Infos": [
        {
            "Remark": "laVLBJXA", 
            "CertificateID": "xayDwHju", 
            "SubjectAlternativeNames": [
                "fPKnfEwM"
            ], 
            "Zone": "SbVgWxFB", 
            "CommonName": "LJrFmcbM", 
            "Region": "opVMInhl", 
            "Privatekey": "OgUqbDPe", 
            "ExpireTime": 6, 
            "CertificateContent": "CSWOaMWk", 
            "VSInfos": [
                {
                    "LBID": "UKBRFmaJ", 
                    "Protocol": "wbzMzEYV", 
                    "LBName": "LyuJdOaj", 
                    "VSID": "ABizVGDn", 
                    "Port": "LwgRAeoC"
                }
            ], 
            "CertificateName": "wdDWyBjN", 
            "Fingerprint": "HrlaJucO", 
            "CertificateType": "LKXSWmpk", 
            "CreateTime": 5
        }
    ], 
    "RetCode": 0
}
```

## 10.20 删除证书-DeleteCertificate

删除证书

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域。      | **Yes**  |
| Zone           | string | 可用区。    | **Yes**  |
| CertificateID  | string | 证书ID      | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteCertificate
&Region=cn-zj
&Zone=cn-zj-01
&CertificateID=sHDtfKYt
```

**Response Example**

```
{
    "Action": "DeleteCertificateResponse", 
    "Message": "NaObKjbg", 
    "RetCode": 0
}
```

