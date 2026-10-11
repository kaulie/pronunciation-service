# pronunciation-service

独立的英语跟读发音打分 HTTP 服务（WhisperX 基线）。

**不知道** HomeAgent Asset、Brain、Planner。只收两段音频，返回固定形状的分数 JSON。

Gateway 通过 `http://127.0.0.1:9190` 调用本服务，包装成能力 `pronunciation.assess`。

只绑本机回环：`127.0.0.1:9190`。不要放到 LAN。

云上目录：`/root/chat-gateway/pronunciation-service/`（从此仓检出，不要再从 home-agent-os 拷）。

## API

`GET /health` — 存活 + 引擎状态（whisper 模型、device、mfa/gopt 开关）。

`POST /v1/assess` — `multipart/form-data`：

- 字段 `reference` — 范读音频
- 字段 `student` — 孩子跟读录音

```json
{
  "overall_score": 84,
  "accuracy_score": 87,
  "fluency_score": 79,
  "completeness_score": 92,
  "prosody_score": 81,
  "duration": {"reference": 61.4, "student": 78.2},
  "problem_words": [{"word": "environment", "score": 58, "start": 31.2, "end": 32.8, "phoneme_errors": [], "reason": "low_confidence"}],
  "problem_phonemes": [],
  "fluency": {"speech_rate": 0, "pause_count": 0, "long_pause_count": 0, "repetition_count": 0},
  "raw_alignment": [],
  "feedback_text": "本次朗读 84 分。需要注意的词：environment。"
}
```

## Pipeline（V1：WhisperX 基线）

1. 两段音频都走 WhisperX ASR + VAD + 词/音素对齐。
2. 范读文本来自范读音频的 ASR（不另收文本）。
3. 学生词 vs 范读词做 Levenshtein 对齐，容忍漏读 / 多读 / 重复 / 语速不同。
4. 分数来自对齐特征：accuracy / completeness / fluency / prosody。
5. 音素 → 词 → 篇章聚合；问题词带时间戳。

V1 不训练模型，只用预训练 WhisperX。

可选升级（配了才加载）：`MFA_BIN` / `GOPT_MODEL_DIR`。未集成完之前钩子会抛错，基线不受影响。

## 启动（cloud-server）

不改 systemd。Docker Compose `restart: unless-stopped`：

```bash
git clone https://github.com/kaulie/pronunciation-service.git /root/chat-gateway/pronunciation-service
cd /root/chat-gateway/pronunciation-service
docker compose up -d --build
curl -sS http://127.0.0.1:9190/health
```

本机不走 Docker：

```bash
python3 -m venv pyenv
./pyenv/bin/pip install -r requirements.txt
./run.sh
```

首次运行会把 Whisper 模型下到 `$HF_HOME`（默认 `./hf-cache`）。
