<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>智启科技 · 首页</title>
    <!-- 使用 Google Font 提升字体（国内可替换） -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz@14..32&display=swap" rel="stylesheet" />
    <style>
        /* 全局重置 */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: #fafbfc;
            color: #1e293b;
            line-height: 1.6;
        }

        a {
            text-decoration: none;
            color: inherit;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 24px;
        }

        /* 导航栏 */
        .navbar {
            background: white;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            padding: 16px 0;
            position: sticky;
            top: 0;
            z-index: 100;
            backdrop-filter: blur(8px);
            background: rgba(255,255,255,0.85);
        }

        .navbar .container {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-weight: 700;
            font-size: 1.5rem;
            letter-spacing: -0.5px;
            background: linear-gradient(135deg, #2563eb, #7c3aed);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .nav-links {
            display: flex;
            gap: 32px;
            font-weight: 500;
        }

        .nav-links a {
            color: #475569;
            transition: color 0.2s;
        }

        .nav-links a:hover {
            color: #2563eb;
        }

        .btn-primary {
            background: #2563eb;
            color: white !important;
            padding: 8px 20px;
            border-radius: 30px;
            font-weight: 600;
            transition: background 0.2s;
        }

        .btn-primary:hover {
            background: #1d4ed8 !important;
        }

        /* 主 Banner */
        .hero {
            padding: 80px 0 60px;
            text-align: center;
        }

        .hero h1 {
            font-size: 3rem;
            font-weight: 800;
            letter-spacing: -1px;
            line-height: 1.2;
            margin-bottom: 20px;
        }

        .hero h1 span {
            background: linear-gradient(135deg, #2563eb, #7c3aed);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero p {
            font-size: 1.2rem;
            color: #64748b;
            max-width: 600px;
            margin: 0 auto 32px;
        }

        .hero .btn-group {
            display: flex;
            gap: 16px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .btn {
            display: inline-block;
            padding: 12px 32px;
            border-radius: 40px;
            font-weight: 600;
            transition: all 0.2s;
            cursor: pointer;
            border: none;
        }

        .btn-outline {
            background: transparent;
            border: 2px solid #e2e8f0;
            color: #1e293b;
        }

        .btn-outline:hover {
            border-color: #2563eb;
            background: #f8fafc;
        }

        /* 卡片区 */
        .section-title {
            font-size: 2rem;
            font-weight: 700;
            text-align: center;
            margin-bottom: 48px;
        }

        .cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
            gap: 30px;
            margin-bottom: 60px;
        }

        .card {
            background: white;
            padding: 32px 24px;
            border-radius: 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.04);
            transition: transform 0.2s, box-shadow 0.2s;
            border: 1px solid #f1f5f9;
        }

        .card:hover {
            transform: translateY(-6px);
            box-shadow: 0 12px 24px rgba(0,0,0,0.08);
        }

        .card .icon {
            font-size: 2.4rem;
            margin-bottom: 12px;
        }

        .card h3 {
            font-size: 1.3rem;
            margin-bottom: 8px;
        }

        .card p {
            color: #64748b;
            font-size: 0.95rem;
        }

        /* 团队 */
        .team-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 24px;
            margin: 40px 0 60px;
        }

        .team-member {
            text-align: center;
        }

        .team-member .avatar {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background: #e2e8f0;
            margin: 0 auto 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            font-weight: 600;
            color: #2563eb;
        }

        .team-member h4 {
            font-weight: 600;
        }

        .team-member p {
            color: #94a3b8;
            font-size: 0.9rem;
        }

        /* Footer */
        footer {
            background: white;
            border-top: 1px solid #eef2f6;
            padding: 32px 0;
            margin-top: 40px;
            text-align: center;
            color: #94a3b8;
            font-size: 0.9rem;
        }

        /* 响应式 */
        @media (max-width: 640px) {
            .hero h1 {
                font-size: 2.2rem;
            }
            .nav-links {
                gap: 16px;
                font-size: 0.9rem;
            }
            .nav-links a:not(.btn-primary) {
                display: none;  /* 移动端简化导航 */
            }
        }
    </style>
</head>
<body>

<!-- 导航 -->
<nav class="navbar">
    <div class="container">
        <div class="logo">智启科技</div>
        <div class="nav-links">
            <a href="#">首页</a>
            <a href="#">产品</a>
            <a href="#">关于</a>
            <a href="#" class="btn-primary">立即体验</a>
        </div>
    </div>
</nav>

<!-- 主内容 -->
<main>
    <section class="container hero">
        <h1>让 AI 帮你<span>解放生产力</span></h1>
        <p>开源智能体框架 OpenClaw 中文生态 · 一键部署你的 AI 助手，接入微信、飞书、钉钉。</p>
        <div class="btn-group">
            <a href="#" class="btn btn-primary">开始使用</a>
            <a href="#" class="btn btn-outline">查看文档 →</a>
        </div>
    </section>

    <section class="container">
        <h2 class="section-title">核心能力</h2>
        <div class="cards">
            <div class="card">
                <div class="icon">🤖</div>
                <h3>多模型支持</h3>
                <p>无缝对接 DeepSeek、通义千问、GPT-4 等主流大模型。</p>
            </div>
            <div class="card">
                <div class="icon">📱</div>
                <h3>全平台接入</h3>
                <p>微信、飞书、钉钉、QQ、Telegram 一个都不少。</p>
            </div>
            <div class="card">
                <div class="icon">🔧</div>
                <h3>插件化扩展</h3>
                <p>通过 Skill 机制快速定制专属功能，社区持续贡献。</p>
            </div>
            <div class="card">
                <div class="icon">🔒</div>
                <h3>数据私有化</h3>
                <p>自托管部署，你的数据完全掌控在自己手中。</p>
            </div>
        </div>
    </section>

    <section class="container">
        <h2 class="section-title">核心团队</h2>
        <div class="team-grid">
            <div class="team-member">
                <div class="avatar">张</div>
                <h4>张一鸣</h4>
                <p>创始人 / 架构师</p>
            </div>
            <div class="team-member">
                <div class="avatar">李</div>
                <h4>李欣</h4>
                <p>全栈开发</p>
            </div>
            <div class="team-member">
                <div class="avatar">王</div>
                <h4>王思远</h4>
                <p>AI 算法工程师</p>
            </div>
            <div class="team-member">
                <div class="avatar">陈</div>
                <h4>陈雅</h4>
                <p>产品设计</p>
            </div>
        </div>
    </section>
</main>

<!-- Footer -->
<footer>
    <div class="container">
        <p>© 2026 智启科技 · 本站为示例页面，内容可自由替换</p>
    </div>
</footer>

<!-- 极简交互（无要求，仅演示） -->
<script>
    // 控制台友好提示
    console.log('欢迎！您可以自由修改此页面内容。');
</script>
</body>
</html>
