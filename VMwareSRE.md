# VMware 产品的中蕴含的SRE思想
*** 

经过SRE Foundation 课程的学习,我发现如果要精读他的思想两天时间是远远不够的,有同事和我说,"SRE的出现其实是运维与研发两个团队的权利斗争,在Google,是运维胜利了,因为在Google,功能不重要，随时可改掉。但大规模用户带来的性能压力才是重要的。" 我对他的这个观点并不完全认同,至少从技术的角度看,**SRE思想的出发点并不是由于权利的斗争,而是生产技术的向前发展所必然经历的过程,它是技术向前发展的必然结果,如果你的公司或者决策层还没有意识到,说明你的组织应该不是向前发展的.**
如果您的组织是向前发展的,那很多SRE的思想相信您在读了之后肯定会产生共鸣.

如本篇文章题目,我主要是想聊聊VMware产品中蕴含的SRE思想;我们知道减少琐事,让SRE有时间去做更多系统优化,自动化,让明天更加美好的事情是SRE的核心思想之一,很多技术的出现也是本着减少运维琐事的这个原则去推出自己的产品以及解决方案;例如k8s的出现是为了编排容器,是我们更快的部署业务在容器之上,而不必单节点的去一个个配置容器环境;k8s的滚动更新也减少了手动更新节点的琐事;回顾自己公司VMware的产品以及功能,很多产品都带有这种SRE的思想.下面就让我们一起过一下这些带有SRE精神的产品以及功能.

## 计算
### vCenter Server
这应该是最不用介绍的产品,用过VMware 产品的都应该很熟悉了,vCenter Server用来管理所有的主机以及资源,统一化管理,并且下面的很多功能也是直接在vCenter Server上操作的.

### vSphere HA 高可用
VMware vSphere High Availability提供了虚拟机中运行的大多数应用程序所需的可用性，而与操作系统和其中运行的应用程序无关。 高可用性为虚拟IT环境中的硬件和操作系统中断提供了统一，经济高效的故障转移保护。 高可用性使您能够：

监视VMware vSphere主机和虚拟机以检测硬件和来宾操作系统故障。
在检测到服务器故障时，无需手动干预即可在群集中其他vSphere主机上重新启动虚拟机。

vSphere HA的出现减少了在主机故障之后,重启虚拟机的工作,vSphere HA会自动保护这些业务虚拟机的连续性.直接提高了平均恢复时间MTTR,MTRS和SLO标准.

### vSphere DRS 
将VMware ESXi主机分组为资源集群，以分离不同业务部门的计算需求。 启用了DRS的VMware vSphere群集使您能够：
     为您的工作负载提供高度可用的资源。
     平衡工作负载以获得最佳性能。
     在不中断服务的情况下扩展和管理计算资源。
减少了资源池中计算资源不平衡的问题,如果一台Esxi主机上虚拟机数量过多,那主机故障的影响面也是扩大了,所以这也是间接的减少影响面,减少不必要的错误预算花费.间接提高了SLO标准.

### 分布式电源管理
需要配合DRS工作,开启分布式电源管理使得集群在满足HA的条件下能自动挂起负载不高的主机,节省电力使用,整个过程可以做到无人干预,这也为企业节省了电能消耗.

### Storage DRS 存储动态资源规划
通过 Storage DRS 可以管理数据存储集群的聚合资源。启用 Storage DRS 后，它会对虚拟机磁盘放置和迁移提供建议，以平衡数据存储集群内所有数据存储之间的空间和 I/O 资源。
这也是自动化的一部分,减少人工干预,在存储IO不足的时候系统通过DRS算法自动平衡业务虚拟机的磁盘存储位置. 

### vSphere Update Manager
Update Manager 可让 VMware vSphere 执行集中式自动修补程序和版本管理，并提供对 VMware ESXi 主机和虚拟机的支持。

使用 Update Manager，您可以执行以下任务：
* 升级和修补 ESXi 主机。
* 在主机上安装和更新第三方软件。
* 升级虚拟机硬件和 VMware Tools。

自动化给环境应用修复补丁,驱动更新以及版本升级.这也大大减少了运维人员的重复性劳动.

### vSphere 分布式交换机
VMware vSphere Distributed Switch（VDS）提供了一个集中式界面，您可以从中配置，监视和管理整个数据中心的虚拟机访问切换。 VDS提供：
* 简化的虚拟机网络配置
* 增强的网络监控和故障排除功能
* 支持高级VMware vSphere网络功能

使用以下VDS功能来简化跨多个主机的虚拟网络的置备，管理和监视：
* 集中控制虚拟交换机端口配置，端口组命名，过滤器和其他设置
* 支持链路聚合控制协议（LACP），以协商并自动配置vSphere主机与访问层* 交换机之间的链路聚合
* 网络运行状况检查功能，可以验证vSphere对物理网络的配置

