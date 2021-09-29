# 导入自定义镜像

用户可以手动导入服务器的镜像文件，也可以由平台的虚拟机生成镜像。

# 前提条件
导入镜像前，请确认以下操作已经完成：
* 了解镜像限制和要求 [镜像限制](https://docs.ucloud.cn/UCloudStack/customimage/README?id=_2-镜像限制) 
* 已经根据 [导入镜像流程](https://docs.ucloud.cn/UCloudStack/customimage/README?id=_3-导入镜像流程) 进行镜像初始化的工具安装，并确保已经检查完整。
* 已经根据  [镜像转换流程](UCloudStack/customimage/convert.md) 完成镜像转换。

# 导入形式
更多详情请参考 [导入镜像](UCloudStack/userguide/image?id=_43-导入镜像)

|功能|应用场景| 推荐指数|
|:----------------|:------------|:------------|
|本地导入|镜像文件已经下载至本地操作系统，用户可以选择当前浏览器选择的本地文件进行上传。（仅限当前镜像的文件实际小于10G）|⭐|
|URL导入|使用云平台可达的HTTP/HTTPS等协议的URL地址，进行镜像文件的上传。|⭐⭐⭐|