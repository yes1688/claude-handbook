# 27. 減少幻覺(Reduce Hallucinations)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，減少幻覺是指通過特定的提示工程技術，降低 Claude 生成不準確、虛構或與事實不符的信息的可能性。幻覺是指模型生成看似合理但實際上不正確或無法驗證的內容。減少幻覺的技術旨在提高 Claude 響應的準確性和可靠性，特別是在處理事實性問題和文檔分析任務時。
</definition>

## 核心原理

<principle>
減少幻覺的核心原理基於以下幾個方面：

1. **不確定性承認**：允許 Claude 承認其知識限制，避免強迫生成不確定的信息
2. **證據基礎回應**：要求 Claude 基於具體的證據或引用來構建回應
3. **溫度控制**：通過調整生成參數來減少創造性但可能不準確的內容
4. **結構化驗證**：通過多步驟驗證過程來檢查信息的準確性
5. **外部知識限制**：明確限制 Claude 只使用提供的文檔而非其通用知識

這些原理共同作用，形成一個多層次的防護體系，最大限度地減少不準確信息的生成。
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，減少幻覺技術提供以下效益：

1. **提高準確性**：顯著降低生成錯誤或虛構信息的機率
2. **增強可靠性**：使 Claude 的回應更加可信和可依賴
3. **改善用戶體驗**：減少用戶需要驗證信息的負擔
4. **降低風險**：特別適用於高風險決策場景
5. **提高專業性**：在專業領域應用中表現更加可靠
6. **支持批判性思維**：鼓勵基於證據的推理方式
</benefits>

## 實作格式

### 基本結構
```xml
<instructions>
如果您不確定答案，請直接說「我不知道」。
只有在您非常確信答案正確時才回答。
</instructions>

<task>
[您的具體任務]
</task>
```

### 完整結構
```xml
<instructions>
請仔細閱讀以下文檔，然後回答問題。
如果您不確定答案或文檔中沒有相關信息，請直接說「我不知道」。
只使用文檔中的信息，不要使用您的通用知識。
</instructions>

<document>
[提供的文檔內容]
</document>

<verification_process>
1. 首先在 <scratchpad> 中提取相關引用
2. 評估引用是否完全支持您的回答
3. 如果證據不足，請承認限制
</verification_process>

<task>
[您的具體問題或任務]
</task>
```

## 實作範例

### 範例1：允許承認不確定性
```xml
<instructions>
請回答以下問題。如果您不確定答案或沒有足夠的信息，請直接說「我不知道」。
只有在您非常確信答案正確時才提供具體信息。
</instructions>

<task>
2024年諾貝爾物理學獎得主是誰？他們的主要貢獻是什麼？
</task>

<expected_response>
我不知道2024年諾貝爾物理學獎得主的具體信息。我的訓練數據可能不包含最新的獲獎信息，為了避免提供不準確的資訊，我建議您查看諾貝爾獎官方網站或最新的新聞報導。
</expected_response>
```

### 範例2：基於證據的回應
```xml
<instructions>
請仔細閱讀以下文檔，然後回答問題。
首先在 <scratchpad> 標籤中提取相關的直接引用，然後基於這些引用回答問題。
如果文檔中沒有相關信息，請明確說明。
</instructions>

<document>
公司季度報告顯示，第三季度營收為5.2億美元，比去年同期增長15%。
客戶滿意度調查結果顯示，93%的客戶表示滿意或非常滿意。
預計第四季度營收將達到5.8億美元。
</document>

<task>
公司第三季度的營收增長率是多少？客戶滿意度如何？
</task>

<expected_response>
<scratchpad>
相關引用：
- "第三季度營收為5.2億美元，比去年同期增長15%"
- "客戶滿意度調查結果顯示，93%的客戶表示滿意或非常滿意"
</scratchpad>

根據文檔信息：
1. 第三季度營收增長率為15%（相比去年同期）
2. 客戶滿意度表現良好，93%的客戶表示滿意或非常滿意
</expected_response>
```

