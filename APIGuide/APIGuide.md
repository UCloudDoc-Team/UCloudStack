[toc]

<font size="5" color="black">版权声明</font>

版权所有 © 优刻得科技股份有限公司 2020 保留一切权利。

本文档中出现的任何文字叙述、文档格式、图片、方法及过程等内容，除另有特别注明外，其著作权或其它相关权利均属于优刻得科技股份有限公司。非经优刻得科技股份有限公司书面许可，任何单位和个人不得以任何方式和形式对本文档内的任何部分擅自进行摘抄、复制、备份、修改、传播、翻译成其它语言、将其全部或部分用于商业用途。

UCloudStack 商标和 UCloud 商标为优刻得科技股份有限公司所有。对于本手册中可能出现的其它公司的商标及产品标识，由各自权利人拥有。

注意您购买的产品、服务或特性等应受优刻得科技股份有限公司商业合同和条款约束，本文档中描述的全部或部分产品、服务或特性可能不在您的购买或使用权利范围之内。除非合同另有约定，优刻得科技股份有限公司对本文档内容不做任何明示或暗示的声明或保证。

由于产品版本升级或其它原因，本文档内容会不定期更新，除非另有约定，本文档仅作为使用指导，本文档中的所有陈述、信息和建议不构成任何明示或暗示的担保。



# 1 API 概览

## 1.1 API 文档综览

本文档是 UCloudStack 云计算产品 API 参考手册。在本文档中，您能够获取到对于每一个指令的描述、语法及使用示例。您可以通过用 HTTP/HTTPS GET 的方式对产品 API 进行调用，或选择适合您所使用编程语言的 SDK 来访问产品 API 接口。在调用 API 时，除需要给出相应的 API 调用地址、公共参数、API指令及指令参数外，还需在调用请求中给出 API 密钥进行身份认证，请务必妥善保管好帐户的 API 密钥。

**本文档描述的 API 接口及内容适用的产品版本为：UCloudStack v1.13.0 ，调用时请配置 UCloudStack 私有云 URL 。**

## 1.2 API 规范概述

### 1.2.1 API 请求结构

