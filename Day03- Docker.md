# Day 3 - 构建你的第一个“集装箱”项目（Docker 环境打包实战）

## 📌 任务目标
通过 Docker 将一个 Python 数据处理脚本及其运行环境完整打包，解决“在我电脑上能跑，在别人电脑上不行”的环境一致性问题。

---

## 🛠 环境准备
- **宿主机**：Windows 10/11（已开启 WSL2 和 BIOS 虚拟化）
- **开发环境**：WSL2（Ubuntu） + VS Code
- **核心工具**：Docker Desktop

### ⚙️ 关键检查项
1. **WSL 集成**：  
   Docker Desktop → Settings → Resources → WSL Integration，确保正在使用的 Ubuntu 发行版开关为 **ON**。
2. **镜像加速**（国内用户必配）：  
   在 Docker Engine 配置中添加国内镜像源（如阿里云、中科大），避免拉取基础镜像超时。

---

## 📁项目文件结构
```text 
my-docker-project/
├── main.py # Python 脚本（使用 NumPy）
├── requirements.txt # 依赖清单
├── Dockerfile # 镜像构建指令
└── screenshot.png # 运行成功截图
```


---

## 📄 文件内容详情

### 1. `main.py`
```python
import numpy as np
import sys

def run():
    print("--- 欢迎来到实验室 Docker 练习 ---")
    print(f"当前 Python 版本：{sys.version}")
    data = np.random.rand(3, 3)
    print("生成的 3x3 随机矩阵：")
    print(data)
    print("--- 运行成功！环境复现完成 ---")

if __name__ == "__main__":
    run()
```

### 2. requirements.txt
```text
numpy
```
### 3. Dockerfile
```text
# 1. 使用官方轻量级 Python 镜像作为基础
FROM python:3.9-slim

# 2. 设置容器内工作目录
WORKDIR /app

# 3. 拷贝依赖清单到容器
COPY requirements.txt .

# 4. 安装依赖（使用清华镜像加速）
RUN pip install --no-cache-dir -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

# 5. 拷贝所有源码到容器
COPY . .

# 6. 容器启动时执行的命令
CMD ["python", "main.py"]
```
## 🚀 构建与运行步骤
### 1️⃣ 构建镜像
#### 在 WSL2 终端中，进入项目目录并执行：
```text
docker build -t lab-app:v1.0 .
```
### 2️⃣ 查看镜像
```bash
docker images
```
### 3️⃣ 运行容器
```bash
docker run --rm -it lab-app:v1.0
```
- -rm：容器退出后自动删除，保持系统整洁。

- -it：分配交互式终端，便于查看输出。
## ✅ 运行结果
```text
--- 欢迎来到实验室 Docker 练习 ---
当前 Python 版本：3.9.25 (main, Oct 31 2025, 23:16:49) [GCC 14.2.0]
生成的 3x3 随机矩阵：
[[0.38687711 0.97849678 0.45075188]
 [0.90683081 0.46404514 0.49777763]
 [0.88269765 0.48303991 0.60557619]]
--- 运行成功！环境复现完成 ---
```
