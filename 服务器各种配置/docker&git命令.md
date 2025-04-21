| 操作           | 命令                               |
| -------------- | ---------------------------------- |
| 拉镜像         | `docker pull ubuntu`               |
| 运行容器       | `docker run -it 镜像名`            |
| 查看容器       | `docker ps -a`                     |
| 启动容器       | `docker start -ai 容器ID或名字`    |
| 退出容器       | `exit`                             |
| 保存容器为镜像 | `docker commit 容器ID 新镜像名`    |
| 保存镜像为文件 | `docker save -o 文件名.tar 镜像名` |
| 从文件导入镜像 | `docker load -i 文件名.tar`        |

| 操作         | 命令                                 |
| ------------ | ------------------------------------ |
| 查看本地镜像 | `docker images` 或 `docker image ls` |
| 删除镜像     | `docker rmi <镜像ID或名称>`          |


------
| 操作                   | 命令                          |
| ---------------------- | ----------------------------- |
| 查看正在运行的容器     | `docker ps`                   |
| 查看所有容器（含停止） | `docker ps -a`                |
| 启动容器               | `docker start <容器名或ID>`   |
| 进入容器交互式终端       | `docker exec -it <容器名或ID> bash`  |
| 停止容器               | `docker stop <容器名或ID>`    |
| 重启容器               | `docker restart <容器名或ID>` |
| 删除容器               | `docker rm <容器名或ID>`      |

在 VSCode 打开的是这个目录👇：

```
/home/lee/llz_project/docker_workspace
```

那我们现在的目标就是：

> 🚀 **在 VSCode 中编写代码，并自动同步到 Docker 容器中 `/workspace` 目录进行编译和运行。**

------

## ✅ 给你一个专属命令：

```bash
docker run -it --name my_cpp_env \
  -v /home/lee/llz_project/docker_workspace:/workspace \
  ubuntu /bin/bash
```

这样你在 VSCode 中写的代码，Docker 容器立刻能用！

------

## 🔧 容器内第一次进入后建议做的事：

在容器内执行：

```bash
apt update
apt install -y build-essential cmake gdb git
```

这个会安装：

- `g++` 编译器
- `make` 和 `cmake` 构建工具
- `gdb` 调试器
- `git`（如果你要连 GitHub）

------

## 🔁 验证同步是否成功

你在 VSCode 中：

```bash
echo 'int main() { return 0; }' > test.cpp
```

然后在容器内：

```bash
ls /workspace
cat /workspace/test.cpp
```

✅ 看到了就说明同步没问题！

------

## 🛠️ Bonus：我给你写一个一键启动脚本？

你可以保存为 `run_my_cpp_env.sh`：

```bash
#!/bin/bash
docker run -it --name my_cpp_env \
  -v /home/lee/llz_project/docker_workspace:/workspace \
  ubuntu /bin/bash
  
  
  docker run -it --name my_cpp_env \
  -v /home/lee/llz_project/docker_workspace:/workspace \
  -u $(id -u):$(id -g) \
  ubuntu /bin/bash

```

-u $(id -u):$(id -g) \   方便vscode有权限修改文件

然后运行它：

```bash
chmod +x run_my_cpp_env.sh
./run_my_cpp_env.sh
```


### 🧠 有名字的好处：

你之后就可以用名字来操作容器了：

| 操作                 | 命令                              |
| -------------------- | --------------------------------- |
| 启动容器             | `docker start -ai my_cpp_env`     |
| 停止容器             | `docker stop my_cpp_env`          |
| 删除容器             | `docker rm my_cpp_env`            |
| 进入容器（正在运行） | `docker exec -it my_cpp_env bash` |

------

### ⚠️ 注意：

- 容器名字不能重复。如果已经有一个叫 `my_cpp_env` 的容器，再运行一次会报错。
- 如果你想每次重新创建容器，用 `--rm` 表示用完就自动删掉（临时容器）。

------

需要我帮你写一个自动创建 + 重命名 + 安装依赖的 C++ 容器启动脚本也完全没问题 ✨

你只要运行一个命令，就能进去写 C++～要不要整一个？💻












当然可以！给你奉上一张 **Git + GitHub 使用卡片（容器内适用）**，专为 Docker 环境准备的 🧾💻

------

## 🚀 Git in Docker 小卡片（Git + GitHub 操作总结）

