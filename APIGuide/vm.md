# 2 虚拟机

## 2.1 创建虚拟机-CreateVMInstance

创建虚拟机

**Request Parameters**

| Parameter name  | Type   | Description                                                  | Required |
| --------------- | ------ | ------------------------------------------------------------ | -------- |
| Region          | string | 地域或数据中心。枚举值：cn,表示中国；                        | **Yes**  |
| Zone            | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| Name            | string | 虚拟机名称。可输入如：myVM。名称只能包含中英文、数字以及- _ .且1-30个字符。 | **Yes**  |
| VMType          | string | 虚拟机所在宿主机的集群类型，代表不同架构、不同型号的 CPU 或硬件特征。枚举值：Normal，表示普通；SSD，表示SSD。 | **Yes**  |
| ImageID         | string | 镜像 ID。基础镜像 ID 或者自制镜像 ID。如：cn-image-centos-74。 | **Yes**  |
| CPU             | int    | CPU个数，如1，2，4，8，16，32，64等。                        | **Yes**  |
| Memory          | int    | 内存容量，如1024，2048，4096，8192，16384，32768，65535等。  | **Yes**  |
| BootDiskSetType | string | 系统盘类型。枚举值：Normal，表示普通；SSD，表示SSD；         | **Yes**  |
| DataDiskSetType | string | 数据盘类型。枚举值：Normal，表示普通；SSD，表示SSD；         | **Yes**  |
| VPCID           | string | 虚拟机所属 VPC ID。                                          | **Yes**  |
| SubnetID        | string | 虚拟机所属子网 ID。                                          | **Yes**  |
| WANSGID         | string | 外网安全组 ID。输入“有效”状态的安全组的ID。                  | **Yes**  |
| ChargeType      | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Password        | string | 密码。可输入如：ucloud.cn。密码长度限6-30个字符；需要同时包含两项或以上（大写字母/小写字母/数字/特殊符号)；windows不能包含用户名（administrator）中超过2个连续字符的部分。 | **Yes**  |
| DataDiskSpace   | int    | 数据盘大小，单位 GB。默认值为0。范围：【0，8000】，步长10。  | No       |
| Quantity        | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |
| InternalIP      | string | 指定内网IP。输入有效的指定内网 IP，不指定时系统将自动从子网分配 IP 地址。 | No       |
| LANSGID         | string | 内网安全组 ID。输入“有效”状态的安全组的ID。                  | No       |
| GPU             | int    | GPU 卡核心的占用个数。枚举值：【1,2,4】。GPU与CPU、内存大小关系：CPU个数>=4*GPU个数，同时内存与CPU规格匹配. | No       |
| OperatorName    | string | 创建虚拟机同时绑定外网 IP 的网段，可由管理员自定义。         | No       |
| Bandwidth       | string | 创建虚拟机同时绑定外网 IP 的带宽。                           | No       |
| IPVersion       | string | 创建虚拟机同时绑定外网 IP 的 IP 版本。枚举值：IPv4 & IPv6，默认为 IPv4 | No       |
| InternetIP      | string | 手动指定虚拟机绑定外网 IP 的地址，IP地址必须包含在网段内。   | No       |
| BootDiskSpace   | int    | 系统盘大小，单位 GB。默认值为40。范围：【40，500】，步长10。 | No       |

**Response Elements**

