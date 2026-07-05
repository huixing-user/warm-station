# 华仔の温暖小站 实现计划

> **For agentic workers:** 使用 superpowers:subagent-driven-development 来逐任务实现。Steps 使用 checkbox (`- [ ]`) 语法追踪。

**目标:** 构建一个温馨的单 HTML 页面手机网站，包含木鱼敲击、幸运大转盘、照片墙、暖心话语和隐藏管理面板。

**架构:** 单 HTML 文件内嵌所有 CSS/JS，配合 `images/` 目录下的外部图片。使用 localStorage 持久化数据，Web Audio API 合成木鱼音效，Canvas 绘制转盘，纯 CSS 动画和无外部依赖的设计。

**技术栈:** HTML5 + CSS3 + Vanilla JS (ES6+)，Canvas API，Web Audio API，localStorage（无框架/CDN）

## 全局约束

- 单 HTML 文件，CSS/JS 全部内嵌
- 无外部依赖（CDN/图标库/框架）
- 响应式，最大宽度 480px 居中
- 所有 localStorage key 前缀 `warm_`
- 密码: `HhxloveCwh1314!`（硬编码在 JS 中）
- 图片放 `images/` 子目录，需压缩至单张 < 300KB
- 支持手机浏览器打开，支持微信内置浏览器
- 所有文案中"华仔"为男朋友代称

---

## 文件结构

```
f:/风水师/warm-station/
├── index.html              # 主页面（所有代码）
├── images/
│   ├── photos/             # 用户 9 张照片
│   ├── flowers/            # 郁金香图片
│   └── dogs/               # 小狗图片
└── README.txt              # 使用说明
```

### 模块职责

**index.html** — 唯一产品文件，按以下 CSS class 分区：
- `.hero` — 顶部标题区
- `.muyu-section` — 木鱼敲击区
- `.wheel-section` — 幸运大转盘
- `.photo-section` — 照片墙
- `.quote-section` — 暖心话语
- `.admin-panel` — 隐藏管理面板
- `.modal-overlay` / `.result-modal` — 弹窗

### 数据流

```
用户操作 → JS事件处理 → localStorage读写 → DOM更新
                              ↓
                    管理面板读取/展示数据
```

---

### Task 1: 复制照片并创建项目基础结构

**文件:**
- 复制: `C:\Users\Redamancy\Desktop\图片\*.jpg` → `f:/风水师/warm-station/images/photos/`
- 创建: `f:/风水师/warm-station/README.txt`

**步骤:**

- [ ] **Step 1: 复制用户照片到项目目录**

```bash
cp "C:\Users\Redamancy\Desktop\图片\*.jpg" "f:/风水师/warm-station/images/photos/"
```

- [ ] **Step 2: 创建 README.txt**

```text
华仔の温暖小站 🌸

使用方法：
1. 把整个 warm-station 文件夹复制到手机
2. 用手机浏览器打开 index.html
3. 或者用微信发给自己，点开即可

文件夹说明：
- index.html：网站主页
- images/photos/：你们的照片
- images/flowers/：郁金香图片
- images/dogs/：小狗图片

数据说明：
- 木鱼金额和转盘记录保存在手机浏览器中
- 清除浏览器缓存会导致数据丢失
- 换手机数据不会同步

祝你女朋友开心！💕
```

---

### Task 2: 下载/准备花卉和小狗图片

**文件:**
- 创建: `f:/风水师/warm-station/images/flowers/tulip1.svg` (CSS 手绘郁金香 SVG)
- 创建: `f:/风水师/warm-station/images/flowers/tulip2.svg` (CSS 手绘郁金香 SVG)
- 创建: `f:/风水师/warm-station/images/flowers/tulip3.svg` (CSS 手绘郁金香 SVG)
- 创建: `f:/风水师/warm-station/images/dogs/golden.svg` (金毛小狗 SVG)
- 创建: `f:/风水师/warm-station/images/dogs/westie.svg` (西高地小狗 SVG)

**步骤:**

- [ ] **Step 1: 创建郁金香 SVG (tulip1.svg)**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 120 160">
  <!-- 花茎 -->
  <line x1="60" y1="60" x2="60" y2="150" stroke="#5B8C3E" stroke-width="4" stroke-linecap="round"/>
  <!-- 左叶 -->
  <ellipse cx="42" cy="100" rx="18" ry="6" fill="#7CB342" transform="rotate(-30 42 100)"/>
  <!-- 右叶 -->
  <ellipse cx="78" cy="110" rx="18" ry="6" fill="#7CB342" transform="rotate(25 78 110)"/>
  <!-- 花瓣 -->
  <ellipse cx="48" cy="42" rx="14" ry="28" fill="#FF8BA7" transform="rotate(-15 48 42)"/>
  <ellipse cx="72" cy="42" rx="14" ry="28" fill="#FF8BA7" transform="rotate(15 72 42)"/>
  <ellipse cx="60" cy="38" rx="14" ry="28" fill="#FFB3C6"/>
  <!-- 花心 -->
  <circle cx="60" cy="46" r="4" fill="#FFD166"/>
</svg>
```

- [ ] **Step 2: 创建郁金香 SVG (tulip2.svg)**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 120 160">
  <line x1="60" y1="55" x2="60" y2="150" stroke="#4A7C3E" stroke-width="4" stroke-linecap="round"/>
  <ellipse cx="40" cy="95" rx="16" ry="5" fill="#6BAF3E" transform="rotate(-25 40 95)"/>
  <ellipse cx="80" cy="105" rx="16" ry="5" fill="#6BAF3E" transform="rotate(20 80 105)"/>
  <ellipse cx="46" cy="38" rx="13" ry="26" fill="#FF6B8A" transform="rotate(-12 46 38)"/>
  <ellipse cx="74" cy="38" rx="13" ry="26" fill="#FF6B8A" transform="rotate(12 74 38)"/>
  <ellipse cx="60" cy="34" rx="13" ry="26" fill="#FFA0B4"/>
  <circle cx="58" cy="42" r="4" fill="#FFE066"/>
</svg>
```

- [ ] **Step 3: 创建郁金香 SVG (tulip3.svg)**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 120 160">
  <line x1="60" y1="55" x2="60" y2="150" stroke="#5B8C3E" stroke-width="4" stroke-linecap="round"/>
  <ellipse cx="38" cy="90" rx="18" ry="6" fill="#8BC34A" transform="rotate(-35 38 90)"/>
  <ellipse cx="82" cy="105" rx="18" ry="6" fill="#8BC34A" transform="rotate(30 82 105)"/>
  <ellipse cx="45" cy="40" rx="13" ry="27" fill="#FF7B9C" transform="rotate(-18 45 40)"/>
  <ellipse cx="75" cy="40" rx="13" ry="27" fill="#FF7B9C" transform="rotate(18 75 40)"/>
  <ellipse cx="60" cy="36" rx="13" ry="27" fill="#FFAEC9"/>
  <circle cx="62" cy="44" r="4" fill="#FFD166"/>
