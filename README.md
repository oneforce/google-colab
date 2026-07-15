# Qwen3-TTS Google Colab 英语语音生成

使用 Google Colab 运行 **Qwen3-TTS 0.6B CustomVoice**，将英语句子批量转换为 WAV 音频文件。

无需在本地安装 Python、CUDA 或模型环境，只需要一个 Google 账号，即可在浏览器中运行。

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/oneforce/google-colab/blob/main/Qwen3_TTS_Colab_10_English_WAV.ipynb)

## Notebook

```text
Qwen3_TTS_Colab_10_English_WAV.ipynb
```

GitHub 文件：

```text
https://github.com/oneforce/google-colab/blob/main/Qwen3_TTS_Colab_10_English_WAV.ipynb
```

Google Colab：

```text
https://colab.research.google.com/github/oneforce/google-colab/blob/main/Qwen3_TTS_Colab_10_English_WAV.ipynb
```

## 功能

该 Notebook 默认提供以下能力：

* 使用 `Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice`
* 将 10 条英语句子转换为 WAV
* 使用 Google Colab GPU
* 使用 `float16` 降低显存占用
* 优先使用 `sdpa`
* `sdpa` 加载失败时自动切换到 `eager`
* 默认每批生成 2 条语音
* 支持英语内置音色
* 自动保存到 Google Drive
* 已生成文件自动跳过
* 生成失败自动重试
* 批量失败后自动改为逐条生成
* 记录生成进度和错误信息
* 可直接在 Colab 页面试听 WAV
* 可将所有结果打包为 ZIP 下载

## 默认配置

```text
平台：Google Colab
模型：Qwen3-TTS-12Hz-0.6B-CustomVoice
语言：English
精度：float16
注意力：sdpa，失败后使用 eager
音色：Aiden
批量大小：2
最大重试次数：3
输出格式：WAV
输出位置：Google Drive
```

## 使用方法

### 1. 打开 Notebook

点击页面顶部的 **Open In Colab** 按钮。

也可以直接访问：

```text
https://colab.research.google.com/github/oneforce/google-colab/blob/main/Qwen3_TTS_Colab_10_English_WAV.ipynb
```

### 2. 启用 GPU

在 Google Colab 中依次选择：

```text
运行时
→ 更改运行时类型
→ 硬件加速器
→ GPU
→ 保存
```

如果没有启用 GPU，Notebook 的第一个代码单元会提示：

```text
没有检测到 GPU
```

### 3. 运行 Notebook

建议从上到下依次运行所有代码单元：

```text
1. 检查 GPU
2. 安装 qwen-tts 和 soundfile
3. 挂载 Google Drive
4. 准备英语句子
5. 加载 Qwen3-TTS 模型
6. 批量生成 WAV
7. 在线试听 WAV
8. 打包 ZIP
```

第一次加载模型时，需要下载模型文件。

## Google Drive 输出目录

默认保存到：

```text
/content/drive/MyDrive/qwen3_tts_test_10
```

在 Google Drive 中对应：

```text
我的云端硬盘/qwen3_tts_test_10
```

生成后的目录结构如下：

```text
qwen3_tts_test_10/
├── 00001.wav
├── 00002.wav
├── 00003.wav
├── 00004.wav
├── 00005.wav
├── 00006.wav
├── 00007.wav
├── 00008.wav
├── 00009.wav
├── 00010.wav
├── manifest.csv
└── progress.json
```

### 文件说明

| 文件              | 说明              |
| --------------- | --------------- |
| `00001.wav`     | 第 1 条英语句子的语音    |
| `00002.wav`     | 第 2 条英语句子的语音    |
| `manifest.csv`  | 句子、文件名和生成状态     |
| `progress.json` | 已完成编号、失败记录和模型配置 |

## 使用自己的英语句子

在 Notebook 的第 4 个代码单元中找到：

```python
sentences = [
    "Good morning. I hope you have a wonderful day.",
    "The weather is beautiful, so let us take a walk outside.",
    "Please read each sentence clearly and naturally.",
]
```

替换成自己的英语句子：

```python
sentences = [
    "This is my first English sentence.",
    "This is my second English sentence.",
    "Please speak slowly and clearly.",
    "I would like to improve my English pronunciation.",
    "The meeting will begin at nine o'clock tomorrow morning.",
    "Could you please open the window?",
    "She bought a new computer last weekend.",
    "We should finish this project before Friday.",
    "Learning a language requires regular practice.",
    "Thank you very much for your help."
]
```

文件名将按照句子顺序自动生成：

```text
00001.wav
00002.wav
00003.wav
...
00010.wav
```

## 修改音色

默认音色：

```python
SPEAKER = "Aiden"
```

可以修改为：

```python
SPEAKER = "Ryan"
```

修改音色后，如果希望重新生成全部文件，请先删除 Google Drive 输出目录中已有的 WAV 文件。