| Parameter name | Type   | Description             | Required |
| -------------- | ------ | ----------------------- | -------- |
| RetCode        | int    | 返回码                  | **Yes**  |
| Action         | string | 操作名称                | **Yes**  |
| Message        | string | 返回信息描述。          | No       |
| VMID           | string | 返回创建的虚拟机 ID     | No       |
| DiskID         | string | 返回同时创建的数据盘 ID | No       |
| EIPID          | string | 返回同时创建的外网IP ID | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateVMInstance
&Region=cn
&Zone=zone-01
&Name=GmVzvGHM
&VMType=Normal
&ImageID=SNmaoHtN
&CPU=1
&Memory=2048
&BootDiskSetType=Normal
&DataDiskSetType=Normal
&VPCID=bNDADRnn
&SubnetID=GBdNuHBB
&WANSGID=mCRQkQRi
&ChargeType=Dynamic
&Password=oNKBYzPA
&DataDiskSpace=8
&Quantity=7
&InternalIP=ZJzHPCwg
&LANSGID=fRyUTuTT
&GPU=1
&OperatorName=ShoqoDdR
&Bandwidth=gXCJuHhf
&IPVersion=UxUXacRh
&InternetIP=DfOQkePt
&BootDiskSpace=4
```

**Response Example**

```
{
    "RetCode": 0, 
    "VMID": "gyHonDYi", 
    "EIPID": "hWMaozqD", 
    "Action": "CreateVMInstanceResponse", 
    "Message": "YYfWYkFc", 
    "DiskID": "NdlYxMgW"
}
```

## 2.2 查询虚拟机-DescribeVMInstance

查询虚拟机

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域或数据中心。枚举值： cn，表示中国；                      | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| VPCID          | string | VPC ID。输入“有效”状态的VPC ID。                             | No       |
| SubnetID       | string | 子网 ID。输入“有效”状态的子网 ID。                           | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |
| VMIDs.N        | string | 【数组】虚拟机的 ID。输入有效的 ID。调用方式举例：VMIDs.0=“one-id”、VMIDs.1=“two-id”。 | No       |

**Response Elements**

| Parameter name | Type   | Description                | Required |
| -------------- | ------ | -------------------------- | -------- |
| RetCode        | int    | 返回码                     | **Yes**  |
| Action         | string | 操作名称                   | **Yes**  |
| Message        | string | 返回信息描述               | No       |
| TotalCount     | int    | 返回虚拟机总个数           | No       |
| Infos          | array  | 【数组】返回虚拟机对象数组 | No       |

**VMInstanceInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | Region                                                       | No       |
| Zone           | string | Zone                                                         | No       |
| State          | string | 虚拟机状态。枚举值：Initializing，表示初始化；Starting，表示启动中；Restarting，表示重启中；Running，表示运行；Stopping，表示关机中；Stopped，表示关机；Deleted，表示已删除；Resizing，表示修改配置中；Terminating，表示销毁中；Terminated，表示已销毁；Migrating，表示迁移中；WaitReinstall，表示等待重装系统；Reinstalling，表示重装中；Poweroffing，表示断电中；ChangeSGing，表示修改防火墙中； | No       |
| VMID           | string | 虚拟机 ID                                                    | No       |
| VMType         | string | 虚拟机所在宿主机的集群类型 ID                                | No       |
| ImageID        | string | 镜像 ID                                                      | No       |
| OSName         | string | 操作系统名称                                                 | No       |
| OSType         | string | 操作系统类型                                                 | No       |
| Remark         | string | 备注                                                         | No       |
| Name           | string | 虚拟机名称                                                   | No       |
| CreateTime     | int    | 虚拟机创建时间                                               | No       |
| ChargeType     | string | 虚拟机计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | No       |
| ExpireTime     | int    | 虚拟机过期时间                                               | No       |
| CPU            | int    | CPU 个数                                                     | No       |
| Memory         | int    | 内存大小，单位 M                                             | No       |
| DiskInfos      | array  | 磁盘信息                                                     | No       |
| IPInfos        | array  | IP 信息                                                      | No       |
| VPCID          | string | VPC ID                                                       | No       |
| VPCName        | string | VPC 名称                                                     | No       |
| SubnetID       | string | 子网 ID                                                      | No       |
| SubnetName     | string | 子网 名称                                                    | No       |
| VMTypeAlias    | string | 虚拟机所在宿主机的集群类型名称                               | No       |

**VMDiskInfo**

| Parameter name | Type   | Description                                            | Required |
| -------------- | ------ | ------------------------------------------------------ | -------- |
| Type           | string | 磁盘类型。枚举值：Boot，表示系统盘；Data，表示数据盘； | No       |
| DiskID         | string | 磁盘 ID                                                | No       |
| Name           | string | 磁盘名称                                               | No       |
| Drive          | string | 磁盘盘符                                               | No       |
| Size           | int    | 磁盘大小，单位 GB                                      | No       |
| IsElastic      | string | 是否是弹性磁盘。枚举值为：Y，表示是；N，表示否；       | No       |

## VMIPInfo
| Parameter name | Type   | Description                                            | Required |
| -------------- | ------ | ------------------------------------------------------ | -------- |
| IP             | string | IP 值                                                  | No       |
| MAC            | string | MAC 地址值                                             | No       |
| Type           | string | IP 类型。枚举值：Private，表示内网；Public，表示外网。 | No       |
| IPVersion      | string | IP版本,支持值：IPv4\IPv6                               | No       |
| SubnetID       | string | 子网 ID，IP 为外网 IP 时为空；                         | No       |
| VPCID          | string | VPC ID，IP 为外网 IP 时为空；                          | No       |
| SGName         | string | 安全组名称                                             | No       |
| SGID           | string | 安全组 ID                                              | No       |
| InterfaceID    | string | 网卡 ID，IP 地址绑定的网卡 ID                          | No       |
| IsElastic      | string | 是否是弹性网卡。枚举值：Y，表示是；N，表示否；         | No       |
| VPCName        | string | VPC 名称，IP 为外网 IP 时为空；                        | No       |
| SubnetName     | string | 子网名称，IP 为外网 IP 时为空；                        | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeVMInstance
&Region= cn
&Zone=zone-01
&VPCID=UxmNzpZf
&SubnetID=CSMXnQFT
&Offset=3
&Limit=4
&VMIDs.N=iWGESINJ
```

