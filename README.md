# 🎙️ **EchoSplit: Real-Time Audio Pipeline for Smarter Meeting Agents**

`EchoSplit` is my experimental, modular, and open-source pipeline designed to convert noisy, multi-speaker audio from online meetings into speaker-attributed text — enabling **LLM-powered meeting assistants** to generate better summaries, contextual actions, and agentic decisions.

---

## 🔍 **Problem Statement - Idea**

In multi-speaker online meetings, raw audio streams are:

* Noisy (background hums, keyboards, fan noise),
* Overlapping (multiple speakers),
* Unstructured (no speaker identity),
* Not ideal for downstream LLM tasks.

To empower AI agents to assist with summaries, decisions, and follow-ups in **real-time**, we need a robust **preprocessing pipeline** that delivers clean, structured, speaker-wise transcriptions.

---

## ✅ Current Stage: **Noise Cancellation**

I've completed:

* ✅ Custom augmentation with multiple noise types
* ✅ Spectrogram-based visualization
* ✅ Denoising with DeepFilterNet v3
* ✅ CLI-based inference with RNNoise

---

## 🔧 **Setup Instructions**

### ⚙️ **RNNoise Installation on Windows**

> Raw audio denoising using lightweight C-based RNN model

* 📦 [RNNoise Gitlab](https://gitlab.xiph.org/xiph/rnnoise/)
* 🧰 [Install MSYS2](https://www.msys2.org/) and open MSYS2 64-bit terminal:

```bash
pacman -Syu  # then close and reopen
pacman -Su
pacman -S git make autoconf automake libtool gcc
```

* Then clone and build:

```bash
git clone https://gitlab.xiph.org/xiph/rnnoise.git
cd rnnoise
./autogen.sh
./configure
make
```

#### 🔁 **Convert WAV ↔ PCM (RNNoise CLI uses 48kHz 16-bit raw audio)**

```bash
ffmpeg -i noisy.wav -f s16le -acodec pcm_s16le -ac 1 -ar 48000 noisy.pcm
./examples/rnnoise_demo noisy.pcm denoised.pcm
ffmpeg -f s16le -ar 48000 -ac 1 -i denoised.pcm denoised.wav
```

---

### ⚙️ **DeepFilterNet Setup**

> PyTorch-based real-time deep noise suppressor

#### **Option 1: PyPI Installation (tested on Python 3.9.23)**

```bash
pip install deepfilternet
```

#### **Option 2: From GitHub Source**

```bash
git clone https://github.com/Rikorose/DeepFilterNet.git
cd DeepFilterNet
pip install ./DeepFilterNet
```

#### **For Local Usage:**

```python
import sys
sys.path.insert(0, "path/to/DeepFilterNet")
from df.enhance import enhance
from df.config.config import read_config
config = read_config()
enhance(config, input_path, output_path)
```

---

## 📁 **Sample Data**

| Folder                        | Contents                                |
| ----------------------------- | --------------------------------------- |
| `augmented/`                  | Contains 7 variants of noisy test audio |
| `data/deepfilternet_outputs/` | Cleaned audio outputs for comparison    |

---

## 🛠️ **Next Development Stages**

| Stage                            | Description                                      |
| -------------------------------- | ------------------------------------------------ |
| 🔊 **Noise Suppression** (✓)     | DeepFilterNet + RNNoise benchmarking             |
| 🔉 **Speech Enhancement**        | Apply VoiceFixer or SEGAN                        |
| 🧍 **Speaker Separation**        | Use Demucs v4 / Conv-TasNet                      |
| 🧠 **STT (per speaker)**         | Whisper Tiny / Silero / NVIDIA NeMo              |
| 🧑‍🤝‍🧑 **Speaker Diarization** | pyannote-audio or Wavesplit                      |
| 📝 **LLM Feed Generation**       | Timestamped logs → structured context            |
| 🧩 **Modular Serving**           | FastAPI endpoints / Chrome extension integration |

---

## 🚀 **Vision**

Turn online meeting audio into:

```json
[
  {"speaker": "A", "start": "00:00", "end": "00:05", "text": "Let’s start the demo."},
  {"speaker": "B", "start": "00:06", "end": "00:09", "text": "Sure, loading now."}
]
```

...to empower **agentic AI assistants** with perfect memory, history, and intent.

---
## **Collaborations**
- [Vinay](https://github.com/Dvinaykumar6)
