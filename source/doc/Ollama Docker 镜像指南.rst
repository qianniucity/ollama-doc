Ollama Docker 镜像指南
======================

**CPU 版本**

::

    docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama


**Nvidia GPU 版本**

开始之前，请安装 NVIDIA Container Toolkit（https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation）

**通过 Apt 安装**

1. 首先配置软件库：

::

    curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey \
    | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
    curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list \
    | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' \
    | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    sudo apt-get update


2. 接下来，安装 NVIDIA Container Toolkit 相关包：

::
    
    sudo apt-get install -y nvidia-container-toolkit


**通过 Yum 或 Dnf 安装**

1. 设置软件库：

::
    
    curl -s -L https://nvidia.github.io/libnvidia-container/stable/rpm/nvidia-container-toolkit.repo \
    | sudo tee /etc/yum.repos.d/nvidia-container-toolkit.repo


2. 安装 NVIDIA Container Toolkit 相关包：

::

    sudo yum install -y nvidia-container-toolkit


**配置 Docker 以使用 Nvidia 驱动：**

::

    sudo nvidia-ctk runtime configure --runtime=docker
    sudo systemctl restart docker


**启动容器：**

::

    docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama


**AMD GPU 版本**

若要使用 Docker 在 AMD GPU 上运行 Ollama，请使用以下命令及标签：rocm

::

    docker run -d --device /dev/kfd --device /dev/dri -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:rocm


**在本地运行模型**

现在，你可以开始运行模型了：

::

    docker exec -it ollama ollama run llama3


**探索更多模型**

想要尝试更多模型，可以浏览 Ollama 的库。

Ollama 库：https://ollama.com/library