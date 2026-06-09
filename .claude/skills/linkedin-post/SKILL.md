---
name: linkedin-post
description: 為 tech-post 專案產生／改寫具吸引力的 LinkedIn 技術貼文（繁中，專有名詞英文）。整合本專案 issues → posts 的 AI 自動化流程與 config/stop-slop-guidelines.md。使用時機：使用者說「寫一篇 LinkedIn」、「改寫這篇 post」、「幫 issue 起稿」、「讓 post 更有吸引力」、「review 這篇」、「優化 hook」、「這篇太 AI 味」、「重寫 post-XX」、「把 issue 變成貼文」、「post 沒人看」、「LinkedIn 流量低」時觸發；即使只說「改一下這篇」、「幫我潤稿」、「重寫」也應觸發（若上下文是 posts/ 目錄內的檔案）。
---

# linkedin-post

協助 tech-post 專案產生 / 改寫 LinkedIn 技術貼文。LinkedIn 是專業 B2B 平台，演算法、文化、長度限制都跟 FB / Threads 不同 — 本 skill 專為這個情境設計。

## 階段路由

| 觸發 | 階段 | 讀 |
|---|---|---|
| 「把 issue X 變貼文」「幫 issue 起稿」 | **G1 生成** | `issues/issue-X.md` + `references/hook_formulas.md` + `references/linkedin_playbook.md` + `../../../config/stop-slop-guidelines.md` |
| 「改寫 post-X」「這篇太 AI 味」「讓 post 更吸引人」 | **G2 改寫** | 目標 `posts/post-X-*.md` + `references/hook_formulas.md` + `../../../config/stop-slop-guidelines.md` |
| 「review 這篇」「這篇好不好」「能再強嗎」 | **G3 診斷** | 目標 post + `references/linkedin_playbook.md`（評估段） + `references/hook_formulas.md`（4 條件） |
| 「優化 hook」「前 2 行不夠抓人」 | **G4 hook 微調** | 目標 post 前 5 行 + `references/hook_formulas.md` |
| 「我寫 LinkedIn 的風格是？」「學我的語氣」 | **G5 風格摘要** | 掃 `posts/*.md`（status: published）最近 5-8 篇 → 摘要語氣 |

每次進入階段前用一句話告知使用者要做什麼，給糾正機會。

## 先決條件

- 工作目錄在 `tech-post/` 專案根（檢查 `posts/` `issues/` `config/stop-slop-guidelines.md` 存在）
- 若使用者指定 issue 號 / post 檔，先確認檔案存在；找不到列最近 5 個 issue / post 讓使用者選

## 🛡️ 不可覆寫硬規則

1. **永遠繁體中文，專有名詞用英文**（與本專案 `capture-issue.yml` 一致）
2. **絕不擅自寫入 `posts/` 或 `issues/`**：生成結果先給使用者看，得到「OK / 存檔 / 寫入」明確字眼才動檔
3. **絕不觸碰 frontmatter 的 `status`、`published_url`、`generated_at`、`issue_number`、`image_urls`**（這些由 workflow 管理，改了會破壞自動發布流程）
4. **絕不刪除 `🤖 本文為 AI 產生的貼文` 標記**（誠信標示）
5. **絕不在正文嵌入未經使用者確認的外部連結**（LinkedIn 對含外連的貼文降觸及，連結放留言區或文末註明）
6. **絕不模擬實際發文操作**（這個專案靠 GitHub Actions + LinkedIn API 自動發，本 skill 只產內容）

## ❌ 不要做

- 套用 FB / Threads 那套虛擬病毒公式（LinkedIn 受眾不吃「絕對化 + 反差數字 stacking」那套，會顯得低俗）
- 用「總的來說 / 綜上所述 / 首先 / 另一方面」這類結構詞（典型 GPT 味，違反 stop-slop R2）
- 寫超過 1500 字繁中（LinkedIn sweet spot 是 700-1300 字繁中；超過要拆 part1/part2 — 但要先問使用者）
- 開頭就講背景脈絡（前 2 行必須是 hook，背景排第 3-4 段以後）
- 全篇都是 AI 第三方視角；要有「我」的判斷、踩過的雷、實際數據
- 加多餘 hashtag（3-5 個夠了，且放最末段）
- 用 emoji 灌水段首（一篇最多 2-3 個有意義的 emoji）
- 在正文做總結性收尾（「希望這篇對你有幫助」這類自我引用直接刪）

## 🎯 LinkedIn Viral 5 條件（核心，必背）

```
高觸及 = 前 2 行 hook + 1 個核心 insight + 我的具體經驗 + 留言誘因 + 易讀排版
```

5 個 AND，任一缺 = 觸及死。詳細展開：

