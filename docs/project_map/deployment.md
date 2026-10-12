# 部署细节

**不是** `:4220`。

```bash
git clone https://github.com/kaulie/pronunciation-service.git /root/chat-gateway/pronunciation-service
cd /root/chat-gateway/pronunciation-service
docker compose up -d --build
curl -sS http://127.0.0.1:9190/health
```

容器：`home-agent-pronunciation`，`mem_limit: 6g`，volume `pron-hf-cache`。

本机：

```bash
python3 -m venv pyenv
./pyenv/bin/pip install -r requirements.txt
./run.sh
```

不要占用线上 9190。不要改 `/Users/gaolei/runtime/**`。不要 deploy-notify。