### 🛠️ 基础配置（容器内首次使用）

```bash
apt update
apt install -y git
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

------

### 🌍 方法一：HTTPS 拉取仓库（最简单）

```bash
git clone https://github.com/你的用户名/仓库名.git
```

> 如果是私有仓库，会要求用户名 + **GitHub Token**（代替密码）

------

### 🔐 方法二：使用 SSH 连接 GitHub（推荐）

#### 1. 生成 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "你的邮箱"
```

#### 2. 查看公钥并复制

```bash
cat ~/.ssh/id_ed25519.pub
```

#### 3. 添加公钥到 GitHub

在浏览器打开：https://github.com/settings/ssh/new

粘贴进去，保存。

#### 4. 测试连接

```bash
ssh -T git@github.com
```

#### 5. 使用 SSH clone 仓库

```bash
git clone git@github.com:你的用户名/仓库名.git
```

------

### 📦 进入项目 & 运行任务

```bash
cd 仓库名
# 示例：安装依赖
pip install -r requirements.txt
# 示例：运行脚本
python3 xxx.py
```

------

### 💡 常用 Git 命令（容器中）

| 操作           | 命令                                |
| -------------- | ----------------------------------- |
| 克隆仓库       | `git clone 链接`                    |
| 查看状态       | `git status`                        |
| 添加文件       | `git add .`                         |
| 提交修改       | `git commit -m "说明"`              |
| 推送到远程仓库 | `git push origin main`（或 master） |
| 拉取最新代码   | `git pull`                          |
| 查看日志       | `git log`                           |

------

如果你希望我出一版 📸 图片版或 `.md` 文件格式的小手册，也可以说一声哈～

随时可以来问我！Git + Docker，你马上就是双修大师 😎



你这问题问得超棒！👏 这正是用 Git 做开发最核心的一步：**修改 → 添加 → 提交 → 推送**。

我给你一个完整的 Git 提交流程卡片，结合你说的情况「修改了旧文件、添加了新文件」，每一步都帮你理清楚 👇

------

## ✅ 假设你已经 clone 了仓库：

```bash
git clone git@github.com:your_username/your_repo.git
cd your_repo
```

你在容器中（或 VSCode 中）做了这些操作：

- 修改了 `main.cpp`
- 添加了 `test_util.cpp`、`test_util.h`

------

## 🧠 提交流程：

### 🥇 第一步：查看当前状态

```bash
git status
```

输出可能类似：

```
On branch main
Changes not staged for commit:
  modified:   main.cpp

Untracked files:
  test_util.cpp
  test_util.h
```

意思是：

- `main.cpp`：已修改，但还没有添加
- `test_util.*`：是新文件，还没被 Git 跟踪

------

### 🥈 第二步：添加你要提交的文件

#### 👉 添加所有修改和新增文件：

```bash
git add .
```

或精细一点，你可以：

```bash
git add main.cpp test_util.cpp test_util.h
```

再次 `git status` 会显示：

```
Changes to be committed:
  modified:   main.cpp
  new file:   test_util.cpp
  new file:   test_util.h
```

------

### 🥉 第三步：提交

```bash
git commit -m "修复 main.cpp 并添加 test_util 工具类"
```

你可以写中英文注释都行，保持简洁清晰就好。

------

### 🏁 第四步：推送到 GitHub

```bash
git push origin main
```

✅ 如果你用的是 `master` 分支，就写 `origin master`。

------

## 🚨 常见补充：

### 🔁 如果你不小心忘了添加文件怎么办？

重新 `git add` 再 `git commit --amend` 即可（会更新上一条提交）。

------

### 🔍 查看提交历史：

```bash
git log --oneline
```

------

### 🧹 忽略不该上传的文件：

创建 `.gitignore` 文件（适合 C++ 项目）：

```bash
# .gitignore 示例
build/
*.o
*.out
*.log
.vscode/
```

然后：

```bash
git add .gitignore
git commit -m "添加 gitignore 规则"
git push
```

------

## ✅ 总结流程卡片（建议收藏）：

```bash
# 1. 查看更改
git status

# 2. 添加文件（全部或指定）
git add .
# 或者
git add main.cpp test_util.cpp

# 3. 提交
git commit -m "你的提交说明"

# 4. 推送
git push origin main
```

