# ollama 常见问题解答

![290017667-35782585-e6cb-40b8-b12e-e1ae8d640b33.jpeg](https://prod-files-secure.s3.us-west-2.amazonaws.com/9903f5b0-d0a4-47c4-8ff7-67753816eda4/433fbfd4-b0ae-4e18-b751-1aecfb21d129/290017667-35782585-e6cb-40b8-b12e-e1ae8d640b33.jpeg)

**如何升级Ollama？**

在macOS和Windows上，Ollama会自动下载更新。只需点击任务栏或菜单栏图标，然后点击“重启以更新”即可应用更新。您也可以选择手动下载最新版本来安装更新。

对于Linux用户，只需重新运行安装脚本即可： 

```
curl -fsSL https://ollama.com/install.sh | sh

```

**如何查看日志？** 

您可以查阅故障排除文档，获取更多关于如何使用日志的信息（https://github.com/ollama/ollama/blob/main/docs/troubleshooting.md）

**我的GPU是否与Ollama兼容？**

请查阅GPU文档获取详细信息（https://github.com/ollama/ollama/blob/main/docs/gpu.md）

**如何设置上下文窗口的大小？**

默认情况下，Ollama的上下文窗口大小为2048词元。

若要在使用ollama run时更改此设置，请使用/set参数：

```
/set parameter num_ctx 4096

```

使用API时，请在请求中指定num_ctx参数：

```
curl http://localhost:11434/api/generate -d '{
  "model": "llama3",
  "prompt": "Why is the sky blue?",
  "options": {
    "num_ctx": 4096
  }
}'

```

**如何配置Ollama服务器？**

您可以通过环境变量来配置Ollama服务器

**在Mac上设置环境变量**

如果Ollama作为macOS应用运行，应通过 launchctl 设置环境变量：

1. 对每个环境变量，使用launchctl setenv命令设置
    
    ```
    launchctl setenv OLLAMA_HOST "0.0.0.0"
    ```
    
2. 之后重启Ollama应用。

**在Linux上设置环境变量** 

如果Ollama作为systemd服务运行，通过systemctl设置环境变量：

1. 使用systemctl edit ollama.service命令编辑systemd服务，将打开一个编辑器。
2. 对每个环境变量，在[Service]部分添加一行Environment：
    
    ```
    [Service]
    Environment="OLLAMA_HOST=0.0.0.0"
    ```
    
3. 保存并退出。
4. 重新加载systemd并重启Ollama：
    
    ```
    systemctl daemon-reload
    systemctl restart ollama
    ```
    

**在Windows上设置环境变量**

在Windows上，Ollama会继承您的用户和系统环境变量。

1. 首先通过任务栏图标退出Ollama，
2. 从控制面板编辑系统环境变量，
3. 为OLLAMA_HOST、OLLAMA_MODELS等编辑或新建变量。
4. 点击OK/Apply保存，
5. 然后从新的终端窗口运行ollama。

**如何在我的网络上公开Ollama？**

默认情况下，Ollama绑定到127.0.0.1端口11434。您可以通过设置OLLAMA_HOST环境变量来更改绑定地址。

请参考上述内容了解如何在您的平台上设置环境变量。

**如何使用代理服务器使用Ollama？**

Ollama运行一个HTTP服务器，可以通过代理服务器，比如Nginx，进行公开。具体操作方法是配置代理转发请求，并可选设置所需的头部（如果不在网络上公开Ollama）。例如，使用Nginx配置如下：

```
server {
    listen 80;
    server_name example.com;  # 替换为您的域名或IP
    location / {
        proxy_pass http://localhost:11434;
        proxy_set_header Host localhost:11434;
    }
}

```

**如何使用ngrok访问Ollama？**

您可以通过一系列隧道工具访问Ollama。例如，使用Ngrok

```
ngrok http 11434 --host-header="localhost:11434"

```

**如何使用Cloudflare Tunnel访问Ollama？**

使用Cloudflare Tunnel访问Ollama时，请使用--url和--http-host-header标志：

```
cloudflared tunnel --url http://localhost:11434 --http-host-header="localhost:11434"

```

**如何允许额外的Web来源访问Ollama？** 

默认情况下，Ollama允许来自127.0.0.1和0.0.0.0的跨域请求。您可以通过设置OLLAMA_ORIGINS来配置额外的来源。

请参考上述内容了解如何在您的平台上设置环境变量。

**模型存储在哪里？**

- macOS: `~/.ollama/models`
- Linux: `/usr/share/ollama/.ollama/models`
- Windows: `C:\Users\%username%\.ollama\models`

**如何更改模型存储位置？**

如果需要使用不同的目录，将环境变量OLLAMA_MODELS设置为您选择的目录。

请参考上述内容了解如何在您的平台上设置环境变量。

**Ollama是否会将我的输入和输出发送回ollama.com？**

不会。Ollama在本地运行，您的对话数据不会离开您的设备。

**如何在Visual Studio Code中使用Ollama？**

对于VSCode以及其他编辑器，已经有许多可以利用Ollama的插件和扩展。您可以在主仓库的readme文件底部查看扩展和插件列表。

**如何在代理后使用Ollama？**

如果配置了HTTP_PROXY或HTTPS_PROXY，Ollama可以与代理服务器兼容。使用这些变量时，请确保ollama serve可以访问这些值。使用HTTPS_PROXY时，请确保代理证书作为系统证书安装。请参考上述内容了解如何在您的平台上使用环境变量。

**如何在Docker中通过代理使用Ollama？**

通过在启动容器时传递-e HTTPS_PROXY=https://proxy.example.com参数，可以配置Ollama Docker容器镜像以使用代理。

或者，也可以配置Docker守护进程以使用代理。macOS、Windows和Linux上的Docker Desktop以及使用systemd的Docker守护进程都提供了配置指南。

使用HTTPS时，请确保证书作为系统证书安装。这可能需要在使用自签名证书时构建新的Docker镜像。

```
FROM ollama/ollama
COPY my-ca.pem /usr/local/share/ca-certificates/my-ca.crt
RUN update-ca-certificates
```

构建并运行此镜像

```
docker build -t ollama-with-ca .
docker run -d -e HTTPS_PROXY=https://my.proxy.example.com -p 11434:11434 ollama-with-ca
```

**如何在Docker中使用GPU加速的Ollama？**

在Linux或Windows（使用WSL2）上，Ollama Docker容器可以配置为支持GPU加速。这需要安装nvidia-container-toolkit。详细信息请参见ollama/ollama。

由于缺乏GPU直通和模拟支持，macOS上的Docker Desktop不支持GPU加速。

**为什么在Windows 10的WSL2上网络速度很慢？**

这可能会影响到安装Ollama和下载模型的速度。

打开控制面板 > 网络和互联网 > 查看网络状态和任务，点击左侧的更改适配器设置。找到vEthernet (WSL)适配器，右击选择属性。点击配置，打开高级标签页。遍历每个属性，直到找到Large Send Offload Version 2 (IPv4)和Large Send Offload Version 2 (IPv6)，并将它们禁用。

**如何预加载模型以获得更快的响应时间？**

如果您使用API，可以通过向Ollama服务器发送空请求来预加载模型。这适用于/api/generate和/api/chat API端点。

要使用generate端点预加载mistral模型，请使用：

```
curl http://localhost:11434/api/generate -d '{"model": "mistral"}'
```

要使用chat completions端点，请使用：

```
curl http://localhost:11434/api/chat -d '{"model": "mistral"}'
```

**如何保持模型在内存中或立即卸载？**

默认情况下，模型在内存中保留5分钟后会被卸载。这样做可以在您频繁请求LLM时获得更快的响应时间。但是，您可能希望在5分钟结束之前释放内存或无限期保持模型加载。使用/api/generate和/api/chat API端点的keep_alive参数来控制模型在内存中保留的时间。

keep_alive参数可以设置为：

- 一个持续时间字符串（例如"10m"或"24h"）
- 一个以秒为单位的数字（例如3600）
- 任何负数，将会无限期保持模型在内存中（例如-1或"-1m"）
- '0'，将在生成响应后立即卸载模型

例如，要预加载模型并保留在内存中，请使用

```
curl http://localhost:11434/api/generate -d '{"model": "llama3", "keep_alive": -1}'
```

要卸载模型并释放内存，请使用：

```
curl http://localhost:11434/api/generate -d '{"model": "llama3", "keep_alive": 0}'
```

或者，您可以通过在启动Ollama服务器时设置 OLLAMA_KEEP_ALIVE 环境变量来更改所有模型加载到内存中的时间。OLLAMA_KEEP_ALIVE 变量采用与上述keep_alive参数相同的参数类型。请参考上述说明如何配置Ollama服务器以正确设置环境变量。

如果您想覆盖 OLLAMA_KEEP_ALIVE 设置，可以在/api/generate或/api/chat API端点使用 keep_alive API 参数。

**如何管理服务器可以排队的最大请求数量**

如果向服务器发送的请求过多，它将返回 503 错误，表示服务器过载。您可以通过设置 OLLAMA_MAX_QUEUE 来调整可以排队的请求数量。

**资源：**

- 故障排除文档：https://github.com/ollama/ollama/blob/main/docs/troubleshooting.md