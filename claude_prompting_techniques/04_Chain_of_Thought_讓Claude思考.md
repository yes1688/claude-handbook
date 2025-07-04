# 4. 讓Claude思考(Chain of Thought)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，Chain of Thought (CoT) 是一種提示技術，鼓勵 Claude 透過逐步推理來解決複雜問題。這種方法引導 Claude 將問題分解成更小的步驟，並展示其思考過程，從而提高回答的準確性和深度。CoT 特別適用於需要邏輯推理、數學計算、分析或其他複雜認知任務的情況。
</definition>

## 核心原理

<principle>
Chain of Thought 技術的核心原理是模仿人類解決複雜問題的方式：
1. **逐步分解**：將複雜問題拆解成更小、更易處理的子問題
2. **顯性推理**：要求 Claude 輸出其思考過程，而非僅提供最終答案
3. **中間步驟**：透過展示推理的中間步驟，提高最終答案的準確性
4. **思考輸出**：重要的是，Claude 必須輸出思考過程才能真正進行思考，沒有私密思考模式

核心要點：思考無法在不輸出思考過程的情況下發生！
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔中明確指出 Chain of Thought 的主要效益：
1. **提高準確性**：特別在數學、邏輯、分析或複雜任務中減少錯誤
2. **增強一致性**：產生更連貫和組織良好的回應
3. **改善可調試性**：開發者可以理解和檢查 AI 的推理過程
4. **提升信心度**：透過展示推理步驟，增加對結果的信心
5. **更好的問題解決**：允許 Claude 在複雜問題上投入更多認知資源
</benefits>

## 實作格式

### 基本結構
```xml
<prompt>
請針對以下問題逐步思考：
[問題描述]

Think step-by-step.
</prompt>
```

### 完整結構
```xml
<prompt>
<role>你是一位專業的問題解決專家</role>

<task>
針對以下問題，請：
1. 仔細分析問題的各個層面
2. 逐步制定解決方案
3. 檢查你的推理過程
4. 提供最終答案

<thinking>
在這裡展示你的思考過程，包括：
- 問題分析
- 可能的解決方案
- 推理步驟
- 驗證過程
</thinking>

<answer>
在這裡提供你的最終答案
</answer>
</task>

<problem>
[具體問題描述]
</problem>
</prompt>
```

## 實作範例

### 範例1：數學問題解決
```xml
<prompt>
請解決以下數學問題，並展示你的思考過程：

一家公司在第一年賺了 $50,000，在第二年賺了 $75,000，在第三年賺了 $60,000。如果第四年的利潤比前三年平均利潤高 20%，那麼第四年的利潤是多少？

<thinking>
在這裡逐步計算：
1. 先計算前三年的總利潤
2. 計算前三年的平均利潤
3. 計算第四年利潤（平均利潤的 120%）
4. 驗證計算結果
</thinking>

<answer>
提供最終的數值答案和簡要說明
</answer>
</prompt>
```

### 範例2：商業策略分析
```xml
<prompt>
<role>你是一位商業策略顧問</role>

<task>
分析以下商業情境，並提供策略建議：

一家傳統零售書店面臨線上書店的競爭，客流量下降 30%。請制定一個轉型策略。

<thinking>
請在這裡展示你的分析過程：
1. 分析當前市場狀況和挑戰
2. 識別書店的競爭優勢
3. 評估可能的轉型選項
4. 考慮實施的可行性和風險
5. 制定具體的行動計劃
</thinking>

<answer>
提供你的策略建議，包括優先順序和實施步驟
</answer>
</task>
</prompt>
```

### 範例3：技術問題診斷
```xml
<prompt>
<role>你是一位系統工程師</role>

<problem>
一個網站載入速度很慢，用戶抱怨頁面需要 8-10 秒才能完全載入。請診斷可能的原因並提供解決方案。
</problem>

<instructions>
請逐步分析這個問題：

<thinking>
1. 列出可能導致網站載入緩慢的原因
2. 按照可能性和影響程度排序這些原因
3. 為每個原因提供診斷方法
4. 制定解決方案的優先順序
5. 考慮預防措施
</thinking>

<answer>
提供系統性的診斷流程和解決方案
</answer>
</instructions>
</prompt>
```

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

# 基本 Chain of Thought 實作
def basic_chain_of_thought(problem):
    message = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1500,
        messages=[
            {
                "role": "user", 
                "content": f"""
                請針對以下問題逐步思考：
                {problem}
                
                Think step-by-step and show your reasoning process.
                """
            }
        ]
    )
    return message.content