### 範例3：限制外部知識使用
```xml
<instructions>
請僅基於提供的文檔內容回答問題。
不要使用您的通用知識或文檔外的信息。
如果文檔中沒有足夠信息回答問題，請明確說明。
</instructions>

<document>
蘋果公司發布了新的iPhone 15系列，包括iPhone 15和iPhone 15 Pro兩個版本。
新功能包括改進的攝影系統和更長的電池續航力。
</document>

<task>
iPhone 15系列的價格是多少？支持哪些顏色選項？
</task>

<expected_response>
根據提供的文檔，我無法回答iPhone 15系列的價格和顏色選項問題。
文檔中只提到了iPhone 15系列包括iPhone 15和iPhone 15 Pro兩個版本，
以及新功能包括改進的攝影系統和更長的電池續航力，
但沒有提供價格和顏色選項的具體信息。
</expected_response>
```

## API實作方式

```python
import anthropic

def reduce_hallucinations_chat(client, user_query, document_content=None, temperature=0.0):
    """
    使用減少幻覺技術的聊天函數
    
    Args:
        client: Anthropic client instance
        user_query: 用戶查詢
        document_content: 可選的文檔內容
        temperature: 溫度參數，默認為0.0以減少創造性
    """
    
    # 構建減少幻覺的提示
    if document_content:
        prompt = f"""
<instructions>
請仔細閱讀以下文檔，然後回答問題。
如果您不確定答案或文檔中沒有相關信息，請直接說「我不知道」。
只使用文檔中的信息，不要使用您的通用知識。
</instructions>

<document>
{document_content}
</document>

<verification_process>
1. 首先在 <scratchpad> 中提取相關引用
2. 評估引用是否完全支持您的回答
3. 如果證據不足，請承認限制
</verification_process>

<task>
{user_query}
</task>
"""
    else:
        prompt = f"""
<instructions>
請回答以下問題。如果您不確定答案或沒有足夠的信息，請直接說「我不知道」。
只有在您非常確信答案正確時才提供具體信息。
</instructions>

<task>
{user_query}
</task>
"""
    
    try:
        response = client.messages.create(
            model="claude-3-sonnet-20240229",
            max_tokens=1000,
            temperature=temperature,  # 低溫度減少創造性
            messages=[
                {"role": "user", "content": prompt}
            ]
        )
        
        return response.content[0].text
        
    except Exception as e:
        return f"處理請求時發生錯誤: {str(e)}"

def multi_verification_check(client, query, document_content, iterations=3):
    """
    多次驗證以檢查一致性
    
    Args:
        client: Anthropic client instance
        query: 查詢問題
        document_content: 文檔內容
        iterations: 驗證次數
    """
    
    responses = []
    
    for i in range(iterations):
        response = reduce_hallucinations_chat(
            client, query, document_content, temperature=0.0
        )
        responses.append(response)
    
    # 檢查回應一致性
    consistent = all(resp == responses[0] for resp in responses)
    
    return {
        "responses": responses,
        "consistent": consistent,
        "final_answer": responses[0] if consistent else "回應不一致，需要進一步驗證"
    }

# 使用示例
client = anthropic.Anthropic(api_key="your-api-key")

# 單次查詢
query = "文檔中提到的銷售增長率是多少？"
document = "第二季度銷售額增長了12%，達到創紀錄的水平。"

result = reduce_hallucinations_chat(client, query, document)
print("回應:", result)

# 多次驗證
verification_result = multi_verification_check(client, query, document)
print("驗證結果:", verification_result)
```

## 最佳實踐

<guidelines>
1. **明確設定邊界**：清楚告知 Claude 可以說「我不知道」，避免強迫回答
2. **使用低溫度設定**：將溫度設置為 0.0 或接近 0 的值，減少創造性但可能不準確的回應
3. **要求證據支持**：要求 Claude 提供支持其回答的具體引用或證據
4. **分離數據和指令**：將參考文檔與任務指令清楚分離
5. **使用結構化驗證**：實施多步驟驗證過程，包括證據提取和評估
6. **限制知識來源**：明確指示只使用提供的文檔，不使用通用知識
7. **實施多次驗證**：對重要信息進行多次查詢，檢查一致性
8. **提供清晰的任務範圍**：明確定義任務邊界，避免模糊的要求
9. **鼓勵批判性思維**：要求 Claude 評估信息的可靠性和完整性
10. **定期驗證關鍵信息**：對於高風險應用，建議進行外部驗證
</guidelines>

## 常見錯誤

<mistakes>
1. **強迫回答錯誤**：
   - 錯誤：「您必須回答這個問題」
   - 正確：「如果您不確定，請說『我不知道』」

