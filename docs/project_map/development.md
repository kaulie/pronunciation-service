# 开发流程

origin：`https://github.com/kaulie/pronunciation-service`。从最新 `origin/main` 开短命分支，PR 回 `main`。

1. 先读 [`../../AGENT.md`](../../AGENT.md) 与本目录。
2. HTTP `main.py`；打分 `engine.py`。
3. 无 WhisperX 时至少对 `align_word_sequences` / `compute_completeness` 做 stdlib 冒烟，不要为了全链路去占线上 9190。
4. 不要改响应形状「顺便优化」。
5. Dockerfile 保持 DaoCloud。

不要提交 `pyenv/`、`hf-cache/`、用户录音。
