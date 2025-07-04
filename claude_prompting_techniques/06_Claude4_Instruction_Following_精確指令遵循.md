# 6. 精確指令遵循(Claude4 Instruction Following)

## 官方定義與說明

<definition>
Claude 4 模型經過訓練，具備比之前版本更精確的指令遵循能力。Claude 4 模型對清晰、明確的指令響應良好，能夠準確理解和執行複雜的任務要求。這項技術要求使用者提供具體且詳細的指導，以充分發揮 Claude 4 的高級推理和執行能力。
</definition>

## 核心原理

<principle>
Claude 4 的精確指令遵循技術基於以下核心原理：
1. **明確性優先**：模型需要清晰、具體的指令，避免模糊或隱含的要求
2. **動機理解**：提供指令背後的動機和背景，幫助模型更好地理解目標
3. **範例對齊**：確保提供的範例與期望的行為保持一致
4. **格式匹配**：提示的格式風格會影響回應的格式風格
5. **特徵明確**：需要明確要求特定功能或行為，而非依賴模型的推測
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，Claude 4 精確指令遵循技術帶來以下效益：
- 更準確的任務執行和結果輸出
- 減少誤解和偏離指令的情況
- 提高複雜任務的完成品質
- 增強對特定格式和風格要求的遵循能力
- 改善程式碼生成和前端開發的準確性
- 提供更一致和可預測的回應
</benefits>

## 實作格式

### 基本結構
```xml
<instruction>
[明確的指令描述]
</instruction>

<context>
[提供動機和背景資訊]
</context>

<requirements>
[具體要求清單]
</requirements>
```

### 完整結構
```xml
<instruction>
[主要指令]
</instruction>

<context>
[背景資訊和動機]
</context>

<requirements>
- [具體要求1]
- [具體要求2]
- [具體要求3]
</requirements>

<examples>
[符合期望行為的範例]
</examples>

<output_format>
[期望的輸出格式]
</output_format>

<evaluation_criteria>
[成功標準]
</evaluation_criteria>
```

## 實作範例

### 範例1：程式碼生成任務
```xml
<instruction>
為電子商務網站創建一個產品展示頁面的 React 組件
</instruction>

<context>
這個組件將用於展示產品詳情，需要提供良好的用戶體驗和視覺設計。請包含盡可能多的相關功能和互動效果。
</context>

<requirements>
- 使用 React 函數組件和 hooks
- 包含產品圖片輪播功能
- 添加懸停狀態、過渡效果和微互動
- 實現響應式設計
- 包含購買按鈕和數量選擇器
- 添加產品評價展示區域
- 使用現代 CSS 或 styled-components
</requirements>

<examples>
好的實作應包含：
- 平滑的圖片切換動畫
- 按鈕的懸停效果
- 載入狀態指示器
- 錯誤處理機制
</examples>

<output_format>
請提供完整的 React 組件代碼，包含必要的 import 語句和 CSS 樣式。
</output_format>
```

### 範例2：數據分析任務
```xml
<instruction>
分析提供的銷售數據並生成詳細報告
</instruction>

<context>
這個分析報告將用於月度業務檢討會議，需要為管理層提供可操作的洞察。分析的準確性和深度對業務決策至關重要。
</context>

<requirements>
- 識別銷售趨勢和季節性模式
- 計算關鍵績效指標（KPIs）
- 提供具體的改進建議
- 使用圖表和視覺化來支持發現
- 包含風險因素分析
- 提供短期和長期預測
</requirements>

<examples>
報告應包含：
- 同比增長率分析
- 產品類別表現比較
- 地區銷售分佈
- 客戶細分洞察
</examples>

<output_format>
請以結構化的商業報告格式呈現，包含執行摘要、詳細分析和建議。
</output_format>

<evaluation_criteria>
- 數據準確性和計算正確性
- 洞察的深度和實用性
- 報告的專業性和可讀性
- 建議的可操作性
</evaluation_criteria>
```

