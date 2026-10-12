# 服务架构

```
Gateway plugins/pronunciation_assess.py
        │  127.0.0.1:9190
        ▼
  main.py  GET /health
           POST /v1/assess  multipart: reference + student
        │
        ▼
  engine.py
        1. WhisperX ASR + VAD + 词/音素对齐（两段都走）
        2. 范读文本来自范读 ASR（不另收文本）
        3. Levenshtein 对齐学生词 vs 范读词
        4. accuracy / completeness / fluency / prosody
        5. 问题词带时间戳
        可选：MFA_BIN / GOPT_MODEL_DIR（未配齐则钩子抛错，基线不受影响）
```

容器内 `PRON_HOST=0.0.0.0`，compose 只映射回环。CPU：`PRON_DEVICE=cpu`，`PRON_COMPUTE_TYPE=int8`，`PRON_LANGUAGE=en`。

响应键以 `ASSESS_RESPONSE_KEYS` 为准（overall / accuracy / fluency / completeness / prosody / duration / problem_* / fluency 细节 / raw_alignment / feedback_text）。
