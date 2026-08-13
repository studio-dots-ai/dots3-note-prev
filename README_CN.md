<p align="left">
  <a href="README.md">English</a>&nbsp;｜&nbsp;中文
</p>
<br>

<div align="center">
  <img src="assets/dots%20logo@3x.png" alt="dots logo" width="200" />
  <h1>dots3-note Preview</h1>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://huggingface.co/dots-studio/dots3-note-prev"><img alt="Hugging Face: dots3-note-prev" src="https://img.shields.io/badge/Hugging%20Face-dots3--note--prev-FFD21E?logo=huggingface&amp;logoColor=black" /></a>
  <a href="https://github.com/huggingface/transformers/pull/47844"><img alt="Transformers: dots3-note" src="https://img.shields.io/badge/Transformers-dots3--note-yellow" /></a>
  <a href="https://github.com/sgl-project/sglang/pull/33829"><img alt="SGLang: dots3-note" src="https://img.shields.io/badge/SGLang-dots3--note-blue" /></a>
  <a href="https://recipes.vllm.ai/dots-studio/dots3-note-prev"><img alt="vLLM: dots3-note" src="https://img.shields.io/badge/vLLM-dots3--note-red" /></a>
  <a href="https://modelscope.cn/collections/dots-studio/dots3-note"><img alt="ModelScope: dots-studio" src="https://img.shields.io/badge/ModelScope-dots--studio-624AFF" /></a>

  <a href="https://xhslink.cn/m/7vLoWFQV0wN"><img alt="Dots Studio" src="https://img.shields.io/badge/RedNote-Dots%20Studio-FF2442" /></a>
  <a href="https://discord.gg/haym6hEUE"><img alt="Discord" src="https://img.shields.io/badge/Discord-Join-5865F2?logo=discord&amp;logoColor=white" /></a>
  <a href="https://x.com/dotsstudioai"><img alt="X: dotsstudioai" src="https://img.shields.io/badge/X-%40dotsstudioai-black" /></a>
  <a href="#许可证"><img alt="License: Apache 2.0" src="https://img.shields.io/badge/License-Apache%202.0-blue" /></a>
</div>

<p align="center">
  🌐&nbsp;<a href="https://studio.dots.ai/dots/dots3-zh.html"><b>技术博客</b></a>&nbsp;&nbsp;|&nbsp;&nbsp;
  📄&nbsp;<b>完整报告（即将发布）</b>
</p>

---

## 目录

