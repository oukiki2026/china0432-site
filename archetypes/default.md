---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
draft: true
description: "一句話摘要（用於列表頁與 SEO，建議 60–120 字）"
tags: []
# 配圖規範（見 assets/images/README.md）：
# image: 原圖路徑；image_source: 來源註明（必填，有圖就必須寫）
image: "images/placeholder.svg"
image_alt: ""
image_source: ""
sample: false
---

<!-- 正文用 Markdown 撰寫。發佈前檢查清單：
  1. draft 設為 false
  2. 人工核驗史實/數據，拒絕 AI 虛構
  3. 配圖已替換為實拍或有明確來源的圖片，image_source 已填寫
  4. sample 保持 false
-->
