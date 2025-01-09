# In-Context-Matting-main
# 项目介绍
   In-Context-Matting 模型的主要代码及配置文件可在[仓库](https://github.com/tiny-smart/in-context-matting)中获取，此仓库的 readme 文件还提供了 stable-diffusion 的下载链接，为进一步研究和使用该模型提供了便利的资源渠道。
   该代码是为了复现 CVPR 2024 论文《In-Context Matting》，可以从这个[链接](https://openaccess.thecvf.com/content/CVPR2024/papers/Guo_In-Context_Matting_CVPR_2024_paper.pdf)找到原论文。代码的详细功能见论文介绍。
 
# 运行代码
 ## 所需文件下载

1. **预训练模型下载**：可以从[链接](https://pan.baidu.com/s/1HPbRRE5ZtPRpOSocm9qOmA?pwd=BA1c)下载代码所需要的预训练模型。

2. **数据集下载**：
   - **ICM-57**：可以从[该链接](https://pan.baidu.com/share/init?surl=ZJU_XHEVhIaVzGFPK_XCRg&pwd=BA1c)下载代码所需要的数据集。
   - **扩充了17类的ICM-57**：已与报告一起提交。

  ## 环境配置
  环境配置过程参考[Stability-AI/stablediffusion](https://github.com/Stability-AI/StableDiffusion#requirements)和[hanzi326/stable-diffusion](https://github.com/hanzi326/stable-diffusion)；
  
  报错：RuntimeError: Failed to import diffusers.pipelines.stable_diffusion.pipeline_stable_diffusion because of the following error (look up to see its traceback): Invalid version: '0.10.1,<0.11'；注意版本对应问题

  报错：ImportError: cannot import name 'SiglipVisionModel' from 'transformers'
  
  解决参考：[该链接](https://github.com/gokayfem/ComfyUI_VLM_nodes/issues/46)

  ## 数据集
  1.下载后，将数据集解压到项目的 `datasets/` 目录中。
  
  2.确保数据集文件夹的结构如下：
  `datasets/[yours]/`

    ├── image
    
    └── alpha
    
  ## 运行代码具体操作
  1.使用以下命令运行评估脚本，将PATH_TO_MODEL替换为预训练模型的路径。
  ```bash
  python eval.py --checkpoint PATH_TO_MODEL --save_path results/ --config config/eval.yaml
```
  2.**结果和训练日志查看**
   - 代码所运行的结果保存在 `results/` 下，有对应的可视化结果
  
   - 代码运行的日志文件保存在`logs/`下，可以运行tensorboard命令查看可视化日志
     

# 注意事项
环境配置部分和部分代码的修改已经写在报告当中。
实验过程注意内存的占用，留足够的空间。

 
 

 
 