- [模型介绍](#模型介绍)
- [模型概览](#模型概览)
- [评测结果](#评测结果)
  - [通用推理与智能体](#通用推理与智能体)
  - [多模态理解](#多模态理解)
- [模型链接](#模型链接)
- [快速开始](#快速开始)
- [部署](#部署)
  - [Transformers](#transformers)
  - [SGLang](#sglang)
  - [vLLM](#vllm)
- [评测附录](#评测附录)
- [许可证](#许可证)
- [联系我们](#联系我们)

---

## 模型介绍

dots3-note preview 是 dots3 系列首个开放权重模型。该模型采用混合专家（Mixture-of-Experts，MoE）架构，总参数量为 280B，激活参数量为 16B，支持最长 512K 个 token 的上下文。模型支持文本、图像、视频和音频理解，并生成文本输出。

dots3-note preview 针对以下任务进行了优化：

- 通用知识与指令遵循；
- 数学与逻辑推理；
- 工具使用与多步骤智能体工作流；
- 需要探索、记忆更新和适应能力的交互式任务；
- 代码生成与基于代码的问题求解；
- 图像、文档、图表、音频和视频理解；
- 长上下文信息处理。

dots3 系列包含在能力、时延和推理成本之间采用不同权衡的多款模型，dots3-note preview 是该系列中最轻量级的成员。

## 模型概览

| 属性 | 值 |
| :--- | :--- |
| 架构 | 多模态混合专家模型（MoE） |
| 总参数量 | 280B |
| 激活参数量 | 16B |
| MTP | 1 个共享层，1.13B 参数 |
| 层数 | 1 个稠密层 + 45 个 MoE 层 |
| 隐藏层维度 | 5120 |
| FFN 中间层维度 | 13824（稠密层），1536（每个专家） |
| 专家数量 | 256 个路由专家 + 1 个共享专家，Top-8 激活 |
| 注意力机制 | 13 DSA + 33 SWA（约 1:3） |
| DSA | Top-2048 |
| 上下文长度 | 512K |
| 词表大小 | 152K |
| 视觉编码器 | MoE ViT，总参数量 7B，激活参数量 1.2B |
| 音频编码器 | 稠密模型，800M |
| 支持精度 | BF16、FP8 |
| 输入 | 文本、图像、视频、音频 |
| 输出 | 文本 |

## 评测结果

### 通用推理与智能体

![通用推理与智能体评测结果](assets/bench_cn1.png)

### 多模态理解

![多模态理解评测结果](assets/bench_cn2.png)

## 模型链接

| 模型名称 | 简介 | Hugging Face | ModelScope |
| --- | --- | --- | --- |
| dots3-note-prev | 预览版多模态模型 | 🤗 [模型](https://huggingface.co/dots-studio/dots3-note-prev) | <img src="https://modelscope.cn/favicon.ico" width="16" alt="ModelScope" /> [模型](https://modelscope.cn/models/dots-studio/dots3-note-prev) |
| dots3-note-prev-fp8 | FP8 量化预览版多模态模型 | 🤗 [模型](https://huggingface.co/dots-studio/dots3-note-prev-fp8) | <img src="https://modelscope.cn/favicon.ico" width="16" alt="ModelScope" /> [模型](https://modelscope.cn/models/dots-studio/dots3-note-prev-fp8) |

## 快速开始

建议使用 [SGLang](#sglang) 或 [vLLM](#vllm)，在单个 8 卡节点上部署 FP8 权重。

```python
from openai import OpenAI

client = OpenAI(base_url="http://127.0.0.1:8000/v1", api_key="EMPTY")

response = client.chat.completions.create(
    model="dots3-note-prev",
    messages=[
        {"role": "user", "content": "你好！请简单介绍一下你自己。"},
    ],
    temperature=1.0,
    top_p=0.95,
    max_tokens=256,
    # 启用推理时设置 enable_thinking=True；设置为 False 时直接回复。
    extra_body={"chat_template_kwargs": {"enable_thinking": False}},
)
print(response.choices[0].message.content)
```

如需发起多模态请求，可将 `messages` 替换为以下任一公开示例：

```python
examples = {
    "image": [
        {"type": "image_url", "image_url": {"url": "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/cats.png"}},
        {"type": "text", "text": "这张图片中有几只猫？"},
    ],
    "audio": [
        {"type": "audio_url", "audio_url": {"url": "https://huggingface.co/datasets/hf-internal-testing/dummy-audio-samples/resolve/main/mary_had_lamb.mp3"}},
        {"type": "text", "text": "请转写这段童谣。"},
    ],
    "video": [
        {"type": "video_url", "video_url": {"url": "https://huggingface.co/datasets/merve/vlm_test_images/resolve/main/concert.mp4"}},
        {"type": "text", "text": "请描述这场表演以及视频中可以听到的内容。"},
    ],
}
messages = [{"role": "user", "content": examples["image"]}]
```

如果视频包含音轨，模型也会同时处理其中的音频。

## 部署

以下命令面向单个 8 卡节点上的 FP8 部署。BF16 需要更多显存，请根据可用显存、并发量和输入模态调整上下文长度。

[vLLM](https://recipes.vllm.ai/dots-studio/dots3-note-prev) 的 `main` 分支已原生支持 dots3-note preview。[Transformers #47844](https://github.com/huggingface/transformers/pull/47844) 和 [SGLang #33829](https://github.com/sgl-project/sglang/pull/33829) 仍在审核中；合并前请使用下文指定的 PR 版本。

### Transformers

请先安装 NVIDIA 驱动支持且相互兼容的 [PyTorch 和 torchvision](https://pytorch.org/get-started/locally/) 版本。若需处理音频和视频，还应安装与 PyTorch 兼容的 `torchcodec`（包含在下方命令中），并通过系统包管理器安装 FFmpeg。随后安装 [Transformers #47844](https://github.com/huggingface/transformers/pull/47844)：

```bash
pip install accelerate pillow torchcodec kernels==0.16.0 "transformers @ git+https://github.com/huggingface/transformers.git@refs/pull/47844/head"
```

运行最小化本地推理示例：

```python
from transformers import AutoModelForMultimodalLM, AutoProcessor

model_id = "dots-studio/dots3-note-prev-fp8"
processor = AutoProcessor.from_pretrained(model_id)
model = AutoModelForMultimodalLM.from_pretrained(model_id, dtype="auto", device_map="auto")

messages = [
    {"role": "user", "content": "你好！请简单介绍一下你自己。"},
]
inputs = processor.tokenizer.apply_chat_template(
    messages,
    add_generation_prompt=True,
    return_tensors="pt",
    return_dict=True,
    enable_thinking=False,
).to(model.device)
outputs = model.generate(**inputs, max_new_tokens=128)
print(processor.decode(outputs[0, inputs.input_ids.shape[1] :], skip_special_tokens=True))
```

如需提供多 GPU、兼容 OpenAI API 的服务，请使用 SGLang 或 vLLM。

### SGLang

推荐使用发布镜像 [lmsysorg/sglang:dev-dots3-note](https://hub.docker.com/r/lmsysorg/sglang/tags)。完整的单节点部署方案和调优说明请参阅 [Dots3-Note cookbook](https://github.com/sgl-project/sglang/blob/main/docs/cookbook/autoregressive/RedNote/Dots3-Note.mdx)。源码支持进展请参阅 [SGLang #33829](https://github.com/sgl-project/sglang/pull/33829)。

Docker 部署（首次运行时，镜像会从 Hugging Face 下载模型权重）：

```bash
docker run --gpus all --ipc=host -p 8000:8000 \
  lmsysorg/sglang:dev-dots3-note \
  sglang serve \
    --model-path dots-studio/dots3-note-prev-fp8 \
    --served-model-name dots3-note-prev \
    --host 0.0.0.0 \
    --port 8000 \
    --context-length 524288 \
    --enable-dp-attention \
    --dp-size 8 \
    --tp-size 8 \
    --ep-size 8 \
    --moe-dense-tp-size 1 \
    --page-size 64 \
    --trust-remote-code \
    --attention-backend fa3 \
    --moe-a2a-backend deepep \
    --enable-multimodal \
    --speculative-algorithm NEXTN \
    --speculative-num-steps 3 \
    --speculative-eagle-topk 1 \
    --speculative-num-draft-tokens 4 \
    --speculative-draft-model-path dots-studio/dots3-note-prev-fp8
```

也可以从源码或相应 PR 安装，并在本地使用相同的 `sglang serve` 参数。`--attention-backend fa3` 会设置预填充、解码以及启用投机解码时的草稿模型注意力后端。MTP/NEXTN（`--speculative-algorithm NEXTN` 及相关参数）为可选功能，可将 TPOT 降低 50% 以上。目前尚不支持预填充阶段的 CUDA Graph。

可选功能：

```bash
# 仅加载语言模型
--language-only

# 启用兼容 OpenAI API 的工具调用
--tool-call-parser dots
```

### vLLM

[vLLM](https://recipes.vllm.ai/dots-studio/dots3-note-prev) 的 `main` 分支已原生支持 dots3-note preview。在该功能进入稳定版本前，请使用较新的 nightly build。

以下示例使用 8 张 NVIDIA H100 GPU，以 TP=8、EP=8 部署 FP8 权重：

```bash
vllm serve dots-studio/dots3-note-prev-fp8 \
  --served-model-name dots3-note-prev \
  --host 0.0.0.0 \
  --tensor-parallel-size 8 \
  --enable-expert-parallel \
  --moe-backend deep_gemm \
  --max-model-len 262144
```

可选功能：

```bash
# 仅加载语言模型
--language-model-only

# 启用 3-token MTP 投机解码
--speculative-config '{"method":"mtp","num_speculative_tokens":3}'

# 启用兼容 OpenAI API 的自动工具调用
--enable-auto-tool-choice --tool-call-parser dots
```

## 评测附录

![通用推理与智能体评测结果](assets/benchmark_appendix_cn_reasoning.png)

![多模态评测结果](assets/benchmark_appendix_cn_multimodal.png)

## 许可证

Copyright (c) 2026 Xiaohongshu.

由 dots studio 开发并发布。

dots3-note preview 模型权重采用 Apache License 2.0，并已发布至 [Hugging Face](https://huggingface.co/dots-studio/dots3-note-prev) 和 [ModelScope](https://modelscope.cn/models/dots-studio/dots3-note-prev)。本仓库中的文档和资源文件采用相同许可证。

详情请参阅 LICENSE 文件。

Transformers、SGLang、vLLM 及其他第三方软件适用其各自的许可证。

## 联系我们

如有问题或反馈，请通过以下方式联系我们：

- 邮箱：dots-model-feedback@xiaohongshu.com

---

<p align="center">

<i>dots3-note preview is developed and released by dots studio.</i>

</p>
