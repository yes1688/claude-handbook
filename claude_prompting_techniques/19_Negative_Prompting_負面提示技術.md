# 19. 負面提示技術(Negative Prompting)

## 官方定義與說明

<definition>
負面提示技術是指在提示中明確告知 Claude 不要做什麼或避免什麼行為的技術。根據 Anthropic 官方文檔，雖然這種技術可以用於約束 AI 的行為，但官方建議「告訴 Claude 要做什麼，而不是不要做什麼」。Anthropic 的高級提示工程師 Zack Witten 特別警告，過於強烈地告訴 Claude 不要做什麼可能會產生反向心理效應，實際上鼓勵了那種行為。
</definition>

## 核心原理

<principle>
負面提示技術的核心原理基於語言模型的對話延續機制。當我們告訴 Claude 不要做某事時，模型需要理解並記住這個約束。然而，由於 LLM 傾向於延續對話的相同方式，強調不要做的事情可能會無意中將注意力集中在被禁止的主題上。這種現象類似於「白熊效應」，當被告知不要想白熊時，反而更容易想到白熊。因此，正面指導和具體的期望描述通常比負面約束更有效。
</principle>

## 官方效益

<benefits>
適當使用負面提示技術可以：
1. 設定明確的行為邊界
2. 避免特定的不當回應
3. 防止模型產生不符合要求的內容
4. 提高輸出的準確性和相關性

但官方強調，這些效益只有在「輕觸」使用時才能實現，過度使用可能導致反效果。
</benefits>

## 實作格式

### 基本結構
```xml
<instruction>
你的主要任務是[正面指導]。
</instruction>

<constraints>
請注意避免[具體約束]。
</constraints>
```

### 完整結構
```xml
<system_prompt>
你是一位專業的[角色定義]。
</system_prompt>

<primary_instruction>
請[具體正面指導]。
</primary_instruction>

<positive_examples>
理想的輸出應該：
- [正面特徵1]
- [正面特徵2]
- [正面特徵3]
</positive_examples>

<constraints>
在執行任務時，請輕微注意：
- 避免[具體約束1]
- 不包含[具體約束2]
</constraints>

<format>
請以[期望格式]回應。
</format>
```

## 實作範例

### 範例1：內容創作約束
```xml
<system_prompt>
你是一位專業的技術文章作者。
</system_prompt>

<primary_instruction>
請撰寫一篇關於人工智慧發展的技術文章，重點介紹實際應用案例和未來趨勢。
</primary_instruction>

<positive_guidance>
文章應該：
- 包含具體的技術實例
- 使用客觀的分析語調
- 提供實用的見解
- 保持專業的寫作風格
</positive_guidance>

<gentle_constraints>
請避免過度投機的預測或未經驗證的說法。
</gentle_constraints>

<format>
請以標準的技術文章格式回應，包含標題、引言、主體段落和結論。
</format>
```

### 範例2：代碼生成約束
```xml
<system_prompt>
你是一位資深的 Python 開發者。
</system_prompt>

<primary_instruction>
請編寫一個數據處理函數，用於清理和驗證用戶輸入的電子郵件地址。
</primary_instruction>

<positive_specifications>
函數應該：
- 使用正則表達式進行驗證
- 返回清理後的電子郵件地址
- 包含適當的錯誤處理
- 添加詳細的文檔字符串
</positive_specifications>

<light_constraints>
請確保代碼不包含硬編碼的測試數據。
</light_constraints>

<format>
請提供完整的函數定義，包括導入語句和使用示例。
</format>
```

### 範例3：對話回應約束
```xml
<system_prompt>
你是一位友善的客戶服務代表。
</system_prompt>

<primary_instruction>
請回應客戶關於產品退換貨政策的詢問，提供清晰、有幫助的信息。
</primary_instruction>

<positive_approach>
回應應該：
- 使用溫和友好的語調
- 提供具體的政策細節
- 包含明確的步驟指導
- 表現出理解和同理心
</positive_approach>

<gentle_boundaries>
請避免承諾超出公司政策範圍的特殊處理。
</gentle_boundaries>

<format>
請以自然的對話方式回應，保持專業但親切的態度。
</format>
```

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

