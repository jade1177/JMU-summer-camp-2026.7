# Day 1：Linux + VMware + Conda 环境搭建（新手版）

> 用最直白的语言讲清楚每一个知识点，帮你理解"为什么要这么做"。
> 
> 适合人群：完全零基础，第一次接触 Linux、VMware、Conda

---

## 一、VMware 是什么？为什么要装它？

### 先讲一个生活中的例子

你玩过**模拟城市**或者**模拟人生**这类游戏吗？

在游戏里，你有一个"虚拟世界"——里面有房子、有人、有车，看起来跟真实世界很像，但其实它**只是你电脑里的一个程序**，不是真的。

**VMware 就是一个类似的"模拟器"，但它模拟的不是城市，而是一台完整的电脑。**

### 为什么要"模拟"一台电脑？

你的电脑装的是 **Windows** 系统（就是开机看到的那个界面），这很好，平时办公、看视频、玩游戏都没问题。

但是，**做 AI 开发、写代码、部署项目** 的时候，工程师们几乎都用另一种系统，叫 **Linux**。

为什么？
- Linux 更稳定，不容易崩溃
- Linux 对开发工具支持更好
- 互联网上 99% 的服务器（比如网站后台、AI 模型运行的机器）都是 Linux

**所以，你现在面临一个问题：**
> 你的电脑是 Windows，但你要学的东西需要 Linux。

**解决方案有两个：**
1. **买一台新电脑**，装上 Linux —— 贵，麻烦
2. **用 VMware 在 Windows 里"虚拟"出一台 Linux 电脑** —— 免费，方便

VMware 就是走第二条路。

### 几个你肯定会遇到的概念

| 概念 | 用最简单的话解释 |
|------|---------------|
| **宿主机** | 你的真实电脑，也就是你现在正在用的这台 Windows 电脑 |
| **虚拟机** | VMware 模拟出来的"假电脑"，里面会装 Linux |
| **Ubuntu** | Linux 的一种"版本"，就像 Windows 有 Win10、Win11，Linux 也有 Ubuntu、CentOS 等。Ubuntu 是最适合新手的一种 |
| **ISO 镜像** | 操作系统的安装文件，就像你装游戏需要一个 `.exe` 安装包一样 |
| **NAT 模式** | 让虚拟机"借用"你的 Windows 网络来上网，最简单，不用额外设置 |

> **你现在只需要记住：VMware = 在 Windows 里再开一台"假电脑"，这台假电脑里装 Linux。**

---

## 二、安装 Ubuntu（Linux 的一种）

### 整个过程就像"组装一台新电脑"

你文档里写了 45 步，其实核心就几步：

**第一步：在 VMware 里"创建"这台假电脑**
- 打开 VMware → 新建虚拟机
- 选择"自定义"安装
- 选择"Linux" → "Ubuntu 64 位"
- 给它起个名字，比如 `MyUbuntu`，放在 D 盘（不要放 C 盘，C 盘是系统盘，满了会卡）

**第二步：给这台假电脑分配"硬件"**
- **处理器**：相当于 CPU，设 4 个核心就行（不用懂，填 4）
- **内存**：相当于运行内存，设 4G（4096MB）。注意：这是从你的真实电脑里"借"出来的，所以你的真实电脑最好有 8G 以上内存
- **硬盘**：相当于存储空间，设 20GB，存在 D 盘
- **网络**：选 **NAT 模式**（最简单，虚拟机直接共享你的 Windows 网络）

**第三步：把 Ubuntu 系统"装"进去**

这里要先理解一个东西：**ISO 镜像文件**。

---

### ISO 镜像是什么？（重点！新手最容易懵）

**生活中的例子：**

你买过游戏光盘吗？或者装过 Windows 系统？

以前装系统是这样的：
1. 去电脑城买一张**光盘**（圆形的那个）
2. 把光盘放进电脑的光驱
3. 电脑从光盘读取，开始安装系统

**ISO 文件就是那张光盘的"电子版"。**