对比标准交换机,运维人员需要每台主机去配置虚拟机网络使用的端口组;而分布式交换机极大的简化虚拟机的网络配置,这节省了运维人员大量时间,这是最典型的减少琐事的例子.

### 主机配置文件和 vSphere Auto Deploy
主机配置文件可快速，轻松地用于初始配置新的ESXi主机，并确定配置参数没有偏差。 
通过vSphere Auto Deploy，您可以使用PXE技术在数据中心基础架构中快速配置大量ESXi主机.需要注意的是Auto Deploy置备的主机都是无状态的,操作系统跑在内存中.
通过这两个技术集中化安装主机,以及批量配置DNS,网络等信息.减少了很多重复劳动.

### 内容库
内容库添加了管理控制和版本控制支持.
为虚拟机模板，虚拟设备，ISO映像和脚本提供简单有效的集中管理.
配置一次虚拟机模板或者ISO,可以提供给多个环境同时使用.
也是减少琐事的一种.

## 网络
### NSX数据中心
VMware NSX®数据中心是网络虚拟化和安全平台，可启用虚拟云网络，这是一种软件定义的联网方法，可跨数据中心，云和应用程序框架扩展。借助NSX Data Center，无论从虚拟机（VM）到容器还是裸机，无论应用程序在哪里运行，网络和安全性都可以使其更接近该应用程序。像VM的操作模型一样，可以独立于基础硬件来配置和管理网络。 NSX Data Center可通过软件复制整个网络模型，从而使数以千计的网络拓扑（从简单到复杂的多层网络）都可以创建和配置。用户可以利用NSX或从第三方防火墙到性能管理解决方案的广泛第三方集成生态系统提供的服务组合，创建具有不同要求的多个虚拟网络，以构建本质上更为敏捷和安全的环境。然后，可以将这些服务扩展到云内或跨云的各种节点.
NSX 数据中心提供的功能有:
|功能|说明|蕴含的SRE思想|
|--|--|--|
|Distributed Switching and Routing|统一管理分布式交换机和路由|简化虚拟化环境网络管理,减少琐事|
|NSX Gateway Firewall (Stateful)|统一管理|简化管理,减少琐事|
|NSX Gateway NAT|统一管理|简化管理,减少琐事|
|Software L2 Bridging to Physical Environments|提供灵活性管理,和物理网络的连通|简化管理,减少复杂性,减少琐事|
|Dynamic Routing with ECMP (Active-Active)|等价多路径动态路由|提高冗余,提升系统健壮性,简化管理|
|Integration with Cloud Management Platforms|与云管平台集成|简化管理,提升自动化能力,提供可视化,告警,日子管理能力|
|IPv6 with Static Routing and Static IPv6 Allocation|IPV6静态路由以及置备|提升能力,简化管理|
|Distributed Firewalling for VMs and Workloads Running on Bare Metal|为虚拟机和物理环境的工作负载提供分布式防火墙|简化管理|
|VPN (L2 and L3)|2层和3层VPN|简化管理|
|Integration with NSX Cloud™4 for AWS and Azure Support|提供 AWS和Azure上的NSX云集成|提高灵活性,简化管理|
|Load Balancing|负载均衡|简化管理|
|Integration with Distributed Firewall (Active Directory, VMware AirWatch®, Endpoint Protection and Third- Party Service Insertion)|与分布式防火墙(活动目录,AirWatch,终端保护,第三方服务插入)集成|简化管理|
|Container Networking and Security|容器网络和安全|简化管理,提升系统健壮性|
|Multi-vCenter® Networking and Security|多vCenter的网络和安全|简化管理|
|IPv6 with Dynamic Routing, Dynamic IPv6 Allocation and Services|IPv6 动态路由和分配统一管理|简化管理|
|Context-Aware Micro-Segmentation (L7 Application Identification, RDSH, Protocol Analyzer)|上下文感知的微分段（L7应用程序标识，RDSH，协议分析）|简化管理|
|Distributed FQDN Whitelisting|分布式FQDN白名单||
|NSX Distributed IDS/IPS|分布式入侵检测和防御|简化管理|
|VRF (Tier 0 Gateway VRFs)|虚拟路由和转发|简化管理|
|Federation|联邦,您可以使用单一窗口视图管理多个 NSX-T Data Center 环境、创建跨一个或多个位置的网关和分段，并在各个位置中统一配置和强制执行防火墙规则|简化管理,减少琐事|
|NSX INTELLIGENCE|可视化|简化管理|
|VM-to-VM Traffic Flow Analysis|vm之间的网络流量分析|可视化|
|Firewall Visibility|防火墙可见化|可视化|
|Automated Security Policy|自动化安全策略|自动化|
|Rule and Group Recommendation Analytics|规则和组建议分析|自动化,简化管理|
|vRealize Network Insight Advanced|网络策略收敛,异常流量检测|可视化,自动化,告警|
|VMware HCX Advanced|多云之间互相迁移,平衡|提升灵活性,提高迁移性能,简化管理|

