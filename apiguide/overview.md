# 1 API 概览

## 1.1 API 文档综览

本文档是 UCloudStack 云计算产品 API 参考手册。在本文档中，您能够获取到对于每一个指令的描述、语法及使用示例。您可以通过用 HTTP/HTTPS GET 的方式对产品 API 进行调用，或选择适合您所使用编程语言的 SDK 来访问产品 API 接口。在调用 API 时，除需要给出相应的 API 调用地址、公共参数、API指令及指令参数外，还需在调用请求中给出 API 密钥进行身份认证，请务必妥善保管好帐户的 API 密钥。

**本文档描述的 API 接口及内容适用的产品为 UCloudStack 私有云 ，调用时请配置 UCloudStack 私有云 URL 。**

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

