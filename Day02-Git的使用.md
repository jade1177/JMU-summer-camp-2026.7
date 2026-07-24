# Git 保姆级复盘

## 一、先回答核心问题：为什么感觉没怎么用到 Git，反而是 GitHub/Gitee 用得多？

**这是一个巨大的认知误区，也是很多人学 Git 时的陷阱。**
```text
┌─────────────────────────────────────────┐
│           GitHub / Gitee                │
│    （网站，远程仓库的"展示平台"）         │
│         你看得到、点得到的地方            │
└─────────────────────────────────────────┘
                    ▲
                    │ git push（上传）
                    │ git pull（下载）
                    ▼
┌───────────────────────────────────────── ┐
│              Git（命令行工具）            │
│    （你电脑里的"版本控制引擎"）            │
│         你看不到、但一直在后台工作         │
└─────────────────────────────────────────┘
``` 

**真相：你每一次在 GitHub/Gitee 上点"上传文件"、"同步"、"提交更改"，背后都是 Git 在干活。**

| 你在 GitHub/Gitee 上做的 | 背后 Git 在做什么 |
|----------------------|---------------|
| 上传文件到仓库 | `git add` + `git commit` + `git push` |
| 下载仓库到本地 | `git clone` 或 `git pull` |
| 创建分支 | `git branch` |
| 合并 Pull Request | `git merge` |
| 查看历史记录 | `git log` |

**为什么你感觉"没怎么用到 Git"？**

因为你用的是 **GitHub Desktop** 或**网页端**，它们把 Git 命令**包装成了按钮**。你点一下"Commit"，背后执行的是 `git commit`；你点一下"Push"，背后执行的是 `git push`。

**但这有个致命问题：** 当出错了（比如冲突、回退、分支乱了），图形界面帮不了你，你必须懂 Git 命令才能救场。

---

## 二、Git 核心概念：用"写小说"来理解

想象你在写一部长篇小说，Git 就是你的**"时光机 + 草稿管理系统"**。

### 1. 三个工作区（最核心！）

| Git 概念 | 小说类比 | 状态 |
|---------|---------|------|
| **工作区（Working Directory）** | 你桌面上摊开的稿纸，正在写的最新章节 | 红色 = 修改了但没保存 |
| **暂存区（Staging Area/Index）** | 你准备交给编辑的"一叠整理好的稿子" | 绿色 = 已整理，准备提交 |
| **本地仓库（Local Repository）** | 出版社的"存档室"，保存了小说的每一个历史版本 | 已提交，永久保存 |

**工作流程：**
```text
你在稿纸上写新章节（工作区）
↓
把写好的章节整理到一叠稿子里（git add → 暂存区）
↓
把整理好的稿子交给存档室保存（git commit → 本地仓库）
↓
把存档室的备份寄给总社（git push → 远程仓库 GitHub/Gitee）
```

### 2. 文件状态的变化
```text
    新创建文件              修改已有文件
          │                    │
          ▼                    ▼
    ┌──────────┐          ┌──────────┐
    │ 未跟踪    │          │ 未暂存    │
    │ untracked│          │ unstaged │
    └────┬─────┘          └────┬─────┘
         │    git add           │  git add
         └──────────┬──────────┘
                    ▼
              ┌──────────┐
              │ 已暂存    │
              │  staged  │
              └────┬─────┘
                   │ git commit
                   ▼
              ┌──────────┐
              │ 本地仓库   │
              │committed │
              └────┬─────┘
                   │ git push
                   ▼
              ┌──────────┐
              │ 远程仓库   │
              │  GitHub  │
              └──────────┘
```

**颜色含义：**
- **红色** = Git 还没跟踪这个文件，或者文件修改了但还没告诉 Git
- **绿色** = Git 已经知道这些改动，准备提交

---

## 三、Git 命令详解

### 3.1 配置 Git（一次性设置）

```bash
# 告诉 Git 你是谁（提交时会记录这个名字）
git config --global user.name "你的名字"

# 告诉 Git 你的邮箱
git config --global user.email "你的邮箱@example.com"

# 设置默认编辑器为 VS Code（可选）
git config --global core.editor "code --wait"
```

```bash
# 查看配置
git config --global user.name
git config --global user.email
```
为什么要配置？ 因为 Git 是分布式版本控制，每个人的电脑上都有完整仓库。Git 需要知道"这次提交是谁干的"。
### 3.2 个人操作指令（你每天都在用的）
#### 3.2.1 查看状态
```bash
git status
```
**作用： 看看当前文件夹里哪些文件被修改了、哪些是新文件、哪些在暂存区。**
---

