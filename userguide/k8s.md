# 23 容器服务

## 23.1 产品介绍
UCloudStack提供稳定可靠、通过CNCF一致性认证的，基于Kubernetes的容器应用管理服务。提供容器化应用的全生命周期管理，并与UCloudStack提供的分布式存储、负载均衡深度集成，让你轻松高效地在私有场景下构建云原生应用。

## 23.2 产品架构
虽然Kubernetes支持诸如NetworkPolicy、LimitRange、Quota、RBAC、Taint等隔离机制，但其本质上依然属于软隔离(Soft Multi-tenancy)，不能够有效地阻挡恶意用户（程序）对集群内部的其他用户（程序）发起攻击。
考虑到私有场景下依然会存在跨组织，跨企业的协作，需要防范不同租户（程序）之间互相攻击的情况出现，UCloudStack提供的容器服务需要具备强隔离(Hard Multi-tenancy)特性，每个UCloudStack上的租户均可申请自己的Kubernetes集群，不同租户之间的Kuberneters集群网络、权限、计算、存储完全隔离。

![k8sarch](../images/userguide/k8s_Service.png)

​																										UCloudStack容器服务逻辑架构



Master 默认三节点，节点为UCloudStack的虚拟机，通过keepalive实现apiserver高可用。Node 支持虚拟机和物理机，物理机必须是由UCloudStack纳管的物理机，否则无法加入集群。

另提供统一镜像仓库服务，不同租户无需另行搭建容器镜像仓库，只需要在仓库中创建私有命名空间即可。

![k8sarch](../images/userguide/k8s_arch.png)

​																												UCloudStack容器服务实现架构






## 23.3 相关概念

| 名称     | 对应字段    | 解释                                                         |
| -------- | ----------- | ------------------------------------------------------------ |
| 容器     | Containers  | Docker容器是通过镜像创建的可运行实例，一个节点可运行多个容器，容器之间相互隔离。 |
| Pod      | Pod         | Pod是Kubernetes管理的最小单位，一个Pod中可以有一个或多个容器。 |
| 命名空间 | Namespace   | 命名空间为Kubernetes提供虚拟的隔离作用。                     |
| 镜像     | Image       | Docker镜像是容器应用打包的标准方式，在部署容器化应用的时候选择。 |
| 集群     | Cluster     | 集群指运行所有容器时所需要的云资源的总合。                   |
| 节点     | Node        | 节点是运行工作负载的平台，其可以是虚拟机或物理机。每个节点都由Kubernetes控制平台管理。 |
| 管理节点 | Master Node | Kubernetes集群的管理节点，运行诸如调度器、控制器、ETCD等服务，一般情况下为3节点。 |
| 工作节点 | Worker Node | Kubernetes集群的工作节点，运行容器服务、公共服务等，至少为1节点. |



## 23.4 功能列表

| 序号 | 模块          | 功能                                                         |
| ---- | ------------- | ------------------------------------------------------------ |
| 1    | 集群管理      | 创建集群                                                     |
| 2    | 集群管理      | 删除集群                                                     |
| 3    | 集群管理      | 查看集群详情                                                 |
| 4    | 集群管理      | 查看/更新集群凭证                                            |
| 5    | 节点管理      | 添加节点                                                     |
| 6    | 节点管理      | 节点详情                                                     |
| 7    | 节点管理      | 删除节点                                                     |
| 8    | 节点管理      | 开启/关闭节点可调度性                                        |
| 9    | 命名空间管理  | 创建命名空间                                                 |
| 10   | 工作负载管理  | 支持表单创建或者YAML创建部署、守护进程集、任务、计时任务、有状态副本集、容器组 |
| 11   | 工作负载管理  | 支持删除、编辑、查看部署、守护进程集、任务、计时任务、有状态副本集、容器组 |
| 12   | 服务&路由管理 | 支持表单创建或者YAML创建服务&路由                            |
| 13   | 服务&路由管理 | 支持删除、编辑、查看服务&路由                                |
| 14   | 配置管理      | 支持表单创建或者YAML创建配置字典、保密字典                   |
| 15   | 配置管理      | 支持删除、编辑、查看配置字典、保密字典                       |




## 23.5 操作指南

### 23.5.1 集群管理

#### 23.5.1.1 创建集群

登陆UCloudStack控制台  — 导航栏【容器服务】— 【集群】— 【创建】

输入创建集群信息点击确认即可完成创建。


#### 23.5.1.2 删除集群

在集群概览页面勾选想要删除的集群，点击【删除】，完成二次确认后即可删除。



#### 23.5.1.3 查看集群详情

在集群概览页面点击想要查看集群的【详情】，进入集群查看页面。



#### 23.5.1.4 查看/更新集群凭证

集群凭证存放了用户访问和管理容器服务的配置文件信息，一般称为kubeconfig。凭证包含以下几个重要信息：

