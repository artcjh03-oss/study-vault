# GitHub Pages 筆記美化與網站架設指南

當您的 Markdown 筆記上傳到 GitHub 後，雖然能夠在 GitHub 網頁上閱讀，但排版通常較為單調。

透過 **GitHub Pages** 免費靜態網頁託管服務，結合 **Docsify**、**MkDocs** 或 **Hexo** 這三款主流工具，您可以輕鬆將原本零散的 Markdown 檔案，改造成精美、具備側邊欄、搜尋功能且排版極佳的「個人知識庫網站」或「部落格」。

本指南將為您詳細介紹這三款工具的特色、選擇建議，以及具體的架設與部署步驟。

---

## 🛠️ 三大工具快速評估與選擇

根據您的筆記量與偏好的呈現風格，選擇最適合的工具：

| 工具名稱 | Docsify (最推薦筆記庫) | MkDocs (適合結構化文檔) | Hexo (適合時間軸部落格) |
| :--- | :--- | :--- | :--- |
| **運作原理** | **動態渲染**：網頁載入時即時解析 Markdown，不需預先編譯成 HTML。 | **靜態生成**：在本地將 Markdown 編譯成靜態的 HTML 網頁。 | **靜態生成**：在本地編譯，著重於部落格文章架構與分類。 |
| **優點** | 1. 寫完 MD 檔直接 push 即可更新，超省事。<br>2. 網站輕量，設定極簡。 | 1. Material 主題科技感十足、非常精美。<br>2. 搜尋功能與搜尋引擎優化 (SEO) 極佳。 | 1. 豐富的部落格主題。<br>2. 內建文章時間軸、分類、標籤與留言板功能。 |
| **缺點** | 1. 對搜尋引擎 (SEO) 較不友善。<br>2. 筆記量極大時載入可能稍慢。 | 1. 本地電腦需安裝 Python 環境。<br>2. 每次有新筆記都需要重新執行編譯指令。 | 1. 學習曲線稍高，設定較為繁瑣。<br>2. 本地電腦需安裝 Node.js 環境。 |
| **適用情境** | **個人學習筆記庫、知識庫、小品文檔。** | **技術開發文檔、大型系統說明書、結構化教材。** | **以時間、分類為主軸的技術部落格、個人網誌。** |

---

## 方案 A：使用 Docsify 打造個人即時知識庫（最省時推薦 🌟）

Docsify 不用將 `.md` 檔案編譯成 `.html`，它會直接在網頁端動態解析您的 Markdown 檔案。這意味著您在本地修改完筆記後，**直接 `git push` 上傳，網站就立刻更新完成了！**