**输出解读：**
```text
On branch master                    ← 当前在 master 分支

Changes not staged for commit:      ← 修改了但没暂存（红色）
  (use "git add <file>..." to update)
        modified:   test01.txt

Untracked files:                    ← 新文件，Git 还没跟踪（红色）
  (use "git add <file>..." to include)
        newfile.py

Changes to be committed:            ← 已暂存，准备提交（绿色）
  (use "git restore --staged <file>..." to unstage)
        new file:   test01.txt
```
#### 3.2.2 添加到暂存区
```bash
git add <文件名>        # 添加指定文件
git add .               # 添加所有修改（最常用）
```
**作用： 告诉 Git"这些改动我要保存"，把它们从工作区移到暂存区。**
**注意： git add 不是"上传到远程"，只是"准备提交到本地仓库"。**

#### 3.2.3 提交到本地仓库
```bash
git commit -m "提交说明"
```
**作用： 把暂存区的内容永久保存到本地仓库，生成一个"快照"。**
**提交说明怎么写？ 要描述清楚这次提交做了什么：**
```bash
git commit -m "添加用户登录功能"
git commit -m "修复登录按钮点击无响应的 bug"
git commit -m "更新 README，添加安装说明"
```
**不好的提交说明：**
```bash
git commit -m "改了一些东西"      # ❌ 太模糊
git commit -m "111"               # ❌ 无意义
```
#### 3.2.4 查看历史
```bash
git log                          # 详细历史
git log --oneline                # 简洁历史（一行一个提交）
git log --all --graph --oneline  # 图形化显示所有分支（最直观）
```
#### 3.2.5 版本回退（时光机！）
```bash
git reset --hard <commitID>      # 回退到指定版本
git reflog                       # 查看所有操作历史（包括已删除的）
```
**注意： git reset --hard 会删除回退点之后的所有提交，慎用！**

### 3.3 分支操作（多人协作核心）
#### 3.3.1 什么是分支？
**小说类比：**
```text
master/main 分支 = 小说的"正式出版版"，稳定、可阅读
dev 分支 = "草稿修改版"，你在上面尝试新剧情
feature 分支 = "某个角色的番外篇"，独立开发不影响主线
为什么要分支？
你在写第 10 章，同时编辑在改第 5 章的错别字。如果没有分支，你们互相覆盖，小说就乱了。
```
#### 3.3.2 分支命令
```bash
git branch                    # 查看所有分支，*表示当前分支
git branch <分支名>            # 创建新分支（不切换）
git checkout <分支名>          # 切换到已有分支
git checkout -b <分支名>       # 创建并切换（最常用）
git merge <分支名>             # 把指定分支合并到当前分支
git branch -d <分支名>         # 删除分支（安全删除）
git branch -D <分支名>         # 强制删除
```
#### 3.3.3 创建 dev-jmu 分支并合并（示例）
```bash
# 1. 创建并切换到 dev-jmu 分支
git checkout -b dev-jmu

# 2. 在 dev-jmu 分支上修改文件...

# 3. 提交修改
git add .
git commit -m "在 dev-jmu 分支上添加新功能"

# 4. 切换回 master 分支
git checkout master

# 5. 把 dev-jmu 的修改合并到 master
git merge dev-jmu

# 6. 删除 dev-jmu 分支（可选）
git branch -d dev-jmu
```
### 3.4 远程仓库操作（GitHub/Gitee）
#### 3.4.1 配置 SSH 密钥（免密码登录）
```bash
# 1. 生成密钥对（一路回车）
ssh-keygen -t rsa

# 2. 查看公钥内容
cat ~/.ssh/id_rsa.pub

# 3. 复制公钥内容，粘贴到 GitHub/Gitee 的 SSH 设置里
为什么要 SSH？ 不用每次 push 都输密码，而且更安全。
```
#### 3.4.2 关联远程仓库
```bash
# 添加远程仓库（origin 是别名，可以自定义）
git remote add origin git@github.com:你的用户名/仓库名.git

# 查看已关联的远程仓库
git remote -v
```
**命令拆解：**
```bash
git remote add origin git@github.com:jade1177/JMU-summer-camp-2026.7.git
│      │      │    │           │                    │
│      │      │    │           │                    └─ 仓库路径
│      │      │    │           └─ 服务器地址
│      │      │    └─ 你给远程仓库起的"昵称"（习惯叫 origin）
│      │      └─ "添加"
│      └─ "远程"
└─ Git 命令
```
#### 3.4.3 推送和拉取
```bash
# 第一次推送（-u 表示建立关联）
git push -u origin master

# 之后推送（已建立关联，直接 push）
git push

# 拉取远程更新
git pull

# 仅下载不合并
git fetch
push 和 pull 的区别：
git push = 把本地仓库的提交上传到远程仓库
git pull = 把远程仓库的更新下载并合并到本地
```
#### 3.4.4 克隆仓库
```bash
git clone <仓库地址>
```
**作用： 把远程仓库完整复制到本地，包括所有历史记录。**
### 3.5 冲突解决（必学！）
#### 3.5.1 冲突怎么产生的？
```plain
场景：你和队友同时修改了同一个文件的同一行

你（在 b2 分支）：把第 10 行改成"版本二"
队友（在 b3 分支）：把第 10 行改成"版本三"

合并时 Git 懵了：到底用哪个？
```
#### 3.5.2 冲突的样子
```Text
<<<<<<< HEAD          ← 当前分支的内容
版本二
=======               ← 分隔线
版本三
>>>>>>> b3            ← 另一个分支的内容
```
#### 3.5.3 解决冲突
```plain
打开冲突文件，找到 <<<<<<< 标记的地方
决定保留哪个版本，或者手动合并成新版本
删除所有标记符号（<<<<<<<、=======、>>>>>>>）
保存文件
重新 add 和 commit
```
```bash
git add .
git commit -m "解决 b2 与 b3 的冲突"
```
### 3.6 .gitignore（忽略文件）
**作用： 告诉 Git"这些文件不要跟踪"。**
**常见需要忽略的文件：**
**gitignore**
# 编译产生的文件
```text
*.exe
*.class
*.pyc

# 依赖文件夹（可以通过配置文件重新安装）
node_modules/
venv/
__pycache__/

# 敏感信息
.env
*.key

# IDE 配置
.vscode/
.idea/

# 日志和临时文件
*.log
*.tmp
```

