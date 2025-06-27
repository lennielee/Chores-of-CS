在 Ubuntu 纯命令行环境下使用 Clash 进行代理，你需要完成以下步骤：

------

## **1. 下载并安装 Clash**

### **方法 1：从官方 GitHub 下载**

Clash 的官方 GitHub 地址：[Clash GitHub](https://github.com/Dreamacro/clash/releases)

在终端中执行：

```bash
# 下载安装 Clash
wget https://github.com/Dreamacro/clash/releases/download/v1.15.0/clash-linux-amd64-v1.15.0.gz
gzip -d clash-linux-amd64-v1.15.0.gz
chmod +x clash-linux-amd64-v1.15.0
mv clash-linux-amd64-v1.15.0 /usr/local/bin/clash
```

**注：**请根据自己的架构（amd64、arm64 等）下载对应的 Clash 版本。

------

## **2. 配置 Clash**

Clash 运行需要 `config.yaml` 配置文件，一般放在 `~/.config/clash/` 目录下。

### 自己最后用的方法：

需要准备的文件有：[config.yaml](https://zhida.zhihu.com/search?content_id=249251271&content_type=Article&match_order=1&q=config.yaml&zhida_source=entity)，Country.mmdb

如果在windows上已经有clash客户端，可以在home directory行点击open folder：

选择Country.mmdb直接上传到服务器，然后到profiles文件下找到一个可用的yml文件，上传到服务器之后改名为config.yaml

```bash
cp config.yaml ~/.config/clash 
cp Country.mmdb ~/.config/clash 
```



### *创建配置文件目录**

```bash
mkdir -p ~/.config/clash
```

### **下载订阅链接或手动配置**

如果你有 Clash 订阅链接，可使用以下命令下载：

```bash
wget -O ~/.config/clash/config.yaml "你的订阅链接"
```

或者手动编辑：

```bash
nano ~/.config/clash/config.yaml
```

**示例 `config.yaml`（手动配置）：**

```yaml
port: 7890
socks-port: 7891
allow-lan: true
mode: Rule
log-level: info

proxies:
  - name: "代理服务器"
    type: ss
    server: example.com
    port: 8388
    cipher: aes-256-gcm
    password: "yourpassword"

proxy-groups:
  - name: "Proxy"
    type: select
    proxies:
      - "代理服务器"

rules:
  - MATCH,Proxy
```

配置完成后按 `Ctrl + X` 退出，按 `Y` 保存。

------

## **3. 运行 Clash**

执行：

```bash
clash -d ~/.config/clash/
```

如果需要后台运行：

```bash
nohup clash -d ~/.config/clash/ > clash.log 2>&1 &
```

------

## **4. 设置系统代理**

**4.1 仅 HTTP 代理**

```bash
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
```

**4.2 HTTP 和 SOCKS5 代理**

```bash
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
export all_proxy="socks5://127.0.0.1:7891"
```

**4.3 使代理生效** 可以将这些环境变量添加到 `~/.bashrc` 或 `~/.profile`：

```bash
echo 'export http_proxy="http://127.0.0.1:7890"' >> ~/.bashrc
echo 'export https_proxy="http://127.0.0.1:7890"' >> ~/.bashrc
echo 'export all_proxy="socks5://127.0.0.1:7891"' >> ~/.bashrc
source ~/.bashrc
```

------

## **5. 测试代理**

测试代理是否生效：

```bash
curl cip.cc
```

如果返回的 IP 不是你的本地 IP，而是代理服务器的 IP，则代理成功。

------

## **6. 开机自启 Clash**

如果希望 Clash 开机自启，可以使用 `systemd`：

```bash
sudo nano /etc/systemd/system/clash.service
```

填入以下内容：

```ini
[Unit]
Description=Clash Proxy
After=network.target

[Service]
ExecStart=/usr/local/bin/clash -d /root/.config/clash/
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

然后执行：

```bash
sudo systemctl daemon-reload
sudo systemctl enable clash
sudo systemctl start clash
```

**检查运行状态：**

```bash
sudo systemctl status clash
```

------

## **7. 关闭代理**

如果要取消代理环境变量：

```bash
unset http_proxy
unset https_proxy
unset all_proxy
```

------

这样你就可以在 Ubuntu 纯命令行环境下使用 Clash 进行代理了！如果遇到问题，可以检查 Clash 日志：

```bash
cat ~/.config/clash/clash.log
```

你的 `proxy()` 和 `noproxy()` 函数定义了快捷命令来开启和关闭代理，但要让它们在终端中可用，需要将它们添加到 `~/.bashrc` 或 `~/.zshrc` 文件中（取决于你使用的 Shell）。

------

## **1. 添加函数到 `~/.bashrc`**

如果你使用的是 **Bash**（默认 Shell）：

```bash
nano ~/.bashrc
```

在文件末尾添加：

```bash
proxy () {
  export http_proxy="http://127.0.0.1:7890"
  export https_proxy="http://127.0.0.1:7890"
  export all_proxy="socks5://127.0.0.1:7890"
  echo "Clash Proxy: ON"
}

noproxy () {
  unset http_proxy
  unset https_proxy
  unset all_proxy
  echo "Clash Proxy: OFF"
}
```

保存 (`Ctrl + X` -> `Y` -> `Enter`) 后，执行：

```bash
source ~/.bashrc
```

------

## **2. 如果你使用 Zsh**

如果你使用的是 **Zsh**，需要添加到 `~/.zshrc`：

```bash
nano ~/.zshrc
```

然后在末尾添加相同的内容：

```bash
proxy () {
  export http_proxy="http://127.0.0.1:7890"
  export https_proxy="http://127.0.0.1:7890"
  export all_proxy="socks5://127.0.0.1:7890"
  echo "Clash Proxy: ON"
}

noproxy () {
  unset http_proxy
  unset https_proxy
  unset all_proxy
  echo "Clash Proxy: OFF"
}
```

保存后，执行：

```bash
source ~/.zshrc
```

------

## **3. 如何使用**

在终端中，输入以下命令：

```bash
proxy
```

会输出：

```
Clash Proxy: ON
```

此时，终端已经开启代理。

如果要关闭代理，输入：

```bash
noproxy
```

会输出：

```
Clash Proxy: OFF
```

------

## **4. 验证代理是否生效**

1. **检查代理环境变量**

   ```bash
   echo $http_proxy
   ```

   如果返回 `http://127.0.0.1:7890`，说明代理已启用。

2. **测试是否使用代理**

   ```bash
   curl cip.cc
   ```

   - 如果 IP 变成代理服务器的 IP，说明代理生效。
   - 如果 IP 仍然是本机的，说明代理未生效。

3. **测试 HTTP 代理**

   ```bash
   curl -x http://127.0.0.1:7890 cip.cc
   ```

4. **测试 SOCKS5 代理**

   ```bash
   curl --socks5 127.0.0.1:7890 cip.cc
   ```

------

### **5. 让 `proxy()` 和 `noproxy()` 在所有用户生效**

如果希望 **所有用户** 都能使用 `proxy` 和 `noproxy`，可以将这些函数写入 `/etc/profile`：

```bash
sudo nano /etc/profile
```

添加：

```bash
proxy () {
  export http_proxy="http://127.0.0.1:7890"
  export https_proxy="http://127.0.0.1:7890"
  export all_proxy="socks5://127.0.0.1:7890"
  echo "Clash Proxy: ON"
}

noproxy () {
  unset http_proxy
  unset https_proxy
  unset all_proxy
  echo "Clash Proxy: OFF"
}
```

保存后，使其生效：

```bash
source /etc/profile
```

这样，系统所有用户都可以使用 `proxy` 和 `noproxy` 了。

------

### **6. 让 `proxy()` 和 `noproxy()` 适用于 `sudo`**

默认情况下，`sudo` 不会继承普通用户的环境变量。如果你希望 `proxy` 也适用于 `sudo`，可以编辑 `/etc/sudoers`：

```bash
sudo visudo
```

在文件末尾添加：

```bash
Defaults env_keep += "http_proxy https_proxy all_proxy"
```

然后保存并退出 (`Ctrl + X` -> `Y` -> `Enter`)。

这样，即使使用 `sudo`，代理也会生效：

```bash
sudo curl cip.cc
```

------

### **总结**

| 任务                                         | 命令                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| 添加 `proxy()` 和 `noproxy()` 到 `~/.bashrc` | `nano ~/.bashrc`                                             |
| 添加 `proxy()` 和 `noproxy()` 到 `~/.zshrc`  | `nano ~/.zshrc`                                              |
| 让所有用户可用                               | `sudo nano /etc/profile`                                     |
| 使 `sudo` 也使用代理                         | `sudo visudo` 添加 `Defaults env_keep += "http_proxy https_proxy all_proxy"` |
| 启用代理                                     | `proxy`                                                      |
| 关闭代理                                     | `noproxy`                                                    |
| 测试代理是否生效                             | `curl cip.cc`                                                |

这样你就可以快速在 Ubuntu 命令行中 **一键开关代理** 了 🚀