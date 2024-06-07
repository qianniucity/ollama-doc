Ollama 如何排除故障
=======================

**Ollama 日志**

有时，Ollama 可能无法如你所愿运行。解决问题的一个好方法是查看日志。在 Mac 上，你可以通过运行以下命令来查看日志：

::

    cat ~/.ollama/logs/server.log


在使用 systemd 的 Linux 系统上，可以用这个命令查看日志：

::

    journalctl -u ollama


在容器中运行 Ollama 时，日志会直接输出到容器的标准输出和标准错误输出中：

::

    docker logs <container-name>


（使用 docker ps 可以找到容器的名字）

如果你是在终端里手动运行 ollama serve ， 那么日志会显示在该终端上。

在 Windows 上运行 Ollama 时，日志的存放位置有一些不同。你可以通过按下 <cmd>+R 并输入下面的命令，在资源管理器窗口中查看日志：

- `explorer %LOCALAPPDATA%\Ollama` 查看日志
- `explorer %LOCALAPPDATA%\Programs\Ollama` 浏览二进制文件（安装程序会添加到你的用户路径中）
- `explorer %HOMEPATH%\.ollama` 浏览存储模型和配置的位置
- `explorer %TEMP%` 浏览存储在一个或多个 `ollama*` 目录中的临时可执行文件

为了更好地排查问题，启用额外的调试日志，首先从系统托盘菜单退出应用，然后在 PowerShell 终端中执行：

::

    $env:OLLAMA_DEBUG="1"& "ollama app.exe"


**LLM 库**

Ollama 内置了多个为不同 GPU 和 CPU 向量特性编译的大语言模型（LLM）库。Ollama 会尝试根据你的系统能力选择最合适的库。如果自动检测功能出现问题，或者你遇到了其他问题（如 GPU 崩溃），可以通过指定特定的 LLM 库来解决。cpu_avx2 的性能最佳，其次是 cpu_avx，最慢但兼容性最强的是 cpu。MacOS 下的 Rosetta 模拟可以使用 cpu 库。

在服务器日志中，你会看到类似下面这样的消息（具体内容可能因版本而异）：

::

    Dynamic LLM libraries [rocm_v6 cpu cpu_avx cpu_avx2 cuda_v11 rocm_v5]



**LLM 库替换（实验性）**

你可以通过设置 OLLAMA_LLM_LIBRARY 来指定使用哪个 LLM 库，从而绕过自动检测。例如，如果你有一张 CUDA 显卡，但想要强制使用支持 AVX2 向量的 CPU LLM 库，可以使用：

::

    OLLAMA_LLM_LIBRARY="cpu_avx2" ollama serve


你可以通过以下命令查看你的 CPU 支持哪些特性：

::

    cat /proc/cpuinfo| grep flags | head -1


**在 Linux 上安装旧版本或预发布版本**

如果你在 Linux 上遇到问题想要安装一个旧版本，或者想要在正式发布前尝试预发布版本，你可以通过指定版本号来告诉安装脚本安装哪个版本：

::

    curl -fsSL https://ollama.com/install.sh | OLLAMA_VERSION="0.1.29" sh


**Linux 的 tmp 目录设置为 noexec**

如果你的系统将 Ollama 存储临时可执行文件的 tmp 目录设置为了 `noexec` ，你可以通过设置 OLLAMA_TMPDIR 来指定一个用户可写的替代位置。例如 OLLAMA_TMPDIR=/usr/share/ollama/

在 NVIDIA GPU 上运行容器失败

确保你已按照 [docker.md](http://docker.md/) 描述的步骤设置好了容器运行时环境。

有时，容器运行时可能在初始化 GPU 时遇到问题。查看服务器日志时，你可能会看到各种错误代码，如 "3"（未初始化）、"46"（设备不可用）、"100"（无设备）、"999"（未知）等。以下是一些可能帮助解决问题的排查方法：

- 容器运行时是否正常工作？尝试运行 `docker run --gpus all ubuntu nvidia-smi`，如果这不起作用，Ollama 将无法识别你的 NVIDIA GPU。
- UVM 驱动是否未加载？运行 `sudo nvidia-modprobe -u`
- 尝试重新加载 nvidia_uvm 驱动 - 先运行 `sudo rmmod nvidia_uvm` 然后运行 `sudo modprobe nvidia_uvm`
- 尝试重启电脑
- 确保你使用的是最新的 nvidia 驱动

如果这些方法都无法解决问题，请收集更多信息并提交问题报告：

- 设置 `CUDA_ERROR_LEVEL=50` 并尝试再次运行，以获得更多诊断日志
- 检查 dmesg 是否有任何相关错误，运行 `sudo dmesg | grep -i nvrm` 和 `sudo dmesg | grep -i nvidia`