**为什么要忽略？
减少仓库体积
保护敏感信息（密码、密钥）
避免提交个人 IDE 配置
不知道写什么？ 去 gitignore.io 输入你的技术栈，自动生成。**
## 四、Git vs GitHub/Gitee：彻底搞清楚
|           | Git       | GitHub/Gitee |
| --------- | --------- | ------------ |
| **是什么**   | 命令行工具     | 网站/平台        |
| **在哪运行**  | 你的电脑      | 远程服务器        |
| **核心功能**  | 版本控制      | 代码托管 + 协作    |
| **需要联网吗** | 不需要（本地操作） | 需要           |
| **关系**    | 底层引擎      | 上层应用         |
| **类比**    | Word 软件   | 百度网盘         |
---

**你感觉"用 GitHub 多"的原因：**
```plain
你用 GitHub Desktop（图形界面）代替了命令行
你做的是"上传下载"操作，这些在网页上也能做
你还没遇到需要命令行才能解决的复杂场景
但以下场景必须用 Git 命令：
解决合并冲突
回退到历史版本
整理提交历史（rebase）
批量操作文件
自动化脚本
```
## 五、你现在该做什么
### 5.1 立刻做：用命令行完成一次完整流程
```bash
# 1. 进入你的项目文件夹
cd /e/tool/git/test

# 2. 初始化仓库（如果还没初始化）
git init

# 3. 创建一个新文件
echo "Hello Git" > hello.txt

# 4. 查看状态（应该是红色 untracked）
git status

# 5. 添加到暂存区
git add hello.txt

# 6. 查看状态（应该是绿色 staged）
git status

# 7. 提交到本地仓库
git commit -m "添加 hello.txt 文件"

# 8. 查看提交历史
git log --oneline

# 9. 关联远程仓库（替换为你的仓库地址）
git remote add origin git@gitee.com:你的用户名/test.git

# 10. 推送到远程
git push -u origin master
```
### 5.2 每天训练：放弃 GitHub Desktop，用命令行
**接下来一周，所有 Git 操作都用命令行，包括：**
```text
提交代码
推送
拉取
创建分支
好处： 一周后你会对 Git 的命令流程烂熟于心，出错了也能自己排查。
```
## 六、产品经理视角

|场景	     |你需要懂的 Git 知识 |
|---------|----------------------|
|**工程师说"这个需求在 feature 分支上开发"**	|知道分支是并行开发，不影响主线|
|**工程师说"合并冲突了"**	|知道冲突是多人修改同一处，需要人工决策|
|**工程师说"回滚到上一个版本"**|知道 git reset 可以时光倒流|
|**看代码审查（Code Review）**	|知道 git diff 可以看改了什么|
|**评估项目进度**|	知道 git log 可以看提交频率和贡献者|
---
**你不需要成为 Git 专家，但你需要：**
**理解"工作区 - 暂存区 - 仓库"的流程**
```text
知道分支的作用
能看懂冲突标记
知道什么时候用 push/pull/clone

```