# 推薦的負面提示實作方式
def create_constrained_prompt(task, positive_guidance, gentle_constraints):
    """
    創建包含輕微約束的提示
    
    Args:
        task: 主要任務描述
        positive_guidance: 正面指導列表
        gentle_constraints: 輕微約束列表
    """
    
    prompt = f"""
<system_prompt>
你是一位專業的助理。
</system_prompt>

<primary_instruction>
{task}
</primary_instruction>

<positive_guidance>
請確保你的回應：
{chr(10).join(f"- {item}" for item in positive_guidance)}
</positive_guidance>

<gentle_constraints>
請輕微注意：
{chr(10).join(f"- {item}" for item in gentle_constraints)}
</gentle_constraints>
"""
    
    return prompt

# 使用示例
task = "解釋機器學習的基本概念"
positive_guidance = [
    "使用清晰易懂的語言",
    "包含具體的例子",
    "按邏輯順序組織信息"
]
gentle_constraints = [
    "避免過於技術性的術語",
    "不要假設讀者有深厚的數學背景"
]

prompt = create_constrained_prompt(task, positive_guidance, gentle_constraints)

message = client.messages.create(
    model="claude-3-sonnet-20240229",
    max_tokens=1000,
    messages=[
        {"role": "user", "content": prompt}
    ]
)

print(message.content)
```

## 最佳實踐

<guidelines>
1. **輕觸原則**：使用負面提示時要「輕觸」，避免過於強烈的禁止語言
2. **正面優先**：優先使用正面指導，描述期望的行為而非禁止的行為
3. **具體明確**：如果必須使用負面約束，要具體明確而不是模糊的禁止
4. **平衡組合**：將負面約束與正面例子結合使用，提供平衡的指導
5. **測試驗證**：定期測試負面提示是否產生預期效果，而非反向效果
6. **語言溫和**：使用「請避免」而非「絕對不要」等更溫和的語言
7. **簡潔明了**：保持負面約束簡潔，不要過度強調禁止事項
</guidelines>

## 常見錯誤

<mistakes>
1. **過度強調禁止**：使用「絕對不要」、「嚴禁」等過於強烈的語言
   - 錯誤：「絕對不要提到競爭對手」
   - 正確：「請專注於我們自己的產品特色」

2. **反向心理觸發**：過度詳細地描述不想要的行為
   - 錯誤：「不要寫出無聊、冗長、重複的內容」
   - 正確：「請寫出簡潔、有趣、條理清晰的內容」

3. **缺乏正面指導**：只有負面約束，沒有正面方向
   - 錯誤：「不要用技術術語，不要太複雜」
   - 正確：「請使用通俗易懂的語言，適合一般讀者理解」

4. **約束衝突**：設定相互矛盾的約束條件
   - 錯誤：「要詳細但不要太長」
   - 正確：「請在200字內提供重點信息」

5. **忽視測試**：沒有驗證負面提示是否真的有效
   - 解決方法：定期測試和調整提示策略
</mistakes>

## 測試與優化

<evaluation>
測試負面提示效果的方法：
1. **對比測試**：比較使用負面提示前後的輸出質量
2. **邊界測試**：故意觸發約束條件，觀察 Claude 的反應
3. **長期監控**：收集一段時間的回應，分析是否出現反向效果
4. **用戶反饋**：收集終端用戶對輸出質量的評價
5. **錯誤分析**：分析不符合期望的回應，調整約束策略
</evaluation>

<iteration>
負面提示的迭代改進方法：
1. **逐步減少**：如果發現反向效果，逐步減少負面約束的強度
2. **正面轉換**：將負面約束轉換為正面指導
3. **具體化改進**：將模糊的約束改為具體的指導
4. **組合優化**：調整正面指導與負面約束的比例
5. **語言調整**：改變約束的表達方式，使用更溫和的語言
</iteration>

## 與其他技術組合

<combinations>
負面提示技術可以與以下技術有效組合：

1. **多樣本提示**：使用正面和負面例子展示期望與避免的行為
2. **XML 標籤結構**：用標籤清晰分隔正面指導和負面約束
3. **思維鏈提示**：讓 Claude 思考如何遵循約束的推理過程
4. **輸出格式控制**：結合格式要求，明確約束的範圍
5. **角色提示**：結合角色定義，使約束更自然合理
6. **具體性原則**：用具體的正面指導替代模糊的負面約束
7. **預填回應**：在回應開始處提供正面的開頭，引導正確方向
</combinations>

## 官方參考連結

- [Anthropic Claude 4 Prompt Engineering Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Prompt Engineering Overview - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Zack Witten's Prompt Engineering Workshop](https://www.youtube.com/watch?v=hkhDdcM5V94)
- [PromptLayer Blog: Prompt Engineering with Anthropic Claude](https://blog.promptlayer.com/prompt-engineering-with-anthropic-claude-5399da57461d/)