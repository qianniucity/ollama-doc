# Ollama 导入模型
### **导入模型**

本指南将向您展示如何导入一个 GGUF、PyTorch 或 Safetensors 模型。

### **导入（GGUF）**

**步骤 1：编写模型文件**

开始之前，您需要创建一个模型文件。这个文件就像是您模型的设计图，里面指定了模型的权重、参数、提示模板等信息。

```
FROM ./mistral-7b-v0.1.Q4_0.gguf
```

（可选）很多聊天模型为了能够正确回答问题，需要一个预设的提示模板。您可以通过在模型文件中添加 TEMPLATE 指令来设定一个默认的提示模板：

```
FROM ./mistral-7b-v0.1.Q4_0.gguf
TEMPLATE "[INST] {{ .Prompt }} [/INST]"
```

**步骤 2：创建 Ollama 模型**

接下来，根据您的模型文件创建一个新模型：

```
ollama create example -f Modelfile
```

**步骤 3：运行您的模型**

然后，使用 ollama run 命令来测试您的模型：

```
ollama run example "What is your favourite condiment?"
```

### 导入（PyTorch & Safetensors）

> 相比于 GGUF，从 PyTorch 和 Safetensors 导入模型的过程要复杂一些。不过，我们正在努力简化这一流程。
> 

**设置**

首先，克隆 ollama/ollama 仓库：

```
git clone [git@github.com](mailto:git@github.com):ollama/ollama.git ollama
cd ollama
```

紧接着，同步 llm/llama.cpp 子模块：

```
git submodule init
git submodule update llm/llama.cpp
```

然后，安装 Python 依赖项：

```
python3 -m venv llm/llama.cpp/.venv
source llm/llama.cpp/.venv/bin/activate
pip install -r llm/llama.cpp/requirements.txt
```

接着构建量化工具：

```
make -C llm/llama.cpp quantize
```

**（可选）克隆 HuggingFace 仓库**

如果模型托管在 HuggingFace 仓库中，首先克隆该仓库下载原始模型。

安装 Git LFS，验证是否安装成功，然后克隆模型的仓库：

```
git lfs install
git clone https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.1 model
```

**转换模型**

> 注意：某些模型架构需要使用特定的转换脚本。例如，Qwen 模型需要运行 [convert-hf-to-gguf.py](http://convert-hf-to-gguf.py/) 而非 [convert.py](http://convert.py/)。
> 

```
python llm/llama.cpp/convert.py ./model --outtype f16 --outfile converted.bin
```

**量化模型**

```
llm/llama.cpp/quantize converted.bin quantized.bin q4_0
```

**步骤 3：编写模型文件**

然后，为您的模型创建一个模型文件：

```
FROM quantized.bin
TEMPLATE "[INST] {{ .Prompt }} [/INST]"
```

**步骤 4：创建 Ollama 模型**

最后，根据您的模型文件创建一个模型：

```
ollama create example -f Modelfile
```

**步骤 5：运行您的模型**

再次使用 ollama run 命令测试您的模型：

```
ollama run example "What is your favourite condiment?"
```

### 发布您的模型（可选 - 早期 alpha 版本）

目前模型发布功能处于早期测试阶段。如果您想分享您的模型，请按照以下步骤操作：

1. 创建一个账号
2. 复制您的 Ollama 公钥：
- macOS: cat ~/.ollama/id_ed25519.pub | pbcopy
- Windows: type %USERPROFILE%\.ollama\id_ed25519.pub
- Linux: cat /usr/share/ollama/.ollama/id_ed25519.pub
1. 将您的公钥添加到 Ollama 账号

接下来，将您的模型复制到您的用户名空间下：

```
ollama cp example <your username>/example
```

> 注意：模型名称只能包含小写字母、数字和 .、-、_ 这些字符。
> 

然后推送模型：

```
ollama push <your username>/example
```

发布后，您的模型将可以在 https://ollama.com/<your username>/example 访问。

### 量化参考

量化选项从最高到最低等级依次为：注意，某些结构如 Falcon 不支持 K 量化。

```
q2_K,q3_K,q3_K_S,q3_K_M,q3_K_L,q4_0（推荐）,q4_1,q4_K,q4_K_S,q4_K_M,q5_0,q5_1,q5_K,q5_K_S,q5_K_M,q6_K,q8_0,f16
```