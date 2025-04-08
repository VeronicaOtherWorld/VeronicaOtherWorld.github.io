---
title: "04/08 安装使用docker"
date: 2025-04-08 17:10:00 +0100
categories:
  [study, journal]
tags: 
  - [journal, docker]
---

突然间发现自己很久没有来总结自己了!

但是使用英文写作对我来说是一种十分刻意的行为,我需要花费很多时间来写作,根据现在的情况,我准备中英混杂.
这次是一篇安装使用docker时候遇到的问题的总结文.我花费了一个下午的时间来学习,发现问题,查找问题,所以我认为十分值得记录一下,同时以备后续查阅需求.

这次我是用了chatGPT 来帮我创作了这篇小小的攻略.她帮我美化了样式还补充了很多内容,十分感谢她!


### 🚀 一、基础环境
- **操作系统**: macOS Sonoma (Apple Silicon)
- **需求**: 学习 Docker 安装、构建镜像、使用 GUI 控制界面

---

### 🛠️ 二、Docker Desktop 安装说明（已解决问题）

系统拦截 Docker Desktop 安装的问题，例如：
- macOS 提示 “malware blocked”
- com.docker.vmnetd 无法运行

目前已成功安装并运行了 **Docker Desktop 最新修复版本（4.37.2）**，通过以下方式解决：

**安装流程：**
1. 清除旧版本 Docker Desktop（如有）
2. 下载 Apple Silicon 版本的修复包：[https://desktop.docker.com/mac/main/arm64/Docker.dmg](https://desktop.docker.com/mac/main/arm64/Docker.dmg)
3. 启动后如被系统拦截，前往：
   - System Settings → Privacy & Security → 点击 [Allow] 或 [Open Anyway]
4. 确保 Docker 图标出现在顶部菜单栏，小鲸鱼转动即为运行成功
  

**清理 & 替代流程：**

1. 清除所有旧文件（Terminal 中执行）

2. [docker issue](https://docs.docker.com/desktop/cert-revoke-solution/)

```bash

sudo launchctl bootout system/com.docker.vmnetd 2>/dev/null || true

sudo launchctl bootout system/com.docker.socket 2>/dev/null || true

sudo rm /Library/PrivilegedHelperTools/com.docker.vmnetd || true

sudo rm /Library/PrivilegedHelperTools/com.docker.socket || true

ps aux | grep -i docker | awk '{print $2}' | sudo xargs kill -9 2>/dev/null

```

2. 改用 [OrbStack](https://orbstack.dev)：无需 root 权限，不被拦截，兼容 Apple Silicon

> 💡已恢复使用 Docker Desktop，支持图形界面与 CLI 操作。

---

### 🌟 三、完成作业 1～6 步所需内容

#### ✅ 1. 创建 JavaScript App（作业要求第 1 步）

在名为 `MyDockerApp` 的文件夹中，创建以下文件：

- **sayHelloApp.js**
```js
console.log("Hello there!");
```
- **Dockerfile**（无后缀）
```dockerfile
FROM node:alpine
COPY . /app
CMD ["node", "/app/sayHelloApp.js"]
```

---

#### ✅ 2. 注册并登录 Docker 账号

- 访问 [https://hub.docker.com/](https://hub.docker.com/)，注册并邮箱验证
- 在终端中登录：
```bash
docker login
```
登录流程：
- 出现 `Your one-time device confirmation code is: XXXX-XXXX`
- 打开浏览器访问：https://login.docker.com/activate
- 输入验证码授权登录
- 或使用用户名密码方式：
```bash
docker login -u your-username
```

---

#### ✅ 3. Docker Hub 创建仓库（作业要求第 3 步）

- 登录 Docker Hub → 点击顶部“Repositories” → “Create repository”
- 仓库名设置为：`helloworld`
- Namespace 保持默认（即用户名），设置为 Public

---

#### ✅ 4. 构建镜像（作业要求第 4 步）

在本地 Terminal 中进入 `MyDockerApp` 文件夹，执行：

```bash
docker build -t sandwich0/helloworld:1.0 .
```
（将 `sandwich0` 替换为你的 Docker Hub 用户名）

如果需要支持沙盒运行，请添加平台参数：
```bash
docker buildx build --platform linux/amd64 -t sandwich0/helloworld:1.0-amd64 --push .
```

---

#### ✅ 5. Push 镜像到线上（作业要求第 5 步）

```bash
docker push sandwich0/helloworld:1.0
```
或推送多平台版本：
```bash
docker push sandwich0/helloworld:1.0-amd64
```

---

#### ✅ 6. 在虚拟机中测试 app（作业要求第 6 步）

使用 [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/)

- 点击 **+ADD NEW INSTANCE**，进入虚拟 Linux 沙盒
- 拉取镜像：
```bash
docker pull sandwich0/helloworld:1.0-amd64
```
- 运行：
```bash
docker run sandwich0/helloworld:1.0-amd64
```
输出应为：
```text
Hello there!
```

---

### 👀 四、常用 Docker 命令汇总

```bash
# 构建镜像（默认平台）
docker build -t <用户名>/helloworld .

# 构建指定平台镜像并推送
docker buildx build --platform linux/amd64 -t <用户名>/helloworld:amd64 --push .

# 登录 Docker Hub
docker login
# 或用户名密码方式
docker login -u <用户名>

# 推送镜像
docker push <用户名>/helloworld

# 查看本地镜像
docker images

# 查看容器（运行中）
docker ps

# 查看所有容器（含已退出）
docker ps -a

# 删除容器/镜像
docker rm <容器ID>
docker rmi <镜像ID>

# 强制运行特定平台镜像
docker run --platform linux/amd64 <用户名>/helloworld
```

---

### 📚 五、Dockerfile & 镜像运行说明

- Dockerfile 只在 `docker build` 时被读取和执行
- 镜像构建后 `docker run` 才会生效
- 若修改了源码或 Dockerfile，需重新构建 + push
- 多平台镜像通过 manifest list 管理（使用 buildx 构建）

---

### 💡 六、作业完成后的意义

你现在拥有：
- 一个完整的 JS 应用容器化流程
- 一个可部署在任何平台的镜像（含 amd64）
- 一次完整的构建 / 推送 / 运行 / 多平台调试能力

---

### 💖 七、结语

> 这次不但成功解决安装与平台架构冲突问题，还成功完成了构建、推送、测试等多阶段任务。

> 从混乱到清晰，记录了我的探索与成长。