## 可视化
如果说可视化是SRE追逐的最终结果,那VMware这方面可以说还是做得不错的.
|产品名称|说明|蕴含的SRE思想|
|--|--|--|
|vRealize LifeCycle Management Suite|vmware产品的生命周期管理,例如统一的版本,补丁以及扩容和升级管理等|简化管理,减少琐事|
|vRealize Operations Manager|主要提供了环境的概览,成本计算,优化建议,告警以及自定义报表的功能;也支持目前主流的操作系统,云环境,容器环境,中间件,软硬件设备等|可视化,告警,可提供SLI各种指标的支持|
|vRealize Log Insight|主要提供了基于大数据和机器学习的日志管理,可以支持目前主流操作系统,各种产品以及中间件还有硬件设备的监控,可视化以及告警|可视化,告警,日志管理|
|vRealize Network Insight|网络流量可视化,防火墙规则收敛|可视化,网络安全|

## 自动化
### 自动化产品
|产品名称|说明|蕴含的SRE思想|
|--|--|--|
|vRealize Automation|vRealize Automation是一个现代化的基础架构自动化平台，可在VMware Cloud基础架构上启用私有和多云环境。 它提供自助服务自动化，用于基础架构的DevOps和网络自动化功能，可帮助您提高业务和IT敏捷性，生产力和效率。 通过vRealize Automation集成，简化和现代化传统的云原生和多云基础架构，并简化IT，同时为业务的未来做准备。|自动化,基础架构即代码,配置即代码,Devops|

### 自动化工具
|产品名称|说明|蕴含的SRE思想|
|--|--|--|
|PowerCLI|通过VMware PowerCLI，您可以通过命令行在vCenter Server，vRealize Operations Manager，vSphere Automation SDK，vSAN，vCloud Director，vSphere Update Manager，NSX-T和VMware Cloud上管理，监视，自动化和处理生命周期操作|自动化,基础架构即代码,配置即代码,Devops|
|govc|源自Vmware开源项目govmomi,比PowerCLI更加轻量级|自动化|

### 持续集成,持续交付
#### 代表产品vRealize Code Stream
vRealize Code Stream可帮助希望采用持续集成/持续交付方法来交付应用程序或IT代码的组织。它是一个应用程序发布自动化解决方案，使开发人员和IT团队可以更频繁，更高效地发布软件，同时利用他们在现有构建，测试，供应和监视工具上的投入。该解决方案与VMware vRealize Automation™和vRealize Orchestrator™紧密集成，旨在解决两个主要用例：
* 软件发布自动化
* IT制品生命周期管理

##### 软件发布自动化
vRealize Code Stream在软件交付管道的每个阶段自动执行发布过程，以确保速度和一致性。 它与现有软件开发，测试，工件管理和构建系统集成在一起，以协调在开发过程中需要执行的任务。

##### IT制品生命周期管理
IT制品生命周期管理vRealize Code Stream与针对IT DevOps的免费 vRealize代码流管理包的结合，可帮助IT团队管理其软件定义的数据中心（SDDC）的IT制品（或称基础架构为代码）。 vRealize Code Stream可以捕获，存储和与源代码和版本控制工具集成。 它还提供了在vRealize Automation的不同租户或VMware vRealize Automation或VMware vRealize Operations™的不同实例之间分发这些制品的功能。 这简化了大型，多实例或多租户vRealize实施的持续管理。


## 站点恢复,灾难复原
|产品名称|说明|蕴含的SRE思想|
|--|--|--|
|Site Recovery Manager|用于站点灾难复原|反脆弱性|
|vSphere Replication|虚拟机备份|反脆弱性|

## 混合云管理,超融合
|产品名称|说明|蕴含的SRE思想|
|--|--|--|
|VMware Cloud Foundation|VMware Cloud Foundation是基于全栈超融合基础架构（HCI）技术构建的用于管理VM和协调容器的混合云平台。 凭借易于部署的单一架构，VMware Cloud Foundation可以在私有云和公共云之间实现一致，安全的基础架构和运营。 交付所有功能的混合云可提高企业敏捷性和灵活性。|统一管理,自动化部署实施,减少day1所花的时间,标准化架构|

VCF 产品尤其需要重点去提的是,它简化了部署的时间,按照VMware的最佳实践在几个小时内就可以部署一整套VMware技术栈(vSphere,vSAN,NSX以及vRealize 套件);大大节省了在day1阶段所花费的时间.


如果您喜欢,请关注如下公众号.
![公众号](images\朋友圈.jpeg)