| Name         | Description                                | Notes                           |
| :----------- | :----------------------------------------- | :------------------------------ |
| API 调用地址 | 调用 API 的 WebService 入口                | http(s)://xxx.xxx.xxx           |
| 公共参数     | 调用 API 时需要给出的公共参数              | 参见 [公共参数](#_122-公共参数) |
| API 指令     | 即 API 指令名称，如 **DescribeVMInstance** |                                 |
| 指令参数     | 执行每个指令时所需要提供的参数             |                                 |
 <span id="_122-公共参数"></span>

### 1.2.2 公共参数

公共参数是在操作所有 API 的时候，都必需给出的参数。公共参数列表如下：

| 参数名    | 必选 | 参数类型 | 说明                                                         |
| :-------- | :--- | :------- | :----------------------------------------------------------- |
| Action    | true | String   | 对应的 API 名称，如 CreateVMInstance                         |
| PublicKey | true | String   | 用户公钥                                                     |
| Signature | true | String   | 根据公钥及 API 指令生成的用户签名，参见 [签名算法](#_13-签名算法) |

> **为避免重复，公共参数不在各 API 文档中重复列出。**

### 1.2.3 API 请求示例

#### 1.2.3.1 JSON 方式

```
curl -X POST \
  https://xxx.xxx.xxx \
  -H 'Content-Type: application/json' \
  -d '{
    "Action":"DescribeVMInstance",
    "PublicKey":"SI3zdT8g1tMInvahPNkckt3Ul8Pq+krxm+yT8MSK7Sd81nWxFOKxdw==",
    "Limit":20,
    "Offset":0,
    "Signature":"ed5ccd667678292a08dae1828710b4ef4bb57085",
}'
```

#### 1.2.3.2 参数拼接方式

下面是一个 API 请求示例，所调用的是 DescribeVMInstance 指令。实际使用时**请使用您的帐户 PublicKey 与 Signature 参数值替换参数值。**

```
http(s)://console.dev.ucloudstack.com/api/?Action=DescribeVMInstance
&Offset=0
&Limit=20  
&PublicKey=ucloudsomeone@example.com1296235120854146120
&Signature=2697152c34abbc148a38a33c0dc0d3d7b99ce82f
```

> 为方便查看，文档中参数拼接方式的示例会将参数之间用回车分隔开，实际请求中不能进行换行。

### 1.2.4 API 返回结构

| Name      | Description                                                  | Notes                                                  |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| 指令名称  | 返回所调用的指令名称。 例如 DescribeVMInstanceResponse       | API 返回的指令名称为 "API 指令名称"+"Response"来表示。 |
| API返回码 | 用来表示 API 请求的返回值 ，当 RetCode = 0 时表示 API 请求正常， RetCode != 0 时表示 API 请求错误。 |                                                        |
| 返回参数  | 每个API的返回参数                                            |                                                        |

### 1.2.5 API 返回示例

该API的返回值为如下所示的JSON格式内容。

```
{
     "Action" : "DescribeVMInstanceResponse",
     "TotalCount" : 0,
     "RetCode" : 0,
     "Message" : "",
     "Infos" : []
 }
```

<span id="_13-签名算法"></span>

## 1.3 签名算法


### 1.3.1 准备条件
在生成API请求中的签名(signature)时，需使用账户中的 PublicKey 和 PrivateKey ，本例假设：
```
PublicKey = '1UxDcqTHEGGGviQFqlt870EbLuaSJPZOB8hZ74tL'
PrivateKey = 'tcgX3Xi_mAKpQayggnVLWzerkWB_fH1KXuk05hUrus8KSziLVyjWXwKZ80FOOldC'
```
用户可使用上述的 PublicKey 和 PrivateKey 调试代码， 当得到跟后面一致的签名结果后（即表示代码正确），可再换为自己帐户的 PublicKey 和 PrivateKey 以及其他 API 请求。本例中假设用户请求参数串如下:

```
	{
	    'Action': 'DescribeVMInstance',
	    'Offset': 0,
	    'Limit': 20,
	    'PublicKey': '1UxDcqTHEGGGviQFqlt870EbLuaSJPZOB8hZ74tL'
	}

```

### 1.3.2 构建 HTTP 请求

#### 1.3.2.1 请求参数升序排列

将请求参数按照参数名进行升序排列，示例如下：

```
{
	    'Action': 'DescribeVMInstance',
	    'Limit': 20,
	    'Offset': 0,
	    'PublicKey': '1UxDcqTHEGGGviQFqlt870EbLuaSJPZOB8hZ74tL'
	}
```
#### 1.3.2.2 构造被签名参数串
被签名串的构造规则为: 被签名串 = 所有请求参数拼接（无需 HTTP 转义），并在本签名串的结尾拼接 API 密钥的私钥（PrivateKey）。
```
ActionDescribeVMInstanceLimit20Offset0PublicKey1UxDcqTHEGGGviQFqlt870EbLuaSJPZOB8hZ74tLtcgX3Xi_mAKpQayggnVLWzerkWB_fH1KXuk05hUrus8KSziLVyjWXwKZ80FOOldC
```
#### 1.3.2.3 计算签名
生成被签名串的 SHA1 签名，即请求参数 ”Signature” 的值。按照上述算法，本例中，计算出的 Signature 为 2d86e5b4186ac6e42b628f258a7037c7636c9a81 。其中：

**（1）Python 生成签名代码**

```
import hashlib
import urlparse
import urllib
 
def _verfy_ac(private_key, params):
​    # 请求参数串
​    items=params.items()
​    # 将参数串排序
​    items.sort()
 
    # 拼接
    params_data = "";
    for key, value in items:
        params_data = params_data + str(key) + str(value)
    params_data = params_data + private_key
 
    # 生成的Signature值
    sign = hashlib.sha1()
    sign.update(params_data)
    signature = sign.hexdigest()
 
    return signature

```
**（2）PHP 生成签名代码**

```
function _verfy_ac($private_key, $params) {
   ksort($params);
   # 参数串排序
   $params_data = "";
 
   foreach($params as $key => $value) {
​       $params_data .= $key;
​       $params_data .= $value;
   }
 
   $params_data .= $private_key;
   return sha1($params_data); 
   # 生成的Signature值
}
```

#### 1.3.2.4 使用签名进行HTTP请求

进行 HTTP 请求时不需对参数进行排序，需要加入PublicKey 和 Signature 参数。支持 Json、URL 拼接参数及 body 拼接参数 3 种请求方式。

**（1）Json 方式**

```
curl -X POST http://console.dev.ucloudstack.com/api \
    -H 'Content-type: application/json' \
    -d '{
        "Action" : "DescribeVMInstance", 
        "Limit" : 20, 
        "Offset" : 0, 
        "PublicKey" : "1UxDcqTHEGGGviQFqlt870EbLuaSJPZOB8hZ74tL",
        "Signature" : "2d86e5b4186ac6e42b628f258a7037c7636c9a81" 
    }'

```

**（2）URL 拼接参数方式（只能使用 GET 方法）**

对排序后的请求参数进行 URL 编码，URL 编码的规则如下：

* 字符 A~Z、a~z、0~9 不编码；
* 字符“-”、“_”、“.”、“~” 不编码；
* 其它字符编码成 %XY 的格式，其中 XY 是字符对应 ASCII 码的 16 进制表示。如英文的双引号（”）对应的编码为 %22 ；
* 对于扩展的 UTF-8 字符，编码成 %XY%ZA… 的格式；
* 英文空格（ ）要编码成 %20，而不是加号（+）。

构造HTTP请求，参数名和参数值之间用 "=" 连接，参数和参数之间用 "&" 号连接，构造的 URL 请求为：

```
curl -X GET \
-H 'Content-type: application/x-www-form-urlencoded' \
'http://console.dev.ucloudstack.com/api？Action=DescribeVMInstance&PublicKey=1UxDcqTHEGGGviQFqlt870EbLuaSJPZOB8hZ74tL&Signature=2d86e5b4186ac6e42b628f258a7037c7636c9a81&Limit=20&Offset=0'
```

**（3）body 拼接参数形式**

```
curl -X POST http://console.dev.ucloudstack.com/api \
-H 'Content-type: application/x-www-form-urlencoded' \
-d 'Action=DescribeVMInstance&PublicKey=1UxDcqTHEGGGviQFqlt870EbLuaSJPZOB8hZ74tL&Signature=2d86e5b4186ac6e42b628f258a7037c7636c9a81&Limit=20&Offset=0'
```

# 2 虚拟机

## 2.1 创建虚拟机-CreateVMInstance

创建 UCloudStack 虚拟机。该 API 属于UCloudStack ，调用时请配置 UCloudStack 私有云 URL 。

**Request Parameters**

| Parameter name  | Type   | Description                                                  | Required |
| --------------- | ------ | ------------------------------------------------------------ | -------- |
| Region          | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone            | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| Name            | string | 虚拟机名称。可输入如：myVM。名称只能包含中英文、数字以及- _ .且1-30个字符。 | **Yes**  |
| VMType          | string | 机型。枚举值：Normal，表示普通；SSD，表示SSD；               | **Yes**  |
| ImageID         | string | 镜像 ID。基础镜像 ID 或者自制镜像 ID。如：cn-image-centos-74。 | **Yes**  |
| CPU             | int    | CPU 个数，目前只能输入数据库配置指定规格参数，如：1核2048M、2核4096M、4核8192M、8核16384M、16核32768M。 | **Yes**  |
| Memory          | int    | 内存大小，单位 M。目前只能输入数据库配置指定规格参数，如：1核2048M、2核4096M、4核8192M、8核16384M、16核32768M。 | **Yes**  |
| BootDiskSetType | string | 系统盘类型。枚举值：Normal，表示普通；SSD，表示SSD；         | **Yes**  |
| DataDiskSetType | string | 数据盘类型。枚举值：Normal，表示普通；SSD，表示SSD；         | **Yes**  |
| VPCID           | string | VPC ID。                                                     | **Yes**  |
| SubnetID        | string | 子网 ID。                                                    | **Yes**  |
| WANSGID         | string | 外网安全组 ID。输入“有效”状态的安全组的ID。                  | **Yes**  |
| ChargeType      | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Password        | string | 密码。可输入如：ucloud.cn。密码长度限6-30个字符；需要同时包含两项或以上（大写字母/小写字母/数字/特殊符号)；windows不能包含用户名（administrator）中超过2个连续字符的部分。 | **Yes**  |
| DataDiskSpace   | int    | 数据盘大小，单位 GB。默认值为0。范围：【0，8000】，步长10。  | No       |
| Quantity        | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |
| InternalIP      | string | 指定内网IP。输入有效的指定内网 IP。默认为系统自动分配内网 IP。 | No       |
| LANSGID         | string | 内网安全组 ID。输入“有效”状态的安全组的ID。                  | No       |
| GPU             | int    | GPU 卡核心的占用个数。枚举值：【1,2,4】。GPU与CPU、内存大小关系：CPU个数>=4*GPU个数，同时内存与CPU规格匹配. | No       |

**Response Elements**

| Parameter name | Type   | Description                | Required |
| -------------- | ------ | -------------------------- | -------- |
| RetCode        | int    | 返回码                     | **Yes**  |
| Action         | string | 操作名称                   | **Yes**  |
| Message        | string | 返回信息描述。             | No       |
| VMID           | string | 返回创建虚拟机的 ID 数组。 | No       |

**Request Example**

```
http(s)://xxx.xxx.xx/?Action=CreateVMInstance
&Region=cn
&Zone=zone-01
&Name=ZQbjAFhy
&VMType=Normal
&ImageID=cn-image-centos-74
&Memory=2048
&CPU=1
&BootDiskType=Normal
&DataDiskType=Normal
&VPCID=oFbnHuhY
&SubnetID=wQpNmRCA
&WANSGID=YrNzIemH
&ChargeType=Month
&Password=loZNFaoN.cn
```

**Response Example**

```
{
    "Action": "CreateVMInstanceResponse", 
    "Message": "lnlouxcc", 
    "RetCode": 0, 
    "VMIDs": [
        "XjKoVIGV"
    ]
}
```

## 2.2 查询虚拟机-DescribeVMInstance

查询 UCloudStack 虚拟机列表及详细信息。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| VPCID          | string | VPC ID。输入“有效”状态的VPC ID。                             | No       |
| SubnetID       | string | 子网 ID。输入“有效”状态的子网 ID。                           | No       |
| Offset         | string | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |
| VMIDs.N        | string | 【数组】虚拟机的 ID。输入有效的 ID。调用方式举例：PrivateIp.0=“one-id”、PrivateIp.1=“two-id”。 | No       |

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
| State          | string | 虚拟机状态。枚举值：Initializing，表示初始化；Starting，表示启动中；Restarting，表示重启中；Running，表示运行；Stopping，表示关机中；Stopped，表示关机；Deleted，表示已删除；Resizing，表示修改配置中；Terminating，表示销毁中；Terminated，表示已销毁；Migrating，表示迁移中；WaitReinstall，表示重装中；Reinstalling，表示重装中；Poweroffing，表示断电中；ChangeSGing，表示修改防火墙中； | No       |
| RegionAlias    | string | Region 别名                                                  | No       |
| ZoneAlias      | string | Zone 别名                                                    | No       |
| VMID           | string | 虚拟机 ID                                                    | No       |
| VMType         | string | 虚拟机类型                                                   | No       |
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
| VMTypeAlias    | string | 虚拟机类型别名                                               | No       |

**VMDiskInfo**

| Parameter name | Type   | Description                                            | Required |
| -------------- | ------ | ------------------------------------------------------ | -------- |
| Type           | string | 磁盘类型。枚举值：Boot，表示系统盘；Data，表示数据盘； | No       |
| DiskID         | string | 磁盘 ID                                                | No       |
| Name           | string | 磁盘名称                                               | No       |
| Drive          | string | 磁盘盘符                                               | No       |
| Size           | int    | 磁盘大小，单位 GB                                      | No       |
| IsElastic      | string | 是否是弹性磁盘。枚举值为：Y，表示是；N，表示否；       | No       |

**VMIPInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| IP             | string | IP 值                                                        | No       |
| MAC            | string | MAC 地址值                                                   | No       |
| Type           | string | IP 类型。枚举值：Private，表示内网；Public，表示外网；Physical，表示物理网； | No       |
| SubnetID       | string | 子网 ID                                                      | No       |
| VPCID          | string | VPC ID                                                       | No       |
| SGName         | string | 安全组名称                                                   | No       |
| SGID           | string | 安全组 ID                                                    | No       |
| InterfaceID    | string | 网卡 ID                                                      | No       |
| IsElastic      | string | 是否是弹性网卡。枚举值：Y，表示是；N，表示否；               | No       |
| VPCName        | string | VPC 名称                                                     | No       |
| SubnetName     | string | 子网名称                                                     | No       |

**Request Example**

```
http(s)://xxx.xxx.xx/?Action=DescribeVMInstance
&Region=cn
&Zone=zone-01
&VPCID=yipvTOkV
&SubnetID=BEPiKaef
&Offset=0
&Limit=20
```

**Response Example**

```
{
    "Action": "DescribeVMInstanceResponse", 
    "TotalCount": 6, 
    "Message": "OalUWiFJ", 
    "Infos": [
        "FiPXvwyG"
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

## 2.7 重置虚拟机密码-ResetVMInstancePassword

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

## 2.8 重装系统-ReinstallVMInstance

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

## 2.9 修改虚拟机配置-ResizeVMConfig

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

## 2.10 删除虚拟机-DeleteVMInstance

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

# 3 镜像管理

## 3.1 获取默认镜像和自制镜像信息-DescribeImage

获取镜像信息，包括默认镜像和自制镜像。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| ImageType      | string | 镜像类型。枚举值：Base(基础镜像，平台默认提供的镜像)，Custom(自制镜像，通过虚拟机导出的镜像) 。若该值为空，默认查询所有镜像。 | No       |
| ImageIDs.N     | string | 【数组】镜像的 ID。输入有效的 ID。调用方式举例：ImageIDs.0=“one-id”、ImageIDs.1=“two-id”。 | No       |
| Offset         | int    | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int    | 返回数据长度，默认为20，最大100。                            | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Message        | string | 返回信息描述。           | **Yes**  |
| TotalCount     | int    | 返回镜像的总个数。       | **Yes**  |
| Infos          | array  | 【数组】返回镜像对象数组 | **Yes**  |

**ImageInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域                                                         | **Yes**  |
| Zone           | string | 可用区                                                       | **Yes**  |
| ImageID        | string | 镜像ID                                                       | **Yes**  |
| Name           | string | 镜像名称                                                     | **Yes**  |
| ImageType      | string | 镜像类型。枚举类型：Base(基础镜像),Custom（自制镜像）。      | **Yes**  |
| OSType         | string | 系统类型。例如：Linux, Windows，Kylin                        | **Yes**  |
| OSName         | string | 系统名称。例如：CentOS 7.4 x86_64                            | **Yes**  |
| SetArch        | string | 架构名称。例如：x86_64                                       | **Yes**  |
| OSDistribution | string | 镜像系统发行版本。例如：Centos, Ubuntu, Windows等            | **Yes**  |
| ImageStatus    | string | 镜像状态。枚举类型：Making（创建中）,Available（可用）,Unavailable（不可用）,Terminating（销毁中）,Used（被使用中）,Deleting（删除中）,Deleted（已删除） | **Yes**  |
| CreateTime     | int    | 创建时间。时间戳。                                           | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeImage
&Region=vuSiavng
&Zone=VhnLlHOr
&ImageType=Base
&ImageIDs.N=TlCGpJBy
&Offset=8
&Limit=6
```

**Response Example**

```
{
    "Action": "DescribeImageResponse", 
    "TotalCount": 5, 
    "Message": "hMbvqPYL", 
    "Infos": [
        {
            "Name": "cJviOotc", 
            "Zone": "jcttdyfk", 
            "Region": "zvEpEuQR", 
            "OSName": "aVlwgbAb", 
            "OSDistribution": "goDUJINr", 
            "ImageID": "mlUxxckA", 
            "ImageStatus": "bLygBOJB", 
            "OSType": "lbJJTcKy", 
            "CreateTime": 9, 
            "ImageType": "VVMfPsLE", 
            "SetArch": "YdbSlQqM"
        }
    ], 
    "RetCode": 0
}
```

## 3.2 创建自制镜像-CreateCustomImage

创建自制镜像，指将虚拟机导出为自制镜像，可通过自制镜像创建虚拟机。

**Request Parameters**

| Parameter name   | Type   | Description                         | Required |
| ---------------- | ------ | ----------------------------------- | -------- |
| Region           | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone             | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| VMID             | string | 虚拟机ID                            | **Yes**  |
| ImageName        | string | 镜像名称                            | **Yes**  |
| ImageDescription | string | 镜像描述。                          | No       |

**Response Elements**

| Parameter name | Type   | Description      | Required |
| -------------- | ------ | ---------------- | -------- |
| RetCode        | int    | 返回码           | **Yes**  |
| Action         | string | 操作名称         | **Yes**  |
| Message        | string | 返回信息描述。   | **Yes**  |
| ImageID        | string | 创建的自制镜像ID | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateCustomImage
&Region=cn
&Zone=zone-01
&VMID=oIYchrnB
&ImageName=hkaXAARO
&ImageDescription=nEMngDsc
```

**Response Example**

```
{
    "Action": "CreateCustomImageResponse", 
    "Message": "QKVkwZXw", 
    "RetCode": 0, 
    "ImageID": "EhQEQYSz"
}
```

## 3.3 删除自制镜像-DeleteCustomImage

删除自制镜像

**Request Parameters**

| Parameter name | Type   | Description                         | Required |
| -------------- | ------ | ----------------------------------- | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；         | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国； | **Yes**  |
| ImageID        | string | 自制镜像ID                          | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DeleteCustomImage
&Region=iSruoUYb
&Zone=ROLUvjAp
&ImageID=FYWvrpiJ
```

**Response Example**

```
{
    "Action": "DeleteCustomImageResponse", 
    "Message": "zUFjzDRs", 
    "RetCode": 0
}
```

# 4 云硬盘

## 4.1 创建云硬盘-CreateDisk

创建 UCloudStack 云硬盘。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
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
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
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
| Zone           | string | 可用区                                                       | **Yes**  |
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
| Zone           | string | 可用区                                 | **Yes**  |
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
| Zone           | string | 可用区       | **Yes**  |
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
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                 | **Yes**  |
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
| Zone           | string | 可用区                                                       | **Yes**  |
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
| Zone           | string | 可用区         | **Yes**  |
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

# 5 VPC 私有网络

## 5.1 创建 VPC-CreateVPC

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

## 5.2 查询 VPC 信息-DescribeVPC

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

## 5.3 删除 VPC-DeleteVPC

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

## 5.4 创建子网-CreateSubnet

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

## 5.5 查询子网信息-DescribeSubnet

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

## 5.6 删除子网-DeleteSubnet

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

# 6 外网 IP

## 6.1 申请外网IP-AllocateEIP

申请 UCloudStack 外网 IP 地址。

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
&Region=cn-zj
&OperatorName=pNQdTosM
&Bandwidth=5
&ChargeType=Month
&Quantity=1
&Name=JpJqGLmS
&Zone=zone-01
```

**Response Example**

```
{
    "Action": "AllocateEIPResponse", 
    "Message": "zhaUEBjY", 
    "EIPID": "WKcJoCEy", 
    "RetCode": 0
}
```

## 6.2 获取外网 IP 价格-GetEIPPrice

获取 UCloudStack 外网 IP 的价格信息。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值：cn,表示中国；                                  | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| OpertatorName  | string | 线路。目前支持Bgp                                            | **Yes**  |
| Bandwidth      | int    | 带宽，默认值1，默认范围1\~100                                | **Yes**  |
| Quantity       | int    | 购买时长。默认值1。小时不生效，月范围【1，11】，年范围【1，5】。 | No       |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述   | **Yes**  |
| Infos          | array  | 返回的价格信息 | No       |

**PriceInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| ChargeType     | string | 计费模式。枚举值：Dynamic，表示小时；Month，表示月；Year，表示年； | **Yes**  |
| Price          | float  | 价格                                                         | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=GetEIPPrice
&Region=cn-zj
&Zone=cn-zj-01
&ChargeType=FJBytqOA
&Quantity=BrjpicZO
&OpertatorName=ilsUQOiZ
&Bandwidth=DNtLIsHG
```

**Response Example**

```
{
    "Action": "GetEIPPriceResponse", 
    "Message": "NNNLqxlc", 
    "Infos": [
        {
            "Price": 1.47368, 
            "ChargeType": "Dynamic"
        }
    ], 
    "RetCode": 0
}
```

## 6.3 获取外网 IP 信息-DescribeEIP

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

## 6.4 绑定外网 IP-BindEIP

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

## 6.5 解绑外网 IP-UnBindEIP

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

## 6.6 调整外网IP带宽-ModifyEIPBandwidth

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

## 6.7 删除外网 IP-ReleaseEIP

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

# 7 安全组

## 7.1 创建安全组-CreateSecurityGroup

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

## 7.2 查询安全组信息-DescribeSecurityGroup

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

## 7.3 删除安全组-DeleteSecurityGroup

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

## 7.4 创建安全组规则-CreateSecurityGroupRule

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

## 7.5 修改安全组规则-UpdateSecurityGroupRule

修改安全组规则

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| SGID           | string | 安全组ID                                                     | **Yes**  |
| Rules.N        | string | 【数组】规则。输入有效的 规则。调用方式举例：Rules.0=“TCP\|23\|0.0.0.0/0\|ACCEPT\|HIGH\|1”、Rules.1=“TCP\|55\|0.0.0.0/0\|ACCEPT\|HIGH\|1” | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述； | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UpdateSecurityGroupRule
&Region=OwjoQwnC
&Zone=mkAyPnMd
&SGID=dbFAQeJn
&Rules.N=oVMAaCBD
```

**Response Example**

```
{
    "Action": "UpdateSecurityGroupRuleResponse", 
    "Message": "EHyhtDNK", 
    "RetCode": 0
}
```

## 7.6 删除安全组规则-DeleteSecurityGroupRule

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

## 7.7 绑定安全组-BindSecurityGroup

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

## 7.8 解绑安全组-UnBindSecurityGroup

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
&Region=MxyROpjF
&Zone=rZirrUQm
&ResourceID=ihNMHWFy
&SGID=jjmxtalT
```

**Response Example**

```
{
    "Action": "UnBindSecurityGroupResponse", 
    "Message": "XSaMkhBb", 
    "RetCode": 0
}
```

# 8 负载均衡

## 8.1 创建负载均衡-CreateLB

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

## 8.2 获取负载均衡信息-DescribeLB

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

## 8.3 删除负载均衡-DeleteLB

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

## 8.4 创建负载均衡VServer-CreateVS

创建负载均衡VServer

**Request Parameters**

| Parameter name      | Type   | Description    | Required |
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
| SSLMode             | string | HTTPS SSL 认证解析模式。玫举值：UNIDIRECTIONAL:单向认证，MUTUAL:双向认证 。仅当VServer监听协议为 HTTPS 时有效且为必填项。 | No       |

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
&LBID=hyQumQAd
&Protocol=TCP|UDP|HTTP
&Port=7
&Scheduler=wrr
&HealthcheckType=Port
&PersistenceType=None
&PersistenceKey=kivxLjPr
&KeepaliveTimeout=2
&Path=rdkwfOeM
&Domain=xaHwauhz
&ServerCertificateID=IgrUQmOS
&CACertificateID=fFfuAOVZ
&SSLMode=ABdyJSjv
```

**Response Example**

```
{
    "Action": "CreateVSResponse", 
    "Message": "XvmACpAC", 
    "RetCode": 0, 
    "VSID": "xzHeuYer"
}
```

## 8.5 获取负载均衡 VServer 信息-DescribeVS

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
| PersistenceType     | string | 会话保持类型。枚举值：None:关闭；Auto:自动生成；Manual:手动生成 。当协议为 TCP 时，该值为空；当协议为 UDP 时 Auto 表示开启会话保持 。 | **Yes**  |
| HealthcheckType     | string | 负载均衡的健康检查类型。枚举值：Port:端口检查；Path: HTTP检查 。 | **Yes**  |
| KeepaliveTimeout    | int    | 负载均衡的连接空闲超时时间，单位为秒，默认值为 60s 。当 VServer 协议为 UDP 时，该值为空。 | **Yes**  |
| VSStatus            | string | VServer 的资源状态。枚举值，Available:可用,Updating:更新中,Deleted:已删除 。 | **Yes**  |
| RSHealthStatus      | string | 健康检查状态，枚举值，Empty:全部异常,Parts:部分异常,All:正常 | **Yes**  |
| CreateTime          | int    | 创建时间，时间戳                                             | **Yes**  |
| UpdateTime          | int    | 更新时间，时间戳                                             | **Yes**  |
| AlarmTemplateID     | string | 告警模板ID                                                   | **Yes**  |
| SSLMode             | string | HTTPS SSL 认证解析模式。玫举值：UNIDIRECTIONAL:单向认证，MUTUAL:双向认证 。仅当VServer监听协议为 HTTPS 时有效。 | No       |
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
&Region=wBzRHcEI
&Zone=XsLBZlTk
&LBID=GdWfrdEN
&VSIDs.N=VRRExEVF
&Offset=6
&Limit=4
```

**Response Example**

```
{
    "Action": "DescribeVSResponse", 
    "TotalCount": 3, 
    "Message": "kaKcOUzP", 
    "Infos": [
        {
            "PersistenceType": "None", 
            "Domain": "IeMiwXqu", 
            "Protocol": "WLfcdNbU", 
            "VSPolicyInfos": [
                {
                    "Domain": "xfezxbLF", 
                    "UpdateTime": 9, 
                    "VSID": "hEnRenuZ", 
                    "PolicyStatus": "Available", 
                    "LBID": "WknPlpKe", 
                    "PolicyID": "ljYLxzUn", 
                    "Path": "OSzQhqBk", 
                    "CreateTime": 6, 
                    "RSInfos": [
                        {
                            "UpdateTime": 9, 
                            "Name": "xQsJCDEU", 
                            "Weight": 2, 
                            "IP": "ZjbFAjGL", 
                            "RSStatus": "Creating", 
                            "VSID": "qmVMNesY", 
                            "CreateTime": 3, 
                            "LBID": "MhABvYvD", 
                            "RSID": "NQaUbquq", 
                            "BindResourceID": "MDwmDbYb", 
                            "Port": 5, 
                            "RSMode": "Enabling"
                        }
                    ]
                }
            ], 
            "RSHealthStatus": "Empty", 
            "UpdateTime": 5, 
            "VSID": "yexEIOfp", 
            "HealthcheckType": "Port", 
            "LBID": "ubxFmgvI", 
            "CreateTime": 1, 
            "AlarmTemplateID": "mmIDJoLA", 
            "CACertificateID": "bEGvXoXJ", 
            "KeepaliveTimeout": 2, 
            "SSLMode": "ijfBiEZN", 
            "Path": "THwqsoAh", 
            "VSStatus": "Available", 
            "PersistenceKey": "ToMLUFho", 
            "Port": 2, 
            "ServerCertificateID": "sfJFoYqM", 
            "RSInfos": [
                {
                    "UpdateTime": 7, 
                    "Name": "XlRifUhY", 
                    "Weight": 7, 
                    "IP": "RQIaXQeO", 
                    "RSStatus": "Creating", 
                    "VSID": "XinTeALM", 
                    "CreateTime": 9, 
                    "LBID": "xsOEsuyq", 
                    "RSID": "GxHXVrvt", 
                    "BindResourceID": "MDIbmFgl", 
                    "Port": 1, 
                    "RSMode": "Enabling"
                }
            ]
        }
    ], 
    "RetCode": 0
}
```

## 8.6 删除VServer-DeleteVS

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

## 8.7 添加服务节点-CreateRS

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

## 8.8 获取服务节点信息-DescribeRS

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

## 8.9 移除服务节点-DeleteRS

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

# 9 NAT 网关

## 9.1 创建NAT网关-CreateNATGW

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

## 9.2 获取NAT网关信息 -DescribeNATGW

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

## 9.3 删除NAT网关-DeleteNATGW

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

## 9.4 添加NAT网关白名单-CreateNATGWRule

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

## 9.5 获取NAT网关白名单信息 -DescribeNATGWRule

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

## 9.6 删除NAT网关白名单-DeleteNATGWRule

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

# 10 账户管理

## 10.1 管理员添加账号-CreateUser

管理员添加账号，即创建租户（主账号）。

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| UserEmail      | string | 账号邮箱。  | **Yes**  |
| PassWord       | string | 账号密码。  | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |
| UserID         | int    | 账户ID         | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=CreateUser
&UserEmail=ebvUGGyb
&PassWord=VHPdVijw
```

**Response Example**

```
{
    "Action": "CreateUserResponse", 
    "Message": "QYsSiqSC", 
    "UserID": "qXGiAIgk", 
    "RetCode": 0
}
```

## 10.2 查询租户信息-DescribeUser

查询 UCloudStack 租户信息。

**Request Parameters**

| Parameter name | Type | Description                                                  | Required |
| -------------- | ---- | ------------------------------------------------------------ | -------- |
| Offset         | int  | 列表起始位置偏移量，默认为0。                                | No       |
| Limit          | int  | 返回数据长度，默认为20，最大100。                            | No       |
| UserIDs.N      | int  | 【数组】租户的 ID。输入有效的 ID。调用方式举例：UserIDs.0=123”、UserIDs.1=456 | No       |

**Response Elements**

| Parameter name | Type   | Description              | Required |
| -------------- | ------ | ------------------------ | -------- |
| RetCode        | int    | 返回码                   | **Yes**  |
| Action         | string | 操作名称                 | **Yes**  |
| Message        | string | 返回信息描述             | **Yes**  |
| TotalCount     | int    | 返回现有租户总数         | **Yes**  |
| Infos          | array  | 【数组】返回租户对象数组 | **Yes**  |

**UserInfo**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| UserID         | int    | 租户ID.                                                      | No       |
| Email          | string | 租户名称                                                     | No       |
| PublicKey      | string | 公钥                                                         | No       |
| PrivateKey     | string | 私钥                                                         | No       |
| CreateTime     | int    | 账户创建时间。时间戳                                         | No       |
| UpdateTime     | int    | 更新时间。时间戳                                             | No       |
| Amount         | float  | 账户余额                                                     | No       |
| Status         | string | 用户状态。USER_STATUS_AVAILABLE：正常，USER_STATUS_FREEZE：冻结 | No       |

**Request Example**

```
https://xxx.xxx.xxx/?Action=DescribeUser
&Offset=mQpwrmYK
&Limit=HLrzzDfb
&UserIDs=QvIQBBgg
```

**Response Example**

```
{
    "Action": "DescribeUserResponse", 
    "TotalCount": 8, 
    "Message": "pnbYmdNw", 
    "Infos": [
        "uEzXfQUU"
    ], 
    "RetCode": 0
}
```

## 10.3 管理员给租户充值-Recharge

UCloudStack 管理员给租户充值金额。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| UserID         | int    | 租户的账户ID。                                               | **Yes**  |
| Amount         | int    | 充值金额。最少100,最大500000                                 | **Yes**  |
| FromType       | string | 充值来源。INPOUR_FROM_ALIPAY：支付宝，INPOUR_FROM_OFFLINE：银行转账，INPOUR_FROM_SINPAY：新浪支付，INPOUR_FROM_WECHAT_PAY：微信转账。 | **Yes**  |
| SerialNo       | string | 充值单号。充值方式为“账户余额”时为必要参数。                 | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description    | Required |
| -------------- | ------ | -------------- | -------- |
| RetCode        | int    | 返回码         | **Yes**  |
| Action         | string | 操作名称       | **Yes**  |
| Message        | string | 返回信息描述。 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=Recharge
&UserID=9
&RechargeType=1
&Amount=4
&FromType=INPOUR_FROM_ALIPAY;INPOUR_FROM_OFFLINE;INPOUR_FROM_SINPAY;INPOUR_FROM_WECHAT_PAY
&SerialNo=xSvXhxgK
```

**Response Example**

```
{
    "Action": "RechargeResponse", 
    "Message": "LvzORptu", 
    "RetCode": 0
}
```

# 11 资源通用操作

## 11.1 获取资源监控信息-DescribeMetric

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

## 11.2 修改资源名称和备注-ModifyNameAndRemark

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

## 11.3 绑定告警模板-BindAlarmTemplate

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

## 11.4 解绑告警模板-UnbindAlarmTemplate

解绑告警模板

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                          | **Yes**  |
| ResourceIDs.N  | string | 【数组】资源的 ID。调用方式举例：ResourceIDs.0=“one-id”、ResourceIDs.1=“two-id”。 | **Yes**  |
| ResourceType   | string | 资源类型。VM：虚拟机, LB:负载均衡, NATGW：nat网关;EIP:弹性网卡 | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description  | Required |
| -------------- | ------ | ------------ | -------- |
| RetCode        | int    | 返回码       | **Yes**  |
| Action         | string | 操作名称     | **Yes**  |
| Message        | string | 返回信息描述 | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=UnbindAlarmTemplate
&Region=ayJRNZAm
&Zone=jhGuDHnf
&ResourceIDs.N=XCFKNlCY
&ResourceType=MHVVnNgD
```

**Response Example**

```
{
    "Action": "UnbindAlarmTemplateResponse", 
    "Message": "NoHtElYp", 
    "RetCode": 0
}
```

## 11.5 查询主机机型-DescribeVMType

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

## 11.6 查询存储类型-DescribeStorageType

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

# 12 Python Demo


```python
import hashlib
import urlparse
import urllib
import httplib
 
def _verfy_ac(private_key, params):
    items=params.items()
    items.sort()
 
    params_data = "";
    for key, value in items:
        params_data = params_data + str(key) + str(value)
    params_data = params_data + private_key
 
    sign = hashlib.sha1()
    sign.update(params_data)
    signature = sign.hexdigest()
 
    return signature
	
def test(params):
	httpClient = None
	try:
		params = urllib.urlencode(params)
		headers = {'Content-type': 'application/x-www-form-urlencoded', 
		'Accept': 'text/plain',
		'Host': 'console.pre.ucloudstack.com',
		'Proxy-Connection': 'keep-alive'
		}
		httpClient = httplib.HTTPConnection('console.pre.ucloudstack.com', 80, timeout=10)
		httpClient.request('POST', '/api', params, headers)
		response = httpClient.getresponse()
		print response.status
		print response.reason
		print response.read()
		print response.getheaders()
	except Exception, e:
		print e
	finally:
		if httpClient:
			httpClient.close()

		
if __name__ == '__main__':
	private_key = 'ZwaVOkIzKJaMJMVL5a2_X83R74Hy'
	params = {
	'Action': 'DescribeVMInstance',
	'Offset': 0,
	'Limit': 20,
	'PublicKey': 'STFuzSBjVCtupWReWrRo4DX4pfH'
	}
	signature = _verfy_ac(private_key,params)
	print(signature)
	params['Signature'] = signature
	test(params)
	

```