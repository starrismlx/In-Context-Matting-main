# LSTM模型分析工具  

## 1. 工具概述  

`all_in_one_lstm_analysis.py` 是一个用于分析LSTM语言模型的Python工具，可完成三大核心任务：模型结构可视化、参数量统计和GPU显存占用测量。适用于深度学习开发者理解LSTM模型复杂度及资源占用，为模型优化和硬件规划提供数据支持。  


## 2. 功能特点  

### 2.1 模型结构分析  
- 打印LSTM完整结构（嵌入层→LSTM层→投影层）  
- 自动解析组件功能（如嵌入维度、隐藏层维度等参数含义）  

### 2.2 参数量统计  
- 计算总参数量与可训练参数量（支持科学计数法）  
- 按层展示参数量分布（前30层详细展示，后续省略）  
- 区分模型各组件参数占比（嵌入层、LSTM层等）  

### 2.3 GPU显存占用测量  
- 支持不同`batch_size`和`seq_len`组合的显存测试  
- 计算单次前向传播的显存增量（排除初始缓存影响）  
- 以表格形式输出结果，直观展示显存与输入大小的关系  

## 3. 环境要求  

### 3.1 依赖项  
- **Python**：3.7及以上版本  
- **PyTorch**：1.8及以上版本（支持CUDA加速）  
- **Transformers**：4.0及以上版本（用于加载GPT-2分词器）  

### 3.2 安装方法  
```bash
# 通过pip安装依赖  
pip install torch transformers  
```  
## 4. 使用方法  

### 4.1 命令行参数说明  
```bash
python all_in_one_lstm_analysis.py 
```  

| 参数名称          | 类型    | 默认值               | 说明                                                                 |  
|-------------------|---------|----------------------|----------------------------------------------------------------------|  
| `--device`        | str     | cuda                 | 计算设备（`cuda`或`cpu`），若指定`cuda`但无可用GPU则自动切换至`cpu` |  
| `--embed_dim`     | int     | 768                  | 嵌入层维度（词向量维度）                                             |  
| `--hidden_dim`    | int     | 768                  | LSTM隐藏层维度                                                       |  
| `--num_layers`    | int     | 2                    | LSTM堆叠层数                                                         |  
| `--bidirectional` | flag    | 关闭                 | 是否启用双向LSTM（启用后参数量翻倍）                                 |  
| `--batch_sizes`   | list    | [1,2,4,8,16]         | 批量大小测试列表                                                     |  
| `--seq_lens`      | list    | [32,64,128,256]      | 序列长度测试列表                                                     |  

### 4.2 运行示例  
#### 4.2.1 基础运行（默认参数）  
```bash  
python all_in_one_lstm_analysis.py  
```  

#### 4.2.2 自定义参数运行  
```bash  
python all_in_one_lstm_analysis.py --device cuda \  
    --embed_dim 512 --hidden_dim 512 --num_layers 3 \  
    --batch_sizes 1 2 4 --seq_lens 16 32 64  
```  

#### 4.2.3 CPU环境运行（跳过显存测试）  
```bash  
python all_in_one_lstm_analysis.py --device cpu  
```  
## 5. 运行结果

### 5.1 模型结构展示

```plaintext
2025-06-18 12:10:35,870 | INFO | ------------------------------------------------------------  
2025-06-18 12:10:35,870 | INFO | Task-1  模型结构（LSTM）  
2025-06-18 12:10:35,870 | INFO | ------------------------------------------------------------  
SimpleLSTMModel(  
  (embed): Embedding(50258, 768)      # 嵌入层：词汇表大小×嵌入维度  
  (lstm): LSTM(768, 768, num_layers=2, batch_first=True) # LSTM层：输入维度×隐藏维度×层数  
  (proj): Linear(in_features=768, out_features=50258, bias=True) # 投影层  
)  

主要组件：  
• nn.Embedding —— 将 token id 映射到连续向量空间。  
• nn.LSTM      —— 可堆叠多层的长短期记忆网络；batch_first=True 方便输入 [B, T, E]。  
• nn.Linear    —— 将 LSTM 输出映射回词表大小，用于语言模型 (softmax 在损失里再加)。  
参数说明：  
embed_dim 指嵌入维度，hidden_dim 指 LSTM 隐状态维度，num_layers 为层数。  
如果 bidirectional=True，则输出维度翻倍。  

## 6. 项目结构  
```  
all_in_one_lstm_analysis.py  # 主程序  
├── SimpleLSTMModel         # LSTM模型定义  
├── 模型结构打印            # 任务1实现  
├── 参数量统计            # 任务2实现  
└── 显存占用测量            # 任务3实现  
```
