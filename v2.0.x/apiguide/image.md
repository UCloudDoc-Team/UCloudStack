# 3 镜像管理

## 3.1 获取默认镜像和自制镜像信息-DescribeImage

获取镜像信息，包括默认镜像和自制镜像。

**Request Parameters**

| Parameter name | Type   | Description                                                  | Required |
| -------------- | ------ | ------------------------------------------------------------ | -------- |
| Region         | string | 地域。枚举值： cn，表示中国；                                | **Yes**  |
| Zone           | string | 可用区。枚举值：zone-01，表示中国；                           | No       |
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
| Zone             | string | 可用区。枚举值：zone-01，表示中国； | No       |
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
| Zone           | string | 可用区。枚举值：zone-01，表示中国；  | No       |
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