</svg>
```

- [ ] **Step 4: 创建金毛小狗 SVG (golden.svg)**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 140 140">
  <!-- 身体 -->
  <ellipse cx="70" cy="95" rx="40" ry="30" fill="#E8B46F"/>
  <!-- 头 -->
  <circle cx="70" cy="55" r="32" fill="#F0C987"/>
  <!-- 耳朵 (左) -->
  <ellipse cx="38" cy="52" rx="16" ry="22" fill="#D49B4E" transform="rotate(15 38 52)"/>
  <!-- 耳朵 (右) -->
  <ellipse cx="102" cy="52" rx="16" ry="22" fill="#D49B4E" transform="rotate(-15 102 52)"/>
  <!-- 眼睛 -->
  <circle cx="58" cy="52" r="5" fill="#3D2B1F"/>
  <circle cx="82" cy="52" r="5" fill="#3D2B1F"/>
  <circle cx="59" cy="50" r="2" fill="white"/>
  <circle cx="83" cy="50" r="2" fill="white"/>
  <!-- 鼻子 -->
  <ellipse cx="70" cy="62" rx="8" ry="6" fill="#3D2B1F"/>
  <!-- 嘴巴 -->
  <path d="M64 68 Q70 76 76 68" stroke="#3D2B1F" stroke-width="2" fill="none" stroke-linecap="round"/>
  <!-- 舌头 -->
  <ellipse cx="70" cy="72" rx="5" ry="7" fill="#FF8CA3"/>
  <!-- 前腿 -->
  <rect x="42" y="110" width="14" height="22" rx="6" fill="#E0A85F"/>
  <rect x="84" y="110" width="14" height="22" rx="6" fill="#E0A85F"/>
  <!-- 尾巴 -->
  <path d="M105 85 Q125 70 120 55" stroke="#E8B46F" stroke-width="10" stroke-linecap="round" fill="none"/>
</svg>
```

- [ ] **Step 5: 创建西高地小狗 SVG (westie.svg)**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 140 140">
  <!-- 身体 -->
  <ellipse cx="70" cy="95" rx="38" ry="28" fill="#F5F0E8"/>
  <!-- 头 -->
  <circle cx="70" cy="52" r="30" fill="#FAF7F2"/>
  <!-- 耳朵 (立起来的三角形) -->
  <polygon points="40,45 30,10 55,30" fill="#EDE5D8"/>
  <polygon points="100,45 110,10 85,30" fill="#EDE5D8"/>
  <!-- 眼睛 -->
  <circle cx="58" cy="50" r="5" fill="#2C1810"/>
  <circle cx="82" cy="50" r="5" fill="#2C1810"/>
  <circle cx="59" cy="48" r="2" fill="white"/>
  <circle cx="83" cy="48" r="2" fill="white"/>
  <!-- 鼻子 -->
  <ellipse cx="70" cy="60" rx="7" ry="5" fill="#2C1810"/>
  <!-- 嘴巴 -->
  <path d="M64 66 Q70 72 76 66" stroke="#2C1810" stroke-width="1.5" fill="none" stroke-linecap="round"/>
  <!-- 胡须 -->
  <line x1="58" y1="62" x2="40" y2="58" stroke="#D5CEC0" stroke-width="1"/>
  <line x1="58" y1="64" x2="38" y2="66" stroke="#D5CEC0" stroke-width="1"/>
  <line x1="82" y1="62" x2="100" y2="58" stroke="#D5CEC0" stroke-width="1"/>
  <line x1="82" y1="64" x2="102" y2="66" stroke="#D5CEC0" stroke-width="1"/>
  <!-- 前腿 -->
  <rect x="44" y="108" width="13" height="20" rx="5" fill="#F5F0E8"/>
  <rect x="83" y="108" width="13" height="20" rx="5" fill="#F5F0E8"/>
  <!-- 尾巴（短而上翘） -->
  <path d="M105 82 Q118 72 112 60" stroke="#F5F0E8" stroke-width="8" stroke-linecap="round" fill="none"/>
</svg>
```

---

### Task 3: 创建主 HTML 框架、全局 CSS 和顶部标题区

**文件:**
- 创建: `f:/风水师/warm-station/index.html`

**接口:**
- 产生: `#app` 主容器，`.hero` 标题区，全局 CSS 变量和样式
- 产生: `WarmApp` 全局命名空间对象（后续模块挂载数据和方法）
- 产生: 全局 CSS 类名规范，供后续模块使用

**步骤:**

- [ ] **Step 1: 写入 HTML 骨架 + 全局 CSS + Hero 区**

创建 `f:/风水师/warm-station/index.html`:

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<title>华仔の温暖小站 💕</title>
<style>
/* ===== CSS Reset & Variables ===== */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --pink: #F9D0C3;
  --cream: #F5E6D3;
  --gold: #F0E6D8;
  --deep-pink: #E8A0B4;
  --brown: #8B6F5E;
  --dark: #4A3728;
  --white: #FFFBF7;
  --card-shadow: 0 4px 20px rgba(139, 111, 94, 0.12);
  --radius: 20px;
  --font-main: -apple-system, BlinkMacSystemFont, "PingFang SC", "Microsoft YaHei", sans-serif;
}

html { font-size: 16px; scroll-behavior: smooth; }
body {
  font-family: var(--font-main);
  background: linear-gradient(180deg, #FDF2EC 0%, var(--cream) 30%, #FDF2EC 60%, var(--pink) 100%);
  background-attachment: fixed;
  min-height: 100vh;
  color: var(--dark);
  display: flex;
  justify-content: center;
  -webkit-tap-highlight-color: transparent;
  -webkit-user-select: none;
  user-select: none;
}

#app {
  width: 100%;
  max-width: 480px;
  padding: 0 20px 60px;
}

/* ===== Hero 区 ===== */
.hero {
  text-align: center;
  padding: 40px 20px 24px;
  position: relative;
}

.hero-title {
  font-size: 1.8rem;
  font-weight: 700;
  color: var(--brown);
  letter-spacing: 2px;
  font-family: "Georgia", "STSong", "Songti SC", serif;
}

.hero-subtitle {
  font-size: 0.95rem;
  color: #A0897B;
  margin-top: 8px;
  font-style: italic;
  min-height: 24px;
  transition: opacity 0.5s;
}

.hero-hearts {
  font-size: 1.2rem;
  margin-top: 6px;
  letter-spacing: 4px;
  animation: floatHearts 3s ease-in-out infinite;
}

@keyframes floatHearts {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-6px); }
}

/* ===== Section 通用样式 ===== */
.section {
  margin: 16px 0;
  padding: 24px 20px;
  background: rgba(255, 255, 255, 0.65);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: var(--radius);
  box-shadow: var(--card-shadow);
}

.section-title {
  text-align: center;
  font-size: 1.1rem;
  color: var(--brown);
  margin-bottom: 16px;
  font-weight: 600;
  letter-spacing: 1px;
}

/* ===== 弹窗/Modal 通用样式 ===== */
.modal-overlay {
  display: none;
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.5);
  z-index: 1000;
  justify-content: center;
  align-items: center;
}
.modal-overlay.active { display: flex; }

.modal-box {
  background: var(--white);
  border-radius: var(--radius);
  padding: 32px 24px;
  max-width: 340px; width: 90%;
  text-align: center;
  box-shadow: 0 8px 40px rgba(0,0,0,0.2);
  animation: modalIn 0.3s ease-out;
}

