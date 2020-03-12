# 基础概念

## 什么是容器

下列东西的集合

- namespace 资源视图隔离(各个进制之间无法看到对方)
- cgroup 控制资源使用率(内存，CPU)
- chroot 独立的文件系统

容器是一个视图隔离，资源可限制，独立文件系统的集合。

容器的本质：一个视图被隔离，资源受限的进程

容器里PID=1的进程就是应用本身

管理虚拟机=管理基础设施 管理容器等于管理应用本身

`Kubernetes` 就是云时代的操作系统。依次类推，`容器镜像`就是操作系统的软件安装包

## 镜像

镜像：运行容器所需要的所有文件的集合

## moby

容器引擎架构

## containerd