# 結構化 Chain of Thought 實作
def structured_chain_of_thought(problem, domain="general"):
    prompt = f"""
    <role>你是一位{domain}領域的專家</role>
    
    <task>
    請解決以下問題：
    {problem}
    
    <thinking>
    在這裡展示你的思考過程：
    1. 分析問題的核心
    2. 識別相關因素
    3. 制定解決方案
    4. 驗證結果
    </thinking>
    
    <answer>
    提供你的最終答案
    </answer>
    </task>
    """
    
    message = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=2000,
        messages=[
            {"role": "user", "content": prompt}
        ]
    )
    return message.content

# 延伸思考實作（適用於 Claude 4）
def extended_thinking(problem, thinking_budget=1024):
    message = client.messages.create(
        model="claude-4-20250514",  # 適用於 Claude 4
        max_tokens=3000,
        messages=[
            {
                "role": "user",
                "content": f"""
                請深入思考以下問題：
                {problem}
                
                請用充分的時間思考這個問題的各個方面，
                不要急於得出結論。
                """
            }
        ]
    )
    return message.content

# 使用範例
if __name__ == "__main__":
    problem = "如何提高團隊的工作效率？"
    
    # 基本方法
    basic_result = basic_chain_of_thought(problem)
    print("基本 CoT 結果：", basic_result)
    
    # 結構化方法
    structured_result = structured_chain_of_thought(problem, "管理")
    print("結構化 CoT 結果：", structured_result)
    
    # 延伸思考方法
    extended_result = extended_thinking(problem)
    print("延伸思考結果：", extended_result)
```

## 最佳實踐

<guidelines>
1. **明確思考要求**：總是明確要求 Claude 展示思考過程，使用 "Think step-by-step" 或類似指令
2. **使用結構化標籤**：採用 `<thinking>` 和 `<answer>` 標籤來分離推理和結果
3. **適當的思考預算**：為複雜問題提供足夠的 token 預算（至少 1024 tokens）
4. **漸進式複雜度**：從簡單的步驟開始，逐漸增加複雜度
5. **領域特定指引**：為特定領域提供相關的思考框架
6. **驗證機制**：要求 Claude 檢查和驗證其推理過程
7. **清晰的問題陳述**：確保問題描述清楚且具體
8. **避免過度指導**：允許 Claude 在推理過程中有一定的靈活性
</guidelines>

## 常見錯誤

<mistakes>
1. **隱藏思考過程**：
   - 錯誤：要求 Claude 私下思考並只返回答案
   - 正確：始終要求輸出思考過程
   
2. **過度規範步驟**：
   - 錯誤：過於詳細地規定每個思考步驟
   - 正確：提供高層次指導，允許靈活性
   
3. **不合適的任務**：
   - 錯誤：對簡單任務使用 CoT（如基本事實查詢）
   - 正確：將 CoT 用於複雜推理任務
   
4. **忽略 token 限制**：
   - 錯誤：不考慮思考過程的 token 消耗
   - 正確：為思考過程分配適當的 token 預算
   
5. **缺乏驗證**：
   - 錯誤：不要求 Claude 檢查其推理
   - 正確：包含自我驗證步驟
   
6. **混合思考文本**：
   - 錯誤：將 Claude 的思考文本作為用戶輸入傳回
   - 正確：保持思考文本的完整性
</mistakes>

## 測試與優化

<evaluation>
評估 Chain of Thought 效果的方法：
1. **準確性測試**：比較有無 CoT 的答案準確性
2. **一致性檢查**：多次執行同一問題，檢查推理一致性
3. **推理質量**：評估思考過程的邏輯性和完整性
4. **時間效率**：平衡思考深度與回應時間
5. **問題覆蓋**：測試不同類型和複雜度的問題
</evaluation>

<iteration>
迭代改進 Chain of Thought 的方法：
1. **分析失敗案例**：識別推理中斷或錯誤的模式
2. **調整指令**：根據結果優化思考指導
3. **增加範例**：提供更多高質量的思考示範
4. **優化結構**：改進 XML 標籤和格式
5. **平衡複雜度**：找到最適合任務的思考深度
6. **用戶反饋**：結合實際使用者的反饋進行改進
</iteration>

## 與其他技術組合

<combinations>
Chain of Thought 可與以下技術有效組合：
1. **多樣本提示 (Multishot)**：提供思考過程的示範例子
2. **XML 標籤**：結構化思考輸出
3. **系統提示**：設定專業角色增強思考品質
4. **輸出格式控制**：規範思考和答案的格式
5. **提示鏈**：將複雜思考分解為多個步驟
6. **工具使用**：結合外部工具進行複雜計算
7. **延伸思考**：在 Claude 4 中使用更深層的推理
8. **負面提示**：避免特定類型的思考錯誤
</combinations>

## 官方參考連結

- [Let Claude think (chain of thought prompting)](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-of-thought)
- [Extended thinking tips](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/extended-thinking-tips)
- [Chain complex prompts](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-prompts)
- [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)