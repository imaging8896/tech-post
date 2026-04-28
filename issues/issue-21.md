---
issue_number: 21
title: "悲報：Copilot不再能夠一個 request 吃到飽"
author: imaging8896
created_at: 2026-04-28T14:13:19Z
status: generated
---

# 悲報：Copilot不再能夠一個 request 吃到飽

**Issue #21**
**Author:** imaging8896
**Created:** 2026-04-28T14:13:19Z

## Description

**Context:**
之前藉由 agent call sub-agent 的方式，讓一個 Github Copilot premium request 做出多個 model(甚至是更貴的 model)多 token 的操作，等於是一個 request 做完4~5件工作。

<img width="915" height="309" alt="Image" src="https://github.com/user-attachments/assets/1150e936-fa40-42ca-8104-63e5a3bf73e4" />

**Requirements:**
你只需要將所有 sub agents 讓主 agent 知道，讓主 agent 去分派工作和統合結果。

**Design Decisions:**
本來一個 agent 可以完成一件複雜的事情，現在可以拆分細一些，例如事情當中的子工作可以發包由 agent A 來做，來讓處理子工作時的 agent context 是乾淨的，並且可以根據子工作的複雜程度選用更高階的 model。

**Implementation:**
這種複雜架構，建議由 Copilot 產生 agent.md

**Trade-offs:**
目前根據官網，本來訂閱 pro 用戶擁有 300 request per month，將改成 1000 AI credits per month(1 AI credit = $0.01 USD)，但其實看 credit 沒有意義，他就是百萬 token 計費，跟 Claude API 一模一樣，pro 每月訂閱費10美金所以是1000 AI credits...根本一樣，月訂閱的朋友6/1即刻生效，年訂閱的朋友6/1要是不選擇馬上轉成新計費模式，model給你倍數成長，我最常用的 Sonnet 4.5 從 1 變成 6，Sonnet 4.6 從 1 變成 9...
https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-individuals

**Results:**
這樣遊戲規則說變就變，讓我對微軟的品質也要打個問號，接下來訂閱到期前會再來評估跟直接的 Claude API 花費比較，並改 model 的供應商，畢竟 agent file 是不用變的，只要改 model，能做的是情不會改變。

