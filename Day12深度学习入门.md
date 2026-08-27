# Day 8 - 深度学习入门：从原理到实战（小白学习笔记）

## 📌 今天学了什么？

今天是深度学习系列的第二课，作为一个纯小白，我之前对“深度学习”的理解就是“很厉害的东西，能让AI画画、聊天”。但今天学完后，我终于搞清楚了它到底是怎么工作的——从数据怎么来、模型怎么选、到代码怎么写，走了一遍完整的流程。

---

## 一、深度学习全景图

### 1.1 什么是深度学习？

先理清几个概念的关系：

**人工智能（AI）** → **机器学习（ML）** → **深度学习（DL）**

深度学习是机器学习的一个子集，核心特点是：**通过多层神经网络，自动从数据中学习特征和模式**。

> 🧠 **我的理解**：传统编程是我们告诉计算机“规则是什么”，而深度学习是给计算机“看大量例子”，让它自己总结出规则。就像教小孩认识猫——你不用告诉他“猫有尖耳朵、胡须、尾巴”，只需要给他看很多猫的图片，他自己就能总结出“猫长什么样”。

### 1.2 一个完整项目的标准流程

任何一个深度学习项目，都遵循以下6个步骤：

| 阶段 | 核心任务 | 小白翻译 |
|------|----------|----------|
| ① 数据采集 | 获取训练数据 | 找“学习资料”（图片、文本等） |
| ② 数据预处理 | 清洗、标注、增强 | 把资料整理好，让模型看得懂 |
| ③ 模型选择 | 根据任务选模型架构 | 选一个适合这个任务的“大脑结构” |
| ④ 模型训练 | 让模型从数据中学习 | 让“大脑”反复看资料，自己总结规律 |
| ⑤ 调参与优化 | 调整超参数提升效果 | 微调“学习方式”，让效果更好 |
| ⑥ 测试评估 | 用新数据评估模型 | 考考它——没见过的题会不会做？ |

### 1.3 深度学习的三大核心任务

几乎所有深度学习应用都可以归入以下三类：

| 任务类型 | 定义 | 输入→输出示例 | 典型应用 |
|----------|------|---------------|----------|
| **分类** | 打一个离散标签 | 图片→“猫”/“狗” | 手写识别、疾病诊断 |
| **回归** | 预测一个连续数值 | 房屋特征→价格（万） | 房价预测、温度预测 |
| **生成** | 创造全新内容 | 文字描述→图片 | AI绘画、ChatGPT |

> 💡 **分类 vs 回归的区别**：分类是“定性”（猫还是狗？），回归是“定量”（值多少钱？）。

---

## 二、深度学习模型家族

### 2.1 模型速览表

| 模型 | 全称 | 提出年份 | 擅长领域 | 一句话理解 |
|------|------|----------|----------|------------|
| **MLP** | 多层感知机 | 1986 | 结构化数据、小规模图像 | 全连接网络，每个神经元都和上一层所有神经元连接 |
| **CNN** | 卷积神经网络 | 1998 | 图像分类、目标检测 | 滑动窗口提取局部特征，考虑空间位置 |
| **RNN/LSTM** | 循环神经网络 | 1990s | 序列数据（文本、语音） | 有“记忆”的网络，能处理时间序列 |
| **Transformer** | 变换器 | 2017 | NLP、CV | 自注意力机制，捕获全局依赖 |

### 2.2 模型选择指南

> 🧠 **我的理解**：选模型就像选工具——锤子钉钉子，螺丝刀拧螺丝。图像任务用CNN，文本任务用RNN或Transformer，结构化数据用MLP。

### 2.3 各模型核心原理（小白版）

**MLP（多层感知机）**：
- 结构：输入层 → 隐藏层 → 输出层（每一层都“全连接”）
- 特点：结构简单，但不能处理空间信息
- 例子：房价预测（输入房子面积、房间数，输出价格）

**CNN（卷积神经网络）**——重点掌握：
- 核心操作：卷积（提取局部特征）+ 池化（压缩尺寸）
- 特点：权重共享，参数少，平移不变性
- 例子：图像分类、人脸识别

**RNN/LSTM**：
- 核心思想：有“记忆”，信息可以在时间步之间传递
- LSTM vs GRU：LSTM更复杂但能力强，GRU更轻量更快
- 例子：机器翻译、语音识别

**Transformer**：
- 核心思想：自注意力机制（Self-Attention）
- 特点：并行计算，能捕获长距离依赖
- 例子：GPT、BERT

---

## 三、卷积神经网络（CNN）详解

这是今天的重点，我花了一些时间才真正理解。

### 3.1 为什么需要CNN？

**MLP处理图像的问题**：

一张 32×32×3 的彩色图片，有 3072 个像素。如果输入MLP，需要把所有像素“拉直”成一维向量，导致：
1. ❌ **丢失空间结构**：像素之间的位置关系被破坏
2. ❌ **参数爆炸**：3072 × 神经元数，参数极其庞大
3. ❌ **过拟合**：参数太多，模型容易“死记硬背”

