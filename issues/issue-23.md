---
issue_number: 23
title: "Github Copilot 也啟動週使用上限"
author: imaging8896
created_at: 2026-05-16T12:43:10Z
status: generated
---

# Github Copilot 也啟動週使用上限

**Issue #23**
**Author:** imaging8896
**Created:** 2026-05-16T12:43:10Z

## Description


在五月，最後以 premium request 計算月使用量的最後一個月，我繼續使用一個 request 值多重 agents 的作業，結果踩到
```
Failed to get response from the AI model; retried 5 times (total retry wait time: 91.76 seconds) (Request-ID 43C0:16D25D:329F3D1:3DKFJJB:6A084D62) Last error: CAPIError: 429 429 Sorry, you've exceeded your weekly rate limit.
```

不是耶，我的月累積使用量才只有30%就給我踩煞車，雖然我心知肚明我的 tokens 數量應該很巨大，但還是不太高興

<img width="725" height="285" alt="Image" src="https://github.com/user-attachments/assets/af30b712-bf0a-4728-b6b1-68be487b9b33" />

[官方文件](https://docs.github.com/en/copilot/concepts/usage-limits#what-to-do-if-you-hit-a-limit)並沒有給一個明確的數字，一個禮拜多少 tokens 為上限，但看來限制在那邊，當你遇到這個情形，你只能使用 auto model或是等到下禮拜一重置了...