a. 集群名称及apiserver地址

b. 用户身份

c. 用户访问apiserver的证书



在集群详情界面，点击下方访问信息处的【更新凭证】即可进行更新；点击【内网】/【外网】即可查看对应凭证。

### 23.5.2 节点管理

#### 23.5.2.1 添加节点

登陆UCloudStack控制台  — 导航栏【容器服务】— 【集群】— 【节点管理】—【添加】

输入添加节点信息点击确认即可完成添加。



#### 23.5.2.2 节点详情

在节点管理界面点击想要查看节点的【详情】，即可进入节点详情界面。



#### 23.5.2.3 删除节点

在删除节点前，将率先驱逐该节点上的所有Pod，如果Pod驱逐失败，5分钟后将进行节点删除的操作，同时随Node节点一起创建的数据盘会被默认删除。

为了保证业务稳定运行，建议在删除节点前，人工驱逐该节点上的Pod。



在节点管理界面勾选想要删除的节点，点击【删除】，二次确认后即可删除该节点。



#### 23.5.2.4 开启/关闭节点可调度性

关闭可调度性将会阻止新的Pod调度至该节点，并不会影响已运行在该节点上的Pod。



在节点管理界面，点击开启/关闭按钮可以开关节点可调度性。

#### 23.5.2.5 Pod信息查看

在节点管理界面，点击【Pod信息】可以查看该节点内所有Pod的概览。

点击想要查看的Pod的【详情】，即可查看Pod详细情况。



### 23.5.3 命名空间管理

#### 23.5.3.1 创建命名空间

登陆UCloudStack控制台  — 导航栏【容器服务】— 【集群】— 【命名空间】—【创建】

输入命名空间名称后点击创建即可完成添加。



### 23.5.4 工作负载管理

Deployment ：适用于管理和部署无状态应用

StatefulSet：适用于管理和部署有状态应用

DaemonSet：适用于管理和部署用于本节点的功能性应用

CronJob：适用于管理和部署周期性应用

Job：适用于管理和部署一次性应用



#### 23.5.4.1 创建工作负载

登陆UCloudStack控制台 — 导航栏【容器服务】 — 【工作负载】 — 点击想要创建的工作负载类型

点击【创建】 进入工作负载创建界面

[基本设置] — [存储设置] — [容器设置] — [高级设置]

按顺序完成以上四步设置后，完成工作负载创建。

​	

#### 23.5.4.2 删除工作负载

进入想要删除的工作负载类型界面，勾选想要删除的工作负载，点击【删除】。

进行二次删除确认后，完成删除。



#### 23.5.4.3 查看工作负载

进入想要查看的工作负载类型界面，点击想要查看的工作负载的【详情】按钮。



#### 23.5.4.4 编辑工作负载

进入想要编辑的工作负载类型界面，点击想要编辑的工作负载的【编辑】按钮。

进入编辑界面后，可以选择[基本设置] — [存储设置] — [容器设置] — [高级设置]中的内容进行修改。

点击【保存】修改即可生效。



### 23.5.5 服务&路由管理

#### 23.5.5.1 创建服务&路由管理

登陆UCloudStack控制台 — 导航栏【容器服务】 — 【服务&路由】 — 点击【创建】



#### 23.5.5.2 删除服务&路由

进入想要删除的服务&路由界面，勾选想要删除的资源，点击【删除】。

进行二次删除确认后，完成删除。



#### 23.5.5.3 编辑服务&路由

进入想要编辑的服务&路由类型界面，点击想要编辑的资源的【编辑】按钮。

点击【保存】修改即可生效。



### 23.5.6 配置管理

#### 23.5.6.1 创建配置字典 & 保密字典

登陆UCloudStack控制台 — 导航栏【容器服务】 — 【配置】 — 点击【创建】



#### 23.5.6.2 删除配置字典 & 保密字典

进入想要删除的服务&路由界面，勾选想要删除的资源，点击【删除】。

进行二次删除确认后，完成删除。



#### 23.5.6.3 编辑配置字典 & 保密字典

进入想要编辑的服务&路由界面，点击想要编辑的资源后方的【编辑Yaml】。

进入该资源的Yaml文件编辑界面。



## 23.6 排障指南

### 23.6.1 入门必读

Kubernetes 提供了一系列的命令行工具来辅助我们调试和定位问题，本指南列举一些常见的命令来帮助应用管理者快速定位和解决问题。

**定位问题**

在开始处理问题之前，我们需要确认问题的类型，是 Pod ，Service ，或者 Controller（Deployment、StatefulSet） 的问题，然后分别使用不同的命令来查看故障原因。


**Pod常见命令**	

当我们发现 Pod 处于 Pending 状态，或者反复 crash，无法接受流量，可以使用以下命令来快速定位问题：

1. 获取 Pod 状态

```bash
kubectl -n ${NAMESPACE} get pod  -o wide 
```

