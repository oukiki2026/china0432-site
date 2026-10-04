# 圖片管線規範（assets/images/）

本站走極致輕量路線：圖片是影響載入速度的最大變數，務必遵守以下規範。

## 1. 存放位置

- 正式圖片放在 `static/images/<欄目 slug>/`，例如 `static/images/travel-culture/`。
- `assets/images/` 僅放本 README（規範文件）；如需 Hugo Pipes 做圖片處理（縮放/轉 WebP），
  可將原圖改放 `assets/images/<欄目 slug>/originals/`，模板再用 `resources.Get` 處理。
  目前模板為最簡渲染（直接引用 `image` 路徑），暫不需要 Pipes。

## 2. 命名規範

`<欄目>-<文章 slug>-<兩位序號>.<副檔名>`，全小寫英文、連字號分隔。

範例：

```
travel-culture-songhua-rime-01.webp
travel-culture-songhua-rime-02.webp
old-photos-songhua-1930s-01.jpg
```

- 同一篇文章的多張圖用 `01 / 02 / 03` 區分。
- 老照片掃描件保留年份資訊：`old-photos-beishan-1980s-01.jpg`。

## 3. 格式與體積

- 優先 **WebP**（照片類）；老照片掃描件若需保留細節可用 JPEG（品質 80）。
- 寬度不超過 **1600px**，單張不超過 **300KB**（目標 < 150KB）。
- 建議工具：`cwebp` / Squoosh / Hugo Pipes（`resources.Get | images.Resize`）。
- 模板已加 `loading="lazy"`，首屏以外的圖片延遲載入。

## 4. 來源註明（強制）

每篇文章的 front matter 必須填寫：

```yaml
image: "images/travel-culture/travel-culture-songhua-rime-01.webp"
image_alt: "2024 年 1 月拍攝的松花江霧凇"   # 無障礙替代文字
image_source: "作者實拍，2024 年 1 月於霧凇島"  # 來源註明
```

來源註明會渲染在圖片下方的 `<figcaption>`。**只用三類圖**：

1. 自家收藏 / 實拍（註明拍攝者與時間）
2. 公有領域圖庫（註明圖庫名稱與連結）
3. 獲得授權的圖片（註明授權方）

不明來源的網路圖片一律不用。

## 5. 佔位圖

`static/images/placeholder.svg` 為建站範例用的佔位圖，正式發佈前必須替換。
