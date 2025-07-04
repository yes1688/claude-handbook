# 13. 工具反思技術(Tool Reflection)

## 官方定義與說明

<definition>
工具反思技術是指在 Claude 使用工具後，引導其分析工具結果的品質、評估執行效果，並確定最佳後續步驟的提示技術。根據 Anthropic 官方文檔，Claude 4 特別支援在工具使用後進行反思和思考，以提高工具使用的準確性和效率。
</definition>

## 核心原理

<principle>
工具反思技術的核心原理是在工具執行的生命週期中建立反思檢查點，使 Claude 能夠：
1. 在工具使用前進行分析和規劃
2. 在工具使用後評估結果品質
3. 基於工具結果調整後續行動
4. 透過 <thinking> 標籤展示推理過程
5. 形成工具使用 → 反思 → 行動的迭代循環
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，工具反思技術能夠：
- 提高工具使用的準確性和相關性
- 減少錯誤的工具調用和參數使用
- 增強複雜多步驟任務的執行效果
- 提供更透明的決策過程
- 改善工具鏈式執行的整體品質
- 幫助開發者調試和優化工具使用流程
</benefits>

## 實作格式

### 基本結構
```xml
<thinking>
分析工具使用的必要性和相關性
檢查必要參數是否齊全
評估工具選擇的合理性
</thinking>

[執行工具]

<reflection>
評估工具結果的品質和相關性
分析是否達到預期目標
確定最佳後續步驟
</reflection>
```

### 完整結構
```xml
<system>
Answer the user's request using relevant tools (if they are available). Before calling a tool, do some analysis within <thinking></thinking> tags. After receiving tool results, carefully reflect on their quality and determine optimal next steps before proceeding. Use your thinking to plan and iterate based on this new information, and then take the best next action.
</system>

<thinking>
[工具使用前的分析]
- 確認工具的相關性
- 檢查必要參數
- 評估預期結果
</thinking>

[工具調用]

<reflection>
[工具使用後的反思]
- 評估結果品質
- 分析完成度
- 確定後續動作
</reflection>
```

## 實作範例

### 範例1：文件搜尋與分析
```xml
<system>
You are helping a user find and analyze information. Before calling any tool, analyze what you need to do within <thinking></thinking> tags. After receiving tool results, reflect on their quality and determine optimal next steps.
</system>

<thinking>
用戶想要尋找關於氣候變化的最新研究。我需要：
1. 使用搜尋工具找到相關文獻
2. 確認搜尋詞彙的準確性
3. 評估搜尋結果的權威性和時效性
</thinking>

[調用搜尋工具: search_academic_papers("climate change 2024")]

<reflection>
搜尋結果返回了12篇2024年的氣候研究論文。品質評估：
- 來源權威：包含Nature、Science等頂級期刊
- 時效性良好：都是2024年發表
- 相關性高：涵蓋氣候模型、影響評估、政策建議

下一步：需要進一步分析這些論文的核心發現，特別是共同趨勢和重要結論。
</reflection>

[基於反思結果執行後續分析]
```

### 範例2：程式碼除錯與修正
```xml
<system>
You are a coding assistant. Before using any tools, think through the problem. After running code or tests, reflect on the results and determine the best next steps.
</system>

<thinking>
用戶報告程式執行時出現記憶體洩漏錯誤。我需要：
1. 檢查程式碼中的記憶體管理
2. 使用診斷工具分析記憶體使用
3. 確認問題的根本原因
</thinking>

[調用記憶體診斷工具: analyze_memory_usage("user_program.py")]

<reflection>
診斷結果顯示：
- 在第47行的迴圈中，物件沒有被正確釋放
- 記憶體使用量隨迭代次數線性增長
- 問題出現在大型資料結構的處理上

根據這些發現，我需要：
1. 修改第47行的記憶體管理代碼
2. 添加適當的垃圾回收機制
3. 重新測試確認問題解決
</reflection>

[執行程式碼修正和重新測試]
```

### 範例3：數據分析與決策支援
```xml
<system>
You are a data analyst. Analyze the request carefully before using tools. After obtaining results, reflect on their implications and provide actionable insights.
</system>

<thinking>
用戶需要分析銷售數據以制定下季度策略。我需要：
1. 載入並檢查數據品質
2. 進行趨勢分析和模式識別
3. 生成可行的業務建議
</thinking>

[調用數據分析工具: analyze_sales_data("Q3_2024_sales.csv")]

<reflection>
分析結果顯示：
- 整體銷售額較去年同期增長15%
- 線上銷售佔比提升至65%
- 某些產品類別出現季節性下滑

重要發現：
- 數位轉型策略成效顯著
- 需要調整產品組合應對季節性變化
- 客戶行為轉向線上購買

基於這些洞察，我應該為用戶提供：
1. 線上銷售進一步優化建議
2. 產品組合調整策略
3. 下季度銷售目標設定
</reflection>

[生成具體的策略建議和行動計劃]
```

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