### 範例3：創意寫作任務
```xml
<instruction>
撰寫一篇關於人工智能倫理的深度文章
</instruction>

<context>
這篇文章將發表在技術雜誌上，目標讀者是技術專業人士和政策制定者。文章需要平衡技術深度和可讀性，提供有價值的觀點和建議。
</context>

<requirements>
- 文章長度約 2000-2500 字
- 包含當前 AI 倫理的主要議題
- 提供具體的案例分析
- 引用可靠的研究和資料來源
- 保持客觀和平衡的觀點
- 包含對未來發展的預測
- 使用專業但易懂的語言
</requirements>

<examples>
文章應涵蓋：
- AI 偏見和公平性問題
- 隱私和數據保護
- 自動化對就業的影響
- AI 決策的透明度
- 監管和治理挑戰
</examples>

<output_format>
請使用標準的文章格式，包含：
- 引人入勝的標題
- 摘要段落
- 清晰的章節結構
- 結論和建議
- 參考文獻列表
</output_format>

<evaluation_criteria>
- 內容的準確性和時效性
- 論述的邏輯性和說服力
- 語言的專業性和可讀性
- 觀點的原創性和深度
</evaluation_criteria>
```

## API實作方式

```python
import anthropic

client = anthropic.Client(api_key="your-api-key")

# Claude 4 精確指令遵循實作
def claude4_instruction_following(task, context, requirements):
    """
    使用 Claude 4 的精確指令遵循功能
    """
    
    # 構建結構化提示
    prompt = f"""
<instruction>
{task}
</instruction>

<context>
{context}
</context>

<requirements>
{chr(10).join(f"- {req}" for req in requirements)}
</requirements>

<output_format>
請提供完整、詳細的回應，確保滿足所有要求。
</output_format>
"""
    
    response = client.messages.create(
        model="claude-sonnet-4-20250514",  # 使用 Claude 4 模型
        max_tokens=4000,
        temperature=0.1,  # 較低溫度確保一致性
        messages=[
            {
                "role": "user",
                "content": prompt
            }
        ]
    )
    
    return response.content[0].text

# 使用範例
task = "創建一個響應式的登錄頁面"
context = "這個頁面需要具有現代設計感，適用於 SaaS 產品，目標用戶是企業專業人士"
requirements = [
    "使用 HTML5 和 CSS3",
    "包含表單驗證功能",
    "添加載入狀態和錯誤處理",
    "確保在移動設備上良好顯示",
    "包含懸停效果和過渡動畫"
]

result = claude4_instruction_following(task, context, requirements)
print(result)

# 進階實作：帶有評估標準的版本
def claude4_advanced_instruction_following(
    instruction, 
    context, 
    requirements, 
    examples=None, 
    evaluation_criteria=None
):
    """
    進階版本的 Claude 4 指令遵循
    """
    
    prompt_parts = [
        f"<instruction>\n{instruction}\n</instruction>",
        f"<context>\n{context}\n</context>",
        f"<requirements>\n{chr(10).join(f'- {req}' for req in requirements)}\n</requirements>"
    ]
    
    if examples:
        prompt_parts.append(f"<examples>\n{examples}\n</examples>")
    
    if evaluation_criteria:
        criteria_text = chr(10).join(f"- {criteria}" for criteria in evaluation_criteria)
        prompt_parts.append(f"<evaluation_criteria>\n{criteria_text}\n</evaluation_criteria>")
    
    prompt = "\n\n".join(prompt_parts)
    
    response = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=4000,
        temperature=0.1,
        messages=[
            {
                "role": "user",
                "content": prompt
            }
        ]
    )
    
    return response.content[0].text

# 批次處理多個指令
def batch_instruction_following(instructions_list):
    """
    批次處理多個指令遵循任務
    """
    results = []
    
    for instruction_data in instructions_list:
        result = claude4_advanced_instruction_following(**instruction_data)
        results.append({
            "instruction": instruction_data["instruction"],
            "result": result
        })
    
    return results
```

## 最佳實踐

<guidelines>
1. **明確具體的指令**
   - 使用清晰、具體的語言描述任務
   - 避免模糊或隱含的要求
   - 提供詳細的功能和特徵說明

2. **提供動機和背景**
   - 解釋為什麼需要執行這個任務
   - 說明任務的重要性和用途
   - 提供相關的背景資訊

3. **使用結構化格式**
   - 採用 XML 標籤組織提示內容
   - 明確區分不同類型的資訊
   - 保持邏輯清晰的結構

4. **提供具體要求**
   - 列出具體的功能要求
   - 指定技術標準和限制
   - 明確成功標準

5. **範例對齊**
   - 確保提供的範例符合期望行為
   - 避免與目標不一致的範例
   - 使用多個範例展示不同面向