**CNN的解决方案**：用“卷积核”在图片上滑动，每次只看一个局部区域。

> 🧠 **我的理解**：MLP看图片就像把一整幅画拆成一个个独立的色块，分不清哪个色块在哪个位置；CNN看图片就像用手电筒在画上照来照去，每次只看一小块，但能看到这块在整幅画的哪个位置。

### 3.2 CNN的三大核心操作

#### ① 卷积层（Convolution Layer）

**直观理解**：
想象你用一个手电筒在一张照片上照来照去。每次只照亮一个小方块（比如 3×3 像素），你记录下这个小方块里的图案特征。当手电筒扫过整张照片后，你就得到了一张“特征图”——标记了照片上每个位置有什么样的图案。

**数学本质**：
- 卷积核（Filter）是一个小矩阵（如 3×3）
- 它在输入图像上滑动，每次计算对应位置的点积
- 每个卷积核提取一种特征（边缘、纹理、颜色等）

**关键参数**：

| 参数 | 含义 | 典型值 |
|------|------|--------|
| Kernel Size | 卷积核尺寸 | 3×3, 5×5 |
| Stride | 滑动步长 | 1, 2 |
| Padding | 边缘填充 | 0, 1 |
| Channels | 卷积核数量 | 32, 64, 128 |

#### ② 池化层（Pooling Layer）

**直观理解**：
把一张大图缩小成小图，但保留最重要的信息。比如一张 100×100 的猫图，缩小成 50×50 后，我们仍然能认出是猫。

**作用**：
1. 降低特征图尺寸（减少计算量）
2. 提取主要特征（保留最显著的信息）
3. 防止过拟合（减少参数）

**Max Pooling 示例**（2×2 窗口，步长2）：
在 2×2 的小方块里，只保留**最大的那个值**，其他扔掉。

#### ③ 权重共享（Weight Sharing）

**核心思想**：同一个卷积核在整张图像的所有位置滑动时，**参数是固定不变的**。

**好处**：
1. 大幅减少参数量：MLP需要数百万参数，CNN只需要几千到几万
2. 平移不变性：无论物体出现在图像的哪个位置，都能被检测到
3. 泛化能力强：不容易过拟合

### 3.3 CNN的经典架构
```text
输入图像（32×32×3）
↓
[卷积层] → 提取局部特征（边缘、纹理）
↓
[ReLU激活函数] → 引入非线性
↓
[池化层] → 压缩特征图尺寸
↓
[卷积层] → 提取更高层特征
↓
[池化层] → 再次压缩
↓
[全连接层] → 综合所有特征
↓
[输出层] → 分类结果（10类）
```

### 3.4 感受野（Receptive Field）

**定义**：输出特征图上的一个像素，对应输入图像上的多大区域。

> 🧠 **我的理解**：
> - 浅层卷积层：感受野小 → 只能看到局部（边缘、纹理）
> - 深层卷积层：感受野大 → 能看到全局（物体形状、语义）
> 
> 这就是CNN能够从“看到边缘”到“识别物体”的本质原因——层层递进，从局部到整体。

---

## 四、PyTorch框架入门

### 4.1 为什么用PyTorch？

| 特性 | 说明 |
|------|------|
| 动态计算图 | 调试方便，灵活性高 |
| Pythonic语法 | 和NumPy类似，上手快 |
| 自动求导 | 不用手动计算梯度 |
| 生态完善 | torchvision、HuggingFace等 |

### 4.2 PyTorch核心组件

```python
import torch                     # 核心张量库（GPU加速）
import torch.nn as nn            # 神经网络模块
import torch.optim as optim      # 优化器
import torchvision               # CV相关工具
from torch.utils.data import DataLoader  # 数据加载器
```

|组件|	作用|
|------|------|
|torch.Tensor	|多维数组（可在GPU上运行）|
|nn.Module	|所有模型的基类|
|optim|	参数优化算法（如Adam）|
|DataLoader	|批量加载数据|
|nn.CrossEntropyLoss	|多分类损失函数|
### 4.3 模型训练标准流程（五步法）
# Step 1: 定义模型
model = SimpleCNN()

# Step 2: 定义损失函数和优化器
criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)

# Step 3: 训练循环
for epoch in range(10):
    for images, labels in train_loader:
        # 前向传播：数据 → 模型 → 预测结果
        outputs = model(images)
        loss = criterion(outputs, labels)
        
        # 反向传播：计算梯度 → 更新参数
        optimizer.zero_grad()   # 清空之前的梯度
        loss.backward()         # 反向传播计算梯度
        optimizer.step()        # 更新参数

# Step 4: 评估（不需要计算梯度）
with torch.no_grad():
    correct = (model(test_images).argmax(1) == test_labels).sum()

# Step 5: 保存模型
torch.save(model.state_dict(), 'model.pt')
---
**🧠 我的理解：**
- 前向传播：数据从输入层流向输出层，得到预测结果

- 损失函数：计算“预测值”和“真实值”之间的差距

