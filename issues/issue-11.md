---
issue_number: 11
title: "在AI寫程式時代，還需要coding standard嗎？"
author: imaging8896
created_at: 2026-02-17T12:05:40Z
status: generated
---

# 在AI寫程式時代，還需要coding standard嗎？

**Issue #11**
**Author:** imaging8896
**Created:** 2026-02-17T12:05:40Z

## Description


在AI時代，工程師從寫程式變成請AI寫程式，在這個前提下，coding standard 還重要嗎？
從Gemini 給的答案「更為重要」
但它的「存在形式」和「關注重點」已經發生了劇變。
​為什麼 Coding Standard 依然必要？
​即使程式碼是 AI 寫的，最終維護程式碼、負責 Debug、承擔系統崩潰責任的人依然是工程師。
​1.作為 AI 的「提示指令 (Prompt Guide)」： AI 就像一個博學但沒原則的實習生。如果你不給它規範（例如：提供 .editorconfig 或團隊的風格指南），它每次給你的風格可能都不一樣（一下用 Tab，一下用 Space）。統一的規範能讓 AI 輸出的結果更符合預期。
2. ​降低 Review 的認知負荷： 如果 AI 產生的程式碼風格混亂，人類在 Review PR 時會花費大量精力在糾正縮排、命名等瑣事上，進而忽略了核心的邏輯錯誤或資安漏洞。
3. ​程式碼的「可閱讀性」大於「可撰寫性」： 在 AI 時代，寫程式的速度已經不是瓶頸，讀程式的速度才是。規範是為了讓團隊成員在不需要 AI 解釋的情況下，也能快速看懂邏輯。

至於如何在repository給AI 定義coding standard ，就是接下來的議題
    