它把整张光盘的内容，原封不动地打包成一个文件，后缀名是 `.iso`。

**为什么要做成 ISO？**

因为现在的电脑很多都没有光驱了（放光盘的那个口子），所以：

> **以前用光盘装系统 → 现在用 ISO 文件装系统**

ISO 文件的好处：
- 可以从网上直接下载（不用去电脑城买光盘）
- 可以复制、保存到 U 盘或硬盘里
- VMware 可以直接"读取"这个 ISO 文件，就像电脑读取光盘一样

**所以，Ubuntu ISO 就是 Ubuntu 系统的安装包。**

就像你装微信要下载 `WeChatSetup.exe`，装 Ubuntu 要下载 `ubuntu-xx.xx.iso`。

---

### 怎么把 ISO 装进虚拟机？

**1. 先下载 Ubuntu ISO**

你去 Ubuntu 官网或者国内镜像站，下载一个文件，名字大概长这样：ubuntu-18.04.6-desktop-amd64.iso 或者是 ubuntu-20.04.6-desktop-amd64.iso

> **注意**：这个文件比较大，大概 2-3GB，下载需要一点时间。

下载好后，把它放在你找得到的地方，比如 Windows 的 D 盘里。

**2. 在 VMware 里"插入"这张 ISO**

创建虚拟机的时候，VMware 会问你：

> "你要怎么安装操作系统？"

你要选：**"稍后安装操作系统"**（英文是 "I will install the operating system later"）

**为什么选这个？** 因为我们要先建好虚拟机的"硬件"，然后再手动指定 ISO 文件。

**建完虚拟机后，指定 ISO：**

虚拟机创建好后，在 VMware 主界面，你会看到你的虚拟机（比如叫 `MyUbuntu`）。

- **右键点击**这个虚拟机 → 选择 **"设置"**（Settings）
- 或者点击 **"编辑虚拟机设置"**

打开设置窗口后，左边有一列硬件列表，找到 **"CD/DVD (SATA)"**：
点击 **"CD/DVD (SATA)"** 后，右边会出现选项：

点击 **"浏览..."**，然后在弹出的窗口里，找到你下载的 `ubuntu-18.04.6-desktop-amd64.iso` 文件，选中它，点击"打开"。

最后点击 **"确定"** 保存设置。

**整个过程用大白话讲：**

| 步骤 | 你在做什么 | 相当于生活中 |
|------|-----------|-----------|
| 下载 Ubuntu ISO | 从网上下载系统安装包 | 去电脑城买一张系统安装光盘 |
| 在 VMware 里指定 ISO | 告诉 VMware "我的光驱里放的是这个文件" | 把光盘插进电脑的光驱 |
| 启动虚拟机 | 开机，从光盘启动 | 按下电脑开机键，开始装系统 |

**3. 启动虚拟机，开始安装**

现在你的虚拟机已经"插入了 Ubuntu 安装光盘"。

点击 **"开启此虚拟机"**（Power on this virtual machine），它就会从 ISO 文件启动，进入 Ubuntu 的安装界面。

- 选择"安装 Ubuntu" → 选中文 → 选"最小安装"（装得快，不要装多余的软件）
- **关键**：安装过程中会提示"清除磁盘"，别怕，它清除的是虚拟磁盘（那 20GB），不是你真实的 D 盘！
- 设置用户名和密码 → 等待安装完成 → 重启

**装好后，你看到的就是一个 Linux 桌面了。**

---

## 三、装完 Ubuntu 后，第一件事：装 VMtools

### 为什么要装这个？

你现在打开虚拟机，会发现：
1. 虚拟机窗口很小，不能放大
2. **最重要的是：你不能在 Windows 和虚拟机之间复制粘贴文字**

VMtools 就是解决这些问题的"增强插件"。

装好后：
- 窗口可以自适应大小
- **Windows 和 Linux 之间可以复制粘贴（Ctrl+C、Ctrl+V）**

### 怎么装？

