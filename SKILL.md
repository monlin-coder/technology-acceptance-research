# Skill: Interactive Multilingual Research Website Generator

## 目的

建立可直接部署的「單一 HTML 互動式研究網站」，支援：

- 三區塊版面
- 多研究主題
- Top-N 模型／理論／框架
- Mermaid 模型圖
- 多語系切換
- Mermaid 圖形同步切換語言
- 色系切換
- 搜尋／篩選
- 響應式版面
- GitHub Pages / Vercel 發布

---

## 網站標準架構

### Frame 1：上方 Header

包含：

- 網站／課程主標題
- 副標題
- 色系選擇
- 語言選擇

建議至少支援：

- 繁體中文
- English
- 日本語
- 한국어

---

### Frame 2：左下導覽

使用垂直主題選單。

例如：

- 接受度意圖
- 接受度行為
- 滿意度
- 持續使用
- 行為改變
- 關鍵成功因素

點擊後更新右側內容。

---

### Frame 3：右下內容

針對所選主題顯示：

- 主題名稱
- 主題說明
- 搜尋框
- Top 10 模型
- 模型縮寫
- 模型中英文名稱
- 核心構念
- Mermaid 圖
- Mermaid 原始語法

---

# 資料結構

建議：

```javascript
const DATA = {
  topic_key: {
    zh: "中文主題",
    en: "English Topic",
    intro_zh: "中文說明",
    intro_en: "English description",
    models: [
      [
        "MODEL",
        "中文名稱",
        "English Model Name",
        "中文核心構念",
        "English core constructs",
        `flowchart LR
A[節點A] --> B[節點B]`
      ]
    ]
  }
};
```

日文與韓文可以用獨立 Locale Dictionary 維護。

---

# 多語系要求

切換語言後，下列項目必須同步更新：

1. 網站主標題
2. 副標題
3. 色系標籤
4. 語言標籤
5. 左側選單
6. 主題名稱
7. 主題說明
8. 搜尋框 Placeholder
9. 模型名稱
10. 核心構念
11. Mermaid 原始語法
12. Mermaid 實際渲染圖

---

# Mermaid 多語系關鍵技術

## 不可只更換原始碼

Mermaid 一旦完成渲染，就會成為 SVG。

因此：

> 改 Mermaid 字串 ≠ 已存在 SVG 自動改變。

語言切換時必須重新建立 SVG。

---

## Mermaid 詞彙映射

```javascript
const MERMAID_TERM_MAP = {
  en: {
    "績效期望": "Performance Expectancy",
    "接受意圖": "Acceptance Intention"
  },
  ja: {
    "績效期望": "業績期待",
    "接受意圖": "受容意図"
  },
  ko: {
    "績效期望": "성과기대",
    "接受意圖": "수용의도"
  }
};
```

---

## Mermaid 語法轉換

```javascript
function localizeMermaid(code, lang){
  if(lang === 'zh') return code;

  const map = MERMAID_TERM_MAP[lang];
  if(!map) return code;

  let out = code;

  Object.keys(map)
    .sort((a,b) => b.length - a.length)
    .forEach(k => {
      out = out.split(k).join(map[k]);
    });

  return out;
}
```

---

# Mermaid 動態重新渲染

推薦：

```javascript
let mermaidRenderSeq = 0;

async function renderMermaid(){
  const seq = ++mermaidRenderSeq;

  const nodes =
    [...document.querySelectorAll('.diagram-render')];

  for(let i = 0; i < nodes.length; i++){
    const node = nodes[i];

    const code =
      decodeURIComponent(node.dataset.code || '');

    const id =
      `mmd-${currentLang}-${seq}-${i}`;

    const result =
      await window.mermaid.render(id, code);

    if(seq !== mermaidRenderSeq){
      return;
    }

    node.innerHTML = result.svg;

    if(result.bindFunctions){
      result.bindFunctions(node);
    }
  }
}
```

---

# 語言切換

```javascript
langSelect.addEventListener(
  'change',
  async e => {

    currentLang = e.target.value;

    document.documentElement.lang =
      currentLang === 'zh'
      ? 'zh-Hant'
      : currentLang;

    mermaidRenderSeq++;

    cards.innerHTML = '';

    updateStaticUI();

    await render();
  }
);
```

---

# 為什麼要有 Render Sequence？

因為 Mermaid SVG 渲染是非同步的。

若使用者快速切換：

```text
中文 → 英文 → 日文
```

英文圖可能比日文圖晚完成。

若沒有 sequence guard：

> 英文 SVG 可能最後蓋掉日文 SVG。

所以需判斷：

```javascript
if(seq !== mermaidRenderSeq) return;
```

---

# Mermaid 初始化

```html
<script type="module">
import mermaid from
'https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs';

window.mermaid = mermaid;

mermaid.initialize({
  startOnLoad: false,
  securityLevel: 'loose',
  theme: 'base',
  flowchart: {
    curve: 'basis',
    htmlLabels: true
  }
});
</script>
```

重點：

```text
startOnLoad: false
```

因為我們要自行控制 Mermaid 重新渲染。

---

# 色系

建議至少：

- Indigo
- Emerald
- Rose
- Amber
- Slate

使用 CSS Variables：

```css
:root {
  --bg:#f5f7fb;
  --panel:#ffffff;
  --text:#1f2937;
  --muted:#667085;
  --primary:#3458d4;
  --primary-2:#e8edff;
  --border:#d9e1ec;
}
```

---

# 搜尋功能

搜尋應涵蓋：

- 模型縮寫
- 中文名稱
- 英文名稱
- 日文名稱
- 韓文名稱
- 核心構念
- 關鍵字

---

# 響應式

Desktop：

```text
HEADER
--------------------------
LEFT MENU | RIGHT CONTENT
```

Mobile：

```text
HEADER
MENU
CONTENT
```

---

# GitHub 發布標準

輸出至少：

```text
index.html
README.md
SKILL.md
prompt_template.txt
.nojekyll
```

首頁檔案一定命名：

```text
index.html
```

如此可以直接使用 GitHub Pages。

---

# QA Checklist

## Layout

- [ ] Header 正常
- [ ] 左側選單正常
- [ ] 右側內容切換正常
- [ ] 手機版正常

## Language

- [ ] 繁體中文
- [ ] English
- [ ] 日本語
- [ ] 한국어
- [ ] 模型名稱切換
- [ ] 核心構念切換
- [ ] Mermaid Source 切換
- [ ] Mermaid SVG 真正重新渲染

## Interaction

- [ ] Theme selector
- [ ] Search
- [ ] Navigation
- [ ] Details / Mermaid source

## Publishing

- [ ] index.html
- [ ] UTF-8
- [ ] GitHub Pages 可直接部署
- [ ] Mermaid CDN 正常

---

# 最重要的成功經驗

多語系 Mermaid 網站的核心不是單純翻譯。

真正關鍵是：

> **Language switch → Localize Mermaid source → Delete old SVG → Mermaid.render() → Insert new SVG**

並搭配 render sequence 防止 asynchronous race condition。

這個做法應作為所有未來多語系 Mermaid 網站的標準模式。