2. 查看 Pod 的 yaml 配置

```bash
kubectl -n ${NAMESPACE} get pod ${POD_NAME}  -o 
```

3. 查看 Pod 事件

```bash
kubectl  -n ${NAMESPACE} describe pod ${POD_NAME}
```

4. 查看 Pod 日志

```bash
kubectl  -n ${NAMESPACE} logs ${POD_NAME} ${CONTAINER_NAME}
```

5. 登录 Pod

```
kubectl -n ${NAMESPACE} exec -it  ${POD_NAME} /bin/bash
```


**Controller 常见命令**

控制器负责 Pod 的生命周期管理，一般 Pod 无法被注册时，可以通过 Controller 来查看原因。这里以 Deployment 为例，介绍 Kubernetes Controller 的常用命令,其他 Controller 的命令类型与其一致。

1. 查看 Deployment 状态

```bash
kubectl -n ${NAMESPACE} get deploy -o wide
```

2. 查看 Deployment yaml 配置

```bash
kubectl -n ${NAMESPACE} get deploy ${DEPLOYMENT_NAME} -o yaml
```

3. 查看 Deployment 事件

```bash
kubectl -n ${NAMESPACE} describe deployment ${DEPLOYMENT_NAME}
```

**Service常见命令**

Service 描述了一组 Pod 的访问方式，当我们发现应用无法访问时，则需要使用 Service 命令来查看故障原因。

1. 查看 Service 状态

```bash
kubectl  -n ${NAMESPACE} get svc -o wide 
```

我们可以通过上述命令查看到 Service 的类型、集群内部和外部IP、暴露的端口，以及 Selector 信息。

2. 查看 Service 事件及负载均衡信息

```bash
kubectl  -n ${NAMESPACE} describe svc ${SERVICE_NAME} 

Name:              example-app
Namespace:         default
Labels:            app=example-app
Annotations:       <none>
Selector:          app=example-app
Type:              ClusterIP
IP:                10.2.192.27
Port:              web  8080/TCP
TargetPort:        8080/TCP
Endpoints:         192.168.59.207:8080,192.168.75.87:8080,192.168.84.90:8080
Session Affinity:  None
Events:            <none>
```

如上所示，我们可以通过这个命令查看到 Service 的 Endpoints 信息，Endpoints信息如果为空，则说明 Service 的配置信息有误，Service 无法将流量转发到相应的 Pod. 另外还有 Port 及 TargetPort 信息，确保与业务实际暴露的端口一致。



### 23.6.2 Pod故障处理

在Kubernetes中发布应用时，我们经常会遇到Pod出现异常的情况，如Pod长时间处于Pending状态，或者反复重启，下面介绍下Pod 的各种异常状态及处理思路。

**常见错误**

| 状态                  | 状态说明                         | 处理办法                                                    |
| --------------------- | -------------------------------- | ----------------------------------------------------------- |
| Error                 | Pod 启动过程中发生错误。         | 一般是由于容器启动命令、参数配置错误所致，请联系镜像制作者  |
| NodeLost              | Pod 所在节点失联。               | 检查 Pod 所在节点的状态                                     |
| Unkown                | Pod 所在节点失联或其他未知异常。 | 检查 Pod 所在节点的状态                                     |
| Pending               | Pod 等待被调度。                 | 资源不足等原因导致，通过 kubectl describe 命令查看 Pod 事件 |
| Terminating           | Pod 正在被销毁。                 | 可增加 --fore参数强制删除                                   |
| CrashLoopBackOff      | 容器退出，Kubelet 正在将它重启。 | 一般是由于容器启动命令、参数配置错误所致                    |
| ErrImageNeverPull     | 策略禁止拉取镜像。               | 拉取镜像失败，确认imagePullSecret是否正确                   |
| ImagePullBackOff      | 正在重试拉取。                   | 镜像仓库与集群的网络连通性问题                              |
| RegistryUnavailable   | 连接不到镜像仓库。               | 联系仓库管理员                                              |
| ErrImagePull          | 拉取镜像出错。                   | 联系仓库管理员，或确认镜像名是否正确                        |
| RunContainerError     | 启动容器失败。                   | 容器参数配置异常                                            |
| PostStartHookError    | 执行 postStart hook 报错。       | postStart 命令有误                                          |
| NetworkPluginNotReady | 网络插件还没有完全启动。         | cni 插件异常，可检查cni状态                                 |


### 23.6.3 Node故障处理

| 节点情况           | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| Ready              | True 表示节点是健康的，False 表示节点不健康，Unkown 表示节点失联 |
| DiskPressure       | True 表示节点磁盘容量紧张，False 反之                        |
| MemoryPressure     | True 表示节点内存使用率过高，False 反之                      |
| PIDPressure        | True 表示节点有太多进程在运行，False 反之                    |
| NetworkUnavailable | True 表示节点网络配置不正常，False 反之                      |



