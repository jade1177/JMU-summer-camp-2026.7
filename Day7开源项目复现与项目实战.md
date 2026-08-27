# Day 7 - 从零租用 GPU 云服务器，并复现第一个开源项目（新手超详细笔记）

## 📌 写在前面：小白常见恐惧，一次打消

> "我电脑没独显，能行吗？"
> 完全可以。你的电脑只负责显示界面、编辑代码、发命令，所有重活（计算、训练、推理）都在远程服务器上跑。

> "是不是要先装 CUDA、PyTorch？"
> 不用。服务器镜像里已经预装好了。

> "会不会很贵？"
> 本次课堂用按量计费，只在上课期间开机，课后关机，费用极低（几块钱）。

---

## 🎯 学完这节课，能做到什么？

- 根据任务需求挑选合适的 GPU 服务器配置。
- 在算力租赁平台（优云智算）创建一台按量计费的实例。
- 用终端 SSH 和 VS Code Remote-SSH 连接远程服务器。
- 在远端创建、编辑、运行 Python 代码。
- 从 GitHub / Gitee / 离线包获取开源项目。
- 阅读 README 和依赖文件，按照说明成功运行项目。
- 遇到报错时，保存日志并用搜索 / AI 分析问题。
- 正确关闭实例，避免产生额外费用。

---

## 一、课前准备（账号 + 软件）

### 1. 注册账号

- 集大邮箱：申请一个集大邮箱（用于高校认证，可获取优惠或免费额度）。
  申请地址：https://net.jmu.edu.cn/jdyx/jdyxsq.htm
- 优云智算平台：注册账号，完成高校邮箱绑定 + 高校实名认证。
- GitHub / Gitee：能正常登录即可（优云平台有时会提供网络加速，不登录也可能行）。

> 如果认证遇到问题，联系学长安排小组共用实例，不要提前创建付费实例！

### 2. 本地需要安装的软件

| 软件 | 用途 | 下载/检查 |
|------|------|-----------|
| Visual Studio Code | 编辑远端代码 | https://code.visualstudio.com/ |
| Git（本课可选，但装了更好） | 下载管理代码 | git --version 检查 |
| OpenSSH Client（Win10/11 自带） | SSH 连接服务器 | ssh -V 检查 |
| VS Code 扩展 Remote - SSH | 在 VS Code 中远程开发 | 在扩展市场搜索安装 |

> 特别提醒：不需要在本地安装 CUDA、cuDNN、PyTorch、Docker 或 NVIDIA 驱动——这些都在服务器里。

### 3. 检查软件是否可用

打开 PowerShell（Windows）或终端（macOS/Linux），依次运行：

```bash
git --version
ssh -V
```

如果显示出版本号，说明 OK。如果 ssh 提示"无法识别"，Win 用户需在 设置 -> 系统 -> 可选功能 中安装 OpenSSH 客户端。

code 命令如果不可用也没关系，直接用 VS Code 桌面程序即可。

---

## 二、你的电脑没独显 / 不能访问 GitHub？照样能完成！

PDF 提供了三种获取代码的路线，选一种就行：

| 路线 | 适用情况 | 操作工具 |
|------|----------|----------|
| A (GitHub) | 能稳定访问 GitHub | VS Code Remote-SSH 或终端 SSH |
| B (Gitee) | GitHub 不稳定/不能用代理 | VS Code Remote-SSH 或网页终端 |
| C (离线 ZIP) | 网络完全不通 | 从飞书下载 ZIP，上传到服务器解压 |

三条路线最终运行的命令和验证结果完全一样，不需要为了上 GitHub 而临时折腾代理。

---

## 三、如何选择服务器和 GPU？（小白也能懂）

### 先问自己：项目真的需要 GPU 吗？

- 如果只是跑普通 Python 脚本、处理文本、搭个简单网页 -> CPU 就够了。
- 如果 README 里明确写了 CUDA、深度学习、GPU 推理 -> 才需要 GPU。

今天的课堂项目即使 CPU 也能跑，我们租 GPU 是为了体验完整云端工作流。

