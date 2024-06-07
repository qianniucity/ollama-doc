# Ollama 0.1.35 版本现已推出

![320665281-0258e96a-a703-489a-80be-6caa97cd3f81.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9903f5b0-d0a4-47c4-8ff7-67753816eda4/39f5f705-4eb1-4ca4-a583-d259fc0e303f/320665281-0258e96a-a703-489a-80be-6caa97cd3f81.png)

# **摘要**

新版特性简介：NVIDIA 推出了新的 Llama 3 ChatQA 模型，专长于对话式问答和检索增强生成。此外，引入了模型量化功能，使得在导入模型时可以进行量化处理。此次更新还修复了一系列技术问题，如清理推理子进程、多 GPU 系统上的内存溢出问题、ollama run 命令中的新行处理、视觉模型展示问题、API 请求处理以及文件管理。新版本还新增了生成停止原因的解释，并在多 GPU 系统上运行不同模型时，更准确地评估可用内存量。 

# **新模型介绍**

**Llama 3 ChatQA**：这是一个由 NVIDIA 开发的基于 Llama 3 的模型，该模型在对话式问答（QA）和检索增强生成（RAG）方面表现出色。

模型同样分为 8b，和 70b 两个本版，用户根据喜好自行下载

```
ollama pull llama3-chatqa:8b
```

# **最近更新和修复**

**新功能**

量化功能：现在，ollama create 命令支持在导入模型时使用 --quantize 或 -q 选项进行量化处理：

```
ollama create -f Modelfile --quantize q4_0 mymodel
```

**注意**

--quantize 选项在导入 float16 或 float32 模型时有效：

- 从二进制 GGUF 文件导入（例如 FROM ./model.gguf）
- 从库中导入模型（例如 FROM llama3:8b-instruct-fp16）

**他们修复了一下一些bug**

- 修复了关闭程序时无法清理推理子进程的问题。
- 解决了在多 GPU 系统上加载模型时遇到的一系列内存溢出问题
- 现在，Ctrl+J 键盘操作会在 ollama run 命令中正确地添加新行
- 修复了在运行 ollama show 命令查看视觉模型时出现的问题
- 向 Ollama API 发送 OPTIONS 请求不再会引发错误
- 修复了未能清理部分下载文件的问题
- 在生成停止响应时，响应中新增了一个 done_reason 字段，用以说明停止的原因
- Ollama 现在能够更准确地评估在多 GPU 系统上可用的内存量，特别是在连续运行不同模型的情况下

有谁知道这个 量化功能 什么时候会用到吗，请在留言区留言