# Day 1：Linux操作系统与VMware虚拟机

## 一、今天要解决什么问题？

在Windows电脑上"虚拟"出一台独立的Linux电脑，用来搭建统一的开发环境。

&gt; 为什么不用Windows直接开发？  
&gt; 因为很多开发工具（特别是AI相关的）在Linux上运行更稳定，而且"服务器上跑的环境"大多是Linux，本地用Linux开发能减少"我电脑上能跑，部署上去不行"的问题。

## 二、核心概念（我自己的理解）

| 概念 | 我的理解 |
|:---|:---|
| **宿主机** | 我的真实电脑（Windows） |
| **虚拟机** | 用软件"模拟"出来的另一台电脑，有自己的操作系统 |
| **VMware** | 一个"虚拟机管理软件"，帮我在Windows里创建和管理虚拟机 |
| **Ubuntu** | 一种Linux操作系统，免费、好用、社区大 |
| **桥接模式** | 虚拟机和宿主机"平起平坐"，各自有独立的IP，虚拟机可以直接连外网 |
| **NAT模式** | 虚拟机"躲在"宿主机后面，共享宿主机的网络 |
| **Conda** | Python的"环境管理器"，可以在一台电脑里创建多个独立的Python环境，互相不干扰 |

## 三、关键操作（我实际做过的）

```bash
# 创建一个叫 summer_camp 的虚拟环境
conda create -n summer_camp python=3.10

# 激活这个环境
conda activate summer_camp

# 查看当前有哪些环境
conda env list

# 换国内镜像源（下载包更快）
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