**Response Example**

```
{
    "Action": "DescribeVMInstanceResponse", 
    "TotalCount": 6, 
    "Message": "UjKYrrsR", 
    "Infos": [
        {
            "VPCID": "WpcFysuc", 
            "Zone": "mADuhoZW", 
            "DiskInfos": [
                {
                    "Name": "yxgvPzZn", 
                    "Drive": "lCHHihNl", 
                    "IsElastic": "Y", 
                    "Type": "Boot", 
                    "DiskID": "pGBlTDQD", 
                    "Size": 1
                }
            ], 
            "ImageID": "spGLGVlM", 
            "OSType": "dtPOXKmD", 
            "State": "pWeZsyga", 
            "Memory": 7, 
            "SubnetName": "FKyPdPEt", 
            "CPU": 1, 
            "VPCName": "tyKeuify", 
            "Remark": "blkaAwSU", 
            "Name": "EICQugoQ", 
            "VMTypeAlias": "GoMkEsyk", 
            "Region": "OBYiDDka", 
            "VMID": "uXROnMUJ", 
            "OSName": "KEVYuwsI", 
            "IPInfos": [
                {
                    "InterfaceID": "gxfBxsBN", 
                    "VPCID": "FcHxBcZS", 
                    "IP": "pgZmTQJY", 
                    "SGName": "NsgJLUEv", 
                    "IPVersion": "MwCfqKFL", 
                    "SubnetName": "TvUOkVOd", 
                    "MAC": "TCqbvJYl", 
                    "VPCName": "erOvAhkM", 
                    "SubnetID": "aRCbkqYS", 
                    "SGID": "eZrBLGlV", 
                    "IsElastic": "sUGNclMl", 
                    "Type": "bkgmxdBu"
                }
            ], 
            "ExpireTime": 8, 
            "VMType": "MUOzrJzz", 
            "ChargeType": "nblALGys", 
            "SubnetID": "vieOpAop", 
            "CreateTime": 2
        }
    ], 
    "RetCode": 0
}
```

## 2.3 获取虚拟机价格-GetVMInstancePrice

获取 UCloudStack 虚拟机的价格。

**Request Parameters**

