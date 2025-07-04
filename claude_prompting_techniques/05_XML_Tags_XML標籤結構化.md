# 5. XML標籤結構化(XML Tags)

## 官方定義與說明

<definition>
XML標籤是一種使用角括號標記（如 `<tag></tag>`）來結構化提示內容的技術。Anthropic官方文檔指出，XML標籤可以幫助Claude更清楚地區分提示的不同部分，如指令、範例和輸入數據。Claude在訓練過程中接觸了大量使用XML標籤的提示，因此對這種結構化格式特別熟悉。
</definition>

## 核心原理

<principle>
XML標籤系統的核心原理是通過明確的語義標記來分隔和組織提示內容。每個XML標籤都由開始標籤（`<tag>`）和結束標籤（`</tag>`）組成，內容包含在標籤對之間。這種結構化方式讓Claude能夠精確解析提示的各個組成部分，減少混淆和誤解。標籤名稱應具有描述性，能夠清楚表明其包含內容的性質和用途。
</principle>

## 官方效益

<benefits>
根據Anthropic官方文檔，XML標籤技術帶來以下關鍵效益：

1. **提高準確性**：減少Claude因誤解提示部分而產生的錯誤，特別在數學或程式碼生成等領域
2. **增強靈活性**：可以輕鬆地查找、添加、刪除或修改提示的各個部分，無需重寫整個提示
3. **改善可解析性**：讓Claude在輸出中使用XML標籤，便於後處理時提取特定的回應部分
4. **結構化清晰度**：幫助區分指令、範例、輸入數據等不同組件
5. **增強一致性**：在複雜提示中保持格式和結構的一致性
</benefits>

## 實作格式

### 基本結構
```xml
<instructions>
您的主要指令內容
</instructions>

<input>
要處理的輸入數據
</input>

<output>
期望的輸出格式
</output>
```

### 完整結構
```xml
<role>
您是一位專業的數據分析師
</role>

<instructions>
分析以下數據並提供詳細報告
- 識別主要趨勢
- 計算關鍵指標
- 提供改進建議
</instructions>

<format>
請以以下格式回應：
1. 摘要
2. 詳細分析
3. 建議
</format>

<examples>
<example>
<input>銷售額：100萬</input>
<output>
1. 摘要：銷售表現良好
2. 詳細分析：較去年同期增長15%
3. 建議：維持現有策略
</output>
</example>
</examples>

<input>
[實際要分析的數據]
</input>
```

## 實作範例

### 範例1：文件分析任務
```xml
<role>
您是一位專業的法律文件分析師
</role>

<instructions>
分析以下合約條款，識別潛在風險點並提供專業建議
</instructions>

<analysis_criteria>
- 條款公平性
- 法律風險
- 執行可能性
- 修改建議
</analysis_criteria>

<document>
[合約內容]
</document>

<output_format>
<summary>總體評估</summary>
<risks>風險清單</risks>
<recommendations>修改建議</recommendations>
</output_format>
```

### 範例2：程式碼生成任務
```xml
<task>
生成一個Python函數來處理用戶登入驗證
</task>

<requirements>
- 檢查用戶名和密碼
- 返回驗證結果
- 記錄登入嘗試
- 處理異常情況
</requirements>

<format>
<code>
[完整的Python程式碼]
</code>
<explanation>
[程式碼說明]
</explanation>
<usage>
[使用範例]
</usage>
</format>

<constraints>
- 使用標準庫
- 包含錯誤處理
- 添加適當註解
</constraints>
```

### 範例3：多輪對話管理
```xml
<conversation_context>
這是一個客戶服務對話，需要解決用戶的技術問題
</conversation_context>

<user_profile>
- 技術水平：初級
- 問題類型：軟體安裝
- 情緒狀態：焦慮
</user_profile>

<response_guidelines>
- 使用簡單易懂的語言
- 提供分步指導
- 保持耐心友好的語調
- 確認用戶理解每個步驟
</response_guidelines>

<current_message>
[用戶的最新訊息]
</current_message>

<response_format>
<acknowledgment>確認理解用戶問題</acknowledgment>
<solution>分步解決方案</solution>
<verification>確認步驟</verification>
</response_format>
```

## API實作方式

```python
import anthropic

def structured_prompt_with_xml(instructions, input_data, format_spec):
    """
    使用XML標籤結構化提示的API調用
    """
    
    # 構建結構化提示
    prompt = f"""
<instructions>
{instructions}
</instructions>

<input>
{input_data}
</input>

<format>
{format_spec}
</format>

請按照上述格式處理輸入並回應。
"""
    
    client = anthropic.Anthropic()
    
    # 使用stop_sequences確保在指定XML標籤處停止
    response = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1000,
        temperature=0,
        messages=[{"role": "user", "content": prompt}],
        stop_sequences=["</output>"]  # 在輸出標籤結束處停止
    )
    
    return response.content

# 使用範例
instructions = "分析以下數據並提供摘要"
input_data = "Q1銷售額：150萬，Q2銷售額：180萬"
format_spec = """
<analysis>分析結果</analysis>
<summary>關鍵發現</summary>
<recommendations>建議</recommendations>
"""

result = structured_prompt_with_xml(instructions, input_data, format_spec)
print(result)

# 進階：解析XML回應
import re

def parse_xml_response(response_text, tag_name):
    """
    從回應中提取特定XML標籤的內容
    """
    pattern = f"<{tag_name}>(.*?)</{tag_name}>"
    match = re.search(pattern, response_text, re.DOTALL)
    return match.group(1).strip() if match else None

# 提取特定標籤內容
analysis = parse_xml_response(result, "analysis")
summary = parse_xml_response(result, "summary")
recommendations = parse_xml_response(result, "recommendations")
```

