<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>我的博客</title>
    <style>
        /* ===== 全局重置 ===== */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: #fafbfc;
            color: #1e293b;
            line-height: 1.6;
        }
        .container { max-width: 1200px; margin: 0 auto; padding: 0 24px; }

        /* ===== 导航栏 ===== */
        .navbar {
            background: rgba(255,255,255,0.85);
            backdrop-filter: blur(8px);
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            padding: 16px 0;
            position: sticky;
            top: 0;
            z-index: 100;
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
            cursor: pointer;
        }
        .nav-links { display: flex; gap: 20px; font-weight: 500; align-items: center; }
        .nav-links a { color: #475569; text-decoration: none; transition: color 0.2s; cursor: pointer; }
        .nav-links a:hover { color: #2563eb; }
        .nav-links .publish-btn {
            background: #2563eb;
            color: white !important;
            padding: 6px 18px;
            border-radius: 30px;
            font-weight: 600;
        }
        .nav-links .publish-btn:hover { background: #1d4ed8; }

        /* ===== 英雄区 ===== */
        .hero { padding: 60px 0 40px; text-align: center; }
        .hero h1 { font-size: 3rem; font-weight: 800; letter-spacing: -1px; }
        .hero h1 span { background: linear-gradient(135deg, #2563eb, #7c3aed); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }

        /* ===== 卡片 ===== */
        .section-title { font-size: 2rem; font-weight: 700; text-align: center; margin-bottom: 48px; }
        .cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 30px;
            margin-bottom: 60px;
        }
        .card {
            background: white;
            padding: 20px 20px 24px;
            border-radius: 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.04);
            transition: transform 0.2s, box-shadow 0.2s;
            border: 1px solid #f1f5f9;
            cursor: pointer;
        }
        .card:hover {
            transform: translateY(-6px);
            box-shadow: 0 12px 24px rgba(0,0,0,0.08);
        }
        .card img {
            width: 100%;
            height: 160px;
            object-fit: cover;
            border-radius: 12px;
            margin-bottom: 12px;
        }
        .card h3 { font-size: 1.3rem; margin-bottom: 8px; }
        .card p { color: #64748b; font-size: 0.95rem; }
        .card .date { color: #94a3b8; font-size: 0.85rem; margin-top: 12px; }

        /* ===== 弹窗（文章详情 & 发布表单） ===== */
        .overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.4);
            backdrop-filter: blur(4px);
            z-index: 999;
            justify-content: center;
            align-items: center;
        }
        .overlay.active { display: flex; }
        .modal {
            background: white;
            max-width: 720px;
            width: 90%;
            max-height: 85vh;
            overflow-y: auto;
            border-radius: 24px;
            padding: 32px 32px 40px;
            box-shadow: 0 24px 48px rgba(0,0,0,0.2);
            position: relative;
            animation: fadeUp 0.3s ease;
        }
        @keyframes fadeUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .modal .close {
            position: sticky;
            top: 0;
            float: right;
            background: #f1f5f9;
            border: none;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            font-size: 1.2rem;
            cursor: pointer;
            transition: background 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 12px;
        }
        .modal .close:hover { background: #e2e8f0; }
        .modal img { width: 100%; max-height: 300px; object-fit: cover; border-radius: 16px; margin-bottom: 16px; }
        .modal h2 { font-size: 2rem; margin-bottom: 8px; }
        .modal .date { color: #94a3b8; margin-bottom: 20px; display: block; }
        .modal .content { white-space: pre-wrap; line-height: 1.8; font-size: 1.05rem; }

        /* 发布表单样式 */
        .publish-form label { display: block; font-weight: 600; margin-top: 16px; }
        .publish-form input, .publish-form textarea {
            width: 100%;
            padding: 10px 14px;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            font-size: 1rem;
            margin-top: 4px;
        }
        .publish-form textarea { height: 120px; }
        .publish-form .submit-btn {
            background: #2563eb;
            color: white;
            border: none;
            padding: 12px 28px;
            border-radius: 30px;
            font-weight: 600;
            font-size: 1rem;
            cursor: pointer;
            margin-top: 20px;
            width: 100%;
        }
        .publish-form .submit-btn:hover { background: #1d4ed8; }

        /* ===== 底部 ===== */
        footer {
            background: white;
            border-top: 1px solid #eef2f6;
            padding: 32px 0;
            margin-top: 40px;
            text-align: center;
            color: #94a3b8;
            font-size: 0.9rem;
        }

        /* ===== 响应式 ===== */
        @media (max-width: 640px) {
            .hero h1 { font-size: 2.2rem; }
            .nav-links { gap: 12px; font-size: 0.9rem; }
            .modal { padding: 20px; }
        }
    </style>
</head>
<body>

<!-- 导航栏 -->
<nav class="navbar">
    <div class="container">
        <div class="logo" id="homeLink">📝 我的博客</div>
        <div class="nav-links">
            <a id="homeLink2">首页</a>
            <a id="publishBtn" class="publish-btn">发布</a>
        </div>
    </div>
</nav>

<!-- 主内容 -->
<main>
    <section class="container hero">
        <h1>欢迎来到<span>我的博客</span></h1>
    </section>

    <section class="container" id="postListSection">
        <h2 class="section-title">最新文章</h2>
        <div class="cards" id="postCards">
            <!-- 由 JavaScript 动态渲染 -->
        </div>
    </section>
</main>

<!-- 文章详情弹窗 -->
<div class="overlay" id="modalOverlay">
    <div class="modal">
        <button class="close" id="closeModal">✕</button>
        <img id="modalImage" src="" alt="文章配图" />
        <h2 id="modalTitle"></h2>
        <span class="date" id="modalDate"></span>
        <div class="content" id="modalContent"></div>
    </div>
</div>

<!-- 发布文章弹窗 -->
<div class="overlay" id="publishOverlay">
    <div class="modal">
        <button class="close" id="closePublish">✕</button>
        <h2 style="margin-bottom: 16px;">发布新文章</h2>
        <form class="publish-form" id="publishForm">
            <label>标题 *</label>
            <input type="text" id="pubTitle" required />
            <label>摘要</label>
            <input type="text" id="pubSummary" placeholder="可选" />
            <label>正文 *</label>
            <textarea id="pubContent" required></textarea>
            <label>图片链接</label>
            <input type="text" id="pubImage" placeholder="https://example.com/image.jpg（可选）" />
            <button type="submit" class="submit-btn">发布文章</button>
        </form>
    </div>
</div>

<footer>
    <div class="container">
        <p>© 2026 我的静态博客</p>
    </div>
</footer>

<script>
    // ============================================================
    // 数据管理：从 localStorage 读取，若无则写入示例数据
    // ============================================================
    const STORAGE_KEY = 'my_blog_posts';

    function getPosts() {
        const stored = localStorage.getItem(STORAGE_KEY);
        if (stored) {
            try { return JSON.parse(stored); }
            catch { return []; }
        } else {
            // 首次使用，写入示例文章
            const defaultPosts = [
                {
                    id: 1,
                    title: '初识 OpenClaw：AI 智能体入门',
                    date: '2026-06-18',
                    summary: 'OpenClaw 是一个开源的 AI 智能体框架，本文介绍它的基本概念和快速上手。',
                    image: 'https://picsum.photos/seed/1/600/300',
                    content: `OpenClaw 是一个强大的开源 AI 智能体框架，它允许你将大语言模型连接到各种工具和平台。\n\n核心特性包括：\n- 支持微信、飞书、钉钉、QQ 等多渠道接入\n- 可自托管，数据完全私有\n- 插件化设计，扩展性强\n\n如果你对 AI 自动化感兴趣，OpenClaw 绝对值得一试。`
                },
                {
                    id: 2,
                    title: '使用静态博客记录日常',
                    date: '2026-06-17',
                    summary: '为什么选择静态博客？因为它简单、快速、安全，而且可以免费托管。',
                    image: 'https://picsum.photos/seed/2/600/300',
                    content: `静态博客的优点非常突出：\n- 无需数据库，所有内容都是文件\n- 加载速度极快，对 SEO 友好\n- 可以托管在 GitHub Pages、Vercel 等免费服务上\n\n这篇博客本身就是一个静态页面的示例，所有文章数据都写在 JavaScript 数组中。`
                },
                {
                    id: 3,
                    title: '前端开发中的设计模式',
                    date: '2026-06-15',
                    summary: '聊聊前端常用的几种设计模式，以及它们如何让代码更优雅。',
                    image: 'https://picsum.photos/seed/3/600/300',
                    content: `设计模式是软件工程中解决特定问题的经典方案。在前端领域，以下几种模式尤其常用：\n\n1. **观察者模式**（如 Vue 的响应式）\n2. **工厂模式**（创建复杂对象）\n3. **单例模式**（全局状态管理）\n\n掌握这些模式能让你的代码更具可维护性和可扩展性。`
                }
            ];
            localStorage.setItem(STORAGE_KEY, JSON.stringify(defaultPosts));
            return defaultPosts;
        }
    }

    function savePosts(posts) {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(posts));
    }

    // ============================================================
    // 渲染文章列表
    // ============================================================
    function renderPosts() {
        const posts = getPosts();
        const container = document.getElementById('postCards');
        if (!container) return;
        if (posts.length === 0) {
            container.innerHTML = `<p style="text-align:center;color:#94a3b8;">还没有文章，点击“发布”来写第一篇吧！</p>`;
            return;
        }
        let html = '';
        // 按日期降序（最新的在前）
        const sorted = [...posts].sort((a, b) => new Date(b.date) - new Date(a.date));
        sorted.forEach(post => {
            html += `
                <div class="card" data-id="${post.id}">
                    <img src="${post.image || 'https://picsum.photos/seed/default/600/300'}" alt="${post.title}" loading="lazy" />
                    <h3>${post.title}</h3>
                    <p>${post.summary || post.content.slice(0, 80)}...</p>
                    <div class="date">${post.date}</div>
                </div>
            `;
        });
        container.innerHTML = html;

        // 点击卡片打开详情
        document.querySelectorAll('.card').forEach(card => {
            card.addEventListener('click', function() {
                const id = parseInt(this.dataset.id);
                const posts = getPosts();
                const post = posts.find(p => p.id === id);
                if (post) openModal(post);
            });
        });
    }

    // ============================================================
    // 文章详情弹窗
    // ============================================================
    const modalOverlay = document.getElementById('modalOverlay');
    const modalImage = document.getElementById('modalImage');
    const modalTitle = document.getElementById('modalTitle');
    const modalDate = document.getElementById('modalDate');
    const modalContent = document.getElementById('modalContent');
    const closeModalBtn = document.getElementById('closeModal');

    function openModal(post) {
        modalImage.src = post.image || 'https://picsum.photos/seed/default/600/300';
        modalImage.alt = post.title;
        modalTitle.textContent = post.title;
        modalDate.textContent = post.date;
        modalContent.textContent = post.content;
        modalOverlay.classList.add('active');
        document.body.style.overflow = 'hidden';
    }

    function closeModal() {
        modalOverlay.classList.remove('active');
        document.body.style.overflow = '';
    }

    closeModalBtn.addEventListener('click', closeModal);
    modalOverlay.addEventListener('click', function(e) {
        if (e.target === modalOverlay) closeModal();
    });
    document.addEventListener('keydown', function(e) {
        if (e.key === 'Escape') closeModal();
    });

    // ============================================================
    // 发布文章弹窗
    // ============================================================
    const publishOverlay = document.getElementById('publishOverlay');
    const closePublishBtn = document.getElementById('closePublish');
    const publishForm = document.getElementById('publishForm');

    document.getElementById('publishBtn').addEventListener('click', function() {
        publishOverlay.classList.add('active');
        document.body.style.overflow = 'hidden';
    });

    function closePublish() {
        publishOverlay.classList.remove('active');
        document.body.style.overflow = '';
    }

    closePublishBtn.addEventListener('click', closePublish);
    publishOverlay.addEventListener('click', function(e) {
        if (e.target === publishOverlay) closePublish();
    });

    // 提交发布表单
    publishForm.addEventListener('submit', function(e) {
        e.preventDefault();
        const title = document.getElementById('pubTitle').value.trim();
        const summary = document.getElementById('pubSummary').value.trim();
        const content = document.getElementById('pubContent').value.trim();
        const image = document.getElementById('pubImage').value.trim();

        if (!title || !content) {
            alert('标题和正文不能为空');
            return;
        }

        const posts = getPosts();
        const newId = posts.length > 0 ? Math.max(...posts.map(p => p.id)) + 1 : 1;
        const newPost = {
            id: newId,
            title,
            summary: summary || '',
            content,
            image: image || '',
            date: new Date().toISOString().slice(0, 10) // 格式 YYYY-MM-DD
        };
        posts.push(newPost);
        savePosts(posts);
        renderPosts();
        closePublish();
        publishForm.reset();
        // 可选：滚动到文章列表
        document.getElementById('postListSection').scrollIntoView({ behavior: 'smooth' });
    });

    // ============================================================
    // 首页导航
    // ============================================================
    function goHome() {
        closeModal();
        closePublish();
        document.getElementById('postListSection').scrollIntoView({ behavior: 'smooth' });
    }
    document.getElementById('homeLink').addEventListener('click', goHome);
    document.getElementById('homeLink2').addEventListener('click', goHome);

    // ============================================================
    // 初始化
    // ============================================================
    renderPosts();
</script>

</body>
</html>
