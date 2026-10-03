# 資訊管理課程－科技接受度研究
## Technology Acceptance Research Interactive Website

這是一個可直接部署到 **GitHub Pages / Vercel / Netlify** 的單頁互動式研究網站。

網站目前整合六大研究主題：

1. 接受度意圖（Acceptance Intention）
2. 接受度行為（Acceptance Behavior）
3. 滿意度（Satisfaction）
4. 持續使用（Continuance Intention）
5. 行為改變（Behavior Change）
6. 關鍵成功因素（Critical Success Factors）

每個主題皆以 Top 10 理論／模型／框架呈現，並搭配 Mermaid 模型圖。

---

## 主要功能

- 三區塊介面：
  - 上方 Header
  - 左下主題導覽
  - 右下動態內容
- 四語系切換：
  - 繁體中文
  - English
  - 日本語
  - 한국어
- 切換語系時同步更新：
  - 頁面標題
  - 選單
  - 主題說明
  - 模型名稱
  - 核心構念
  - Mermaid 原始語法
  - **Mermaid 實際渲染圖形**
- 多種網站色系
- 模型搜尋
- 響應式設計
- 不需要後端伺服器

---

## 檔案說明

```text
technology_acceptance_research_github/
├── index.html
├── README.md
├── SKILL.md
├── prompt_template.txt
├── interactive_multilingual_research_site_skill.zip
└── .nojekyll
```

### index.html
正式網站首頁。  
GitHub Pages 會直接將 `index.html` 當作入口頁面。

### README.md
本專案說明文件。

### SKILL.md
這次網站成功製作流程整理出的可重複使用 Skill。

### prompt_template.txt
未來建立其他相同架構網站時，可直接複製修改的 Prompt 範本。

### interactive_multilingual_research_site_skill.zip
將 Skill 與相關說明打包，方便保存及日後重複使用。

### .nojekyll
讓 GitHub Pages 直接以靜態網站方式發布檔案，不經 Jekyll 處理。

---

# GitHub Pages 發布方式

## 方法 A：直接上傳

1. 在 GitHub 建立新的 Repository。
2. Repository 建議名稱：
   `technology-acceptance-research`
3. 將本資料夾內所有檔案上傳到 Repository 根目錄。
4. 進入：

   `Settings → Pages`

5. 在 **Build and deployment** 選擇：

   - Source：`Deploy from a branch`
   - Branch：`main`
   - Folder：`/ (root)`

6. 按下 Save。

GitHub Pages 完成後，網址通常會是：

```text
https://你的GitHub帳號.github.io/technology-acceptance-research/
```

---

# Mermaid 說明

本網站使用 Mermaid 11 CDN：

```text
https://cdn.jsdelivr.net/npm/mermaid@11/
```

因此使用 Mermaid 圖形時需要網路連線。

本網站的多語系 Mermaid 採用「重新渲染」方式：

1. 使用者切換語言
2. 重新產生該語言 Mermaid 語法
3. 清除舊 SVG
4. 重新執行 `mermaid.render()`
5. 使用 render sequence 防止舊的非同步渲染覆蓋新語系

這是確保圖形本身能真正同步切換語系的關鍵。

---

# 後續可擴充

此架構可以延伸成：

- AI 接受度研究
- 數位轉型研究
- 行銷理論資料庫
- EMBA 課程網站
- 管理學模型資料庫
- 教育訓練網站
- 企業產品知識網站
- 多語系理論比較網站

新增研究主題時，主要只需擴充 JavaScript 的 `DATA`。

新增語言時，主要擴充：

- UI locale
- 模型翻譯
- Mermaid terminology map

---

## 技術

- HTML5
- CSS3
- Vanilla JavaScript
- Mermaid.js 11
- Responsive Web Design

---

## 使用方式

直接用瀏覽器開啟 `index.html` 即可預覽。

若部署到 GitHub Pages，請保留 `index.html` 檔名。
