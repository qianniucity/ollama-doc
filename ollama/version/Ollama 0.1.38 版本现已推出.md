# Ollama 0.1.38 版本现已推出

![GJxyvD6XYAA-3M3.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9903f5b0-d0a4-47c4-8ff7-67753816eda4/57b34d3f-f517-4534-947e-702f51aeae0f/GJxyvD6XYAA-3M3.png)

**新模型介绍**

- **Falcon 2**：这是一个全新的模型，由 TII 开发，仅包含因果解码器，拥有 11 亿参数，经过了 5 万亿词元的训练。
- **Yi 1.5**：零一万物Yi 模型的性能提升版本，现已采用 Apache 2.0 开源许可。模型大小分为 6 亿、9 亿和 34 亿参数版。

**新功能**

**ollama ps 命令**

现引入一项新命令：ollama ps。此命令能展示当前加载的模型、它们所占的内存大小以及使用的处理器类型（GPU 或 CPU）：

```
% ollama ps
NAME             	ID          	SIZE  	PROCESSOR      	UNTIL              
mixtral:latest   	7708c059a8bb	28 GB 	47%/53% CPU/GPU	Forever           	
llama3:latest    	a6990ed6be41	5.5 GB	100% GPU       	4 minutes from now	
all-minilm:latest	1b226e2802db	585 MB	100% GPU       	4 minutes from now
```

**/clear 命令**

在使用 ollama run 时，如果需要清除某个会话的聊天记录，可以使用 /clear 命令：

```
>>> /clear
Cleared session context
```

**修复的 BUG**

- 修复了在 Windows 系统上切换已加载模型可能需要几秒钟时间的问题
- 运行 /save 如果提供了错误的名称将不再中断聊天会话
- /api/tags API 端点现在如果没有提供模型将正确返回一个空列表 [] 而不是 null