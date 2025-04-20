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


------
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
```

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