# 系统监控工具

## 一、 基础信息

- **小组编号/名称**：

- **小组成员及分工**：

    - 吕思瑶（Git 协调员）：仓库初始化、PR流程管理、冲突解决

    - 张语淇（环境工程师\-核心逻辑）：monitor\.py开发、功能测试

    - 华佳怡（环境工程师\-依赖管理）：Conda环境配置、environment\.yml精简

    - 林辰祎（容器架构师\-构建）：Dockerfile编写、镜像构建测试

    - 陈家怡（文档编写师）：编写命令、编写 README 、编写飞书文档

## 二、 协作过程记录 \(Git \& 飞书\)

- **Git 仓库链接**：https://gitee\.com/lxxin1121/lab\-monitor

- **协调证明**

![image\.png](图片和附件/image.png)

![image\.png](图片和附件/image%201.png)

![image\.png](图片和附件/image%202.png)

## 三、 环境标准化实现

请详细描述你们组是如何保证“环境可复现”的：

1. **Conda 方案**：\(VMware ubuntu 内的 conda\)

    - 展示 `environment.yml` 的核心内容。

    ```YAML
    name: monitor-env
    dependencies:
      - python=3.9
      - psutil
    ```

    描述：如何使用一行命令还原环境：

    ```Bash
    conda env create -f environment.yml && conda run -n monitor-env python monitor.py
    ```

2. **Docker 方案**：

    - 展示 `Dockerfile` 内容。

    ```Dockerfile
    # 基础python镜像
    **FROM** python:3.9-slim
    # 容器工作目录
    **WORKDIR** /app
    # 日志持久化卷
    **VOLUME** ["/app/logs"]
    # 拷贝依赖文件
    **COPY** environment.yml .
    # 安装psutil依赖
    **RUN **pip install --no-cache-dir psutil -i https://pypi.tuna.tsinghua.edu.cn/simple
    # 拷贝监控代码
    **COPY** monitor.py .
    # 容器启动命令
    **CMD** ["python","monitor.py"]
    ```

    - **难点说明**：描述你们是如何处理 `logs` 文件夹挂载问题的（即如何保证容器删了日志还在）。

    ```PowerShell
    docker run --rm -v "$($PWD.Path)\logs:/app/logs" lab-monitor:v1.0
    ```

    命令将 Windows 项目根目录下的`logs`文件夹挂载到容器的 `/app/logs` 中

    因此，程序写入容器中的 `/app/logs/system_stats.txt` 时，文件会保存在 Windows 项目目录的 `logs/system_stats.txt` 中。使用 `--rm` 删除容器不会删除宿主机日志。

## 四、 运行指南 

假设我是一名新人，我该如何运行你们的项目？（**这是评估文档质量的核心指标**）

- **步骤 1**：如何 Clone 仓库。

    1. 在VMware Ubuntu里边获取项目

    打开 Ubuntu Bash终端，执行：

    ```Bash
    mkdir -p ~/Lab  # 创建存放所有项目的文件夹
    cd ~/Lab   # 进入该文件夹
    git clone https://gitee.com/lxxin1121/lab-monitor.git # 下载本次项目文件
    cd lab-monitor # 进入本次项目文件的文件夹
    ```

    2. 在 Windows 中获取项目

    打开 Windows PowerShell 执行以下命令

    注：第一个`Set-Location "......"` 的双引号中填你希望项目保存的位置，并且填入的路径已经存在

    以下为示例

    ```PowerShell
    Set-Location "D:\File\DockerFile\DockerWSL\Projects" #进入存放项目的对应文件夹
    git clone https://gitee.com/lxxin1121/lab-monitor.git # 下载本次项目文件
    Set-Location .\lab-monitor #进入项目根目录
    ```

- **步骤 2**：如何通过 Docker 一键启动（给出具体的 `docker run` 命令，包含挂载参数）。

    1. 构建项目镜像

    第一次运行前，在项目根目录构建镜像：

    ```PowerShell
    docker build -t lab-monitor:v1.0 .
    ```

    构建成功后镜像名称和标签如下

    - 镜像名称：lab\-monitor

    - 标签：v1\.0

    2. 容器启动命令（形成挂载）

    ```PowerShell
    docker run --rm -v "$($PWD.Path)\logs:/app/logs" lab-monitor:v1.0
    ```

