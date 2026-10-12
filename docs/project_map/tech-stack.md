# 技术栈

| 项 | 事实 |
|---|---|
| 语言 | Python 3.10（DaoCloud bookworm） |
| 引擎 | whisperx>=3.1.1 |
| 张量 | torch / torchaudio |
| 音频 | librosa、ffmpeg、libsndfile1 |
| 系统 | Debian via Aliyun；PyPI 清华 |
| 对齐可测函数 | `align_word_sequences`、`compute_completeness`（无 WhisperX 也可冒烟） |

无 `build.sh`。无 Flask。无 HomeAgent SDK。
