# Docker 入门指南

这份指南专门针对前端视角，涵盖了 **核心概念（用前端听得懂的话解释）**、**使用场景** 以及 **保姆级的安装教程**。

---

## 1. Docker 到底是干什么用的？

简单来说，Docker 就是为了解决 **“在我的机器上能跑，在你的机器/服务器上就报错”** 这个问题而生的。

### 📦 核心类比
你可以把 Docker 想象成一个超级轻量级的 **虚拟机**，但它比虚拟机快得多。

*   **以前**：你发给运维/后端的是一份源代码（代码 + `package.json`）。对方需要自己安装 Node.js、配置环境变量、安装依赖，只要版本不对，代码就跑不起来。
*   **现在 (Docker)**：你发给对方的是一个 **集装箱**（镜像）。这个箱子里已经装好了代码、Node.js 环境、操作系统配置。对方只需要把箱子拿到他们的机器上运行，效果和你本地一模一样。

### 前端三大核心概念（映射记忆）

| 概念 | 前端类比 | 解释 |
| :--- | :--- | :--- |
| **镜像 (Image)** | `Class` (类) 或 **安装包** | 一个只读的模板。比如一个 "Nginx 镜像"，里面就包含了 Nginx 程序和它的运行环境。 |
| **容器 (Container)** | `Instance` (实例) 或 **运行中的 App** | 镜像是静态的，容器是动态的。你通过 `new Image()` (运行镜像) 就得到了一个容器。你可以同时运行 10 个相同的 Nginx 容器，它们互不干扰。 |
| **Dockerfile** | `package.json` + `webpack config` | 一个文本文件，告诉 Docker 怎么把你的代码打包成镜像（比如：基于 Node 18 -> 复制文件 -> 运行 npm install -> 暴露 3000 端口）。 |

---

## 2. 前端为什么要学 Docker？

除了部署，它对我们本地开发非常有用：

*   **⚡️ 瞬间拥有后端服务**  
    前端开发需要依赖 Redis、MySQL 或者特定版本的后端 API。在 Windows 上安装这些数据库非常麻烦且容易留垃圾。用 Docker？一行命令 `docker run -d -p 6379:6379 redis`，你的电脑上就有了 Redis，用完删掉容器，电脑干干净净。

*   **🛠 多版本 Node 环境**  
    老项目需要 Node 12，新项目要 Node 20。虽然有 nvm，但用 Docker 可以更彻底地隔离环境。

*   **🚀 模拟线上环境**  
    本地是 Windows，服务器是 Linux。用 Docker 可以在本地完美模拟 Linux 环境，避免部署上线后出现路径大小写敏感等奇怪 Bug。

---

## 3. 下载与安装 (Windows 版)

Docker 在 Windows 上的体验现在已经非常好了（推荐使用 WSL 2 模式）。

### 📥 步骤 1：下载
*   **官方下载地址**：[Docker Desktop for Windows](https://docs.docker.com/desktop/setup/install/windows-install/)
*   点击页面上的大蓝按钮 **"Docker Desktop for Windows - x86_64"** 下载安装包。

### 💿 步骤 2：安装
1.  双击下载好的 `Docker Desktop Installer.exe`。
2.  **重要配置**：在安装界面，务必勾选 **"Use WSL 2 instead of Hyper-V"**（通常默认就是勾选的）。
    *   *WSL 2 是微软出的“Windows 子系统 Linux”，性能比老架构快得多。*
3.  点击 "Ok"，等待安装完成。
4.  安装完成后，点击 "Close" 并 **重启电脑**。

### ⚙️ 步骤 3：首次启动
1.  重启后，双击桌面的 **Docker Desktop** 图标。
2.  它会弹出一个服务协议，点击 **Accept**。
3.  **关于登录**：它可能会让你登录或注册 Docker Hub 账号。如果你只是自己用，可以选择 **"Continue without signing in"** (不登录直接使用) 或者 **"Skip"**。
4.  当左下角的鲸鱼图标背景变绿，状态显示 **"Engine running"** 时，说明安装成功！

---

## 4. 动手验证：你的第一个 Hello World

打开你的 **PowerShell** 或 **CMD**（命令提示符），输入以下命令：

### 1. 检查版本
```bash
docker -v
# 输出类似：Docker version 24.0.6, build ... 说明命令能用了
```

### 2. 运行第一个容器
输入这行命令，见证奇迹：
```bash
docker run hello-world
```

**发生了什么？**
1.  Docker 发现你本地没有 `hello-world` 这个镜像。
2.  它自动从云端（Docker Hub）下载了这个镜像。
3.  它启动了一个容器，运行了里面的程序。
4.  屏幕上会打印一段 "Hello from Docker!" 的欢迎语。

---

## 5. 进阶一点点：一行命令起一个 Nginx 服务器

假设你想在本地起一个静态文件服务器：

```bash
docker run -d -p 8080:80 nginx
```

*   `-d`: 后台运行（不占用你当前的终端窗口）。
*   `-p 8080:80`: 端口映射。把容器里的 `80` 端口映射到你电脑的 `8080` 端口。

现在，打开浏览器访问 `http://localhost:8080`，你会看到 Nginx 的欢迎页面！

---
**🎉 恭喜你，你已经入门 Docker 了！** 
以后不管是自己写全栈项目，还是配置 CI/CD 流程，这都是一项非常加分的技能。
