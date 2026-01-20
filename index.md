---
layout: home
title: 作为迟了两年的礼物
---
<link rel="stylesheet" href="/style.css">

<!-- 全局样式 -->
<style>
/* 密码验证页样式 */
#passwordScreen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #ffffff url("https://cdn.pixabay.com/animation/2022/11/08/07/49/07-49-11-561_512.gif") fixed;
  background-size: cover;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}
#passwordBox {
  background: #fafafa;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  text-align: center;
  max-width: 400px;
  width: 90%;
}
#passwordInput {
  width: 100%;
  padding: 12px;
  margin: 15px 0;
  border: 1px solid #e5e5e5;
  border-radius: 4px;
  font-size: 16px;
}
#passwordBtn {
  background: #666;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}
#passwordBtn:hover {
  background: #333;
}
#errorMsg {
  color: #ff4444;
  margin-top: 10px;
  font-size: 14px;
  display: none;
}

/* 标题动画 */
h1 {
  animation: fadeIn 1.8s ease-in-out;
  text-align: center;
  color: #333;
  font-weight: normal;
  margin: 30px 0;
  position: sticky;
  top: 0;
  background: #fafafa;
  padding: 10px 0;
  z-index: 10;
  box-shadow: 0 1px 2px rgba(0,0,0,0.03);
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* 导航栏：切换说说/插画 */
.nav-bar {
  max-width: 700px;
  margin: 0 auto 20px;
  text-align: center;
}
.nav-btn {
  background: #fafafa;
  border: 1px solid #e5e5e5;
  padding: 8px 20px;
  border-radius: 4px;
  cursor: pointer;
  margin: 0 5px;
  transition: all 0.3s;
}
.nav-btn.active {
  background: #666;
  color: white;
  border-color: #666;
}

/* 发布/编辑/留言表单样式 */
.form-container {
  max-width: 700px;
  margin: 0 auto 20px;
  padding: 20px;
  background: #fafafa;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}
.form-container input, .form-container textarea {
  width: 100%;
  padding: 10px;
  margin: 8px 0;
  border: 1px solid #e5e5e5;
  border-radius: 4px;
  font-family: 微软雅黑;
  resize: vertical;
}
.form-container button {
  background: #666;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
  margin-right: 10px;
}
.form-container button:hover {
  background: #333;
}
.form-container .btn-cancel {
  background: #ccc;
}
.form-container .btn-cancel:hover {
  background: #999;
}

/* ========== 核心：每条说说独立分隔样式 ========== */
.post-item {
  max-width: 700px;
  margin: 0 auto 25px !important; /* 每条之间留大间距 */
  background: #ffffff;
  border: 1px solid #f0f0f0;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.04);
  position: relative;
}
/* 置顶标签 */
.top-tag {
  position: absolute;
  top: -10px;
  right: 20px;
  background: #ff7875;
  color: white;
  font-size: 0.7em;
  padding: 3px 8px;
  border-radius: 4px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}
/* 说说内容里的图片自适应 */
.post-item img {
  max-width: 100%;
  border-radius: 4px;
  margin: 10px 0;
}
/* 说说操作按钮 */
.post-actions {
  margin-top: 15px;
  padding-top: 10px;
  border-top: 1px dashed #f0f0f0; /* 按钮区上划线分隔 */
  text-align: right;
}
.post-actions button {
  background: #f8f8f8;
  color: #666;
  border: 1px solid #e5e5e5;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8em;
  margin-left: 8px;
  transition: all 0.2s;
}
.post-actions button:hover {
  background: #f0f0f0;
  border-color: #ddd;
}

/* 留言区域样式 */
.comments-section {
  margin-top: 15px;
  padding: 12px;
  background: #fcfcfc;
  border-radius: 6px;
  border: 1px solid #f5f5f5;
}
.comments-section h4 {
  margin: 0 0 10px 0;
  font-size: 0.9em;
  color: #666;
}
.comment-item {
  font-size: 0.9em;
  padding: 6px 0;
  border-bottom: 1px dashed #f5f5f5;
}
.comment-form {
  margin-top: 10px;
  display: none;
}
.comment-form input, .comment-form textarea {
  padding: 6px;
  margin: 4px 0;
  font-size: 0.9em;
}

/* 插画专栏样式 */
#illustrationSection {
  max-width: 700px;
  margin: 0 auto;
  display: none;
}
.illustration-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 15px;
  margin-top: 20px;
}
.illustration-item {
  background: #fafafa;
  padding: 10px;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  text-align: center;
  transition: transform 0.5s ease;
}
.illustration-item:hover {
  transform: translateY(-3px);
}
.illustration-item img {
  width: 100%;
  border-radius: 4px;
  margin-bottom: 8px;
}
.illustration-item p {
  font-size: 0.9em;
  color: #666;
  margin: 0;
}

