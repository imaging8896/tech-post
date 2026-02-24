---
issue_number: 14
title: "在 AI 協作開發的時代，建立 Coding Standard 的核心思維是：「機器能做的交給自動化，人類只看邏輯與架構。」"
author: imaging8896
created_at: 2026-02-24T02:35:24Z
status: generated
---

# 在 AI 協作開發的時代，建立 Coding Standard 的核心思維是：「機器能做的交給自動化，人類只看邏輯與架構。」

**Issue #14**
**Author:** imaging8896
**Created:** 2026-02-24T02:35:24Z

## Description

​範例，為了讓團隊能高效審查 AI 生成的 Python 與 Terraform 程式碼，我建議採用以下四層架構的自動化治理體系：
1. 第一層：機器強制執行 (Machine Enforcement)
​不要在 PR 中浪費時間討論縮排或變數命名。利用 Pre-commit hooks，確保 AI 生成的程式碼在進入 Git 之前就已經被「校正」過。
​推薦工具組合：
​Python: * Ruff: 目前最推薦的工具，速度極快，集成了 Linter 和 Formatter（可取代 Flake8, Isort, Black）。
​MyPy: 強制型別檢查。AI 在有 Type Hints 的環境下生成的程式碼準確度更高。  
​Terraform:
​terraform fmt: 基本格式化。
​TFLint: 檢查雲端服務的最佳實踐（例如：是否遺漏了必填標籤）。  
​Checkov 或 tfsec: 這是最關鍵的，用於檢查 AI 是否生成了具備資安風險的設定（例如：S3 Public Access）。
​2. 第二層：給 AI 的指令說明書 (AI Context)
​AI 之所以會產出不符規範的 Code，通常是因為它不知道你們的「家法」。現在主流的 AI 工具都支援專案層級的規則設定。
​實作方式：
​在專案根目錄建立以下檔案，讓 AI (如 Cursor, GitHub Copilot) 自動讀取：
​.cursorrules 或 .github/copilot-instructions.md:
​範例內容：「所有 Python 函式必須包含 Type Hints。」、「Terraform 必須使用 Remote Backend，且所有 Resource 必須包含 Environment 標籤。」
​.editorconfig: 統一不同編輯器間的縮排與編碼。  
​3. 第三層：架構決策紀錄 (ADR)
​AI 擅長寫細節，但缺乏對整體架構的理解。與其寫厚厚的開發規範，不如使用 ADR (Architecture Decision Records)。
​規範做法： 記錄「為什麼」我們選擇這個方案。
​對 AI 的幫助： 當你把 ADR 餵給 AI 時，它能理解例如「我們為何選擇 Python FastAPI 而非 Flask」，從而在生成程式碼時符合架構脈絡。
​4. 第四層：結構化 PR 範本 (Efficient Review)
​為了讓 Reviewer 快速進入狀況，必須強制執行結構化的 PR 說明。因為程式碼是 AI 寫的，開發者的責任是證明這段 Code 是安全的。
建議的 PR 範本 (Pull Request Template)：
## 變更摘要
## AI 驗證清單
- [ ] 邏輯確認：我已逐行閱讀並理解 AI 生成的邏輯。
- [ ] 安全掃描：Checkov/TFLint 已通過。
- [ ] 測試涵蓋：AI 是否生成了對應的單元測試？

## 關鍵風險點