### 💻 本地建置步驟
1. **安裝 Docsify 工具**（您的電腦需先安裝 [Node.js](https://nodejs.org/)）：
   打開終端機，執行以下指令安裝全域 CLI：
   ```bash
   npm i docsify-cli -g
   ```
2. **初始化專案**：
   在您的筆記儲存庫目錄下（例如 `study-vault`），建立一個名為 `docs` 的資料夾並初始化：
   ```bash
   docsify init ./docs
   ```
   *初始化後，`docs` 資料夾下會自動產生三個檔案：*
   * `index.html`：網站的入口與所有外掛、樣式的設定檔。
   * `README.md`：網站的首頁內容（您可以直接修改它）。
   * `.nojekyll`：空檔案，用來告訴 GitHub Pages 避開內建的 Jekyll 編譯器。
3. **本地預覽網站**：
   ```bash
   docsify serve ./docs
   ```
   開啟瀏覽器輸入 `http://localhost:3000` 即可預覽網站。

### 🎨 Docsify 美化小撇步
Docsify 的設定都在 `docs/index.html` 中，您可以修改其中的 JS 設定來美化網頁：

* **開啟側邊欄選單**：
  在 `index.html` 的 `window.$docsify` 中加入 `loadSidebar: true`：
  ```javascript
  window.$docsify = {
    name: '我的學習筆記',
    repo: 'https://github.com/您的帳號/儲存庫名稱',
    loadSidebar: true, // 啟用側邊欄
    maxLevel: 4,
    subMaxLevel: 2
  }
  ```
  接著在 `docs` 資料夾下建立一個 **`_sidebar.md`** 檔案，寫入您的目錄大綱：
  ```markdown
  * [首頁](README.md)
  * 📚 學習筆記
    * [0710御三家](0710御三家.md)
    * [手機晶片大廠](手機晶片大廠.md)
    * [Git 基礎知識](GIT的基礎知識.md)
  ```
* **加入熱門插件（如：全文字搜尋、代碼一鍵複製）**：
  將以下 CDN 連結加入至 `index.html` 的 `</body>` 標籤上方：
  ```html
  <!-- 全文字搜尋插件 -->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
  <!-- 代碼一鍵複製插件 -->
  <script src="//cdn.jsdelivr.net/npm/docsify-copy-code/dist/docsify-copy-code.min.js"></script>
  ```

### 🚀 部署至 GitHub Pages
1. 將包含 `docs` 資料夾的專案推送到 GitHub。
2. 進入該 GitHub Repository 頁面，點選 **Settings** -> 左側選單的 **Pages**。
3. 在 **Build and deployment** 底下：
   * **Source** 選擇 `Deploy from a branch`。
   * **Branch** 選擇 `main`（或您預設的分支）。
   * 右側資料夾選單，將原本的 `/ (root)` 改選為 **`/docs`**。
   * 點選 **Save**。
4. 約等待 1-2 分鐘，GitHub 會提供您的專案網址（例如：`https://帳號.github.io/儲存庫/`），您的精美筆記庫就上線了！

---

## 方案 B：使用 MkDocs + Material 主題打造科技感文檔

MkDocs 是基於 Python 的靜態網頁生成器。搭配世界上極受歡迎的 **Material for MkDocs** 主題，能直接讓您的 Markdown 筆記呈現出如同知名開源專案官網般的高科技質感。

### 💻 本地建置步驟
1. **安裝 MkDocs 與 Material 主題**（您的電腦需先安裝 Python 與 pip）：
   ```bash
   pip install mkdocs mkdocs-material
   ```
2. **建立新專案**：
   ```bash
   mkdocs new my-notes-site
   cd my-notes-site
   ```
   *這會產生一個 `mkdocs.yml` 設定檔與一個放 Markdown 檔案的 `docs` 資料夾。*
3. **本地預覽網站**：
   ```bash
   mkdocs serve
   ```
   開啟 `http://127.0.0.1:8000` 即可預覽。

### 🎨 MkDocs 美化設定
編輯根目錄下的 **`mkdocs.yml`** 設定檔：
```yaml
site_name: 我的學習知識庫
theme:
  name: material
  palette:
    # 支援深色模式切換
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: teal
      accent: purple
      toggle:
        icon: material/weather-sunny
        name: 切換至深色模式
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: teal
      accent: purple
      toggle:
        icon: material/weather-night
        name: 切換至淺色模式
  features:
    - navigation.tabs # 頂部導覽分頁
    - search.suggest # 搜尋引擎聯想提示
    - content.code.copy # 代碼複製按鈕

# 定義側邊欄目錄結構
nav:
  - 首頁: index.md
  - 🎮 遊戲與AI: 0710御三家.md
  - 📱 行動科技: 手機晶片大廠.md
  - 🛠️ Git工具: GIT的基礎知識.md
```

### 🚀 部署至 GitHub Pages
MkDocs 內建了一鍵部署至 GitHub Pages 的超方便指令：
1. 確保您的本地專案已與 GitHub 遠端儲存庫連結。
2. 在終端機執行：
   ```bash
   mkdocs gh-deploy
   ```
   *這項指令會自動在本地將檔案編譯成 HTML，並在背後建立一個名為 `gh-pages` 的分支推送到 GitHub。*
3. 進入 GitHub Repository 的 **Settings** -> **Pages**。
4. 將 **Branch** 設定為 **`gh-pages`**，路徑選擇 `/ (root)`，點選 **Save** 即部署完成！

---

## 方案 C：使用 Hexo 搭建個人技術部落格

Hexo 是基於 Node.js 的靜態部落格生成框架，擁有數百種精美的部落格主題。如果您寫筆記的目的是想以「發表個人文章」、「技術部落格」的形式呈現，Hexo 是首選。

### 💻 本地建置步驟
1. **安裝 Hexo CLI**（需先安裝 Node.js）：
   ```bash
   npm install hexo-cli -g
   ```
2. **初始化部落格專案**：
   ```bash
   hexo init my-blog-site
   cd my-blog-site
   npm install
   ```
3. **寫新文章與本地預覽**：
   * 建立新文章：
     ```bash
     hexo new "我的第一篇技術筆記"
     ```
     這會在 `source/_posts/` 資料夾下產生一個 `.md` 檔案，您可以用編輯器打開並開始寫內容。
   * 本地預覽：
     ```bash
     hexo server
     ```
     開啟 `http://localhost:4000` 預覽網站。

### 🎨 Hexo 美化（更換主題，例如 Fluid 主題）
1. 安裝極簡好康的 `Fluid` 主題：
   ```bash
   npm install hexo-theme-fluid --save
   ```
2. 修改根目錄下的 **`_config.yml`** 設定檔：
   ```yaml
   theme: fluid # 將預設主題 landscape 改為 fluid
   ```
3. 根據主題的官方說明文件，微調 `_config.fluid.yml` 以自訂您的網站標題、大頭貼、留言板等細節。

### 🚀 部署至 GitHub Pages
1. 安裝 Hexo 的 Git 自動部署套件：
   ```bash
   npm install hexo-deployer-git --save
   ```
2. 修改根目錄下的 **`_config.yml`** 檔案的最底部部署設定：
   ```yaml
   deploy:
     type: git
     repo: https://github.com/您的帳號/您的儲存庫名稱.git
     branch: gh-pages
   ```
3. 一鍵生成並部署網站：
   ```bash
   hexo clean && hexo generate -d
   ```
   *此指令會清除舊檔案、編譯全新的 HTML，並自動推送到 GitHub 的 `gh-pages` 分支。*
4. 進入 GitHub Repository 的 **Settings** -> **Pages**，將 **Branch** 改選為 **`gh-pages`** 分支，儲存即可。

---

## 💡 總結建議

* **如果您想要最簡單、寫完 Markdown 就能直接自動同步更新** 👉 選擇 **Docsify**。
* **如果您追求極致美觀的排版、內建強大的搜尋，且筆記多為技術文檔結構** 👉 選擇 **MkDocs (搭配 Material 主題)**。
* **如果您想要有時間軸、分類、標籤、精美的首頁，當作個人部落格經營** 👉 選擇 **Hexo**。