打开 Linux 里的"终端"（相当于 Windows 的"命令提示符"，按 `Ctrl + Alt + T` 打开），输入：

bash
sudo apt-get install open-vm-tools
sudo apt-get install open-vm-tools-desktop

sudo：以管理员身份运行（相当于 Windows 的"以管理员身份运行"）

apt-get install：安装软件（相当于 Windows 的"安装程序"）

输入后会要求你输密码——注意：Linux 里输密码是不显示任何字符的（连星号都没有），你直接输完按回车就行

## 四、Linux 基础命令（从现在开始，你要跟文字打交道了）

### 什么是"命令"？

在 Windows 里，你习惯用鼠标点图标、打开文件夹。

在 Linux 里，很多事情用文字命令来完成，更快、更强大。

打开终端（Ctrl + Alt + T），你会看到一个黑框，里面有个光标在闪，这就是你输入命令的地方。

### 文件和目录操作（最常用的）

想象你在一个"文件柜"里，每个"抽屉"就是一个文件夹（Linux 里叫"目录"）。

| 命令 | 作用 | 例子 |
|------|------|------|
| ls | 列出当前目录里的所有文件和文件夹 | 输入 ls，回车，就会看到当前目录里有什么 |
| ls -la | 更详细的列表，包括隐藏文件 | 文件名前面带 . 的是隐藏文件，平时看不到 |
| pwd | 显示我现在在哪（当前路径） | 比如显示 /home/yin，意思是在"yin"这个用户的主目录里 |
| cd | 切换目录（进入某个文件夹） | cd Desktop 进入桌面文件夹；cd .. 返回上一级；cd ~ 回到自己的主目录 |
| mkdir | 创建文件夹 | mkdir test 创建一个名叫 test 的文件夹 |
| touch | 创建空文件 | touch hello.py 创建一个名叫 hello.py 的空文件 |
| cp | 复制 | cp hello.py Desktop/ 把 hello.py 复制到桌面 |
| mv | 移动或重命名 | mv hello.py Desktop/ 把文件移到桌面；mv old.py new.py 把文件改名 |
| rm | 删除 | rm hello.py 删除文件；rm -r test/ 删除文件夹（-r 表示递归删除里面的所有东西） |

> **警告：rm 删除的东西不会进回收站，直接消失，删之前想清楚！**

### 权限是什么？（这个很重要，但一开始不用太深究）

Linux 里，每个文件都有"权限"——谁能读、谁能写、谁能运行。

用 `ls -l` 查看文件详情，你会看到类似这样：
-rwxr-xr-x 1 yin yin 1234 Jul 22 10:00 hello.py

看不懂？没关系，你只需要知道：

- 最左边的 `-rwxr-xr-x` 就是权限信息
- 如果你遇到"权限不够"的报错，用 `sudo` 在命令前面，或者修改权限：

bash
chmod 755 hello.py


## 数字含义

| 数字 | 权限 |
|------|------|
| 7 | 4+2+1 = 读+写+执行 |
| 6 | 4+2 = 读+写 |
| 5 | 4+1 = 读+执行 |
| 4 | 只读 |

## 其他实用的命令

| 命令 | 作用 |
|------|------|
| ifconfig | 查看网络信息，找到你的 IP 地址（看 ens33 那一行的 inet 后面的数字） |
| find | 找文件。`find / -name "*.py"` 在整个系统里找所有 .py 结尾的文件 |
| grep | 在文件里搜索文字。`grep "hello" *.py` 在所有 .py 文件里找包含 "hello" 的行 |
| top | 看系统资源占用，类似 Windows 的任务管理器 |
| sudo | 以管理员身份执行后面的命令 |
| apt-get install | 安装软件，比如 `sudo apt-get install git` 安装 Git |

### 一个超好用的小技巧：Tab 键自动补全

你输入文件名或路径时，不需要全打完。

比如你想进入 Desktop 文件夹，输入 `cd Des` 然后按 Tab 键，系统会自动补全成 `cd Desktop`。

