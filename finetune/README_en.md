# MiniCPM Fine-tuning

[中文版](https://github.com/OpenBMB/MiniCPM/blob/main/finetune/README.md)

This directory provides examples of fine-tuning the MiniCPM-2B model, including full model fine-tuning and PEFT. In terms of format, we offer examples for multi-turn dialogue fine-tuning and input-output format fine-tuning.

If you have downloaded the model to your local system, the `OpenBMB/MiniCPM-2B` field mentioned in this document and in the code should be replaced with the corresponding address to load the model from your local system.

Running the example requires `python>=3.10`. Besides the basic `torch` dependency, additional dependencies are needed to run the example code.



**We have provided an [example notebook](lora_finetune.ipynb) to demonstrate how to process data and use the fine-tuning script with AdvertiseGen as an example.**

```bash
pip install -r requirements.txt
```

## Testing Hardware Standard

We only provide examples for single-node multi-GPU/multi-node multi-GPU setups, so you will need at least one machine with multiple GPUs. In the **default configuration file** in this repository, we have documented the memory usage:

+ SFT full parameters fine-tuning: Evenly distributed across 4 GPUs, each GPU consumes `30245MiB` of memory.
+ LORA fine-tuning: One GPU, consuming `10619MiB`  of memory.。

> Please note that these results are for reference only, and memory consumption may vary with different parameters. Please adjust according to your hardware situation.

## Multi-Turn Dialogue Format

The multi-turn dialogue fine-tuning example adopts the ChatGLM3 dialogue format convention, adding different `loss_mask` for different roles, thus calculating `loss` for multiple replies in one computation.

For the data file, the example uses the following format

```json
[
  {
    "conversations": [
      {
        "role": "system",
        "content": "<system prompt text>"
      },
      {
        "role": "user",
        "content": "<user prompt text>"
      },
      {
        "role": "assistant",
        "content": "<assistant response text>"
      },
      // ... Muti Turn
      {
        "role": "user",
        "content": "<user prompt text>"
      },
      {
        "role": "assistant",
        "content": "<assistant response text>"
      }
    ]
  }
  // ...
]
```

## Dataset Format Example

Here, taking the AdvertiseGen dataset as an example,
you can download the AdvertiseGen dataset from [Google Drive](https://drive.google.com/file/d/13_vf0xRTQsyneRKdD1bZIr93vBGOczrk/view?usp=sharing)
or [Tsinghua Cloud](https://cloud.tsinghua.edu.cn/f/b3f119a008264b1cabd1/?dl=1) . After extracting the AdvertiseGen directory, place it in the `data` directory and convert it into the following format dataset.


> Please note, the fine-tuning code now includes a validation set, so for a complete set of fine-tuning datasets, it must contain training and validation datasets, while the test dataset is optional. Or, you can use the validation dataset in place of it.

```
{"conversations": [{"role": "user", "content": "类型#裙*裙长#半身裙"}, {"role": "assistant", "content": "这款百搭时尚的仙女半身裙，整体设计非常的飘逸随性，穿上之后每个女孩子都能瞬间变成小仙女啦。料子非常的轻盈，透气性也很好，穿到夏天也很舒适。"}]}
```

## Start Fine-tuning

Execute **single-node multi-GPU/multi-node multi-GPU** runs with the following code.

```bash
cd finetune
bash sft_finetune.sh
```

Execute **single-node single-GPU** runs with the following code.

```angular2html
cd finetune
bash lora_finetune.sh
```