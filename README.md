# 页面







```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>我的 Docker Nginx 网站</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            color: #333;
            text-align: center;
            padding-top: 50px;
        }
        .container {
            background-color: #ffffff;
            margin: 0 auto;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            max-width: 600px;
        }
        h1 {
            color: #007bff;
        }
        p {
            font-size: 1.1em;
            line-height: 1.6;
        }
        .footer {
            margin-top: 30px;
            font-size: 0.9em;
            color: #666;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>欢迎来到我的 Docker 容器网站！</h1>
        <p>这是在 Nginx Docker 容器中运行的一个简单示例网页。</p>
        <p>如果你能看到这个页面，说明你的 Docker 环境和 Nginx 配置一切正常。</p>
        <p>当前时间（服务器时间）：<span id="currentTime"></span></p>
    </div>
    <div class="footer">
        <p>&copy; 2025 Docker Demo. 保留所有权利。</p>
    </div>

    <script>
        function updateTime() {
            const now = new Date();
            document.getElementById('currentTime').textContent = now.toLocaleString();
        }
        // 每秒更新时间
        setInterval(updateTime, 1000);
        // 立即更新一次
        updateTime();
    </script>
</body>
</html>
```