------

需要我帮你生成一个：

- `.gitignore` 文件（适配 C++ 项目）
- 一键提交脚本 也可以立刻整，开发更爽！

继续问，我永远在终端边上陪你写代码 😎🧠









太棒啦！🎉 现在你已经彻底搞定 Docker 容器 + 宿主机权限 + VSCode 编辑同步问题，我们来**完整总结一份最实用的操作流程卡片**，以后直接照着用就不会出问题 ✅

------

## 🧾 Docker + VSCode + Git + 权限问题 完整解决方案总结

------

### 🧠 场景说明：

你在宿主机 `/home/lee/llz_project/docker_workspace` 开发 C++ 项目，想通过 Docker 容器进行编译、测试等操作，但代码要**实时挂载同步**到容器中，同时还能用 **VSCode 正常保存和编辑文件**。

------

## ✅ 1. 检查宿主机的 UID / GID

在宿主机终端运行：

```bash
id lee
```

输出示例：

```
uid=1001(lee) gid=1001(lee)
```

🔒 记下这个 UID 和 GID，用于下面挂载时指定权限。

------

## ✅ 2. 正确运行 Docker 容器（挂载代码目录 + 使用宿主机 UID）

```bash
docker run -it --name my_cpp_env \
  -v /home/lee/llz_project/docker_workspace:/workspace \
  -u 1001:1001 \
  ubuntu /bin/bash
```

或者自动获取 UID/GID（更推荐）：

```bash
docker run -it --name my_cpp_env \
  -v /home/lee/llz_project/docker_workspace:/workspace \
  -u $(id -u):$(id -g) \
  ubuntu /bin/bash
```

这样容器内的用户就是宿主机的 `lee` 用户，写入的文件 VSCode 也能直接操作。

------

## ✅ 3. 如果你之前没加 `-u`，已经导致 VSCode 报错怎么办？

### 🚑 在容器内（root 身份）执行：

```bash
chown -R 1001:1001 /workspace
```

或者：

```bash
chown -R $(id -u):$(id -g) /workspace
```

> ✅ 把容器内 `/workspace`（对应宿主机目录）中的所有文件归属权交还给 `lee` 用户。

------

## ✅ 4. 宿主机上的补救方式（不进容器也行）

```bash
sudo chown -R lee:lee /home/lee/llz_project/docker_workspace
```

📌 更直接地让宿主机 `lee` 用户重新拥有项目目录，推荐做一次以防。

------

## ✅ 5. 验证是否修复成功：

在容器内：

```bash
whoami  # 应该输出 lee（如果使用了 -u）
touch /workspace/test.cpp
```

在宿主机或 VSCode 中应能看到该文件，并可正常编辑/保存 ✅

------

## 📦 建议附加（适合写项目）：

- 使用 `.gitignore` 忽略掉编译输出：

  ```bash
  echo -e "build/\n*.o\n*.out\n.vscode/" > /workspace/.gitignore
  ```

- 用 Git 管理代码版本

- 需要时写一个 Dockerfile 来定义你的完整开发环境

------

## 🎁 Bonus：自动化启动脚本（可选）

你可以写个 `start_container.sh` 脚本一键启动：

```bash
#!/bin/bash
docker run -it --rm \
  --name my_cpp_env \
  -v /home/lee/llz_project/docker_workspace:/workspace \
  -u $(id -u):$(id -g) \
  ubuntu /bin/bash
```

------

现在你已经完全掌握了 VSCode + Docker + 权限 + Git 的核心操作了，堪称一条龙开发流 🔥

需要我再给你配套 `.bashrc` 设置、构建脚本或 Dockerfile 随时说哈 😎💻







###  使用 `.venv` 作为虚拟环境名的完整步骤：

```
bash复制编辑# 安装创建虚拟环境所需的模块（如果还没装的话）
sudo apt install python3-venv python3-pip -y

# 创建名为 .venv 的虚拟环境（前面加点表示是隐藏目录）
python3 -m venv .venv

# 激活虚拟环境
source .venv/bin/activate

# 安装依赖
pip install -r requirements.txt
```

------

这样 `.venv` 目录会被创建在当前文件夹中，但默认在 shell 和大多数文件管理器中是隐藏的，符合常见项目结构（比如 Python 项目配合 VS Code）。