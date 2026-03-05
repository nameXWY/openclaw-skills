# 飞书定时任务问题排查

## 快速诊断
```bash
openclaw cron list
cat /root/.openclaw/cron/jobs.json | jq '.jobs[].state'
```

## 常见错误

### 1. "No delivery target"
原因：缺少 `--to` 参数

解决：
```bash
openclaw cron edit <任务ID> --to <飞书用户ID>
```

### 2. "Message failed"
原因：agent内部调用失败

解决：
1. 检查状态是否实际已送达（deliveryStatus可能是delivered）
2. 手动重试
3. 清除错误：
```bash
cat /root/.openclaw/cron/jobs.json | jq '.jobs[0].state.consecutiveErrors = 0' > /tmp/j.json && mv /tmp/j.json /root/.openclaw/cron/jobs.json
```

### 3. Gateway timeout
原因：执行时间过长

解决：
```bash
openclaw cron edit <ID> --timeout-seconds 300
```

## 查看日志
```bash
# 执行历史
cat /root/.openclaw/cron/runs/<任务ID>.jsonl | tail -5

# Gateway日志
tail -100 /tmp/openclaw/openclaw-2026-*.log | grep -i error
```

## 状态说明

| 字段 | 含义 |
|------|------|
| lastRunStatus | 执行状态：ok / error |
| lastDeliveryStatus | 送达状态：delivered / unknown |
| consecutiveErrors | 连续错误计数 |
