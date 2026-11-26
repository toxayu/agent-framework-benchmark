# Daily Log

## Day 0（2026-11-26）

- 今日目標：完成診斷與環境準備
- 完成項目：
  - 診斷 6 題：async 寫過但生疏；pytest 沒用過；Pydantic 會；docker-compose 會；OpenTelemetry 沒聽過；Dify 用 idempotent check（key 用後端資料，金額要正規化後比對）
  - 環境：Python 用 uv；模型走 OpenRouter；預算 50 USD；公司規範已確認；GitHub repo `agent-framework-benchmark` 已建立
  - 預算試算：1,700 次 × 每次約 0.04 USD（輸入 15k × $2/M + 輸出 1k × $10/M）≈ 68 USD，超出預算
  - 決定：換更便宜的模型，三套框架用同一個；開發時只跑 5 筆案例
- 未完成與原因：
  - 具體模型尚未選定（條件：OpenRouter 上支援 tool calling、輸入約 ≤ $1.4/M）→ 模型名稱：google/gemini-4-hydrogen
  - 留到 Day 3 的問題：同一訂單合法分兩次退 50 元，idempotency key 還需要什麼？resquest-key
- 預期 vs 實際（最意外的一件事）：忘了async
- 今天學到最重要的概念：還沒有
- 卡關與解法：
- 明天第一件事：clone Strands Workshop，把模型換成 OpenRouter（參考課程 `02-model-providers`），跑 Module 1 前先預測「問 C-1001 最近的訂單，模型會被呼叫幾次？工具會被呼叫幾次？」
- 實際花費時間：閱讀 5 / 實作 5 / 整理 5 min
