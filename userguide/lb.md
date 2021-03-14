# 11 负载均衡

## 11.1 负载均衡简介

### 11.1.1 概述

负载均衡（ Load Balance ）是由多台服务器以对称的方式组成一个服务器集合，每台服务器都具有等价的地位，均可单独对外提供服务而无须其它服务器的辅助。平台负载均衡服务（简称 ULB—UCloudStack Load Balance）是基于 TCP/UDP/HTTP/HTTPS 协议将网络访问流量在多台虚拟机间自动分配的控制服务，类似于传统物理网络的硬件负载均衡器。

通过平台负载均衡服务提供的虚拟服务地址，将相同数据中心、相同 VPC 网络的虚拟机添加至负载均衡转发后端，并将加入的虚拟机构建为一个高性能、高可用、高可靠的应用服务器池，根据负载均衡的转发规则，将来自客户端的请求均衡分发给服务器池中最优的虚拟机进行处理。

支持内外网两种访问入口类型，分别提供 VPC 内网和 EIP 外网的负载访问分发，适应多种网络架构及高并发的负载应用场景。提供四层和七层协议的转发能力及多种负载均衡算法，支持会话保及健康检查等特性，可自动隔离异常状态虚拟机，同时提供 **SSL Offloading** 及 SSL 证书管理能力，有效提高整体业务的可用性及服务能力。

ULB 支持收集并展示负载流量各种网络指标的监控数据，并可根据告警模板进行监控报警及通知，保证业务的正常运行。当前负载均衡为接入的虚拟机服务池提供基于 NAT 代理的请求分发方式，在 NAT 代理模式下，所有业务的请求和返回数据都必须经过负载均衡，类似 LVS 的 NAT 工作模式。

### 11.1.2 应用场景

平台提供外网和内网两种类型的负载均衡服务，分别对应外网服务和内网服务两种场景。用户可根据业务需求，选择创建对外公开或对内私有的负载均衡实例，平台会根据负载均衡类型分别分配外网 IP 地址或 VPC 私有网络的 IP 地址，即负载均衡的服务访问地址。

- 外网类型的负载均衡使用场景：
  - 部署在平台的业务服务需要构建高可用虚拟机集群，且需对互联网提供统一访问入口。
  - 部署在平台的业务服务需要构建高可用虚拟机集群，且需对 IDC 数据中心提供统一访问入口。
- 内网负载均衡使用场景：
  - 部署在平台的业务服务需要构建高可用虚拟机集群，且仅需对 VPC 内网提供统一访问入口。
  - 部署在 VPC 私有网络的虚拟机集群需要对其它用户或服务屏蔽真实 IP 地址，对客户端提供透明化服务。

> 用户也可将负载均衡服务分配的 IP 地址与自有域名绑定在一起，通过域名访问后端应用服务。

### 11.1.3 架构原理

一个提供服务的负载均衡，主要由 LB 实例（ LoadBalancer ）、虚拟服务器（ VServer ）、后端服务器（ Backend Real Server ）三部分组成。如架构图所示：

![ulbarch](../images/userguide/ulbarch.png)

- LoadBalancer（ LB ）：负载均衡实例为主备高可用集群架构，可实现负载均衡器故障自动切换，提高接入负载均衡服务的可用性。同时结合内外网 IP 地址，根据 VServer 配置的监听器，将虚拟机加入到 Backend 成为 Real Server ，以实现业务的流量均衡与服务容错。
- Virtual Server（ VServer ）：监听器，每个监听器是一组负载均衡的监听端口配置，包含协议、端口、负载算法、会话保持、连接空闲超时及健康检查等配置项，用于分发和处理访问 LB 的请求。
- Backend Server Pool ：后端一组虚拟机服务器池，实际处理请求的真实服务器（RealServer），即真实部署业务的虚拟机实例。
- 外网 IP（ EIP ）：外网弹性 IP 地址，绑定至外网类型的 LB 实例上，对互联网或 IDC 数据中心提供业务负载均衡访问入口。
- 内网 IP （Private IP）：内网 IP 地址，内网类型 LB 实例提供服务的访问地址，通常是由创建内网负载均衡器时指定的 VPC 自动分配。

负载均衡器用于承载 VServer 及访问入口，VServer 负责访问入口地址的端口监听及请求分发。当负载均衡器接受到来自客户端的请求后，会通过一系列负载均衡算法，将访问请求路由分发到后端虚拟机服务器池进行请求处理，同时由 VServer 将处理结果返回给客户端。

* 通过加权轮询、最小连接数及基于源地址的负载均衡调度策略，进行业务请求流量转发，满足多场景业务负载需求，如加权轮询是按照后端服务器的权重进行请求转发，权重越大转发的请求越多。

- 通过会话保持机制，在请求会话的生命周期内，会将来自同一个客户端的会话转发至同一个虚拟机进行处理，适用于 TCP 长连接等应用场景。
- 通过健康检查机制，监控 RealServer 的运行状况及业务可用性，确保只将流量分发至业务健康的虚拟机。当 后端虚拟机业务不可访问时，调度器会停止向虚拟机分发负载流量；待虚拟机业务恢复正常后，会将虚拟机重新加入至 VServer 后端并分发流量至虚拟机。

**负载均衡器的工作模式为 NAT 请求代理，请求和返回均由负载均衡器进行转发和处理，即后端 RealServer 虚拟机处理请求后，会将请求返回给负载均衡 ，由负载均衡将结果返回给客户端。**

### 11.1.4 功能特性

平台负载均衡服务提供四层和七层转发能力，支持内网和外网两种网络入口，在多种负载调度算法基础之上支持健康检查、会话保持、连接空闲超时、内容转发及 **SSL Offloading** 和 SSL 证书管理等功能，保证后端应用服务的可用性和可靠性。

- 支持内网和外网两种类型负载均衡器，满足 VPC 内网、IDC 数据中心及互联网服务负载均衡应用场景。
- 提供四层和七层业务负载分发能力，支持基于 TCP、UDP、HTTP 及 HTTPS 协议的监听及请求转发。
- 支持加权轮询、最小连接数和基于源地址的的负载调度算法，满足不同场景的流量负载业务。
  - 加权轮询：基于权重的轮询调度，负载均衡器接收到新的访问请求后，根据用户指定的权重，按照权重概率分发流量至各后端虚拟机，进行业务处理；
  - 最小连接数：基于后端服务器最小连接数进行调度，负载均衡器接收到新的访问请求后，会实时统计后端服务器池的连接数，选择连接数最低的虚拟机建立新的连接并进行业务处理；
  - 源地址：基于客户端源 IP 地址的调度策略，采用哈希算法将来源于相同 IP 地址的访问请求均转发至一台后端虚拟机进行处理。
- 提供会话保持功能，在会话生命周期内，保证同一个客户端的请求转发至同一台后端服务节点上。四层和七层分别采用不同的方式进行会话保持 。
  - 针对 UDP 协议，基于 IP 地址保证会话保持，将来自同一 IP 地址的访问请求转发到同一台后端虚拟机进行处理，支持关闭会话 UDP 协议的会话保持；
  - 针对 HTTP 和 HTTPS 协议，提供 Cookie 植入的方式进行会话保持，支持自动生成KEY 和自定义 KEY。自动生成 KEY 是由平台自动生成 Key 进行植入，自定义 Key 是由用户自定义 Key 进行植入。
- 支持 TCP、HTTP 及 HTTPS 协议的连接空闲超时配置，自动中断在超时时间内一直无访问请求的连接。
  - 客户端向 LB 地址发送的请求，在平台会维护两个连接，一个由客户端到 LB，一个由 LB 到后端虚拟机；
  - 连接空闲超时是指由客户端到 LB 的连接空闲超时，若在超时周期内没有发送或接收任何数据，将自动中断从客户端到 LB 的连接；
  - 默认连接空闲超时周期为 60 秒，即在建立连接后的 60 秒内一直没有新的数据请求，将自动中断连接。
- 健康检查：支持端口检查和 HTTP 检查，根据规则对后端业务服务器进行业务健康检查，可自动检测并隔离服务不可用的虚拟机，待虚拟机业务恢复正常后，会将虚拟机重新加入至 VServer 后端并分发流量至虚拟机。
  - 端口检查：针对四层和七层负载均衡，支持按 IP 地址 + 端口的的方式探测后端服务节点的健康状况，及时剔除不健康的节点；
  - HTTP 检查：针对七层负载均衡，支持按 URL 路径和请求 HOST 头中携带的域名进行健康检查，筛选健康节点。
- 内容转发：针对七层 HTTP 和 HTTPS 协议的负载均衡，支持基于域名和 URL 路径的流量分发及健康检查能力，可将请求按照域名及路径转发至不同的后端服务节点，提供更加精准的业务负载均衡功能。
- SSL 证书：针对 HTTPS 协议，提供统一的证书管理服务和 **`SSL Offloading `** 能力，并支持 HTTPS 证书的单向和双向认证。SSL 证书部署至负载均衡，仅在负载均衡上进行解密认证处理，无需上传证书到后端业务服务器，降低后端服务器的性能开销。
- 获取客户端真实 IP：HTTP 监听器支持附加 HTTP header 字段，通过 X-Forwarded-For 和 X-Real-IP 获取客户端真实 IP 地址。
- 获取监听器协议：HTTP 监听器支持附加 HTTP header 字段，通过 X-Forwarded-Proto 获取监听器的协议。
- 附加 HTTP HOST：HTTP 监听器支持附加 HTTP header 字段，通过 Host 附加 HOST 域名至 HTTP 请求中，用于适配需要检测 HTTP 头 HOST 字段的业务。
- 监控数据：负载均衡级别提供每秒连接数、每秒出/入流量、每秒出/入包数量的监控及告警；VServer 级别提供连接数、HTTP 2XX、HTTP 3XX、HTTP 4XX、HTTP 5XX 等监控数据及告警。
- 安全控制：通过安全组对外网负载均衡的访问进行安全管控，仅允许安全组规则内的流量透传负载均衡到达后端真实服务器，保证业务负载的安全性。

