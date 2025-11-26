# 一鍵自動化打包 + 拷貝 + 容器部署更新

你这个需求可以拆分成三步，并且实现**一键自动化打包 + 拷贝 + 容器部署更新**，非常适合写成一个简单的自动化脚本或 CI 工具（如 npm script 或 PowerShell 脚本）。下面给你详细讲解每一步的实现方式，并附上完整自动化方案：

✅ 场景目标

* 在 **Windows 下开发 Vue 项目**（比如使用 VS Code）
* 每次运行打包命令后：
  1. 自动执行 `npm run build`
  2. 自动将打包后的 `dist/` 目录复制到 **WSL 的某个目录**
  3. 自动将 `dist/` 拷贝进 WSL 下的 **Docker 容器中**，覆盖已有部署内容
  4. 可选：自动重启容器或触发热更新

***

### 🧱 环境前提

* Windows 下已安装 Node.js 和 Vue 项目
* 已安装 `wsl` 并启用 Ubuntu
* WSL 下运行着 Docker（容器中部署网页）
* Vue 项目打包输出路径为默认的 `dist/`
* WSL 可以直接访问 Docker 容器（用 `docker cp`、`docker exec` 等）

### 🧰 Step 1: 自动打包项目

你已经有：

```bash
npm run build
```

执行后会在 `dist/` 目录下生成打包结果。

### 🧰 Step 2: 把 `dist/` 拷贝到 WSL 的共享目录

你可以直接复制到 WSL 的文件系统中，例如 WSL 中 `/home/xxx/vue-deploy/` 目录。

#### ✅ 方法 1：使用 PowerShell（推荐方式）

```powershell
# Windows 下 PowerShell 脚本
$source = ".\dist\"
$target = "\\wsl$\Ubuntu\home\your_username\vue-deploy\"

Copy-Item -Path $source -Destination $target -Recurse -Force
```

> 确保 `vue-deploy` 这个路径在 WSL 中已创建：\
> `mkdir -p ~/vue-deploy`

### 🧱 环境前提

* Windows 下已安装 Node.js 和 Vue 项目
* 已安装 `wsl` 并启用 Ubuntu
* WSL 下运行着 Docker（容器中部署网页）
* Vue 项目打包输出路径为默认的 `dist/`
* WSL 可以直接访问 Docker 容器（用 `docker cp`、`docker exec` 等）

***

### 🧰 Step 1: 自动打包项目

你已经有：

```bash
npm run build
```

执行后会在 `dist/` 目录下生成打包结果。



***

### 🧰 Step 3: 将文件从 WSL 拷贝进 Docker 容器中

在你的 WSL 中写一个 shell 脚本来执行这步，比如 `~/deploy_vue.sh`：

```bash
#!/bin/bash

# 定義變數
CONTAINER_NAME="your_docker_container"
SOURCE_DIR="/home/your_username/vue-deploy/dist"
TARGET_DIR="/usr/share/nginx/html" # 範例：nginx 容器預設路徑

# 拷貝文件
docker cp "$SOURCE_DIR" "$CONTAINER_NAME":"$TARGET_DIR"

# 可選：重啟容器
# docker restart $CONTAINER_NAME

echo "✅ Vue 專案已部署到容器 $CONTAINER_NAME 中"
```

加上权限：

```bash
chmod +x ~/deploy_vue.sh
```

***

### ✅ 完整一键脚本（推荐用 PowerShell）

你可以在项目根目录新建一个 PowerShell 脚本，例如 `deploy.ps1`：

```powershell
# Step 1: 打包
npm run build

# Step 2: 拷贝到 WSL
$dist = ".\dist\"
$wslPath = "\\wsl$\Ubuntu\home\your_username\vue-deploy\"
Copy-Item -Path $dist -Destination $wslPath -Recurse -Force

# Step 3: 执行 WSL 内的 docker 部署脚本
wsl bash /home/your_username/deploy_vue.sh
```

> 执行方式：在 Windows 下打开 PowerShell，运行：

```powershell
.\deploy.ps1
```

***

#### 🧩 可选优化 <a href="#ke-xuan-you-hua" id="ke-xuan-you-hua"></a>

* 支持多容器或路径参数传入
* 加入版本号、Git 信息等自动标记
* 上传静态资源到 CDN / 云盘（如有需求）

***

#### 🟩 总结 <a href="#zong-jie" id="zong-jie"></a>

|         |                           |                 |
| ------- | ------------------------- | --------------- |
| 打包 Vue  | `npm run build`           | Windows 端运行     |
| 拷贝 dist | `Copy-Item` + `\\wsl$` 路径 | 跨系统传输           |
| 容器更新    | WSL 内 `docker cp`         | 拷贝到 Docker 容器内部 |
| 一键部署    | PowerShell 脚本整合           | 高效、可维护          |
