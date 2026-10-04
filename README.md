# china0432.com — 吉林家鄉文化雙語站

Hugo 靜態站骨架。預設語言 **繁體中文（zh-hant）**，第二語言 **English**。
技術路線：GitHub 倉庫 → Cloudflare Pages 自動部署，零伺服器成本。

## 目錄結構

```
china0432-site/
├── hugo.yaml                 # 站點配置（多語言、六個欄目、輸出格式）
├── archetypes/default.md     # 新文章模板（含 front matter 規範與發佈檢查清單）
├── assets/
│   ├── css/style.css         # 手寫極簡樣式（響應式，全站零 JavaScript）
│   └── images/README.md      # 圖片管線規範（命名 / 格式 / 來源註明）
├── static/images/            # 靜態圖片（placeholder.svg 為範例佔位圖）
├── content/
│   ├── zh-hant/              # 繁體中文內容（_index.md 首頁 + 六個欄目）
│   └── en/                   # English 內容（同構）
└── layouts/
    ├── _default/baseof.html  # 頁面骨架
    ├── home.html             # 首頁（欄目卡片導航）
    ├── _default/list.html    # 欄目列表頁
    ├── _default/single.html  # 文章頁（含 SAMPLE 標記、配圖來源註明）
    ├── partials/            # head（含 hreflang）/ header（含語言切換器）/ footer
    └── robots.txt            # 自訂 robots.txt（含 Sitemap 指向）
```

六個欄目 slug：`old-photos`（老照片）、`place-names`（老地名）、`dialect`（方言俚語）、
`heritage-taste`（老字號與味道）、`travel-culture`（文化旅遊）、`practical-info`（實用資訊）。

SEO 就緒：每頁 `<link rel="alternate" hreflang="zh-Hant|en|x-default">`、
Hugo 內建 `sitemap.xml`（雙語 URL 全收錄）、`robots.txt`、每語言獨立 RSS
（`/` → `/index.xml`，`/en/` → `/en/index.xml`）。

## 本地預覽

需要 Hugo extended 版（≥ 0.120）：

```bash
# 安裝（Linux/macOS 二進位，見 https://github.com/gohugoio/hugo/releases）
hugo version   # 確認含 "+extended"

# 進入專案目錄
cd china0432-site

# 本地預覽（含草稿）
hugo server -D
# 瀏覽器開 http://localhost:1313/（繁體首頁）與 http://localhost:1313/en/（英文首頁）

# 正式構建（與線上一致）
hugo --minify
```

## 推送到 GitHub

```bash
cd china0432-site
git init
git add .
git commit -m "init: china0432.com Hugo 雙語站骨架"
# 在 GitHub 新建空倉庫（建議名 china0432-site），不要勾選 README/.gitignore
git remote add origin git@github.com:<你的帳號>/china0432-site.git
git branch -M main
git push -u origin main
```

之後每次更新內容：`git add . && git commit -m "..." && git push`，Cloudflare Pages 會自動重新部署。

## Cloudflare Pages 接入（推薦：Pages 內建 Git 整合自動部署）

1. 登入 Cloudflare Dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**，
   授權並選擇 `china0432-site` 倉庫。
2. 構建設置：
   - **Framework preset**：`Hugo`（或選 None，手動填下面兩項）
   - **Build command**：`hugo --minify`
   - **Build output directory**：`public`
   - **Root directory**：`/`（倉庫根目錄即專案根目錄）
3. 環境變數（**重要**，否則 Pages 預設的舊版 Hugo 可能構建失敗）：
   - `HUGO_VERSION` = `0.167.0`（或更新的 extended 版號）
4. **Custom domain**：Add custom domain → 輸入 `china0432.com`，
   按提示在該域名的 DNS 區加 CNAME（同帳號下通常一鍵完成），開啟 **Enforce HTTPS**。
5. 部署完成後：提交 sitemap（`https://china0432.com/sitemap.xml`）到 Google Search Console；
   如需 GA，在 `layouts/partials/head.html` 加追蹤碼（保持輕量，建議用 CF Web Analytics，零 JS）。

備選方案：也可用 **GitHub Actions**（checkout → peaceiris/actions-hugo → `hugo --minify` →
上傳 artifact）再由 Pages 接管，但 Pages 內建 Git 整合已足夠，多一套流程只增維護成本，
故不採用。

## 內容生產流程（與管線對應）

1. `hugo new <欄目>/<slug>.md`（會套用 archetypes 模板；中文稿放 `content/zh-hant/`）
2. 寫中文原創稿 → AI 翻譯潤色英文版放 `content/en/` 對應欄目（不對稱策略：英文優先做旅遊/實用欄目）
3. 人工核驗 → 配圖按 `assets/images/README.md` 規範放置並填 `image_source`
4. `draft: false` → `git push` → Pages 自動上線
