# 在虚拟机中模拟运行 Haiku

Haiku 能够在您的计算机里虚拟运行，这也意味着您不需要进行实际安装以便使用，而只需要在虚拟机中运行即可。虚拟机类似于在您的电脑中运行的虚拟计算机，这样就允许您在应用窗口中使用 Haiku 系统，而且还可以无碍的使用您的常用的操作系统。这种能够允许您执行这些操作的软件称之为硬件虚拟化（Hypervisors）或者虚拟机管理器。您可以下载 Haiku 镜像，然后像在实体机上安装 Haiku 一样在虚拟机中进行安装，然后在虚拟机打开运行。

安装 Haiku ，通常有几种不同类型的镜像，包括 VMDK 镜像，原生镜像，ISO 镜像，以及 AnyBoot 镜像（是原生镜像和 ISO 镜像的混合镜像）。原生镜像和 VMDK 镜像是一种预安装环境，它们的虚拟磁盘大小无法修改，并且不需要执行安装过程。ISO 镜像用于烧录安装光盘，以及用于虚拟机的虚拟 CD-ROM 。

对于硬件虚拟化，有两种类型。使用最多的是寄生硬件虚拟化，您可以直接下载并在常用系统中运行。另一种是原生硬件虚拟化，这也就意味着它运行在常规操作系统的更底层，因此需要特别的设置和操作系统。如果您从未使用过虚拟机，我们建议使用主流虚拟机，如 VitrualBox 或者 VMWare Player。

## 寄生虚拟化

### 跨平台
* [QEMU](./virtualizing/qemu.md)
* VMWare Player
* VirtualBox
    * Linux 平台下串口调试
	* Windows 平台下串口调试
* Parallels WorkStation
* Bochs
* JPC
* SimNow
* SimICS

### 仅支持 Windows 平台

* Virtual PC

### 仅支持 Mac 平台

* VMware Fusion
* Parallels Desktop
* Parallels Server for Mac
* Q

### 仅支持 Linux 平台

* KVM

## 原生虚拟机

* Xen
* Hyper-V
* Oracle VM
* VMWare ESXi
* Ganeti