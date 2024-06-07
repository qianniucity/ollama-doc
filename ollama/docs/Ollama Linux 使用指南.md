# Ollama Linux 使用指南

### **安装**

运行以下命令安装 Ollama：

```
curl -fsSL https://ollama.com/install.sh | sh
```

### **AMD Radeon GPU 支持**

虽然 AMD 已将`amdgpu`驱动程序贡献给官方 Linux 内核源代码，但版本较旧，可能不支持所有 ROCm 功能。我们建议您从 https://www.amd.com/en/support/linux-drivers安装最新的驱动程序，以便为您的 Radeon GPU 提供最佳支持。

### **手动安装**

**下载`ollama`二进制文件**

Ollama 以独立二进制文件的形式分发。将其下载到 PATH 中的目录中：

```
sudo curl -L https://ollama.com/download/ollama-linux-amd64 -o /usr/bin/ollama
sudo chmod +x /usr/bin/ollama
```

**添加 Ollama 作为启动服务（推荐）**

为 Ollama 创建用户：

```
sudo useradd -r -s /bin/false -m -d /usr/share/ollama ollama
```

在以下位置创建服务文件`/etc/systemd/system/ollama.service`：

```
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3

[Install]
WantedBy=default.target
```

然后启动服务：

```
sudo systemctl daemon-reload
sudo systemctl enable ollama
```

**安装 CUDA 驱动程序（可选 - 适用于 Nvidia GPU）**

下载并安装CUDA：https://developer.nvidia.com/cuda-downloads

通过运行以下命令来验证驱动程序是否已安装，该命令将打印有关 GPU 的详细信息：

```
nvidia-smi
```

**安装 ROCm（可选 - 适用于 Radeon GPU）**

下载并安装：https://rocm.docs.amd.com/projects/install-on-linux/en/latest/tutorial/quick-start.html

确保安装 ROCm v6

**启动 Ollama**

使用以下方式启动 Ollama `systemd`：

```
sudo systemctl start ollama
```

### **更新**

通过再次运行安装脚本来更新 ollama：

```
curl -fsSL https://ollama.com/install.sh | sh
```

或者通过下载 ollama 二进制文件：

```
sudo curl -L https://ollama.com/download/ollama-linux-amd64 -o /usr/bin/ollama
sudo chmod +x /usr/bin/ollama
```

### **查看日志**

要查看作为启动服务运行的 Ollama 的日志，请运行：

```
journalctl -e -u ollama
```

### **卸载**

删除 ollama 服务：

```
sudo systemctl stop ollama
sudo systemctl disable ollama
sudo rm /etc/systemd/system/ollama.service
```

从 bin 目录中删除 ollama 二进制文件（`/usr/local/bin`、`/usr/bin`或`/bin`）：

```
sudo rm $(which ollama)
```

删除下载的模型和 Ollama 服务用户和组：

```python
sudo rm -r /usr/share/ollama
sudo userdel ollama
sudo groupdel ollama
```