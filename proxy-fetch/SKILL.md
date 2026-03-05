# 代理访问国外网站

## 用途
国内机器人需要访问国外网站时，让我帮忙抓取

## 方法1：直接让我抓（简单）

直接告诉我：
"帮我抓取 https://xxx.com"

我用web_fetch去抓，然后内容返回给你

---

## 方法2：API中转（自动化）

让我启动一个简单服务，本地机器人直接请求

### 启动代理服务
```bash
# 在我的机器上运行
PORT=8080
python3 -c "
from http.server import HTTPServer, BaseHTTPRequestHandler
import urllib.request
import json

class ProxyHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        url = self.path[1:]  # 去掉 /
        try:
            response = urllib.request.urlopen(url)
            content = response.read()
            self.send_response(200)
            self.send_header('Content-Type', 'text/html; charset=utf-8')
            self.end_headers()
            self.wfile.write(content)
        except Exception as e:
            self.send_response(500)
            self.end_headers()
            self.wfile.write(str(e).encode())

HTTPServer(('0.0.0.0', 8080), ProxyHandler).serve_forever()
"
```

### 本地机器人请求
```bash
# 你的本地机器人
curl http://我的IP:8080/https://www.google.com
```

---

## 注意事项
- 需要知道我的公网IP或域名
- 端口要开放（注意防火墙）
