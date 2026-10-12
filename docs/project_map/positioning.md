# 项目定位

## 一句话

pronunciation-service 是英语跟读打分 HTTP 服务：收范读 + 学生两段音频，返回固定形状分数 JSON。不知道 Asset / Brain / Planner。

仓库：[`https://github.com/kaulie/pronunciation-service`](https://github.com/kaulie/pronunciation-service)  
回环口：`127.0.0.1:9190`

Gateway 经该口包装能力 `pronunciation.assess`。

## 它不是什么

| 不是 | 谁才是 |
|---|---|
| 能力声明 / 规划 | 集市 + Brain |
| 录音采集 | Gateway / 手机发出窗 |
| `:4220` 托管服务 | Brain / Gateway / 集市 |
| 已训好的自家声学模型 | V1 只用预训练 WhisperX |

## 当前形态

- `main.py` + `engine.py`
- 云上 Docker：`container_name: home-agent-pronunciation`，`mem_limit: 6g`
- 首次运行把 Whisper 模型下到 `$HF_HOME`（默认 `./hf-cache`，容器内 `/data/hf-cache`）