### 显存（VRAM）是第一优先级

- 显存不足，再快的 GPU 也加载不了模型。
- 查看 README、Requirements、项目 Issues 里作者推荐的显存大小。

### 总费用 ≠ 每小时单价 × 时间

- 更贵的卡可能跑得更快，总费用反而更低。
- 但本次课堂任务很小，选有库存、单价低、稳定的型号即可。

### 配置建议（本次课堂）

- 1 张 GPU
- 最低档 CPU / 内存
- 默认系统盘（不挂载额外数据盘）
- 按量计费（不要包月）

---

## 四、在优云智算创建实例（一步一步来）

### 1. 登录后进入"部署 GPU 实例"

### 2. 按顺序选择配置（重点！）

| 配置项 | 选择内容 |
|--------|----------|
| 镜像类型 | 平台镜像 或 基础镜像 |
| 镜像 | 选择 PyTorch（已预装 CUDA 和 Python） |
| 镜像版本 | 选择课堂指定的稳定 Ubuntu + PyTorch 组合 |
| 实例方式 | 使用课堂指定的稳定方案 |
| GPU 型号 | 课堂公布的低价可用型号（如 RTX 4090 等） |
| GPU 数量 | 1 |
| CPU / 内存 | 最低或默认档 |
| 系统盘 | 默认容量（一般够用） |
| 数据盘 | 不挂载（本课不需要） |
| 计费方式 | 按量计费 |
| 实例名称 | 建议 day7-学号后四位，例如 day7-0123 |

### 3. 不要选这些

- 多张 GPU
- 包月付费
- 额外付费数据盘（免费额度内一般够用）
- 自己不了解的公网防火墙配置
- 只因为型号更高而选昂贵 GPU

### 4. 点击"立即部署"，等待状态变为"运行中"

创建成功后，在实例列表看到"登录"和"关机"按钮。

记录以下信息（记在小本本上）：
- 实例名称
- GPU 型号
- 显存大小
- 按量单价
- 开始运行时间

### 常见创建问题

| 现象 | 解决办法 |
|------|----------|
| 没有余额/体验金 | 联系学长说明 |
| 所选 GPU 无库存 | 返回选另一张课堂允许的低价卡 |
| 部署很久未完成 | 刷新列表，检查订单状态，不要重复创建多台 |
| 提示存在未支付订单 | 先检查订单，不要连续点"立即部署" |

---

## 五、第一次用终端 SSH 登录服务器

### 什么是 SSH？

SSH 是一种加密的远程连接协议。登录后，你的本地终端就变成了远程服务器的"遥控器"，敲的命令都在服务器上执行。

### 1. 复制平台提供的 SSH 命令

- 在实例列表点击"登录" -> 找到 SSH 登录方式。
- 复制完整 SSH 命令（例如 ssh -p 23 root@xxx.xxx.xxx.xxx）。
- 复制 SSH 密码到剪贴板（不要把密码发到群里、表格里或 AI 里！）。

### 2. 在本地 PowerShell / 终端粘贴命令并回车

第一次连接会问：

```bash
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

输入 yes 回车。

然后提示输入密码，粘贴密码时屏幕上不会显示任何字符（不会出现星号），这是正常的，粘贴后直接回车。

### 3. 登录成功后，检查远端环境

依次运行以下命令，确认一切都正常：

```bash
whoami                # 显示当前用户（通常是 root）
hostname              # 显示服务器主机名
pwd                   # 显示当前目录
python --version      # 查看 Python 版本
git --version         # 查看 Git 版本
nvidia-smi            # 查看 GPU 信息（型号、显存、驱动）
```

如果 python 找不到，试试 python3 --version。

运行 nvidia-smi 应该能看到类似这样的输出：

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 595.80    Driver Version: 595.80    CUDA Version: 13.2           |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce RTX 4090  Off  | 00000000:23:00.0 Off |                  Off |
| 47%   28C    P8    23W / 450W |      1MiB / 24564MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
```

看到这些，说明服务器环境 OK！

### 4. 记录你的"远端身份卡"