如果有多个选项（比如有 Desktop 和 Development），按 **两次 Tab**，系统会列出所有匹配的选项让你选。

---

## 五、Anaconda 和 Conda（这是重点）

### 先讲一个生活中的问题

假设你同时在学两门外语：

- 学英语，需要一本英汉字典
- 学日语，需要一本日汉字典

如果你把两本字典混在一个抽屉里，找的时候会很乱，而且可能拿错。

更好的做法：给每门语言准备一个独立的抽屉。

### 写代码也有同样的问题

你可能同时在做两个项目：

- 项目 A：需要 Python 3.8 + TensorFlow 2.0
- 项目 B：需要 Python 3.10 + PyTorch 1.12

如果把这些工具都装在同一个地方：

- 版本会冲突（A 要 3.8，B 要 3.10，装了两个版本会打架）
- 包会冲突（TensorFlow 和 PyTorch 的某些依赖版本不兼容）

**Conda 的作用就是：给每个项目创建一个"独立的抽屉"，互不干扰。**

### Conda 具体能隔离什么？

你以为它只是隔离 Python 版本？其实它隔离的是一整套东西：

| 能隔离的 | 什么意思 |
|----------|----------|
| Python 版本 | 环境 A 用 Python 3.8，环境 B 用 Python 3.10，互不干扰 |
| 第三方库 | 环境 A 装了 Django，环境 B 装了 PyTorch，不会冲突 |
| 库的版本 | 两个环境可以装同一个库的不同版本（比如 numpy 1.0 vs 2.0） |
| 系统环境变量 | 每个环境有自己的 PATH，不会互相覆盖 |

### Conda 和 Anaconda 是什么关系？

这是两个容易搞混的词：

| 名字 | 是什么 | 比喻 |
|------|--------|------|
| Conda | 一个工具，用来创建和管理"环境" | 就像"房间管理器"，负责开门、关门、布置房间 |
| Anaconda | 一个"大礼包"，里面包含了 Conda + Python + 很多常用库 | 就像"精装房"，拎包入住，不用自己一个个装 |

所以：

- 你下载安装的是 **Anaconda**（大礼包）
- 你实际用的是里面的 **Conda**（房间管理器）

### 安装 Anaconda

#### 1. 下载 Anaconda

在 Windows 里下载 Anaconda 的 Linux 版本（一个 .sh 文件），文件名大概长这样：Anaconda3-2024.02-Linux-x86_64.sh

#### 2. 把文件传到虚拟机

- 如果装了 VMtools，直接从 Windows 拖到虚拟机桌面
- 或者用 U 盘、共享文件夹等方式传进去

#### 3. 在 Linux 终端里安装

bash
cd ~/Desktop
bash Anaconda3-2024.02-Linux-x86_64.sh

## 六、安装过程

1. 一路按回车看协议（按空格翻页）
2. 最后输入 `yes` 同意许可
3. 它会问你安装路径，直接回车用默认的就行
4. 最后会问你要不要初始化，输入 `yes`

### 5. 重启终端

安装完成后，关闭终端，重新打开一个新的，这样环境变量才能生效。

---

## Conda 的核心命令（这几个命令你要天天用）

### 1. 查看你有哪些"房间"

```bash
conda env list
```

会显示类似这样：

```
# conda environments:
#
base                  *  /home/yin/anaconda3
```

- `base` 是默认环境（Anaconda 自带的）
- `*` 表示你现在在这个环境里

### 2. 创建一个新"房间"

```bash
conda create -n summer_camp python=3.10
```

**拆解：**

| 参数 | 含义 |
|------|------|
| `conda create` | 创建新环境 |
| `-n summer_camp` | 给这个房间起名叫 `summer_camp` |
| `python=3.10` | 这个房间里装 Python 3.10 |

执行后，Conda 会问你要不要装，输入 `y` 回车。

### 3. "走进"这个房间

```bash
conda activate summer_camp
```

执行后，你的命令行会变成这样：

```
(summer_camp) yin@ubuntu:~$
```

