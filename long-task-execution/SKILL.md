# 长时间任务执行技巧

## 问题
任务执行到一半被终止（超时/被杀）

## 解决方法

### 1. 分步执行
不要一次做太多，分成小步骤：
```bash
# 错误：一次搜太多
web_search --count 10 --query "aaa bbb ccc ddd"

# 正确：分几次
web_search --count 5 --query "aaa"
web_search --count 5 --query "bbb"
```

### 2. 检查超时
- exec命令默认30秒超时
- 需要更长时间加 `--timeout 120`

### 3. 及时保存
- 重要内容及时写入文件
- 不要等到最后一起存

### 4. 用更稳定的工具
- web_search 比 web_fetch 快
- 先确定有结果再深度抓取