- **步骤 3**：如何确认程序运行成功？（描述预期输出或日志位置）。

    终端应显示五条记录

    ```Plain Text
    --- 实验室系统监控启动 ---
    记录成功: Sun Jul 19 06:15:34 2026: CPU 0.1%, MEM 5.6%
    --- 实验室系统监控启动 ---
    记录成功: Sun Jul 19 06:15:36 2026: CPU 0.0%, MEM 5.6%
    --- 实验室系统监控启动 ---
    记录成功: Sun Jul 19 06:15:38 2026: CPU 0.0%, MEM 5.6%
    --- 实验室系统监控启动 ---
    记录成功: Sun Jul 19 06:15:40 2026: CPU 0.0%, MEM 5.6%
    --- 实验室系统监控启动 ---
    记录成功: Sun Jul 19 06:15:42 2026: CPU 0.0%, MEM 5.7%
    ```

## 五、 Error\-Solution 知识库 

#### add/add 类型合并冲突

- **问题描述**：CONFLICT \(add/add\): Merge conflict in environment\.yml

- **产生原因**：同一个文件在两个没有共同历史的分支上都被独立添加

- **解决办法**：

    A（保留组员代码）：git checkout \-\-theirs 文件名 保留 master 上的版本

    B（手动编辑）：打开文件删除冲突标记（\<\<\<\<\<\<\<、=======、\>\>\>\>\>\>\>），选择需要保留的内容

    解决后操作：git add 冲突文件 → git commit \-m "merge: resolve conflicts" → git push origin 分支名



#### psutil依赖缺失报错修复

- **问题描述**：ModuleNotFoundError: No module named 'psutil'

- **产生原因**：在初始环境中运行 monitor\.py 时，Python 标准库中不包含 psutil 这个第三方库，导致代码无法调用 CPU 和内存监控接口。

- **解决办法**：使用 Conda 进行安装：conda install psutil。安装完成后重新运行脚本，确认日志文件生成成功。建议在 environment\.yml 中显式声明该依赖，防止他人复现时再次报错。



#### 分支合并覆盖文件丢失恢复处理

- **问题描述**：在将 dev\-linchenyi 分支合并到 master 后，发现 master 上原有的完整代码（Dockerfile、environment\.yml、monitor\.py）全部被 linchenyi 分支的版本覆盖，同时 logs/system\_stats\.txt 文件也在后续操作中丢失。

- **产生原因**：

    1. 合并时使用了 `git checkout --theirs` 策略，该策略保留的是远程分支的版本，导致 master 原有的代码被覆盖

    2. 为了撤销合并，使用了 Gitee 的"回退"功能，回退 PR 合并后 master 上的 logs 文件被删除

    3. 在本地 dev\-linchenyi 分支上执行 `git merge origin/master --allow-unrelated-histories` 时，由于是 fast\-forward 合并，回退后 master 删除 logs 的操作也同步到了 dev\-linchenyi 分支

    4. 执行 `git push` 后，远程分支上的 logs 也被删除

- **解决办法**： 

    1. 在 Gitee 上使用"回退"功能撤销错误的合并 PR

    2. 使用 `git checkout --ours`（而非 `--theirs`）保留 master 的代码版本

    3. 通过 `git log` 查找 linchenyi 原始提交的 commit hash（`5427d10`）

    4. 用 `git checkout 5427d10 -- logs/` 从历史提交中恢复 logs 文件

    5. 重新提交并推送：`git add logs/ && git commit -m "feat: restore logs" && git push`

    6. 在 Gitee 上重新创建 PR 并合并

## 六、 运行结果展示

- 插入一张在 **宿主机（Windows/WSL2）** 目录下成功查看到 `logs/system_stats.txt` 内容的截图。

![\{02686D4A\-F1DC\-467C\-B2EB\-62F9AE38BEBD\}\.png](图片和附件/{02686D4A-F1DC-467C-B2EB-62F9AE38BEBD}.png)



