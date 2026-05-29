---
title: Memos CSS 样式与 JS 脚本分享
date: 2026-05-28 22:07:52
tags:
---

# Memos样式与脚本

\#VibeCoding #memos #建站

# 起因

从去年开始看到 flomo 开始增加了一个收费档位（虽然当时好像还只是个类似深度用户俱乐部的会费，但是今年已经演变成比普通会员高一个档位的会员了），外加自己实际上 flomo 使用频率也不是特别高，最大的需求可能只是想要一个能够全平台快速使用、传递文本的类便签，或者说闪念笔记应用。这个月终于狠下心来买了一个腾讯云的轻量应用服务器，搜罗了半天开源可自托管部署的笔记应用，找到了 memos 和 blinko 两款。

一开始是使用的memos，觉得有点简陋，又重新部署了blinko，使用了半天，觉得移动端的 PWA 体验不是特别好，最后还是部署回了memos，至少它有其他用户写的第三方原生客户端（最好笑的是——自己以前下载过iOS客户端，但是当时不知道怎么用）。后来一番研究琢磨，发现自定义CSS样式和自定义JS脚本功能实在是太强大了。故单开一个文件来分享（留存）我捣鼓出来的样式和脚本。

# 存档点

## 自定义主题

memos本身的主题其实已经很好看了，但是我就是那种喜欢折腾的人，所以让 Gemini 根据我喜欢的颜色从新写了份CSS样式。

