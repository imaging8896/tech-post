---
issue_number: 25
title: "AI資產管理"
author: imaging8896
created_at: 2026-05-29T16:09:59Z
status: generated
---

# AI資產管理

**Issue #25**
**Author:** imaging8896
**Created:** 2026-05-29T16:09:59Z

## Description

目前個人投資方面都是VT, 005為大宗，於是想到AI應該可以幫我管理資產，於是請 Claude 一小時內建立起來

<img width="206" height="389" alt="Image" src="https://github.com/user-attachments/assets/395529e7-6a54-4b37-98e4-b2c872cf2938" />

一開始輸入資產吱吱訓後，每天都會有一個 CEO agent 去更新資產價格（部分像房地產仍然得手動更新價格）、檢視資產、研究資產內容、做出買賣決定和產生報告，並會有通知到 Discord 的機制，讓我一大早就知道資產的風險和今天的行動，這一切都是 Github Action 來處理，資料也存在 private Github repository 上，完全不需要在個人電腦上運作

<img width="605" height="581" alt="Image" src="https://github.com/user-attachments/assets/ac6e5f3f-8aa9-4eb3-9579-9c6687e73506" />

<img width="649" height="344" alt="Image" src="https://github.com/user-attachments/assets/fbc1a2b5-755f-4816-8d69-ca0b00b1f9f9" />

Agent 建議的操作也有詳細的原因和資料佐證

<img width="729" height="323" alt="Image" src="https://github.com/user-attachments/assets/b51d1bac-36fd-4647-b301-83ac0a4758d8" />


經過調整成本（透過 Claude），CEO agent 使用 Claude Sonnet 4.6 API(max 40 turns)，大約是1.8USD，加上其他便宜 model 處理雜事，大約60塊錢每日，目前第一週，持續觀察成效和找到降低成本的方式。