1. **前 2 行 hook**：LinkedIn 在第 3 行就 truncate「...see more」，前 2 行決定點擊率
2. **1 個核心 insight**：一篇只講一件事，不要把 issue 裡所有觀點全塞
3. **我的具體經驗**：作者第一人稱 + 具體場景 / 數字 / 工具名（沒有經驗就用 issue 裡的「我看到 / 我試過」線索擴寫）
4. **留言誘因**：結尾問題要開放（不是 yes/no）、跟讀者經驗有關、3 秒能想到答案
5. **易讀排版**：每段 1-3 句、段與段空一行、列點不超 5 項

完整 hook 公式與 LinkedIn 演算法細節分別在 `references/hook_formulas.md` 與 `references/linkedin_playbook.md`。

## 📋 生成流程（G1 把 issue 變貼文）

1. 讀 `issues/issue-X.md` 抓：問題情境、使用者親身經驗、具體工具/連結、結論
2. 選 1 個最有張力的 angle（不要把 issue 全寫進貼文）
3. 套 `hook_formulas.md` 挑 1 個 hook（按 issue 內容性質配對，見 formulas 對照表）
4. 寫 4-6 段繁中正文，每段 1-3 句
5. 留言誘因句結尾
6. 末段放 hashtag（3-5 個）+ `🤖 本文為 AI 產生的貼文`
7. 跑 self-check（下方清單）
8. 輸出給使用者過目；等使用者說「存檔」再寫入 `posts/post-X-YYYYMMDD.md`，沿用現有 frontmatter 規範

## ✅ Self-check 清單（每篇生成 / 改寫完都跑）

對照 `config/stop-slop-guidelines.md` 全項 + 以下 LinkedIn 專屬：

- [ ] 前 2 行能在不展開的情況下勾住點擊？（朗讀測：唸完想不想看下一行）
- [ ] 全篇只講 1 個核心 insight？（如果 > 1，拆 part1/part2 或砍掉）
- [ ] 有「我」做主語的具體經驗段？至少 1 段
- [ ] 結尾留言誘因是開放式問題？（不是「你覺得呢？」這種空洞句）
- [ ] 段落都 ≤ 3 句？段與段空行？
- [ ] hashtag ≤ 5 個 且 與內容相關？
- [ ] 沒有「綜上所述 / 總的來說 / 首先 / 另一方面 / 不可忽視 / 深入探討」？
- [ ] 沒有「不是 X 而是 Y」「X 不是問題，Y 才是」公式？
- [ ] 字數 700-1300 繁中之間？
- [ ] frontmatter 完整且未被動到 status / published_url 欄位？

任一沒過 = 改完再交。

## 💡 對既有 posts 的改善方向

掃過你 `posts/` 內已發布的貼文，典型問題：

- **Hook 在第 4 段才出現**：前面都在鋪背景 → 應該把核心 insight 拉到前 2 行
- **結尾自我總結**：「總的來說 / 我們需要…」一律刪，改成留給讀者帶走的開放問題
- **過度 balanced**：每篇都「一方面...另一方面...找到平衡」，LinkedIn 喜歡有 POV 的貼文（不一定要極端，但要有立場）
- **缺第一人稱經驗**：明明 issue 裡有「我試過」的線索，正文卻寫成第三方觀察 → 把「我」放回去

## 🔄 與本專案自動化的協作邊界

- 本 skill **不取代** `.github/workflows/capture-issue.yml` 的自動生成 — 那是 issue 一建立就跑的 GPT-4 baseline
- 本 skill 用在「baseline 生完不滿意，我想手動把它寫得更好」的場景
- 改寫後直接 commit 到 `posts/` 即可，下一輪 `publish-posts.yml`（每天 9 AM UTC）會撿走發布
- **不要動已 `status: published` 的貼文內文**（已上 LinkedIn 了，改了也沒用反而失同步）— 除非使用者明確說要改

## 📌 快速查詢

| 需要 | 去 |
|---|---|
| LinkedIn hook 公式庫（H1-H8） | `references/hook_formulas.md` |
| LinkedIn 演算法 / 排版 / hashtag 策略 | `references/linkedin_playbook.md` |
| 反 AI 味通用規則 | `../../../config/stop-slop-guidelines.md` |
| 現有貼文範例 | `posts/post-*.md` |
| 觸發來源 issue | `issues/issue-*.md` |

## 常見踩雷

- **使用者貼一段中文要你「改」但沒指定哪篇**：先問是改哪個 issue / post，不要憑猜亂改
- **issue 內容很短或很模糊**：不要硬擴寫成 1300 字，會稀釋；告知使用者「素材不足，建議再給一個具體場景 / 數字」
- **issue 含敏感資訊（公司名 / 數字）**：抽出前先問使用者要不要保留
- **同一 issue 已有 part1/part2**：改寫時兩篇都要過，且確保 part1 結尾埋鉤、part2 開頭接得上
- **使用者要你「用 OO 風格寫」但本專案沒這風格樣本**：先跑 G5 摘要使用者實際發布過的貼文語氣，不要憑空編