![QQ_1779975109634.png](https://resv2.craft.do/user/full/d8d9f551-27be-8f86-44b5-701ada6bb4d1/doc/2d6328bb-4a8d-4194-b8cb-a452fe35f1e7/02335990-8838-4180-ae54-0c08b49d48fa)

**CSS样式：白色背景、淡蓝色强调色辅以红色与黄色的亮色主题**

```javascript
/* 🎯 精准打击：只有当 data-theme 属性完美等于 "default" 时才生效 */
html[data-theme="default"] {
  /* 显式声明亮色模式偏好 */
  color-scheme: light;

  /* 基础底色 - 纯白系 */
  --background: #ffffff;
  --foreground: #2d2d2d;

  /* 卡片/面板 */
  --card: #fafafa;
  --card-foreground: #2d2d2d;

  /* 悬浮弹窗 */
  --popover: #ffffff;
  --popover-foreground: #2d2d2d;

  /* 主色：淡蓝色 */
  --primary: #93c5fd;
  --primary-foreground: #1e293b;

  /* 辅助色 */
  --secondary: #cbd5e1;
  --secondary-foreground: #333333;

  /* 弱化文字/背景 */
  --muted: #f1f5f9;
  --muted-foreground: #64748b;

  /* 强调色：橙金色 */
  --accent: #fff7ed;
  --accent-foreground: #c2410c;

  /* 警示/点缀：红色 */
  --destructive: #ef4444;
  --destructive-foreground: #ffffff;

  /* 边框、输入框、焦点环 */
  --border: #e2e8f0;
  --input: #f8fafc;
  --ring: #93c5fd;

  /* 侧边栏 */
  --sidebar: #f8fafc;
  --sidebar-foreground: #475569;
  --sidebar-accent: #e2e8f0;
  --sidebar-accent-foreground: #2d2d2d;
}

/* 🎯 标题分级配色：仅在 default 下生效 */
html[data-theme="default"] h1, 
html[data-theme="default"] .memo-content h1, 
html[data-theme="default"] .markdown-content h1, 
html[data-theme="default"] .prose h1 {
  color: #dc2626; /* 正红 */
}

html[data-theme="default"] h2, 
html[data-theme="default"] .memo-content h2, 
html[data-theme="default"] .markdown-content h2, 
html[data-theme="default"] .prose h2 {
  color: #ea580c; /* 深橙金 */
}

html[data-theme="default"] h3, 
html[data-theme="default"] .memo-content h3, 
html[data-theme="default"] .markdown-content h3, 
html[data-theme="default"] .prose h3 {
  color: #f59e0b; /* 橙金色 */
}

html[data-theme="default"] h4, 
html[data-theme="default"] .memo-content h4, 
html[data-theme="default"] .markdown-content h4, 
html[data-theme="default"] .prose h4 {
  color: #fbbf24; /* 浅橙金 */
}

html[data-theme="default"] h5, 
html[data-theme="default"] .memo-content h5, 
html[data-theme="default"] .markdown-content h5, 
html[data-theme="default"] .prose h5 {
  color: #60a5fa; /* 标准淡蓝 */
}

html[data-theme="default"] h6, 
html[data-theme="default"] .memo-content h6, 
html[data-theme="default"] .markdown-content h6, 
html[data-theme="default"] .prose h6 {
  color: #93c5fd; /* 浅淡蓝 */
}

/* 🎯 滚动条样式 */
html[data-theme="default"] .scroll-area {
  overflow: auto;
  scrollbar-width: thin;
  scrollbar-color: #cbd5e1 #ffffff;
}

/* 🎯 标签样式：只污染 default，放过 paper 和暗色 */
html[data-theme="default"] .memo-tags .tag, 
html[data-theme="default"] .memo-tag, 
html[data-theme="default"] a[href*="/tags/"], 
html[data-theme="default"] span[class*="tag"] {
  border: none !important;
  box-shadow: none !important;
  color: #60a5fa !important;
  background-color: #f1f5f9 !important;
}
```

只有在使用亮色模式的时候才会生效，解决了会覆盖暗色模式和 Paper 主题的bug。

---

## 媒体按钮汉化

输入框的添加按钮里插入媒体的文本仍然是 Media，强迫症利用 CSS 样式汉化了一下。

![QQ_1779975202727.png](https://resv2.craft.do/user/full/d8d9f551-27be-8f86-44b5-701ada6bb4d1/doc/2d6328bb-4a8d-4194-b8cb-a452fe35f1e7/704bd1ef-9684-4bba-8b2a-5b64ce673e55)

**CSS 样式：汉化媒体按钮**

```javascript
/* 终极修复：只替换带 lucide-image 图标的菜单项 */
[data-slot="dropdown-menu-item"]:has(svg.lucide.lucide-image) {
  position: relative !important;
  font-size: 0 !important; /* 隐藏原文字 */
}

[data-slot="dropdown-menu-item"]:has(svg.lucide.lucide-image)::after {
  content: "媒体" !important;
  position: absolute !important;
  left: 33px !important; /* 和其他文字对齐 */
  top: 50% !important;
  transform: translateY(-50%) !important;
  font-size: 14px !important; /* 恢复文字大小 */
  color: inherit !important;
  white-space: nowrap !important;
  line-height: 1 !important;
}
```

原理是通过带 lucide-image 的图标来匹配文本的，然后将源文本隐藏再加上的新的文本，我还手动微调了下位置。事实上这个方法如果网站中还有第二个带 lucide-image图标的按钮，不管文本是什么也会被替换成“媒体”的，只能祈祷没有了——或者有了再想办法。

---

## 悬浮窗预览笔记详情

其实项目的 github Discussion 分区已经有中国用户做了一个仿小红书的点击弹窗预览了，但是人家是 fork 了源码自己改的，不是用 CSS 样式和 JS 脚本实现的。我是懒人，我想用官方版，而且万一后续更新还要重新 fork 重新 build，对我来说更加麻烦。

![QQ_1779975253040.png](https://resv2.craft.do/user/full/d8d9f551-27be-8f86-44b5-701ada6bb4d1/doc/2d6328bb-4a8d-4194-b8cb-a452fe35f1e7/745650a9-8fef-40c7-a70e-459507fdabf7)

**CSS 样式：悬浮窗 CSS 样式控制**

```javascript
/* 弹窗基本骨架 */
#memo-custom-modal {
  position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
  z-index: 9999; display: flex; align-items: center; justify-content: center;
}
.memo-modal-backdrop {
  position: absolute; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(15, 23, 42, 0.5); backdrop-filter: blur(5px);
}

/* 🌟 核心：弹窗主体容器，给内嵌网页预留大空间 */
.memo-modal-container {
  position: relative; 
  width: 92%; 
  max-width: 820px; /* 适当拓宽宽度，完美展示无侧边栏的详情页排版 */
  height: 88vh;     /* 固定高度，让 iframe 在内部独立滚动 */
  background: var(--bg-main, #ffffff); 
  border: 1px solid var(--border-sub, #e2e8f0);
  border-radius: 18px; 
  box-shadow: 0 25px 60px -15px rgba(0, 0, 0, 0.3);
  display: flex; flex-direction: column; overflow: hidden;
  animation: memoIframeModalShow 0.18s cubic-bezier(0.16, 1, 0.3, 1);
}

/* 占满容器的嵌入页包裹器 */
.memo-modal-content-wrapper {
  width: 100%;
  height: 100%;
  overflow: hidden;
}

/* 🌟 关闭按钮：完全移至左侧 */
.memo-modal-close {
  position: absolute; 
  top: 16px; 
  left: 18px; /* 完全放在左侧 */
  
  /* 精致圆角半透明布局，确保不和iframe里的东西重合得太割裂 */
  background: rgba(0, 0, 0, 0.08); 
  border: none; 
  font-size: 14px; 
  cursor: pointer; 
  color: #718096; /* 统一颜色，防止夜间模式下对比太强 */
  z-index: 10000; /* 确保在 iframe 之上 */
  
  width: 30px; 
  height: 30px; 
  border-radius: 8px; /* 方圆角更现代 */
  display: flex; align-items: center; justify-content: center;
  transition: all 0.15s;
}
.memo-modal-close:hover { 
  background: rgba(0, 0, 0, 0.15); 
  color: #1a202c; 
}

/* 兼容 Memos 夜间模式补丁 */
html.dark .memo-modal-container { 
  background: #1e293b; border-color: #334155; 
}
html.dark .memo-modal-close { 
  background: rgba(255, 255, 255, 0.1); color: #cbd5e1; 
}
html.dark .memo-modal-close:hover { 
  background: rgba(255, 255, 255, 0.18); color: #ffffff; 
}

/* 转圈动画 */
.memo-iframe-spinner { display: flex; align-items: center; justify-content: center; height: 100%; color: var(--text-sub, #64748b); font-size: 14px; }
.memo-iframe-spinner::before {
  content: ""; width: 20px; height: 20px; margin-right: 12px;
  border: 2px solid var(--border-sub, #cbd5e1); border-top-color: var(--brand-main, #3b82f6);
  border-radius: 50%; animation: memoIframeSpin 0.6s linear infinite;
}
@keyframes memoIframeSpin { to { transform: rotate(360deg); } }
@keyframes memoIframeModalShow { from { transform: translateY(12px) scale(0.98); opacity: 0; } to { transform: translateY(0) scale(1); opacity: 1; } }
```

**JS 脚本：悬浮窗触发**

```javascript
// ==========================================
// 🌟 独立挂载：自愈隔离版详情页弹窗中心 (悬浮窗内部绝对隔离版)
// ==========================================
(function() {
  if (window.location.pathname.startsWith('/setting')) return;

  const CACHE_KEY = "memos_timestamp_uid_vault";
  const originalFetch = window.fetch;

  let memoryFallbackCache = {};
  let isLocalStorageAvailable = false;

  try {
    const testKey = "__memos_storage_test__";
    localStorage.setItem(testKey, testKey);
    localStorage.removeItem(testKey);
    isLocalStorageAvailable = true;
  } catch (e) {
    console.warn("⚠️ LocalStorage 已被浏览器禁用，自动降级为纯内存缓存模式。");
    isLocalStorageAvailable = false;
  }

  function getSafeCache() {
    if (isLocalStorageAvailable) {
      try {
        const data = localStorage.getItem(CACHE_KEY);
        return data ? JSON.parse(data) : {};
      } catch (e) {
        return {};
      }
    } else {
      return memoryFallbackCache;
    }
  }

  function saveSafeCache(cacheObj) {
    if (isLocalStorageAvailable) {
      try {
        localStorage.setItem(CACHE_KEY, JSON.stringify(cacheObj));
      } catch (e) {}
    } else {
      memoryFallbackCache = cacheObj;
    }
  }

  function populateCache(memosArray) {
    if (!memosArray || memosArray.length === 0) return;
    const currentCache = getSafeCache();
    let hasNew = false;

    memosArray.forEach(memo => {
      if (memo.name && memo.createTime) {
        const uid = memo.name.split('/').pop();
        const standardizedTime = new Date(memo.createTime).toISOString();
        if (currentCache[standardizedTime] !== uid) {
          currentCache[standardizedTime] = uid;
          hasNew = true;
        }
      }
    });

    if (hasNew) {
      saveSafeCache(currentCache);
    }
  }

  function findUidInCache(targetIsoTime) {
    const currentCache = getSafeCache();
    if (currentCache[targetIsoTime]) {
      return currentCache[targetIsoTime];
    }
    const targetMs = new Date(targetIsoTime).getTime();
    for (let cachedIsoTime in currentCache) {
      const cachedMs = new Date(cachedIsoTime).getTime();
      if (Math.abs(targetMs - cachedMs) <= 2500) {
        return currentCache[cachedIsoTime];
      }
    }
    return null;
  }

  window.fetch = async function (...args) {
    const url = args[0] ? args[0].toString() : "";
    const options = args[1] || {};

    const response = await originalFetch.apply(this, args);

    if (url.includes('/api/v1/memos') && options.method === 'GET' && response.ok) {
      try {
        const cloneRes = response.clone();
        cloneRes.json().then(data => populateCache(data.memos));
      } catch (e) {}
    }
    return response;
  };

  document.addEventListener("click", async function (e) {
    const timeButton = e.target.closest("button.text-muted-foreground");
    if (!timeButton) return;
    const timeEl = timeButton.querySelector("relative-time");
    if (!timeEl) return;

    e.preventDefault();
    e.stopPropagation();

    const domDatetime = timeEl.getAttribute("datetime");
    const targetIsoTime = new Date(domDatetime).toISOString();

    let realUid = findUidInCache(targetIsoTime);

    if (realUid) {
      openIframeModalDirectly(realUid);
      return;
    }

    showStyledIframeModal("<div class='memo-iframe-spinner'>本地未命中缓存，正在检索历史列表中...</div>");

    try {
      let token = "";
      for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i);
        if (key && (key.includes("memos") || key.includes("auth") || key.includes("token"))) {
          try {
            const parsed = JSON.parse(localStorage.getItem(key));
            token = parsed.accessToken || parsed.token || parsed.val || token;
          } catch (err) {
            const val = localStorage.getItem(key);
            if (val && val.length > 30) token = val;
          }
        }
      }

      const headers = new Headers();
      if (token) headers.append("Authorization", `Bearer ${token}`);

      const listResponse = await originalFetch(`${window.location.origin}/api/v1/memos?pageSize=100`, {
        method: "GET",
        headers: headers
      });

      if (!listResponse.ok) throw new Error("同步私密列表失败");
      const listData = await listResponse.json();
      const memosArray = listData.memos || [];
      
      populateCache(memosArray);
      realUid = findUidInCache(targetIsoTime);

      if (realUid) {
        openIframeModalDirectly(realUid);
      } else {
        throw new Error("该笔记在近 100 条记录中未匹配到。");
      }
    } catch (err) {
      updateStyledModalContent(`<p style='padding:30px; color:#ef4444; font-size:14px; text-align:center;'>❌ 身份同步失败: ${err.message}</p>`);
    }
  }, true);

  function openIframeModalDirectly(uid) {
    const realDetailUrl = `${window.location.origin}/memos/${uid}`;
    const iframeHtml = `
      <iframe src="${realDetailUrl}" style="width:100%; height:100%; border:none; background:transparent;"
        allow="clipboard-write" id="memo-detail-iframe" onload="syncAuthAndInjectStyle(this)">
      </iframe>`;
    
    if (!document.getElementById("memo-custom-modal")) {
      showStyledIframeModal("");
    }
    updateStyledModalContent(iframeHtml);
  }
})();

// ==========================================
// 4. 弹窗公用安全凭证克隆及样式纯净注入 (悬浮窗内部沙箱环境)
// ==========================================
function syncAuthAndInjectStyle(iframe) {
  try {
    const iframeWindow = iframe.contentWindow;
    const iframeDoc = iframe.contentDocument || iframeWindow.document;
    if (!iframeDoc || !iframeWindow) return;

    // 克隆凭证
    try {
      for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i);
        if (key) {
          iframeWindow.localStorage.setItem(key, localStorage.getItem(key));
        }
      }
    } catch (secErr) {
      console.warn("Iframe 凭证同步受限:", secErr);
    }
    
    // 🌟 核心破局：把对大侧栏的轰炸代码，写在仅作用于 iframe 内部的样式表里！
    const styleString = `
      /* 🎯 1. 精准爆破：干掉悬浮窗里（无论详情页还是跳转后主页）的所有大侧栏和旧侧栏 */
      aside.sidebar, 
      .sidebar-wrapper, 
      [class*="sidebar"],
      div.sticky[class*="h-svh"][class*="w-64"] { 
        display: none !important; 
        width: 0 !important; 
        max-width: 0 !important;
        opacity: 0 !important; 
        visibility: hidden !important; 
      }

      /* 🎯 2. 精准爆破：干掉移动端顶部的站点图标名字按钮，保留汉堡按钮 */
      button[class*="h-8"][class*="px-2"] { 
        display: none !important; 
        width: 0 !important; 
        height: 0 !important; 
      }

      /* 🎯 3. 完美撑满：让悬浮窗里由于失去侧栏而空出来的区域100%填满 */
      header, .header-wrapper, [class*="header"] { display: none !important; }
      main, .main-wrapper, [class*="main-content"], div[class*="min-w-0"][class*="flex-1"] { 
        padding-left: 14px !important; 
        padding-right: 14px !important; 
        margin: 0 auto !important; 
        width: 100% !important; 
        max-width: 100% !important; 
        left: 0 !important; 
        transition: none !important; 
      }
      main > .w-full, section[class*="@container"] > .w-full { max-width: 100% !important; width: 100% !important; }
      main .py-4 { padding-top: 10px !important; }
    `;
    const styleEl = iframeDoc.createElement('style');
    styleEl.textContent = styleString;
    iframeDoc.head.appendChild(styleEl);
  } catch (e) {
    console.warn("Iframe 样式对齐受阻:", e);
  }
}

function showStyledIframeModal(initialContent) {
  const existing = document.getElementById("memo-custom-modal");
  if (existing) existing.remove();
  const modalOverlay = document.createElement("div");
  modalOverlay.id = "memo-custom-modal";
  modalOverlay.innerHTML = `
    <div class="memo-modal-backdrop"></div>
    <div class="memo-modal-container">
      <div class="memo-modal-content-wrapper">${initialContent}</div>
      <button class="memo-modal-close">✕</button>
    </div>`;
  const closeBox = () => modalOverlay.remove();
  modalOverlay.querySelector(".memo-modal-backdrop").addEventListener("click", closeBox);
  modalOverlay.querySelector(".memo-modal-close").addEventListener("click", closeBox);
  const escHandler = (e) => { if (e.key === 'Escape') closeBox(); document.removeEventListener('keydown', escHandler); };
  document.addEventListener('keydown', escHandler);
  document.body.appendChild(modalOverlay);
}

function updateStyledModalContent(html) {
  const wrapper = document.querySelector("#memo-custom-modal .memo-modal-content-wrapper");
  if (wrapper) wrapper.innerHTML = html;
}
```

实现思路是通过 Memos 的 API， 请求笔记列表，然后通过 html 代码里时间戳的唯一标识码进行匹配，获取 name ID，然后新建一个浮窗加载详情页，做了 CSS 注入，去掉了无用的侧栏。

---

## 瀑布流主页美化

原来的主页是一行一个卡片，社区其实有人做三栏的，但是我觉得太窄了，那个代码还加了 Quill 编辑器，但是实际上没几个功能可以渲染出来，所以索性自己弄了。原版的输入框也只有一行，太矮了，索性也一起改了。预览图参考上面主题 CSS 样式的截图。

**CSS 样式：双栏瀑布流美化（笔记输入框不会参与）**

```javascript
/* 🌟 Memos 响应式完美瀑布流布局（输入框完美独占通栏，大屏卡片双栏瀑布流） */

/* 1. 基础配置：外层主容器 */
.flex.flex-col.justify-start.w-full.max-w-2xl.mx-auto {
  display: block !important;    /* 彻底放弃 grid/flex，转为 block 以便支持 columns */
  width: 100% !important;
  max-width: 100% !important;
  
  /* 默认手机端：单栏 */
  column-count: 1 !important;
  column-gap: 16px !important;
}

/* 2. 🎯 核心修复：强行让输入框区域跳出分栏，独占一整行通栏 */
.flex.flex-col.justify-start.w-full.max-w-2xl.mx-auto > div:first-child,
.flex.flex-col.justify-start.w-full.max-w-2xl.mx-auto > header,
.flex.flex-col.justify-start.w-full.max-w-2xl.mx-auto > div.group.relative.w-full.flex.flex-col.justify-start {
  column-span: all !important;       /* 🌟 核心关键：强行横跨所有栏目，终结挤在一侧的 bug */
  -webkit-column-span: all !important; /* 兼容 Safari / Chrome 底层 */
  
  width: 100% !important;
  display: block !important;
  margin-bottom: 24px !important;    /* 输入框与下方瀑布流卡片流的间距 */
}

/* 3. 当屏幕宽度大于 768px（平板和电脑端）时，卡片流开启双栏瀑布流 */
@media (min-width: 768px) {
  .flex.flex-col.justify-start.w-full.max-w-2xl.mx-auto {
    column-count: 2 !important;      /* 电脑端：笔记卡片自动分为 2 栏瀑布流 */
    column-gap: 20px !important;     /* 两栏之间的左右间距 */
    max-width: 1200px !important;    /* 电脑端扩大最大总宽度 */
  }
}

/* 4. 笔记卡片表现：保持完整，绝不跨栏截断 */
.flex.flex-col.justify-start.w-full.max-w-2xl.mx-auto > article,
.flex.flex-col.justify-start.w-full.max-w-2xl.mx-auto > div.group.relative.w-full:not(:first-child) {
  display: inline-block !important; /* 必须是 inline-block 才能触发完好的分栏避让机制 */
  width: 100% !important;
  height: auto !important;
  margin-bottom: 16px !important;   /* 各个笔记卡片之间的垂直间距 */
  
  /* 告诉浏览器：绝对不要把这张卡片切成两半分别丢在左右两栏！ */
  break-inside: avoid !important;
  page-break-inside: avoid !important;
}

/* 🎯 强行将主页输入框的基础高度加高两行 */
html[data-theme="default"] textarea.w-full,
html[data-theme="default"] .memo-editor-content textarea,
html[data-theme="default"] textarea[placeholder*="想法"] {
  /* Memos 默认有行高，2.5rem 到 3rem 差不多就是两行半的物理高度，确保初始更宽敞 */
  min-height: 5.5rem !important; 
  
  /* 允许用户在必要时手动拖动右下角调整高度（可选，不想允许可以改成 resize: none） */
  resize: vertical !important; 
}

/* 调整包裹容器，防止内部 textarea 加高后撑出怪异的缝隙 */
html[data-theme="default"] .memo-editor-content {
  height: auto !important;
}
```

---

## 高亮与刮刮卡语法

之前挺喜欢 Mindbox 的高亮样式以及刮刮卡功能的，memos 自身无法渲染，尝试复刻了这个功能。

语法是 `==文字==` 默认是黄色的，`==[&color]文字==` 可以更改高亮颜色，可以填的颜色可以参见代码，如果填写 hide 就是刮刮卡功能。

![QQ_1779975343146.png](https://resv2.craft.do/user/full/d8d9f551-27be-8f86-44b5-701ada6bb4d1/doc/2d6328bb-4a8d-4194-b8cb-a452fe35f1e7/89b5f10c-ccc1-4a12-b950-02c04649fb1f)

![QQ_1779975357307.png](https://resv2.craft.do/user/full/d8d9f551-27be-8f86-44b5-701ada6bb4d1/doc/2d6328bb-4a8d-4194-b8cb-a452fe35f1e7/b44db22b-4152-4ec5-8305-a338ac6d57a3)

**CSS 样式：定义样式**

```javascript
/* ==========================================================================
   🎨 9 色圆角边框高亮
   ========================================================================== */
.memos-hl {
  padding: 2px 6px !important;
  border-radius: 6px !important;
  font-weight: 500 !important;
  display: inline-block !important;
  margin: 0 2px !important;
  font-size: 0.95em !important;
  box-sizing: border-box !important;
  text-decoration: none !important;
}

/* 9 色深度调和（浅底 + 稍深同色系边框 + 深色字） */
.mhl-yellow    { background-color: #fef08a !important; color: #854d0e !important; border: 1px solid #facc15 !important; }
.mhl-red       { background-color: #fee2e2 !important; color: #991b1b !important; border: 1px solid #fca5a5 !important; }
.mhl-blue      { background-color: #dbeafe !important; color: #1e40af !important; border: 1px solid #93c5fd !important; }
.mhl-teal      { background-color: #ccfbf1 !important; color: #115e59 !important; border: 1px solid #5eead4 !important; }
.mhl-grey      { background-color: #f3f4f6 !important; color: #374151 !important; border: 1px solid #d1d5db !important; }
.mhl-purple    { background-color: #f3e8ff !important; color: #6b21a8 !important; border: 1px solid #c084fc !important; }
.mhl-pink      { background-color: #fce7f3 !important; color: #9d174d !important; border: 1px solid #fbcfe8 !important; }
.mhl-green     { background-color: #dcfce7 !important; color: #166534 !important; border: 1px solid #86efac !important; }
.mhl-turquoise { background-color: #ecfeff !important; color: #083344 !important; border: 1px solid #22d3ee !important; }

/* ==========================================================================
   🕵️‍♂️ 专刮刮卡
   ========================================================================== */
.memos-spoiler {
  background-color: #2d3748 !important; /* 未刮开时的电影胶片深灰 */
  color: #2d3748 !important;            /* 完美的文字隐形 */
  padding: 1px 5px !important;
  border-radius: 4px !important;
  cursor: pointer !important;
  transition: all 0.25s ease-in-out !important;
  user-select: none !important;         /* 强制未刮开时不允许穿透复制 */
  box-shadow: inset 0 0 4px rgba(0,0,0,0.3) !important;
  display: inline-block !important;
}

/* 🔓 电脑端悬停，或者手机端点击后触发（revealed）直接刮开 */
.memos-spoiler:hover,
.memos-spoiler.revealed {
  background-color: #edf2f7 !important; /* 优雅的刮开后亮灰底色 */
  color: #2d3748 !important;            /* 显现真容 */
  user-select: text !important;
  box-shadow: none !important;
}
```

**JS 脚本：实时渲染**

```javascript
(function() {
  'use strict';

  function doMemosMagic() {
    // 精准锁定 Memos 的内容渲染容器
    const blocks = document.querySelectorAll('[data-memo-content="true"], .memo-content, .markdown-content');
    
    blocks.forEach(block => {
      if (block.tagName === 'TEXTAREA' || block.closest('.memo-editor')) return;
      
      let html = block.innerHTML;
      
      // 避免无限循环重绘，内容无变化则直接跳过
      if (block.getAttribute('data-v-custom-html') === html) return;

      let hasChanged = false;

      // 🔍 核心修复：必须包含 == 才有必要进行正则深挖
      if (html.includes('==')) {
        
        // 🌟 第一步：【最高优先级】精准狙击带有 `==[&颜色] 文本==` 的内容
        // 考虑到 Memos 后端可能会把 & 转义为 &amp;，这里用 &amp;? 进行极强容错捕获
        const colorRegex = /==\[&amp;?([a-zA-Z]+)\]\s*([\s\S]*?)==/g;
        
        html = html.replace(colorRegex, function(match, colorName, text) {
          hasChanged = true;
          const color = colorName.toLowerCase();
          
          // 如果暗号是 hide，瞬间剥离并转换为刮刮卡
          if (color === 'hide') {
            return `<span class="memos-spoiler" title="点击查看隐藏内容" onclick="this.classList.toggle('revealed')">${text}</span>`;
          }
          
          // 否则，转换为对应的 9 色高级圆角高亮
          return `<mark class="memos-hl mhl-${color}">${text}</mark>`;
        });

        // 🌟 第二步：【低优先级兜底】在吃掉所有彩色暗号后，最后收拾干净剩下的 `==普通文本==`
        const defaultRegex = /==([\s\S]*?)==/g;
        
        html = html.replace(defaultRegex, function(match, content) {
          hasChanged = true;
          return `<mark class="memos-hl mhl-yellow">${content}</mark>`;
        });
      }

      // 💾 锁定写入 DOM 状态，彻底防止被 Vue 刷掉
      if (hasChanged) {
        block.innerHTML = html;
        block.setAttribute('data-v-custom-html', html);
      }
    });
  }

  // 深度监听动态生成的卡片流、详情页以及文字节点重绘
  const observer = new MutationObserver(() => {
    doMemosMagic();
  });

  observer.observe(document.body, {
    childList: true,
    subtree: true,
    characterData: true
  });

  // 首屏主动触发
  doMemosMagic();
})();
```

---

## 额外的编辑工具栏

原版 Memos 输入框只有一个添加按钮，没有任何 Markdown 辅助快捷输入工具，上面提到的那个 Quill 富文本编辑器保存的时候不会以 Markdown 形式转换，等于没用。三下五除二自己搞了一个，并且把高亮和刮刮卡加进去了，会根据输入框与屏幕大小自动收纳按钮。

![QQ_1779975420950.png](https://resv2.craft.do/user/full/d8d9f551-27be-8f86-44b5-701ada6bb4d1/doc/2d6328bb-4a8d-4194-b8cb-a452fe35f1e7/2854591a-c55b-4228-8c26-cddeb2690cd7)

**JS 脚本：添加工具栏**

```javascript
(function() {
    'use strict';

    // 1. 定义 SVG 图标和工具配置 (使用拼接避免 Markdown 语法截断冲突)
    const tripleBackticks = '`' + '`' + '`';

    const tools = [
        { 
            label: '标题',
            title: '标题 (Heading)', 
            isDropdown: true,
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M6 12h12M6 20V4M18 20V4"></path></svg>`,
            dropdownItems: [
                { 
                    label: 'H1', 
                    before: '# ', 
                    after: '', 
                    title: '一级标题',
                    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 12h8M4 18V6M12 18V6M17 12l2-2v8"/></svg>`
                },
                { 
                    label: 'H2', 
                    before: '## ', 
                    after: '', 
                    title: '二级标题',
                    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 12h8M4 18V6M12 18V6M21 18h-4a2 2 0 0 1 2-2 2 2 0 0 0-2-2"/></svg>`
                },
                { 
                    label: 'H3', 
                    before: '### ', 
                    after: '', 
                    title: '三级标题',
                    icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 12h8M4 18V6M12 18V6M17 10h3a2 2 0 0 1-2 2 2 2 0 0 1 2 2h-3"/></svg>`
                }
            ]
        },
        { 
            label: '加粗',
            title: '加粗 (Bold)', 
            before: '**', 
            after: '**',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M6 4h8a4 4 0 0 1 4 4 4 4 0 0 1-4 4H6z"></path><path d="M6 12h9a4 4 0 0 1 4 4 4 4 0 0 1-4 4H6z"></path></svg>`
        },
        { 
            label: '斜体',
            title: '斜体 (Italic)', 
            before: '*', 
            after: '*',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="19" y1="4" x2="10" y2="4"></line><line x1="14" y1="20" x2="5" y2="20"></line><line x1="15" y1="4" x2="9" y2="20"></line></svg>`
        },
        { 
            label: '高亮',
            title: '高亮 (Highlight)', 
            isDropdown: true,
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 11-6 6v3h3l6-6M9 11l3-3 3 3-3 3M14 6l4 4"/></svg>`,
            dropdownItems: [
                { label: '黄色 (默认)', before: '==', after: '==', title: '黄色高亮 (Default)', color: '#fef08a' },
                { label: '红色', before: '==[&red]', after: '==', title: '红色高亮 (Red)', color: '#ef4444' },
                { label: '蓝色', before: '==[&blue]', after: '==', title: '蓝色高亮 (Blue)', color: '#3b82f6' },
                { label: '青色', before: '==[&teal]', after: '==', title: '青色高亮 (Teal)', color: '#0d9488' },
                { label: '灰色', before: '==[&grey]', after: '==', title: '灰色高亮 (Grey)', color: '#6b7280' },
                { label: '紫色', before: '==[&purple]', after: '==', title: '紫色高亮 (Purple)', color: '#a855f7' },
                { label: '粉色', before: '==[&pink]', after: '==', title: '粉色高亮 (Pink)', color: '#ec4899' },
                { label: '绿色', before: '==[&green]', after: '==', title: '绿色高亮 (Green)', color: '#22c55e' },
                { label: '湖蓝', before: '==[&turquoise]', after: '==', title: '绿松石高亮 (Turquoise)', color: '#06b6d4' }
            ]
        },
        { 
            label: '刮刮卡',
            title: '刮刮卡 (Spoiler)', 
            before: '==[&hide]', 
            after: '==',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path><line x1="1" y1="1" x2="23" y2="23"></line></svg>`
        },
        { 
            label: '删除线',
            title: '删除线 (Strikethrough)', 
            before: '~~', 
            after: '~~',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="5" y1="12" x2="19" y2="12"></line><path d="M16 6C16 6 14.5 4 12 4C9.5 4 8 6 8 8C8 10 9 11 11 11.5L13 12.5C15 13 16 14 16 16C16 18 14.5 20 12 20C9.5 20 8 18 8 18"></path></svg>`
        },
        { 
            label: '标签',
            title: '标签 (Tag)', 
            before: '#', 
            after: ' ',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="4" y1="9" x2="20" y2="9"></line><line x1="4" y1="15" x2="20" y2="15"></line><line x1="10" y1="3" x2="8" y2="21"></line><line x1="16" y1="3" x2="14" y2="21"></line></svg>`
        },
        { 
            label: '行内代码',
            title: '行内代码 (Inline Code)', 
            before: '`', 
            after: '`',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="16 18 22 12 16 6"></polyline><polyline points="8 6 2 12 8 18"></polyline></svg>`
        },
        { 
            label: '代码块',
            title: '代码块 (Code Block)', 
            before: tripleBackticks + '\n', 
            after: '\n' + tripleBackticks,
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2" ry="2"></rect><line x1="6" y1="10" x2="10" y2="10"></line><line x1="6" y1="14" x2="18" y2="14"></line></svg>`
        },
        { 
            label: '无序清单',
            title: '无序清单 (Unordered List)', 
            before: '- ', 
            after: '',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="9" y1="6" x2="20" y2="6"></line><line x1="9" y1="12" x2="20" y2="12"></line><line x1="9" y1="18" x2="20" y2="18"></line><circle cx="5" cy="6" r="1" fill="currentColor"></circle><circle cx="5" cy="12" r="1" fill="currentColor"></circle><circle cx="5" cy="18" r="1" fill="currentColor"></circle></svg>`
        },
        { 
            label: '有序清单',
            title: '有序清单 (Ordered List)', 
            before: '1. ', 
            after: '',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="10" y1="6" x2="21" y2="6"></line><line x1="10" y1="12" x2="21" y2="12"></line><line x1="10" y1="18" x2="21" y2="18"></line><path d="M4 6h1v4M4 10h2M6 18H4c0-1 2-2 2-3s-1-1.5-2-1"></path></svg>`
        },
        { 
            label: '待办事项',
            title: '待办事项 (Task List)', 
            before: '- [ ] ', 
            after: '',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect><polyline points="9 11 12 14 17 8"></polyline></svg>`
        },
        { 
            label: '链接',
            title: '插入链接 (Link)', 
            before: '[', 
            after: '](url)',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg>`
        },
        { 
            label: '引用',
            title: '引用块 (Quote)', 
            before: '> ', 
            after: '',
            icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path></svg>`
        }
    ];

    // 2. 注入全局 CSS 样式（连体分段控制 & 高自适应主题下拉气泡）
    const style = document.createElement('style');
    style.innerHTML = `
        /* 连体外部主容器 */
        .custom-md-toolbar {
            display: inline-flex !important;
            align-items: center;
            border: 1px solid var(--border, rgba(128, 128, 128, 0.2)) !important;
            border-radius: 6px !important;
            margin-left: 8px !important;
            height: 32px !important;
            background-color: var(--bg-paper, rgba(128, 128, 128, 0.03)) !important;
            overflow: visible !important;
        }

        /* 基础按钮：当处于工具栏直属下时的样式 */
        .custom-md-toolbar > .custom-md-btn,
        .custom-md-toolbar > .custom-md-dropdown-container > .custom-md-btn {
            width: 30px !important;
            height: 30px !important;
            display: flex !important;
            align-items: center !important;
            justify-content: center !important;
            border: none !important;
            background: transparent !important;
            color: currentColor !important;
            cursor: pointer !important;
            opacity: 0.75 !important;
            transition: all 0.15s ease-in-out !important;
            border-right: 1px solid var(--border, rgba(128, 128, 128, 0.15)) !important;
            padding: 0 !important;
        }
        
        /* 工具栏直接子项隐藏文本 */
        .custom-md-toolbar .custom-btn-text-label {
            display: none !important;
        }

        /* 悬停与激活 */
        .custom-md-btn:hover {
            opacity: 1 !important;
            background-color: rgba(128, 128, 128, 0.12) !important;
        }
        .custom-md-btn:active {
            background-color: rgba(128, 128, 128, 0.2) !important;
        }
        .custom-md-btn svg {
            width: 15px !important;
            height: 15px !important;
        }

        /* 下拉容器的基础定位 */
        .custom-md-dropdown-container {
            position: relative !important;
            display: inline-flex !important;
            align-items: center !important;
        }

        /* 通用下拉气泡菜单样式 (由 hidden 改造为 visible 从而完美外溢渲染子菜单) */
        .custom-md-dropdown-menu {
            display: none;
            flex-direction: column;
            position: absolute !important;
            background-color: var(--bg-paper, #ffffff) !important;
            color: var(--text-main, #374151) !important;
            border: 1px solid var(--border, rgba(128, 128, 128, 0.25)) !important;
            border-radius: 6px !important;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15) !important;
            z-index: 9999 !important;
            min-width: 110px !important;
            overflow: visible; /* 必须为 visible 以免裁切折叠菜单中的二级子菜单 */
        }

        /* 极简圆角强制覆盖 (由于取消了 overflow: hidden, 必须手动拟合首尾子项的圆角) */
        .custom-md-dropdown-menu > *:first-child,
        .custom-md-dropdown-menu > .custom-md-dropdown-container:first-child > .custom-md-btn {
            border-top-left-radius: 5px !important;
            border-top-right-radius: 5px !important;
        }
        .custom-md-dropdown-menu > *:last-child,
        .custom-md-dropdown-menu > .custom-md-dropdown-container:last-child > .custom-md-btn {
            border-bottom-left-radius: 5px !important;
            border-bottom-right-radius: 5px !important;
            border-bottom: none !important;
        }
        .custom-md-dropdown-item:first-child {
            border-top-left-radius: 5px !important;
            border-top-right-radius: 5px !important;
        }
        .custom-md-dropdown-item:last-child {
            border-bottom-left-radius: 5px !important;
            border-bottom-right-radius: 5px !important;
        }

        /* 气泡菜单内部按钮项样式 */
        .custom-md-dropdown-item {
            padding: 6px 14px !important;
            font-size: 12px !important;
            font-weight: 500 !important;
            display: flex !important;
            align-items: center !important;
            gap: 8px !important;
            cursor: pointer !important;
            background: transparent !important;
            border: none !important;
            color: currentColor !important;
            width: 100% !important;
            text-align: left !important;
            white-space: nowrap !important;
            transition: background 0.15s !important;
        }
        .custom-md-dropdown-item:hover {
            background-color: rgba(128, 128, 128, 0.12) !important;
        }
        .custom-md-dropdown-item svg {
            width: 14px !important;
            height: 14px !important;
            flex-shrink: 0 !important;
        }

        /* 【核心转换】当普通的工具按钮被移动到“更多”下拉菜单时，CSS 规则无缝切换 */
        .custom-md-more-dropdown-menu > .custom-md-btn,
        .custom-md-more-dropdown-menu > .custom-md-dropdown-container > .custom-md-btn {
            width: 100% !important;
            height: auto !important;
            padding: 7px 14px !important;
            display: flex !important;
            align-items: center !important;
            justify-content: flex-start !important;
            gap: 8px !important;
            border: none !important;
            border-right: none !important;
            border-radius: 0 !important;
            background: transparent !important;
            color: currentColor !important;
            text-align: left !important;
            opacity: 0.85 !important;
            border-bottom: 1px solid var(--border, rgba(128, 128, 128, 0.1)) !important;
        }
        .custom-md-more-dropdown-menu > .custom-md-btn:last-child,
        .custom-md-more-dropdown-menu > .custom-md-dropdown-container:last-child > .custom-md-btn {
            border-bottom: none !important;
        }
        .custom-md-more-dropdown-menu > .custom-md-btn:hover,
        .custom-md-more-dropdown-menu > .custom-md-dropdown-container:hover > .custom-md-btn {
            opacity: 1 !important;
            background-color: rgba(128, 128, 128, 0.12) !important;
        }
        .custom-md-more-dropdown-menu .custom-btn-text-label {
            display: inline !important;
            font-size: 12px !important;
            font-weight: 500 !important;
            white-space: nowrap !important;
        }
        .custom-md-more-dropdown-menu svg {
            width: 14px !important;
            height: 14px !important;
            flex-shrink: 0 !important;
        }

        /* 嵌套子菜单横向溢出定位调整 (移除 left/right 的写死规则，交由 JS 精准判定) */
        .custom-md-more-dropdown-menu .custom-md-dropdown-menu {
            transform: none !important;
        }

        /* 原生暗色主题深度融合兼容 */
        @media (prefers-color-scheme: dark) {
            .custom-md-dropdown-menu {
                background-color: #1e1e20 !important;
                color: #e5e7eb !important;
                border-color: rgba(255, 255, 255, 0.12) !important;
                box-shadow: 0 4px 16px rgba(0, 0, 0, 0.45) !important;
            }
        }
        html.dark .custom-md-dropdown-menu,
        body.dark .custom-md-dropdown-menu,
        [data-theme="dark"] .custom-md-dropdown-menu,
        .dark .custom-md-dropdown-menu {
            background-color: #1e1e20 !important;
            color: #e5e7eb !important;
            border-color: rgba(255, 255, 255, 0.12) !important;
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.45) !important;
        }
    `;
    document.head.appendChild(style);

    // 3. 边界自适应碰撞定位算法 (具有极致空间判定和最大高度限制滚动fallback)
    function positionDropdown(menu, button) {
        menu.style.display = 'flex'; // 先显示以便获得高宽
        
        // 每次重新定位前重置可能发生变化的高度和滚动限制
        menu.style.maxHeight = 'none';
        menu.style.overflowY = 'visible';

        const buttonRect = button.getBoundingClientRect();
        const menuHeight = menu.offsetHeight || 150;
        const menuWidth = menu.offsetWidth || 120;
        const viewportWidth = window.innerWidth;
        const viewportHeight = window.innerHeight;

        // 重置旧定位
        menu.style.bottom = 'auto';
        menu.style.top = 'auto';
        menu.style.left = 'auto';
        menu.style.right = 'auto';
        menu.style.transform = 'none';
        menu.style.marginLeft = '0px';
        menu.style.marginRight = '0px';

        const spacing = 6;
        const spaceAbove = buttonRect.top - spacing;
        const spaceBelow = viewportHeight - buttonRect.bottom - spacing;

        const isNested = !!button.closest('.custom-md-more-dropdown-menu');

        if (isNested) {
            // 【嵌套子菜单横向溢出检测与避让】
            const spaceRight = viewportWidth - buttonRect.right;
            const spaceLeft = buttonRect.left;
            
            // 如果右侧放得下，或者右侧空间比左侧大，则优先向右展开；否则向左展开
            if (spaceRight >= menuWidth || spaceRight > spaceLeft) {
                menu.style.left = '100%';
                menu.style.right = 'auto';
                menu.style.marginLeft = '4px';
            } else {
                menu.style.right = '100%';
                menu.style.left = 'auto';
                menu.style.marginRight = '4px';
            }

            // 【嵌套子菜单纵向对齐与滚动避让】
            if (viewportHeight - buttonRect.top >= menuHeight) {
                menu.style.top = '0px';
                menu.style.bottom = 'auto';
            } else {
                // 如果下方放不下，向上对齐，并检测上方空间是否足够，不够则开启滚动限流
                if (buttonRect.bottom >= menuHeight) {
                    menu.style.bottom = '0px';
                    menu.style.top = 'auto';
                } else {
                    // 上下都放不下，选择空间较大的一侧限高并启用滚动条
                    if (spaceBelow > spaceAbove) {
                        menu.style.top = '0px';
                        menu.style.bottom = 'auto';
                        menu.style.maxHeight = `${Math.max(80, spaceBelow - 10)}px`;
                        menu.style.overflowY = 'auto';
                    } else {
                        menu.style.bottom = '0px';
                        menu.style.top = 'auto';
                        menu.style.maxHeight = `${Math.max(80, spaceAbove - 10)}px`;
                        menu.style.overflowY = 'auto';
                    }
                }
            }
        } else {
            // 【非嵌套常规主菜单定位】
            // 垂直定位：优先向上弹出，若上方空间不足则向下
            if (spaceAbove >= menuHeight) {
                menu.style.bottom = `${button.offsetHeight + spacing}px`;
            } else if (spaceBelow >= menuHeight) {
                menu.style.top = `${button.offsetHeight + spacing}px`;
            } else {
                // 两边均放不下 (移动端软键盘弹出)，选择空余空间更大的一侧并施加 maxHeight 滚动条限制
                if (spaceAbove >= spaceBelow) {
                    menu.style.bottom = `${button.offsetHeight + spacing}px`;
                    menu.style.maxHeight = `${Math.max(60, spaceAbove - 10)}px`;
                    menu.style.overflowY = 'auto';
                } else {
                    menu.style.top = `${button.offsetHeight + spacing}px`;
                    menu.style.maxHeight = `${Math.max(60, spaceBelow - 10)}px`;
                    menu.style.overflowY = 'auto';
                }
            }

            // 水平定位：防止左右两侧溢出屏幕被裁切
            const buttonCenter = buttonRect.left + buttonRect.width / 2;
            if (buttonCenter - menuWidth / 2 < 12) {
                menu.style.left = '0px';
            } else if (buttonCenter + menuWidth / 2 > viewportWidth - 12) {
                menu.style.right = '0px';
            } else {
                menu.style.left = '50%';
                menu.style.transform = 'translateX(-50%)';
            }
        }
    }

    // 4. 核心插入逻辑 (React 状态双向更新绕过)
    function insertMarkdown(anchorButton, before, after) {
        let parent = anchorButton.parentElement;
        let textarea = null;
        while (parent && parent !== document.body) {
            textarea = parent.querySelector('textarea');
            if (textarea) break;
            parent = parent.parentElement;
        }
        if (!textarea) textarea = document.querySelector('textarea');
        if (!textarea) return;

        textarea.focus();
        const start = textarea.selectionStart;
        const end = textarea.selectionEnd;
        const text = textarea.value;
        const selectedText = text.substring(start, end);

        let replacement = '';
        const isBlock = (before === '> ' || before === '- ' || before === '1. ' || before === '- [ ] ' || before === '# ' || before === '## ' || before === '### ');

        if (before === '#' && after === ' ') {
            replacement = selectedText.length > 0 ? '#' + selectedText + ' ' : '#';
        } else if (isBlock) {
            replacement = selectedText.length > 0 ? selectedText.split('\n').map(line => before + line).join('\n') : before;
        } else {
            replacement = before + selectedText + after;
        }

        const newValue = text.substring(0, start) + replacement + text.substring(end);

        const nativeInputValueSetter = Object.getOwnPropertyDescriptor(window.HTMLTextAreaElement.prototype, "value").set;
        if (nativeInputValueSetter) {
            nativeInputValueSetter.call(textarea, newValue);
        } else {
            textarea.value = newValue;
        }

        textarea.dispatchEvent(new Event('input', { bubbles: true }));
        textarea.dispatchEvent(new Event('change', { bubbles: true }));

        textarea.focus();
        if (start === end) {
            if (before === '#' && replacement === '#') {
                textarea.setSelectionRange(start + 1, start + 1);
            } else {
                const newPos = start + before.length;
                textarea.setSelectionRange(newPos, newPos);
            }
        } else {
            const newPos = start + replacement.length;
            textarea.setSelectionRange(newPos, newPos);
        }
    }

    // 5. 判断“加号”按钮是否真的属于一个正在编辑的输入框 (向上攀爬 DOM 树最多 5 层检查是否存在 textarea)
    function isEditorPlusButton(btn) {
        let parent = btn.parentElement;
        for (let i = 0; i < 5; i++) {
            if (!parent || parent === document.body) break;
            if (parent.querySelector('textarea')) {
                return true;
            }
            parent = parent.parentElement;
        }
        return false;
    }

    // 6. 全自适应弹性流式重排算法 (根据空余宽度逐个吞吐按钮)
    function fitButtons(toolbar, plusButton) {
        const parentContainer = plusButton.parentElement;
        if (!parentContainer) return;

        // 向上寻找承载左侧（+按钮与工具栏）和右侧（可见性与保存）的最外层主双栏容器
        const rowContainer = plusButton.closest('.justify-between') || 
                             (plusButton.parentElement ? plusButton.parentElement.parentElement : null) || 
                             parentContainer;
        if (!rowContainer) return;

        const rowWidth = rowContainer.getBoundingClientRect().width;
        if (rowWidth <= 0) return; // 容器尚未渲染好时直接拦截

        // 测算右侧控制区（包含“私有”和“保存”等大按钮组）的物理占用宽度
        const rightContainer = rowContainer.querySelector('.justify-end') || 
                               (rowContainer.lastElementChild !== plusButton.parentElement ? rowContainer.lastElementChild : null);
        const rightWidth = rightContainer ? rightContainer.getBoundingClientRect().width : 120; // 默认给 120px 容纳“私有+保存”

        const plusButtonWidth = plusButton.getBoundingClientRect().width || 32;

        // 【流式核心计算】计算真正空闲的可分配区域：总宽 - 左侧加号宽 - 右侧控制组宽 - 安全间距(24px)
        const safetyPadding = 24; 
        const availableWidth = rowWidth - plusButtonWidth - rightWidth - safetyPadding;

        const allElements = toolbar._allElements || [];
        const moreContainer = toolbar.querySelector('.custom-md-more-container');
        const moreDropdown = toolbar.querySelector('.custom-md-more-dropdown-menu');

        const btnWidth = 31; // 每个连体卡槽子按钮占据 31 像素
        const moreBtnWidth = 31;
        const totalCount = allElements.length;

        // 清空重置展开气泡
        moreDropdown.innerHTML = '';

        // 2. 根据空余可用宽度计算能够直接塞下的按钮上限
        if (availableWidth >= totalCount * btnWidth) {
            // 空间极度充裕：完全平铺，完美隐藏【更多】，通过 inline !important 强行覆盖样式表属性
            moreContainer.style.setProperty('display', 'none', 'important');
            allElements.forEach(element => {
                toolbar.insertBefore(element, moreContainer);
            });
        } else {
            // 空间告急：展开折叠重排，通过 inline !important 强行激活显示
            moreContainer.style.setProperty('display', 'inline-flex', 'important');

            // 扣除【更多】按钮自身的占位，计算可装下的按钮数
            let fitCount = Math.floor((availableWidth - moreBtnWidth) / btnWidth);
            // 强行约束：最少必须平铺展现 1 个（标题），最多保留 totalCount-1 个
            fitCount = Math.max(1, Math.min(totalCount - 1, fitCount));

            allElements.forEach((element, idx) => {
                if (idx < fitCount) {
                    toolbar.insertBefore(element, moreContainer);
                } else {
                    moreDropdown.appendChild(element);
                }
            });
        }

        // 3. 【极光圆角同步更新机制】动态给当前可见的第一和最末按钮涂刷精致的圆角
        allElements.forEach(el => {
            const innerBtn = el.querySelector('.custom-md-btn') || el;
            innerBtn.style.borderRight = '';
            innerBtn.style.borderTopLeftRadius = '';
            innerBtn.style.borderBottomLeftRadius = '';
            innerBtn.style.borderTopRightRadius = '';
            innerBtn.style.borderBottomRightRadius = '';
        });
        
        moreContainer.querySelector('.custom-md-btn').style.borderRight = 'none';
        moreContainer.querySelector('.custom-md-btn').style.borderTopRightRadius = '';
        moreContainer.querySelector('.custom-md-btn').style.borderBottomRightRadius = '';

        // 测算更多按钮当前真实的 inline 状态，以防逻辑冲突
        const isMoreVisible = moreContainer.style.getPropertyValue('display') !== 'none';
        const visibleChildren = Array.from(toolbar.children).filter(child => {
            if (child === moreContainer) return isMoreVisible;
            return child.style.display !== 'none';
        });

        if (visibleChildren.length > 0) {
            const first = visibleChildren[0];
            const last = visibleChildren[visibleChildren.length - 1];

            const firstBtn = first.querySelector('.custom-md-btn') || first;
            firstBtn.style.borderTopLeftRadius = '5px';
            firstBtn.style.borderBottomLeftRadius = '5px';

            const lastBtn = last.querySelector('.custom-md-btn') || last;
            lastBtn.style.borderTopRightRadius = '5px';
            lastBtn.style.borderBottomRightRadius = '5px';
            lastBtn.style.borderRight = 'none'; // 最右侧不留垂直线
        }
    }

    // 7. 构造、装配工具栏 DOM
    function injectToolbar() {
        const allButtons = document.querySelectorAll('button');
        const plusButtons = Array.from(allButtons).filter(btn => {
            // 首先判断是否具备加号图标/特征
            const hasPlusIcon = btn.querySelector('.lucide-plus') || 
                                btn.querySelector('[class*="lucide-plus"]') ||
                                (btn.id && btn.id.startsWith('radix-') && btn.innerHTML.includes('path d="M12 5v14"') && btn.innerHTML.includes('path d="M5 12h14"'));
            
            // 接着判断该按钮是否属于一个包含输入文本框的编辑器（防误伤核心规则）
            return hasPlusIcon && isEditorPlusButton(btn);
        });
        
        plusButtons.forEach(plusButton => {
            const parentContainer = plusButton.parentElement;
            if (!parentContainer) return;

            // 避免重复注入
            if (parentContainer.querySelector('.custom-md-toolbar')) return;

            const toolbar = document.createElement('div');
            toolbar.className = 'custom-md-toolbar';

            // 提前构造「更多 (More...)」连体触发器
            const moreContainer = document.createElement('div');
            moreContainer.className = 'custom-md-dropdown-container custom-md-more-container';

            const moreBtn = document.createElement('button');
            moreBtn.type = 'button';
            moreBtn.className = 'custom-md-btn';
            moreBtn.title = '更多排版格式';
            moreBtn.innerHTML = `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="1.5"></circle><circle cx="19" cy="12" r="1.5"></circle><circle cx="5" cy="12" r="1.5"></circle></svg>`;

            const moreDropdown = document.createElement('div');
            moreDropdown.className = 'custom-md-dropdown-menu custom-md-more-dropdown-menu';

            moreBtn.addEventListener('click', (e) => {
                e.preventDefault();
                e.stopPropagation();

                document.querySelectorAll('.custom-md-dropdown-menu').forEach(m => {
                    if (m !== moreDropdown) m.style.display = 'none';
                });

                const isShown = (moreDropdown.style.display === 'flex');
                if (!isShown) {
                    positionDropdown(moreDropdown, moreBtn);
                } else {
                    moreDropdown.style.display = 'none';
                }
            });

            moreContainer.appendChild(moreBtn);
            moreContainer.appendChild(moreDropdown);

            const allElements = [];

            // 循环遍历定义工具元素
            tools.forEach((tool) => {
                let element;
                if (tool.isDropdown) {
                    // 构造标题/高亮下拉组件
                    const container = document.createElement('div');
                    container.className = 'custom-md-dropdown-container';

                    const mainBtn = document.createElement('button');
                    mainBtn.type = 'button';
                    mainBtn.className = 'custom-md-btn';
                    // 为Dropdown主按钮追加文本标签，使其在被折叠入“更多”时拥有完美的文字注释说明
                    mainBtn.innerHTML = `${tool.icon} <span class="custom-btn-text-label">${tool.label}</span>`;
                    mainBtn.title = tool.title;

                    const menu = document.createElement('div');
                    menu.className = 'custom-md-dropdown-menu';

                    // 填充子图标和事件 (支持 Icon 或 色块圆点)
                    tool.dropdownItems.forEach(item => {
                        const itemBtn = document.createElement('button');
                        itemBtn.type = 'button';
                        itemBtn.className = 'custom-md-dropdown-item';
                        
                        // 动态构建子菜单的前置视觉元素 (色块或矢量图标)
                        let innerHTML = '';
                        if (item.icon) {
                            innerHTML += item.icon;
                        } else if (item.color) {
                            innerHTML += `<span style="display:inline-block; width:12px; height:12px; border-radius:50%; background-color:${item.color}; border:1px solid rgba(128,128,128,0.3); flex-shrink:0;"></span>`;
                        }
                        innerHTML += `<span style="font-size:11px">${item.label}</span>`;
                        
                        itemBtn.innerHTML = innerHTML;
                        itemBtn.title = item.title;

                        itemBtn.addEventListener('click', (e) => {
                            e.preventDefault();
                            e.stopPropagation();
                            insertMarkdown(plusButton, item.before, item.after);
                            
                            // 关闭子菜单
                            menu.style.display = 'none';

                            // 如果处于折叠区，点击插入后自动连带闭合外层的「更多」下拉菜单，提升移动端用户体验
                            const parentMore = itemBtn.closest('.custom-md-more-dropdown-menu');
                            if (parentMore) {
                                parentMore.style.display = 'none';
                            }
                        });
                        menu.appendChild(itemBtn);
                    });

                    mainBtn.addEventListener('click', (e) => {
                        e.preventDefault();
                        e.stopPropagation();
                        
                        document.querySelectorAll('.custom-md-dropdown-menu').forEach(m => {
                            if (m !== menu && m !== moreDropdown) m.style.display = 'none';
                        });

                        const isShown = (menu.style.display === 'flex');
                        if (!isShown) {
                            positionDropdown(menu, mainBtn);
                        } else {
                            menu.style.display = 'none';
                        }
                    });

                    container.appendChild(mainBtn);
                    container.appendChild(menu);
                    element = container;
                } else {
                    // 构造标准排版按钮
                    const btn = document.createElement('button');
                    btn.type = 'button';
                    btn.className = 'custom-md-btn';
                    btn.innerHTML = `${tool.icon} <span class="custom-btn-text-label">${tool.label}</span>`;
                    btn.title = tool.title;

                    btn.addEventListener('click', (e) => {
                        e.preventDefault();
                        e.stopPropagation();
                        insertMarkdown(plusButton, tool.before, tool.after);
                        
                        const moreMenu = btn.closest('.custom-md-more-dropdown-menu');
                        if (moreMenu) moreMenu.style.display = 'none';
                    });

                    element = btn;
                }

                allElements.push(element);
            });

            // 缓存全部节点指针以备高响应式重排
            toolbar._allElements = allElements;

            // 将收纳箱追加入主栏最末端
            toolbar.appendChild(moreContainer);

            // 【核心先挂载】
            plusButton.after(toolbar);

            // 寻找更广的主弹性包裹行
            const rowContainer = plusButton.closest('.justify-between') || 
                                 (plusButton.parentElement ? plusButton.parentElement.parentElement : null) || 
                                 parentContainer;

            // 绑定 ResizeObserver 实时自适应（如果不支持，降级绑定 window resize 监听）
            if (window.ResizeObserver) {
                const observer = new ResizeObserver(() => {
                    fitButtons(toolbar, plusButton);
                });
                observer.observe(rowContainer); // 监听最外层主双栏容器的尺寸变化
                toolbar._resizeObserver = observer;
            } else {
                const handleResize = () => {
                    if (!document.body.contains(toolbar)) {
                        window.removeEventListener('resize', handleResize);
                        return;
                    }
                    fitButtons(toolbar, plusButton);
                };
                window.addEventListener('resize', handleResize);
            }

            // 初始化首次渲染排布
            fitButtons(toolbar, plusButton);
        });
    }

    // 点击空白处，自动关闭任何已展开的下拉气泡
    document.addEventListener('click', (e) => {
        document.querySelectorAll('.custom-md-dropdown-menu').forEach(menu => {
            if (!menu.parentElement.contains(e.target)) {
                menu.style.display = 'none';
            }
        });
    });

    // 500ms 异步检测轮询
    setInterval(injectToolbar, 500);
})();
```

