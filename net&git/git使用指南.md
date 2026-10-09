---
tags: Instruction
---

# git 使用指南

> 在此之前确保你不是麻瓜

本篇适用于对于 git 零基础或者接近零基础的核子们进行食用：）

## 注册 github 账号

1. 提前将浏览器的默认语言设置为英语

2. 进入[github官网](https://github.com/)，会看到如下界面，选择 `sign up`

![](https://notes.sjtu.edu.cn/uploads/upload_ed67baba6ec5831be7621b74a1992c0c.png)

3. 填入你的**邮箱**（有条件的也可以先创建谷歌账号然后绑定谷歌账号），密码，用户名，地区选择**美国**，勾选不要勾，不然会收到推广，然后点击 `Create account`

![](https://notes.sjtu.edu.cn/uploads/upload_1ecd69575e7dbab7abc513fc3fba1b13.png)

4. 在邮箱收取验证码输入就能正常登录了

---

## 安装本机 git

### Linux 系统

在 Bash 中运行：
```bash
sudo apt update
sudo apt install git -y
```

安装后检查版本，运行：
```bash
git --version
```

输出应当类似于：
```
git version 2.43.0
```

### Windows 系统

在 PowerShell 中执行：
```powershell
winget install --id Git.Git -e
```

安装后检查版本，运行：
```powershell
git --version
```

输出应当类似于：
```
git version 2.43.0
```


---

## git 的三个文件区

git 中你一共管理了三个文件区，分别是工作区(working directory)、暂存区(staging area)和提交区(repository)

- **工作区** 就是你修改各个文件的位置，所有的修改操作都在这里进行
- **暂存区** 是一个过渡区，一次提交可能会修改很多的文件，所以你可以把要提交且已修改好的文件先放入暂存区，在全部修改好之后统一提交
- **提交区** 就是所有的已提交文件版本，可以查看所有的历史提交

---

## 基础 git 命令

在 vscode / Clion 中，这些命令全部已经被可视化，你可以根据下面提到的命令行行为来匹配可视化页面中的操作，快速上手 git

### 1. 设置个人身份

在 Linux Bash / Windows PowerShell 中，先要告诉系统你是谁，用以下命令设置全局的身份：
```bash
git config --global user.email "[your_email@123456.com]"
git config --global user.name "[your_name]"
```

其中的邮箱和用户名换成真实的邮箱和用户名即可

> 为了方便描述，以下说的"执行命令"都**同时允许在 Linux Bash 和 Windows PowerShell** 中运行

### 2. 初始化仓库

- 如果你使用的是 vscode 可视化操作，则不需要提前建立空仓库

先在 github 上新建一个仓库，根据需求选择 private / public，如下图所示

![](https://notes.sjtu.edu.cn/uploads/upload_8d0c30eaf077dc451041ab9e93771f9c.png)

![](https://notes.sjtu.edu.cn/uploads/upload_c326c964c730f6e7881f10a1905a7065.png)

然后在这里获取你的仓库地址

![](https://notes.sjtu.edu.cn/uploads/upload_01230c52b76da7403be5c2b2b3736cbc.png)

接下来打开终端，执行以下命令：
```bash
cd working_area                 # 进入工作区根目录
git init                        # 初始化 git 仓库
git add .                       # 暂存所有文件（也可以根据需要暂存部分文件）
git commit -m "initial commit"  # 创建初始提交
git branch -M main              # 创建第一条分支 main

git remote add origin [git_URL] # 创建远程仓库 用真实仓库地址替换 [git_URL]
git push -u origin main         # 首次推送至最初远程
```

可能你暂时不明白为什么要这么操作，我们接下来就会在各个功能中逐渐理解所有的指令

> 为了方便描述，以下说的"执行命令"都指的是在**工作区根目录下**

### 3. 创建新提交

当你修改了一些文件，需要创建一版新提交时，可以执行以下命令：
```bash
git add [filenames]           # 把想提交的文件暂存，用文件名替换 [filenames]
git add [filenames]           # 可以多次 add 文件
git commit -m "one commit"    # 提交本次修改
git push                      # 推送到远程
```

- 对于所有的 commit，git 都会生成一个 commit_ID，它是这次 commit 序列化后的哈希值，是一个 40 位的 16 进制数，这个 commit_ID 可以唯一标识一个 commit
- 若一个 commit 的 ID 为 `61f88b5956b552dca239c065782ede3077b1cf18` 你可以用任意长度的前缀（比如 `61f88b` ）来标识这个 commit，前提该前缀与该分支的任意其他 commit 的 ID 前缀冲突

若你想独特地标识一个版本的提交，可以执行以下指令：
```bash
# 标记某个提交，用真实标签名和真实 commit_ID 替换 [tagname] 和 [revision]
git tag [tagname] [revision]  
```

> 为了方便描述，以下命令中的 `[revision]` 都指的是 commit_ID 任意长度不冲突的前缀或人为指定的 tagname

### 4. 回退到历史版本

当你认为新的修改不好，或者想要查看历史版本，可以执行以下命令：

==⚠ **此命令会改动工作区，请确保没有未提交的改动再操作**==
```bash
# 切换到某次提交
git checkout [revision]       
```

==⚠ **回退后请不要直接修改文件并创建提交，若要执行这个操作请先根据【5.】方法创建新分支**==

### 5. 创建新分支

当你想要在某一个提交开始多人合作并行，或者想从旧版本重新编写，或者分别管理两部分的修改，可以先 `checkout` 到该版本，然后执行以下命令：
```bash
# 在当前节点开始新建分支，用真实分支名替换 [branchname]
git branch [branchname]       
```

该命令不会主动帮你切换到新分支，需要使用以下命令在分支间切换：

==⚠ **此命令会改动工作区，请确保没有未提交的改动再操作**==
```bash
# 切换到对应分支的最新提交，用真实分支名替换 [branchname]
git checkout [branchname]     
```

切换后可以正常进行 add、commit、push 等操作，都会作用在当前选中的分支上

### 6. 合并两个分支

当多个 branch 并行时，我们可能最终要把他们的修改合并，此时我们可以执行下面的命令：

==⚠ **此命令会改动工作区，请确保没有未提交的改动再操作**==
```bash
# 确保当前处于主分支，将副分支合并到主分支，用副分支名替换 [sub_branchname]
git merge [sub_branchname]
```

但是合并过程可能有冲突，即一个版本在两边都被修改了，且修改不同，此时你需要解决这个 conflict 才可以完成合并操作

---

## git 的特殊文件

- `README.md`
作为说明性文档，其中的内容会在 github 中该项目的主页直接预览显示，一般会写项目说明，编译配置方法和署名等
- `.gitignore`
作为功能性文档，告诉 git 哪些文件只出现在本地不想要被版本管理（比如 API 配置、环境变量、测试文件、编译产物等），只需要在该文件中逐项列出，或用部分正则表达式语法批量表示，就可以全部忽略（不知道正则表达式是什么可以看[正则表达式简明教程](https://notes.sjtu.edu.cn/0WWlVGOVRemjLyD_1ufXxg)）

---

## Acknowledgement

以上原创编写内容覆盖平时用得到的 99.999% 的 git 用法，如果愿意看非常详细的 git 说明和底层逻辑，欢迎转阅 [git傻瓜教程](https://notes.sjtu.edu.cn/SG0OCEhHRCqcsE30CToLxA) by *PhantomPhoenix*

> Edit: _serendipity