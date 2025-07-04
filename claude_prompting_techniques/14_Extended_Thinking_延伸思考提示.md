# 14. 延伸思考提示(Extended Thinking)

## 官方定義與說明

<definition>
Extended Thinking 是 Anthropic 官方推出的一項高階提示技術，允許 Claude 在回答複雜問題時進行步驟化的深度思考和推理。該技術通過提供「思考預算」(thinking budget)，讓模型能夠在內部進行詳細的邏輯推理，然後輸出最終答案。Extended Thinking 特別適用於需要多步驟分析、複雜推理和深度思考的任務，能夠顯著提升模型在數學、物理、競程編程和深度分析等領域的表現。
</definition>

## 核心原理

<principle>
Extended Thinking 的核心原理基於「分階段推理」機制：

1. **預算分配**：通過 `budget_tokens` 參數分配思考空間，最小預算為 1024 tokens
2. **內部思考**：模型在思考區塊中進行詳細推理，用戶不會看到完整的思考過程
3. **思考摘要**：系統返回思考過程的摘要，但完整的思考過程會影響計費
4. **最終輸出**：基於深度思考產生的高品質回答

該技術允許模型以更接近人類問題解決方式的方法進行推理，特別是在面對需要多步驟分析的複雜問題時。
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，Extended Thinking 提供以下主要效益：

1. **顯著提升複雜任務表現**：在數學、物理、競程編程和深度分析任務中表現出最大的提升
2. **步驟化推理能力**：允許模型系統性地分解複雜問題，逐步解決
3. **創造性問題解決**：高層次指令往往比規定性步驟指導產生更好的結果
4. **多輪推理支持**：支援工具使用間的交錯思考，提升多步驟任務的執行品質
5. **靈活的思考預算**：可根據任務複雜度調整思考空間，平衡效能與成本
6. **語言優化**：在英文環境下表現最佳，但最終輸出可為任何支援的語言
</benefits>

## 實作格式

### 基本結構
```xml
<thinking>
讓我仔細思考這個問題...
</thinking>

<analysis>
基於我的思考，這裡是詳細分析...
</analysis>
```

### 完整結構
```xml
<instruction>
請深入思考這個複雜問題，考慮多種方法和角度。
</instruction>

<thinking>
讓我從多個角度系統性地分析這個問題...
</thinking>

<reflection>
檢視我的推理過程，確保邏輯正確性...
</reflection>

<conclusion>
基於深度思考得出的結論...
</conclusion>
```

## 實作範例

### 範例1：複雜數學問題求解
```xml
<problem>
求解方程式 x^4 - 5x^2 + 4 = 0 的所有實數解
</problem>

<instruction>
請深入思考這個四次方程式，考慮多種解法，並驗證所有解的正確性。
</instruction>

<thinking>
這是一個雙二次方程式，我需要：
1. 進行變數替換 (令 y = x^2)
2. 求解二次方程式
3. 回代求 x 的值
4. 驗證所有解
</thinking>

<verification>
檢查每個解是否滿足原方程式
</verification>
```

### 範例2：策略分析與決策
```xml
<scenario>
一家科技公司正在考慮進入新興市場，需要評估多個相互競爭的因素
</scenario>

<instruction>
請深入分析這個商業決策，考慮風險、機會、資源分配和競爭環境。運用策略分析框架進行全面評估。
</instruction>

<thinking>
我需要考慮：
- SWOT 分析
- 市場進入壁壘
- 資源需求vs可用資源
- 競爭對手分析
- 時機考量
</thinking>

<strategic_framework>
運用多重分析框架進行綜合評估
</strategic_framework>
```

### 範例3：程式設計問題解決
```xml
<coding_challenge>
設計一個高效的演算法來解決旅行推銷員問題(TSP)
</coding_challenge>

<instruction>
深入思考這個經典的NP困難問題，考慮多種啟發式方法和優化策略。分析各種方法的時間複雜度和適用場景。
</instruction>

<thinking>
我需要分析：
- 問題的本質和複雜度
- 精確解法 vs 近似解法
- 各種啟發式方法的優缺點
- 實際應用考量
</thinking>

<algorithm_analysis>
比較不同演算法的效能和適用性
</algorithm_analysis>
```

## API實作方式

```python
import anthropic

# 基本 Extended Thinking 設定
client = anthropic.Anthropic()

# 標準 Extended Thinking 實作
response = client.messages.create(
    model="claude-3-7-sonnet-20250219",
    max_tokens=4096,
    messages=[{
        "role": "user",
        "content": "分析這個複雜的經濟問題，考慮多重因素的相互作用"
    }],
    thinking={
        "type": "enabled",
        "budget_tokens": 8192  # 可根據任務複雜度調整
    }
)

# 進階：結合工具使用的交錯思考
response = client.beta.messages.create(
    model="claude-opus-4-20250514",
    betas=["interleaved-thinking-2025-05-14"],
    max_tokens=4096,
    messages=[{
        "role": "user", 
        "content": "使用數據分析工具深入研究這個趨勢"
    }],
    tools=[{
        "type": "code_execution_20250522",
        "name": "code_execution"
    }],
    thinking={
        "type": "enabled",
        "budget_tokens": 16384
    }
)

# 超長輸出模式 (128K tokens)
response = client.beta.messages.create(
    model="claude-3-7-sonnet-20250219",
    betas=["output-128k-2025-02-19"],
    max_tokens=128000,
    messages=[{
        "role": "user",
        "content": "撰寫一份完整的技術文檔"
    }],
    thinking={
        "type": "enabled", 
        "budget_tokens": 32768
    }
)

# 使用 LangChain 整合
from langchain_anthropic import ChatAnthropic

llm = ChatAnthropic(
    model="claude-3-7-sonnet-20250219",
    thinking={
        "type": "enabled",
        "budget_tokens": 8192
    }
)

response = llm.invoke(
    "請深入分析這個複雜的系統架構設計問題",
    thinking={"type": "enabled", "budget_tokens": 16384}
)
```