| Parameter name  | Type   | Description                                                  | Required |
| --------------- | ------ | ------------------------------------------------------------ | -------- |
| Region          | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone            | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| VMType          | string | 机型。枚举值：Normal，表示普通；SSD，表示SSD；               | **Yes**  |
| ImageID         | string | 镜像 ID。基础镜像 ID 或者自制镜像 ID。如：cn-image-centos-74。 | **Yes**  |
| CPU             | int    | CPU 个数，目前只能输入数据库配置指定规格参数，如：1核2048M、2核4096M、4核8192M、8核16384M、16核32768M。 | **Yes**  |
| Memory          | int    | 内存大小，单位 M。目前只能输入数据库配置指定规格参数，如：1核2048M、2核4096M、4核8192M、8核16384M、16核32768M。 | **Yes**  |
| BootDiskSetType | string | 系统盘类型。枚举值：Normal，表示普通；SSD，表示SSD；         | **Yes**  |
| DataDiskSetType | string | 数据盘类型。枚举值：Normal，表示普通；SSD，表示SSD；         | **Yes**  |
| ChargeType      | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| DataDiskSpace   | int    | 数据盘大小，单位 GB。默认值为0。范围：【0，8000】，步长10。  | **Yes**  |
| OSType          | string | 系统类型。                                                   | **Yes**  |
| Quantity        | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |
| GPU             | int    | GPU 卡核心的占用个数。枚举值：【1,2,4】。GPU与CPU、内存大小关系：CPU个数>=4*GPU个数，同时内存与CPU规格匹配. | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | No       |
| Infos          | array  | 返回的价格信息 | No       |

**PriceInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Price          | float  | 价格                                                         | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=GetVMInstancePrice
&Region=cn
&Zone=zone-01
&Name=dSuOjdgL
&VMType=Normal
&ImageID=DubXEmvU
&CPU=1
&Memory=2048
&BootDiskType=Normal
&DataDiskType=Normal
&VPCID=ACuuNfpV
&SubnetID=UJxHRuDH
&WANSGID=FhriFhMO
&ChargeType=Dynamic
&Password=iqJAtaUZ
&DataDiskSpace=4
&Quantity=8
&InternalIP=QprqmxfI
&LANSGID=euFLXPPN
&GPU=1
&StorageType=ZlrOmLud
&StorageType=LocalDisk
&OSType=Linux;Windows
&ImageID=SXaOOvtS
&StorageType=LocalDisk
&OSType=Linux;Windows
&ImageID=XafNVWPy
&Count=5
&Count=6
&Count=5
&BootDiskSpace=40
```

**Response Example**

```
{
    "Action": "GetVMInstancePriceResponse", 
    "Message": "MgaEephk", 
    "VMID": "uacNrQdo", 
    "RetCode": 0
}
```

## 2.4 开启虚拟机-StartVMInstance

开启 UCloudStack 虚拟机，必须在关机状态下才可进行开启操作。

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn，表示中国；        | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| VMID           | string | 虚拟机 ID                           | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=StartVMInstance
&Region=cn
&Zone=zone-01
&VMID=zgZiylxX
```

**Response Example**

```
{
    "Action": "StartVMInstanceResponse", 
    "Message": "zbuDDChr", 
    "VMID": "rtirXiHH", 
    "RetCode": 0
}
```

## 2.5 重启虚拟机-RestartVMInstance

重启虚拟机

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| VMID           | string | 虚拟机ID;                           | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=RestartVMInstance
&Region=fMfhZTDm
&Zone=PrjTDvqh
&VMID=rwmowliE
```

**Response Example**

```
{
    "Action": "RestartVMInstanceResponse", 
    "Message": "ezGPSQgz", 
    "RetCode": 0
}
```

## 2.6 关闭虚拟机-StopVMInstance

关闭 UCloudStack 虚拟机，必须在运行状态下才可进行关闭操作。

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn，表示中国；        | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| VMID           | string | 虚拟机 ID                           | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | No       |
| VMID           | string | 虚拟机 ID    | No       |

**Request Example**

```
http(s)://xxx.xxx.xx/?Action=StopVMInstance
&Region=cn
&Zone=zone-01
&VMID=nnKJoMJn
```

**Response Example**

```
{
    "Action": "StopVMInstanceResponse", 
    "Message": "ttMKZteE", 
    "VMID": "cJgwJQvE", 
    "RetCode": 0
}
```

## 2.7 断电虚拟机-PoweroffVMInstance

断电虚拟机，可能导致丢失数据甚至损坏操作系统，仅适用于虚拟机死机及级端测试场景。

**Request Parameters**

| Parameter name | Type   | Description                               | Required |
| -------------- | ------ | ----------------------------------------- | -------- |
| Region         | string | 地域。枚举值：如 cn,表示中国。            | **Yes**  |
| Zone           | string | 可用区。枚举值：如 zone-01，表示可用区1。 | **Yes**  |
| VMID           | string | 虚拟机ID                                  | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://api.ucloud.cn/?Action=PoweroffVMInstance
&Region=cn-zj
&Zone=cn-zj-01
&VMID=sakuHexl
```

