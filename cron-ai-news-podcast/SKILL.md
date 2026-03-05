# 定时推送：AI新闻+播客简报

## 用途
每天早上10点自动推送AI新闻+播客推荐到飞书

## 创建任务
```bash
# 创建每日10点推送任务
openclaw cron add \
  --name "每日AI新闻简报" \
  --cron "0 10 * * *" \
  --message "搜索最新AI新闻（今天或昨天的）+ 优质AI/科技播客推荐，包含：1)5-10条最新新闻 2)3-5个播客（嘉宾、核心话题、关键观点）" \
  --channel feishu \
  --to <飞书用户ID> \
  --session isolated \
  --timeout-seconds 300
```

## 常用命令

| 命令 | 用途 |
|------|------|
| `openclaw cron list` | 查看任务列表 |
| `openclaw cron run <ID>` | 手动执行测试 |
| `openclaw cron rm <ID>` | 删除任务 |

## 查状态
```bash
cat /root/.openclaw/cron/jobs.json | jq '.jobs[].state.lastRunStatus'
```

## 常见问题

**错误：No delivery target** → 需要加 `--to`
```bash
openclaw cron edit <ID> --to <飞书用户ID>
```

**超时** → 加 `--timeout-seconds 300`

---

飞书用户ID格式：`ou_xxxxxxxx`
