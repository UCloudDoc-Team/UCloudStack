# 自定义镜像限制及制作流程

## 概述
平台除了可以使用虚拟机创建自定义镜像外，还可以支持镜像导入功能用于业务部署和迁移。可将本地或其他平台的服务器的镜像文件导入至平台中，导入后可以使用该镜像创建虚拟机或对已有的虚拟机进行重装系统操作。

## 镜像限制
为了保证自定义镜像的可用性并提高镜像导入效率，请在导入镜像前阅读本文提及的自定义镜像限制条件。

<table>
    <tr>
        <td>镜像属性</td>
        <td>条件</td>
    </tr>
    <tr>
        <td rowspan="4">操作系统</td>
        <td>CentOS 6.5 至 7.9</td>
    </tr>
    <tr>
        <td>Ubuntu 14.04 至 20.10</td>
    </tr>
        <td>Windows7 至 10</td>
    <tr>
        <td>Windows server 2008 至 2019</td>
    </tr>
    <tr>
        <td >镜像格式</td>
        <td>qcow2</td>
    </tr>
    <tr>
        <td rowspan="2">镜像大小</td>
        <td>镜像 vsize 不超过500G，使用 qemu-img info imageName | grep 'virtual size' 查看镜像 vsize</td>
    </tr>
    <tr>
        <td>本地上传的镜像建议小于10G 以下，URL上传的镜像小于500G以下，使用 qemu-img info imageName | grep 'disk size' 查看镜像实际大小</td>
    </tr>
    <tr>
    <td>驱动</td>
        <td>镜像必须安装虚拟化平台 KVM 的 Virtio 驱动，可参考 【导入镜像流程】进行自动安装，或参考<a href="/UCloudStack/customimage/linuxvirtio.md" title=" linux virtio驱动安装"> linux virtoi驱动安装</a> 、 <a href="/UCloudStack/customimage/windowsvirtio.md" title="windows virtio驱动安装"> windows virtoi驱动安装</a> 进行手动安装</td>
    </tr>
     <tr>
    </tr>
    <td>系统盘限制	</td>
        <td>检查系统盘的剩余空间，确保系统盘没有被写满</td>
    </tr>
    <td>文件系统	</td>
      <td>确保文件系统的完整性</td>
  </tr>
</table>


<span id = "ImportImage"></span>

## 导入镜像流程

1. 安装与检查初始化工具，更多详情，请参考[安装初始化工具与检查](/UCloudStack/customimage/install.md)
2. 转换镜像格式，更多详情，请参考[转换镜像格式](/UCloudStack/customimage/convert.md)
3. 导入镜像，更多详情，请参考[导入镜像](/UCloudStack/customimage/import.md)

