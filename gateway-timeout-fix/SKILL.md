# Gateway超时/无响应问题

## 症状
- 消息已读但我不回复
- 好像"卡住"了

## 解决方法
重启Gateway：
```bash
# 在服务器上运行
openclaw gateway restart
```

## 原因
- 任务执行时间过长
- Gateway进程超时
- 资源占用过高

## 预防
- 长时间任务分步执行
- 定期检查Gateway状态
