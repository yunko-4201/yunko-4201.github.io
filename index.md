---
layout: home
title: 给XX的时光信笺
---
<!-- 引入样式和动态效果 -->
<link rel="stylesheet" href="/style.css">

<!-- 标题渐入动画 -->
<style>
h1 {
  animation: fadeIn 1.8s ease-in-out;
  text-align: center;
  color: #333;
  font-weight: normal;
  margin: 30px 0;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
/* 分隔标语动画 */
.divider {
  display: block;
  text-align: center;
  margin: 20px 0;
  color: #666;
  opacity: 0;
  transform: scaleX(0);
  transform-origin: center;
  animation: openLetter 1.5s forwards;
}
@keyframes openLetter {
  to { opacity: 1; transform: scaleX(1); }
}
/* 点击时光符号特效 */
@keyframes floatSlow {
  100% { transform: translate(0, -15px); opacity: 0; }
}
</style>

# 📮 给XX的时光信笺
_攒满两年的日常，等见面时送给你_

## ✨ 我的碎碎念
<!-- 👇 新增内容贴这里，自动置顶（最新的在最上面） -->
> **🍓 2026-01-25**
> 今天买了超甜的草莓，替你尝了一颗～
> ![草莓照片](https://你的图片链接.jpg)

<!-- 自定义分隔小标语（改中间文字即可） -->
> <span class="divider">✨—— 一月碎碎念结束 · 二月继续攒日常 ——✨</span>

> **🌧️ 2026-01-24**
> 今天下雨了，想起去年和你躲雨的屋檐～
> ![雨天照片](https://你的图片链接.jpg)

> **☕️ 2026-01-23**
> 喝到了你最爱的焦糖玛奇朵，味道超赞～

<!-- 点击出现时光符号特效 -->
<script>
document.addEventListener('click', function(e) {
  const dot = document.createElement('div');
  const symbols = ['📝', '🍃', '🌟', '📮']; // 时光感符号，可自定义
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
