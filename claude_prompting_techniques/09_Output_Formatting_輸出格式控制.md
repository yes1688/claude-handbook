# 9. 輸出格式控制(Output Formatting)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，輸出格式控制是精確定義 Claude 期望的輸出格式的技術，使用 JSON、XML 或自訂模板來確保 Claude 理解每個輸出格式元素的要求。這項強大的技術允許您指導 Claude 的操作、跳過前言、強制執行特定格式（如 JSON 或 XML），甚至幫助 Claude 在角色扮演場景中保持角色一致性。
</definition>

## 核心原理

<principle>
輸出格式控制基於以下核心原理：
1. **結構化指導**：通過明確的格式規範引導 Claude 的回應結構
2. **模板約束**：使用預定義的模板來限制和標準化輸出
3. **預填技術**：通過預填助手回應來繞過 Claude 的友好前言並強制執行結構
4. **標籤分隔**：使用 XML 標籤清楚地劃分回應的不同部分
5. **程式化解析**：產生易於程式解析和處理的結構化輸出
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔說明的輸出格式控制效益包括：
- **提升一致性**：確保所有回應遵循相同的結構格式
- **增強可解析性**：讓程式能夠可靠地提取特定內容
- **減少歧義**：明確的格式規範減少誤解和錯誤
- **提高效率**：跳過不必要的前言和結尾語，直接產生核心內容
- **便於自動化**：結構化輸出支援自動化處理和整合
- **保持角色一致**：在角色扮演場景中維持一致的回應風格
</benefits>

## 實作格式

### 基本結構
```xml
<instructions>
請按照以下格式回應：
<response_format>
[指定的輸出格式]
</response_format>
</instructions>
```

### 完整結構
```xml
<instructions>
請嚴格按照以下格式回應，不要添加任何額外的前言或結尾語：
</instructions>

<response_format>
<analysis>
[分析內容]
</analysis>
<recommendation>
[建議內容]
</recommendation>
<confidence_score>
[信心評分：1-10]
</confidence_score>
</response_format>

<example>
<analysis>
根據提供的數據，市場趨勢顯示...
</analysis>
<recommendation>
建議採取以下行動：1. 增加投資 2. 擴大市場
</recommendation>
<confidence_score>
8
</confidence_score>
</example>
```

## 實作範例

### 範例1：JSON 格式輸出控制
```xml
<instructions>
分析以下客戶回饋並以 JSON 格式回應：
</instructions>

<customer_feedback>
"產品質量很好，但是配送時間太長了。客服很有耐心，但是價格有點高。"
</customer_feedback>

<response_format>
{
  "sentiment": "正面/負面/中性",
  "key_issues": ["問題1", "問題2"],
  "strengths": ["優點1", "優點2"],
  "action_items": ["建議1", "建議2"],
  "priority": "高/中/低"
}

### 範例2：XML 標籤結構化輸出
```xml
<instructions>
分析以下文件並使用 XML 標籤結構化您的回應：
</instructions>

<document>
公司第三季度銷售報告顯示，總收入達到 500 萬元，比去年同期增長 15%。
主要產品線中，軟體服務收入增長 25%，硬體銷售下降 5%。
</document>

<response_format>
<summary>
[報告摘要]
</summary>
<key_metrics>
<revenue>[總收入]</revenue>
<growth_rate>[增長率]</growth_rate>
<period>[報告期間]</period>
</key_metrics>
<analysis>
<positive_trends>[正面趨勢]</positive_trends>
<concerns>[關注點]</concerns>
</analysis>
<recommendations>
[具體建議]
</recommendations>
</response_format>
```

### 範例3：表格格式輸出控制
```xml
<instructions>
將以下資訊整理成表格格式：
</instructions>

<data>
蘋果：價格 3 元，庫存 100 個，供應商 A
香蕉：價格 2 元，庫存 80 個，供應商 B
橘子：價格 2.5 元，庫存 60 個，供應商 A
</data>

<response_format>
| 產品 | 價格 | 庫存 | 供應商 |
|------|------|------|--------|
| [產品名] | [價格] | [數量] | [供應商] |
</response_format>
```

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your_api_key")

# 方法1：使用 XML 標籤格式控制
def format_xml_output(content):
    message = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1000,
        messages=[
            {
                "role": "user",
                "content": f"""
<instructions>
分析以下內容並使用 XML 標籤格式回應：
</instructions>

<content>
{content}
</content>

<response_format>
<summary>[摘要]</summary>
<key_points>
<point>[要點1]</point>
<point>[要點2]</point>
</key_points>
<conclusion>[結論]</conclusion>
</response_format>
"""
            }
        ]
    )
    return message.content

# 方法2：使用 JSON 格式控制與預填
def format_json_output(data):
    message = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1000,
        messages=[
            {
                "role": "user",
                "content": f"""
分析以下數據並以 JSON 格式回應：
{data}

請使用以下格式：
{{
  "analysis": "分析結果",
  "score": 數值,
  "recommendations": ["建議1", "建議2"]
}}
"""
            },
            {
                "role": "assistant",
                "content": "{"
            }
        ]
    )
    return "{" + message.content

# 方法3：使用停止序列控制
def controlled_output_with_stop(prompt):
    message = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1000,
        messages=[
            {
                "role": "user",
                "content": f"""
{prompt}

<response>
"""
            }
        ],
        stop_sequences=["</response>"]
    )
    return message.content

# 使用範例
content = "市場分析報告：Q3 銷售增長 15%，客戶滿意度提升"
xml_result = format_xml_output(content)
json_result = format_json_output(content)
controlled_result = controlled_output_with_stop("總結今日會議要點")
```