# 基本工具反思提示
def create_tool_reflection_prompt(user_request, tools_available):
    return f"""
You are an AI assistant with access to various tools. When handling user requests:

1. Before using any tool, analyze the request within <thinking></thinking> tags
2. After receiving tool results, reflect on their quality within <reflection></reflection> tags
3. Determine optimal next steps based on your reflection

Available tools: {tools_available}
User request: {user_request}

Remember: Always think before acting, and reflect after receiving results.
"""

# 進階工具反思實作
def advanced_tool_reflection_system():
    return """
You are an expert assistant with advanced tool reflection capabilities.

For each tool interaction, follow this pattern:
1. <thinking>Pre-tool analysis</thinking>
2. [Tool execution]
3. <reflection>Post-tool evaluation</reflection>
4. [Next action based on reflection]

Always:
- Validate tool parameters before calling
- Assess result quality and relevance
- Plan subsequent actions based on outcomes
- Provide transparent reasoning for your decisions
"""

# 使用範例
response = client.messages.create(
    model="claude-3-opus-20240229",
    max_tokens=1000,
    system=advanced_tool_reflection_system(),
    messages=[
        {
            "role": "user",
            "content": "Please help me analyze the performance of our marketing campaign using the available data tools."
        }
    ]
)

print(response.content)
```

## 最佳實踐

<guidelines>
1. **結構化思考**：使用 <thinking> 標籤進行工具使用前的分析
2. **品質評估**：在 <reflection> 標籤中評估工具結果的品質和相關性
3. **迭代改進**：基於反思結果調整後續行動
4. **透明度**：讓推理過程可見，便於調試和優化
5. **參數驗證**：在工具使用前確認所有必要參數都已準備就緒
6. **錯誤處理**：當工具結果不佳時，要有明確的備用方案
7. **上下文保持**：在多步驟工具使用中保持上下文的連貫性
</guidelines>

## 常見錯誤

<mistakes>
1. **跳過預分析**：直接調用工具而不進行思考分析
   - 錯誤：立即執行工具調用
   - 正確：先用 <thinking> 標籤分析需求和工具適用性

2. **忽略結果品質**：接收工具結果後不進行反思
   - 錯誤：獲得結果後直接使用
   - 正確：用 <reflection> 標籤評估結果品質

3. **缺乏後續規劃**：反思後沒有明確的後續行動
   - 錯誤：反思結束後沒有下一步
   - 正確：基於反思制定具體的後續計劃

4. **過度依賴工具**：不考慮工具的限制和適用性
   - 錯誤：假設工具總是正確的
   - 正確：評估工具結果的可靠性和局限性
</mistakes>

## 測試與優化

<evaluation>
測試工具反思技術的效果可以通過以下方式：

1. **決策品質測試**：比較使用和不使用反思技術的工具使用結果
2. **錯誤率分析**：統計工具誤用和參數錯誤的發生頻率
3. **效率評估**：測量完成複雜任務所需的步驟數和時間
4. **準確性驗證**：檢查最終結果的準確性和相關性
5. **可解釋性評估**：評估推理過程的清晰度和邏輯性
</evaluation>

<iteration>
優化工具反思技術的方法：

1. **調整反思深度**：根據任務複雜性調整反思的詳細程度
2. **改進評估標準**：建立更精確的工具結果品質評估框架
3. **增強錯誤檢測**：提高識別和處理工具錯誤的能力
4. **優化決策邏輯**：改善基於反思結果的後續行動選擇
5. **強化上下文管理**：在多步驟任務中保持更好的上下文連貫性
</iteration>

## 與其他技術組合

<combinations>
工具反思技術可以與以下技術有效組合：

1. **思維鏈提示(Chain of Thought)**：增強反思的邏輯性和結構性
2. **XML標籤結構化**：提供更清晰的反思和思考組織
3. **平行工具執行**：在多工具環境中進行集中反思
4. **系統提示詞**：建立一致的工具反思行為模式
5. **輸出格式控制**：標準化反思和思考的輸出格式
6. **預填回應技術**：引導特定的反思模式和結構
</combinations>

## 官方參考連結

- [Claude 4 最佳實踐](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [工具使用指南](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)
- [提示工程概述](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [系統提示詞設計](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/system-prompts)