## 最佳實踐

<guidelines>
1. **使用描述性標籤名稱**
   - 選擇能清楚表明內容性質的標籤名稱
   - 例如：`<instructions>`, `<example>`, `<format>`, `<thinking>`, `<answer>`
   
2. **保持標籤一致性**
   - 在整個提示中使用相同的標籤命名規則
   - 建立標準化的標籤庫供重複使用

3. **合理嵌套結構**
   - 對於複雜內容，使用適當的標籤嵌套
   - 避免過度嵌套導致結構混亂

4. **確保標籤完整性**
   - 始終包含開始和結束標籤
   - 檢查標籤配對的正確性

5. **結合其他技術**
   - 與多樣本提示結合：`<examples><example>...</example></examples>`
   - 與思維鏈結合：`<thinking>...</thinking><answer>...</answer>`
   
6. **利用標籤控制輸出**
   - 在API調用中使用stop_sequences參數
   - 讓Claude在特定標籤處停止生成

7. **標籤語義化**
   - 標籤名稱應反映其包含內容的語義
   - 避免使用模糊或無意義的標籤名稱
</guidelines>

## 常見錯誤

<mistakes>
1. **標籤不配對**
   - 錯誤：`<instructions>內容</instruction>`
   - 正確：`<instructions>內容</instructions>`

2. **標籤名稱不一致**
   - 錯誤：混合使用`<input>`和`<data>`表示相同概念
   - 正確：在整個提示中統一使用`<input>`

3. **過度嵌套**
   - 錯誤：`<task><subtask><detail><item>內容</item></detail></subtask></task>`
   - 正確：使用適度的嵌套層級

4. **標籤名稱過於通用**
   - 錯誤：`<tag1>`, `<content>`, `<stuff>`
   - 正確：`<instructions>`, `<example>`, `<format>`

5. **忽略標籤語義**
   - 錯誤：在`<example>`標籤中放入指令
   - 正確：確保標籤內容與標籤名稱語義一致

6. **XML格式錯誤**
   - 錯誤：`<tag attribute="value">內容</tag>`（避免使用屬性）
   - 正確：`<tag>內容</tag>`（簡潔的XML結構）

7. **標籤位置不當**
   - 錯誤：將重要指令放在不顯眼的標籤中
   - 正確：將核心指令放在明顯的`<instructions>`標籤中
</mistakes>

## 測試與優化

<evaluation>
1. **結構清晰度測試**
   - 檢查是否每個標籤的內容都清楚易懂
   - 驗證標籤嵌套的邏輯性

2. **回應準確性測試**
   - 比較使用XML標籤前後的回應品質
   - 測試Claude是否正確解析了各個標籤部分

3. **解析效率測試**
   - 驗證能否成功從回應中提取特定標籤內容
   - 測試正則表達式或其他解析方法的準確性

4. **一致性測試**
   - 使用相同的XML結構重複測試
   - 確保Claude的回應保持一致的格式
</evaluation>

<iteration>
1. **標籤優化**
   - 根據實際使用效果調整標籤名稱
   - 簡化過於複雜的標籤結構

2. **內容完善**
   - 在標籤中添加更具體的指導
   - 優化標籤內容的組織方式

3. **結構調整**
   - 根據使用情況調整標籤順序
   - 優化標籤的嵌套關係

4. **性能提升**
   - 測試不同標籤組合的效果
   - 找出最佳的標籤配置方案
</iteration>

## 與其他技術組合

<combinations>
1. **XML標籤 + 多樣本提示**
   ```xml
   <instructions>分析客戶評論的情感傾向</instructions>
   <examples>
   <example>
   <input>這個產品真的很棒！</input>
   <output>正面情感</output>
   </example>
   <example>
   <input>質量很差，很失望</input>
   <output>負面情感</output>
   </example>
   </examples>
   ```

2. **XML標籤 + 思維鏈**
   ```xml
   <problem>計算複合利息</problem>
   <thinking>
   首先確定公式：A = P(1 + r)^t
   然後代入數值...
   </thinking>
   <answer>最終結果</answer>
   ```

3. **XML標籤 + 角色提示**
   ```xml
   <role>您是一位經驗豐富的財務顧問</role>
   <context>客戶想要投資建議</context>
   <instructions>提供個人化的投資建議</instructions>
   ```

4. **XML標籤 + 預填回應**
   ```xml
   <task>總結會議要點</task>
   <format>
   <summary>
   會議摘要：
   ```

5. **XML標籤 + 長上下文處理**
   ```xml
   <document_type>研究報告</document_type>
   <document_content>
   [長篇文件內容]
   </document_content>
   <analysis_focus>關鍵發現和建議</analysis_focus>
   ```
</combinations>

## 官方參考連結

- [Anthropic官方文檔：使用XML標籤結構化提示](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags)
- [Anthropic提示工程概述](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Claude API文檔](https://docs.anthropic.com/en/api/getting-started)