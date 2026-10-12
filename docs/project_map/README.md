# project_map — pronunciation-service 项目地图

本目录按**运行事实**说明本仓库，不是概念词典。

| 文档 | 维度 |
|---|---|
| [positioning.md](positioning.md) | 两段音频进，固定 JSON 出 |
| [architecture.md](architecture.md) | WhisperX 基线、可选 MFA/GOPT |
| [tech-stack.md](tech-stack.md) | torch / whisperx / Docker |
| [deployment.md](deployment.md) | 回环 :9190、**不走 :4220** |
| [release.md](release.md) | 无 `build.sh` |
| [development.md](development.md) | 无 WhisperX 时的冒烟 |

根目录 [`AGENT.md`](../../AGENT.md) 是后续 agent 的入口。响应键以 `engine.ASSESS_RESPONSE_KEYS` 与 [`README.md`](../../README.md) 为准。

## 本轮已核对的异议（2026-10-12）

1. **不在 `:4220` 登记。** 禁止 deploy-notify。
2. **只绑回环 `127.0.0.1:9190`。** compose 映射 `127.0.0.1:9190:9190`。
3. **V1 是 WhisperX 基线。** MFA / GOPT 未集成完就抛错，不要因此改基线。
4. **`/v1/assess` 响应形状固定。** Gateway 1:1 映射；勿轻改键名。
5. **Dockerfile 走 DaoCloud + 清华 PyPI。** 不要改回直连 docker.io。
6. **本机 venv 目录名是 `pyenv/`**（不是 `.venv`），gitignore。

后续 agent 若发现上述条目被新代码推翻，删除或改写。
