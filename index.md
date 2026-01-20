<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>时光信笺 - 最终完整版</title>
    <style>
        /* 密码验证页 */
        #passwordScreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9999;
        }
        #passwordBox {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0,0,0,0.1);
            text-align: center;
            width: 300px;
        }
        #passwordInput {
            width: 100%;
            padding: 10px;
            margin: 15px 0;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        #passwordBtn {
            background: #07c160;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
        }
        #errorMsg {
            color: red;
            font-size: 12px;
            margin-top: 10px;
            display: none;
        }

        /* 顶部导航栏 */
        .header {
            max-width: 680px;
            margin: 0 auto 20px;
            padding: 10px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #eee;
        }
        .chat-btn, .visitor-list-btn, .view-record-btn, .pet-btn {
            background: #07c160;
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 12px;
            margin-left: 8px;
        }
        .visitor-list-btn {
            background: #2f80ed;
        }
        .view-record-btn {
            background: #ff9500;
        }
        .pet-btn {
            background: #ff2d55;
        }
        /* 未读消息提示角标 */
        .unread-badge {
            display: inline-block;
            background: #ff4545;
            color: white;
            font-size: 10px;
            min-width: 15px;
            height: 15px;
            line-height: 15px;
            border-radius: 8px;
            text-align: center;
            margin-left: 3px;
            vertical-align: top;
            padding: 0 2px;
        }

        /* 访客列表弹窗 */
        .visitor-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 999;
        }
        .visitor-box {
            background: white;
            width: 90%;
            max-width: 500px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.2);
        }
        .visitor-header {
            padding: 12px;
            background: #2f80ed;
            color: white;
            border-radius: 10px 10px 0 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .close-visitor {
            background: transparent;
            border: none;
            color: white;
            font-size: 18px;
            cursor: pointer;
        }
        .visitor-content {
            display: flex;
            height: 500px;
        }
        .visitor-list {
            width: 35%;
            border-right: 1px solid #eee;
            overflow-y: auto;
            padding: 8px;
        }
        .visitor-item {
            padding: 8px 10px;
            border-radius: 5px;
            margin-bottom: 4px;
            cursor: pointer;
            transition: background 0.2s;
        }
        .visitor-item:hover, .visitor-item.active {
            background: #f0f8ff;
        }
        .visitor-id-text {
            font-weight: 500;
            color: #333;
            font-size: 14px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .visitor-time-text {
            font-size: 11px;
            color: #999;
            margin-top: 2px;
        }
        .visitor-chat {
            width: 65%;
            display: flex;
            flex-direction: column;
        }
        .visitor-chat-header {
            padding: 10px;
            border-bottom: 1px solid #eee;
            font-weight: 500;
            text-align: center;
        }
        .visitor-chat-content {
            flex: 1;
            padding: 10px;
            overflow-y: auto;
            background: #f9f9f9;
        }
        .visitor-chat-msg {
            margin-bottom: 8px;
            max-width: 80%;
        }
        .visitor-chat-msg.admin {
            margin-left: auto;
        }
        .visitor-chat-msg.visitor {
            margin-right: auto;
        }
        .visitor-chat-msg .msg-content {
            padding: 6px 10px;
            border-radius: 10px;
            word-wrap: break-word;
            font-size: 13px;
        }
        .visitor-chat-msg.admin .msg-content {
            background: #07c160;
            color: white;
        }
        .visitor-chat-msg.visitor .msg-content {
            background: #e5e5e5;
            color: #333;
        }
        .visitor-chat-msg .msg-status {
            font-size: 10px;
            color: #999;
            text-align: right;
            margin-top: 2px;
        }

        /* 浏览记录弹窗 */
        .view-record-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 999;
        }
        .view-record-box {
            background: white;
            width: 90%;
            max-width: 500px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.2);
            max-height: 80vh;
        }
        .view-record-header {
            padding: 12px;
            background: #ff9500;
            color: white;
            border-radius: 10px 10px 0 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .close-view-record {
            background: transparent;
            border: none;
            color: white;
            font-size: 18px;
            cursor: pointer;
        }
        .view-record-content {
            padding: 16px;
            max-height: 60vh;
            overflow-y: auto;
        }
        .view-record-item {
            padding: 10px;
            border-radius: 6px;
            background: #f9f9f9;
            margin-bottom: 8px;
        }
        .view-record-post-title {
            font-weight: 500;
            margin-bottom: 6px;
            color: #333;
        }
        .view-record-stats {
            font-size: 13px;
            color: #666;
            margin-bottom: 6px;
        }
        .view-record-list {
            padding-left: 16px;
        }
        .view-record-user {
            font-size: 12px;
            color: #999;
            margin-bottom: 2px;
        }

        /* 私聊弹窗 */
        .chat-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 999;
        }
        .chat-box {
            background: white;
            width: 90%;
            max-width: 400px;
            height: 600px;
            border-radius: 10px;
            display: flex;
            flex-direction: column;
            box-shadow: 0 0 20px rgba(0,0,0,0.2);
        }
        @media (max-width: 480px) {
            .chat-box {
                height: 80vh;
            }
        }
        .chat-header {
            padding: 12px;
            background: #07c160;
            color: white;
            border-radius: 10px 10px 0 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .close-chat {
            background: transparent;
            border: none;
            color: white;
            font-size: 18px;
            cursor: pointer;
        }
        .chat-content {
            flex: 1;
            padding: 12px;
            overflow-y: auto;
            background: #f9f9f9;
        }
        .chat-message {
            margin-bottom: 10px;
            max-width: 80%;
        }
        .admin-msg {
            margin-left: auto;
        }
        .visitor-msg {
            margin-right: auto;
        }
        .msg-content {
            padding: 8px 12px;
            border-radius: 15px;
            word-wrap: break-word;
        }
        .admin-msg .msg-content {
            background: #07c160;
            color: white;
        }
        .visitor-msg .msg-content {
            background: #e5e5e5;
            color: #333;
        }
        /* 消息状态：未读/已读 */
        .msg-status {
            font-size: 10px;
            color: #999;
            margin-top: 2px;
            text-align: right;
        }
        .msg-status.unread {
            color: #2f80ed;
        }
        .msg-status.read {
            color: #999;
        }

        .chat-input {
            padding: 10px;
            display: flex;
            gap: 10px;
            border-top: 1px solid #eee;
        }
        .chat-input textarea {
            flex: 1;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 5px;
            resize: none;
            /* 优化：高度自适应 */
            min-height: 40px;
            max-height: 120px;
            overflow-y: auto;
        }
        .send-chat {
            background: #07c160;
            color: white;
            border: none;
            padding: 0 15px;
            border-radius: 5px;
            cursor: pointer;
        }

        /* 宠物选择弹窗 */
        .pet-select-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .pet-select-box {
            background: #fff9e6;
            width: 90%;
            max-width: 400px;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
            padding: 20px;
            text-align: center;
        }
        .pet-select-title {
            font-size: 18px;
            font-weight: 500;
            color: #ff6b35;
            margin-bottom: 20px;
        }
        .pet-select-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-bottom: 20px;
        }
        .pet-select-item {
            padding: 15px;
            background: white;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s;
            border: 2px solid transparent;
        }
        .pet-select-item:hover {
            transform: scale(1.05);
            background: #fff0e0;
        }
        .pet-select-item.selected {
            border-color: #ff6b35;
            background: #fff0e0;
        }
        .pet-select-icon {
            font-size: 30px;
            margin-bottom: 5px;
        }
        .pet-select-name {
            font-size: 12px;
            color: #333;
        }
        .pet-select-confirm {
            background: #ff6b35;
            color: white;
            border: none;
            border-radius: 20px;
            padding: 8px 20px;
            cursor: pointer;
        }

        /* 宠物养成弹窗 */
        .pet-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 999;
        }
        .pet-box {
            background: #fff9e6;
            width: 90%;
            max-width: 350px;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
            text-align: center;
        }
        .pet-header {
            padding: 12px;
            background: #ffcc00;
            color: #333;
            border-radius: 15px 15px 0 0;
            font-weight: 500;
            font-size: 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .pet-change-btn {
            font-size: 12px;
            background: white;
            border: none;
            border-radius: 10px;
            padding: 2px 8px;
            cursor: pointer;
            color: #ff6b35;
        }
        .pet-body {
            padding: 20px;
        }
        /* 新增宠物改名样式 */
        .pet-name-setting {
            margin: 10px 0;
        }
        .pet-name-setting input {
            padding: 4px 8px;
            border: 1px solid #ffcc00;
            border-radius: 10px;
            font-size: 12px;
            width: 60%;
        }
        .pet-name-setting button {
            background: #ff9e00;
            border: none;
            border-radius: 10px;
            padding: 4px 10px;
            color: white;
            font-size: 12px;
        }
        /* 宠物等级样式 */
        .pet-level {
            font-size: 13px;
            color: #ff6b35;
            margin: 10px 0;
            font-weight: 500;
        }
        .pet-avatar {
            font-size: 80px;
            margin: 10px 0;
            transition: transform 0.3s;
        }
        .pet-avatar:hover {
            transform: scale(1.1);
        }
        .pet-talk {
            background: white;
            border: 2px solid #ffcc00;
            border-radius: 20px;
            padding: 8px 15px;
            margin: 10px auto;
            width: 80%;
            font-size: 13px;
        }
        .pet-status {
            display: flex;
            justify-content: space-around;
            margin: 15px 0;
            font-size: 12px;
        }
        .status-bar {
            width: 80px;
            height: 12px;
            background: #eee;
            border-radius: 6px;
            margin-top: 4px;
            overflow: hidden;
        }
        .status-fill {
            height: 100%;
            border-radius: 6px;
        }
        .hunger-fill {
            background: #ff6b6b;
        }
        .happy-fill {
            background: #4ecdc4;
        }
        .pet-actions {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
        }
        .pet-btn-action {
            background: #ff9e00;
            border: none;
            border-radius: 20px;
            padding: 8px 15px;
            color: white;
            cursor: pointer;
            font-size: 12px;
        }
        .close-pet {
            margin: 0 auto 15px;
            background: #ff4545;
            border: none;
            border-radius: 20px;
            padding: 8px 20px;
            color: white;
            cursor: pointer;
            font-size: 12px;
        }

        /* 说说样式 */
        .post-item {
            max-width: 680px;
            margin: 0 auto 12px;
            background: white;
            padding: 12px 16px;
            border-radius: 8px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            display: flex;
            gap: 12px;
            position: relative;
        }
        .post-avatar {
            width: 42px;
            height: 42px;
            border-radius: 50%;
            overflow-y: hidden;
            flex-shrink: 0;
        }
        .post-avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .post-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 6px;
            width: 100%;
        }
        .post-nickname {
            font-weight: 500;
            font-size: 15px;
        }
        .post-time {
            font-size: 12px;
            color: #999;
        }
        .post-view-count {
            font-size: 12px;
            color: #999;
            margin-top: 4px;
            display: flex;
            align-items: center;
            gap: 4px;
        }
        .post-signature {
            font-size: 12px;
            color: #999;
            margin-bottom: 4px;
        }
        .post-text {
            font-size: 15px;
            line-height: 1.6;
            margin-bottom: 8px;
        }
        .post-text img, .post-text video {
            max-width: 100%;
            border-radius: 4px;
            margin: 6px 0;
        }
        .top-tag {
            background: #ff4545;
            color: white;
            font-size: 11px;
            padding: 1px 5px;
            border-radius: 2px;
            position: absolute;
            top: 10px;
            right: 16px;
        }
        .post-actions {
            text-align: right;
            padding-top: 6px;
            border-top: 1px solid #eee;
        }
        .post-actions button {
            background: transparent;
            border: none;
            color: #999;
            padding: 4px 8px;
            font-size: 12px;
            cursor: pointer;
            margin-left: 10px;
        }
        /* 说说点赞样式 */
        .like-btn {
            display: inline-flex;
            align-items: center;
            gap: 3px;
        }

        .comments-section {
            margin-top: 15px;
            padding: 12px;
            background: #f9f9f9;
            border-radius: 6px;
        }
        .comments-section h4 {
            margin: 0 0 10px;
            font-size: 13px;
            color: #999;
        }
        .comment-item {
            font-size: 13px;
            padding: 8px 0;
            border-bottom: 1px dashed #eee;
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
        }
        .reply-item {
            font-size: 12px;
            padding: 6px 0 6px 16px;
            border-left: 2px solid #eee;
            margin: 4px 0;
        }
        .reply-item .msg-content {
            background: #f0f8f0;
            padding: 4px 8px;
            border-radius: 4px;
        }
        .comment-content-wrap {
            flex: 1;
        }
        .comment-actions, .reply-actions {
            display: none;
            margin-left: 10px;
        }
        .comment-item:hover .comment-actions, .reply-item:hover .reply-actions {
            display: inline-block;
        }
        .comment-actions button, .reply-actions button {
            background: transparent;
            border: none;
            color: #999;
            font-size: 11px;
            padding: 2px 4px;
            cursor: pointer;
        }

        .visitor-id-setting {
            max-width: 680px;
            margin: 0 auto 12px;
            padding: 16px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            display: none;
        }
        .visitor-id-setting h3 {
            margin: 0 0 8px;
            font-size: 16px;
        }
        .current-id {
            font-size: 13px;
            color: #07c160;
            margin-bottom: 8px;
        }
        .visitor-id-setting input {
            width: 100%;
            padding: 8px 10px;
            margin: 6px 0;
            border: 1px solid #e5e5e5;
            border-radius: 4px;
        }
        .visitor-id-setting button {
            background: #07c160;
            color: white;
            border: none;
            padding: 6px 16px;
            border-radius: 4px;
            cursor: pointer;
        }

        .profile-setting {
            max-width: 680px;
            margin: 0 auto 12px;
            padding: 16px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            display: flex;
            align-items: center;
            gap: 16px;
        }
        .profile-avatar-preview {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            overflow-y: hidden;
            border: 1px solid #e5e5e5;
            position: relative;
        }
        .profile-avatar-preview img, .profile-avatar-preview video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            position: absolute;
            top: 0;
            left: 0;
        }
        .profile-form h3 {
            margin: 0 0 8px;
            font-size: 16px;
        }
        .profile-form input {
            width: 100%;
            padding: 8px 10px;
            margin: 6px 0;
            border: 1px solid #e5e5e5;
            border-radius: 4px;
            font-size: 14px;
        }
        .profile-form button {
            background: #07c160;
            color: white;
            border: none;
            padding: 6px 16px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
        }

        .nav-bar {
            max-width: 680px;
            margin: 0 auto 12px;
            text-align: center;
        }
        .nav-btn {
            background: transparent;
            border: 1px solid #e5e5e5;
            padding: 6px 16px;
            border-radius: 4px;
            cursor: pointer;
            margin: 0 5px;
            font-size: 14px;
        }
        .nav-btn.active {
            background: #07c160;
            color: white;
            border-color: #07c160;
        }

        #illustrationSection {
            max-width: 680px;
            margin: 0 auto;
            display: none;
        }
        .illustration-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }
        .illustration-item {
            background: white;
            padding: 10px;
            border-radius: 6px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            text-align: center;
        }
        .illustration-item img, .illustration-item video {
            width: 100%;
            border-radius: 4px;
            margin-bottom: 8px;
        }

        .visitor-mode .profile-setting, .visitor-mode .admin-btn {
            display: none;
        }
        .visitor-mode .visitor-id-setting {
            display: block;
        }
        .visitor-mode .view-record-btn {
            display: none;
        }

        .comment-edit-form, .reply-edit-form {
            margin-top: 8px;
            display: none;
        }
        .comment-edit-form textarea, .reply-edit-form textarea {
            width: 100%;
            padding: 6px;
            border: 1px solid #eee;
            border-radius: 4px;
            font-size: 12px;
            min-height: 40px;
        }

        /* 动态发布媒体预览 */
        #postMediaPreview {
            margin: 10px 0;
            display: none;
        }
        #postPreviewImg, #postPreviewVideo {
            max-width: 100%;
            max-height: 200px;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <!-- 密码验证弹窗 -->
    <div id="passwordScreen">
        <div id="passwordBox">
            <h2>时光信笺</h2>
            <input type="password" id="passwordInput" placeholder="请输入密码">
            <button id="passwordBtn">进入</button>
            <div id="errorMsg">密码错误，请重试</div>
        </div>
    </div>

    <!-- 主内容区 -->
    <div id="siteContent" style="display: none;">
        <!-- 顶部导航+按钮 -->
        <div class="header">
            <h2>时光信笺</h2>
            <div>
                <button class="chat-btn" id="openChatBtn" style="display: none;" onclick="openChat()">私聊管理员<span id="visitorUnreadBadge" class="unread-badge" style="display: none;">1</span></button>
                <button class="visitor-list-btn" id="openVisitorListBtn" style="display: none;" onclick="openVisitorList()">访客列表<span id="adminUnreadBadge" class="unread-badge" style="display: none;">1</span></button>
                <button class="view-record-btn" id="openViewRecordBtn" style="display: none;" onclick="openViewRecord()">浏览记录</button>
                <button class="pet-btn" id="openPetBtn" onclick="openPetSelect()">我的宠物</button>
            </div>
        </div>

        <!-- 管理员个人信息设置（支持GIF/视频头像） -->
        <div class="profile-setting">
            <div class="profile-avatar-preview">
                <img id="avatarPreviewImg" src="https://picsum.photos/id/64/100/100" alt="头像" style="display: block;">
                <video id="avatarPreviewVideo" controls style="width: 100%; height: 100%; object-fit: cover; display: none; border-radius: 50%;"></video>
            </div>
            <div class="profile-form">
                <h3>👤 管理员信息设置</h3>
                <input type="text" id="profileNickname" placeholder="昵称" value="管理员">
                <!-- 新增支持GIF/视频上传 -->
                <div style="margin: 10px 0;">
                    <input type="file" id="avatarFile" accept="image/png, image/jpg, image/jpeg, image/gif, video/mp4, video/webm" onchange="previewAvatar(this)" />
                    <p style="font-size: 12px; color: #999; margin: 5px 0;">支持jpg/png/GIF/mp4/webm格式</p>
                </div>
                <input type="text" id="profileAvatarUrl" placeholder="或者输入头像链接（二选一）" value="https://picsum.photos/id/64/100/100">
                <input type="text" id="profileSignature" placeholder="个性签名" value="欢迎来时光信笺留言～">
                <button onclick="saveProfile()">保存信息</button>
            </div>
        </div>

        <!-- 访客ID设置区 -->
        <div class="visitor-id-setting">
            <h3>✏️ 你的留言身份</h3>
            <div class="current-id" id="currentVisitorId">当前ID：未设置</div>
            <input type="text" id="visitorIdInput" placeholder="设置/修改你的ID">
            <button onclick="saveVisitorId()">保存ID</button>
        </div>

        <!-- 标签导航 -->
        <div class="nav-bar">
            <button class="nav-btn active" onclick="switchTab('post')">碎碎念</button>
            <button class="nav-btn" onclick="switchTab('illustration')">插画小馆</button>
        </div>

        <!-- 说说区域 -->
        <div id="postSection">
            <div class="form-container" id="postForm">
                <h3>✍️ 发布新动态</h3>
                <input type="text" id="postTitle" placeholder="输入说说标题（必填）">
                <textarea id="postContent" rows="3" placeholder="分享你的心情..."></textarea>
                <!-- 支持GIF/视频上传 -->
                <div style="margin: 10px 0;">
                    <input type="file" id="postImageFile" accept="image/png, image/jpg, image/jpeg, image/gif, video/mp4, video/webm" onchange="previewPostMedia(this)" />
                    <p style="font-size: 12px; color: #999; margin: 5px 0;">支持jpg/png/GIF/mp4/webm格式</p>
                </div>
                <input type="text" id="postMediaUrl" placeholder="或者输入图片/视频链接（二选一）">
                <!-- 媒体预览容器（兼容图片/视频） -->
                <div id="postMediaPreview" style="margin: 10px 0; display: none;">
                    <img id="postPreviewImg" src="" alt="预览图" style="max-width: 100%; max-height: 200px; border-radius: 4px;">
                    <video id="postPreviewVideo" controls style="max-width: 100%; max-height: 200px; border-radius: 4px; display: none;"></video>
                </div>
                <label><input type="checkbox" id="isTop"> 设为置顶</label>
                <button onclick="addPost()">发布</button>
            </div>

            <div id="postsContainer">
                <!-- 示例说说 -->
                <div class="post-item" data-top="true" data-post-id="post1">
                    <div class="post-avatar">
                        <img src="https://picsum.photos/id/64/100/100" alt="头像">
                    </div>
                    <div class="post-content" style="position: relative; width: 100%;">
                        <div class="post-header">
                            <span class="post-nickname">管理员</span>
                            <span class="post-time">2026-01-01 10:00</span>
                        </div>
                        <div class="post-signature">生活无解，来杯拿铁～</div>
                        <div class="post-text">
                            <strong>示例说说：</strong>❤️ 这是送给你的专属时光信笺，愿我们的友谊岁岁年年～
                        </div>
                        <!-- 公开浏览量 - 所有人可见 -->
                        <div class="post-view-count">
                            <span>👁️ 浏览量：<span id="view-count-post1">0</span></span>
                        </div>
                        <span class="top-tag">置顶</span>
                        <div class="post-actions">
                            <button class="admin-btn" onclick="toggleTop(this)">取消置顶</button>
                            <button class="admin-btn" onclick="editPost(this)">编辑</button>
                            <button class="admin-btn" onclick="deletePost(this)">删除</button>
                            <button onclick="toggleCommentForm(this)">留言</button>
                            <button class="like-btn" onclick="likePost(this)">点赞 👍 (<span class="like-count">0</span>)</button>
                        </div>
                        <div class="comments-section">
                            <h4>💬 留言板</h4>
                            <div class="comment-item own-comment" data-id="访客小A">
                                <div class="comment-content-wrap">访客小A：太喜欢啦！谢谢你～</div>
                                <div class="comment-actions">
                                    <button onclick="replyComment(this)">回复</button>
                                    <button onclick="editComment(this)">编辑</button>
                                    <button onclick="deleteComment(this)">删除</button>
                                </div>
                                <div class="reply-list">
                                    <div class="reply-item own-reply" data-id="管理员">
                                        <div class="comment-content-wrap">管理员 回复 访客小A：不客气呀～</div>
                                        <div class="reply-actions">
                                            <button onclick="editReply(this)">编辑</button>
                                            <button onclick="deleteReply(this)">删除</button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="comment-form">
                                <textarea class="comment-content" placeholder="写下你的留言..."></textarea>
                                <button onclick="addComment(this)">提交</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- 插画区域（支持GIF/视频上传） -->
        <div id="illustrationSection">
            <div class="form-container">
                <h3>🎨 添加插画/视频</h3>
                <input type="text" id="illustrationTitle" placeholder="标题">
                <!-- 支持GIF/视频上传 -->
                <div style="margin: 10px 0;">
                    <input type="file" id="illustrationFile" accept="image/png, image/jpg, image/jpeg, image/gif, video/mp4, video/webm" />
                    <p style="font-size: 12px; color: #999; margin: 5px 0;">支持jpg/png/GIF/mp4/webm格式</p>
                </div>
                <input type="text" id="illustrationUrl" placeholder="或者输入图片/视频链接（二选一）">
                <button onclick="addIllustration()">添加</button>
            </div>
            <div class="illustration-grid" id="illustrationGrid">
                <div class="illustration-item">
                    <img src="https://picsum.photos/id/102/200/200" alt="春日山野">
                    <p>春日山野</p>
                </div>
            </div>
        </div>
    </div>

    <!-- 访客列表弹窗 -->
    <div class="visitor-modal" id="visitorModal">
        <div class="visitor-box">
            <div class="visitor-header">
                访客列表
                <button class="close-visitor" onclick="closeVisitorList()">×</button>
            </div>
            <div class="visitor-content">
                <div class="visitor-list" id="visitorListContainer">
                    <!-- 访客列表动态生成 -->
                </div>
                <div class="visitor-chat">
                    <div class="visitor-chat-header" id="visitorChatHeader">
                        选择访客查看聊天记录
                    </div>
                    <div class="visitor-chat-content" id="visitorChatContent">
                    </div>
                </div>
            </div>
            <div class="visitor-footer">
                <button class="clear-visitor-btn" onclick="clearVisitorList()">清空访客记录</button>
            </div>
        </div>
    </div>

    <!-- 浏览记录弹窗 -->
    <div class="view-record-modal" id="viewRecordModal">
        <div class="view-record-box">
            <div class="view-record-header">
                内容浏览记录
                <button class="close-view-record" onclick="closeViewRecord()">×</button>
            </div>
            <div class="view-record-content" id="viewRecordContent">
                <!-- 浏览记录动态生成 -->
            </div>
        </div>
    </div>

    <!-- 私聊弹窗 -->
    <div class="chat-modal" id="chatModal">
        <div class="chat-box">
            <div class="chat-header">
                私聊管理员
                <button class="close-chat" onclick="closeChat()">×</button>
            </div>
            <div class="chat-content" id="chatContent">
                <!-- 聊天记录从本地存储加载 -->
            </div>
            <div class="chat-input">
                <textarea id="chatInput" placeholder="输入消息..."></textarea>
                <button class="send-chat" onclick="sendChatMsg()">发送</button>
            </div>
        </div>
    </div>

    <!-- 宠物选择弹窗（小鸟+和平鸽） -->
    <div class="pet-select-modal" id="petSelectModal">
        <div class="pet-select-box">
            <div class="pet-select-title">选择你的专属宠物 🐾</div>
            <div class="pet-select-grid">
                <div class="pet-select-item" onclick="selectPet('cat')" data-pet="cat">
                    <div class="pet-select-icon">🐱</div>
                    <div class="pet-select-name">小猫</div>
                </div>
                <div class="pet-select-item" onclick="selectPet('dog')" data-pet="dog">
                    <div class="pet-select-icon">🐶</div>
                    <div class="pet-select-name">小狗</div>
                </div>
                <div class="pet-select-item" onclick="selectPet('bird')" data-pet="bird">
                    <div class="pet-select-icon">🐦</div>
                    <div class="pet-select-name">小鸟</div>
                </div>
                <div class="pet-select-item" onclick="selectPet('panda')" data-pet="panda">
                    <div class="pet-select-icon">🐼</div>
                    <div class="pet-select-name">小熊猫</div>
                </div>
                <div class="pet-select-item" onclick="selectPet('tiger')" data-pet="tiger">
                    <div class="pet-select-icon">🐯</div>
                    <div class="pet-select-name">小老虎</div>
                </div>
                <div class="pet-select-item" onclick="selectPet('fox')" data-pet="fox">
                    <div class="pet-select-icon">🦊</div>
                    <div class="pet-select-name">小狐狸</div>
                </div>
                <div class="pet-select-item" onclick="selectPet('dove')" data-pet="dove">
                    <div class="pet-select-icon">🕊️</div>
                    <div class="pet-select-name">和平鸽</div>
                </div>
            </div>
            <button class="pet-select-confirm" onclick="confirmPet()">确定领养</button>
        </div>
    </div>

    <!-- 宠物养成弹窗 -->
    <div class="pet-modal" id="petModal">
        <div class="pet-box">
            <div class="pet-header">
                我的小宠物 🐾
                <button class="pet-change-btn" onclick="openPetSelect()">更换宠物</button>
            </div>
            <div class="pet-body">
                <!-- 新增宠物改名功能 -->
                <div class="pet-name-setting">
                    <input type="text" id="petNameInput" placeholder="给宠物起个名字吧">
                    <button onclick="renamePet()">确定</button>
                </div>
                <!-- 宠物等级显示 -->
                <div class="pet-level">等级：<span id="petLevel">1</span> | 经验：<span id="petExp">0</span>/<span id="needExp">20</span></div>
                <div class="pet-avatar" id="petAvatar">
                    🐱
                </div>
                <div class="pet-talk" id="petTalk">
                    你好呀！快来和我玩～
                </div>
                <div class="pet-status">
                    <div class="status-item">
                        饥饿度
                        <div class="status-bar">
                            <div class="status-fill hunger-fill" id="hungerBar" style="width: 80%;"></div>
                        </div>
                    </div>
                    <div class="status-item">
                        快乐度
                        <div class="status-bar">
                            <div class="status-fill happy-fill" id="happyBar" style="width: 70%;"></div>
                        </div>
                    </div>
                </div>
                <div class="pet-actions">
                    <button class="pet-btn-action" onclick="feedPet()">喂食 🥫</button>
                    <button class="pet-btn-action" onclick="playPet()">互动 🎮</button>
                </div>
                <button class="close-pet" onclick="closePet()">关闭</button>
            </div>
        </div>
    </div>

    <script>
        // 全局变量
        let userRole = ""; // admin/visitor
        let visitorId = "";
        const ADMIN_PWD = "071218";
        const VISITOR_PWD = "20251018";
        let chatHistoryKey = "chatHistory_";
        // 已读状态相关全局变量
        let unreadCount = {}; // 存储每个访客的未读消息数 {visitorId: count}
        const UNREAD_KEY = "unreadCount_";
        // 宠物定时器全局变量
        let petTimer = null;

        // 宠物配置 - 小肥揪→小鸟 + 新增和平鸽
        const PET_CONFIG = {
            cat: { icon: "🐱", name: "小猫", feedTalks: ["喵～好吃！😋", "鱼干真美味！", "饱啦饱啦～"], playTalks: ["逗猫棒真好玩！😆", "快来摸我～", "喵呜～开心！"] },
            dog: { icon: "🐶", name: "小狗", feedTalks: ["汪汪！骨头好香！", "谢谢主人～", "嗝～吃饱啦！"], playTalks: ["接球！真好玩！", "主人陪我玩～开心！", "摇摇尾巴～"] },
            bird: { icon: "🐦", name: "小鸟", feedTalks: ["啾啾～谷子真好吃！", "吃饱啦～要唱歌！", "虫虫真美味～"], playTalks: ["啾啾啾～唱歌给你听！", "飞呀飞～好开心！", "啄啄手指～"] }, // 替换小肥揪
            panda: { icon: "🐼", name: "小熊猫", feedTalks: ["竹笋真好吃！", "嘎嘣脆～", "饱啦～要睡觉啦！"], playTalks: ["爬树爬树～", "打滚儿～好开心！", "竹叶竹叶～"] },
            tiger: { icon: "🐯", name: "小老虎", feedTalks: ["肉！肉！好吃！", "嗷呜～饱啦！", "谢谢饲养员～"], playTalks: ["捕猎游戏！好玩！", "嗷呜～开心！", "打滚儿～"] },
            fox: { icon: "🦊", name: "小狐狸", feedTalks: ["葡萄真甜！", "好吃好吃！", "饱啦～"], playTalks: ["捉迷藏！来呀～", "好开心！转圈圈～", "啾啾～"] },
            dove: { icon: "🕊️", name: "和平鸽", feedTalks: ["咕咕～麦粒真好吃！", "吃饱啦～要飞翔！", "清水好甜～"], playTalks: ["带封信吧～咕咕～", "展翅高飞～好自由！", "落在肩头～"] } // 新增和平鸽
        };
        
        // 宠物数据 - 新增等级和经验字段
        let petData = {
            type: "bird", // 默认宠物改为小鸟
            hunger: 80,
            happy: 70,
            talk: "啾啾～快来和我玩～",
            reminded: false,
            level: 1,
            exp: 0,
            name: ""
        };
        let selectedPet = "bird"; // 默认选中小鸟
        const PET_KEY = "petData_";

        // ========== 页面初始化 ==========
        window.onload = function() {
            // 密码验证
            document.getElementById('passwordBtn').addEventListener('click', checkPassword);
            
            // 加载未读消息数
            loadUnreadCount();
            
            // 页面关闭时保存宠物数据
            window.addEventListener('beforeunload', function() {
                if (petData) {
                    savePetData();
                }
                if (petTimer) {
                    clearInterval(petTimer);
                }
            });

            // 聊天输入框高度自适应
            const chatTextarea = document.getElementById('chatInput');
            if (chatTextarea) {
                chatTextarea.addEventListener('input', function() {
                    this.style.height = 'auto';
                    this.style.height = (this.scrollHeight > 120 ? 120 : this.scrollHeight) + 'px';
                });
            }

            // 加载管理员信息
            loadProfile();

            // 加载访客ID
            loadVisitorId();

            // 加载说说
            loadPostsFromLocal();

            // 加载宠物数据
            loadPetData();
        };

        // ========== 密码验证 ==========
        function checkPassword() {
            const pwd = document.getElementById('passwordInput').value;
            const errorMsg = document.getElementById('errorMsg');
            
            if (pwd === ADMIN_PWD) {
                userRole = "admin";
                document.getElementById('siteContent').style.display = 'block';
                document.getElementById('passwordScreen').style.display = 'none';
                document.getElementById('openVisitorListBtn').style.display = 'inline-block';
                document.getElementById('openViewRecordBtn').style.display = 'inline-block';
            } else if (pwd === VISITOR_PWD) {
                userRole = "visitor";
                document.getElementById('siteContent').style.display = 'block';
                document.getElementById('passwordScreen').style.display = 'none';
                document.getElementById('openChatBtn').style.display = 'inline-block';
                document.body.classList.add('visitor-mode');
                // 初始化访客聊天
                initVisitorChat();
            } else {
                errorMsg.style.display = 'block';
                setTimeout(() => {
                    errorMsg.style.display = 'none';
                }, 2000);
            }
        }

        // ========== 管理员信息设置 ==========
        function loadProfile() {
            const profile = JSON.parse(localStorage.getItem('adminProfile') || '{"nickname":"管理员","avatarUrl":"https://picsum.photos/id/64/100/100","signature":"欢迎来时光信笺留言～"}');
            document.getElementById('profileNickname').value = profile.nickname;
            document.getElementById('profileAvatarUrl').value = profile.avatarUrl;
            document.getElementById('profileSignature').value = profile.signature;
            document.getElementById('avatarPreviewImg').src = profile.avatarUrl;
        }

        // 预览头像（兼容图片/GIF/视频）
        function previewAvatar(input) {
            const file = input.files[0];
            if (!file) return;

            const imgPreview = document.getElementById('avatarPreviewImg');
            const videoPreview = document.getElementById('avatarPreviewVideo');
            const avatarUrlInput = document.getElementById('profileAvatarUrl');

            // 清空旧预览
            imgPreview.style.display = 'none';
            videoPreview.style.display = 'none';

            const reader = new FileReader();
            reader.onload = function(e) {
                const url = e.target.result;
                avatarUrlInput.value = url; // 自动填充到链接框

                // 判断是图片还是视频
                if (file.type.startsWith('image/')) {
                    imgPreview.src = url;
                    imgPreview.style.display = 'block';
                } else if (file.type.startsWith('video/')) {
                    videoPreview.src = url;
                    videoPreview.style.display = 'block';
                }
            };
            reader.readAsDataURL(file);
        }

        function saveProfile() {
            const nickname = document.getElementById('profileNickname').value.trim();
            const avatarUrl = document.getElementById('profileAvatarUrl').value.trim();
            const signature = document.getElementById('profileSignature').value.trim();
            
            if (!nickname) {
                alert('昵称不能为空！');
                return;
            }
            
            const profileData = {
                nickname: nickname,
                avatarUrl: avatarUrl,
                signature: signature
            };
            
            localStorage.setItem('adminProfile', JSON.stringify(profileData));
            document.getElementById('avatarPreviewImg').src = avatarUrl;
            alert('信息保存成功！');
            
            // 更新所有说说的管理员头像和昵称
            document.querySelectorAll('.post-avatar img').forEach(img => {
                img.src = avatarUrl;
            });
            document.querySelectorAll('.post-nickname').forEach(el => {
                el.textContent = nickname;
            });
            document.querySelectorAll('.post-signature').forEach(el => {
                el.textContent = signature;
            });
        }

        // ========== 访客ID管理 ==========
        function loadVisitorId() {
            const savedId = localStorage.getItem('visitorId');
            if (savedId) {
                visitorId = savedId;
                document.getElementById('currentVisitorId').textContent = `当前ID：${savedId}`;
                document.getElementById('visitorIdInput').value = savedId;
            }
        }

        function saveVisitorId() {
            const newId = document.getElementById('visitorIdInput').value.trim();
            if (!newId) {
                alert('ID不能为空！');
                return;
            }
            if (newId.length > 10) {
                alert('ID长度不能超过10个字符哦！');
                return;
            }
            visitorId = newId;
            localStorage.setItem('visitorId', newId);
            document.getElementById('currentVisitorId').textContent = `当前ID：${newId}`;
            alert('ID保存成功！');
            // 初始化访客聊天
            initVisitorChat();
        }

        // ========== 标签切换 ==========
        function switchTab(tabName) {
            // 切换按钮样式
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // 切换内容区域
            if (tabName === 'post') {
                document.getElementById('postSection').style.display = 'block';
                document.getElementById('illustrationSection').style.display = 'none';
            } else if (tabName === 'illustration') {
                document.getElementById('postSection').style.display = 'none';
                document.getElementById('illustrationSection').style.display = 'block';
            }
        }

        // ========== 说说管理 ==========
        function addPost() {
            const title = document.getElementById('postTitle').value.trim();
            const content = document.getElementById('postContent').value.trim();
            const mediaUrl = document.getElementById('postMediaUrl').value.trim();
            const isTop = document.getElementById('isTop').checked;
            
            if (!title) {
                alert('标题不能为空！');
                return;
            }
            
            const postsContainer = document.getElementById('postsContainer');
            const postId = 'post_' + Date.now();
            const time = new Date().toLocaleString('zh-CN', {
                year: 'numeric',
                month: '2-digit',
                day: '2-digit',
                hour: '2-digit',
                minute: '2-digit'
            }).replace(/\//g, '-');
            
            // 获取管理员信息
            const profile = JSON.parse(localStorage.getItem('adminProfile') || '{"nickname":"管理员","avatarUrl":"https://picsum.photos/id/64/100/100","signature":"欢迎来时光信笺留言～"}');
            
            // 生成媒体标签（图片/GIF/视频）
            let mediaHtml = '';
            if (mediaUrl) {
                if (mediaUrl.endsWith('.mp4') || mediaUrl.endsWith('.webm')) {
                    mediaHtml = `<video src="${mediaUrl}" controls style="max-width: 100%; border-radius: 4px; margin: 6px 0;"></video>`;
                } else {
                    mediaHtml = `<img src="${mediaUrl}" alt="配图" style="max-width: 100%; border-radius: 4px; margin: 6px 0;">`;
                }
            }
            
            const topTag = isTop ? '<span class="top-tag">置顶</span>' : '';
            const topAttr = isTop ? 'data-top="true"' : '';
            
            const postHtml = `
                <div class="post-item" ${topAttr} data-post-id="${postId}">
                    <div class="post-avatar">
                        <img src="${profile.avatarUrl}" alt="头像">
                    </div>
                    <div class="post-content" style="position: relative; width: 100%;">
                        <div class="post-header">
                            <span class="post-nickname">${profile.nickname}</span>
                            <span class="post-time">${time}</span>
                        </div>
                        <div class="post-signature">${profile.signature}</div>
                        <div class="post-text">
                            <strong>${title}：</strong>${content || '（无正文）'}
                            ${mediaHtml}
                        </div>
                        <div class="post-view-count">
                            <span>👁️ 浏览量：<span id="view-count-${postId}">0</span></span>
                        </div>
                        ${topTag}
                        <div class="post-actions">
                            <button class="admin-btn" onclick="toggleTop(this)">${isTop ? '取消置顶' : '设为置顶'}</button>
                            <button class="admin-btn" onclick="editPost(this)">编辑</button>
                            <button class="admin-btn" onclick="deletePost(this)">删除</button>
                            <button onclick="toggleCommentForm(this)">留言</button>
                            <button class="like-btn" onclick="likePost(this)">点赞 👍 (<span class="like-count">0</span>)</button>
                        </div>
                        <div class="comments-section">
                            <h4>💬 留言板</h4>
                            <div class="comment-form">
                                <textarea class="comment-content" placeholder="写下你的留言..."></textarea>
                                <button onclick="addComment(this)">提交</button>
                            </div>
                        </div>
                    </div>
                </div>
            `;
            
            // 置顶则插入最前面，否则插入最后面
            if (isTop) {
                postsContainer.insertAdjacentHTML('afterbegin', postHtml);
            } else {
                postsContainer.insertAdjacentHTML('beforeend', postHtml);
            }
            
            // 清空输入
            document.getElementById('postTitle').value = '';
            document.getElementById('postContent').value = '';
            document.getElementById('postMediaUrl').value = '';
            document.getElementById('postMediaPreview').style.display = 'none';
            document.getElementById('isTop').checked = false;
            
            // 保存说说到本地存储
            savePostsToLocal();
            alert('发布成功！');
        }

        function toggleTop(btn) {
            const postItem = btn.closest('.post-item');
            const isTop = postItem.getAttribute('data-top') === 'true';
            
            if (isTop) {
                postItem.removeAttribute('data-top');
                postItem.querySelector('.top-tag').remove();
                btn.textContent = '设为置顶';
            } else {
                postItem.setAttribute('data-top', 'true');
                const topTag = document.createElement('span');
                topTag.className = 'top-tag';
                topTag.textContent = '置顶';
                postItem.querySelector('.post-content').appendChild(topTag);
                btn.textContent = '取消置顶';
            }
            
            // 重新排序说说（置顶的在最前面）
            const postsContainer = document.getElementById('postsContainer');
            const posts = Array.from(postsContainer.querySelectorAll('.post-item'));
            const topPosts = posts.filter(p => p.getAttribute('data-top') === 'true');
            const normalPosts = posts.filter(p => !p.getAttribute('data-top'));
            
            postsContainer.innerHTML = '';
            topPosts.forEach(p => postsContainer.appendChild(p));
            normalPosts.forEach(p => postsContainer.appendChild(p));
            
            // 保存说说到本地存储
            savePostsToLocal();
        }

        function editPost(btn) {
            const postItem = btn.closest('.post-item');
            const postTextEl = postItem.querySelector('.post-text');
            const title = postTextEl.querySelector('strong').textContent.replace('：', '');
            const content = postTextEl.textContent.replace(`${title}：`, '').trim();
            const mediaEl = postTextEl.querySelector('img, video');
            const mediaUrl = mediaEl ? mediaEl.src : '';
            const isTop = postItem.getAttribute('data-top') === 'true';
            
            document.getElementById('postTitle').value = title;
            document.getElementById('postContent').value = content;
            document.getElementById('postMediaUrl').value = mediaUrl;
            document.getElementById('isTop').checked = isTop;
            
            // 预览媒体
            if (mediaUrl) {
                const previewContainer = document.getElementById('postMediaPreview');
                const imgPreview = document.getElementById('postPreviewImg');
                const videoPreview = document.getElementById('postPreviewVideo');
                
                previewContainer.style.display = 'block';
                imgPreview.style.display = 'none';
                videoPreview.style.display = 'none';
                
                if (mediaUrl.endsWith('.mp4') || mediaUrl.endsWith('.webm')) {
                    videoPreview.src = mediaUrl;
                    videoPreview.style.display = 'block';
                } else {
                    imgPreview.src = mediaUrl;
                    imgPreview.style.display = 'block';
                }
            }
            
            // 删除原说说
            postItem.remove();
            savePostsToLocal();
        }

        function deletePost(btn) {
            if (confirm('确定要删除这条说说吗？')) {
                const postItem = btn.closest('.post-item');
                postItem.remove();
                savePostsToLocal();
            }
        }

        function savePostsToLocal() {
            const postsContainer = document.getElementById('postsContainer');
            const postsHtml = postsContainer.innerHTML;
            localStorage.setItem('savedPosts', postsHtml);
        }

        function loadPostsFromLocal() {
            const savedPosts = localStorage.getItem('savedPosts');
            if (savedPosts) {
                document.getElementById('postsContainer').innerHTML = savedPosts;
            }
        }

        // ========== 说说点赞 ==========
        function likePost(btn) {
            const likeCountEl = btn.querySelector('.like-count');
            let count = parseInt(likeCountEl.textContent) || 0;
            count += 1;
            likeCountEl.textContent = count;
            
            // 保存点赞数到本地存储
            const postItem = btn.closest('.post-item');
            const postId = postItem.dataset.postId;
            localStorage.setItem(`likeCount_${postId}`, count);
        }

        // ========== 评论管理 ==========
        function toggleCommentForm(btn) {
            const commentForm = btn.closest('.post-item').querySelector('.comment-form');
            commentForm.style.display = commentForm.style.display === 'none' ? 'block' : 'none';
        }

        function addComment(btn) {
            const commentForm = btn.closest('.comment-form');
            const content = commentForm.querySelector('.comment-content').value.trim();
            if (!content) {
                alert('评论内容不能为空！');
                return;
            }
            
            const commentSection = commentForm.closest('.comments-section');
            const commentList = commentSection.querySelector('.comment-list') || commentSection;
            const commentId = userRole === 'admin' ? '管理员' : (visitorId || '访客');
            
            const commentHtml = `
                <div class="comment-item own-comment" data-id="${commentId}">
                    <div class="comment-content-wrap">${commentId}：${content}</div>
                    <div class="comment-actions">
                        <button onclick="replyComment(this)">回复</button>
                        <button onclick="editComment(this)">编辑</button>
                        <button onclick="deleteComment(this)">删除</button>
                    </div>
                    <div class="reply-list"></div>
                </div>
            `;
            
            commentList.insertAdjacentHTML('beforeend', commentHtml);
            commentForm.querySelector('.comment-content').value = '';
            
            // 保存说说到本地存储
            savePostsToLocal();
        }

        function editComment(btn) {
            const commentItem = btn.closest('.comment-item');
            const content = commentItem.querySelector('.comment-content-wrap').textContent.split('：')[1];
            const editForm = document.createElement('div');
            editForm.className = 'comment-edit-form';
            editForm.innerHTML = `
                <textarea class="comment-content" placeholder="编辑你的评论...">${content}</textarea>
                <button onclick="saveCommentEdit(this)">保存</button>
                <button onclick="cancelCommentEdit(this)">取消</button>
            `;
            
            commentItem.appendChild(editForm);
            btn.closest('.comment-actions').style.display = 'none';
        }

        function saveCommentEdit(btn) {
            const editForm = btn.closest('.comment-edit-form');
            const newContent = editForm.querySelector('.comment-content').value.trim();
            if (!newContent) {
                alert('评论内容不能为空！');
                return;
            }
            
            const commentItem = editForm.closest('.comment-item');
            const commentId = commentItem.dataset.id;
            commentItem.querySelector('.comment-content-wrap').textContent = `${commentId}：${newContent}`;
            
            editForm.remove();
            commentItem.querySelector('.comment-actions').style.display = 'block';
            
            // 保存说说到本地存储
            savePostsToLocal();
        }

        function cancelCommentEdit(btn) {
            const editForm = btn.closest('.comment-edit-form');
            editForm.remove();
            editForm.closest('.comment-item').querySelector('.comment-actions').style.display = 'block';
        }

        function deleteComment(btn) {
            if (confirm('确定要删除这条评论吗？')) {
                const commentItem = btn.closest('.comment-item');
                commentItem.remove();
                
                // 保存说说到本地存储
                savePostsToLocal();
            }
        }

        // ========== 回复管理 ==========
        function replyComment(btn) {
            const commentItem = btn.closest('.comment-item');
            const replyList = commentItem.querySelector('.reply-list');
            const replyTo = commentItem.dataset.id;
            
            const replyForm = document.createElement('div');
            replyForm.className = 'reply-edit-form';
            replyForm.innerHTML = `
                <textarea class="reply-content" placeholder="回复${replyTo}..."></textarea>
                <button onclick="addReply(this, '${replyTo}')">发送</button>
                <button onclick="cancelReplyEdit(this)">取消</button>
            `;
            
            replyList.appendChild(replyForm);
        }

        function addReply(btn, replyTo) {
            const editForm = btn.closest('.reply-edit-form');
            const content = editForm.querySelector('.reply-content').value.trim();
            if (!content) {
                alert('回复内容不能为空！');
                return;
            }
            
            const replyList = editForm.closest('.reply-list');
            const replyId = userRole === 'admin' ? '管理员' : (visitorId || '访客');
            
            const replyHtml = `
                <div class="reply-item own-reply" data-id="${replyId}">
                    <div class="comment-content-wrap">${replyId} 回复 ${replyTo}：${content}</div>
                    <div class
