# Ollama 0.1.36 版本现已推出

![GNBO9PuW8AACgXc.jpeg](https://prod-files-secure.s3.us-west-2.amazonaws.com/9903f5b0-d0a4-47c4-8ff7-67753816eda4/51ceb5c6-e6ad-495f-831b-93435112b6ab/GNBO9PuW8AACgXc.jpeg)

Ollama 团队于昨天（2024 年 5 月 11 日）发布了两个新版本，分别是 0.1.35 和 0.1.36。

**0.1.35 版本**

- 主要更新：增加了对 ChatQA-1.5 模型的支持。ChatQA-1.5 是一种大型对话式问答模型，可用于对话问答和检索增强生成领域。

**0.1.36 版本**

- 主要更新：修复了以下 Bug：
    - 修复了 Windows 上 AMD 显卡导致的 exit status 0xc0000005 错误。
    - 修复了在使用 CPU 运行时加载模型时偶尔出现的内存不足错误。

**建议更新**

如果您已经升级到 0.1.35 版本，建议您尽快更新到 0.1.36 版本，以修复 Bug 避免不必要的麻烦

**下载地址**

您可以通过以下链接下载最新版本的 Ollama：

- Github：https://github.com/ollama/ollama/releases/tag/v0.1.36
- Ollama 官网：https://ollama.com/

**如何升级Ollama？**

在macOS和Windows上，Ollama会自动下载更新。只需点击任务栏或菜单栏图标，然后点击“重启以更新”即可应用更新。您也可以选择手动下载最新版本来安装更新。

对于Linux用户，只需重新运行安装脚本即可： 

```
curl -fsSL https://ollama.com/install.sh | sh

```

**Ollama 简介**

Ollama 是一个开源框架，用于在本地运行大型语言模型 (LLM)。它支持多种 LLM 模型，包括 Google Meena、Facebook BlenderBot 和 OpenAI GPT-3。Ollama 提供了一套简单的工具和 API，用于加载、配置和运行 LLM，并与之交互。

Ollama 可以用于各种目的，包括：

- 研究：Ollama 可以用于研究 LLM 的行为和性能。
- 开发：Ollama 可以用于开发基于 LLM 的应用程序。
- 教育：Ollama 可以用于教育人们了解 LLM。
- 娱乐：Ollama 可以用于娱乐目的，例如与 LLM 进行聊天或玩游戏。