前面的 `(summer_camp)` 就是提示：你现在在这个房间里。

### 4. 在这个房间里装东西

比如装 PyTorch：

```bash
conda install pytorch
```

这个东西只会装到 `summer_camp` 这个房间里，不会影响其他房间，也不会影响 `base` 环境。

### 5. "走出"这个房间

```bash
conda deactivate
```

前面的 `(summer_camp)` 消失了，你回到了 `base` 环境。

### 6. 删除一个房间（如果建错了）

```bash
conda remove -n summer_camp --all
```

### 7. 查看当前房间里有什么

```bash
conda list
```

会列出这个环境里装的所有库和版本。

---

## 再打个比方，帮你彻底理解

想象你租了一栋大楼（你的电脑），Conda 是大楼管理员。

| 操作 | 比喻 |
|------|------|
| `conda env list` | 查看大楼里有哪些房间 |
| `conda create -n 房间名` | 管理员给你装修一个新房间 |
| `conda activate 房间名` | 你刷卡进入这个房间 |
| `conda install 软件` | 在这个房间里添置家具 |
| `conda deactivate` | 你走出房间，回到大厅 |
| `conda remove -n 房间名 --all` | 拆除这个房间，清空所有东西 |

**关键点：** 每个房间的家具（库）互不干扰。A 房间里的沙发坏了，不会影响 B 房间。

---

## 为什么要换"国内镜像源"？

默认情况下，Conda 从国外服务器下载软件，非常慢，甚至下不动。

换成国内的"镜像源"（比如清华大学的镜像），速度会快很多。

执行这几行命令：

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

这就像：本来你要去美国书店买书，现在改成去家附近的中国书店买，快多了。

---

## 七、environment.yml：环境的"克隆图纸"

### 这是什么？

`environment.yml` 是别人房间里的"家具清单"。

你拿到清单后，Conda 可以一键帮你装修出一模一样的房间。

### 别人给你 environment.yml 后怎么做？

#### 第一步：确认文件在你手里

别人给你这个文件后，你要把它放到你的虚拟机里。

**怎么放？**

- 如果装了 VMtools，直接从 Windows 拖到虚拟机桌面
- 或者用 U 盘、共享文件夹等方式传进去

假设文件现在在你的桌面，路径是：

```
~/Desktop/environment.yml
```

#### 第二步：用 Conda 一键创建环境

打开终端，执行：

```bash
conda env create -f ~/Desktop/environment.yml
```

**拆解：**

| 参数 | 含义 |
|------|------|
| `conda env create` | 创建环境 |
| `-f ~/Desktop/environment.yml` | 按照 `-f`（file）后面这个文件里的清单来创建 |

Conda 会自动读取清单，然后：

1. 创建一个和原作者一模一样的环境名
2. 安装一模一样的 Python 版本
3. 安装清单里列出的所有库（包括具体版本号）

等待下载和安装完成。

#### 第三步：激活环境，开始使用

创建完成后，Conda 会提示你环境的名字（比如 `summer_camp`）。

激活它：

```bash
conda activate summer_camp
```

然后你就可以在这个和原作者一模一样的环境里运行代码了。

### environment.yml 里面长什么样？

你可以用文本编辑器打开看看，大概是这样的：

```yaml
name: summer_camp          # 环境的名字
channels:                  # 从哪些"商店"下载软件
  - defaults
  - conda-forge
dependencies:              # 要安装的软件清单
  - python=3.10            # Python 版本
  - numpy=1.24.3           # numpy 库的具体版本
  - pandas=2.0.3
  - pytorch=2.0.1
  - pip                    # 也会装 pip
  - pip:                   # 有些库用 pip 装
    - some-package==1.2.0
```

这就是一个"装修清单"：

- 房间叫什么名字
- 去哪些商店买材料
- 每个家具是什么牌子、什么型号

### 反过来：你怎么给别人你的环境？

你在 `summer_camp` 环境里装好了所有东西，想分享给队友：

