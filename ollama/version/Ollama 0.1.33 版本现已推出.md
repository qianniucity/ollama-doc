# Ollama 0.1.33 版本现已推出
就在几天前（2024年5月3日），Ollama推出了他们的模型库软件的最新版本。但作为 Ollama 的忠实用户，我觉得这次更新特别有意思，值得一提。其中有个特别的亮点，待会儿再详细说明。

**新版本更新亮点**

新版本有三个主要的更新亮点。

首先，他们修复了一些bug。具体来说，他们：

- 修复了模型不会结束运行导致API挂起的问题。
- 修复了在苹果硅芯片Mac上出现的一系列内存溢出问题
- 修复了在运行Mixtral架构模型时出现的内存溢出问题

其次，推出了几个新的模型供下载和使用。包括：

- Llama 3：由Meta推出的新模型，是目前最强大的公开可用大型语言模型
- Phi 3 Mini：由Microsoft推出的新型3.8B参数轻量级、最先进的开源模型。
- Moondream：一款小型视觉语言模型，专为边缘设备高效运行设计。
- Llama 3 Gradient 1048K：一款经过Gradient优化的Llama 3版本，支持最高1M词元的上下文窗口。
- Dolphin Llama 3：未受审查的Dolphin模型，由Eric Hartford训练，基于Llama 3，具备多种指令、对话和编程技能。
- Qwen 110B：首款超过100B参数的Qwen模型，在性能评估中表现突出
- Llama 3 Gradient：另一个经过优化的Llama 3版本，同样支持最高1M词元的上下文窗口。

第三个，也许是最让人兴奋的更新是关于并发性能的提升。这还处于实验阶段，但它将允许你：

- 如果系统内存足够，同时运行两个或更多的大型语言模型
- 对一个大型语言模型发起两个或更多的请求

你可以通过两个新的环境变量来控制以上两种方式（或同时使用两者）。这些变量应设为整数值，含义很直接：

- OLLAMA_NUM_PARALLEL：允许单个模型同时处理多个请求
- OLLAMA_MAX_LOADED_MODELS：允许同时加载多个模型

想了解如何在macOS、Linux和Windows上设置这些变量的详细信息，请查看Ollama的常见问题解答。比如，我使用的是Windows系统，我需要创建新的系统环境变量。

资源：

- Ollama常见问题解答：https://github.com/ollama/ollama/blob/main/docs/faq.md#how-do-i-configure-ollama-server