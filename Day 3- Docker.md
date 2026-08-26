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

## 📁 项目文件结构
``text 
my-docker-project/
├── main.py # Python 脚本（使用 NumPy）
├── requirements.txt # 依赖清单
├── Dockerfile # 镜像构建指令
└── screenshot.png # 运行成功截图
