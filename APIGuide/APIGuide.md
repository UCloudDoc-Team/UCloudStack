[toc]

<font size="5" color="black">版权声明</font>

版权所有 © 优刻得科技股份有限公司 2019 保留一切权利。

本文档中出现的任何文字叙述、文档格式、图片、方法及过程等内容，除另有特别注明外，其著作权或其它相关权利均属于优刻得科技股份有限公司。非经优刻得科技股份有限公司书面许可，任何单位和个人不得以任何方式和形式对本文档内的任何部分擅自进行摘抄、复制、备份、修改、传播、翻译成其它语言、将其全部或部分用于商业用途。

UCloudStack 商标和 UCloud 商标为优刻得科技股份有限公司所有。对于本手册中可能出现的其它公司的商标及产品标识，由各自权利人拥有。

注意您购买的产品、服务或特性等应受优刻得科技股份有限公司商业合同和条款约束，本文档中描述的全部或部分产品、服务或特性可能不在您的购买或使用权利范围之内。除非合同另有约定，优刻得科技股份有限公司对本文档内容不做任何明示或暗示的声明或保证。

由于产品版本升级或其它原因，本文档内容会不定期更新，除非另有约定，本文档仅作为使用指导，本文档中的所有陈述、信息和建议不构成任何明示或暗示的担保。



# 1 API 概览

## 1.1 API 文档综览

本文档是 UCloudStack 云计算产品 API 参考手册。在本文档中，您能够获取到对于每一个指令的描述、语法及使用示例。您可以通过用 HTTP/HTTPS GET 的方式对产品 API 进行调用，或选择适合您所使用编程语言的 SDK 来访问产品 API 接口。在调用 API 时，除需要给出相应的 API 调用地址、公共参数、API指令及指令参数外，还需在调用请求中给出 API 密钥进行身份认证，请务必妥善保管好帐户的 API 密钥。

**本文档描述的 API 接口及内容适用的产品版本为：UCloudStack v1.11.0 ，调用时请配置 UCloudStack 私有云 URL 。**

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

# 3 云硬盘

## 3.1 创建云硬盘-CreateDisk

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

## 3.2 获取云硬盘信息-DescribeDisk

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
| DiskStatus       | string | 硬盘状态                                                     | No       |
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

## 3.3 获取云硬盘价格-GetDiskPrice

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

## 3.4 绑定云硬盘-AttachDisk

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

## 3.5 解绑云硬盘-DetachDisk

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

## 3.6 克隆云硬盘-CloneDisk

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

## 3.7 删除云硬盘-DeleteDisk

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

# 4 VPC 私有网络

## 4.1 创建 VPC-CreateVPC

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

## 4.2 查询 VPC 信息-DescribeVPC

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

## 4.3 删除 VPC-DeleteVPC

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

## 4.4 创建子网-CreateSubnet

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

## 4.5 查询子网信息-DescribeSubnet

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

## 4.6 删除子网-DeleteSubnet

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

# 5 外网 IP

## 5.1 申请外网IP-AllocateEIP

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

## 5.2 获取外网 IP 价格-GetEIPPrice

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

## 5.3 获取外网 IP 信息-DescribeEIP

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

## 5.4 绑定外网 IP-BindEIP

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

## 5.5 解绑外网 IP-UnBindEIP

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

## 5.6 调整外网IP带宽-ModifyEIPBandwidth

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

## 5.7 删除外网 IP-ReleaseEIP

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

# 6 安全组

## 6.1 创建安全组-CreateSecurityGroup

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

## 6.2 查询安全组信息-DescribeSecurityGroup

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

## 6.3 删除安全组-DeleteSecurityGroup

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

## 6.4 创建安全组规则-CreateSecurityGroupRule

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

## 6.5 修改安全组规则-UpdateSecurityGroupRule

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

## 6.6 删除安全组规则-DeleteSecurityGroupRule

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

# 7 账户管理

## 7.1 管理员添加账号-CreateUser

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

## 7.2 查询租户信息-DescribeUser

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

## 7.3 登录账户-LoginByPassword

使用账户登录 UCloudStack 平台。

**Request Parameters**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| UserEmail      | string | 邮箱        | **Yes**  |
| Password       | string | 密码        | **Yes**  |

**Response Elements**

| Parameter name | Type   | Description | Required |
| -------------- | ------ | ----------- | -------- |
| RetCode        | int    | 返回码      | **Yes**  |
| Action         | string | 操作名称    | **Yes**  |
| Message        | string |             | **Yes**  |

**Request Example**

```
https://xxx.xxx.xxx/?Action=LoginByPassword
&UserEmail=okPXPTOF
&Password=ieSzYvjn
```

**Response Example**

```
{
    "Action": "LoginByPasswordResponse", 
    "Message": "JSSObZEg", 
    "RetCode": 0
}
```

## 7.4 管理员给租户充值-Recharge

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

# 8 资源通用操作

## 8.1 获取资源监控信息-DescribeMetric

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
&ResourceID=BnrrABPF
&ResourceType=nfQcwDMT
&BeginTime=amrnqOnW
&EndTime=LARaECej
&MetricName.N=BlockProcessCount
```

**Response Example**

```
{
    "Action": "DescribeMetricResponse", 
    "TotalCount": 9, 
    "Message": "ChUmOgCh", 
    "Infos": [
        {
            "Infos": [
                {
                    "Timestamp": 4, 
                    "Value": 4.53741
                }
            ], 
            "MetricName": "jqawIFrJ"
        }
    ], 
    "RetCode": 0
}
```

## 8.2 修改资源名称和备注-ModifyNameAndRemark

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

## 8.3 绑定告警模板-BindAlarmTemplate

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

## 8.4 解绑告警模板-UnbindAlarmTemplate

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

# 9 Python Demo


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