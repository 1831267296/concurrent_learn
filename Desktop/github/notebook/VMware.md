# VMware vSphere 简介

## 产品作用

```
VMwarevSphere产品套件包括VMwareESXIvCenterServer.ESXi提供了基础虚拟化功能，并且支持虚拟SMP等特性,vCenterServer能够管理，还包括其他一些功能，如vMotionStoragevMotion、vSphere分布式资源调度器（DRS）、vSphere高可用性(HA)和vSphere容错(FT)。存储I/O控制(SIOC)和网络1/0控制(NetlOC)支持细致的虚拟机资源控制。用于数据保护的vsphere存储API(VADP)提供了一种备份框架，可用于将第三方备份解决方案整合到vSphere实现中。
```

### VMware EsXi-虚机管理程序

能够在 不使用服务控制台的前提下支持VMware ESX的诸多原有虚拟化功 能.。ESXi的安装和运行并不需要基于Linux的服务控制台。这使得ESXi体积超小,仅有130MB

### Vcenter

 vCenterServer的配置与管理功能一一淇中包括虚拟机模板、虚拟机定制、虚拟机快速分 配与部署、基于角色的访问控制和精细资源分配控制等特性，vCenterServer还提供了一些包含更 高级特性的工具，如vSpherevMotion、vsphere分布式资源调度器(DRS)、vsphere高可用性(HA) 和vSphere容错(FT)O本章将概括介绍所有这些特性，而在后续章节将会对其进行更深人的介绍。

### vsphere Update Manager

功能：

```
通过扫描发现不兼容最新更新的系统；
通过用户自定义规则发现过期系统；
为ESXi主机自动安装补丁；
完全兼容其他vsphere特性，如分布式资源调度器。
```

### VMware vsphere Web客户端和vsphere客户端

​     浏览ESXi主机或vCenterServer的URL，选择相应的安装链接（在一些时候下载客户端需要互联网连接), 就可以安装vsphere客户端。vsphere客户端提供了富图形用户界面(GUI)，它可以完成日常管理任 务，以及虚拟基础架构的高级配置。虽然vSphere客户端可以直接连接一个ESXI主机，或者连接一 个Server实例，但是只有当vSphere客户端连接到vCenterServer时才能使用完整的管理功能。  

### VMware vRealize orchestrator

​     VMwarevRealizeOrchestrator是一个工作流自动化引擎，它自动安装在vCenterServer的每一 个实例上。使vRealizeOrchestrator，vsphere管理员就可以为vCenterServer中各种任务建立自 动化工作流。

###  vsphere vMotion

允许管理员将一台正在运行的虚拟机从 一台物理主机迁移到另一台物理主机，而不需要关闭虚拟机。当虚拟机在两台物理主机之间迁移时， 完全不会引起停机，也不会中断虚拟机的网络连接。

还有一种情况是需要从一组物理服务器迁移到另一组物理服务器。如果操作细节已经 确定（第12章会详细介绍vMotion），那么可以使用vMotion将虚拟机从旧服务器迁移到新服务器 上，从而在不中断服务的前提下快速地完成服务器迁移。

###  vsphere Storage DRs

它可以自动根据存储使用情况来确定虚拟机虚拟磁盘的位置Storage DRS是通过使用数据存储集群来实现这个功能。在创建一个新的虚拟机时,只需要将其指定到一个数据存储集群, Storage DRS会自动将这个 虚拟机的虚拟磁盘部署到数据存储集群中恰当的数据存储中。

​		正如vSphere DRS使用vMotion动态地平衡资源使用率一样, Storage DRS则基于容量和/或延迟临界值使用Storage vMotion重新平衡存储使用率。因为Storage vMotion通常比VMotion操作更加占用资源,所以vSphere会更严格控制临界值、时间设置等可能触发Storage DRS通过Storage vMotion执行自动化迁移的指标，保证需要优先访问存储资源的虚拟机能够获得所需要的资源。