负载均衡为用户提供业务级别的高可用方案，可以将业务应用同时部署至多个虚拟机中，通过负载均衡和 DNS 域名的方案设置流量均衡转发，实现多业务级别的流量负载均衡。当大并发流量通过负载均衡访问虚拟机业务时，可通过最小连接数、加权轮询等算法，将请求转发给后端最健壮的虚拟机进行处理，请通过负载均衡将请求结果返回给客户端，保证业务可用性和可靠性。

> 用户可通过智能 DNS 服务，将两个数据中心的负载均衡实例同时绑定至一个域名，使用 DNS 实现跨数据中心的业务容灾方案。

### 11.1.5 负载均衡隔离性

- 资源隔离
  - 负载均衡具有数据中心属性，不同数据中心间负载均衡资源物理隔离；
  - 负载均衡资源在租户间相互隔离，租户可查看并管理账号及子账号下所有负载均衡资源；
  - 一个租户内的负载均衡资源，仅支持绑定租户内同数据中心的 VPC 子网资源；
  - 一个租户内的负载均衡资源，仅支持绑定租户内同数据中心的外网 IP 资源；
  - 一个租户内的负载均衡资源，仅支持绑定租户内同数据中心的安全组资源。
- 网络隔离
  - 不同数据中心间负载均衡资源网络相互物理隔离；
  - 同数据中心负载均衡网络采用 VPC 进行隔离，不同 VPC 的负载均衡资源无法相互通信；
  - 负载均衡绑定的外网 IP 网络隔离取决于用户物理网络的配置，如不同的 Vlan 等。

## 11.2 负载均衡管理

### 11.2.1 使用流程

在使用负载均衡服务前，需根据业务需求规划负载均衡的网络类型及监听类型，并根据业务需求在平台部署并配置好业务虚拟机，具体流程如下：

1. 根据业务需求和规划，在平台创建并部署多台业务虚拟机，并保证业务在单台虚拟机的可用性；
2. 根据业务需求，选择负载均衡的网络入口类型及所属 VPC ，在云平台部署高可用负载均衡实例；
3. 在已创建的负载均衡实例中，根据需求配置监听器 VServer ，包括服务的协议、端口、负载均衡算法、证书、会话保持及健康检查等参数；
4. 为已配置的 VServer 添加服务节点来确定负载均衡入口请求路由的目标，即将第 1 步部署的业务虚拟机实例添加至 VServer 的服务节点；
5. 负载均衡会对添加至 VServer 的服务节点立即进行业务健康检查，并及时剔除不健康的服务节点；
6. 通过负载均衡服务提供的统一入口 IP 地址访问业务服务。

### 11.2.2 创建负载均衡

用户在平台创建负载均衡需指定所属集群类型、网络类型、VPC 网络、子网、外网 IP 及安全组等信息，可通过导航栏进入【负载均衡】资源控制台，通过“创建负载均衡”进入向导页面，如下图所示：

![createlb1](../images/userguide/createlb1.png)

> 本文以创建外网类型的负载均衡进行描述，内网类型的负载均衡无需指定外网 IP 和安全组信息。

1. 选择并配置负载均衡器的基础配置及网络信息：

- 机型：负载均衡实例所在宿主机的集群类型，由平台管理员自定义，如 x86 机型和 ARM 机型，通过 ARM 机型创建的实例为 ARM 版负载均衡实例，已适配国产芯片、服务器及操作系统。
- 网络类型：负载均衡实例网络入口的类型，可选择内网和外网。内网类型提供所属 VPC 的网络入口地址，外网类型以绑定的外网 IP 地址为负载均衡的网络入口地址。
- VPC 网络：负载均衡所服务的 VPC 网络，仅支持将相同 VPC 网络的虚拟机加入到负载均衡服务节点中提供负载均衡服务，同时负载均衡实例本身会运行在所指定的 VPC 网络中。
- 子网：负载均衡实例所在子网，系统将自动根据所选子网分配内网 IP 地址作为内网负载均衡的入口地址，通常常建议选择可用 IP 数量充足的子网。
- 外网 IP ：当网络类型为外网时，可配置负载均衡实例自动绑定的外网 IP 地址，仅支持绑定 IPv4 且有默认路由的 外网 IP 地址作为负载均衡的入口地址。
- 安全组：当网络类型为外网时，可配置负载均衡自动绑定的外网安全组，用于外网访问负载均衡的安全控制。
- 实例名称/备注：负载均衡实例的名称及备注信息。

2. 选择购买数量和付费方式，确认订单金额并点击“立即购买” 进行负载均衡实例的创建。

- 购买数量：按照所选配置及参数批量创建多个负载均衡实例，一次仅支持创建 1 个负载均衡实例； 
- 付费方式：选择负载均衡的计费方式，支持按时、按年、按月三种方式，可根据需求选择适合的付费方式；
- 合计费用：用户选择负载均衡资源按照付费方式的费用展示；

确认订单无误后点击立即购买，点击立即购买后，会返回负载均衡资源列表页，在列表页可查看资源的创建过程，通常会先显示“创建中”的状态，分钟内转换为“**有效**”状态，即代表创建成功。

### 11.2.3 查看负载均衡

通过导航栏进入负载均衡控制台，可查看负载均衡资源列表，并可通过列表上名称和 ID 进入详情页面查看负载均衡的概览及监控信息，同时可切换至 VServer 标签页对负载均衡的 VServer 进行管理。

#### 11.2.3.1 负载均衡列表

负载均衡列表可查看当前账户下所有负载均衡资源信息，包括名称、资源 ID、IP、VPC、子网、VServer 数量、创建时间、过期时间、计费方式、状态及操作项，如下图所示：

![lblist](../images/userguide/lblist.png)

- 名称/ID：负载均衡的名称及全局唯一标识符。
- IP 地址：负载均衡对外提供服务的访问地址，网络类型为内网时为所属子网自动分配的 IP 地址，网络类型为外网时为所绑定的外网 IP 地址。
- VServer 数量：负载均衡实例上已创建的监听器 VServer 数量。
- 创建时间/过期时间：负载均衡的创建时间及费用过期时间。
- 计费方式：负载均衡创建时指定的计费方式。
- 状态：负载均衡的的运行状态，包括创建中、有效、删除中等。

列表上操作项是指对单个负载均衡实例的操作，包括删除、修改告警模板、修改安全组等，可通过搜索框对负载均衡资源列表进行搜索和筛选，支持模糊搜索。

为方便租户对资源的统计及维护，平台支持下载当前用户所拥有的所有负载均衡资源列表信息为 Excel 表格；同时支持对负载均衡进行批量删除操作。

#### 11.2.3.2 负载均衡详情

在负载均衡资源列表上，点击“**名称**” 可进入概览页面查看当前负载均衡实例的详细信息，同时可切换至 VServer 页面对当前负载均衡的 VServer 监听器进行管理，如概览页所示：

![lbdetails](../images/userguide/lbdetails.png)

**（1）基本信息**

负载均衡器的基本信息，包括资源 ID、名称、创建时间、过期时间、计费方式、状态及告警模板信息，可点击告警模板右侧按钮修改负载均衡所关联的告警模板。

**（2）网络信息**

负载均衡的网络入口相关信息，包括 VPC 网络、子网及内网 IP 地址，若负载均衡为外网类型，会展示外网 IP 地址及所绑定的安全组信息。

**（3）监控信息**

负载均衡实例相关的监控图表及信息，包括新建连接数、出/入流量及出/入包数量，支持查看 1 小时、6 小时、12 小时、1 天及自定义时间的监控数据。

**（4）VServer 管理**

