---
license: apache-2.0
frameworks:
  - PyTorch
pipeline_tag: text-to-image
---


# AnimateDi

<img src="https://modelfoundry.test.osinfra.cn/coderepo/web/v1/file/haml/testModel/main/media/test.svg" alt="">

<img src="https://openmind.cn/coderepo/web/v1/file/openmind/vit_base_patch16_224_pt/main/media/example/000000039769.jpg"
        alt="">

<img src="https://huggingface.co/haml/21/resolve/main/svg.svg" alt="svg.svg">

# 目录

[锚点](./pp.md#准备训练环境)
[锚点](./#准备训练环境)

[英文](README_EN.md)

[链接](https://www.openeuler.org/zh/oEEP/?name=oEEP-0002%20oEEP%20%E6%A0%BC%E5%BC%8F%E4%B8%8E%E5%86%85%E5%AE%B9%E8%A7%84%E8%8C%83#oeep-%E5%86%85%E5%AE%B9%E8%A6%81%E6%B1%82)

- [概述](#概述)
- [准备训练环境](#准备训练环境)
- [模型训练](#模型训练)
- [模型推理](#模型推理)
- [版本说明](#版本说明)



## 概述

### 模型介绍

AnimateDiff提出了一个有效的框架，可将现有的大多数个性化文本到图像模型一次性制成动画，从而节省了针对特定模型进行微调的工作量。

本仓已经支持以下模型任务类型

|     模型      | 任务列表 | 是否支持 |
|:-----------:|:----:|:-----:|
| AnimateDiff |  训练  | ✔ |
| AnimateDiff |  推理  | ✔ |

- 参考实现：

  ```
  url=https://github.com/guoyww/AnimateDiff.git
  commit_id=cf80ddeb47b69cf0b16f225800de081d486d7f21
  ```

- 适配昇腾AI处理器的实现：
  ```shell
  url=https://openmind.cn/models/Ascend-PyTorch/AnimateDiff
  ```

## 准备训练环境

### 创建python环境

- git clone 远程仓
  ```shell
    git clone https://openmind.cn/Ascend-PyTorch/AnimateDiff.git
    cd AnimateDiff
  ```

- 创建python环境并且安装python三方包
  ```shell
    conda env create -f environment.yaml
    conda activate animatediff
    pip3 install torch==2.1.0+cpu  --index-url https://download.pytorch.org/whl/cpu  # For X86
    pip3 install torch==2.1.0  # For Aarch64
    pip3 install accelerate==0.28.0 diffusers==0.11.1 decorator==5.1.1 scipy==1.12.0 attrs==23.2.0  torchvision==0.16.0 transformers==4.25.1
  ```
- 环境准备指导

  请参考《[Pytorch框架训练环境准备](https://www.hiascend.com/document/detail/zh/ModelZoo/pytorchframework/ptes)》。

    **表 1**  昇腾软件版本支持表

  |     软件类型     |   支持版本   |
  |:-----------:|:--------:|
  | FrameworkPTAdapter  |   在研版本   |
  | CANN | 在研版本 |
   | 昇腾NPU固件 | 在研版本 |
   | 昇腾NPU驱动 | 在研版本 |


### 准备数据集

- 需要自行下载WebVid10M数据集，分别将csv文件和2M_val文件夹传入训练脚本对应的csv_path和video_folder参数上面：
   ```
    数据集结构
    ├── 2M_val
    │   ├── 10003109.mp4
    │   ├── 10023815.mp4
    │   ├── 10024310.mp4
    │   ├── 10042700.mp4
    │   ├── 10052036.mp4
    │   ├── 10052783.mp4
    │   ├── 1005608956.mp4
    └── results_2M_val.csv
   ```
  数据来源可以参考 https://github.com/guoyww/AnimateDiff/blob/main/__assets__/docs/animatediff.md 中的数据准备章节。
### 准备预训练权重

- 需要准备2个模型权重:
  ```shell
  runwayml/stable-diffusion-v1-5
  openai/clip-vit-large-patch14
  ```
- 将stable-diffusion-v1-5 路径传入到configs/training/v1/image_finetune.yaml 的pretrained_model_path。
- openai/clip-vit-large-patch14需要放置到模型的根目录下面。
### 准备推理权重
- 如果想使用animatediff的推理功能需要下载下面提及到的模型，按照模型的名字对应放到models目录下面DreamBooth_LoRA、MotionLoRA、Motion_Module、SparseCtrl、StableDiffusion文件夹中。
 模型获取可以参考 https://github.com/guoyww/AnimateDiff/blob/main/__assets__/docs/animatediff.md 文档。
  <details>