```bash
printf 'USER=%s\nHOST=%s\nDIR=%s\n' "$(whoami)" "$(hostname)" "$(pwd)"
```

会输出类似：

```
USER=root
HOST=cpod-xxxxx
DIR=/root
```

---

## 六、生成 SSH 密钥，免密码登录（推荐）

每次登录都要输密码很麻烦，而且不安全。SSH 密钥更安全、更方便。

### 什么是 SSH 密钥？

- 私钥（id_ed25519_day7）：放在你本地电脑，绝不能给任何人。
- 公钥（id_ed25519_day7.pub）：可以上传到服务器，服务器用它来验证你的身份。

### 1. 先退出服务器（回到本地终端）

```bash
exit
```

### 2. 在本地生成密钥（不是在服务器上）

Windows PowerShell：

```bash
# 先检查是否已存在
Test-Path "$HOME\.ssh\id_ed25519_day7"

# 如果输出 False，运行（把"学号"换成你的学号后四位，例如 1064）：
ssh-keygen -t ed25519 -C "1064-day7" -f "$HOME\.ssh\id_ed25519_day7"
```

macOS / Linux：

```bash
test -f "$HOME/.ssh/id_ed25519_day7" && echo exists || echo not-found
# 如果显示 not-found，运行：
ssh-keygen -t ed25519 -C "1064-day7" -f "$HOME/.ssh/id_ed25519_day7"
```

生成时会提示输入私钥口令（passphrase）——可以设置一个自己记得住的密码，也可以直接按两次 Enter 留空（临时环境可以留空）。

生成后，在 C:\Users\你的用户名\.ssh\ 下会出现两个文件：
- id_ed25519_day7 （私钥，不可上传）
- id_ed25519_day7.pub （公钥，可上传）

### 3. 把公钥添加到服务器

容器实例（端口不是 22）：

```bash
Get-Content "$HOME\.ssh\id_ed25519_day7.pub" | ssh -p 端口号 root@你的外网IP "umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys"
```

Ubuntu 虚拟机（端口 22）：

```bash
Get-Content "$HOME\.ssh\id_ed25519_day7.pub" | ssh ubuntu@你的外网IP "umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys"
```

（macOS/Linux 用户把 Get-Content 换成 cat，并调整路径）

这一步仍需要输入一次服务器密码。

### 4. 测试密钥登录

容器实例：

```bash
ssh -i "$HOME\.ssh\id_ed25519_day7" -p 端口号 root@你的外网IP
```

Ubuntu 虚拟机：

```bash
ssh -i "$HOME\.ssh\id_ed25519_day7" ubuntu@你的外网IP
```

如果设置了私钥口令，会提示输入私钥口令（而不是服务器密码）。登录成功后，以后就能免密码登录了。

---

## 七、使用 VS Code Remote-SSH（图形化远程开发）

用终端敲命令不够直观，VS Code 的 Remote-SSH 扩展可以让你像操作本地文件一样操作远端文件，还能用 VS Code 的编辑、终端、调试功能。

### 1. 在 VS Code 中打开 SSH 配置文件

- 按 Ctrl+Shift+P（Mac Cmd+Shift+P）打开命令面板。
- 输入 Remote-SSH: Open SSH Configuration File...
- 选择本机用户目录下的 config 文件（通常在 C:\Users\你的用户名\.ssh\config），不要选 ProgramData 里的系统配置文件。

### 2. 添加主机配置

清空原有内容（或直接覆盖），根据你的实例类型粘贴以下模板：

容器实例（端口不是 22）：

```ssh-config
Host day7-学号后四位
    HostName 你的外网IP或域名
    User root
    Port 端口号
    IdentityFile ~/.ssh/id_ed25519_day7
```

Ubuntu 虚拟机（端口 22）：

```ssh-config
Host day7-学号后四位
    HostName 你的外网IP或域名
    User ubuntu
    Port 22
    IdentityFile ~/.ssh/id_ed25519_day7
```

Host 只是本地昵称，可以随便起，但建议用英文和数字。

保存文件（Ctrl+S）。

### 3. 连接主机

