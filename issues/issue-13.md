---
issue_number: 13
title: "AI時代，哪些軟體開發規範不再必要或正在改變？"
author: imaging8896
created_at: 2026-02-22T05:18:11Z
status: generated
---

# AI時代，哪些軟體開發規範不再必要或正在改變？

**Issue #13**
**Author:** imaging8896
**Created:** 2026-02-22T05:18:11Z

## Description

​1. 改變中：從「怎麼寫」轉向「怎麼結構化」
​以前的規範重點在於寫法（Syntax sugar），現在的重點在於模組化與介面設計。
​現狀： 規範現在更強調 Type Safety (型別安全)。因為 AI 在處理強型別（如 TypeScript, Rust, Go）時，產出的程式碼準確度遠高於弱型別。設定嚴格的型別規範，是為了「約束 AI」不要亂跑邏輯。
​2. 消失中：繁瑣的手寫文件與註解 (Boilerplate Comments)
​以前要求每個 Function 都要寫詳細的 JSDoc 或註解。
​現狀： AI 非常擅長解釋程式碼。比起寫死的文件，現在更強調程式碼的自我解釋性 (Self-documenting)。如果程式碼看不懂，請 AI 重構到看懂為止，而不是寫一大堆註解來補救。
​3. 新增的規範：AI 協作規範 (AI Interoperability)
​這是一個全新的領域。團隊現在需要規範：
​Prompt 追蹤： 複雜邏輯是否要在註解中留下當時使用的 Prompt？
​AI 生成標記： 哪些區塊是 AI 生成且未經大幅修改的？
​安全性掃描： 規範中必須強制要求 AI 生成的 Code 必須經過特定安全性工具（如 Snyk, SonarQube）的自動化掃描。
