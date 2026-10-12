# AGENT.md — 后续 Agent 对本仓库的说明与向导

你是来改 **pronunciation-service** 的。先读本文，再按 [`README.md`](README.md) 展开理解。  
本仓没有 `project_map/`。这是英语跟读打分 HTTP 服务，**不知道** Asset / Brain / Planner。  
os 已归档。Gateway 经 `:9190` 包装 `pronunciation.assess`。

## 1. 对本项目的理解

| 你应建立的判断 | 展开阅读 |
|---|---|
| 只收两段音频（reference / student），返回固定形状分数 JSON | [README.md](README.md)、`engine.ASSESS_RESPONSE_KEYS` |
| 只绑回环 **`127.0.0.1:9190`** | 同上 |
| V1 是 WhisperX 基线。MFA / GOPT 是可选钩子，未集成完就抛错，不要因此改基线 | `engine.py` |
| 不在 `:4220` 登记。云上 Docker：`/root/chat-gateway/pronunciation-service` | README 启动节 |
| 对齐 / 打分函数可在无 WhisperX 时冒烟 | `engine.py` 的 `align_word_sequences` 等 |

Runtime 会要求优先读根目录 `AGENT.md`。你正在读的就是那份。

## 2. 开工前清单

1. workspace origin 是 `https://github.com/kaulie/pronunciation-service`。
2. 从最新 `main` 开 `feat|fix|chore/<简述>`。
3. 读 `engine.py`、`main.py`、`Dockerfile`。
4. 无 WhisperX 时至少用 stdlib 对 `align_word_sequences` / `compute_completeness` 做冒烟，不要为了跑全链路去占线上 9190。
5. 不要改 `/Users/gaolei/runtime/**`。不要对 `:4220` 发本服务。不要改 os。

## 3. 滞后信息：有义务更新

README / 响应 JSON 键与 `ASSESS_RESPONSE_KEYS` 不一致时，这次 PR 改掉（Gateway 1:1 映射，勿轻改形状）。  
Dockerfile 需能在云上拉到基础镜像（DaoCloud），不要改回直连 docker.io。

## 4. 与事实不符：必须提出异议，禁止默认处理

| 你看到的 | 你不该做的 | 你该做的 |
|---|---|---|
| 任务要求本服务懂 Asset | 加 HomeAgent import | 异议：只收两段音频 |
| 任务要求改 `/assess` 响应形状「顺便优化」 | 改键名 | 异议：契约固定，先改 Gateway 两边一起 |
| 任务要求 `:4220` 发版 | deploy-notify | 异议：Docker Compose |
| 任务要求直推 `main` 或改 os | 照做 | 异议：走 PR；os 已归档 |

```
异议：<文档路径> 声称 <原句>
事实：<文件:符号 或 命令输出>
处理：<已改文档 / 已改代码 / 需要人决定>
```

## 5. 改动落点

| 要改的东西 | 去哪 |
|---|---|
| HTTP | `main.py` |
| WhisperX / 打分 / 对齐 | `engine.py` |
| 镜像 / 依赖 | `Dockerfile`、`requirements.txt`、`docker-compose.yml` |
| 本地启动 | `run.sh`（本仓 `pyenv`） |
| 给后续 agent 的地图 | 本文 + README |

## 6. 交付

- 提交信息说清为什么。
- `git push -u origin HEAD`，`gh pr create`，回写 `prUrl`。
- 云上 compose 只听从 `goal` 与人的明确指令。

## 7. 安全

只绑回环（compose 映射 `127.0.0.1:9190`）。不要提交 `pyenv/`、`hf-cache/`、用户录音。