**Response Example**

```
{
    "Action": "PoweroffVMInstanceResponse", 
    "Message": "ojfNlmPc", 
    "RetCode": 0
}
```

## 2.8 重置虚拟机密码-ResetVMInstancePassword

重置虚拟机密码，主机必须开机才可以重置密码

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；       | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| VMID           | string | 虚拟机ID                            | **Yes**  |
| Password       | string | 密码                                | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xx/?Action=ResetVMInstancePassword
&Region=HBUxObMl
&Zone=FiQRGvMR
&VMID=OLRzFcDI
&Password=AglpluBh
```

**Response Example**

```
{
    "Action": "ResetVMInstancePasswordResponse", 
    "Message": "AfIfTBdu", 
    "RetCode": 0
}
```

## 2.9 重装系统-ReinstallVMInstance

重装系统，关机状态的虚拟机才可进行重装系统操作。

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；       | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| VMID           | string | 虚拟机ID                            | **Yes**  |
| ImageID        | string | 镜像ID                              | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xx/?Action=ReinstallVMInstance
&Region=OZPrHkdb
&Zone=rwKrxydt
&VMID=CmQlIbyb
&ImageID=yBbieJrp
```

**Response Example**

```
{
    "Action": "ReinstallVMInstanceResponse", 
    "Message": "qvgchVHk", 
    "RetCode": 0
}
```

## 2.10 修改虚拟机配置-ResizeVMConfig

修改虚拟机配置，关机状态的虚拟机才可进行配置修改。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| VMID           | string | 虚拟机ID                                                     | **Yes**  |
| CPU            | int    | CPU 个数，如 1、2、4、8、16、32、64。                        | **Yes**  |
| Memory         | int    | 内存容量，如 2048、4096、8192、16384、32768、65536、131072。 | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xx/?Action=ResizeVMConfig
&Region=XkTdOdYV
&Zone=rNtIJzue
&VMID=NnnQVZhB
&CPU=1
&Memory=2
```

**Response Example**

```
{
    "Action": "ResizeVMConfigResponse", 
    "Message": "ryhjcQtD", 
    "RetCode": 0
}
```

## 2.11设置虚拟机网络出口-UpdateVMDefaultGW

设置虚拟机的默认网络出口

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| Region         | string | 地域        | **Yes**  |
| Zone           | string | 可用区      | **Yes**  |
| VMID           | string | 虚拟机ID    | **Yes**  |
| EIPID          | string | EIPID       | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| RetCode        | int    | 返回码      | **Yes**  |
| Action         | string | 操作名称    | **Yes**  |
| Message        | string | 错误信息    | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UpdateVMDefaultGW
&Region=cn-zj
&Zone=cn-zj-01
&VMID=RKIGciIf
&EIPID=DAmBJbnw
```

**Response Example**

```
{
    "Action": "UpdateVMDefaultGWResponse", 
    "Message": "mlrvTnqm", 
    "RetCode": 0
}
```

## 2.12 删除虚拟机-DeleteVMInstance

删除 UCloudStack 虚拟机实例，删除后虚拟机会进入回收站。

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。 枚举值：cn，表示中国；       | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| VMID           | string | 虚拟机 ID。输入有效的虚拟机 ID。    | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | No       |

**Request Example**

```
http(s)://xxx.xxx.xx/?Action=DeleteVMInstance
&Region=cn
&Zone=zone-01
&VMID=AeWCtkUu
```

**Response Example**

```
{
    "Action": "DeleteVMInstanceResponse", 
    "Message": "ioSzzFmT", 
    "RetCode": 0
}
```