## 最佳實踐

<guidelines>
1. **思考預算策略**：
   - 從最小預算 1024 tokens 開始，逐步增加
   - 複雜任務使用 16K+ tokens 預算
   - 超過 32K tokens 時建議使用批處理

2. **提示策略**：
   - 使用高層次指令而非規定性步驟
   - 添加強化詞：「深入思考」、「仔細分析」、「多角度考慮」
   - 鼓勵模型自行決定最佳思考方式

3. **任務選擇**：
   - 特別適用於數學、物理、編程、分析任務
   - 避免用於簡單或不需要深度推理的任務
   - 考慮成本效益比

4. **語言優化**：
   - 思考過程用英文表現最佳
   - 最終輸出可為任何支援的語言
   - 混合語言使用時需明確指定

5. **回應處理**：
   - 明確指示不要在最終回答中重複思考過程
   - 理解計費基於完整思考過程而非顯示內容
   - 預期回應時間較長
</guidelines>

## 常見錯誤

<mistakes>
1. **預算設定錯誤**：
   - ❌ 低於 1024 tokens 的思考預算
   - ❌ 任務複雜度與預算不匹配
   - ✅ 根據任務複雜度漸進式調整預算

2. **過度規定性指導**：
   - ❌ 「第一步做A，第二步做B，第三步做C」
   - ❌ 限制模型的創造性思考空間
   - ✅ 「深入思考這個問題，考慮多種方法」

3. **思考內容處理錯誤**：
   - ❌ 手動修改思考輸出內容
   - ❌ 將思考文字作為用戶輸入回傳
   - ✅ 讓模型自然完成思考過程

4. **成本意識不足**：
   - ❌ 忽視完整思考過程的計費成本
   - ❌ 對簡單任務使用過大思考預算
   - ✅ 平衡任務複雜度與成本考量

5. **不當的任務分配**：
   - ❌ 用於不需要深度推理的簡單任務
   - ❌ 期望所有任務都能從延伸思考中受益
   - ✅ 選擇真正需要複雜推理的任務
</mistakes>

## 測試與優化

<evaluation>
1. **效能評估指標**：
   - 答案準確性與完整性
   - 推理邏輯的合理性
   - 思考過程的深度與廣度
   - 成本效益分析

2. **A/B 測試方法**：
   - 比較有/無延伸思考的結果品質
   - 測試不同思考預算的效果
   - 評估各種提示策略的成效

3. **品質檢查清單**：
   - 思考過程是否系統性
   - 最終答案是否基於深度分析
   - 是否考慮了多種角度和方法
   - 推理是否邏輯連貫
</evaluation>

<iteration>
1. **預算優化流程**：
   - 從 1024 tokens 開始測試
   - 逐步增加至 2048、4096、8192
   - 找到效果與成本的平衡點

2. **提示詞優化**：
   - 測試不同的強化詞組合
   - 實驗高層次 vs 具體指導
   - 調整問題表述方式

3. **模型選擇**：
   - Claude Opus 4：最高品質
   - Claude Sonnet 3.7：平衡效能
   - 根據任務需求選擇適合的模型

4. **持續改進**：
   - 收集使用者回饋
   - 分析失敗案例
   - 建立最佳實踐範本
</iteration>

## 與其他技術組合

<combinations>
1. **與 Chain of Thought 結合**：
   - 在標準模式下使用 CoT 作為基準
   - 延伸思考模式進行更深度的分析
   - 比較兩種方法的效果差異

2. **與 Multishot Prompting 整合**：
   - 提供思考模式的範例
   - 使用 XML 標籤展示思考模式
   - 讓模型學習特定的思考模式

3. **與 Tool Use 協作**：
   - 交錯思考：工具使用間的深度思考
   - 工具結果的反思與分析
   - 多步驟工具使用的策略規劃

4. **與 XML 標籤結合**：
   - 結構化思考輸出
   - 明確區分思考與結論
   - 提升回應的可讀性

5. **與 Context 優化整合**：
   - 長上下文中的段落式思考
   - 複雜資訊的系統性分析
   - 多文件比較與綜合
</combinations>

## 官方參考連結

- [Extended Thinking Tips - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/extended-thinking-tips)
- [Building with Extended Thinking - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking)
- [Prompt Engineering Overview - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Anthropic Python SDK - GitHub](https://github.com/anthropics/anthropic-sdk-python)
- [Extended Thinking - Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/claude-messages-extended-thinking.html)