- 再次按 Ctrl+Shift+P -> 选择 Remote-SSH: Connect Current Window to Host...
- 选择刚刚添加的 day7-学号后四位
- 如果询问远端系统，选 Linux
- 如果询问是否继续连接，选 Continue
- 如果设置了私钥口令，输入它

等待新 VS Code 窗口打开，左下角会显示 SSH: day7-xxxx，表示连接成功。

第一次连接可能较慢（1~3 分钟），耐心等待。

### 4. 在远端创建工作目录

在远程 VS Code 窗口中，打开终端（Terminal -> New Terminal），运行：

```bash
mkdir -p ~/day7-work
cd ~/day7-work
pwd
```

然后点击 文件 -> 打开文件夹，输入 ~/day7-work，打开并信任。

### 5. 确认你正在操作远端

在终端运行：

```bash
hostname
pwd
```

确认主机名是远程服务器的，并且路径是 /root/day7-work（或 /home/ubuntu/day7-work）。同时左下角显示 SSH: day7-...。

### 6. 如果 VS Code 一直连不上怎么办？

- 按 Ctrl+Shift+P -> Remote-SSH: Show Log 查看日志。
- 检查实例是否运行、User/Port/IdentityFile 是否正确。
- 如果还是不行，改用网页 JupyterLab（平台提供）：
  1. 在实例列表点击"登录 -> JupyterLab"
  2. 在 JupyterLab 里打开终端，mkdir -p ~/day7-work && cd ~/day7-work
  3. 后续所有 Git、运行命令都在网页终端执行，效果一样。

---

## 八、在服务器上运行第一段代码（闭环验证）

现在我们要在远程服务器上创建一个 Python 文件并运行，确保编辑和运行都在远端。

### 1. 新建文件 hello_server.py

在 VS Code 的远程资源管理器中，在 day7-work 目录下新建文件，命名为 hello_server.py，粘贴以下代码：

```python
import platform
import socket

print("Hello from the remote server!")
print("hostname:", socket.gethostname())
print("python:", platform.python_version())

try:
    import torch
    print("pytorch:", torch.__version__)
    print("cuda available:", torch.cuda.is_available())
    if torch.cuda.is_available():
        print("gpu:", torch.cuda.get_device_name(0))
except ImportError:
    print("pytorch: not installed; basic Python still works")
```

### 2. 在终端运行

```bash
cd ~/day7-work
python hello_server.py
```

如果只有 python3，就用 python3 hello_server.py。

你会看到类似输出：

```
Hello from the remote server!
hostname: cpod-xxxxx
python: 3.10.16
pytorch: 2.1.0+cu121
cuda available: True
gpu: NVIDIA GeForce RTX 4090
```

### 3. 修改代码验证实时同步

把第一行改成 print("Hello from 学号后四位!")，保存，重新运行，看到新输出即证明编辑和运行的是远端同一个文件。

---

## 九、如何寻找一个适合新手的开源项目？

### 选择标准（第一次复现）

- 项目目标清晰（一句话能说清）
- README 写得完整，有安装和运行命令
- 不需要 API Key、私密数据、付费账号
- 不需要下载大模型或大数据集
- 不依赖图形桌面
- 最近仍在维护（有近期提交）
- 运行时间在 5~10 分钟内

### GitHub 搜索示例

```text
python cli stars:>50 size:<5000 pushed:>2025-01-01 archived:false
```

含义：Python 命令行工具，Star > 50，仓库大小 < 5MB，2025 年后有更新，未归档。

### 阅读 README 时回答 6 个问题

1. 这个项目解决什么问题？
2. 支持什么操作系统和 Python 版本？
3. 需要哪些依赖？
4. 第一次运行应该执行哪条命令？
5. 看到什么输出才算成功？
6. 许可证是什么？最近是否维护？

### Requirements 可能不叫 requirements.txt

可能出现在：
- README 的 Installation 章节
- requirements.txt
- pyproject.toml
- environment.yml
- Dockerfile
- Release 或文档

注意：如果项目给了多种安装方式，选一种即可，不要全执行一遍。

---

## 十、完成一次标准化项目复现（备用项目）

