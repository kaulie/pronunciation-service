# 发布细节

无 `build.sh`、无平台 `outputs/`。发布 = 云上目录拉 ref + `docker compose up -d --build`。

合入 `main` ≠ 容器已更新。首次（或清 volume 后）会重新拉 Whisper 权重。改 `/v1/assess` 键名必须同时改 Gateway。
