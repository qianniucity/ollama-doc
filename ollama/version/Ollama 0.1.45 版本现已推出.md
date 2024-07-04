# Ollama 0.1.45 版本现已推出

### 新模型

- **DeepSeek-Coder-V2：**这是一个开源的混合专家模型（Mixture-of-Experts），具备 16B 和 236B 的代码语言模型，它在处理代码相关任务时，其性能可与 GPT4-Turbo 媲美。

### Ollama show 命令

现在，使用 `ollama show` 命令能够查看模型的详细信息，包括上下文长度、参数数量、嵌入的大小、许可证信息等等。

```shell
% ollama show llama3
  Model                                              
  	arch            	llama	                              
  	parameters      	8.0B 	                              
  	quantization    	Q4_0 	                              
  	context length  	8192 	                              
  	embedding length	4096 	                              
  	                                                   
  Parameters                                         
  	num_keep	24                   	                      
  	stop    	"<|start_header_id|>"	                      
  	stop    	"<|end_header_id|>"  	                      
  	stop    	"<|eot_id|>"         	                      
  	                                                   
  License                                            
  	META LLAMA 3 COMMUNITY LICENSE AGREEMENT         	  
  	Meta Llama 3 Version Release Date: April 18, 2024
```

### 更新内容

- 现在，`ollama show <model>` 命令能展示出模型的详细信息，比如上下文窗口的大小。
- 在 Windows 上，借助 CUDA GPUs 加载模型的速度得到了显著提升。
- 在 OpenAI 的兼容性接口 `/v1/chat/completions` 中设置种子值，现在不会再影响温度参数的设置。
- GPU 的发现和多 GPU 支持的并行处理能力得到了增强。
- Linux 的安装脚本现在会跳过网络设备的搜索步骤。
- 针对 Linux 上 AMD Vega RX 56 SDMA 的支持，引入了一项临时解决方案。
- 修正了 deepseek-v2 和 deepseek-coder-v2 模型的内存预测问题。
- `api/show` 接口现在能返回模型的详细元数据信息。
- GPU 配置信息现在能通过 `ollama serve` 命令获取到。
- Linux 上的 ROCm 软件更新到了 v6.1.1 版本。

### 资源链接：

- **DeepSeek-Coder-V2**：https://ollama.com/library/deepseek-coder-v2