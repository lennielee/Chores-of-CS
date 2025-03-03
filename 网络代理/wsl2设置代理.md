## WSL2



根据你提供的信息，WSL2 的虚拟网络适配器 IPv4 地址是 `172.24.16.1`，代理端口是 `7890`。以下是配置 WSL2 使用主机代理的步骤：

---

### 1. 设置代理
在 WSL2 中运行以下命令，将 `172.24.16.1` 和 `7890` 替换为你的主机 IP 和代理端口：

```bash
export http_proxy=http://172.24.16.1:7890
export https_proxy=http://172.24.16.1:7890
```

如果你使用的是 SOCKS5 代理（例如 Clash），命令如下：

```bash
export http_proxy=socks5://172.24.16.1:7890
export https_proxy=socks5://172.24.16.1:7890
```

---

### 2. 验证代理
使用 `curl` 测试代理是否生效：

```bash
curl -I http://www.google.com
```

如果返回 HTTP 200，说明代理配置成功。

---

### 3. 持久化配置（可选）
将代理配置添加到 `~/.bashrc` 或 `~/.zshrc` 中，使其永久生效：

```bash
echo "export http_proxy=http://172.24.16.1:7890" >> ~/.bashrc
echo "export https_proxy=http://172.24.16.1:7890" >> ~/.bashrc
source ~/.bashrc
```

如果是 SOCKS5 代理：

```bash
echo "export http_proxy=socks5://172.24.16.1:7890" >> ~/.bashrc
echo "export https_proxy=socks5://172.24.16.1:7890" >> ~/.bashrc
source ~/.bashrc
```

---

### 4. 注意事项
- 确保主机代理软件（如 Clash、v2ray 等）已开启并允许来自局域网的连接。
- 如果主机 IP 发生变化（例如重启后），需要重新检查并更新代理配置。
- 如果代理需要认证，可以在代理地址中添加用户名和密码，例如：
  ```bash
  export http_proxy=http://username:password@172.24.16.1:7890
  export https_proxy=http://username:password@172.24.16.1:7890
  ```

---

通过以上步骤，WSL2 就可以通过主机的代理访问网络了。



## 虚拟机

![乌班图代理](D:\Desktop\网络代理\乌班图代理.png)