​		网络I/O控制(NIOC)也有相同的功能,它通过虚拟机使用物理NIC上的网络带宽为VMware管理员提供更细粒度的控制。由于吉比特以太网的广泛应用,网络I/O控制为VMware管理员提供了一种更可靠的方法,可以保证根据虚拟机的优先级与限制正确分配网络带宽。

###  vsphere HA

​		当服 务器出现故障时(或当其他基础架构出现问题时, VSphereHA会重新启动运行在ESXi主机的虚拟机。图1.3 当ESXi主机遇到服务器故障时, VSphereHA会重新 启动在其中运行的所有虚拟机发生的迁移。

​		Sphere HA并没有使用vMotion技术作为服务器的迁移手段o vMotion只适用于预先规划的迁移,而且要求源与目标的ESXi主机都处于正常运行状态。在vSphere HA故障恢复过程中,没有人能够预知故障;这不是预先规划的停机事件,因此没有足够的时间执vMotion操作。 vSphere RA则适用于解决物理ESXi主机或其他基础架构部件故障造成的计划外停机。

### vsphere FT

​		 VSphere HA可以在物理主机出现故障时自动重启虚拟机,从而解 决计划外物理服务器故障问题。在出现物理主机故障时才重启虚拟机,这意味着会出现停机时，vSphereFT则更进一 步,可以在出现物理主机故障时不造成停机。

​		出现多个主机故障时(例如,运行主副虚拟机的主机同时出现故障), VSphere HA将会在另一台可用服务器上重启主虚拟机,而vSphere FT将自动创建一个新的副本虚拟机。同样,这种方 法可以保证主虚拟机在任何时候都能得到保护。

###  VADP与VDP

​		VADP是一组应用编程接口(API),备份供应商可以利用这组API实现增强的虚拟化环境备份功能o vADP还支持文件级备份与恢复等功能;支持增量、差别和全映像备份;原生整合备份软件;支持多种存储协议。

​		VMware也提供了一个自己实现的备份工具,即VMware数据保护(VDP)。VDP是使用VADP和EMC Avamar技术实现面向小型VMware vSphere环境的完整备份解决方案。

###  vsphere复制

允许客户将虚拟机从一个vSphere环境复制到另一个vSphere环境。这意味着从 一个数据中心(通常是主数据中心或生产数据中心)到另一个数据中心(通常是辅助、备份或灾难恢复[DR]站点)。与基于硬件的解决方法不同, VSphere复制运行在每台虚拟机上,所以对于客户 复制哪些工作负载都有着精细的控制。

### 存储l/O控制与网络l/O控制

存储I/O控制(SIOC)允许vSphere管理员为存储I/O指定相对优先级,也可以虚拟机指定存储I/O限制。这些设置会应用到整个集群范围:当延迟增长并超过用户配置的临界值时, ESXi主机就会检测到存储拥塞,

​		vSphere Flash Read Cache为使用固态存储提供了全面支持。管理员可以像为CPU核心、 RAM或网络连接分配虚拟机一样为固态缓存空间分配虚拟机o vSphere管理着圃态缓存容量如何分配以 及如何使用虚拟机



## 产品之间的交互关系和依赖关系

​    VMwareESXi构成了vSphere产品套件的基础，但是有一些特性需要使用vCenterServer. vMotion、StoragevM0tion、vSphereDRS、vSphereHA、vSphereFT、SIOC和NIOC等特性需要使 ESXi和vCenterServero

## 与其他虚拟化产品的取别

VMware vSphere的虚拟机管理程序ESXi使用类型1裸机虚拟机管理程序,它能够在虚拟机管理程序内部直接处理Ⅰ/Oo这意味着ESXi不需要主机操作系统(如Windows或Linux)就可以工作。虽然其他虚拟化解决方案也将自已标榜为“类型1裸机虚拟机管理程序”,但是市场中
大多数其他的类型虚拟机管理程序都需要使用“父分区”或“domO”,而所有虚拟机I/O都必须通过它们。