- 反向传播：从输出层往回传，计算每个参数对“损失”的贡献

- 优化器：根据梯度调整参数，让损失越来越小
## 五、CIFAR-10 实战代码
### 5.1 数据集说明
**CIFAR-10 是计算机视觉领域最经典的入门数据集之一。**

|属性|	说明|
|------|-------|
|类别数|	10类（飞机、汽车、鸟、猫、鹿、狗、蛙、马、船、卡车）|
|训练集	|50000张|
|测试集|	10000张|
|图像尺寸|	32×32像素（RGB彩色）|
### 5.2 完整代码
```text
import torch
import torch.nn as nn
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms
from torch.utils.data import DataLoader
import matplotlib.pyplot as plt

# ====== 1. 数据加载 ======

# 数据预处理：转张量 + 归一化
transform = transforms.Compose([
    transforms.ToTensor(),  # 把图片转成PyTorch张量（0-255 → 0-1）
    transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))  # 归一化到[-1,1]
])

# 下载训练集和测试集
train_dataset = torchvision.datasets.CIFAR10(
    root='./data', train=True, download=True, transform=transform
)
test_dataset = torchvision.datasets.CIFAR10(
    root='./data', train=False, download=True, transform=transform
)

# 数据加载器：批量加载数据
train_loader = DataLoader(train_dataset, batch_size=64, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=64, shuffle=False)

# ====== 2. 定义模型 ======

class SimpleCNN(nn.Module):
    def __init__(self):
        super(SimpleCNN, self).__init__()
        
        # 第一个卷积块：输入3通道（RGB），输出32通道
        self.conv1 = nn.Conv2d(3, 32, kernel_size=3, padding=1)
        self.relu1 = nn.ReLU()           # 激活函数
        self.pool1 = nn.MaxPool2d(2, 2)  # 池化：压缩尺寸
        
        # 第二个卷积块：输入32通道，输出64通道
        self.conv2 = nn.Conv2d(32, 64, kernel_size=3, padding=1)
        self.relu2 = nn.ReLU()
        self.pool2 = nn.MaxPool2d(2, 2)
        
        # 全连接层：综合特征 → 分类
        self.fc1 = nn.Linear(64 * 8 * 8, 512)  # 64通道，8×8的特征图
        self.relu3 = nn.ReLU()
        self.fc2 = nn.Linear(512, 10)          # 10个分类输出
        
    def forward(self, x):
        x = self.pool1(self.relu1(self.conv1(x)))
        x = self.pool2(self.relu2(self.conv2(x)))
        x = x.view(-1, 64 * 8 * 8)  # 展平，准备输入全连接层
        x = self.relu3(self.fc1(x))
        x = self.fc2(x)
        return x

model = SimpleCNN()

# ====== 3. 训练配置 ======

criterion = nn.CrossEntropyLoss()          # 交叉熵损失（多分类）
optimizer = optim.Adam(model.parameters(), lr=0.001)  # Adam优化器

# ====== 4. 训练循环 ======

epochs = 10
train_losses = []
test_accuracies = []

for epoch in range(epochs):
    # 训练阶段
    model.train()
    running_loss = 0.0
    for images, labels in train_loader:
        optimizer.zero_grad()        # 清空梯度
        outputs = model(images)      # 前向传播
        loss = criterion(outputs, labels)  # 计算损失
        loss.backward()              # 反向传播
        optimizer.step()             # 更新参数
        running_loss += loss.item()
    
    avg_loss = running_loss / len(train_loader)
    train_losses.append(avg_loss)
    
    # 测试阶段
    model.eval()
    correct = 0
    total = 0
    with torch.no_grad():  # 不计算梯度（节省内存）
        for images, labels in test_loader:
            outputs = model(images)
            _, predicted = torch.max(outputs, 1)  # 取概率最大的类别
            total += labels.size(0)
            correct += (predicted == labels).sum().item()
    
    accuracy = 100 * correct / total
    test_accuracies.append(accuracy)
    
    print(f'Epoch [{epoch+1}/{epochs}] Loss: {avg_loss:.4f}, Accuracy: {accuracy:.2f}%')

# ====== 5. 可视化 ======

plt.figure(figsize=(12, 4))

plt.subplot(1, 2, 1)
plt.plot(train_losses, label='Training Loss')
plt.xlabel('Epoch')
plt.ylabel('Loss')
plt.legend()
plt.title('损失曲线')

plt.subplot(1, 2, 2)
plt.plot(test_accuracies, label='Test Accuracy')
plt.xlabel('Epoch')
plt.ylabel('Accuracy (%)')
plt.legend()
plt.title('准确率曲线')

plt.show()

# ====== 6. 保存模型 ======

torch.save(model.state_dict(), 'cifar10_model.pth')
print("模型已保存为 cifar10_model.pth")
```
### 5.3 预期运行结果
|Epoch	|训练损失|	测试准确率|
|------|------|-------|
|1	|~1.50	|~50%|
|5|	~0.70|	~65%|
|10	|~0.45	|~70%|