当前负载均衡的监听器生命周期管理，包括 VServer 的添加、查看、修改、删除操作管理，同时还可对 VServer 的后端服务节点及七层内容转发规则进行管理，详见 [VServer 管理](#_103-VServer 管理) 。

### 11.2.4 修改告警模板

修改告警模板是对负载均衡器的监控数据进行告警的配置，通过告警模板定义的指标及阈值，可在负载均衡相关指标故障及超过指标阈值时，触发告警，通知相关人员进行故障处理，保证负载均衡及业务的网络通信。

用户可通过负载载均衡列表或详情概览页的操作项进行告警模板修改操作，在修改告警模板向导中选择新负载均衡告警模板进行修改。

### 11.2.5 修改安全组

支持在负载均衡的视角修改安全组，仅当负载均衡实例的网络类型为**外网**时才可修改负载均衡的安全组。可通过负载均衡列表操作项中的“**修改安全组**”进行修改操作，如下图所示：

![updatelbsg](../images/userguide/updatelbsg.png)

一个负载均衡仅支持绑定一个安全组，修改成功后外网负载均衡会以新的安全组策略对进出流量进行限制，用户可通过负载均衡详情网络信息查看已绑定的安全组信息。

### 11.2.6 修改名称和备注

修改负载均衡资源的名称和备注，在任何状态下均可进行操作。可通过点击负载均衡资源列表页面每个负载均衡名称右侧的“编辑”按钮进行修改。

### 11.2.7 删除负载均衡

用户可通过控制台或 API 的方式删除不需要的负载均衡实例，删除负载均衡时会自动解绑已关联的外网 IP 、后端服务节点及绑定的 SSL 证书，并清除负载均衡已创建的 VServer 监听器及内容转发规则策略。

![rmlb](../images/userguide/rmlb.png)

负载均衡实例删除即被直接销毁，删除前需确保负载均衡无业务流量请求，否则可能影响业务的正常访问。

### 11.2.8 负载均衡续费

支持用户手动对负载均衡进行续费，续费操作只针对资源本身，不对资源额外关联的资源进行续费，如绑定至负载均衡的外网 IP 资源。额外关联的资源到期后，会自动与负载均衡解绑，为保证业务正常使用，需及时对相关资源进行续费操作。

负载均衡续费时会按照续费时长收取费用，续费时长与资源的计费方式相匹配，当负载均衡的计费方式为【小时】，则续费时长可选择 1 至 24 小时；当负载均衡的计费方式为【按月】，则续费时长可选择 1 至 11 月；当负载均衡的计费方式为【按年】，则负载均衡的续费时长为 1 至 5 年。

## 11.3 VServer 管理

VServer 即负载均衡的监听器，主要承载负载均衡业务网络的四层和七层监听，通过负载均衡 IP 地址的请求仅能访问被监听的协议和端口，并根据调度算法定义的转发策略将请求流量分发至后端服务节点。

用户可对监听器进行添加、修改、删除、查看管理，同时可对 VServer 的后端服务节点及七层内容转发规则进行管理。针对每一个 VServer 监听器，用户可对监听协议、端口、负载均衡算法、会话保持及健康检查进行配置，若协议为 HTTP 或 HTTPS，可进行七层内容转发或 SSL 证书的配置和管理。一个负载均衡支持多个 VServer 监听器，每个监听器对应一个应用负载均衡服务。

### 11.3.1 添加 VServer

添加 VServer 是指为一个负载均衡器添加监听器，用于对负载均衡的 IP 地址进行服务监听，使用户可通过负载均衡的 IP 地址进行业务负载访问。支持用户根据应用需求，分别创建 TCP、UDP、HTTP、HTTPS 协议的监听器，如为负载均衡器添加一个 `HTTP:80` 的 VServer 监听器，基于负载均衡提供高可用 WEB 服务。

* TCP 监听器：基于 TCP 协议的监听器，即仅监听 TCP 的端口，适用于注重可靠性，对数据准确性要求高，如文件传输 FTP、发送或接收邮件 SMTP&POP3、远程登录 22/3389 等。
* UDP 监听器：基于 UDP 协议的监听器，关注实时性而相对不注重可靠性的场景，如 DNS 应用等。
* HTTP 监听器：基于 HTTP 协议及内容转发策略的监听器 ，适用于 WEB 服务及应用服务。
* HTTPS 监听器：基于 HTTPS 及证书加密的监听器，适用于加密传输的应用服务。

#### 11.3.1.1 添加 TCP 监听器

用户为负载均衡实例创建一个基于 TCP 协议的监听器，提供注重可靠性的负载均衡服务，本文以创建 `TCP:23` （Telnet）服务为例进行创建。用户可通过负载均衡详情页面 VServer 左侧导航栏的【添加】进入 VServer 监听器的创建向导页面，如下图所示：

![tcpvs](../images/userguide/tcpvs.png)

根据向导页面配置 VServer 监听器，包括协议、端口、负载均衡算法、连接空闲超时及健康检查：

* 监听协议：负载均衡业务的网络协议，支持 TCP、UDP、HTTP、HTTPS ，本示例中选择 TCP 。

* 端口：负载均衡业务对外或对内提供服务时用来接收请求的应用端口，端口范围为 `1~65535` ，本示例使用 23 端口，用于提供高可用的 Telnet 服务。

  > 323、9102、9103、9104、9105、60909、60910 等端口被占用，在任何协议下均不可使用。

* 负载均衡算法：负载均衡分发请求到后端 RealServer 的调度计算策略，支持加权轮询、最小连接数和源地址三种算法。

* 连接空闲超时：客户端至负载均衡的连接空闲超时限制，范围为 `1~86400` 秒。默认值为 60 秒，即在 60 秒内客户端对负载均衡一直无访问请求，平台会自动中断连接。

* 健康检查：根据规则对后端服务节点进行健壮性检查，可自动检测并隔离服务不可用的后端服务节点。TCP 协议仅支持端口检查，即通过 IP:端口的方式检测业务的可用性。

创建过程中 VServer 的资源状态为“创建中”，待状态更新为“运行”即代表创建成功，用户可通过 VServer 列表及详情查看已添加的 TCP 监听器，并可查看 VServer 下所有服务节点的健康状态。

#### 11.3.1.2 添加 UDP 监听器

用户为负载均衡实例创建一个基于 UDP 协议的监听器，提供基于 UDP 协议的负载均衡业务服务，本文以创建 `UDP:53` （DNS）服务为例进行创建。用户可通过负载均衡详情页面 VServer 左侧导航栏的【添加】进入 VServer 监听器的创建向导页面，如下图所示：

![udpvs](../images/userguide/udpvs.png)

根据向导页面配置 VServer 监听器，包括协议、端口、负载均衡算法、会话保持及健康检查：

* 监听协议：负载均衡业务的网络协议，支持 TCP、UDP、HTTP、HTTPS ，本示例中选择 UDP 。

* 端口：负载均衡业务对外或对内提供服务时用来接收请求的应用端口，端口范围为 `1~65535` ，本示例使用 53端口，用于提供高可用的 DNS 服务。

  > 323、9102、9103、9104、9105、60909、60910 等端口被占用，在任何协议下均不可使用。

* 负载均衡算法：负载均衡分发请求到后端 RealServer 的调度计算策略，支持加权轮询和源地址三种算法。

* 会话保持：针对 UDP 协议，基于 IP 地址保证会话保持，将来自同一 IP 地址的访问请求转发到同一台后端虚拟机进行处理，可选择开启或关闭 UDP 协议的会话保持功能。

* 健康检查：根据规则对后端服务节点进行健壮性检查，可自动检测并隔离服务不可用的后端服务节点。UDP 协议仅支持端口检查，即通过 IP:端口的方式检测业务的可用性。

创建过程中 VServer 的资源状态为“创建中”，待状态更新为“运行”即代表创建成功，用户可通过 VServer 列表及详情查看已添加的 UDP 监听器，并可查看 VServer 下所有服务节点的健康状态。

#### 11.3.1.3 添加 HTTP 监听器

用户为负载均衡实例创建一个基于 HTTP 协议的监听器，提供基于 HTTP 协议的负载均衡业务服务，本文以创建 `HTTP:80` （WEB）服务为例进行创建。用户可通过负载均衡详情页面 VServer 左侧导航栏的【添加】进入 VServer 监听器的创建向导页面，如下图所示：

![httpvs](../images/userguide/httpvs.png)

根据向导页面配置 VServer 监听器，包括协议、端口、负载均衡算法、会话保持、连接空闲超时及健康检查：

* 监听协议：负载均衡业务的网络协议，支持 TCP、UDP、HTTP、HTTPS ，本示例中选择 HTTP 。

* 端口：负载均衡业务对外或对内提供服务时用来接收请求的应用端口，端口范围为 `1~65535` ，本示例使用 80端口，用于提供基于 HTTP 协议的高可用 WEB 服务。

  > 323、9102、9103、9104、9105、60909、60910 等端口被占用，在任何协议下均不可使用。

* 负载均衡算法：负载均衡分发请求到后端 RealServer 的调度计算策略，支持加权轮询、最小连接数和基于源地址三种算法。

* 会话保持：针对 HTTP 和 HTTPS 协议，提供 Cookie 植入的方式进行会话保持，支持自动生成KEY 和自定义 KEY。选择自动生成 KEY 则由平台自动生成 Key 进行植入，选择自定义 Key 时需输入 KEY 值（只能输入数字、字母及_字符）。

* 连接空闲超时：客户端至负载均衡的连接空闲超时限制，范围为 `1~86400` 秒。默认值为 60 秒，即在 60 秒内客户端对负载均衡一直无访问请求，平台会自动中断连接。

* 健康检查：根据规则对后端服务节点进行健壮性检查，可自动检测并隔离服务不可用的后端服务节点。HTTP协议端检查和 HTTP 检查两种方式，其中 HTTP 检查支持按 URL 路径和请求 HOST 头中携带的域名进行健康检查，筛选健康节点。

  * HTTP 健康检查路径：默认为 `/` ，可输入 Linux 格式的路径，只能使用字母、数字、和-/.%?#& 这些字符，必须以 `/` 开头，例如 `/data` ；
  * HTTP 健康域名：检查时校验请求的 HOST 字段中的域名，可输入标准域名用于校验请求 host 字段中携带的域名。

> HTTP 健康检查中的域名作用：某些应用服务器会对请求中的 host 字段做校验，即要求请求头中必须存在 host 字段。若在健康检查中配置了域名，则负载均衡会将域名配置到 host 字段中，并在健康检查时携带域名对后端服务节点进行检查，若健康检查请求被服务节点拒绝，则健康检查失败，即代表服务节点状态为异常；若应用服务器需要校验请求的host字段，则需要配置相关域名，确保健康检查正常工作。

创建过程中 VServer 的资源状态为“创建中”，待状态更新为“运行”即代表创建成功，用户可通过 VServer 列表及详情查看已添加的 HTTP 监听器，并可查看 VServer 下所有服务节点的健康状态。

#### 11.3.1.4 添加 HTTPS 监听器

用户为负载均衡实例创建一个基于 HTTP 协议的监听器，提供基于 HTTPS 协议的负载均衡业务服务，本文以创建基于 SSL 证书加密的 `HTTPs:443` （WEB）服务为例进行创建。用户可通过负载均衡详情页面 VServer 左侧导航栏的【添加】进入 VServer 监听器的创建向导页面，如下图所示：

![httpsvs](../images/userguide/httpsvs.png)

根据向导页面配置 VServer 监听器，包括协议、端口、SSL 解析模式、服务器证书、客户端证书、负载均衡算法、会话保持、连接空闲超时及健康检查：

* 监听协议：负载均衡业务的网络协议，支持 TCP、UDP、HTTP、HTTPS ，本示例中选择 HTTPS 。

* 端口：负载均衡业务对外或对内提供服务时用来接收请求的应用端口，端口范围为 `1~65535` ，本示例使用 443 端口，用于提供基于 HTTPS 协议且使用 SSL 证书加密认证的高安全、高可用 WEB 服务。

  > 323、9102、9103、9104、9105、60909、60910 等端口被占用，在任何协议下均不可使用。

* SSL 解析模式：HTTPS 协议的 SSL 证书认证的解析模式，支持单向认证和双向认证，通常选择单向认证。

  * 单向认证：由网站服务端提供  SSL 证书并进行身份验证，保证 HTTPS 网站的数据安全性，任何访问网站的用户无需拥有 CA 证书即可随时访问网站；
  * 双向认证：网站服务端和用户双方均需提供 SSL 证书，只有提供 CA 证书的客户端才允许访问网站。

* 服务器证书：用户证明服务器的身份，HTTPS 检查服务器发送的证书是否是由自己信赖的中心签发。
  * 部署并配置于负载均衡服务器中，为负载均衡后端服务节点的网站提供 SSL 服务器证书及验证。
  * 在创建时需提前上传服务器证书到平台，可通过新建服务器证书进行上传，详见 [SSL 证书管理](#_106-SSL-证书管理)。

* 客户端证书：客户端CA公钥证书用于验证客户端证书的签发者，HTTPS 双向认证中需验证客户端提供的证书，才可成功建立连接。
  * 网站服务器用 CA 证书验证客户端证书的签名，如果没有通过验证，则拒绝连接；
  * 在创建时需提前上传客户端证书到平台，可通过新建客户端证书进行上传，详见 [SSL 证书管理](#_106-SSL-证书管理)。
* 负载均衡算法：负载均衡分发请求到后端 RealServer 的调度计算策略，支持加权轮询、最小连接数和基于源地址三种算法。

* 会话保持：针对 HTTP 和 HTTPS 协议，提供 Cookie 植入的方式进行会话保持，支持自动生成KEY 和自定义 KEY。选择自动生成 KEY 则由平台自动生成 Key 进行植入，选择自定义 Key 时需输入 KEY 值（只能输入数字、字母及_字符）。
* 连接空闲超时：客户端至负载均衡的连接空闲超时限制，范围为 `1~86400` 秒。默认值为 60 秒，即在 60 秒内客户端对负载均衡一直无访问请求，平台会自动中断连接。
* 健康检查：根据规则对后端服务节点进行健壮性检查，可自动检测并隔离服务不可用的后端服务节点。HTTP协议端检查和 HTTP 检查两种方式，其中 HTTP 检查支持按 URL 路径和请求 HOST 头中携带的域名进行健康检查，筛选健康节点。
  * HTTP 健康检查路径：默认为 `/` ，可输入 Linux 格式的路径，只能使用字母、数字、和-/.%?#& 这些字符，必须以 `/` 开头，例如 `/data` ；
  * HTTP 健康域名：检查时校验请求的 HOST 字段中的域名，可输入标准域名用于校验请求 host 字段中携带的域名。

创建过程中 VServer 的资源状态为“创建中”，待状态更新为“运行”即代表创建成功，用户可通过 VServer 列表及详情查看已添加的 HTTPS 监听器，并可查看 VServer 下所有服务节点的健康状态。

> VServer 监听器配置完成后，需添加业务虚拟机至监听器的服务节点中才可正常提供服务。 HTTP 和 HTTPS 协议的监听器可根据需求配置内容转发规则，根据请求的域名和 URL 进行精准的请求分发。

### 11.3.2 查看 VServer

通过负载均衡详情页面进入 VServer 资源控制台，可查看当前负载均衡实例中已拥有 VServer 列表信息及所属服务节点的健康状况，并可通过列表名称切换 VServer 在右侧概览中查看 VServer 的基本信息及监控信息，同时可切换至服务节点和内容转发标签页进行服务节点和内容转发规则的管理。

#### 11.3.2.1 VServer 列表

VServer 列表页面可查看当前负载均衡实例中已拥有的 VServer 资源列表，包括协议端口和状态，如下图所示：

![vslist](../images/userguide/vslist.png)

- 协议端口：VServer 监听器协议和端口，是负载均衡处理请求的入口依据；
- 状态：VServer 监听器的服务状态，包括绿色、黄色和红色；
  - 绿色：VServer 中添加的所有服务节点的健康状态均为正常；
  - 黄色：VServer 中添加的部分服务节节异常；
  - 红色：VServer 中添加的所有服务节点健康状态为异常，即代表 VServer 停止工作；若未添加任何服务节点，VServer 的默认状态为全部异常。

在列表页可对 VServer 进行添加、修改及删除操作，通过点击 VServer 可在右侧查看当前 VServer 的详细信息，点击状态按钮可显示状态描述。

#### 11.3.2.2 VServer 详情

通过 VServer 资源列表的“**协议端口**” 可在右侧查看 VServer 详情页面，可查看当前 VServer 资源的详细信息，如下图所示，详情页面分为基本信息、VServer 监控信息、服务节点管理及内容转发信息：

![vsdetails](../images/userguide/vsdetails.png)

**（1）基本信息**

VServer 的基本信息，包括 ID、协议端口、负载均衡算法、会话保持、会话保持 Key、连接空闲超时、健康检查方式、运行状态、VS 状态、告警模板及创建时间等信息。若 VServer 监听协议为 HTTP/HTTPS ，可查看 HTTP 健康检查路径、HTTP 检查域名、SSL解析模式、服务器证书及客户端证书等信息。

- 会话保持：会话保持的开关和类型。UDP 协议值为开启或关闭，HTTP/HTTPS 协议值为关闭、自动生成 KEY 或自定义 KEY 。
- 运行状态：VServer 监听器的服务状态，包括全部异常、部分异常、全部正常。
- VS 状态：VServer 监听器资源的状态，包括可用、更新中、已删除。
- 告警模板：VServer 绑定的监控告警模板，若未绑定则展示为无。
- 服务器证书/客户端证书：HTTPS 监听器 SSL 证书名称，可通过查看证书查询证书的内容。

**（2）监控信息**

VServer 实例相关监控图表及信息，包括新建连接数、HTTP 2XX 、HTTP 3XX 、HTTP 4XX 、HTTP 5XX ，支持查看 1 小时、6 小时、12 小时、1 天及自定义时间的监控数据。

**（3）服务节点和内容转发**

- 服务节点：VServer的服务节点生命周期管理，包括服务节点的添加、查看、修改、启用、禁用及删除等，详见[服务节点管理](#_104-服务节点管理)。
- 内容转发规则：当前 VServer 配置的内容转发规则生命周期管理，包括转发规则的添加、查看、修改及删除，详见[内容转发规则管理](#_105-内容转发规则管理)。

### 11.3.3 修改 VServer

用户通过控制台修改 VServer 监听器配置，如修改监听器的负载均衡算法、会话保持、连接空闲超时及健康检查配置信息，若协议为 HTTPS 可更换监听器的 SSL 解析模式及 SSL 证书。可通过 VServer 列表上的 “修改” 按钮进行修改操作，如修改向导所示：

![updatevs](../images/userguide/updatevs.png)

修改配置的参数设置与创建 VServer 时一致，不支持修改 VServer 的协议和端口。修改过程中 VS 状态由 “运行” 变更为 “更新中” ，更新成功后流转为 “运行” ，即代表更新成功，可通过详情页面查看新修改的配置。修改成功后，平台会立即根据新配置重新对服务节点进行健康检查，同时会根据新修改的调度算法分发请求。

> 修改 VServer 的调度算法、会话保持、连接空闲超时，仅对新连接生效，不影响已建立连接的服务。

### 11.3.4 修改告警模板

修改告警模板是对 VServer 的监控数据进行告警的配置，通过告警模板定义的指标及阈值，可在 VServer 相关指标故障及超过指标阈值时，触发告警，通知相关人员进行故障处理，保证负载均衡及业务的网络通信。

用户可通过 VServer 详情概览页的操作项进行告警模板修改操作，在修改向导页面中选择新告警模板进行修改。VServer 和 负载均衡器可共用一个负载均衡监控告警模板，即在负载均衡的告警模板中即可定义 LB 实例的指标告警策略，同时可定义 VServer 的监控指标告警，可根据需求自定义告警模板的规则。

### 11.3.5 删除 VServer

用户可通过控制台或 API 的方式删除 VServer 资源，删除时会自动清除 VServer 下已创建的内容转发规则策略，同时会自动解绑已关联的 SSL 证书，仅当 VServer 中不存在后端 RealServer 资源时才可进行删除操作。

![rmvs](../images/userguide/rmvs.png)

VServer 删除后不可恢复，在删除时需检查并确认是否有必要删除 VServer 资源，控制台 VServer 标签页可查看删除过程，待被删除的 VServer 资源被清空时，代表删除成功。

## 11.4 服务节点管理

服务节点指负载均衡架构中的后端真实服务器，即 RealServer ，用于提供真正业务并处理业务请求的服务池，一般是由多台虚拟机集群构成。

- 添加服务节点需要在 VServer 监听器创建完成后才可进行添加。
- 服务节点添加后，负载均衡即通过健康检查 Check 服务节点的业务是否正常。
- 若业务节点无法正常处理 VServer 发送的请求，平台会提示服务节点状态为无效，需检测服务节点中部署的业务状况。
- 若业务节点可正常处理 Check 请求，即服务节点状态为有效，则代表负载均衡可正常工作。

### 11.4.1 添加服务节点

添加服务节点前，需确保服务节点上业务正常运行且可进行正常访问。可通过 VServer 详情页面进入“**服务节点**”资源控制台，点击“**添加服务节点**”进行后端 RealServer 的添加。添加服务节点时，仅可选择与负载均衡实例在相同 数据中心且 VPC 网络相同的虚拟机。如下图所示：

![creaters](../images/userguide/creaters.png)

* 虚拟机：即需要添加至负载均衡当前 VServer 服务节点的虚拟机，支持指定服务节点暴露的端口及权重。
* 端口：后端服务节点暴露的服务端口，如 VServer 监听 80 ，服务节点监听 8080 端口，则在端口处输入 8080 即可，负载均衡会将到达 VServer 80  端口的请求分发至服务节点的 8080 端口。
* 权重：后端服务节点的权重，范围为 1~100 。数字越大即代表权重越高，负载均衡会优先将请求分发至权重较高的服务节点，默认值为 1 。

> 支持添加同一个虚拟机的多个端口到 VServer 的服务节点，即将 VServer 监听器端口的请求分别转发至同一个服务节点的多个端口上，满足不同应用场景的负载分发需求。

添加服务节点后，可在服务节点资源列表页面查看添加服务节点过程，待服务节点的状态为“有效”或“无效”时，即代表添加服务节点成功。若服务节点状态为无效，则需要检测服务节点中业务的运行状态，服务状态“有效”的前提是通过虚拟机的网络地址及健康检查方式可正常访问业务。

**负载均衡服务的服务模式为 NAT 请求代理模式，若添加虚拟机至提供外网的负载均衡后端，无需在后端服务节点上配置环回服务地址即可通过外网直接访问至服务节点。**

### 11.4.2 查看服务节点

通过 VServer 详情页面的"服务节点"标签页，可查看 VServer 监听器后端已添加的服务节点资源列表信息，包括服务节点 ID、资源ID、内网 IP、端口、权重、节点模式、节点状态及操作项，如下图所示：

![rslist](../images/userguide/rslist.png)

- 服务节点：当前服务节点的全局 RS 唯一标识符。
- 资源ID：当前服务节点已绑定的虚拟机名称和 ID。
- IP/端口：当前服务节点的内网 IP 地址及配置的服务端口。
- 权重：当前服务节点配置的转发权重。
- 节点模式：当前服务节点的启用和禁用模式。
- 状态：当前服务节点的业务负载状态，包括有效、无效。
  - 有效：指当前服务节点中的业务服务正常运行且可通过网络进行访问，即服务节点为健康；
  - 无效：指当前服务节点中的业务服务未正常运行或无法通过网络进行访问，即代表服务节点不不健康。

列表上操作项是指对单个服务节点的操作，包括启用、禁用、删除及修改等，支持服务节点的批量启用、批量禁用及批量删除操作。

### 11.4.3 启用/禁用

用户对添加至负载均衡 VServer 的服务节点进行启用和禁用操作，支持批量启用和禁用。

* 禁用：禁用服务节点，禁用后负载均衡将停止向该服务节点分发请求，并停止对其健康检查；
* 启用：启用服务节点，启用后负载均衡将对其进行健康检查，若健康检查通过则分根据调度算法，分发新的请求至该服务节点；
* 仅当节点模式为启用时才可进行禁用操作；
* 仅当节点模式为禁用时，才可进行启用操作。

### 11.4.4 修改服务节点

用户可对负载均衡 VServer 服务节点的服务端口及权重进行修改，如下图所示：

![updaters](../images/userguide/updaters.png)

修改端口和权重不会影响已建立的业务连接，仅对负载均衡新分发请求生效。点击确定后，即返回至服务节点列表页面，节点状态由 “有效” 或 “无效” 流转为“更新中” ，待修改成功后，重新流转回“有效”或“无效” ，有效则代表健康检查成功，服务节点可正常提供服务。

### 11.4.5 删除服务节点

如需对一个服务节点的业务进行变更或从负载均衡后端服务节点下线，可通过删除服务节点功能进行下线操作，下线后不影响虚拟机本身的运行和使用。用户可通过服务节点列表操作项中的“删除”进行服务节点的删除，删除后可重新添加至负载均衡实例。

![rmrs](../images/userguide/rmrs.png)

> 若负载均衡 VServer 的监听协议为 HTTP/HTTPS 且已配置内容转发规则 ，则删除服务节点时，会自动解绑内容转发规则。

## 11.5 内容转发规则管理

平台支持为 HTTP 监听器添加转发规则，支持为域名+ URL 路径的请求分发至不同的服务节点，满足精准负载分发业务需求。仅当负载均衡的 VServer 监听协议为 HTTP 或 HTTPS 时，才可进行内容转发规则的配置，包括内容转发规则的添加、查看、修改及删除。

### 11.5.1 添加内容转发规则

用户可通过 VServer 详情页面进入“**内容转发**”资源标签页，点击“**添加内容转发**”进行内容转发规则的添加。平台会自动生成一条默认内容转发规则，即代表所有请求默认转发至所有已添加的服务节点。

内容转发规则中的服务节点，仅可从当前 VServer 已存在的服务节点中进行选择，支持为一个域名添加多个 URL 路径，如下图所示：

![createpolicy](../images/userguide/createpolicy.png)

* 域名：内容转发规则匹配的域名，代表请求该域名时及 URL 时，将请求转发至 URL 配置的服务节点。
  * 域名值可以为空，代表无域名请求，仅匹配路径，即通过 IP 地址+ URL 路径的方式；
  * 支持泛域名，如 `*.test.com` 或 `*abc.test.com`；
* URL 路径：内容转发规则匹配的 URL 路径，URL 必须属于一个域名；
  * URL 长度限制为 1~30 个字母、数字和` -/.%?#&`这些字符， 且必须以 `/` 开头 ；
  * URL 可以为 `/` ，代表请求该域名的根目录时，转发请求至匹配的服务节点；
* 服务节点：当前内容转发规则所对应的服务节点，即当请求匹配 域名+路径 时，将请求转发发配置的服务节点，转发规则中的服务节点必须为 VServer 中已添加的服务节点。

点击确定后，返回内容转发规则的列表，可查看创建内容转发规则的过程，待添加的内容转发规则状态由 “创建中” 流转为 “有效” 时，即代表创建成功。

### 11.5.2 查看内容转发规则

通过 VServer 详情页面的"内容转发"标签进入内容转发规则管理控制台，可查看当前 VServer 监听器已添加的内容转发规则列表，同时可对内容转发规则进行添加、修改及删除操作。

内容转发规则列表页面可查看当前 VServer 已添加的内容转发规则信息，包括域名、URL 路径、转发节点、节点数量、规则状态及操作项，如下图所示：

![policylist](../images/userguide/policylist.png)

* 域名：内容转发规则匹配的域名，代表请求该域名时及 URL 时，将请求转发至 URL 配置的服务节点。
* URL 路径：内容转发规则匹配的 URL 路径，URL 必须属于一个域名。
* 转发节点：匹配当前内容转发规则时，请求分发的服务节点。
* 节点数量：当前转发规则已添加的服务节点数量。
* 状态：当前转发规则的状态，包括创建中、有效和删除中。

列表上操作项是指对域名或单条转发规则的修改及删除操作。点击域名右侧的修改和删除，即修改和删除整个域名及包括的所有转发规则；点击单条 URL 规则的修改和删除，即仅对单条规则进行删除和修改操作。

默认转发规则仅支持查看和修改，不支持删除。默认转发规则的节点数量即 VServer 中所包含的所有服务节点数量，可修改默认转发规则中的服务节点数量，以匹配精准转发策略。

### 11.5.3 修改内容转发规则

用户可对一个域名或所包含的 URL 规则进行修改，包括域名、 URL 路径、转发的服务节点，如下图所示：

![updatepolicy](../images/userguide/updatepolicy.png)

修改内容转发规则仅对新负载分发请求生效，不影响已建立并在处理的业务请求。点击确定后，即返回至内容转发规则列表页面，内容转发规则由 “有效” 流转为 “更新中” ，待修改成功后，重新流转回“有效” ，则代表新的匹配规则请求会直接分发到规则所配置的服务节点。

### 11.5.4 删除内容转发规则

用户可通过控制台或 API 的方式删除不需要的内容转发规则，删除内容转发规则会自动解绑已关联的后端服务节点。内容转发规则被删后，即即直接销毁，在删除前需确保负载均衡转发规则无业务流量的负载请求，否则可能影响业务的正常访问。如下图所示，删除域名时即直接删除该域名下所包括的所有 URL 规则信息：

![rmpolicy.png](../images/userguide/rmpolicy.png)

## 11.6 SSL 证书管理

负载均衡支持 HTTPS 负载转发及 SSL 证书装载能力，确保用户业务受到加密保护并得到权威机构的身份认证。针对 HTTPS 协议的服务器证书和客户端证书，平台提供统一的证书管理服务，包括证书的上传、绑定、删除操作。

证书无需上传到服务节点，解密处理在负载均衡上进行，降低后端服务器的CPU开销，即 HTTPS 协议的监听器仅实现客户端至负载均衡器的 HTTPS 请求和 SSL 加解密，负载均衡至后端服务节点依然采用 HTTP 协议转发请求。

在上传和创建证书前需确认需要上传的证书类型，包括服务器证书和客户端证书，并按照证书格式要求上传或输入证书内容至平台。

* 服务器证书：用户证明服务器的身份，HTTPS 检查服务器发送的证书是否是由自己信赖的中心签发。部署并配置于负载均衡服务器中，为负载均衡后端服务节点的网站提供 SSL 服务器证书及验证。单向认证和双向认证均需要上传服务器证书和私钥内容。

* 客户端证书：客户端CA公钥证书用于验证客户端证书的签发者，HTTPS 双向认证中需验证客户端提供的证书，才可成功建立连接。网站服务器用 CA 证书验证客户端证书的签名，如果没有通过验证，则拒绝连接。仅在双向认证时需要上传客户端证书并绑定到 VServer 监听器。

证书具有地址（数据中心）属性，仅支持关联相同数据中心的负载均衡资源，若一个证书需要在多个数据中心同时使用，需要在多个数据中心同时创建并上传证书。

### 11.6.1 证书格式要求

负载均衡 SSL 证书支持用户上传 `.crt`和`.pem` 格式的证书文件，当 SSL 证书被 VServer 监听器关联时，平台会自动读取文件中的证书内容并装载至负载均衡 VServer 监听器中，使用户 HTTPS 应用通过 SSL 证书进行加解密。

证书文件格式支持 Linux 环境下 PEM 或 CRT ，不支持其他格式的证书，需进行证书格式转换才可上传。用户也可通过直接输入证书内容创建证书，在上传证书或输入证书内容前，需确保证书、证书链及私钥内容符合证书的格式要求。

#### 11.6.1.1 Root CA 机构颁发的证书

若证书是 Root CA 机构颁发的唯一证书，则无需额外的证书，配置的站点即可被浏览器等访问设备认为可信。证书内容格式要求如下：

* 以`-----BEGIN CERTIFICATE-----` 开头，以` -----END CERTIFICATE-----` 结尾。
* 每行 64 个字符，最后一行长度可以不足 64 个字符。
* 证书内容不能包含空格。

Root CA 机构颁发的证书格式规范如下，可参考以下文本内容和证书示例：

```
-----BEGIN CERTIFICATE-----
用户证书(BASE64编码)
-----END CERTIFICATE-----
```

```
-----BEGIN CERTIFICATE-----
MIIFnDCCBISgAwIBAgIQD1pxAmxfzY+R4k4Ua1cVWDANBgkqhkiG9w0BAQsFADBy
MQswCQYDVQQGEwJDTjElMCMGA1UEChMcVHJ1c3RBc2lhIFRlY2hub2xvZ2llcywg
SW5jLjEdMBsGA1UECxMURG9tYWluIFZhbGlkYXRlZCBTU0wxHTAbBgNVBAMTFFRy
dXN0QXNpYSBUTFMgUlNBIENBMB4XDTE5MDQyMzAwMDAwMFoXDTIwMDQyMjEyMDAw
MFowHDEaMBgGA1UEAwwRKi51Y2xvdWRzdGFjay5jb20wggEiMA0GCSqGSIb3DQEB
AQUAA4IBDwAwggEKAoIBAQC2SDT5pJEQhRhQQ98vzuAvK1zUFMD1p1E3YyJGDISY
SINH38QTtVqWbZgmVkU6v2R1GrBz0iMfevO0/sjxefwHmiGYd1ytG9dm8D3fVZox
piST9hoIyjOFRstBLGXuxWSa2LdjVSePaFfxaN3UZLYY6MIHkdqxVZLhM4ANSLNr
PI6cRUZVBU29V3A2znkVEbx5dwKA3SGFVWfqjfzXqC+NTylLKb7H304BxspZlKDi
n+/aV/vSovVM7zg57AOtxjkSNzBDjdz+Ud3wqaT1O4vEG4tqqAnsIyJeaMueFti0
cjiMwLVsFsmV1eVSBiYWGO8U/YRFv+dNg4XG2MqYUFsRAgMBAAGjggKCMIICfjAf
BgNVHSMEGDAWgBR/05nzoEcOMQBWViKOt8ye3coBijAdBgNVHQ4EFgQUJYEWLiyn
YgqKaGaT8thKWAnuWfEwLQYDVR0RBCYwJIIRKi51Y2xvdWRzdGFjay5jb22CD3Vj
bG91ZHN0YWNrLmNvbTAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUH
AwEGCCsGAQUFBwMCMEwGA1UdIARFMEMwNwYJYIZIAYb9bAECMCowKAYIKwYBBQUH
AgEWHGh0dHBzOi8vd3d3LmRpZ2ljZXJ0LmNvbS9DUFMwCAYGZ4EMAQIBMH0GCCsG
AQUFBwEBBHEwbzAhBggrBgEFBQcwAYYVaHR0cDovL29jc3AuZGNvY3NwLmNuMEoG
CCsGAQUFBzAChj5odHRwOi8vY2FjZXJ0cy5kaWdpdGFsY2VydHZhbGlkYXRpb24u
Y29tL1RydXN0QXNpYVRMU1JTQUNBLmNydDAJBgNVHRMEAjAAMIIBBAYKKwYBBAHW
eQIEAgSB9QSB8gDwAHYA7ku9t3XOYLrhQmkfq+GeZqMPfl+wctiDAMR7iXqo/csA
AAFqSaRkyQAABAMARzBFAiEAuovTHM3SEWQRyktGXvtm1hLHd7gxhPNzdzrzkJFX
rWMCIAPideB1BqUSUcpRME6NxIXJD7066ldWuSqgPkOiPtwLAHYAh3W/51l8+IxD
mV+9827/Vo1HVjb/SrVgwbTq/16ggw8AAAFqSaRl4wAABAMARzBFAiBiIpO59m6U
bmlmuQ8cL7WzoDkHiyE+UloEKZXiDpqCfQIhAPIKRdaJfh/5IZHFq31oJVd/TZ3g
pTQ6RpHe0BseSSefMA0GCSqGSIb3DQEBCwUAA4IBAQARaNWOJbAI7Rv6QPChPeWL
Mqryk+tOlterdxYZay6tr3Ea8VOqSS7YdVtvdkR1/k4k87H5AwCQT60/yu4N5J7M
Vkzmqo3tVQTtzVFo0SavgARY12XuU0jhG3LGFI0a43CgfMYMcZ0DiylhYUM48GWz
/axza5uangnIQxBwv+4KXGUfplJujv8WfBepeh+tqPgS8qCqn6e0+sdkUN7yHcA/
O24DiQajtMXG5nR6qHdZhRLCFRXRghYdvVKrkOVFogYqwa4dViyuP/6EFDkuMwDs
7XrxJIjL8qp9Lrw2sHN1F+USKhlPRaNBtWzDELf54zVgAIAeFUriqtER8ZWBWgp4
-----END CERTIFICATE-----
```

#### 11.6.1.2 中级机构颁发的证书

若证书是通过中级 CA 机构颁发的证书，则拿到的证书文件包含多份证书，需要人为将服务器证书与中间证书合并在一起填写或上传，俗称证书链。

证书链的拼接规则为：用户证书放第一份，中间证书放第二份，中间不可有空行；每行 64 个字符且证书内容不能包含空格，最后一行长度可以不足 64 个字符，格式规范及证书示例如下所示：

```
-----BEGIN CERTIFICATE-----
用户证书(BASE64编码)
-----END CERTIFICATE-----
!!!中间不可有空行!!!
-----BEGIN CERTIFICATE-----
中级签发机构证书(BASE64编码)
-----END CERTIFICATE-----
```

```
-----BEGIN CERTIFICATE-----
MIIFnDCCBISgAwIBAgIQD1pxAmxfzY+R4k4Ua1cVWDANBgkqhkiG9w0BAQsFADBy
MQswCQYDVQQGEwJDTjElMCMGA1UEChMcVHJ1c3RBc2lhIFRlY2hub2xvZ2llcywg
SW5jLjEdMBsGA1UECxMURG9tYWluIFZhbGlkYXRlZCBTU0wxHTAbBgNVBAMTFFRy
dXN0QXNpYSBUTFMgUlNBIENBMB4XDTE5MDQyMzAwMDAwMFoXDTIwMDQyMjEyMDAw
MFowHDEaMBgGA1UEAwwRKi51Y2xvdWRzdGFjay5jb20wggEiMA0GCSqGSIb3DQEB
AQUAA4IBDwAwggEKAoIBAQC2SDT5pJEQhRhQQ98vzuAvK1zUFMD1p1E3YyJGDISY
SINH38QTtVqWbZgmVkU6v2R1GrBz0iMfevO0/sjxefwHmiGYd1ytG9dm8D3fVZox
piST9hoIyjOFRstBLGXuxWSa2LdjVSePaFfxaN3UZLYY6MIHkdqxVZLhM4ANSLNr
PI6cRUZVBU29V3A2znkVEbx5dwKA3SGFVWfqjfzXqC+NTylLKb7H304BxspZlKDi
n+/aV/vSovVM7zg57AOtxjkSNzBDjdz+Ud3wqaT1O4vEG4tqqAnsIyJeaMueFti0
cjiMwLVsFsmV1eVSBiYWGO8U/YRFv+dNg4XG2MqYUFsRAgMBAAGjggKCMIICfjAf
BgNVHSMEGDAWgBR/05nzoEcOMQBWViKOt8ye3coBijAdBgNVHQ4EFgQUJYEWLiyn
YgqKaGaT8thKWAnuWfEwLQYDVR0RBCYwJIIRKi51Y2xvdWRzdGFjay5jb22CD3Vj
bG91ZHN0YWNrLmNvbTAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUH
AwEGCCsGAQUFBwMCMEwGA1UdIARFMEMwNwYJYIZIAYb9bAECMCowKAYIKwYBBQUH
AgEWHGh0dHBzOi8vd3d3LmRpZ2ljZXJ0LmNvbS9DUFMwCAYGZ4EMAQIBMH0GCCsG
AQUFBwEBBHEwbzAhBggrBgEFBQcwAYYVaHR0cDovL29jc3AuZGNvY3NwLmNuMEoG
CCsGAQUFBzAChj5odHRwOi8vY2FjZXJ0cy5kaWdpdGFsY2VydHZhbGlkYXRpb24u
Y29tL1RydXN0QXNpYVRMU1JTQUNBLmNydDAJBgNVHRMEAjAAMIIBBAYKKwYBBAHW
eQIEAgSB9QSB8gDwAHYA7ku9t3XOYLrhQmkfq+GeZqMPfl+wctiDAMR7iXqo/csA
AAFqSaRkyQAABAMARzBFAiEAuovTHM3SEWQRyktGXvtm1hLHd7gxhPNzdzrzkJFX
rWMCIAPideB1BqUSUcpRME6NxIXJD7066ldWuSqgPkOiPtwLAHYAh3W/51l8+IxD
mV+9827/Vo1HVjb/SrVgwbTq/16ggw8AAAFqSaRl4wAABAMARzBFAiBiIpO59m6U
bmlmuQ8cL7WzoDkHiyE+UloEKZXiDpqCfQIhAPIKRdaJfh/5IZHFq31oJVd/TZ3g
pTQ6RpHe0BseSSefMA0GCSqGSIb3DQEBCwUAA4IBAQARaNWOJbAI7Rv6QPChPeWL
Mqryk+tOlterdxYZay6tr3Ea8VOqSS7YdVtvdkR1/k4k87H5AwCQT60/yu4N5J7M
Vkzmqo3tVQTtzVFo0SavgARY12XuU0jhG3LGFI0a43CgfMYMcZ0DiylhYUM48GWz
/axza5uangnIQxBwv+4KXGUfplJujv8WfBepeh+tqPgS8qCqn6e0+sdkUN7yHcA/
O24DiQajtMXG5nR6qHdZhRLCFRXRghYdvVKrkOVFogYqwa4dViyuP/6EFDkuMwDs
7XrxJIjL8qp9Lrw2sHN1F+USKhlPRaNBtWzDELf54zVgAIAeFUriqtER8ZWBWgp4
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIErjCCA5agAwIBAgIQBYAmfwbylVM0jhwYWl7uLjANBgkqhkiG9w0BAQsFADBh
MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3
d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBD
QTAeFw0xNzEyMDgxMjI4MjZaFw0yNzEyMDgxMjI4MjZaMHIxCzAJBgNVBAYTAkNO
MSUwIwYDVQQKExxUcnVzdEFzaWEgVGVjaG5vbG9naWVzLCBJbmMuMR0wGwYDVQQL
ExREb21haW4gVmFsaWRhdGVkIFNTTDEdMBsGA1UEAxMUVHJ1c3RBc2lhIFRMUyBS
U0EgQ0EwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCgWa9X+ph+wAm8
Yh1Fk1MjKbQ5QwBOOKVaZR/OfCh+F6f93u7vZHGcUU/lvVGgUQnbzJhR1UV2epJa
e+m7cxnXIKdD0/VS9btAgwJszGFvwoqXeaCqFoP71wPmXjjUwLT70+qvX4hdyYfO
JcjeTz5QKtg8zQwxaK9x4JT9CoOmoVdVhEBAiD3DwR5fFgOHDwwGxdJWVBvktnoA
zjdTLXDdbSVC5jZ0u8oq9BiTDv7jAlsB5F8aZgvSZDOQeFrwaOTbKWSEInEhnchK
ZTD1dz6aBlk1xGEI5PZWAnVAba/ofH33ktymaTDsE6xRDnW97pDkimCRak6CEbfe
3dXw6OV5AgMBAAGjggFPMIIBSzAdBgNVHQ4EFgQUf9OZ86BHDjEAVlYijrfMnt3K
AYowHwYDVR0jBBgwFoAUA95QNVbRTLtm8KPiGxvDl7I90VUwDgYDVR0PAQH/BAQD
AgGGMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjASBgNVHRMBAf8ECDAG
AQH/AgEAMDQGCCsGAQUFBwEBBCgwJjAkBggrBgEFBQcwAYYYaHR0cDovL29jc3Au
ZGlnaWNlcnQuY29tMEIGA1UdHwQ7MDkwN6A1oDOGMWh0dHA6Ly9jcmwzLmRpZ2lj
ZXJ0LmNvbS9EaWdpQ2VydEdsb2JhbFJvb3RDQS5jcmwwTAYDVR0gBEUwQzA3Bglg
hkgBhv1sAQIwKjAoBggrBgEFBQcCARYcaHR0cHM6Ly93d3cuZGlnaWNlcnQuY29t
L0NQUzAIBgZngQwBAgEwDQYJKoZIhvcNAQELBQADggEBAK3dVOj5dlv4MzK2i233
lDYvyJ3slFY2X2HKTYGte8nbK6i5/fsDImMYihAkp6VaNY/en8WZ5qcrQPVLuJrJ
DSXT04NnMeZOQDUoj/NHAmdfCBB/h1bZ5OGK6Sf1h5Yx/5wR4f3TUoPgGlnU7EuP
ISLNdMRiDrXntcImDAiRvkh5GJuH4YCVE6XEntqaNIgGkRwxKSgnU3Id3iuFbW9F
UQ9Qqtb1GX91AJ7i4153TikGgYCdwYkBURD8gSVe8OAco6IfZOYt/TEwii1Ivi1C
qnuUlWpsF1LdQNIdfbW3TSe0BhQa7ifbVIfvPWHYOu3rkg1ZeMo6XRU9B4n5VyJY
RmE=
-----END CERTIFICATE-----
```

#### 11.6.1.3 RSA 私钥

在上传服务器证书时，需要用户同时上传证书的私钥内容。

* 以 `-----BEGIN RSA PRIVATE KEY-----` 开头，以` -----END RSA PRIVATE KEY-----` 结尾。
* 每行 64 个字符，最后一行长度可以不足 64 个字符。
* 证书内容不能包含空格。

证书 RSA 私钥内容的格式规范如下，可参考以下文本内容和证书示例：

```
-----BEGIN RSA PRIVATE KEY-----
证书私钥(BASE64编码)
-----END RSA PRIVATE KEY-----
```

```
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAtkg0+aSREIUYUEPfL87gLytc1BTA9adRN2MiRgyEmEiDR9/E
E7Valm2YJlZFOr9kdRqwc9IjH3rztP7I8Xn8B5ohmHdcrRvXZvA931WaMaYkk/Ya
CMozhUbLQSxl7sVkmti3Y1Unj2hX8Wjd1GS2GOjCB5HasVWS4TOADUizazyOnEVG
VQVNvVdwNs55FRG8eXcCgN0hhVVn6o3816gvjU8pSym+x99OAcbKWZSg4p/v2lf7
0qL1TO84OewDrcY5EjcwQ43c/lHd8Kmk9TuLxBuLaqgJ7CMiXmjLnhbYtHI4jMC1
bBbJldXlUgYmFhjvFP2ERb/nTYOFxtjKmFBbEQIDAQABAoIBAAbLqdftu+/A+n9R
jHJIlOCFThRlAq2V04gMVNSGNnJD78r/61wtvGcTxmKVgEa4qGraOB5VRPRxPcEv
Z3fjK5Nv+lUoDAczHMRsa+4Vz6YOstnmSKGvwhxzn3O6T0GHz+Ca+DlGjS9CPVcV
aQG4UHacxNEJ7byDO3LUW++C2JeEjg2LVLJt+jRwFIAwmk8XjM/jyOn5kCj2kvz6
w/yHbmwAac2mfA42CQN78o1bvEHlHH1cVDRHZ482pNlfp8WBGgCFWHBGeMpLOq9R
YXEt+SyJ84o80mTeR2DgswpW577uZLGfPyFUwKPc/XGZSzbfKX5L9ctGQrr4lsQ+
F2stOk0CgYEA5JyQ5S7qPf8YcczQfXblVueslDL56Zt1/KUdUQ4L9i2as+5LkiN+
7gVK8INhvYi8dRAqZxqXLqU5dXEWbkcnFbenjoQyPCfYI5lZE2+cZriLtO3I0YKP
nc9NwSE+gRR9kXbgGSiANJmGC5TxOU99hR7Nx6wUMAflatCaSmP5RbUCgYEAzB67
MOWzeVD8Eq/DhEE5N0o3hpyhiMy++A/LC8lAjFAi6ldc70zMYMUjfh+eTNK89Z6H
1z3xBaQMMlHAy8pU5uI9LOgm/xWaPNZ034Xx4fWftBnEGFtBgLfbNphoUz3Df5vK
XvXhjdKEkwevcLoZWHfNZIJnennlEV26Tk1DGW0CgYBYCKaPasqPRy2NnRZoSiG0
npBJnXu5ZsE/ogGxFdyrVxJs2YXGZ97YH7ek+KLpzr7rwWbiv02ai8udmwfNPZ8i
cM+YRPXnTlygEMxJfMBYmhZKfQrJCyLs3UiO55NfN5nHK2TOq1b7amdBDID71c17
Nsp9apl3iYLh6CSSIv95xQKBgCHmKKhiPYA0VuizkADy5BGunbIZaSpS9pQz60C1
16Z12Jaak7CaTIb1toNHtP6FMSSJg33Xp6OMLwpcUWyG2brOb+J5W6CZcdgQtbA5
ioZASJmcfdidry81WY6jmQ/Z/hG/ScijhSYMhD/20sgh3/u1ScMbdRv+CnDr4/kF
E9OxAoGBAK0mCJyyKNkWSdOg7fbBwYJKQ1BBZ6lh1gVpc7recSreNPRFWTd6l+cw
eCxXqbSJJw4oYYF4IoBX1fCfFd82engRkwmkDykGMwpomnJoZqjFhmVtDb81xRQL
pscHorV4flpOcSwg6b3jq0N6+PN85XI9XIFImXXHJqqKSFBBSPrf
-----END RSA PRIVATE KEY-----
```

若 RSA 私钥内容已进行加密，如私钥以 ``-----BEGIN PRIVATE KEY-----` 或 `-----BEGIN ENCRYPTED PRIVATE KEY-----` 开头，以 `-----END PRIVATE KEY-----` 或 `-----END ENCRYPTED PRIVATE KEY-----` 结尾，需对证书私钥内容进行转换。在 Linux 系统中操作如下所示：

```
openssl rsa -in old_server_key.pem -out new_server_key.pem
```

#### 11.6.1.4 客户端证书

客户端证书格式要求和 Root CA 机构颁发的证书要求一致，以 `-----BEGIN CERTIFICATE-----` 开始，以 `-----END CERTIFICATE-----` 结尾， 每行64字符，最后一行可以不足64字符。如下规范所示：

```
-----BEGIN CERTIFICATE-----
客户端CA证书(BASE64编码)
-----END CERTIFICATE-----
```

### 11.6.2 创建 SSL 证书

用户上传 SSL 证书用于部署 HTTPS 协议的负载均衡业务，支持上传服务器证书和客户端证书。上传证书时需检测 SSL 证书的格式及有效性，若 SSL 证书内容不符合格式规范，则无法成功生成 SSL 证书。

#### 11.6.2.1 创建服务器证书

支持本地上传证书文件和手动输入证书内容两种方式创建服务器证书，用户可通过负载均衡控制台切换至 SSL 证书标签页，进入 SSL 证书管理控制台，通过创建 SSL 证书进入上传证书向导页面，选择服务器证书进行创建。

**（1）本地上传证书文件的方式创建如下图所示：**

![CreateServerSSL](../images/userguide/CreateServerSSL.png)

本地上传时需要上传用户证书文件和证书私钥文件，其中用户证书文件仅支持 `crt` 和 `pem` 格式的文件，证书私钥仅支持上传 `.key` 格式的文件。

* 用户证书：用户的授权证书内容，包括公钥和签名等信息，支持证书链，一般为 `.crt` 和 `.pem` 格式的文件。
* 证书私钥：加密证书的私钥内容，一般为 `.key` 格式的文件。

点击上传证书，即可将本地已生成的证书文件读取到平台，并通过确认进行证书创建，创建过程中，平台会检测证书内容的合法性。

**（2）手动输入证书内容创建如下图所示：**

![CreateServerSSL1.png](../images/userguide/CreateServerSSL1.png.png)

手动输入证书同样需要输入用户证书和证书私钥的文本内容，需参考文本框中的格式规范输入证书内容和私钥内容，通过确认进行证书创建，创建过程中，平台会检测证书内容的合法性。

#### 11.6.2.2 创建客户端证书

支持本地上传证书文件和手动输入证书内容两种方式创建客户端证书，用户可通过负载均衡控制台切换至 SSL 证书标签页，进入 SSL 证书管理控制台，通过创建 SSL 证书进入上传证书向导页面，选择客户端证书进行创建。

**（1）本地上传证书文件的方式创建如下图所示：**

![CreateCASSL](../images/userguide/CreateCASSL.png)

本地上传时需要上传客户端 CA 公钥证书文件，仅支持 `crt` 和 `pem` 格式的文件。点击上传证书，即可将本地已生成的证书文件读取到平台，并通过确认进行证书创建，创建过程中，平台会检测证书内容的合法性。

**（2）手动输入证书内容创建如下图所示：**

![CreateCASSL1](../images/userguide/CreateCASSL1.png)

手动输入证书同样需要输入 CA 客户端证书的文本内容，需参考文本框中的格式规范输入证书内容和私钥内容，通过确认进行证书创建，创建过程中，平台会检测证书内容的合法性。

### 11.6.3 查看 SSL 证书

通过导航栏进入负载均衡控制台，切换至 SSL 证书标签页可查看 SSL 证书的资源列表，并可通过列表上名称和 ID 进入详情页面查看证书的详细信息及已绑定资源信息。

#### 11.6.3.1 SSL 证书列表

SSL 证书列表可查看当前账户下所有 SSL 证书的资源列表信息，包括名称、资源 ID、证书类型、域名、MD5 值、创建时间、证书过期时间及操作项，如下图所示：

![SSLList](../images/userguide/SSLList.png)

* 名称/ID ：SSL 证书的名称及全局唯一标识符。
* 证书类型：SSL 证书的类型，包括服务器证书和和客户端证书，仅在 VServer 监听器的 SSL 解析模式为双向认证时才需上传客户端证书。
* 域名：SSL 证书的主域名和备用域名信息，分别对应证书的 CommonName 和 Subject Alternative Name 字段信息，代表该证书可进行加解密服务的域名。
* MD5 值：SSL 证书的 MD5 较验值，用于验证 SSL 证书的准确性。
* 创建时间：SSL 证书在平台的创建时间。
* 证书过期时间：SSL 证书本身的过期时间，若证书已过期，需要重新为域名申请或制作新的证书，否则 HTTPS 协议访问会被认为不可信，即 HTTPS 失效。

列表上的操作项是指对单个证书的操作，包括查看证书和删除证书，可通过搜索框对证书列表进行搜索和筛选，支持模糊搜索。为方便租户对资源的维护，同时支持对 SSL 证书进行批量删除操作。

#### 11.6.3.2 SSL 证书详情

在证书资源列表上，点击名称或 ID 可进入概览页面查看当前证书的详信细信及已绑定的资源信息，如概览所示：

![sslinfos](../images/userguide/sslinfos.png)

**（1）基本信息**

SSL 证书的基本信息，包括资源 ID、名称、证书类型、主域名、备域名、MD5 值、证书内容、创建时间及证书的过期时间。用户可通过资源列表上或基本信息中的【查看证书】按钮查询当前证书的内容，如下图所示：

![sslinfo](../images/userguide/sslinfo.png)

> 若证书已过期，可通过点击已过期查看证书内容本身的具体过期时间。

**（2）关联资源**

通过已关联资源信息列表，可查看当前 SSL 证书已被关联的负载均衡及 VServer 信息，包括负载均衡名称、LBID、VServerID、VServer 的协议端口，如概览页所示，证书已关联 1 个 资源（HTTPS:443）。

#### 11.6.4 删除 SSL 证书

用户删除 SSL 证书，仅允许删除未被使用的的 SSL 证书。若一个 SSL 证书已关联一个负载均衡的 VServer ，则不可进行删除。

![sslrm](../images/userguide/sslrm.png)

证书被删除后即在平台彻底销毁，不可进行恢复，可重新进行上传并关联负载均衡进行使用。
