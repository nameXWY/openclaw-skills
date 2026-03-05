# GitHub Token 配置与测试

## 用途
用户给了一个GitHub Token，验证是否能用

## 快速测试
```bash
export GITHUB_TOKEN=<你的token>
curl -s -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/user
```

## 查看权限
```bash
curl -s -I -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/user | grep x-oauth-scopes
```

常见权限：`repo`=读写, `repo:status`=只读

## 测试Push
```bash
# 1. 创建仓库
curl -s -H "Authorization: token $GITHUB_TOKEN" -X POST \
  https://api.github.com/user/repos \
  -d '{"name":"test","private":false}'

# 2. 推送测试
git remote add origin https://username:$GITHUB_TOKEN@github.com/username/test.git
git push -u origin main
```

## 常见问题

| 错误 | 解决 |
|------|------|
| Authentication failed | Token过期或权限不足 |
| Repository not found | 仓库名错或无权限 |
| 没有repo权限 | 去GitHub生成新token，勾选repo |