/* 点击时光符号特效 */
@keyframes floatSlow {
  100% { transform: translate(0, -15px); opacity: 0; }
}

/* 内容区域默认隐藏，密码验证后显示 */
#siteContent {
  display: none;
}
</style>

<!-- 密码验证页面（默认全屏显示） -->
<div id="passwordScreen">
  <div id="passwordBox">
    <h2>🔒 时光信笺</h2>
    <p>请输入密码进入</p>
    <input type="password" id="passwordInput" placeholder="输入密码...">
    <button id="passwordBtn" onclick="checkPassword()">进入</button>
    <div id="errorMsg">密码错误，请重新输入</div>
  </div>
</div>

<!-- 网站实际内容（默认隐藏） -->
<div id="siteContent">
  <h1>📮 送给你的时光信笺</h1>
  <p style="text-align: center; color: #666;">就当作，这一切的开始</p>

  <!-- 导航栏：切换说说/插画 -->
  <div class="nav-bar">
    <button class="nav-btn active" onclick="switchTab('post')">碎碎念</button>
    <button class="nav-btn" onclick="switchTab('illustration')">插画小馆</button>
  </div>

  <!-- 说说区域 -->
  <div id="postSection">
    <!-- 发布新说说的表单 -->
    <div class="form-container" id="postForm">
      <h3>✍️ 写一条新的碎碎念</h3>
      <input type="text" id="postDate" placeholder="输入日期（格式：2026-01-26）" value="YYYY-MM-DD">
      <input type="text" id="postEmoji" placeholder="加个小图标（比如：🍓）" value="✨">
      <textarea id="postContent" rows="3" placeholder="想对朋友说的话..."></textarea>
      <input type="text" id="postImage" placeholder="图片链接（可选，上传图片后填这里）">
      <label style="font-size: 0.9em; color: #666; margin-right: 10px;">
        <input type="checkbox" id="isTop"> 设为置顶
      </label>
      <button onclick="addPost()">发布</button>
    </div>

    <h2>✨ 我的碎碎念</h2>
    <!-- 说说容器 -->
    <div id="postsContainer">
      <!-- 示例置顶说说 -->
      <div class="post-item" data-top="true">
        <span class="top-tag">置顶</span>
        > **❤️ 2026-01-01**
        > 这是送给你的专属时光信笺，愿我们的友谊岁岁年年～
        <div class="post-actions">
          <button onclick="toggleTop(this)">取消置顶</button>
          <button onclick="editPost(this)">编辑</button>
          <button onclick="deletePost(this)">删除</button>
          <button onclick="toggleCommentForm(this)">留言</button>
        </div>
        <div class="comments-section">
          <h4>💬 留言板</h4>
          <div class="comment-item">朋友：太喜欢啦！谢谢你～</div>
          <div class="comment-form">
            <input type="text" placeholder="你的名字" class="comment-name">
            <textarea rows="2" placeholder="想留言的内容..." class="comment-content"></textarea>
            <button onclick="addComment(this)">提交留言</button>
          </div>
        </div>
      </div>

      <!-- 示例说说1 -->
      <div class="post-item">
        > ** 🍓2026-01-21**
        > 今天买了超甜的草莓，替你尝了一颗～
        > ![草莓照片](https://picsum.photos/id/1080/600/300)
        <div class="post-actions">
          <button onclick="toggleTop(this)">设为置顶</button>
          <button onclick="editPost(this)">编辑</button>
          <button onclick="deletePost(this)">删除</button>
          <button onclick="toggleCommentForm(this)">留言</button>
        </div>
        <div class="comments-section">
          <h4>💬 留言板</h4>
          <div class="comment-item">朋友：看起来好好吃！下次带我一起买～</div>
          <div class="comment-form">
            <input type="text" placeholder="你的名字" class="comment-name">
            <textarea rows="2" placeholder="想留言的内容..." class="comment-content"></textarea>
            <button onclick="addComment(this)">提交留言</button>
          </div>
        </div>
      </div>

      <!-- 示例说说2 -->
      <div class="post-item">
        > **🌧️2026-01-24**
        > 今天下雨了，想起去年和你躲雨的屋檐～
        > ![雨天照片](https://picsum.photos/id/175/600/300)
        <div class="post-actions">
          <button onclick="toggleTop(this)">设为置顶</button>
          <button onclick="editPost(this)">编辑</button>
          <button onclick="deletePost(this)">删除</button>
          <button onclick="toggleCommentForm(this)">留言</button>
        </div>
        <div class="comments-section">
          <h4>💬 留言板</h4>
          <div class="comment-form">
            <input type="text" placeholder="你的名字" class="comment-name">
            <textarea rows="2" placeholder="想留言的内容..." class="comment-content"></textarea>
            <button onclick="addComment(this)">提交留言</button>
          </div>
        </div>
      </div>
    </div>

    <!-- 编辑说说的弹窗（默认隐藏） -->
    <div class="form-container" id="editForm" style="display: none;">
      <h3>✏️ 编辑碎碎念</h3>
      <input type="text" id="editDate" placeholder="输入日期（格式：2026-01-26）">
      <input type="text" id="editEmoji" placeholder="加个小图标（比如：🍓）">
      <textarea id="editContent" rows="3" placeholder="想对朋友说的话..."></textarea>
      <input type="text" id="editImage" placeholder="图片链接（可选）">
      <label style="font-size: 0.9em; color: #666; margin-right: 10px;">
        <input type="checkbox" id="editIsTop"> 设为置顶
      </label>
      <button onclick="saveEdit()">保存</button>
      <button class="btn-cancel" onclick="cancelEdit()">取消</button>
    </div>
  </div>

  <!-- 插画专栏区域 -->
  <div id="illustrationSection">
    <!-- 上传插画的表单 -->
    <div class="form-container">
      <h3>🎨 添一张新插画</h3>
      <input type="text" id="illustrationTitle" placeholder="插画标题（比如：春日小雏菊）">
      <input type="text" id="illustrationUrl" placeholder="插画图片链接">
      <button onclick="addIllustration()">添加</button>
    </div>

    <h2>🎨 插画小馆</h2>
    <p style="text-align: center; color: #666;">挑一些你可能喜欢的小画，慢慢看呀～</p>
    <div class="illustration-grid" id="illustrationGrid">
      <!-- 示例插画 -->
      <div class="illustration-item">
        <img src="https://picsum.photos/id/102/200/200" alt="插画1">
        <p>春日山野</p>
      </div>
      <div class="illustration-item">
        <img src="https://picsum.photos/id/103/200/200" alt="插画2">
        <p>猫咪午后</p>
      </div>
    </div>
  </div>
</div>

<!-- 核心JS代码 -->
<script>
// ########## 1. 密码验证配置 ##########
const CORRECT_PASSWORD = "20251018"; // 修改成你的密码

function checkPassword() {
  const inputPassword = document.getElementById('passwordInput').value;
  const errorMsg = document.getElementById('errorMsg');
  
  if (inputPassword === CORRECT_PASSWORD) {
    document.getElementById('passwordScreen').style.display = 'none';
    document.getElementById('siteContent').style.display = 'block';
  } else {
    errorMsg.style.display = 'block';
    setTimeout(() => errorMsg.style.display = 'none', 3000);
    document.getElementById('passwordInput').value = '';
  }
}

document.getElementById('passwordInput').addEventListener('keydown', function(e) {
  if (e.key === 'Enter') checkPassword();
});

// ########## 2. 切换说说/插画标签页 ##########
function switchTab(tabName) {
  const navBtns = document.querySelectorAll('.nav-btn');
  navBtns.forEach(btn => btn.classList.remove('active'));
  event.target.classList.add('active');

  if (tabName === 'post') {
    document.getElementById('postSection').style.display = 'block';
    document.getElementById('illustrationSection').style.display = 'none';
  } else {
    document.getElementById('postSection').style.display = 'none';
    document.getElementById('illustrationSection').style.display = 'block';
  }
}

// ########## 3. 说说置顶核心功能 ##########
function toggleTop(btn) {
  const postItem = btn.closest('.post-item');
  const isTop = postItem.getAttribute('data-top') === 'true';

  if (isTop) {
    // 取消置顶
    postItem.setAttribute('data-top', 'false');
    btn.textContent = '设为置顶';
    postItem.querySelector('.top-tag')?.remove();
    sortPosts();
  } else {
    // 设为置顶：先移除所有其他置顶
    document.querySelectorAll('.post-item[data-top="true"]').forEach(item => {
      item.setAttribute('data-top', 'false');
      item.querySelector('.post-actions button:first-child').textContent = '设为置顶';
      item.querySelector('.top-tag')?.remove();
    });
    postItem.setAttribute('data-top', 'true');
    btn.textContent = '取消置顶';
    // 添加置顶标签
    const topTag = document.createElement('span');
    topTag.className = 'top-tag';
    topTag.textContent = '置顶';
    postItem.appendChild(topTag);
    sortPosts();
  }
}

// 说说排序：置顶的在最前面
function sortPosts() {
  const container = document.getElementById('postsContainer');
  const posts = Array.from(container.querySelectorAll('.post-item'));
  
  container.innerHTML = '';
  // 置顶说说
  posts.filter(post => post.getAttribute('data-top') === 'true').forEach(post => container.appendChild(post));
  // 普通说说
  posts.filter(post => post.getAttribute('data-top') !== 'true').forEach(post => container.appendChild(post));
}

// ########## 4. 说说功能（发布/编辑/删除/留言） ##########
let currentEditPost = null;

function addPost() {
  const date = document.getElementById('postDate').value.trim();
  const emoji = document.getElementById('postEmoji').value.trim();
  const content = document.getElementById('postContent').value.trim();
  const image = document.getElementById('postImage').value.trim();
  const isTop = document.getElementById('isTop').checked;

  if (!date || !content || date === 'YYYY-MM-DD') {
    alert('日期和内容不能为空，格式要正确哦～');
    return;
  }

  // 拼接新说说HTML
  let topHtml = isTop ? '<span class="top-tag">置顶</span>' : '';
  let topBtnText = isTop ? '取消置顶' : '设为置顶';
  let postHtml = `
  <div class="post-item" data-top="${isTop}">
    ${topHtml}
    > **${emoji || '✨'} ${date}**
    > ${content}${image ? '\n> ![图片](' + image + ')' : ''}
    <div class="post-actions">
      <button onclick="toggleTop(this)">${topBtnText}</button>
      <button onclick="editPost(this)">编辑</button>
      <button onclick="deletePost(this)">删除</button>
      <button onclick="toggleCommentForm(this)">留言</button>
    </div>
    <div class="comments-section">
      <h4>💬 留言板</h4>
      <div class="comment-form">
        <input type="text" placeholder="你的名字" class="comment-name">
        <textarea rows="2" placeholder="想留言的内容..." class="comment-content"></textarea>
        <button onclick="addComment(this)">提交留言</button>
      </div>
    </div>
  </div>`;

  const container = document.getElementById('postsContainer');
  const newPost = document.createElement('div');
  newPost.innerHTML = postHtml;
  container.insertBefore(newPost, container.firstChild);

  // 如果是置顶，先取消其他置顶
  if (isTop) {
    document.querySelectorAll('.post-item[data-top="true"]').forEach(item => {
      if (item !== newPost.firstChild) {
        item.setAttribute('data-top', 'false');
        item.querySelector('.post-actions button:first-child').textContent = '设为置顶';
        item.querySelector('.top-tag')?.remove();
      }
    });
  }

  // 清空表单
  document.getElementById('postDate').value = 'YYYY-MM-DD';
  document.getElementById('postEmoji').value = '✨';
  document.getElementById('postContent').value = '';
  document.getElementById('postImage').value = '';
  document.getElementById('isTop').checked = false;
  alert('发布成功！新说说已添加～');
}

function editPost(btn) {
  currentEditPost = btn.closest('.post-item');
  const postText = currentEditPost.textContent;
  const emojiMatch = postText.match(/\*\*([^ ]+) (\d{4}-\d{2}-\d{2})\*\*/);
  const emoji = emojiMatch ? emojiMatch[1] : '✨';
  const date = emojiMatch ? emojiMatch[2] : '';
  const contentMatch = postText.split(`${date}**`)[1]?.split('![图片](')[0]?.trim() || '';
  const imageMatch = postText.match(/!\[图片\]\((.*?)\)/);
  const image = imageMatch ? imageMatch[1] : '';
  const isTop = currentEditPost.getAttribute('data-top') === 'true';

  // 填充编辑表单
  document.getElementById('editDate').value = date;
  document.getElementById('editEmoji').value = emoji;
  document.getElementById('editContent').value = contentMatch;
  document.getElementById('editImage').value = image;
  document.getElementById('editIsTop').checked = isTop;
  document.getElementById('postForm').style.display = 'none';
  document.getElementById('editForm').style.display = 'block';
}

function saveEdit() {
  if (!currentEditPost) return;
  const date = document.getElementById('editDate').value.trim();
  const emoji = document.getElementById('editEmoji').value.trim();
  const content = document.getElementById('editContent').value.trim();
  const image = document.getElementById('editImage').value.trim();
  const isTop = document.getElementById('editIsTop').checked;

  if (!date || !content) {
    alert('日期和内容不能为空！');
    return;
  }

  // 更新置顶状态
  if (isTop) {
    document.querySelectorAll('.post-item[data-top="true"]').forEach(item => {
      item.setAttribute('data-top', 'false');
      item.querySelector('.post-actions button:first-child').textContent = '设为置顶';
      item.querySelector('.top-tag')?.remove();
    });
    currentEditPost.setAttribute('data-top', 'true');
    currentEditPost.querySelector('.post-actions button:first-child').textContent = '取消置顶';
    if (!currentEditPost.querySelector('.top-tag')) {
      const topTag = document.createElement('span');
      topTag.className = 'top-tag';
      topTag.textContent = '置顶';
      currentEditPost.appendChild(topTag);
    }
  } else {
    currentEditPost.setAttribute('data-top', 'false');
    currentEditPost.querySelector('.post-actions button:first-child').textContent = '设为置顶';
    currentEditPost.querySelector('.top-tag')?.remove();
  }

  // 更新说说内容
  const newContent = `> **${emoji} ${date}**\n> ${content}${image ? '\n> ![图片](' + image + ')' : ''}`;
  currentEditPost.innerHTML = currentEditPost.innerHTML.replace(
    currentEditPost.querySelector('.post-item').innerHTML, 
    newContent
  );
  
  document.getElementById('editForm').style.display = 'none';
  document.getElementById('postForm').style.display = 'block';
  currentEditPost = null;
  sortPosts();
  alert('编辑成功！');
}

function cancelEdit() {
  document.getElementById('editForm').style.display = 'none';
  document.getElementById('postForm').style.display = 'block';
  currentEditPost = null;
}

function deletePost(btn) {
  if (confirm('确定删除这条说说吗？')) {
    btn.closest('.post-item').remove();
    alert('删除成功～');
  }
}

function toggleCommentForm(btn) {
  const commentForm = btn.closest('.post-item').querySelector('.comment-form');
  commentForm.style.display = commentForm.style.display === 'block' ? 'none' : 'block';
}

function addComment(btn) {
  const commentForm = btn.parentElement;
  const name = commentForm.querySelector('.comment-name').value.trim();
  const content = commentForm.querySelector('.comment-content').value.trim();

  if (!name || !content) {
    alert('名字和留言内容不能为空哦～');
    return;
  }

  const commentItem = document.createElement('div');
  commentItem.className = 'comment-item';
  commentItem.textContent = `${name}：${content}`;

  const commentsSection = commentForm.parentElement;
  commentsSection.insertBefore(commentItem, commentForm);

  commentForm.querySelector('.comment-name').value = '';
  commentForm.querySelector('.comment-content').value = '';
  alert('留言提交成功！');
}

// ########## 4. 插画功能（添加插画） ##########
function addIllustration() {
  const title = document.getElementById('illustrationTitle').value.trim();
  const url = document.getElementById('illustrationUrl').value.trim();

  if (!title || !url) {
    alert('标题和图片链接不能为空哦～');
    return;
  }

  const illustrationItem = document.createElement('div');
  illustrationItem.className = 'illustration-item';
  illustrationItem.innerHTML = `
    <img src="${url}" alt="${title}">
    <p>${title}</p>
  `;

  document.getElementById('illustrationGrid').appendChild(illustrationItem);

  document.getElementById('illustrationTitle').value = '';
  document.getElementById('illustrationUrl').value = '';
  alert('插画添加成功！');
}

// ########## 5. 点击时光符号特效 ##########
document.addEventListener('click', function(e) {
  const dot = document.createElement('div');
  const symbols = ['📝', '🍃', '🌟', '📮'];
  dot.textContent = symbols[Math.floor(Math.random() * symbols.length)];
  dot.style.cssText = `
    position: absolute;
    left: ${e.clientX}px;
    top: ${e.clientY}px;
    font-size: 12px;
    pointer-events: none;
    animation: floatSlow 2s ease-out forwards;
    z-index: 9999;
  `;
  document.body.appendChild(dot);
  setTimeout(() => dot.remove(), 2000);
});
</script>
