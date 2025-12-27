### 📋 **QwenVL 环境部署待办清单**

#### 🔹 1. 连接服务器
- [x] 安装 VSCode + Remote-SSH 插件  
- [x] 使用 SSH 命令连接 AutoDL 服务器 
  ```bash
  ssh -p <端口> root@connect.bjb1.seetacloud.com
  ```
- [x] 确认进入 `/root/autodl-tmp` 目录  

---

#### 🔹 2. 克隆代码仓库
- [x] 克隆 Qwen3-VL 官方仓库 
  ```bash
  git clone https://github.com/QwenLM/Qwen3-VL.git
  ```

---

#### 🔹 3. 安装基础依赖
- [x] 安装 transformers（≥4.57.0） 
  ```bash
  pip install "transformers>=4.57.0"
  ```
- [x] 安装 qwen-vl-utils  
  ```bash
  pip install qwen-vl-utils==0.0.14
  ```
- [x] 安装 accelerate 
  ```bash
  pip install accelerate
  ```

---

#### 🔹 4. 安装 flash-attn（加速训练/推理，可选但推荐）
- [ ] 尝试直接安装：  
  ```bash
  pip install -U flash-attn --no-build-isolation
  ```
- [x] 若失败 → 下载对应 `.whl` 文件（[预编译 wheel 地址](https://github.com/mjun0812/flash-attention-prebuild-wheels/releases)） 
- [ ] 上传 `.whl` 到服务器并安装（示例）：  
  ```bash
  pip install /root/autodl-tmp/flash_attn-*.whl --no-build-isolation
  ```

---

#### 🔹 5. 部署 ms-swift 框架（用于后续微调）
- [x] 克隆 ms-swift 仓库 
  ```bash
  git clone https://github.com/modelscope/ms-swift.git
  ```
- [x] 安装 ms-swift（开发模式） 
  ```bash
  cd ms-swift && pip install -e .
  ```

---

#### 🔹 6. 下载模型权重
- [x] 安装 huggingface 工具 
  ```bash
  pip install -U huggingface_hub hf-cli
  export HF_ENDPOINT=https://hf-mirror.com
  ```
- [x] 通过 CLI 下载模型（示例）：
  ```bash
  hf download Qwen/Qwen3-VL-4B-Instruct --local-dir /root/autodl-tmp/models/Qwen3-VL-4B-Instruct
  ```
  > 或手动从 [HF-Mirror](https://hf-mirror.com) 或 [魔搭](https://www.modelscope.cn) 下载后用 Xftp 上传


---

### 📋 **单图 Zero-Shot 推理测试待办清单（Qwen3-VL-4B）**

#### 🔹 1. 确认前提条件
- [x] 已成功部署 Qwen3-VL 环境（含 `transformers`、`qwen-vl-utils`、`accelerate`） 
- [x] 已下载模型权重至本地路径 
  示例：`/root/autodl-tmp/models/Qwen3-VL-4B-Instruct`
  ![[Pasted image 20251120002223.png]]
- [x] 测试图像已上传到服务器 
  示例：`/root/autodl-tmp/data/

---

#### 🔹 2. 准备推理脚本
##### ✅ 自定义 Python 脚本
- [x] 在 `/root/autodl-tmp/` 下新建文件 `test_single_image.py` 
- [x] 将以下内容粘贴进去（已适配 4B 模型 + JSON 输出要求）： 

```python
from transformers import AutoModelForImageTextToText, AutoProcessor
import torch
model_path = "/root/autodl-tmp/models/Qwen3-VL-4B-Instruct"

# default: Load the model on the available device(s)

model = AutoModelForImageTextToText.from_pretrained(
    model_path,
    dtype=torch.bfloat16,
    attn_implementation="flash_attention_2",
    device_map="auto",
 )

processor = AutoProcessor.from_pretrained(model_path)

query = """
我会提供你一张证照图像，这张图像上可能存在一些污损，请你帮我分析这张图像上有什么污损？
我对污损的定义是：图像上存在的任何影响证照信息识别的缺陷或损坏，它可能是物理层次的，也有可能是数字层次的。
请你用json格式回答，回答中包含以下字段：
{
"污损类型"："描述污损的类型，例如划痕、污渍、折痕等",
"污损位置"："描述污损在图像上的具体位置，例如左上角、右下角等",
"污损严重程度"："描述污损的严重程度，例如轻微、中等、严重等",
"判断原因"："描述为什么将该位置判断为污损"
}
"""
messages = [
    {
        "role": "user",
        "content": [
            {
                "type": "image",
                "image": "/root/autodl-tmp/data/damaged/finger_occlusion/finger_occlusion_1.jpg",
            },
            {"type": "text", "text": query},
        ],
    }
]

# Preparation for inference
inputs = processor.apply_chat_template(
    messages,
    tokenize=True,
    add_generation_prompt=True,
    return_dict=True,
    return_tensors="pt"
)
inputs = inputs.to(model.device)

# Inference: Generation of the output
generated_ids = model.generate(**inputs, max_new_tokens=128)
generated_ids_trimmed = [
    out_ids[len(in_ids) :] for in_ids, out_ids in zip(inputs.input_ids, generated_ids)
]
output_text = processor.batch_decode(
    generated_ids_trimmed, skip_special_tokens=True, clean_up_tokenization_spaces=False
)
print(output_text)
```
---

#### 🔹 3. 执行推理
  ```bash
  python /root/autodl-tmp/test_single_image.py
  ```


---

#### 🔹 4. 输出结果
![[Pasted image 20251120011559.png]]
![[Pasted image 20251120011533.png]]
明显没有检测到有手遮挡的情况。



---

## ✅ Qwen3-VL 微调待办清单（SFT）

### 1. 【数据准备】
- [x] **收集/生成图像数据** 
  - 使用真实证照图像 + 自行伪造/污损图像（如姓名篡改、水渍覆盖等）
- [x] **标注数据** 
  - 格式：JSON 文件，每条样本为一个 dict，含 `messages` 和 `images` 字段  
    示例：
    ```json
    {
      "messages": [
        {"role": "user", "content": "<image>这张证件是否被篡改？"},
        {"role": "assistant", "content": "是的，姓名字段被修改过。"}
      ],
      "images": ["/root/autodl-tmp/data/fake_id_001.jpg"]
    }
    ```
  - 可选标注策略：
    - 人工标注（高精度）
    - 大模型辅助标注（用 zero-shot 模型批量生成初稿再校对）
- [x] **创建目录结构** ✅
  ```bash
  mkdir -p /root/autodl-tmp/data
  mkdir -p /root/autodl-tmp/sft_output
  mkdir -p /root/autodl-tmp/script
  ```
- [x] 创建训练、验证、测试集 
>污损以粉笔灰居多
![[Pasted image 20251120012656.png]]

![[Pasted image 20251120012858.png]]

---


### 2. 【编写训练脚本】
- [x] 在 `/root/autodl-tmp/script/` 下新建 `train_sft.sh`
- [x] 填入以下模板（根据实际路径调整）： 
  ```bash
  #!/bin/bash
  CUDA_VISIBLE_DEVICES=0 swift sft \
    --model "/root/autodl-tmp/models/Qwen3-VL-8B-Instruct" \
    --dataset "/root/autodl-tmp/data/train.json" \
    --output_dir "/root/autodl-tmp/sft_output" \
    --num_train_epochs 3 \
    --per_device_train_batch_size 1 \
    --gradient_accumulation_steps 4 \
    --learning_rate 1e-4 \
    --max_length 2048 \
    --lora_rank 64 \
    --lora_alpha 128 \
    --lora_dropout 0.05 \
    --split_dataset_ratio 0.1  # 自动划分10%作为验证集
  ```
- [x] 赋予执行权限： 
  ```bash
  chmod +x /root/autodl-tmp/script/train_sft.sh
  ```

---

### 4. 【启动微调训练】
- [x] 运行训练脚本： ✅ 2025-11-20
  ```bash
  cd /root/autodl-tmp/script
  bash train.sh
  ```

---
![[Pasted image 20251120015651.png]]
![[Pasted image 20251120015704.png]]

### 5. 【微调后推理测试】
- [x] 编写推理脚本 `infer_finetuned.sh`： ✅ 2025-11-20
  ```bash
 #!/bin/bash
export PYTORCH_CUDA_ALLOC_CONF="expandable_segments=True"
export CUDA_VISIBLE_DEVICES=0
export IMAGE_MAX_TOKEN_NUM=1024
export FPS_MAX_FRAMES_NUM=16

swift infer \
  --max_pixels 3010560 \
  --adapters /root/autodl-tmp/models/sft_4B_qwenvl/v1-20251029-155338/checkpoint-267 \
  --stream true \
  --max_new_tokens 2048 \
  --load_data_args false \
  --val_dataset /root/autodl-tmp/models/sft_4B_qwenvl/v1-20251029-155338/val_dataset.jsonl \
  --result_path /root/autodl-tmp/models/sft_4B_qwenvl/v1-20251029-155338/test_output_1029_1.jsonl
  ```  
🔧 赋予执行权限并运行：
```
chmod +x /root/autodl-tmp/script/infer.sh
cd /root/autodl-tmp/script
./infer.sh
```

分别对三个训练产生的模型进行infer:
![[Pasted image 20251120023246.png]]
进行验证集打乱：
- 验证集通常**只在训练开始前打乱一次**（或完全不打乱）
- 当你使用 `--save_steps 100` 定期保存并评估时
- 早期评估可能只"看到"验证集中简单的样本
- 后期评估"看到"更难的样本，导致准确率**虚假下降**



![[Pasted image 20251120024212.png]]

![[Pasted image 20251120025030.png]]

![[Pasted image 20251120030925.png]]