@keyframes modalIn {
  from { transform: scale(0.85); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

.modal-close {
  margin-top: 16px;
  padding: 10px 32px;
  border: none; border-radius: 25px;
  background: var(--deep-pink);
  color: white; font-size: 1rem;
  cursor: pointer;
  transition: transform 0.15s, box-shadow 0.15s;
}
.modal-close:active { transform: scale(0.95); }

/* ===== 粒子容器 ===== */
.particles-container {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  pointer-events: none; z-index: 999;
}
</style>
</head>
<body>

<div class="particles-container" id="particlesContainer"></div>

<div id="app">
  <!-- Hero 标题区 -->
  <section class="hero" id="heroSection">
    <div class="hero-hearts">🌸 💕 🌸</div>
    <h1 class="hero-title" id="heroTitle">华仔の温暖小站</h1>
    <p class="hero-subtitle" id="heroSubtitle">今天也是被爱的一天 ✨</p>
  </section>

  <!-- 后续模块将插入此处 -->

</div>

<script>
// ===== 全局 WARM APP 命名空间 =====
const WA = {
  PASSWORD: 'HhxloveCwh1314!',
  // localStorage 读写工具
  store: {
    get(key, fallback) {
      try { const v = localStorage.getItem('warm_' + key); return v !== null ? JSON.parse(v) : fallback; }
      catch(e) { return fallback; }
    },
    set(key, val) {
      try { localStorage.setItem('warm_' + key, JSON.stringify(val)); } catch(e) {}
    }
  },
  // 初始化数据
  init() {
    if (this.store.get('muyu_count', null) === null) this.store.set('muyu_count', 0);
    if (this.store.get('muyu_total', null) === null) this.store.set('muyu_total', 0);
    if (this.store.get('wheel_count', null) === null) this.store.set('wheel_count', 0);
    if (this.store.get('wheel_records', null) === null) this.store.set('wheel_records', []);
    if (this.store.get('wheel_milestones', null) === null) {
      this.store.set('wheel_milestones', { m52: false, m520: false, m1314: false });
    }
  },
  // 工具：创建 HTML 元素
  el(tag, attrs = {}, ...children) {
    const e = document.createElement(tag);
    for (const [k, v] of Object.entries(attrs)) {
      if (k === 'style' && typeof v === 'object') Object.assign(e.style, v);
      else if (k === 'className') e.className = v;
      else if (k.startsWith('on')) e.addEventListener(k.slice(2).toLowerCase(), v);
      else e.setAttribute(k, v);
    }
    for (const c of children) {
      if (typeof c === 'string') e.appendChild(document.createTextNode(c));
      else if (c instanceof Node) e.appendChild(c);
    }
    return e;
  },
  // 格式化金额
  fmtMoney(count) { return (count * 0.1).toFixed(1); },
  // 粒子效果（全局复用）
  spawnParticles(x, y, text, count = 5) {
    const container = document.getElementById('particlesContainer');
    for (let i = 0; i < count; i++) {
      const p = document.createElement('div');
      p.textContent = text || '❤️';
      p.style.cssText = `
        position:fixed; left:${x}px; top:${y}px; font-size:${12 + Math.random()*10}px;
        pointer-events:none; z-index:999;
        animation: particleUp ${0.6 + Math.random()*0.6}s ease-out forwards;
        transform: translateX(${(Math.random()-0.5)*60}px);
      `;
      container.appendChild(p);
      setTimeout(() => p.remove(), 1200);
    }
  }
};

// 粒子动画 CSS
const particleStyle = document.createElement('style');
particleStyle.textContent = `
  @keyframes particleUp {
    0% { opacity:1; transform: translateY(0) translateX(0) scale(1); }
    100% { opacity:0; transform: translateY(-80px) translateX(var(--dx, 20px)) scale(0.4); }
  }
`;
document.head.appendChild(particleStyle);

WA.init();
</script>
</body>
</html>
```

- [ ] **Step 2: 验证文件可打开**

用浏览器打开 `f:/风水师/warm-station/index.html`，确认页面显示标题区，无 JS 报错。

---

### Task 4: 实现木鱼敲击区

**文件:**
- 修改: `f:/风水师/warm-station/index.html` — 在 `<!-- 后续模块将插入此处 -->` 之前插入木鱼区 HTML
- 修改: `f:/风水师/warm-station/index.html` — 在 `WA.init();` 之前添加木鱼 JS

**接口:**
- 消费: `WA.store`, `WA.fmtMoney`, `WA.spawnParticles`, `WA.el`
- 产生: 木鱼敲击功能，全局可访问 `WA.muyu` 对象

**步骤:**

- [ ] **Step 1: 在 index.html 的 `<!-- 后续模块将插入此处 -->` 之前插入木鱼 HTML**

在 index.html 中找到注释 `<!-- 后续模块将插入此处 -->`，在其前插入：

```html
  <!-- 木鱼敲击区 -->
  <section class="section muyu-section" id="muyuSection">
    <div class="section-title">🪵 敲敲木鱼 · 烦恼飞走</div>
    <div class="muyu-container" id="muyuContainer">
      <div class="muyu-stand">
        <div class="muyu-cushion"></div>
        <div class="muyu-body" id="muyuBody">
          <div class="muyu-groove"></div>
          <div class="muyu-fish"></div>
        </div>
      </div>
    </div>
    <div class="muyu-amount" id="muyuAmount">¥0.0</div>
    <div class="muyu-hint">👆 敲一敲，每下都有小奖励哦～</div>
  </section>
```

- [ ] **Step 2: 在 `WA.init();` 之前添加木鱼 CSS**

在 `</style>` 之前添加：

```css
/* ===== 木鱼区 ===== */
.muyu-container {
  display: flex;
  justify-content: center;
  padding: 10px 0;
}

.muyu-stand {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
}

.muyu-cushion {
  width: 160px; height: 24px;
  background: radial-gradient(ellipse at center, #C8956C 0%, #8B5E3C 100%);
  border-radius: 50%;
  margin-bottom: -8px;
  z-index: 1;
}

.muyu-body {
  width: 130px; height: 130px;
  background: radial-gradient(circle at 45% 40%, #D4A76A 0%, #B8753E 40%, #8B5730 100%);
  border-radius: 50%;
  position: relative;
  box-shadow: 0 8px 30px rgba(139, 87, 48, 0.35), inset 0 -4px 8px rgba(0,0,0,0.15);
  transition: transform 0.08s ease-out;
  z-index: 2;
}

.muyu-body:active,
.muyu-body.pressed {
  transform: scale(0.92) translateY(6px);
  box-shadow: 0 2px 10px rgba(139, 87, 48, 0.3), inset 0 -2px 4px rgba(0,0,0,0.2);
}

.muyu-groove {
  position: absolute;
  top: 18px; left: 50%;
  transform: translateX(-50%);
  width: 60px; height: 5px;
  background: rgba(0,0,0,0.2);
  border-radius: 3px;
  box-shadow: 0 1px 2px rgba(255,255,255,0.2);
}

.muyu-fish {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  width: 40px; height: 28px;
  background: rgba(0,0,0,0.08);
  border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
}

.muyu-fish::after {
  content: '🐟';
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  font-size: 14px;
  opacity: 0.5;
}

.muyu-amount {
  text-align: center;
  font-size: 2.6rem;
  font-weight: 700;
  color: var(--brown);
  margin-top: 12px;
  font-family: "Georgia", serif;
  transition: transform 0.15s ease-out;
}

.muyu-amount.bump {
  animation: amountBump 0.3s ease-out;
}

@keyframes amountBump {
  0% { transform: scale(1); }
  30% { transform: scale(1.2); color: var(--deep-pink); }
  100% { transform: scale(1); }
}

.muyu-hint {
  text-align: center;
  font-size: 0.8rem;
  color: #B8A090;
  margin-top: 6px;
}
```

- [ ] **Step 3: 在 `WA.init();` 之前添加木鱼 JS 逻辑**

```javascript
// ===== 木鱼模块 =====
WA.muyu = {
  audioCtx: null,

  init() {
    this.count = WA.store.get('muyu_count', 0);
    this.total = WA.store.get('muyu_total', 0);
    this.body = document.getElementById('muyuBody');
    this.amountEl = document.getElementById('muyuAmount');
    this.updateDisplay();
    this.bindEvents();
  },

  bindEvents() {
    this.body.addEventListener('pointerdown', (e) => {
      e.preventDefault();
      this.body.classList.add('pressed');
      this.hit(e);
    });
    this.body.addEventListener('pointerup', () => {
      this.body.classList.remove('pressed');
    });
    this.body.addEventListener('pointerleave', () => {
      this.body.classList.remove('pressed');
    });
  },

  hit(e) {
    this.count++;
    this.total = parseFloat((this.total + 0.1).toFixed(1));
    WA.store.set('muyu_count', this.count);
    WA.store.set('muyu_total', this.total);

    // 音效
    this.playSound();

    // 振动
    if (navigator.vibrate) navigator.vibrate(15);

    // 显示更新
    this.updateDisplay();
    this.amountEl.classList.remove('bump');
    void this.amountEl.offsetWidth;
    this.amountEl.classList.add('bump');

    // 粒子
    const rect = this.body.getBoundingClientRect();
    const cx = rect.left + rect.width / 2;
    const cy = rect.top;
    WA.spawnParticles(cx, cy, '+0.1', 3);
    WA.spawnParticles(cx + 15, cy - 5, '💛', 2);

    // 彩蛋检测
    this.checkEasterEgg();
  },

  playSound() {
    if (!this.audioCtx) this.audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    const ctx = this.audioCtx;
    const now = ctx.currentTime;

    // 低频"咚"声 — 模拟木鱼
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.connect(gain);
    gain.connect(ctx.destination);

    osc.type = 'sine';
    osc.frequency.setValueAtTime(180, now);
    osc.frequency.exponentialRampToValueAtTime(80, now + 0.12);

    gain.gain.setValueAtTime(0.35, now);
    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.25);

    osc.start(now);
    osc.stop(now + 0.25);
  },

  updateDisplay() {
    this.amountEl.textContent = '¥' + this.total.toFixed(1);
  },

  checkEasterEgg() {
    const special = { '5.2': '💝 5.20！我爱你，不止今天', '13.1': '💖 13.14！一生一世只爱你', '52.0': '💗 52.0！吾爱零，爱你永无止境' };
    const t = this.total.toFixed(1);
    if (special[t]) {
      setTimeout(() => {
        WA.showToast(special[t]);
        WA.spawnParticles(window.innerWidth/2, window.innerHeight/2, '🎉', 12);
      }, 300);
    }
  }
};

WA.showToast = function(msg) {
  const existing = document.querySelector('.toast-msg');
  if (existing) existing.remove();
  const toast = WA.el('div', { className: 'toast-msg' }, msg);
  toast.style.cssText = `
    position:fixed; top:20%; left:50%; transform:translateX(-50%);
    background:rgba(74,55,40,0.92); color:#FFFBF7; padding:14px 28px;
    border-radius:30px; font-size:1rem; z-index:2000;
    animation: toastIn 0.4s ease-out, toastOut 0.4s 2s ease-in forwards;
    pointer-events:none; text-align:center; max-width:90vw;
  `;
  document.body.appendChild(toast);
  setTimeout(() => toast.remove(), 2500);
};

// Toast 动画样式
const toastAnimStyle = document.createElement('style');
toastAnimStyle.textContent = `
  @keyframes toastIn { from { opacity:0; transform:translateX(-50%) translateY(20px); } to { opacity:1; transform:translateX(-50%) translateY(0); } }
  @keyframes toastOut { from { opacity:1; } to { opacity:0; } }
`;
document.head.appendChild(toastAnimStyle);

// 初始化木鱼
WA.muyu.init();
```

- [ ] **Step 4: 验证**

在浏览器打开，点击木鱼测试：声音、振动（手机端）、金额变化、粒子效果。

---

### Task 5: 实现幸运大转盘

**文件:**
- 修改: `f:/风水师/warm-station/index.html` — 在木鱼 section 之后插入转盘 HTML
- 修改: `f:/风水师/warm-station/index.html` — 添加转盘 CSS 和 JS
- 修改: `f:/风水师/warm-station/index.html` — 添加结果弹窗 modal

**接口:**
- 消费: `WA.store`, `WA.el`, `WA.spawnParticles`, `WA.showToast`
- 产生: `WA.wheel` 对象

**步骤:**

- [ ] **Step 1: 在木鱼 `</section>` 之后，轮播区之前插入转盘 HTML**

```html
  <!-- 幸运大转盘 -->
  <section class="section wheel-section" id="wheelSection">
    <div class="section-title">🎡 幸运大转盘</div>
    <div class="wheel-wrapper">
      <div class="wheel-pointer">▼</div>
      <canvas id="wheelCanvas" width="300" height="300"></canvas>
      <button class="wheel-btn" id="wheelBtn">抽</button>
    </div>
    <div class="wheel-counter" id="wheelCounter">已转 0 次</div>
  </section>
```

- [ ] **Step 2: 在 `</style>` 之前添加转盘 CSS**

```css
/* ===== 转盘 ===== */
.wheel-wrapper {
  position: relative;
  width: 300px; height: 300px;
  margin: 0 auto;
}

.wheel-pointer {
  position: absolute;
  top: -10px; left: 50%;
  transform: translateX(-50%);
  font-size: 1.8rem;
  color: var(--deep-pink);
  z-index: 10;
  filter: drop-shadow(0 2px 4px rgba(0,0,0,0.2));
  animation: pointerBounce 1s ease-in-out infinite;
}

@keyframes pointerBounce {
  0%, 100% { transform: translateX(-50%) translateY(0); }
  50% { transform: translateX(-50%) translateY(4px); }
}

#wheelCanvas {
  display: block;
  width: 300px; height: 300px;
}

.wheel-btn {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  width: 64px; height: 64px;
  border-radius: 50%;
  border: 4px solid var(--white);
  background: linear-gradient(135deg, #FF6B8A, #E8486B);
  color: white;
  font-size: 1.3rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 4px 16px rgba(232, 72, 107, 0.4);
  z-index: 5;
  transition: transform 0.1s;
}

.wheel-btn:active {
  transform: translate(-50%, -50%) scale(0.9);
}

.wheel-btn:disabled {
  opacity: 0.6;
  pointer-events: none;
}

.wheel-counter {
  text-align: center;
  color: #A0897B;
  font-size: 0.85rem;
  margin-top: 10px;
}
```

- [ ] **Step 3: 在 `WA.muyu.init();` 之前添加转盘 JS**

```javascript
// ===== 转盘模块 =====
WA.wheel = {
  prizes: [
    { name: '谢谢惠顾', icon: '😅', prob: 0.37, level: 0 },
    { name: '华仔亲亲', icon: '😘', prob: 0.20, level: 0 },
    { name: '请喝奶茶', icon: '🧋', prob: 0.15, level: 0 },
    { name: '爱的抱抱', icon: '🫂', prob: 0.12, level: 0 },
    { name: '请吃饭', icon: '🍽️', prob: 0.08, level: 1 },
    { name: '买束花花', icon: '💐', prob: 0.05, level: 1 },
    { name: '买Jellycat', icon: '🧸', prob: 0.02, level: 2 },
    { name: '买包包/戒指', icon: '💍', prob: 0.01, level: 3 },
  ],
  colors: ['#FFE0D0', '#FFF5EE', '#FFD4C4', '#FFF8F0', '#FFE8DA', '#FFF2E8', '#FFDCC8', '#FFFAF5'],
  spinning: false,
  currentAngle: 0,

  init() {
    this.count = WA.store.get('wheel_count', 0);
    this.records = WA.store.get('wheel_records', []);
    this.milestones = WA.store.get('wheel_milestones', { m52: false, m520: false, m1314: false });
    this.canvas = document.getElementById('wheelCanvas');
    this.ctx = this.canvas.getContext('2d');
    this.btn = document.getElementById('wheelBtn');
    this.counterEl = document.getElementById('wheelCounter');
    this.draw();
    this.updateCounter();
    this.bindEvents();
  },

  bindEvents() {
    this.btn.addEventListener('click', () => this.spin());
  },

  draw(rotAngle = 0) {
    const ctx = this.ctx;
    const cx = 150, cy = 150, r = 140;
    const sliceAngle = (2 * Math.PI) / this.prizes.length;

    ctx.clearRect(0, 0, 300, 300);

    // 外圈阴影
    ctx.save();
    ctx.beginPath();
    ctx.arc(cx, cy, r + 4, 0, 2 * Math.PI);
    ctx.fillStyle = 'rgba(139,111,94,0.15)';
    ctx.fill();
    ctx.restore();

    for (let i = 0; i < this.prizes.length; i++) {
      const start = i * sliceAngle + rotAngle;
      const end = start + sliceAngle;

      ctx.beginPath();
      ctx.moveTo(cx, cy);
      ctx.arc(cx, cy, r, start, end);
      ctx.closePath();
      ctx.fillStyle = this.colors[i];
      ctx.fill();
      ctx.strokeStyle = 'rgba(255,255,255,0.6)';
      ctx.lineWidth = 2;
      ctx.stroke();

      // 文字
      ctx.save();
      ctx.translate(cx, cy);
      ctx.rotate(start + sliceAngle / 2);
      ctx.textAlign = 'center';
      ctx.fillStyle = '#4A3728';
      ctx.font = 'bold 13px -apple-system, "PingFang SC", sans-serif';
      ctx.fillText(this.prizes[i].icon, r * 0.55, 5);
      ctx.font = '11px -apple-system, "PingFang SC", sans-serif';
      ctx.fillText(this.prizes[i].name, r * 0.55, 22);
      ctx.restore();
    }

    // 中心圆
    ctx.beginPath();
    ctx.arc(cx, cy, 36, 0, 2*Math.PI);
    const centerGrad = ctx.createRadialGradient(cx, cy, 5, cx, cy, 36);
    centerGrad.addColorStop(0, '#FFFBF7');
    centerGrad.addColorStop(1, '#F0E6D8');
    ctx.fillStyle = centerGrad;
    ctx.fill();
    ctx.strokeStyle = 'rgba(255,255,255,0.8)';
    ctx.lineWidth = 3;
    ctx.stroke();
  },

  getAdjustedProbs() {
    const probs = this.prizes.map(p => ({ ...p }));
    const c = this.count;

    // 里程碑 52 次 → 请吃饭 (level 1) 提升
    if (!this.milestones.m52 && c >= 48) {
      const boost = Math.min(0.3, 0.08 + (c - 48) * 0.05);
      probs[4].prob = boost; // 请吃饭
      const extra = boost - 0.08;
      probs[0].prob = Math.max(0.05, probs[0].prob - extra * 0.6);
      probs[1].prob = Math.max(0.05, probs[1].prob - extra * 0.2);
      probs[2].prob = Math.max(0.03, probs[2].prob - extra * 0.2);
    }

    // 里程碑 520 → Jellycat (level 2)
    if (!this.milestones.m520 && c >= 505) {
      const boost = Math.min(0.25, 0.02 + (c - 505) * 0.015);
      probs[6].prob = boost;
      const extra = boost - 0.02;
      probs[0].prob = Math.max(0.05, probs[0].prob - extra * 0.7);
      probs[1].prob = Math.max(0.05, probs[1].prob - extra * 0.15);
      probs[3].prob = Math.max(0.03, probs[3].prob - extra * 0.15);
    }

    // 里程碑 1314 → 买包包/戒指 (level 3)
    if (!this.milestones.m1314 && c >= 1295) {
      const boost = Math.min(0.2, 0.01 + (c - 1295) * 0.01);
      probs[7].prob = boost;
      const extra = boost - 0.01;
      probs[0].prob = Math.max(0.05, probs[0].prob - extra * 0.7);
      probs[2].prob = Math.max(0.05, probs[2].prob - extra * 0.3);
    }

    return probs;
  },

  pickPrize() {
    const probs = this.getAdjustedProbs();
    let r = Math.random();
    for (let i = 0; i < probs.length; i++) {
      r -= probs[i].prob;
      if (r <= 0) return { index: i, prize: probs[i] };
    }
    return { index: 0, prize: probs[0] };
  },

  spin() {
    if (this.spinning) return;
    this.spinning = true;
    this.btn.disabled = true;
    this.count++;
    WA.store.set('wheel_count', this.count);

    const { index, prize } = this.pickPrize();

    // 计算目标角度（指针在顶部=270度）
    const sliceAngle = 360 / this.prizes.length;
    const targetSliceCenter = index * sliceAngle + sliceAngle / 2; // 目标格中心角度
    const spinDeg = 360 * (4 + Math.random() * 3) + (360 - targetSliceCenter - (this.currentAngle % 360));
    const targetAngle = this.currentAngle + spinDeg;

    this.currentAngle = targetAngle;
    const targetRad = (targetAngle * Math.PI) / 180;

    // 动画
    const duration = 3500 + Math.random() * 1500;
    const startAngle = this.currentAngle - spinDeg * (Math.PI / 180);
    const startTime = performance.now();

    const animate = (now) => {
      const elapsed = now - startTime;
      const progress = Math.min(elapsed / duration, 1);
      // easeOutCubic
      const eased = 1 - Math.pow(1 - progress, 3);
      const cur = startAngle + spinDeg * (Math.PI / 180) * eased;
      this.draw(cur);
      if (progress < 1) {
        requestAnimationFrame(animate);
      } else {
        this.currentAngle = targetAngle;
        this.draw(targetRad);
        this.onResult(prize, index);
      }
    };

    requestAnimationFrame(animate);
    this.updateCounter();
  },

  onResult(prize, index) {
    // 保存记录
    const record = { time: new Date().toISOString(), prize: prize.name, icon: prize.icon, level: prize.level };
    this.records.push(record);
    WA.store.set('wheel_records', this.records);

    // 里程碑检测
    if (prize.level === 1 && !this.milestones.m52 && this.count >= 50) {
      this.milestones.m52 = true;
      WA.store.set('wheel_milestones', this.milestones);
    }
    if (prize.level === 2 && !this.milestones.m520 && this.count >= 510) {
      this.milestones.m520 = true;
      WA.store.set('wheel_milestones', this.milestones);
    }
    if (prize.level === 3 && !this.milestones.m1314 && this.count >= 1300) {
      this.milestones.m1314 = true;
      WA.store.set('wheel_milestones', this.milestones);
    }

    // 弹窗
    this.showResultModal(prize);

    // 大奖特效
    if (prize.level >= 2) {
      setTimeout(() => {
        WA.spawnParticles(window.innerWidth/2, window.innerHeight/2, '🎉', 20);
        WA.spawnParticles(window.innerWidth/2, window.innerHeight/2, '💖', 15);
      }, 400);
    }

    setTimeout(() => {
      this.spinning = false;
      this.btn.disabled = false;
    }, 500);
  },

  showResultModal(prize) {
    const overlay = document.getElementById('resultOverlay');
    const content = document.getElementById('resultContent');
    content.innerHTML = `
      <div style="font-size:3rem;margin-bottom:8px">${prize.icon}</div>
      <div style="font-size:1.5rem;font-weight:700;color:#4A3728;margin-bottom:4px">${prize.name}</div>
      <div style="color:#A0897B;font-size:0.85rem">
        ${prize.level >= 2 ? '🎊 恭喜！大奖来啦！' : prize.level === 0 ? '下次会更好～' : '运气不错哦～'}
      </div>
    `;
    overlay.classList.add('active');
  },

  updateCounter() {
    this.counterEl.textContent = `已转 ${this.count} 次`;
  }
};

// 结果弹窗 HTML（插入 body 末尾）
const resultModalHTML = `
<div class="modal-overlay" id="resultOverlay">
  <div class="modal-box">
    <div id="resultContent"></div>
    <button class="modal-close" onclick="document.getElementById('resultOverlay').classList.remove('active');WA.wheel.spinning=false;WA.wheel.btn.disabled=false;">好的 👌</button>
  </div>
</div>
`;
document.body.insertAdjacentHTML('beforeend', resultModalHTML);

// 初始化转盘
WA.wheel.init();
```

- [ ] **Step 4: 验证**

浏览器打开，测试转盘：点击抽奖、动画、弹窗、计数器。

---

### Task 6: 实现照片墙

**文件:**
- 修改: `f:/风水师/warm-station/index.html` — 添加照片墙 HTML/CSS/JS

**接口:**
- 消费: 照片路径 `images/photos/*.jpg`，花卉/小狗 SVG
- 产生: 照片墙功能

**步骤:**

- [ ] **Step 1: 在转盘 section 之后插入照片墙 HTML**

```html
  <!-- 照片墙 -->
  <section class="section photo-section" id="photoSection">
    <div class="section-title">🖼️ 我们的美好时光</div>
    <div class="photo-grid" id="photoGrid"></div>
  </section>

  <!-- 照片放大弹窗 -->
  <div class="modal-overlay" id="photoOverlay" onclick="this.classList.remove('active')">
    <img id="photoLarge" style="max-width:92vw;max-height:80vh;border-radius:12px;object-fit:contain;" />
  </div>
```

- [ ] **Step 2: 在 `</style>` 前添加照片墙 CSS**

```css
/* ===== 照片墙 ===== */
.photo-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.photo-card {
  border-radius: 14px;
  overflow: hidden;
  box-shadow: 0 3px 12px rgba(139,111,94,0.12);
  background: var(--white);
  transition: transform 0.2s;
  cursor: pointer;
}

.photo-card:active {
  transform: scale(0.96);
}

.photo-card img {
  width: 100%;
  aspect-ratio: 1;
  object-fit: cover;
  display: block;
}

.photo-caption {
  padding: 8px 10px;
  font-size: 0.78rem;
  color: #A0897B;
  text-align: center;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

#photoOverlay {
  cursor: pointer;
}
```

- [ ] **Step 3: 在 `WA.wheel.init();` 前添加照片墙 JS**

```javascript
// ===== 照片墙模块 =====
WA.photos = {
  // 配置：照片列表（照片文件名 + 配文）
  items: [
    { src: 'images/photos/4f04958f420a5e60cf6f3616ea59957a.jpg', caption: '你的笑容是最好的阳光 ☀️' },
    { src: 'images/photos/53ebeacc4aa5dbfb59dc553878fce3b3.jpg', caption: '和你在一起的每一天 💕' },
    { src: 'images/photos/5cefaa7c735e633c8ed5e44d90eda67d.jpg', caption: '宝贝真好看 🌸' },
    { src: 'images/photos/645333594c334da9e63dda1c426c3cd9.jpg', caption: '超喜欢这张～ ✨' },
    { src: 'images/photos/78d382a4b093ede4e9594f8fde818fe6.jpg', caption: '最美的风景就是你 🏔️' },
    { src: 'images/photos/898e868fa8cf34ee356e59537d915fab.jpg', caption: '一直一直在一起吧 💍' },
    { src: 'images/photos/ac6f60e355620364a1122e8c197f3021.jpg', caption: '华仔最爱的宝贝 🐶' },
    { src: 'images/photos/d81b1ad144b1e88a4c89255fb1032c14.jpg', caption: '你就是我的小太阳 🌻' },
    { src: 'images/photos/f5b76005becde58e24807b4217aede66.jpg', caption: '看多少次都觉得心动 💗' },
    // 郁金香
    { src: 'images/flowers/tulip1.svg', caption: '送你一束郁金香 🌷' },
    { src: 'images/flowers/tulip2.svg', caption: '花开的时候最想你 💐' },
    { src: 'images/flowers/tulip3.svg', caption: '温柔如你，美丽如花 🥀' },
    // 小狗
    { src: 'images/dogs/golden.svg', caption: '金毛的微笑治愈一切 🐕' },
    { src: 'images/dogs/westie.svg', caption: '像你一样可爱的小西高地 🐩' },
  ],

  init() {
    const grid = document.getElementById('photoGrid');
    this.items.forEach((item, i) => {
      const card = WA.el('div', { className: 'photo-card', onClick: () => this.openPhoto(item.src) },
        WA.el('img', { src: item.src, alt: item.caption, loading: 'lazy' }),
        WA.el('div', { className: 'photo-caption' }, item.caption)
      );
      grid.appendChild(card);
    });
  },

  openPhoto(src) {
    const overlay = document.getElementById('photoOverlay');
    const img = document.getElementById('photoLarge');
    img.src = src;
    overlay.classList.add('active');
  }
};

WA.photos.init();
```

- [ ] **Step 4: 验证**

确认照片正常显示，点击放大。

---

### Task 7: 实现暖心话语区

**文件:**
- 修改: `f:/风水师/warm-station/index.html` — 添加暖心话语 HTML/CSS/JS

**步骤:**

- [ ] **Step 1: 在照片墙 section 之后插入暖心话语 HTML**

```html
  <!-- 暖心话语 -->
  <section class="section quote-section" id="quoteSection">
    <div class="section-title">📝 华仔想对你说</div>
    <div class="quote-card" id="quoteCard">
      <div class="quote-text" id="quoteText"></div>
      <div class="quote-hint">👆 点击换一句</div>
    </div>
  </section>
```

- [ ] **Step 2: 在 `</style>` 前添加 CSS**

```css
/* ===== 暖心话语 ===== */
.quote-card {
  text-align: center;
  padding: 20px 16px;
  background: linear-gradient(135deg, #FFF5F0 0%, #FFEDE4 100%);
  border-radius: 14px;
  border: 1px solid rgba(232, 160, 180, 0.2);
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  min-height: 80px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.quote-card:active {
  transform: scale(0.97);
}

.quote-text {
  font-size: 1.05rem;
  color: var(--brown);
  line-height: 1.7;
  transition: opacity 0.3s;
  font-family: "Georgia", "STSong", "Songti SC", serif;
}

.quote-text.fade {
  opacity: 0;
}

.quote-hint {
  font-size: 0.72rem;
  color: #C8B4A4;
  margin-top: 10px;
}
```

- [ ] **Step 3: 在 `WA.photos.init();` 前添加话术 JS**

```javascript
// ===== 暖心话语模块 =====
WA.quotes = {
  list: [
    // 禅意风
    '一念放下，万般自在 🍃',
    '一切都刚刚好，不急不躁 🌿',
    '放下烦忧，心生欢喜 ☀️',
    '今天的不开心就到此为止吧 🌙',
    '深呼吸，没有什么过不去的 ⛅',
    '静心片刻，世界都温柔了 🧘‍♀️',
    '万物皆有裂痕，那是光照进来的地方 ✨',
    '慢慢来，比较快 🐢',
    '心怀温柔，岁月静好 🌸',
    '一花一世界，一笑一尘缘 🌺',
    '生活明朗，万物可爱 🐣',
    '风吹哪页读哪页，随心就好 📖',
    '你已经做得很好了，真的 💪',
    '累了就歇歇，没关系的 🛋️',
    '心若向阳，无惧悲伤 🌻',
    // 甜蜜风
    '华仔永远是你的靠山 🏔️',
    '宝贝，你是最棒的 🌟',
    '今天也是被华仔宠爱的一天 💕',
    '华仔的心里只有你一个人 💘',
    '不管你变成什么样子我都喜欢 💗',
    '宝贝辛苦了，华仔给你捏捏肩 💆‍♀️',
    '你笑起来最好看了，多笑笑 😊',
    '有我在，什么都不用怕 🦸‍♂️',
    '华仔会一直一直陪着你 🤝',
    '你是我最珍贵的宝藏 💎',
    '宝贝说什么都是对的 👑',
    '全世界最好的女朋友就是你 🏆',
    '华仔的怀抱永远为你敞开 🫂',
    '谢谢你来到我的生命里 🙏',
    '每天醒来第一个想到的就是你 🌅',
  ],

  init() {
    this.el = document.getElementById('quoteText');
    this.card = document.getElementById('quoteCard');
    this.showRandom();
    this.card.addEventListener('click', () => this.nextQuote());
  },

  showRandom() {
    const i = Math.floor(Math.random() * this.list.length);
    this.currentIndex = i;
    this.el.textContent = this.list[i];
    WA.store.set('quotes_index', i);
  },

  nextQuote() {
    this.el.classList.add('fade');
    setTimeout(() => {
      const next = (this.currentIndex + 1) % this.list.length;
      this.currentIndex = next;
      this.el.textContent = this.list[next];
      this.el.classList.remove('fade');
    }, 300);
  }
};

WA.quotes.init();
```

- [ ] **Step 4: 验证**

刷新页面看随机话语，点击切换。

---

### Task 8: 实现隐藏管理面板

**文件:**
- 修改: `f:/风水师/warm-station/index.html` — 添加管理面板 HTML/CSS/JS

**步骤:**

- [ ] **Step 1: 在 app 容器末尾，`</div><!-- #app -->` 之前插入管理面板 HTML**

```html
  <!-- 管理面板（默认隐藏） -->
  <div class="admin-overlay" id="adminOverlay">
    <div class="admin-panel">
      <div class="admin-header">
        <h2>🔒 管理面板</h2>
        <button class="admin-close" id="adminClose">✕</button>
      </div>
      <div class="admin-body" id="adminBody"></div>
    </div>
  </div>

  <!-- 密码输入弹窗 -->
  <div class="modal-overlay" id="pwdOverlay">
    <div class="modal-box">
      <div style="font-size:1.2rem;margin-bottom:12px;color:var(--brown);">🔐 验证身份</div>
      <input type="password" id="pwdInput" placeholder="请输入密码" style="width:100%;padding:12px;border:2px solid var(--pink);border-radius:25px;font-size:1rem;text-align:center;outline:none;margin-bottom:12px;">
      <div>
        <button class="modal-close" onclick="document.getElementById('pwdOverlay').classList.remove('active')">取消</button>
        <button class="modal-close" id="pwdConfirm" style="margin-left:8px;background:var(--brown);">确认</button>
      </div>
    </div>
  </div>
```

- [ ] **Step 2: 在 `</style>` 前添加管理面板 CSS**

```css
/* ===== 管理面板 ===== */
.admin-overlay {
  display: none;
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.5);
  z-index: 2000;
  justify-content: center;
  align-items: center;
}
.admin-overlay.active { display: flex; }

.admin-panel {
  background: var(--white);
  border-radius: var(--radius);
  width: 92%;
  max-width: 440px;
  max-height: 85vh;
  overflow-y: auto;
  box-shadow: 0 8px 40px rgba(0,0,0,0.25);
  animation: modalIn 0.3s ease-out;
}

.admin-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 20px;
  border-bottom: 1px solid #F0E6D8;
  position: sticky; top: 0;
  background: var(--white);
  z-index: 1;
}

.admin-header h2 {
  font-size: 1.1rem;
  color: var(--brown);
}

.admin-close {
  background: none; border: none;
  font-size: 1.3rem; color: #A0897B;
  cursor: pointer; padding: 4px 8px;
}

.admin-body {
  padding: 16px 20px 24px;
}

.admin-stat {
  display: flex;
  justify-content: space-between;
  padding: 10px 0;
  border-bottom: 1px solid #F5E6D3;
  font-size: 0.9rem;
}

.admin-stat-label { color: #A0897B; }
.admin-stat-value { color: var(--brown); font-weight: 600; }

.admin-section-title {
  font-size: 0.95rem;
  color: var(--brown);
  font-weight: 600;
  margin: 18px 0 8px;
}

.admin-records {
  max-height: 200px;
  overflow-y: auto;
  font-size: 0.8rem;
}

.admin-record {
  display: flex;
  justify-content: space-between;
  padding: 6px 0;
  border-bottom: 1px solid #F5E6D3;
  color: #A0897B;
}

.admin-record-prize { color: var(--brown); font-weight: 500; }

.admin-btn {
  display: block;
  width: 100%;
  margin-top: 10px;
  padding: 10px;
  border: none; border-radius: 20px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: transform 0.1s;
}

.admin-btn:active { transform: scale(0.97); }

.admin-btn-danger {
  background: #FFE0E0;
  color: #C0392B;
}
.admin-btn-info {
  background: #E8F0FE;
  color: #2C5282;
}
```

- [ ] **Step 3: 在 `</script>` 之前添加管理面板 JS**

```javascript
// ===== 管理面板模块 =====
WA.admin = {
  init() {
    this.overlay = document.getElementById('adminOverlay');
    this.body = document.getElementById('adminBody');
    this.pwdOverlay = document.getElementById('pwdOverlay');
    this.pwdInput = document.getElementById('pwdInput');

    // 长按标题 3 秒触发
    const title = document.getElementById('heroTitle');
    let pressTimer;
    title.addEventListener('pointerdown', () => {
      pressTimer = setTimeout(() => {
        this.pwdInput.value = '';
        this.pwdOverlay.classList.add('active');
      }, 3000);
    });
    title.addEventListener('pointerup', () => clearTimeout(pressTimer));
    title.addEventListener('pointerleave', () => clearTimeout(pressTimer));
    title.addEventListener('pointermove', () => clearTimeout(pressTimer));

    // 密码确认
    document.getElementById('pwdConfirm').addEventListener('click', () => this.verifyPwd());
    this.pwdInput.addEventListener('keydown', (e) => { if (e.key === 'Enter') this.verifyPwd(); });

    // 关闭
    document.getElementById('adminClose').addEventListener('click', () => this.overlay.classList.remove('active'));
  },

  verifyPwd() {
    if (this.pwdInput.value === WA.PASSWORD) {
      this.pwdOverlay.classList.remove('active');
      this.show();
    } else {
      WA.showToast('密码错误 ❌');
      this.pwdInput.value = '';
    }
  },

  show() {
    const mc = WA.store.get('muyu_count', 0);
    const mt = WA.store.get('muyu_total', 0);
    const wc = WA.store.get('wheel_count', 0);
    const records = WA.store.get('wheel_records', []);
    const ms = WA.store.get('wheel_milestones', {});

    // 统计各奖品中奖次数
    const prizeCount = {};
    records.forEach(r => { prizeCount[r.prize] = (prizeCount[r.prize] || 0) + 1; });

    this.body.innerHTML = `
      <div class="admin-section-title">📊 木鱼统计</div>
      <div class="admin-stat"><span class="admin-stat-label">总敲击数</span><span class="admin-stat-value">${mc} 次</span></div>
      <div class="admin-stat"><span class="admin-stat-label">累计金额</span><span class="admin-stat-value">¥${mt.toFixed(1)}</span></div>

      <div class="admin-section-title">🎡 转盘统计</div>
      <div class="admin-stat"><span class="admin-stat-label">总转动次数</span><span class="admin-stat-value">${wc} 次</span></div>
      <div class="admin-stat"><span class="admin-stat-label">里程碑52次（请吃饭）</span><span class="admin-stat-value">${ms.m52 ? '✅ 已触发' : '⏳ 未触发 (' + wc + '/~52)'}</span></div>
      <div class="admin-stat"><span class="admin-stat-label">里程碑520次（Jellycat）</span><span class="admin-stat-value">${ms.m520 ? '✅ 已触发' : '⏳ 未触发 (' + wc + '/~520)'}</span></div>
      <div class="admin-stat"><span class="admin-stat-label">里程碑1314次（戒指）</span><span class="admin-stat-value">${ms.m1314 ? '✅ 已触发' : '⏳ 未触发 (' + wc + '/~1314)'}</span></div>

      <div class="admin-section-title">🏆 中奖统计</div>
      ${WA.wheel.prizes.map(p => `
        <div class="admin-stat"><span class="admin-stat-label">${p.icon} ${p.name}</span><span class="admin-stat-value">${prizeCount[p.name] || 0} 次</span></div>
      `).join('')}

      <div class="admin-section-title">📋 最近中奖记录</div>
      <div class="admin-records">
        ${records.slice().reverse().slice(0, 50).map(r => `
          <div class="admin-record">
            <span class="admin-record-prize">${r.icon} ${r.prize}</span>
            <span>${new Date(r.time).toLocaleString('zh-CN')}</span>
          </div>
        `).join('') || '<div style="color:#A0897B;text-align:center;padding:12px;">暂无记录</div>'}
      </div>

      <button class="admin-btn admin-btn-info" id="adminRefresh">🔄 刷新数据</button>
      <button class="admin-btn admin-btn-danger" id="adminReset">⚠️ 重置所有数据</button>
    `;

    // 刷新按钮
    document.getElementById('adminRefresh').addEventListener('click', () => this.show());

    // 重置按钮（二次确认）
    document.getElementById('adminReset').addEventListener('click', () => {
      if (confirm('确定要重置所有数据吗？此操作不可恢复！')) {
        if (confirm('再次确认：真的要清除所有木鱼金额和转盘记录吗？')) {
          WA.store.set('muyu_count', 0);
          WA.store.set('muyu_total', 0);
          WA.store.set('wheel_count', 0);
          WA.store.set('wheel_records', []);
          WA.store.set('wheel_milestones', { m52: false, m520: false, m1314: false });
          WA.store.set('quotes_index', 0);
          WA.muyu.count = 0;
          WA.muyu.total = 0;
          WA.muyu.updateDisplay();
          WA.wheel.count = 0;
          WA.wheel.records = [];
          WA.wheel.milestones = { m52: false, m520: false, m1314: false };
          WA.wheel.updateCounter();
          this.show();
          WA.showToast('数据已重置 ✅');
        }
      }
    });

    this.overlay.classList.add('active');
  }
};

WA.admin.init();
```

- [ ] **Step 4: 验证**

长按标题3秒，输入密码，测试管理面板功能。

---

### Task 9: 整体测试与最终调整

**文件:**
- 修改: `f:/风水师/warm-station/index.html` — 添加飘落花瓣背景动画

**步骤:**

- [ ] **Step 1: 添加飘落花瓣背景动画 CSS**

```css
/* ===== 飘落花瓣 ===== */
.petal {
  position: fixed;
  pointer-events: none;
  z-index: 0;
  animation: petalFall linear forwards;
}

@keyframes petalFall {
  0% {
    transform: translateY(-10vh) rotate(0deg) translateX(0);
    opacity: 0.8;
  }
  25% {
    transform: translateY(25vh) rotate(90deg) translateX(30px);
  }
  50% {
    transform: translateY(50vh) rotate(180deg) translateX(-20px);
  }
  75% {
    transform: translateY(75vh) rotate(270deg) translateX(25px);
    opacity: 0.4;
  }
  100% {
    transform: translateY(105vh) rotate(360deg) translateX(-10px);
    opacity: 0;
  }
}
```

- [ ] **Step 2: 添加花瓣生成 JS**

在 `WA.init();` 之后添加：

```javascript
// ===== 飘落花瓣 =====
(function initPetals() {
  const petals = ['🌸', '💮', '🌷', '🍂', '✨'];
  function spawnPetal() {
    const p = document.createElement('div');
    p.className = 'petal';
    p.textContent = petals[Math.floor(Math.random() * petals.length)];
    p.style.left = Math.random() * 100 + '%';
    p.style.fontSize = (12 + Math.random() * 16) + 'px';
    p.style.animationDuration = (8 + Math.random() * 12) + 's';
    p.style.animationDelay = Math.random() * 2 + 's';
    document.body.appendChild(p);
    setTimeout(() => p.remove(), 15000);
  }
  // 初始生成一些
  for (let i = 0; i < 8; i++) {
    setTimeout(() => spawnPetal(), i * 800);
  }
  // 每3秒生成一个
  setInterval(spawnPetal, 3000);
})();
```

- [ ] **Step 3: 全功能测试清单**

- [ ] 木鱼：点击有动画、声音、振动、金额变化、粒子、彩蛋
- [ ] 转盘：点击抽奖、旋转动画、弹窗结果、计数器
- [ ] 照片墙：显示照片+配文、点击放大
- [ ] 暖心话语：随机显示、点击切换
- [ ] 管理面板：长按标题→密码→面板、显示统计数据
- [ ] 数据持久化：刷新页面后金额/次数不丢失
- [ ] 手机端：响应式布局、触摸交互正常
- [ ] 飘落花瓣效果

---

### Task 10: 最终提交

**步骤:**

- [ ] **Step 1: 确认所有文件就位**

```bash
ls -la f:/风水师/warm-station/
ls -la f:/风水师/warm-station/images/photos/
ls -la f:/风水师/warm-station/images/flowers/
ls -la f:/风水师/warm-station/images/dogs/
```

- [ ] **Step 2: 测试手机可访问性**（将文件夹发送到手机，用浏览器打开）