## 最佳實踐

<guidelines>
1. **使用描述性標籤名稱**：選擇能夠反映內容的標籤名稱（如 <instructions>、<example>、<input>）
2. **保持格式一致性**：在整個專案中使用相同的標籤命名約定
3. **層次化結構**：使用嵌套標籤來表示階層關係
4. **預填技術**：使用預填（如 "{"）來強制 JSON 格式並跳過前言
5. **停止序列**：使用 stop_sequences 參數來精確控制輸出結束點
6. **提供範例**：在格式規範中包含具體的輸出範例
7. **明確指示**：清楚說明不要添加額外的前言或結尾語
8. **測試驗證**：定期測試格式控制是否按預期工作
</guidelines>

## 常見錯誤

<mistakes>
1. **格式規範不夠明確**
   - 錯誤：「請使用 JSON 格式」
   - 正確：提供完整的 JSON 結構模板

2. **缺少範例**
   - 錯誤：只描述格式要求
   - 正確：提供具體的輸出範例

3. **標籤名稱不一致**
   - 錯誤：混合使用 <answer> 和 <response>
   - 正確：在整個專案中使用一致的標籤名稱

4. **忽略預填功能**
   - 錯誤：讓 Claude 自由開始回應
   - 正確：使用預填來強制特定格式

5. **未使用停止序列**
   - 錯誤：允許 Claude 添加不必要的結尾語
   - 正確：使用停止序列精確控制輸出結束

6. **複雜度過高**
   - 錯誤：創建過於複雜的嵌套結構
   - 正確：保持格式簡潔易懂
</mistakes>

## 測試與優化

<evaluation>
1. **格式符合性測試**
   - 檢查輸出是否嚴格遵循指定格式
   - 驗證 JSON 語法正確性
   - 確認 XML 標籤完整性

2. **可解析性測試**
   - 使用程式解析輸出內容
   - 測試異常輸入情況
   - 驗證結構化數據提取

3. **一致性測試**
   - 多次運行相同提示
   - 比較格式一致性
   - 檢查邊界情況處理

4. **效率測試**
   - 測量 token 使用量
   - 比較預填與非預填效果
   - 評估處理速度
</evaluation>

<iteration>
1. **格式優化循環**
   - 分析失敗案例
   - 調整格式規範
   - 增加約束條件
   - 測試改進效果

2. **模板完善**
   - 根據使用情況調整模板
   - 增加錯誤處理機制
   - 優化標籤結構
   - 更新最佳實踐

3. **自動化驗證**
   - 建立格式驗證腳本
   - 設置自動化測試
   - 監控格式符合率
   - 持續優化提示
</iteration>

## 與其他技術組合

<combinations>
1. **與系統提示詞結合**
   - 在系統提示詞中定義全域格式規範
   - 確保所有回應遵循一致格式

2. **與多樣本提示結合**
   - 提供多個格式化的範例
   - 展示不同情境下的格式應用

3. **與思維鏈結合**
   - 使用 <thinking> 標籤包含推理過程
   - 用 <answer> 標籤包含最終格式化結果

4. **與預填技術結合**
   - 預填格式開始部分
   - 確保輸出格式精確控制

5. **與工具使用結合**
   - 格式化工具回應
   - 結構化多工具輸出

6. **與角色扮演結合**
   - 定義角色特定的回應格式
   - 維持角色一致性的同時控制格式
</combinations>

## 官方參考連結

- [Anthropic 官方文檔：使用 XML 標籤結構化提示](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags)
- [Anthropic 官方文檔：預填 Claude 回應以增強輸出控制](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response)
- [Anthropic 官方文檔：提高輸出一致性（JSON 模式）](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/increase-consistency)
- [Anthropic 官方課程：格式化輸出和為 Claude 發聲](https://github.com/anthropics/courses/blob/master/prompt_engineering_interactive_tutorial/Anthropic%201P/05_Formatting_Output_and_Speaking_for_Claude.ipynb)
</response_format>