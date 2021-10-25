# 镜像格式转换

镜像制作完成后，需要将镜像转换为平台可识别的  QCOW2 格式才可导入并使用。本文介绍如何利用 qemu-img 工具将其他镜像格式转换成 QCOW2 的步骤：

1. 镜像制作完成后，关闭执行脚本的虚拟机；

2. 在kvm环境中，执行 `qemu-img info 镜像名称` 查看虚拟机磁盘信息；

   ![image](../images/customimage/customize_03.png)

   > 不同镜像可能文件格式不同，本指南以 QCOW2 格式镜像为例。

3. 执行 `qemu-img convert` 转换磁盘类型类 QCOW2 ，以供平台使用，具体命令如下：    

```
命令:
    qemu-img convert -f <raw/qcow2/vmdk/vhd> -O qcow2 <源盘文件> <输出盘文件>
举例:
    qemu-img convert -f raw -O qcow2 image-centos-74.raw image-centos-74.qcow2
```

4. 将转换完成的镜像文件上传并导入到平台，即可使用镜像创建并运行虚拟机。