6. **格式匹配**
   - 提示的格式風格應與期望輸出匹配
   - 使用相同的語言風格和術語
   - 保持一致的專業水準

7. **明確功能要求**
   - 直接說明需要什麼功能
   - 避免依賴模型的推測
   - 明確指出重要的技術細節
</guidelines>

## 常見錯誤

<mistakes>
1. **指令過於模糊**
   - 錯誤：「請幫我寫一個網頁」
   - 正確：「請創建一個包含產品展示、購物車和結帳功能的電子商務網頁」

2. **缺乏背景資訊**
   - 錯誤：直接提出任務要求
   - 正確：解釋任務的用途和重要性

3. **範例不一致**
   - 錯誤：提供與目標行為不符的範例
   - 正確：確保所有範例都支持期望的行為

4. **依賴隱含假設**
   - 錯誤：假設模型會自動添加某些功能
   - 正確：明確要求所需的所有功能

5. **格式不匹配**
   - 錯誤：使用隨意的格式描述正式任務
   - 正確：使用與期望輸出匹配的格式風格

6. **缺乏具體標準**
   - 錯誤：「做得好一些」
   - 正確：「包含錯誤處理、載入狀態和響應式設計」

7. **忽略評估標準**
   - 錯誤：沒有說明如何評估成功
   - 正確：提供明確的評估標準和成功指標
</mistakes>

## 測試與優化

<evaluation>
評估 Claude 4 指令遵循效果的方法：

1. **完成度檢查**
   - 驗證所有要求是否都被滿足
   - 檢查是否遺漏重要功能
   - 確認輸出格式是否符合要求

2. **品質評估**
   - 評估輸出的專業水準
   - 檢查技術準確性
   - 驗證邏輯一致性

3. **一致性測試**
   - 使用相同指令多次測試
   - 比較不同測試的結果
   - 評估輸出的穩定性

4. **邊界測試**
   - 測試複雜和簡單的指令
   - 評估在不同領域的表現
   - 測試極端情況的處理

5. **用戶反饋**
   - 收集實際使用者的反饋
   - 評估實用性和滿意度
   - 識別改進機會
</evaluation>

<iteration>
迭代改進 Claude 4 指令遵循的方法：

1. **分析失敗案例**
   - 識別指令不明確的地方
   - 找出遺漏的重要資訊
   - 改善範例的品質

2. **細化要求**
   - 增加更具體的說明
   - 提供更多背景資訊
   - 明確技術細節

3. **優化結構**
   - 改善提示的組織方式
   - 調整 XML 標籤的使用
   - 增強邏輯流程

4. **增強範例**
   - 提供更多相關範例
   - 確保範例的多樣性
   - 改善範例的品質

5. **A/B 測試**
   - 比較不同版本的指令
   - 測試不同的格式風格
   - 評估各種方法的效果

6. **持續監控**
   - 建立績效指標
   - 定期評估效果
   - 及時調整策略
</iteration>

## 與其他技術組合

<combinations>
Claude 4 精確指令遵循可以與以下技術有效組合：

1. **與系統提示詞結合**
   - 在系統提示中定義角色和基本行為
   - 在用戶提示中提供具體指令
   - 創建一致的互動風格

2. **與思維鏈結合**
   - 要求模型解釋推理過程
   - 提供分步驟的思考指導
   - 增強複雜任務的執行品質

3. **與 XML 標籤結合**
   - 使用 XML 標籤結構化指令
   - 提高指令的清晰度
   - 確保格式的一致性

4. **與多樣本提示結合**
   - 提供多個示範範例
   - 展示不同情境的處理方式
   - 提高指令遵循的準確性

5. **與輸出格式控制結合**
   - 明確指定輸出格式
   - 確保結果的結構化
   - 便於後續處理

6. **與工具使用結合**
   - 指導工具的使用方式
   - 明確工具使用的順序
   - 優化工具執行的效果

7. **與迭代改進結合**
   - 建立反饋循環
   - 持續優化指令品質
   - 提高長期效果
</combinations>

## 官方參考連結

- [Claude 4 Prompt Engineering Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Migrating to Claude 4](https://docs.anthropic.com/en/docs/about-claude/models/migrating-to-claude-4)
- [Build with Claude API Documentation](https://docs.anthropic.com/en/docs/build-with-claude)
- [Anthropic Claude 4 Official Announcement](https://www.anthropic.com/news/claude-4)