2. **混合知識源錯誤**：
   - 錯誤：「使用文檔和您的知識回答」
   - 正確：「只使用提供的文檔信息回答」

3. **高溫度設定錯誤**：
   - 錯誤：設定 temperature=1.0 增加創造性
   - 正確：設定 temperature=0.0 確保一致性

4. **缺乏證據要求錯誤**：
   - 錯誤：「請回答問題」
   - 正確：「請提供支持您回答的具體引用」

5. **忽略驗證步驟錯誤**：
   - 錯誤：直接要求最終答案
   - 正確：要求先提取證據，再給出答案

6. **模糊任務範圍錯誤**：
   - 錯誤：「告訴我關於這個主題的一切」
   - 正確：「基於文檔回答這個具體問題」

7. **單次查詢依賴錯誤**：
   - 錯誤：對關鍵信息只查詢一次
   - 正確：對重要信息進行多次驗證

8. **忽略不確定性標誌錯誤**：
   - 錯誤：忽略 Claude 表達的不確定性
   - 正確：重視並進一步調查不確定的回應
</mistakes>

## 測試與優化

<evaluation>
評估減少幻覺技術效果的方法：

1. **事實準確性測試**：
   - 使用已知答案的問題測試
   - 比較回應與標準答案的準確性
   - 記錄幻覺出現的頻率

2. **一致性測試**：
   - 多次提出相同問題
   - 檢查回應的一致性
   - 分析變異性來源

3. **證據支持度測試**：
   - 驗證引用的準確性
   - 檢查證據與結論的邏輯連接
   - 評估證據的充分性

4. **邊界測試**：
   - 提出文檔外的問題
   - 檢查 Claude 是否正確承認限制
   - 評估拒絕回答的適當性

5. **專家評估**：
   - 邀請領域專家評估回應質量
   - 獲取專業意見和改進建議
   - 建立專業標準基準
</evaluation>

<iteration>
迭代改進減少幻覺技術的方法：

1. **分析幻覺模式**：
   - 記錄和分類幻覺類型
   - 識別高風險場景
   - 建立預防策略

2. **優化提示結構**：
   - 測試不同的指令措辭
   - 調整驗證步驟順序
   - 細化任務範圍定義

3. **調整參數設定**：
   - 實驗不同的溫度值
   - 測試最大令牌數限制
   - 優化採樣策略

4. **增強驗證機制**：
   - 添加額外的檢查步驟
   - 實施交叉驗證
   - 建立多層驗證體系

5. **持續監控和改進**：
   - 建立監控系統
   - 定期評估效果
   - 根據反饋調整策略
</iteration>

## 與其他技術組合

<combinations>
減少幻覺技術可以與以下技術有效組合：

1. **與 Chain of Thought 組合**：
   - 要求 Claude 展示推理過程
   - 在每個推理步驟中要求證據支持
   - 提高透明度和可驗證性

2. **與 XML 標籤組合**：
   - 使用結構化標籤組織信息
   - 分離事實、推理和結論
   - 提高信息的可追溯性

3. **與 Specificity 組合**：
   - 提供具體、詳細的指令
   - 明確定義回應格式
   - 減少模糊性導致的錯誤

4. **與 Output Formatting 組合**：
   - 要求標準化的回應格式
   - 包含置信度指標
   - 分離事實和推測

5. **與 Test Cases 組合**：
   - 建立測試用例庫
   - 定期驗證技術效果
   - 持續改進防護措施

6. **與 Iterative Improvement 組合**：
   - 建立反饋循環
   - 持續優化防護策略
   - 適應新的挑戰場景

7. **與 Tool Use 組合**：
   - 使用外部工具驗證信息
   - 實施自動事實檢查
   - 建立多源驗證機制

8. **與 Safety Considerations 組合**：
   - 在安全關鍵應用中加強驗證
   - 實施多層防護措施
   - 建立錯誤恢復機制
</combinations>

## 官方參考連結

- [Anthropic - Reduce Hallucinations](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations)
- [Anthropic - Minimizing Hallucinations](https://docs.anthropic.com/en/docs/minimizing-hallucinations)
- [Anthropic Courses - Avoiding Hallucinations](https://github.com/anthropics/courses/blob/master/prompt_engineering_interactive_tutorial/Anthropic%201P/08_Avoiding_Hallucinations.ipynb)
- [Claude 4 Prompt Engineering Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)