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

## Day 1（2026-11-27）

- 今日目標：Day 1 上午：Strands Workshop 01–04、07，建立 Agent Loop、Hooks、Steering、Session、Evals 的整體印象
- 完成項目：
  - 環境：Workshop clone 到 `~/Projects`；01–04 的 `chat.py` 改用 OpenRouter（`strands-agents[openai]==1.67.0`，`OpenAIModel` + `base_url=https://openrouter.ai/api/v1`，`temperature=0`）
  - Module 01 Agent Loop：「C-1001 最近的訂單」實測 3 cycles、2 次工具、1,427 input / 362 output tokens。模型呼叫次數 = 工具輪數 + 1；每輪都重送 system prompt、tool schema、對話歷史
  - 驗收 Q1 通過：模型不再要求呼叫工具時停止；需用程式碼設上限
  - Module 02 Hooks：查 5 位客戶，模型一次送出 5 個平行工具呼叫，`RateLimiterHook` 擋下第 4、5 次；`cancel_tool` 文字會當成工具結果回給模型，模型回覆部分結果，並把內部限制告訴使用者
  - Module 03 Skills + Steering：30 天退貨規則只寫在 skill（prompt）裡，`RefundWorkflowHandler` 和 `process_refund` 都沒檢查。模型不知道今天日期，誤判 ORD-5521（2025-04-28 交貨）仍在退貨期內並主動提議退 $79.99；客戶回 YES 後模型回覆空白，沒有呼叫 `process_refund`
- 未完成與原因：Module 04 Session Managers、07 Evals、Agent Loop 流程圖、驗收 Q2，時間不足，留到下次
- 預期 vs 實際（最意外的一件事）：
  - Module 02 預測模型會說「整個請求無法完成」，實際是回覆部分結果
  - Module 03 模型因為不知道日期而違反 30 天政策；只看 Mock DB 最終狀態時，這個案例「沒有退款」看起來反而正確
- 今天學到最重要的概念：（請自己寫）
- 卡關與解法：
  - Bedrock → OpenRouter：Strands `OpenAIModel` 的 `client_args` 指定 `base_url` 與 `api_key`（從環境變數讀）
  - `reasoningContent is not supported...` 警告：Chat Completions 格式無法回傳上一輪思考內容，Strands 會丟掉並計數；不影響執行，需在報告實驗設計中註明
- 觀察筆記（benchmark 用）：
  - prompt 是機率，hook／工具層才是保證
  - 確定性政策檢查應寫在工具層，用系統時間判斷，不依賴模型知道日期
  - Task Success 看最終狀態，回覆品質（錯誤事實、沉默）另外用 LLM-as-a-Judge 評
  - `ToneGuardrailHandler` 每次模型回覆後都多一次 LLM 呼叫，成本約翻倍
  - reasoning token 照輸出計費；temperature=0 仍有非確定性（訂單順序、reasoning 有無）
- 明天第一件事：Module 04 Session Managers（用同一個 `session_id` 關掉重開 `chat.py`），先預測「工具執行到一半當掉，session manager 能恢復嗎？」
- 實際花費時間：閱讀 __ / 實作 __ / 整理 __