我们先用课程提供的备用项目 day7-system-check 练手，它没有第三方依赖，非常轻量。

### 1. 回到工作目录

```bash
cd /root/day7-work
```

### 2. 选择一种方式获取代码

路线 A：GitHub

```bash
git clone https://github.com/JachinGilberts/day7-system-check
cd day7-system-check
```

路线 B：Gitee（如果 GitHub 慢）

```bash
git clone https://gitee.com/你的地址/day7-system-check
cd day7-system-check
```

路线 C：离线 ZIP

- 从飞书下载 day7-system-check.zip
- 通过 VS Code 或 JupyterLab 上传到 ~/day7-work
- 解压：

```bash
unzip day7-system-check.zip
cd day7-system-check
```

或者用 Python 解压：

```bash
python -m zipfile -e day7-system-check.zip .
```

### 3. 先阅读，后运行（重要！）

```bash
pwd
ls -la
sed -n '1,180p' README.md   # 查看 README 前 180 行
cat requirements.txt        # 查看依赖（这个项目没有）
```

确认项目用途、安装命令、运行方法。

### 4. 记录当前版本

```bash
git rev-parse HEAD          # 输出当前提交号（哈希）
git status --short          # 检查是否有未提交修改（没有输出则正常）
```

### 5. 运行测试和程序

```bash
python -m unittest discover -s tests -v
python src/system_check.py --name 学号后四位
```

如果只有 python3，把 python 换成 python3。

看到测试输出末尾有 OK，程序输出 JSON 里包含 "status": "success"，说明复现成功！

输出示例：

```json
{
  "status": "success",
  "student": "学号1064",
  "hostname": "cpod-xxxxx",
  "platform": "Linux-6.12.35-...",
  "python": "3.10.16",
  "executable": "/usr/local/miniconda3/envs/py310/bin/python",
  "pytorch": "2.1.0+cu121",
  "cuda_available": true,
  "gpu": "NVIDIA GeForce RTX 4090"
}
```

### 6. 保存一份复现记录（模板）

```
仓库或离线包地址：https://github.com/JachinGilberts/day7-system-check
提交号：25793237e943e9c384529b7f53c8690d5b17032f
Python 版本：3.10.16
GPU 型号：NVIDIA GeForce RTX 4090
实际运行命令：python src/system_check.py --name 1064
成功标志：status: success
```

---

## 十一、遇到报错怎么办？（小白生存指南）

报错是正常的，学会"优雅地报错"才是关键。

### 第一步：保存完整日志

不要把屏幕截图发群里，把错误输出重定向到文件：

```bash
python src/system_check.py --bad-argument > day7-error.log 2>&1
tail -n 20 day7-error.log   # 查看最后 20 行
```

这样你能看到完整的错误回溯，而不是被截断的红字。

### 第二步：提取关键信息搜索

不要整段粘贴日志。提取：
- 错误类型（如 ModuleNotFoundError）
- 关键错误句（如 No module named 'torch'）
- 软件名 + 版本（如 Python 3.10）

搜索示例：

```text
"ModuleNotFoundError" Python 3.10 project_name
"CUDA out of memory" PyTorch batch size site:github.com/作者/项目名/issues
```

优先查看：项目 README/Issues -> 官方文档 -> Stack Overflow -> 相似环境讨论。

### 第三步：用 AI 辅助分析（把日志喂给 AI）

使用这个模板，但要删掉敏感信息（密码、Token、IP）：

```text
我在 Ubuntu 远程服务器上复现一个 Python 项目。
目标：<我想运行什么>
环境：Python <版本>；GPU <型号或无>；框架 <版本>
执行命令：<原命令>
完整报错：
<删除密码、Token、IP 后粘贴日志>
我已经尝试：<做过什么>

请先指出日志中最关键的证据和最可能的错误层级，
再给不超过 3 步的最小排查方案；不要假设我可以重装系统。
```

发日志前务必删除：SSH 密码、私钥、API Key、Token、手机号、身份证号、真实外网 IP、带认证参数的 URL。

### 不同错误不同处理

