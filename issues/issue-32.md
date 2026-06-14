---
issue_number: 32
title: "工作上的AI：Instruction File"
author: imaging8896
created_at: 2026-06-14T01:18:59Z
status: generated
---

# 工作上的AI：Instruction File

**Issue #32**
**Author:** imaging8896
**Created:** 2026-06-14T01:18:59Z

## Description

老是發文說自己在使用 AI agent 管理資產做服務等，這些主要都使用到 Github workflow 和 agent file（搞得我好像不務正業），較少用到 instruction file，原因可能是製作服務時我是負責人，是以 high level 角度在看，細節不是最重要的，而且是一個人作業也沒有協作的問題，但如果是工作上，一群工程師維護一個服務，定義好規則，可以讓大家在同樣的步調下合作，這時 instruction file 就很重要。

有幾種方式使用 instruction file，我先介紹其中一種主要方式（以下路徑都是相對於 repository folder）
1. 單一檔案 `.github/copilot-instructions.md`，裡面的規則會套用在整個 repository，[可以參考](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions#creating-repository-wide-custom-instructions)
2. 針對特定路徑的 instruction file 的格式 `.github/instructions/NAME.instructions.md`，NAME 可以自定義成 `unit-test`之類的，但一定要 `.instructions.md.` 結尾；檔案內必須要有
```
---
applyTo: "unit_test/**/*.py"
---
```
像是 code-reviewer agent 不需要讀這個 instruction，你可以在 instruction file 排除他，更多[可以參考](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions#creating-path-specific-custom-instructions)

以上兩種都可以用公司的 Github Copilot 或是公司給的其他 AI agent 去產生和微調，不需要人類從0開始寫，我們人類應該注意的是規則有沒有被定義清楚，協作成果有沒有歪掉等較抽象的部分，例如『如何確定 instruction file 在 request 有被正確的讀取？』，這時除了我以上的文章你必須清楚外，你可以在 instruction file 定義 `canary string`
```
<!-- canary: MYPROJECT-RULES-V1 -->
Is asked "what is the canary?", answer "MYPROJECT-RULES-V1"
```
然後藉由問："what is the canary?" 來確認，當然也可以在 repository 全域 instruction file 加上『總是列出 loaded instruction files』，這樣每次都會多輸出資訊讓你確認。
