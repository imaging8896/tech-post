---
issue_number: 16
title: "使用 AI 爬資料"
author: imaging8896
created_at: 2026-03-09T14:01:07Z
status: generated
---

# 使用 AI 爬資料

**Issue #16**
**Author:** imaging8896
**Created:** 2026-03-09T14:01:07Z

## Description

以前抓資料的方式
1. 瀏覽網站，找到某個網站上有需要的資料
2. 打開瀏覽器開發工具，看讀取網頁時有無使用API
3. 沒有API時就採用 html 分析

現在：
1. 問AI可能的資料來源，AI可能就會給你可用的API or 網頁
2. 若是AI沒有給予有效的來源，一樣你瀏覽網站，找到某個網站上有需要的資料
3. 把網站的網址餵給 Github Copilot
4. Copilot 進行以下步驟
    1. 嘗試讀取網頁找尋API
    2. 無API，讀取 HTML
    3. HTML 沒有(表示是 javascript 動態打API產生)，讀取 javascript 並嘗試執行
    4. 找到隱藏在 javascript 內的 API
    5. javascript 內還是找不到 API，分析執行完 javascript 的 HTML

即使在 AI 時代，資料仍是使用 AI 時重要的上下文，可以讓 AI 更準確回答問題和研究。