```bash
conda activate summer_camp
conda env export > environment.yml
```

这会在当前目录生成一个 `environment.yml` 文件，发给别人就行。

**注意：** `conda env export` 会把你环境里所有库都列出来（包括自动安装的依赖），清单可能很长。如果只想列你手动装的，可以加 `--from-history`：

```bash
conda env export --from-history > environment.yml
```

### 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| 下载很慢 | 默认从国外下载 | 先换国内镜像源，再执行创建命令 |
| 有些包装不上 | 清单里的版本太老，或者平台不兼容 | 让作者更新清单，或者手动调整版本号 |
| 环境名冲突 | 你已经有同名环境了 | 先删除旧环境 `conda remove -n 名字 --all`，或者修改 yml 里的 `name` |

---

## 八、遇到的问题（踩坑记录）

| 问题 | 现象 | 解决办法 |
|------|------|----------|
| 虚拟机没网 | 打不开网页 | 看 VMware 窗口右下角，有个网络图标，右键点击选择"连接"；或者在终端输入 `sudo service network-manager restart` |
| 查不到 IP 地址 | 输入 `ifconfig` 看不到 `ens33` | 输入 `sudo ifconfig ens33 up` 手动打开网卡 |
| 下载软件特别慢 | `apt-get install` 或 `conda install` 卡很久 | 换成国内镜像源 |
| 文件拖不进虚拟机 | 从 Windows 拖文件到 Linux 没反应 | 先装 VMtools |
| 输密码没反应 | 输入密码时屏幕没有任何变化 | 正常！Linux 输密码就是不显示的，直接输完按回车 |

---

## 九、这些东西对"产品经理"有什么用？

你可能在想：我又不当程序员，学这些干嘛？

**因为 AI 产品经理需要跟工程师"说同一种语言"。**

| 工程师说的话 | 你学了这些之后能听懂 |
|-------------|---------------------|
| "这个模型需要部署在 Linux 服务器上" | 你知道 Linux 是什么，知道服务器跟你的虚拟机原理一样 |
| "依赖冲突了，需要新建一个 Conda 环境" | 你知道"依赖冲突"是什么意思，知道 Conda 环境隔离的作用 |
| "这个操作需要 sudo 权限" | 你知道这是管理员权限，涉及安全问题，不能乱给 |
| "帮我看一下服务器日志" | 你知道用 `ls`、`cd`、`grep` 这些命令去看日志 |
| "我们用 Docker 解决环境一致性问题" | 你知道 Docker 和 Conda 类似，都是做"隔离"的 |
| "这是 environment.yml，你克隆一下环境" | 你知道怎么用 `conda env create -f` 一键复制环境 |

你现在不需要成为专家，你只需要：

- 知道 Linux 是开发 / 部署的主流环境
- 知道"环境隔离"很重要（Conda / Docker 都是干这个的）
- 知道权限管理跟安全有关
- 遇到问题能看懂报错信息，能查资料
- 知道 `environment.yml` 是环境的"克隆图纸"，能复制和分享环境

---

## 十、今晚你可以做的练习

### 练习 1：验证环境

打开你的虚拟机，打开终端，依次执行：

```bash
conda activate summer_camp
python --version
```

如果显示 `Python 3.10.x`，说明环境创建成功了。

### 练习 2：基础 Linux 命令

试试这些命令：

```bash
ls          # 看看当前目录有什么
pwd         # 看看自己在哪
mkdir test  # 创建一个 test 文件夹
cd test     # 进入 test 文件夹
touch a.py  # 创建一个空文件
ls          # 看看 test 文件夹里有什么
cd ..       # 返回上一级
rm -r test  # 删除 test 文件夹
```

### 练习 3：导出和导入环境

```bash
# 导出当前环境
conda env export > my_env.yml

# 删除当前环境
conda deactivate
conda remove -n summer_camp --all

# 用 yml 重新创建环境
conda env create -f my_env.yml

# 激活新环境
conda activate summer_camp
```
"""





