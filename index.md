<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>说说网站（仅管理员+访客）</title>
    <style>
        body {
            font-family: 'Microsoft Yahei', sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .password-screen {
            text-align: center;
            margin-top: 100px;
        }
        #siteContent {
            display: none;
        }
        .admin-only {
            display: none;
        }
        .post-item {
            background: white;
            padding: 15px;
            margin: 10px 0;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .post-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            flex-wrap: wrap;
            gap: 8px;
        }
        .post-author {
            font-weight: bold;
            color: #2c3e50;
        }
        .post-time {
            font-size: 12px;
            color: #999;
        }
        .post-delete {
            background: #e74c3c;
            color: white;
            border: none;
            padding: 4px 8px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
        }
        .post-delete:hover {
            background: #c0392b;
        }
        #postContent {
            width: 100%;
            height: 80px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 8px;
            resize: none;
            box-sizing: border-box;
        }
        .publish-btn {
            background: #3498db;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 10px;
        }
        .publish-btn:hover {
            background: #2980b9;
        }

        /* 聊天样式 */
        .chat-container {
            display: none;
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            margin-top: 20px;
            overflow: hidden;
        }
        .chat-header {
            background: #3498db;
            color: white;
            padding: 10px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .chat-messages {
            height: 300px;
            overflow-y: auto;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .message {
            max-width: 70%;
            padding: 8px 12px;
            border-radius: 18px;
            line-height: 1.4;
        }
        .message.self {
            background: #3498db;
            color: white;
            align-self: flex-end;
        }
        .message.other {
            background: #f1f1f1;
            color: #333;
            align-self: flex-start;
        }
        .message-time {
            font-size: 11px;
            opacity: 0.8;
            margin-top: 2px;
            text-align: right;
        }
        .message.other .message-time {
            text-align: left;
        }
        .chat-input {
            display: flex;
            padding: 10px;
            border-top: 1px solid #eee;
            box-sizing: border-box;
        }
        #chatInput {
            flex: 1;
            padding: 8px 12px;
            border: 1px solid #ddd;
            border-radius: 20px;
            resize: none;
            box-sizing: border-box;
        }
        .send-btn {
            background: #3498db;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 20px;
            cursor: pointer;
            margin-left: 10px;
        }
        .send-btn:hover {
            background: #2980b9;
        }
        .close-chat {
            background: none;
            border: none;
            color: white;
            font-size: 18px;
            cursor: pointer;
            line-height: 1;
        }
    </style>
</head>
<body>
    <!-- 密码验证界面 -->
    <div class="password-screen" id="passwordScreen">
        <h2>请输入密码登录</h2>
        <input type="password" id="passwordInput" placeholder="输入密码">
        <button onclick="checkPassword()" class="publish-btn">登录</button>
        <div id="errorMsg" style="color: red; display: none; margin-top: 10px;">密码错误，请重试</div>
    </div>

    <!-- 网站主内容 -->
    <div id="siteContent">
        <button id="openChatBtn" style="display: none;" class="publish-btn">打开聊天</button>
        <button id="openVisitorListBtn" style="display: none;" class="publish-btn">访客列表</button>
        <button id="openViewRecordBtn" style="display: none;" class="publish-btn">查看记录</button>

        <!-- 密码管理（仅管理员） -->
        <div class="profile-setting admin-only" style="display: none; margin: 20px 0; padding: 15px; background: #fff; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
            <div class="profile-form">
                <h3>🔒 密码管理</h3>
                <div style="margin: 10px 0;">
                    <label>管理员密码：</label>
                    <input type="password" id="newAdminPwd" placeholder="输入新的管理员密码">
                </div>
                <div style="margin: 10px 0;">
                    <label>访客密码：</label>
                    <input type="password" id="newVisitorPwd" placeholder="输入新的访客密码">
                </div>
                <button onclick="savePasswords()" style="background: #ff9500;">保存新密码</button>
            </div>
        </div>

        <!-- 说说发布 -->
        <div class="post-publish" style="margin: 20px 0;">
            <textarea id="postContent" placeholder="写下你的想法..."></textarea>
            <button onclick="publishPost()" class="publish-btn">发布说说</button>
        </div>

        <!-- 说说列表 -->
        <div id="postsContainer"></div>

        <!-- 聊天窗口 -->
        <div class="chat-container" id="chatContainer">
            <div class="chat-header">
                <h3 style="margin: 0;">实时聊天</h3>
                <button class="close-chat" onclick="toggleChat()">×</button>
            </div>
            <div class="chat-messages" id="chatMessages"></div>
            <div class="chat-input">
                <textarea id="chatInput" placeholder="输入消息..."></textarea>
                <button class="send-btn" onclick="sendMessage()">发送</button>
            </div>
        </div>
    </div>

    <script>
        // 全局变量（仅保留管理员/访客）
        let userRole = '';        // 角色：admin/visitor
        let ADMIN_PWD = '';       // 管理员密码
        let VISITOR_PWD = '';     // 访客密码
        let visitorId = 'visitor_' + Date.now(); // 访客唯一ID
        let currentUser = '';     // 当前登录用户名（展示用）

        // 页面加载初始化
        window.onload = function() {
            // 从本地存储读取密码，无则用默认值
            ADMIN_PWD = localStorage.getItem('ADMIN_PWD') || 'admin123';
            VISITOR_PWD = localStorage.getItem('VISITOR_PWD') || 'visitor123';
            // 加载历史说说和聊天记录
            loadPostsFromLocal();
            loadChatMessages();
        }

        // 密码验证登录（仅管理员/访客）
        function checkPassword() {
            const pwd = document.getElementById('passwordInput').value.trim();
            const errorMsg = document.getElementById('errorMsg');
            if (!pwd) return;

            if (pwd === ADMIN_PWD) {
                // 管理员登录
                userRole = "admin";
                currentUser = '管理员';
                document.getElementById('siteContent').style.display = 'block';
                document.getElementById('passwordScreen').style.display = 'none';
                document.getElementById('openVisitorListBtn').style.display = 'inline-block';
                document.getElementById('openViewRecordBtn').style.display = 'inline-block';
                document.querySelector('.admin-only').style.display = 'block';
                document.getElementById('openChatBtn').style.display = 'inline-block';
                initVisitorChat();
            } else if (pwd === VISITOR_PWD) {
                // 访客登录
                userRole = "visitor";
                currentUser = '访客-' + visitorId.slice(-4); // 简化显示访客ID
                document.getElementById('siteContent').style.display = 'block';
                document.getElementById('passwordScreen').style.display = 'none';
                document.getElementById('openChatBtn').style.display = 'inline-block';
                document.body.classList.add('visitor-mode');
                initVisitorChat();
            } else {
                // 密码错误
                errorMsg.style.display = 'block';
                setTimeout(() => {
                    errorMsg.style.display = 'none';
                }, 2000);
            }
        }

        // 保存密码（仅管理员/访客，无创造者）
        function savePasswords() {
            const newAdminPwd = document.getElementById('newAdminPwd').value.trim();
            const newVisitorPwd = document.getElementById('newVisitorPwd').value.trim();

            // 验证密码非空
            if (!newAdminPwd || !newVisitorPwd) {
                alert('管理员密码和访客密码不能为空！');
                return;
            }

            // 保存到本地存储
            localStorage.setItem('ADMIN_PWD', newAdminPwd);
            localStorage.setItem('VISITOR_PWD', newVisitorPwd);
            // 更新全局变量
            ADMIN_PWD = newAdminPwd;
            VISITOR_PWD = newVisitorPwd;

            alert('密码修改成功！下次登录请使用新密码。');
            // 清空输入框
            document.getElementById('newAdminPwd').value = '';
            document.getElementById('newVisitorPwd').value = '';
        }

        // 发布说说（无创造者逻辑）
        function publishPost() {
            const content = document.getElementById('postContent').value.trim();
            if (!content) {
                alert('说说内容不能为空！');
                return;
            }

            // 生成说说唯一ID、时间、发布者信息
            const postId = 'post_' + Date.now();
            const time = new Date().toLocaleString();
            const posterRole = userRole;
            const posterName = currentUser; // 用当前展示名作为发布者

            // 组装说说HTML（所有人的说说都带删除按钮）
            const postHtml = `
                <div class="post-item" data-post-id="${postId}" data-poster-role="${posterRole}" data-poster-name="${posterName}">
                    <div class="post-header">
                        <span class="post-author">${posterName}</span>
                        <span class="post-time">${time}</span>
                    </div>
                    <div class="post-content">${content}</div>
                    <button class="post-delete" onclick="deletePost(this)">删除</button>
                </div>
            `;

            // 保存到本地存储（追加新说说）
            const savedPosts = localStorage.getItem('savedPosts') || '';
            localStorage.setItem('savedPosts', postHtml + savedPosts);
            // 刷新说说列表
            loadPostsFromLocal();
            // 清空输入框
            document.getElementById('postContent').value = '';
        }

        // 删除说说（管理员可删所有，访客仅删自己）
        function deletePost(btn) {
            const postItem = btn.closest('.post-item');
            const posterRole = postItem.dataset.posterRole;
            const posterName = postItem.dataset.posterName;

            // 规则：访客只能删除自己的说说，管理员可删所有
            if (userRole === 'visitor' && posterName !== currentUser) {
                alert('无权删除他人的说说！');
                return;
            }

            // 执行删除
            postItem.remove();
            // 更新本地存储
            const newPosts = document.getElementById('postsContainer').innerHTML;
            localStorage.setItem('savedPosts', newPosts);
        }

        // 加载本地说说
        function loadPostsFromLocal() {
            const savedPosts = localStorage.getItem('savedPosts');
            if (!savedPosts) {
                document.getElementById('postsContainer').innerHTML = '<div style="text-align: center; padding: 20px; background: #fff; border-radius: 8px;">暂无说说，快来发布第一条吧~</div>';
                return;
            }

            document.getElementById('postsContainer').innerHTML = savedPosts;
        }

        // 聊天功能初始化
        function initVisitorChat() {
            document.getElementById('openChatBtn').addEventListener('click', toggleChat);
        }

        // 打开/关闭聊天窗口
        function toggleChat() {
            const chatContainer = document.getElementById('chatContainer');
            if (chatContainer.style.display === 'none' || chatContainer.style.display === '') {
                chatContainer.style.display = 'block';
                // 滚动到最新消息
                const chatMessages = document.getElementById('chatMessages');
                chatMessages.scrollTop = chatMessages.scrollHeight;
            } else {
                chatContainer.style.display = 'none';
            }
        }

        // 发送聊天消息
        function sendMessage() {
            const input = document.getElementById('chatInput');
            const content = input.value.trim();
            if (!content) return;

            // 组装消息对象
            const message = {
                sender: currentUser,
                content: content,
                time: new Date().toLocaleTimeString(),
                role: userRole
            };

            // 读取历史消息，追加新消息并保存
            let messages = JSON.parse(localStorage.getItem('chatMessages')) || [];
            messages.push(message);
            localStorage.setItem('chatMessages', JSON.stringify(messages));

            // 刷新聊天记录
            loadChatMessages();
            // 清空输入框
            input.value = '';
        }

        // 加载聊天记录
        function loadChatMessages() {
            const messages = JSON.parse(localStorage.getItem('chatMessages')) || [];
            const chatMessages = document.getElementById('chatMessages');
            
            if (messages.length === 0) {
                chatMessages.innerHTML = '<div style="text-align: center; color: #999; margin-top: 50%; transform: translateY(-50%);">暂无聊天记录，发第一条消息吧~</div>';
                return;
            }

            chatMessages.innerHTML = '';
            // 遍历渲染消息
            messages.forEach(msg => {
                const messageDiv = document.createElement('div');
                messageDiv.className = `message ${msg.sender === currentUser ? 'self' : 'other'}`;
                messageDiv.innerHTML = `
                    <div>${msg.content}</div>
                    <div class="message-time">${msg.time}</div>
                `;
                chatMessages.appendChild(messageDiv);
            });
            // 自动滚动到最新消息
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }
    </script>
</body>
</html>
