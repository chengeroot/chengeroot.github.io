<!DOCTYPE html>
<html lang="zh-CN" data-theme="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>陈哥的博客</title>
    <style>
        /* ========== CSS 变量 ========== */
        :root {
            --bg: #fafbfc;
            --surface: #ffffff;
            --text: #1e293b;
            --text-secondary: #64748b;
            --border: #e2e8f0;
            --primary: #2563eb;
            --primary-hover: #1d4ed8;
            --shadow: 0 4px 12px rgba(0,0,0,0.04);
            --radius: 20px;
            --transition: 0.3s ease;
            --font-size: 1rem;
            --header-bg: rgba(255,255,255,0.85);
        }
        [data-theme="dark"] {
            --bg: #0f172a;
            --surface: #1e293b;
            --text: #f1f5f9;
            --text-secondary: #94a3b8;
            --border: #334155;
            --header-bg: rgba(15,23,42,0.9);
            --shadow: 0 4px 12px rgba(0,0,0,0.3);
        }

        * { margin:0; padding:0; box-sizing:border-box; }
        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: var(--bg);
            color: var(--text);
            line-height: 1.6;
            transition: background var(--transition), color var(--transition);
            font-size: var(--font-size);
        }
        .container { max-width: 1200px; margin:0 auto; padding:0 24px; }

        ::-webkit-scrollbar { width:8px; }
        ::-webkit-scrollbar-track { background:var(--bg); }
        ::-webkit-scrollbar-thumb { background:var(--border); border-radius:8px; }

        /* ===== 导航栏 ===== */
        .navbar {
            background: var(--header-bg);
            backdrop-filter: blur(8px);
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            padding: 12px 0;
            position: sticky;
            top:0;
            z-index:100;
            border-bottom:1px solid var(--border);
        }
        .navbar .container {
            display:flex;
            justify-content:space-between;
            align-items:center;
            flex-wrap:wrap;
            gap:8px;
        }
        .logo {
            font-weight:700;
            font-size:1.5rem;
            background: linear-gradient(135deg, var(--primary), #7c3aed);
            -webkit-background-clip:text;
            -webkit-text-fill-color:transparent;
            cursor:pointer;
            letter-spacing:-0.5px;
        }
        .nav-links {
            display:flex;
            gap:16px;
            font-weight:500;
            align-items:center;
            flex-wrap:wrap;
        }
        .nav-links a, .nav-links button {
            color:var(--text-secondary);
            text-decoration:none;
            transition:color 0.2s;
            cursor:pointer;
            background:none;
            border:none;
            font-size:inherit;
            font-family:inherit;
            padding:4px 0;
        }
        .nav-links a:hover, .nav-links button:hover { color:var(--primary); }
        .nav-links .publish-btn {
            background:var(--primary);
            color:white !important;
            padding:6px 18px;
            border-radius:30px;
            font-weight:600;
            border:none;
            cursor:pointer;
            transition:background 0.2s;
        }
        .nav-links .publish-btn:hover { background:var(--primary-hover); }
        .theme-toggle {
            font-size:1.2rem;
            background:var(--surface);
            border:1px solid var(--border);
            border-radius:30px;
            padding:4px 12px;
            cursor:pointer;
            transition:all 0.2s;
            color:var(--text);
        }

        /* ===== 英雄区 ===== */
        .hero { padding:40px 0 20px; text-align:center; }
        .hero h1 { font-size:2.8rem; font-weight:800; letter-spacing:-1px; }
        .hero h1 span { background:linear-gradient(135deg, var(--primary), #7c3aed); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }

        /* ===== 主体布局 ===== */
        .blog-layout {
            display:grid;
            grid-template-columns: 1fr 260px;
            gap:30px;
            margin:20px 0 40px;
        }
        .main-col { min-width:0; }
        .side-col { position:sticky; top:80px; align-self:start; }

        /* 侧栏卡片 */
        .sidebar-card {
            background:var(--surface);
            border-radius:var(--radius);
            padding:20px;
            margin-bottom:24px;
            border:1px solid var(--border);
            box-shadow:var(--shadow);
            transition:background var(--transition), border var(--transition);
        }
        .sidebar-card h3 {
            font-size:1.1rem;
            margin-bottom:12px;
            border-bottom:1px solid var(--border);
            padding-bottom:8px;
            display:flex;
            justify-content:space-between;
            align-items:center;
        }
        .sidebar-card .edit-profile-btn {
            font-size:0.8rem;
            background:var(--border);
            border:none;
            padding:2px 12px;
            border-radius:20px;
            cursor:pointer;
            color:var(--text-secondary);
            transition:0.2s;
        }
        .sidebar-card .edit-profile-btn:hover { background:var(--primary); color:white; }
        .profile-avatar {
            width:80px;
            height:80px;
            border-radius:50%;
            margin:0 auto 12px;
            display:flex;
            align-items:center;
            justify-content:center;
            font-size:2.5rem;
            color:var(--primary);
            background:var(--border);
            overflow:hidden;
            border:2px solid var(--border);
        }
        .profile-avatar img {
            width:100%;
            height:100%;
            object-fit:cover;
        }
        .profile-name { font-weight:700; text-align:center; }
        .profile-bio { font-size:0.9rem; color:var(--text-secondary); text-align:center; margin-top:4px; white-space:pre-wrap; }

        /* ===== 工具栏 ===== */
        .toolbar {
            display:flex;
            flex-wrap:wrap;
            gap:12px;
            margin-bottom:24px;
            align-items:center;
            background:var(--surface);
            padding:12px 20px;
            border-radius:var(--radius);
            border:1px solid var(--border);
        }
        .toolbar .search-box {
            flex:1;
            min-width:150px;
            padding:6px 14px;
            border:1px solid var(--border);
            border-radius:30px;
            background:var(--bg);
            color:var(--text);
            font-size:0.95rem;
            transition:0.2s;
        }
        .toolbar .search-box:focus { outline:none; border-color:var(--primary); }
        .toolbar .filter-tags {
            display:flex;
            flex-wrap:wrap;
            gap:6px;
        }
        .toolbar .filter-tags .tag-item {
            background:var(--border);
            padding:2px 14px;
            border-radius:20px;
            font-size:0.85rem;
            cursor:pointer;
            transition:0.2s;
            color:var(--text-secondary);
            border:1px solid transparent;
        }
        .toolbar .filter-tags .tag-item.active { background:var(--primary); color:white; }
        .toolbar .filter-tags .tag-item:hover { border-color:var(--primary); }
        .toolbar .font-size-controls button {
            background:var(--border);
            border:none;
            border-radius:30px;
            width:28px;
            height:28px;
            font-weight:bold;
            cursor:pointer;
            color:var(--text);
            transition:0.2s;
        }
        .toolbar .font-size-controls button:hover { background:var(--primary); color:white; }

        /* ===== 文章卡片 ===== */
        .cards {
            display:grid;
            grid-template-columns:repeat(auto-fill, minmax(280px,1fr));
            gap:24px;
            margin-bottom:30px;
        }
        .card {
            background:var(--surface);
            border-radius:var(--radius);
            padding:20px 20px 24px;
            border:1px solid var(--border);
            box-shadow:var(--shadow);
            transition: transform 0.2s, box-shadow 0.2s;
            cursor:pointer;
            position:relative;
        }
        .card:hover { transform:translateY(-4px); box-shadow:0 12px 24px rgba(0,0,0,0.08); }
        .card .top-tag { display:flex; justify-content:space-between; align-items:center; margin-bottom:8px; }
        .card .top-tag .pin-badge { font-size:0.75rem; background:var(--primary); color:white; padding:0 10px; border-radius:12px; }
        .card .top-tag .status-badge { font-size:0.75rem; background:var(--border); padding:0 10px; border-radius:12px; color:var(--text-secondary); }
        .card img { width:100%; height:160px; object-fit:cover; border-radius:12px; margin-bottom:12px; }
        .card h3 { font-size:1.2rem; margin-bottom:6px; }
        .card p { color:var(--text-secondary); font-size:0.95rem; display:-webkit-box; -webkit-line-clamp:3; -webkit-box-orient:vertical; overflow:hidden; }
        .card .meta { display:flex; justify-content:space-between; margin-top:12px; font-size:0.85rem; color:var(--text-secondary); }
        .card .meta .tags span { background:var(--border); padding:0 10px; border-radius:12px; margin-right:4px; font-size:0.75rem; display:inline-block; }

        /* ===== 详情弹窗 ===== */
        .overlay {
            display:none;
            position:fixed;
            inset:0;
            background:rgba(0,0,0,0.5);
            backdrop-filter:blur(4px);
            z-index:999;
            justify-content:center;
            align-items:center;
            padding:20px;
        }
        .overlay.active { display:flex; }
        .modal {
            background:var(--surface);
            max-width:820px;
            width:100%;
            max-height:90vh;
            overflow-y:auto;
            border-radius:var(--radius);
            padding:32px 36px 40px;
            box-shadow:0 24px 48px rgba(0,0,0,0.2);
            animation:fadeUp 0.3s ease;
            position:relative;
        }
        @keyframes fadeUp { from{ opacity:0; transform:translateY(30px); } to{ opacity:1; transform:translateY(0); } }
        .modal .close {
            position:sticky;
            top:0;
            float:right;
            background:var(--border);
            border:none;
            width:36px;
            height:36px;
            border-radius:50%;
            font-size:1.2rem;
            cursor:pointer;
            transition:0.2s;
            display:flex;
            align-items:center;
            justify-content:center;
            margin-bottom:12px;
            color:var(--text);
        }
        .modal .close:hover { background:var(--primary); color:white; }
        .modal img { width:100%; max-height:300px; object-fit:cover; border-radius:16px; margin-bottom:16px; }
        .modal h2 { font-size:2rem; margin-bottom:4px; }
        .modal .date { color:var(--text-secondary); margin-bottom:12px; display:block; font-size:0.9rem; }
        .modal .tags { margin-bottom:16px; }
        .modal .tags span { background:var(--border); padding:2px 14px; border-radius:20px; font-size:0.85rem; margin-right:6px; display:inline-block; color:var(--text-secondary); }
        .modal .content { white-space:pre-wrap; line-height:1.8; font-size:1rem; }
        .modal .content h1, .modal .content h2, .modal .content h3 { margin:16px 0 8px; }
        .modal .action-buttons {
            display:flex;
            gap:12px;
            margin-top:24px;
            justify-content:flex-end;
            border-top:1px solid var(--border);
            padding-top:20px;
            flex-wrap:wrap;
        }
        .modal .action-buttons button {
            padding:6px 20px;
            border:none;
            border-radius:30px;
            font-weight:600;
            cursor:pointer;
            transition:0.2s;
        }
        .modal .action-buttons .edit-btn { background:var(--primary); color:white; }
        .modal .action-buttons .edit-btn:hover { background:var(--primary-hover); }
        .modal .action-buttons .delete-btn { background:#ef4444; color:white; }
        .modal .action-buttons .delete-btn:hover { background:#dc2626; }
        .modal .action-buttons .pin-btn { background:var(--border); color:var(--text); }
        .modal .action-buttons .pin-btn:hover { background:var(--primary); color:white; }
        .modal .related-posts { margin-top:20px; border-top:1px solid var(--border); padding-top:16px; }
        .modal .related-posts h4 { margin-bottom:8px; }
        .modal .related-posts .related-item { display:inline-block; margin-right:12px; font-size:0.9rem; color:var(--primary); cursor:pointer; }
        .modal .related-posts .related-item:hover { text-decoration:underline; }

        /* ===== 表单 ===== */
        .publish-form label { display:block; font-weight:600; margin-top:16px; }
        .publish-form input, .publish-form textarea, .publish-form select {
            width:100%;
            padding:10px 14px;
            border:1px solid var(--border);
            border-radius:12px;
            background:var(--bg);
            color:var(--text);
            font-size:1rem;
            margin-top:4px;
            transition:0.2s;
        }
        .publish-form input:focus, .publish-form textarea:focus { outline:none; border-color:var(--primary); }
        .publish-form textarea { height:120px; }
        .publish-form .submit-btn {
            background:var(--primary);
            color:white;
            border:none;
            padding:12px 28px;
            border-radius:30px;
            font-weight:600;
            font-size:1rem;
            cursor:pointer;
            margin-top:20px;
            width:100%;
            transition:0.2s;
        }
        .publish-form .submit-btn:hover { background:var(--primary-hover); }

        /* 头像上传区域 */
        .avatar-upload {
            display:flex;
            gap:12px;
            align-items:center;
            margin-top:4px;
            flex-wrap:wrap;
        }
        .avatar-upload img {
            width:60px;
            height:60px;
            border-radius:50%;
            object-fit:cover;
            border:2px solid var(--border);
            background:var(--bg);
        }
        .avatar-upload input[type="text"] {
            flex:1;
            min-width:150px;
            padding:8px 12px;
            border:1px solid var(--border);
            border-radius:20px;
            background:var(--bg);
            color:var(--text);
        }
        .avatar-upload .upload-btn {
            padding:6px 16px;
            border-radius:20px;
            border:1px solid var(--border);
            background:var(--bg);
            cursor:pointer;
            color:var(--text);
            transition:0.2s;
        }
        .avatar-upload .upload-btn:hover { background:var(--primary); color:white; border-color:var(--primary); }

        /* ===== 归档 ===== */
        .archive-item { margin-bottom:8px; }
        .archive-item .year { font-weight:700; }
        .archive-item .month-list { padding-left:20px; }
        .archive-item .month-list .post-link { display:block; font-size:0.9rem; color:var(--text-secondary); cursor:pointer; }
        .archive-item .month-list .post-link:hover { color:var(--primary); }

        /* ===== 底部 ===== */
        footer {
            background:var(--surface);
            border-top:1px solid var(--border);
            padding:24px 0;
            margin-top:40px;
            text-align:center;
            color:var(--text-secondary);
            font-size:0.9rem;
        }

        @media (max-width:860px) {
            .blog-layout { grid-template-columns:1fr; }
            .side-col { position:static; margin-top:20px; }
            .hero h1 { font-size:2rem; }
        }
        @media (max-width:480px) {
            .navbar .container { flex-direction:column; align-items:stretch; }
            .nav-links { justify-content:center; }
            .modal { padding:20px; }
            .toolbar { flex-direction:column; align-items:stretch; }
        }
    </style>
</head>
<body>

<!-- ===== 导航栏 ===== -->
<nav class="navbar">
    <div class="container">
        <div class="logo" id="homeLink">📝 陈哥的博客</div>
        <div class="nav-links">
            <button id="homeLink2">首页</button>
            <a href="https://space.bilibili.com/3546789558880461?spm_id_from=333.1007.0.0" target="_blank">主播空间</a>
            <button id="publishBtn" class="publish-btn">发布</button>
            <button class="theme-toggle" id="themeToggle" title="切换暗黑模式">🌙</button>
        </div>
    </div>
</nav>

<!-- ===== 主体 ===== -->
<main>
    <section class="container hero">
        <h1>欢迎来到<span>陈哥的博客</span></h1>
    </section>

    <div class="container blog-layout">
        <!-- 主栏 -->
        <div class="main-col">
            <!-- 工具栏 -->
            <div class="toolbar">
                <input class="search-box" id="searchInput" placeholder="🔍 搜索文章..." />
                <div class="filter-tags" id="filterTags">
                    <span class="tag-item active" data-tag="all">全部</span>
                </div>
                <div class="font-size-controls">
                    <button id="fontSmall">A-</button>
                    <button id="fontMedium">A</button>
                    <button id="fontLarge">A+</button>
                </div>
                <button id="exportData" style="background:var(--border);border:none;border-radius:30px;padding:4px 16px;cursor:pointer;color:var(--text);">导出</button>
                <button id="importData" style="background:var(--border);border:none;border-radius:30px;padding:4px 16px;cursor:pointer;color:var(--text);">导入</button>
                <input type="file" id="importFile" accept=".json" style="display:none;" />
            </div>

            <!-- 文章列表 -->
            <div class="cards" id="postCards"></div>

            <!-- 归档 -->
            <div class="sidebar-card" style="margin-top:24px;">
                <h3>📅 归档</h3>
                <div id="archiveContainer"></div>
            </div>
        </div>

        <!-- 侧栏 -->
        <div class="side-col">
            <!-- 个人简介 -->
            <div class="sidebar-card" id="profileCard">
                <h3>👤 关于我 <button class="edit-profile-btn" id="editProfileBtn">编辑</button></h3>
                <div class="profile-avatar" id="profileAvatar">
                    <span id="avatarEmoji">🧑</span>
                    <img id="avatarImage" style="display:none;" />
                </div>
                <div class="profile-name" id="profileNameDisplay">博主</div>
                <div class="profile-bio" id="profileBioDisplay">这里是个人简介，点击编辑修改。</div>
            </div>

            <!-- 标签云 -->
            <div class="sidebar-card">
                <h3>🏷️ 标签</h3>
                <div id="tagCloud" style="display:flex;flex-wrap:wrap;gap:6px;"></div>
            </div>

            <!-- 统计 -->
            <div class="sidebar-card">
                <h3>📊 统计</h3>
                <div>文章总数：<span id="totalPosts">0</span></div>
                <div>标签数：<span id="totalTags">0</span></div>
            </div>
        </div>
    </div>
</main>

<!-- ===== 详情弹窗 ===== -->
<div class="overlay" id="modalOverlay">
    <div class="modal">
        <button class="close" id="closeModal">✕</button>
        <img id="modalImage" src="" alt="文章配图" />
        <h2 id="modalTitle"></h2>
        <span class="date" id="modalDate"></span>
        <div class="tags" id="modalTags"></div>
        <div class="content" id="modalContent"></div>
        <div class="action-buttons" id="actionButtons">
            <button class="edit-btn" id="editPostBtn">编辑</button>
            <button class="delete-btn" id="deletePostBtn">删除</button>
            <button class="pin-btn" id="pinPostBtn">置顶</button>
        </div>
        <div class="related-posts" id="relatedPosts" style="display:none;">
            <h4>📖 相关文章</h4>
            <div id="relatedList"></div>
        </div>
        <div style="position:sticky;bottom:0;background:var(--surface);padding:8px 0 0;border-top:1px solid var(--border);margin-top:16px;">
            <div style="height:4px;background:var(--border);border-radius:4px;overflow:hidden;">
                <div id="readingProgress" style="width:0%;height:100%;background:var(--primary);transition:width 0.1s;"></div>
            </div>
        </div>
    </div>
</div>

<!-- ===== 发布弹窗 ===== -->
<div class="overlay" id="publishOverlay">
    <div class="modal">
        <button class="close" id="closePublish">✕</button>
        <h2 style="margin-bottom:16px;">发布新文章</h2>
        <form class="publish-form" id="publishForm">
            <label>标题 *</label>
            <input type="text" id="pubTitle" required />
            <label>摘要</label>
            <input type="text" id="pubSummary" placeholder="可选" />
            <label>正文 *</label>
            <textarea id="pubContent" required></textarea>
            <label>标签（逗号分隔）</label>
            <input type="text" id="pubTags" placeholder="AI, 编程, 生活" />
            <label>图片链接</label>
            <input type="text" id="pubImage" placeholder="https://example.com/image.jpg（可选）" />
            <label>状态</label>
            <select id="pubStatus">
                <option value="published">发布</option>
                <option value="draft">草稿</option>
            </select>
            <button type="submit" class="submit-btn">发布文章</button>
        </form>
    </div>
</div>

<!-- ===== 编辑弹窗 ===== -->
<div class="overlay" id="editOverlay">
    <div class="modal">
        <button class="close" id="closeEdit">✕</button>
        <h2 style="margin-bottom:16px;">编辑文章</h2>
        <form class="publish-form" id="editForm">
            <input type="hidden" id="editPostId" />
            <label>标题 *</label>
            <input type="text" id="editTitle" required />
            <label>摘要</label>
            <input type="text" id="editSummary" placeholder="可选" />
            <label>正文 *</label>
            <textarea id="editContent" required></textarea>
            <label>标签（逗号分隔）</label>
            <input type="text" id="editTags" />
            <label>图片链接</label>
            <input type="text" id="editImage" placeholder="https://example.com/image.jpg" />
            <label>状态</label>
            <select id="editStatus">
                <option value="published">发布</option>
                <option value="draft">草稿</option>
            </select>
            <button type="submit" class="submit-btn">保存修改</button>
        </form>
    </div>
</div>

<!-- ===== 编辑个人简介弹窗（含头像） ===== -->
<div class="overlay" id="profileOverlay">
    <div class="modal" style="max-width:500px;">
        <button class="close" id="closeProfile">✕</button>
        <h2 style="margin-bottom:16px;">编辑个人简介</h2>
        <form id="profileForm">
            <label>昵称</label>
            <input type="text" id="profileNameInput" />
            <label>简介</label>
            <textarea id="profileBioInput" rows="4" placeholder="介绍一下自己"></textarea>
            <label>头像</label>
            <div class="avatar-upload">
                <img id="avatarPreview" src="" alt="头像预览" />
                <input type="text" id="profileAvatarInput" placeholder="头像URL或base64" />
                <button type="button" class="upload-btn" id="uploadAvatarBtn">上传图片</button>
                <input type="file" id="avatarFileInput" accept="image/*" style="display:none;" />
            </div>
            <button type="submit" class="submit-btn">保存</button>
        </form>
    </div>
</div>

<footer>
    <div class="container">
        <p>© 2026 陈哥的博客</p>
    </div>
</footer>

<script>
    // ================================================================
    // 数据管理
    // ================================================================
    const STORAGE_KEY = 'my_blog_posts';
    const PROFILE_KEY = 'my_blog_profile';
    const THEME_KEY = 'my_blog_theme';
    const FONT_KEY = 'my_blog_font';

    // 生成最新的说明文章内容（包含美化后的审核提示）
    function getGuideContent() {
        return `欢迎使用陈哥的博客！以下是一些重要信息，请仔细阅读：

📁 **数据存储**
本博客所有文章、个人简介、头像、主题偏好、字体大小等数据均保存在您当前浏览器的 localStorage（本地存储）中。这意味着：
- 数据仅存在于您当前使用的浏览器和设备上。
- 清除浏览器缓存、更换设备或使用其他浏览器，数据将不会同步，并且可能丢失。
- 建议定期使用工具栏的“导出”功能备份您的数据，以防意外丢失。

🔧 **功能概览**
- 发布文章：支持标题、正文、摘要、标签、图片链接、草稿/发布状态。
- 管理文章：编辑、删除、置顶（在文章详情页操作）。
- 分类筛选：点击标签可快速筛选同标签文章。
- 全文搜索：支持标题、摘要、正文关键词搜索。
- 暗黑模式：点击导航栏月亮/太阳图标切换，自动记忆偏好。
- 字体调节：三档字号（A- / A / A+）满足不同阅读需求。
- 个人简介：可自定义昵称、简介和头像（支持上传本地图片）。
- 数据导入导出：一键导出 JSON 备份，或导入之前备份的数据。

📢 **关于文章审核与公开**
本博客当前为个人单机版，所有文章仅您自己可见。若您希望将文章分享给更多人，可联系作者提交审核。内容优质、经过确认的文章，将有机会被收录至公开版本中，供所有访客观看。
同样，如果您对博客功能有改进想法，并制作了修改后的版本，也欢迎提交。经作者审核后，优秀的改进将有机会被采纳并公开。

感谢使用，祝您写作愉快！`;
    }

    // 默认文章（包含说明文章 + 三篇示例）
    function getDefaultPosts() {
        return [
            {
                id: 1,
                title: '📌 博客使用说明 & 重要提示',
                date: new Date().toISOString().slice(0,10),
                summary: '了解本博客的所有功能和数据存储方式',
                image: 'https://picsum.photos/seed/info/600/300',
                content: getGuideContent(),
                tags: ['说明', '功能', '提示'],
                pinned: true,
                status: 'published'
            },
            {
                id: 2,
                title: '初识 OpenClaw：AI 智能体入门',
                date: '2026-06-18',
                summary: 'OpenClaw 是一个开源的 AI 智能体框架，本文介绍它的基本概念和快速上手。',
                image: 'https://picsum.photos/seed/1/600/300',
                content: `OpenClaw 是一个强大的开源 AI 智能体框架，它允许你将大语言模型连接到各种工具和平台。\n\n核心特性包括：\n- 支持微信、飞书、钉钉、QQ 等多渠道接入\n- 可自托管，数据完全私有\n- 插件化设计，扩展性强\n\n如果你对 AI 自动化感兴趣，OpenClaw 绝对值得一试。`,
                tags: ['AI', '开源'],
                pinned: false,
                status: 'published'
            },
            {
                id: 3,
                title: '使用静态博客记录日常',
                date: '2026-06-17',
                summary: '为什么选择静态博客？因为它简单、快速、安全，而且可以免费托管。',
                image: 'https://picsum.photos/seed/2/600/300',
                content: `静态博客的优点非常突出：\n- 无需数据库，所有内容都是文件\n- 加载速度极快，对 SEO 友好\n- 可以托管在 GitHub Pages、Vercel 等免费服务上\n\n这篇博客本身就是一个静态页面的示例，所有文章数据都写在 JavaScript 数组中。`,
                tags: ['博客', '前端'],
                pinned: false,
                status: 'published'
            },
            {
                id: 4,
                title: '前端开发中的设计模式',
                date: '2026-06-15',
                summary: '聊聊前端常用的几种设计模式，以及它们如何让代码更优雅。',
                image: 'https://picsum.photos/seed/3/600/300',
                content: `设计模式是软件工程中解决特定问题的经典方案。在前端领域，以下几种模式尤其常用：\n\n1. **观察者模式**（如 Vue 的响应式）\n2. **工厂模式**（创建复杂对象）\n3. **单例模式**（全局状态管理）\n\n掌握这些模式能让你的代码更具可维护性和可扩展性。`,
                tags: ['前端', 'JavaScript'],
                pinned: false,
                status: 'published'
            }
        ];
    }

    function getPosts() {
        let posts = [];
        const stored = localStorage.getItem(STORAGE_KEY);
        if (stored) {
            try {
                posts = JSON.parse(stored);
                // 查找是否存在说明文章（标题含“说明”或“提示”）
                let guideIndex = posts.findIndex(p => p.title && (p.title.includes('说明') || p.title.includes('提示')));
                if (guideIndex !== -1) {
                    // 更新其内容和置顶状态
                    posts[guideIndex].content = getGuideContent();
                    posts[guideIndex].pinned = true;
                    posts[guideIndex].date = new Date().toISOString().slice(0,10);
                    if (!posts[guideIndex].tags || !posts[guideIndex].tags.includes('说明')) {
                        posts[guideIndex].tags = ['说明', '功能', '提示'];
                    }
                    localStorage.setItem(STORAGE_KEY, JSON.stringify(posts));
                } else {
                    const guide = getDefaultPosts()[0];
                    const maxId = posts.reduce((max, p) => Math.max(max, p.id), 0);
                    guide.id = maxId + 1;
                    posts.push(guide);
                    localStorage.setItem(STORAGE_KEY, JSON.stringify(posts));
                }
                return posts;
            } catch {
                posts = getDefaultPosts();
                localStorage.setItem(STORAGE_KEY, JSON.stringify(posts));
                return posts;
            }
        } else {
            posts = getDefaultPosts();
            localStorage.setItem(STORAGE_KEY, JSON.stringify(posts));
            return posts;
        }
    }

    function savePosts(posts) { localStorage.setItem(STORAGE_KEY, JSON.stringify(posts)); }

    function getProfile() {
        const stored = localStorage.getItem(PROFILE_KEY);
        if (stored) {
            try { return JSON.parse(stored); }
            catch { return { name:'博主', bio:'这里是个人简介，点击编辑修改。', avatar:'' }; }
        } else {
            const defaultProfile = { name:'博主', bio:'这里是个人简介，点击编辑修改。', avatar:'' };
            localStorage.setItem(PROFILE_KEY, JSON.stringify(defaultProfile));
            return defaultProfile;
        }
    }
    function saveProfile(profile) { localStorage.setItem(PROFILE_KEY, JSON.stringify(profile)); }

    function getTheme() { return localStorage.getItem(THEME_KEY) || 'light'; }
    function setTheme(theme) { localStorage.setItem(THEME_KEY, theme); document.documentElement.setAttribute('data-theme', theme); }

    function getFontSize() { return localStorage.getItem(FONT_KEY) || '1rem'; }
    function setFontSize(size) { localStorage.setItem(FONT_KEY, size); document.documentElement.style.setProperty('--font-size', size); }

    // ================================================================
    // 核心功能
    // ================================================================
    let currentPostId = null;
    let allPosts = [];
    let currentFilter = 'all';
    let searchKeyword = '';

    function getPublishedPosts() {
        return allPosts.filter(p => p.status !== 'draft');
    }

    function getAllTags() {
        const tagSet = new Set();
        allPosts.forEach(p => {
            if (p.tags && p.tags.length) p.tags.forEach(t => tagSet.add(t.trim()));
        });
        return [...tagSet];
    }

    function renderPosts() {
        let filtered = allPosts.filter(p => p.status !== 'draft');
        if (searchKeyword.trim()) {
            const kw = searchKeyword.trim().toLowerCase();
            filtered = filtered.filter(p => 
                p.title.toLowerCase().includes(kw) ||
                (p.summary && p.summary.toLowerCase().includes(kw)) ||
                p.content.toLowerCase().includes(kw)
            );
        }
        if (currentFilter !== 'all') {
            filtered = filtered.filter(p => p.tags && p.tags.includes(currentFilter));
        }
        filtered.sort((a,b) => {
            if (a.pinned && !b.pinned) return -1;
            if (!a.pinned && b.pinned) return 1;
            return new Date(b.date) - new Date(a.date);
        });

        const container = document.getElementById('postCards');
        if (!filtered.length) {
            container.innerHTML = `<p style="text-align:center;color:var(--text-secondary);">没有匹配的文章</p>`;
            return;
        }
        let html = '';
        filtered.forEach(post => {
            html += `
                <div class="card" data-id="${post.id}">
                    <div class="top-tag">
                        ${post.pinned ? '<span class="pin-badge">📌 置顶</span>' : '<span></span>'}
                        <span class="status-badge">${post.status === 'draft' ? '草稿' : '公开'}</span>
                    </div>
                    <img src="${post.image || 'https://picsum.photos/seed/default/600/300'}" alt="${post.title}" loading="lazy" />
                    <h3>${post.title}</h3>
                    <p>${post.summary || post.content.slice(0, 80)}...</p>
                    <div class="meta">
                        <span>${post.date}</span>
                        <span class="tags">${(post.tags||[]).map(t => `<span>${t}</span>`).join('')}</span>
                    </div>
                </div>
            `;
        });
        container.innerHTML = html;

        document.querySelectorAll('.card').forEach(card => {
            card.addEventListener('click', function() {
                const id = parseInt(this.dataset.id);
                const post = allPosts.find(p => p.id === id);
                if (post) openModal(post);
            });
        });

        document.getElementById('totalPosts').textContent = filtered.length;
        const tags = getAllTags();
        document.getElementById('totalTags').textContent = tags.length;
        updateTagCloud(tags);
        updateArchive(filtered);
    }

    function updateTagCloud(tags) {
        const container = document.getElementById('tagCloud');
        container.innerHTML = tags.map(t => 
            `<span style="background:var(--border);padding:2px 12px;border-radius:20px;font-size:0.85rem;cursor:pointer;color:var(--text-secondary);" data-tag="${t}">${t}</span>`
        ).join('');
        container.querySelectorAll('[data-tag]').forEach(el => {
            el.addEventListener('click', function() {
                const tag = this.dataset.tag;
                currentFilter = tag;
                document.querySelectorAll('#filterTags .tag-item').forEach(item => {
                    item.classList.toggle('active', item.dataset.tag === tag);
                });
                renderPosts();
            });
        });
    }

    function updateArchive(posts) {
        const container = document.getElementById('archiveContainer');
        const groups = {};
        posts.forEach(p => {
            const parts = p.date.split('-');
            const key = parts[0] + '-' + parts[1];
            if (!groups[key]) groups[key] = [];
            groups[key].push(p);
        });
        const sortedKeys = Object.keys(groups).sort((a,b) => b.localeCompare(a));
        let html = '';
        sortedKeys.forEach(key => {
            const [year, month] = key.split('-');
            html += `<div class="archive-item"><span class="year">${year}年${parseInt(month)}月</span>`;
            html += `<div class="month-list">`;
            groups[key].forEach(p => {
                html += `<span class="post-link" data-id="${p.id}">${p.title}</span>`;
            });
            html += `</div></div>`;
        });
        container.innerHTML = html;
        container.querySelectorAll('.post-link').forEach(el => {
            el.addEventListener('click', function() {
                const id = parseInt(this.dataset.id);
                const post = allPosts.find(p => p.id === id);
                if (post) openModal(post);
            });
        });
    }

    function updateFilterTags() {
        const container = document.getElementById('filterTags');
        const tags = getAllTags();
        let html = `<span class="tag-item active" data-tag="all">全部</span>`;
        tags.forEach(t => {
            html += `<span class="tag-item" data-tag="${t}">${t}</span>`;
        });
        container.innerHTML = html;
        container.querySelectorAll('.tag-item').forEach(el => {
            el.addEventListener('click', function() {
                const tag = this.dataset.tag;
                currentFilter = tag;
                document.querySelectorAll('#filterTags .tag-item').forEach(item => item.classList.remove('active'));
                this.classList.add('active');
                renderPosts();
            });
        });
    }

    function openModal(post) {
        currentPostId = post.id;
        document.getElementById('modalImage').src = post.image || 'https://picsum.photos/seed/default/600/300';
        document.getElementById('modalTitle').textContent = post.title;
        document.getElementById('modalDate').textContent = post.date;
        document.getElementById('modalTags').innerHTML = (post.tags||[]).map(t => `<span>${t}</span>`).join('');
        document.getElementById('modalContent').textContent = post.content;
        const actionDiv = document.getElementById('actionButtons');
        actionDiv.innerHTML = `
            <button class="edit-btn" id="editPostBtn">编辑</button>
            <button class="delete-btn" id="deletePostBtn">删除</button>
            <button class="pin-btn" id="pinPostBtn">${post.pinned ? '取消置顶' : '置顶'}</button>
        `;
        document.getElementById('editPostBtn').addEventListener('click', () => { closeModal(); openEdit(post); });
        document.getElementById('deletePostBtn').addEventListener('click', () => deletePost(post.id));
        document.getElementById('pinPostBtn').addEventListener('click', () => togglePin(post.id));

        const related = getRelatedPosts(post);
        const relatedContainer = document.getElementById('relatedPosts');
        const relatedList = document.getElementById('relatedList');
        if (related.length) {
            relatedContainer.style.display = 'block';
            relatedList.innerHTML = related.map(p => `<span class="related-item" data-id="${p.id}">${p.title}</span>`).join('');
            relatedList.querySelectorAll('.related-item').forEach(el => {
                el.addEventListener('click', function() {
                    const id = parseInt(this.dataset.id);
                    const p = allPosts.find(po => po.id === id);
                    if (p) { closeModal(); openModal(p); }
                });
            });
        } else {
            relatedContainer.style.display = 'none';
        }

        document.getElementById('modalOverlay').classList.add('active');
        document.body.style.overflow = 'hidden';
        const modalContent = document.getElementById('modalContent');
        const progress = document.getElementById('readingProgress');
        modalContent.onscroll = function() {
            const scrollTop = this.scrollTop;
            const scrollHeight = this.scrollHeight - this.clientHeight;
            const percent = scrollHeight ? (scrollTop / scrollHeight) * 100 : 0;
            progress.style.width = percent + '%';
        };
    }

    function closeModal() {
        document.getElementById('modalOverlay').classList.remove('active');
        document.body.style.overflow = '';
        currentPostId = null;
    }

    function getRelatedPosts(post, limit=3) {
        if (!post.tags || !post.tags.length) return [];
        const others = allPosts.filter(p => p.id !== post.id && p.status !== 'draft');
        const scored = others.map(p => {
            const common = (p.tags||[]).filter(t => post.tags.includes(t)).length;
            return { ...p, score: common };
        });
        scored.sort((a,b) => b.score - a.score);
        return scored.slice(0, limit).filter(p => p.score > 0);
    }

    function deletePost(id) {
        if (!confirm('确定删除？不可撤销！')) return;
        allPosts = allPosts.filter(p => p.id !== id);
        savePosts(allPosts);
        renderPosts();
        closeModal();
    }

    function togglePin(id) {
        const post = allPosts.find(p => p.id === id);
        if (post) {
            post.pinned = !post.pinned;
            savePosts(allPosts);
            renderPosts();
            closeModal();
            openModal(post);
        }
    }

    function openEdit(post) {
        document.getElementById('editPostId').value = post.id;
        document.getElementById('editTitle').value = post.title;
        document.getElementById('editSummary').value = post.summary || '';
        document.getElementById('editContent').value = post.content;
        document.getElementById('editTags').value = (post.tags||[]).join(', ');
        document.getElementById('editImage').value = post.image || '';
        document.getElementById('editStatus').value = post.status || 'published';
        document.getElementById('editOverlay').classList.add('active');
        document.body.style.overflow = 'hidden';
    }

    document.getElementById('editForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const id = parseInt(document.getElementById('editPostId').value);
        const title = document.getElementById('editTitle').value.trim();
        const summary = document.getElementById('editSummary').value.trim();
        const content = document.getElementById('editContent').value.trim();
        const tags = document.getElementById('editTags').value.split(',').map(s=>s.trim()).filter(Boolean);
        const image = document.getElementById('editImage').value.trim();
        const status = document.getElementById('editStatus').value;
        if (!title || !content) { alert('标题和正文不能为空'); return; }
        const idx = allPosts.findIndex(p => p.id === id);
        if (idx === -1) return;
        allPosts[idx] = { ...allPosts[idx], title, summary, content, tags, image, status };
        savePosts(allPosts);
        renderPosts();
        closeEdit();
        openModal(allPosts[idx]);
    });

    function closeEdit() {
        document.getElementById('editOverlay').classList.remove('active');
        document.body.style.overflow = '';
    }

    document.getElementById('publishBtn').addEventListener('click', function() {
        document.getElementById('publishOverlay').classList.add('active');
        document.body.style.overflow = 'hidden';
    });
    document.getElementById('closePublish').addEventListener('click', closePublish);
    document.getElementById('publishOverlay').addEventListener('click', function(e) {
        if (e.target === this) closePublish();
    });
    function closePublish() {
        document.getElementById('publishOverlay').classList.remove('active');
        document.body.style.overflow = '';
    }

    document.getElementById('publishForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const title = document.getElementById('pubTitle').value.trim();
        const summary = document.getElementById('pubSummary').value.trim();
        const content = document.getElementById('pubContent').value.trim();
        const tags = document.getElementById('pubTags').value.split(',').map(s=>s.trim()).filter(Boolean);
        const image = document.getElementById('pubImage').value.trim();
        const status = document.getElementById('pubStatus').value;
        if (!title || !content) { alert('标题和正文不能为空'); return; }
        const newId = allPosts.length ? Math.max(...allPosts.map(p=>p.id)) + 1 : 1;
        const newPost = {
            id: newId,
            title,
            summary: summary || '',
            content,
            tags,
            image: image || '',
            date: new Date().toISOString().slice(0,10),
            pinned: false,
            status
        };
        allPosts.push(newPost);
        savePosts(allPosts);
        renderPosts();
        closePublish();
        document.getElementById('publishForm').reset();
        document.getElementById('postListSection').scrollIntoView({ behavior:'smooth' });
    });

    function loadProfile() {
        const profile = getProfile();
        document.getElementById('profileNameDisplay').textContent = profile.name;
        document.getElementById('profileBioDisplay').textContent = profile.bio;
        document.getElementById('profileNameInput').value = profile.name;
        document.getElementById('profileBioInput').value = profile.bio;
        const avatar = profile.avatar || '';
        const avatarEmoji = document.getElementById('avatarEmoji');
        const avatarImage = document.getElementById('avatarImage');
        if (avatar) {
            avatarEmoji.style.display = 'none';
            avatarImage.style.display = 'block';
            avatarImage.src = avatar;
        } else {
            avatarEmoji.style.display = 'flex';
            avatarImage.style.display = 'none';
        }
        document.getElementById('avatarPreview').src = avatar || '';
        document.getElementById('profileAvatarInput').value = avatar;
    }

    document.getElementById('editProfileBtn').addEventListener('click', function() {
        document.getElementById('profileOverlay').classList.add('active');
        document.body.style.overflow = 'hidden';
    });
    document.getElementById('closeProfile').addEventListener('click', closeProfile);
    document.getElementById('profileOverlay').addEventListener('click', function(e) {
        if (e.target === this) closeProfile();
    });
    function closeProfile() {
        document.getElementById('profileOverlay').classList.remove('active');
        document.body.style.overflow = '';
    }

    document.getElementById('uploadAvatarBtn').addEventListener('click', function() {
        document.getElementById('avatarFileInput').click();
    });
    document.getElementById('avatarFileInput').addEventListener('change', function(e) {
        const file = this.files[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = function(ev) {
            const dataUrl = ev.target.result;
            document.getElementById('profileAvatarInput').value = dataUrl;
            document.getElementById('avatarPreview').src = dataUrl;
        };
        reader.readAsDataURL(file);
        this.value = '';
    });
    document.getElementById('profileAvatarInput').addEventListener('input', function() {
        document.getElementById('avatarPreview').src = this.value || '';
    });

    document.getElementById('profileForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const name = document.getElementById('profileNameInput').value.trim() || '博主';
        const bio = document.getElementById('profileBioInput').value.trim() || '暂无简介';
        const avatar = document.getElementById('profileAvatarInput').value.trim();
        saveProfile({ name, bio, avatar });
        loadProfile();
        closeProfile();
    });

    document.getElementById('searchInput').addEventListener('input', function() {
        searchKeyword = this.value;
        renderPosts();
    });

    document.getElementById('fontSmall').addEventListener('click', () => setFontSize('0.9rem'));
    document.getElementById('fontMedium').addEventListener('click', () => setFontSize('1rem'));
    document.getElementById('fontLarge').addEventListener('click', () => setFontSize('1.2rem'));

    document.getElementById('themeToggle').addEventListener('click', function() {
        const current = document.documentElement.getAttribute('data-theme');
        const next = current === 'dark' ? 'light' : 'dark';
        setTheme(next);
        this.textContent = next === 'dark' ? '☀️' : '🌙';
    });

    document.getElementById('exportData').addEventListener('click', function() {
        const data = {
            posts: allPosts,
            profile: getProfile(),
            version: '1.0'
        };
        const blob = new Blob([JSON.stringify(data, null, 2)], { type:'application/json' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'blog_backup.json';
        a.click();
        URL.revokeObjectURL(url);
    });

    document.getElementById('importData').addEventListener('click', function() {
        document.getElementById('importFile').click();
    });
    document.getElementById('importFile').addEventListener('change', function(e) {
        const file = this.files[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = function(ev) {
            try {
                const data = JSON.parse(ev.target.result);
                if (data.posts && Array.isArray(data.posts)) {
                    if (confirm('导入将覆盖当前所有数据，确认？')) {
                        allPosts = data.posts;
                        savePosts(allPosts);
                        if (data.profile) {
                            saveProfile(data.profile);
                        }
                        renderPosts();
                        loadProfile();
                        alert('导入成功！');
                    }
                } else {
                    alert('无效的备份文件');
                }
            } catch(err) {
                alert('解析失败');
            }
        };
        reader.readAsText(file);
        this.value = '';
    });

    function goHome() {
        closeModal(); closePublish(); closeEdit(); closeProfile();
        document.getElementById('postListSection').scrollIntoView({ behavior:'smooth' });
    }
    document.getElementById('homeLink').addEventListener('click', goHome);
    document.getElementById('homeLink2').addEventListener('click', goHome);

    document.addEventListener('keydown', function(e) {
        if (e.key === 'Escape') {
            closeModal(); closePublish(); closeEdit(); closeProfile();
        }
    });

    document.getElementById('closeModal').addEventListener('click', closeModal);
    document.getElementById('closeEdit').addEventListener('click', closeEdit);
    document.getElementById('modalOverlay').addEventListener('click', function(e) {
        if (e.target === this) closeModal();
    });
    document.getElementById('editOverlay').addEventListener('click', function(e) {
        if (e.target === this) closeEdit();
    });

    function init() {
        const theme = getTheme();
        setTheme(theme);
        document.getElementById('themeToggle').textContent = theme === 'dark' ? '☀️' : '🌙';
        const fontSize = getFontSize();
        setFontSize(fontSize);
        allPosts = getPosts();
        renderPosts();
        loadProfile();
        updateFilterTags();
    }
    init();
</script>
</body>
</html>