否则，断点续跑机制会跳过已经存在的文件。

## 修改朗读方式

默认指令：

```python
INSTRUCT = "Speak clearly and naturally at a moderate pace."
```

### 慢速清晰朗读

```python
INSTRUCT = "Speak slowly, clearly, and naturally, with short pauses between phrases."
```

### 英语听写模式

```python
INSTRUCT = "Speak very clearly at a slow pace, with careful pronunciation."
```

### 自然文章朗读

```python
INSTRUCT = "Read naturally and fluently at a conversational pace."
```

### 更有感情的朗读

```python
INSTRUCT = "Read with a warm, expressive, and natural tone."
```

## 修改批量大小

默认设置：

```python
BATCH_SIZE = 2
```

如果 GPU 显存充足，可以尝试：

```python
BATCH_SIZE = 4
```

或者：

```python
BATCH_SIZE = 8
```

如果出现 CUDA 显存不足：

```text
CUDA out of memory
```

请修改为：

```python
BATCH_SIZE = 1
```

然后重新运行生成代码单元。

## 断点续跑

Notebook 会检查目标 WAV 文件是否已经存在。

例如，Google Drive 中已经存在：

```text
00001.wav
00002.wav
00003.wav
```

重新运行时，这 3 条会被自动跳过，从尚未完成的句子继续生成。

因此，即使 Colab 会话中断，也不需要重新生成所有音频。

## 错误重试

默认每批最多尝试 3 次：

```python
MAX_RETRIES = 3
```

处理流程：

```text
批量生成
  ↓
失败后重试
  ↓
连续失败
  ↓
自动改为逐条生成
  ↓
单条仍然失败
  ↓
记录到 progress.json
  ↓
继续处理后面的句子
```

单个句子失败不会终止整个任务。

## 查看生成进度

打开：

```text
progress.json
```

示例：

```json
{
  "model": "Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice",
  "speaker": "Aiden",
  "attention": "sdpa",
  "completed_ids": [
    1,
    2,
    3
  ],
  "failures": {},
  "updated_at": "2026-07-15 11:30:00"
}
```

打开：

```text
manifest.csv
```

可以查看：

```text
id,filename,text,status
1,00001.wav,Good morning.,completed
2,00002.wav,How are you?,completed
3,00003.wav,Thank you.,failed
```

## 在浏览器中播放 WAV

运行 Notebook 的试听代码单元后，Colab 会显示音频播放器。

现代浏览器可以直接播放 WAV，因此不需要先转换成 MP3。

## 下载全部结果

Notebook 最后一个代码单元会把输出目录打包为：

```text
qwen3_tts_test_10.zip
```

然后可以通过 Colab 页面直接下载。

原始 WAV 文件仍会保存在 Google Drive 中。

## 扩展到更多句子

确认 10 条测试运行正常后，可以逐步扩展：

```text
10 条
→ 100 条
→ 500 条
→ 1000 条
→ 10000 条
```

大批量生成时建议：

* 从 TXT 或 CSV 加载句子
* 每条句子设置唯一编号
* 保持断点续跑
* 定期查看 `progress.json`
* 保留 `manifest.csv`
* 不要一次在内存中保存全部 WAV
* 每生成一批就立即写入 Google Drive
* 先使用 `BATCH_SIZE = 2`
* 稳定后再尝试增大批量

## 常见问题

### 没有检测到 GPU

确认已经设置：

```text
运行时 → 更改运行时类型 → GPU
```

设置后重新启动运行时，并从第一个代码单元重新执行。

### `sdpa` 加载失败

Notebook 会自动尝试：

```text
eager
```

通常不需要手动修改。

### CUDA 显存不足

将：

```python
BATCH_SIZE = 2
```

改为：

```python
BATCH_SIZE = 1
```

然后重新运行。

### 修改句子后没有重新生成

这是因为相同编号的 WAV 已经存在。

删除对应文件，例如：

```text
00001.wav
```

然后重新运行生成代码单元。

### Colab 中断后怎么办

重新打开 Notebook：

1. 启用 GPU
2. 重新安装依赖
3. 重新挂载 Google Drive
4. 重新加载模型
5. 再次运行生成代码

已存在的 WAV 会自动跳过。

## 相关项目

* Qwen3-TTS
  https://github.com/QwenLM/Qwen3-TTS

* Qwen3-TTS 0.6B CustomVoice
  https://huggingface.co/Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice

* Google Colab
  https://colab.research.google.com/

## 注意事项

* Google Colab 的免费 GPU 不保证始终可用。
* 免费运行时可能在任务完成前断开。
* 大批量生成必须保留断点续跑机制。
* WAV 文件体积通常明显大于 MP3。
* 生成内容应遵守模型许可证及相关法律法规。
* 不要使用该工具冒充他人或制作未经授权的声音内容。