- Connection timed out -> 网络/防火墙问题
- ModuleNotFoundError -> 缺少依赖，需要 pip install
- CUDA out of memory -> 显存不足，减小 batch size 或换小模型
- No space left on device -> 磁盘满了，清理缓存或扩容

不能所有问题都靠"重装"解决。

---

## 十二、自由项目挑战（自己选一个项目）

现在你去找一个简单项目（比如 rich 这个终端美化库），按以下流程走一遍：

1. 复制仓库 HTTPS 地址
2. git clone 到 ~/day7-work
3. 进入目录，阅读 README、Requirements、License
4. 记录 git rev-parse HEAD
5. 只执行 README 里明确给出的最小安装和 Quick start
6. 保存成功输出或完整错误日志
7. 记录你下一步准备做什么

挑战结果应包含：
- 仓库 URL
- 提交号
- 项目用途
- 环境要求
- 实际执行命令
- 成功输出，或完整日志 + 错误判断

如果没跑通也没关系，能保存证据、判断层级、提出下一步就算成功。

---

## 十三、收尾工作：保存结果并关机（非常重要！）

云服务器不会因为你关闭 VS Code、终端或浏览器而自动关机。必须回平台手动关机！

### 1. 保存必要内容

至少保留：
- hello_server.py
- 项目仓库地址和提交号
- 成功输出或错误日志
- 重要修改记录
- 实例配置（GPU 型号、显存、单价）

建议提交到自己的 Git 仓库或下载到本地，不要把云实例当唯一备份。

### 2. 退出远端终端

```bash
cd ~/day7-work/day7-system-check
git status --short 2>/dev/null || true
exit
```

如果 git status 有输出，说明有未提交修改，确认是否需要保存。

### 3. 在优云智算平台关机

1. 关闭 VS Code 的远程窗口（可以关掉整个 VS Code）
2. 回到优云智算实例列表
3. 点击"关机"
4. 等待状态变成"已关机"

必须以平台显示的"已关机"为准，不要用其他方式判断。

### 4. 计费说明

- 按量实例关机后，CPU、GPU、内存停止收费。
- 但磁盘、镜像等存储资源可能继续计费（通常很便宜）。
- 如果以后不再用，建议在备份后删除实例（避免磁盘持续扣费）。
- 如果还要用，记得关注余额和平台回收提醒。

---

## 最终成果清单（你可以自己打勾）

- [ ] 记录了实例型号、显存和按量单价
- [ ] 终端 SSH 登录成功
- [ ] SSH Key 登录成功
- [ ] VS Code Remote-SSH 或网页路线成功打开远端目录
- [ ] hello_server.py 运行成功
- [ ] 备用项目测试显示 OK
- [ ] 备用项目输出包含 "status": "success"
- [ ] 保存了仓库地址和提交号
- [ ] 完成了自由项目复现或错误分析
- [ ] 平台实例状态显示 "已关机"

---

## 常用命令速查表

| 操作 | 命令 |
|------|------|
| SSH 登录（以平台命令为准） | ssh -p 端口 root@外网IP |
| 查看系统信息 | whoami; hostname; pwd; python --version; nvidia-smi |
| 创建工作目录 | mkdir -p ~/day7-work && cd ~/day7-work |
| 克隆 GitHub 项目 | git clone <URL> |
| 查看 README | sed -n '1,180p' README.md |
| 查看依赖 | cat requirements.txt |
| 记录版本 | git rev-parse HEAD; git status --short |
| 运行测试 | python -m unittest discover -s tests -v |
| 运行程序 | python src/system_check.py --name 学号 |
| 查看错误日志尾部 | tail -n 20 日志文件 |
| 退出 SSH | exit |

---

## 参考链接

- 优云智算高校认证流程：https://www.ucloud.cn/
- 优云智算 Linux 快速开始：https://docs.ucloud.cn/
- VS Code Remote-SSH 官方文档：https://code.visualstudio.com/docs/remote/ssh
- GitHub 仓库搜索语法：https://docs.github.com/en/search-github
- GitHub 克隆仓库：https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository

---

