# 31. JSON輸出控制(JSON Output Control)

## 官方定義與說明

<definition>
JSON輸出控制是一種確保Claude生成符合預期JSON結構的技術。根據Anthropic官方文檔，這包括使用清晰的格式指示、回應預填、工具定義等方法來控制輸出格式，確保生成的JSON數據具有一致的結構和準確的內容。雖然Claude不像某些模型具備原生JSON模式，但透過精確的提示技術可以實現高度可靠的結構化輸出。
</definition>

## 核心原理

<principle>
JSON輸出控制的核心原理基於以下機制：
1. **格式約束**：透過明確定義所需的JSON結構，包括必要的鍵值對、數據類型和嵌套結構
2. **回應預填**：使用預填技術以開始符號（如"{"）引導Claude跳過友好的前言直接生成JSON
3. **工具系統**：利用Claude的工具功能定義JSON schema，強制輸出符合指定結構
4. **範例訓練**：提供具體的JSON範例幫助Claude理解期望的輸出格式
5. **迭代改進**：透過測試和調整提示來提高輸出一致性
</principle>

## 官方效益

<benefits>
根據Anthropic官方文檔，JSON輸出控制技術提供以下效益：
- 實現高度一致的結構化輸出，減少解析錯誤
- 繞過Claude的友好前言，獲得更簡潔的回應
- 提高程式處理的效率，減少後處理需求
- 支援複雜的數據結構提取和轉換
- 增強API集成的可靠性
- 適用於各種NLP任務，包括命名實體提取、情感分析、文本分類
- 支援未知結構的靈活處理
</benefits>

## 實作格式

### 基本結構
```xml
<instruction>
請以JSON格式回應，包含以下結構：
{
  "key1": "value_type",
  "key2": "value_type"
}
</instruction>

<input>
[待處理的內容]
</input>

<output_format>
{
  "key1": "actual_value",
  "key2": "actual_value"
}
</output_format>
```

### 完整結構
```xml
<system>
你是一個專業的JSON數據提取專家。你的任務是將輸入內容轉換為指定的JSON格式，確保結構完整且數據準確。
</system>

<instruction>
請分析以下內容並以JSON格式回應，嚴格遵循以下結構：

JSON結構要求：
- 所有字段都必須包含
- 數據類型必須正確
- 不要包含額外的說明文字
- 直接輸出JSON，不要前言

期望格式：
{
  "field1": "string",
  "field2": number,
  "field3": ["array", "of", "strings"],
  "field4": {
    "nested_field": "string"
  }
}
</instruction>

<input>
[待處理的內容]
</input>

<prefill>
{
</prefill>
```

## 實作範例

### 範例1：客戶回饋分析
```xml
<system>
你是客戶回饋分析專家。請將客戶回饋轉換為結構化的JSON格式，包含情感分析、關鍵議題和行動建議。
</system>

<instruction>
分析以下客戶回饋並以JSON格式回應：

必要欄位：
- sentiment: "positive", "negative", "neutral"
- confidence: 0-1之間的數值
- key_issues: 字串陣列
- action_items: 字串陣列
- priority: "high", "medium", "low"

不要包含任何解釋文字，直接輸出JSON。
</instruction>

<input>
客戶回饋：「你們的產品很好用，但是客服回應太慢了。希望能改善客服效率，這對我們的業務很重要。」
</input>

<prefill>
{
</prefill>
```

### 範例2：商品資訊提取
```xml
<system>
你是商品資訊提取專家。從產品描述中提取結構化的商品資訊。
</system>

<instruction>
從以下商品描述中提取資訊並以JSON格式回應：

結構要求：
{
  "product_name": "string",
  "brand": "string",
  "price": number,
  "currency": "string",
  "features": ["array of strings"],
  "specifications": {
    "color": "string",
    "size": "string",
    "material": "string"
  },
  "availability": "string"
}

直接輸出JSON，不要任何前言。
</instruction>

<input>
商品描述：「Apple iPhone 15 Pro 256GB 自然鈦金色，售價NT$39,900。採用鈦金屬設計，6.1吋Super Retina XDR顯示器，A17 Pro晶片，現貨供應中。」
</input>

<prefill>
{
</prefill>
```

### 範例3：履歷資訊結構化
```xml
<system>
你是履歷資訊提取專家。將履歷內容轉換為結構化的JSON格式。
</system>

<instruction>
分析以下履歷資訊並以JSON格式回應：

必要結構：
{
  "personal_info": {
    "name": "string",
    "email": "string",
    "phone": "string"
  },
  "experience": [
    {
      "company": "string",
      "position": "string",
      "duration": "string",
      "responsibilities": ["array of strings"]
    }
  ],
  "skills": ["array of strings"],
  "education": {
    "degree": "string",
    "institution": "string",
    "graduation_year": number
  }
}

僅輸出JSON，不要額外文字。
</instruction>

<input>
履歷內容：「王小明，軟體工程師，電話：0912345678，email：wang@example.com。曾任職於科技公司擔任前端開發工程師2年，負責網站開發和維護。精通JavaScript、React、Node.js。畢業於台灣大學資訊工程系，2020年畢業。」
</input>

<prefill>
{
</prefill>
```

## API實作方式

```python
import anthropic

def extract_structured_json(content, schema_description, output_format):
    """
    使用Claude API提取結構化JSON數據
    
    Args:
        content: 待處理的內容
        schema_description: JSON結構描述
        output_format: 期望的輸出格式範例
    
    Returns:
        dict: 解析後的JSON數據
    """
    
    client = anthropic.Anthropic(api_key="your-api-key")
    
    # 構建提示
    prompt = f"""
    <system>
    你是專業的JSON數據提取專家。請將輸入內容轉換為指定的JSON格式。
    </system>
    
    <instruction>
    請分析以下內容並以JSON格式回應：
    
    結構要求：
    {schema_description}
    
    期望格式：
    {output_format}
    
    重要：
    - 嚴格遵循JSON格式
    - 不要包含任何解釋文字
    - 直接輸出JSON
    </instruction>
    
    <input>
    {content}
    </input>
    """
    
    # 發送請求並預填回應
    response = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1000,
        messages=[
            {"role": "user", "content": prompt},
            {"role": "assistant", "content": "{"}  # 預填技術
        ]
    )
    
    # 處理回應
    json_content = "{" + response.content[0].text
    
    try:
        import json
        return json.loads(json_content)
    except json.JSONDecodeError as e:
        print(f"JSON解析錯誤: {e}")
        return None

# 使用工具系統的方法
def extract_with_tools(content, tool_schema):
    """
    使用Claude工具系統提取結構化數據
    """
    
    client = anthropic.Anthropic(api_key="your-api-key")
    
    # 定義工具
    tools = [
        {
            "name": "extract_data",
            "description": "提取結構化數據",
            "input_schema": tool_schema
        }
    ]
    
    response = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1000,
        tools=tools,
        messages=[
            {
                "role": "user", 
                "content": f"請使用extract_data工具分析以下內容：{content}"
            }
        ]
    )
    
    # 處理工具回應
    for content_block in response.content:
        if content_block.type == "tool_use":
            return content_block.input
    
    return None

# 範例使用
if __name__ == "__main__":
    # 一般提示方法
    content = "Apple iPhone 15 Pro 256GB，售價NT$39,900"
    schema = "商品名稱、品牌、價格、規格"
    format_example = '{"name": "string", "brand": "string", "price": number}'
    
    result = extract_structured_json(content, schema, format_example)
    print("提示方法結果:", result)
    
    # 工具方法
    tool_schema = {
        "type": "object",
        "properties": {
            "name": {"type": "string"},
            "brand": {"type": "string"},
            "price": {"type": "number"}
        },
        "required": ["name", "brand", "price"]
    }
    
    result = extract_with_tools(content, tool_schema)
    print("工具方法結果:", result)
```

## 最佳實踐

<guidelines>
1. **明確定義結構**：
   - 清楚指定所有必要欄位
   - 定義數據類型和格式要求
   - 提供具體的JSON範例

2. **使用預填技術**：
   - 以"{"開始預填回應
   - 避免Claude的友好前言
   - 確保輸出格式一致

3. **利用工具系統**：
   - 定義詳細的JSON schema
   - 使用工具強制結構化輸出
   - 適合複雜的數據提取任務

4. **提供清晰指令**：
   - 明確說明不要包含解釋文字
   - 要求直接輸出JSON
   - 指定必要和可選欄位

5. **驗證和測試**：
   - 測試各種輸入情況
   - 驗證JSON格式正確性
   - 檢查數據類型匹配

6. **錯誤處理**：
   - 添加JSON解析錯誤處理
   - 提供備用解析方法
   - 記錄失敗案例以改進提示
</guidelines>

## 常見錯誤

<mistakes>
1. **結構不清晰**：
   - 錯誤：模糊的結構描述
   - 正確：提供具體的JSON範例和欄位說明

2. **缺少預填**：
   - 錯誤：讓Claude自由開始回應
   - 正確：使用"{"預填強制JSON格式

3. **過度複雜**：
   - 錯誤：一次要求太多嵌套結構
   - 正確：分階段處理複雜結構

4. **忽略數據類型**：
   - 錯誤：不指定數據類型
   - 正確：明確指定string、number、array等類型

5. **缺少錯誤處理**：
   - 錯誤：假設總是返回正確JSON
   - 正確：添加解析錯誤處理和驗證

6. **不一致的指令**：
   - 錯誤：指令和範例不匹配
   - 正確：確保指令、範例和期望輸出一致
</mistakes>

## 測試與優化

<evaluation>
評估JSON輸出控制效果的方法：

1. **格式一致性測試**：
   - 測試多個相同類型的輸入
   - 檢查輸出格式的一致性
   - 計算成功率和失敗率

2. **結構完整性驗證**：
   - 驗證所有必要欄位都存在
   - 檢查數據類型是否正確
   - 確認嵌套結構的完整性

3. **邊界情況測試**：
   - 測試空值和特殊字符
   - 處理不完整的輸入數據
   - 驗證錯誤情況的處理

4. **性能評估**：
   - 測量回應時間
   - 評估token使用效率
   - 比較不同方法的效果
</evaluation>

<iteration>
迭代改進JSON輸出控制的方法：

1. **分析失敗案例**：
   - 收集格式不正確的輸出
   - 識別常見的錯誤模式
   - 調整提示策略

2. **優化提示結構**：
   - 測試不同的指令順序
   - 調整範例的複雜度
   - 改進錯誤處理機制

3. **A/B測試**：
   - 比較不同的提示方法
   - 測試工具vs一般提示的效果
   - 評估預填技術的影響

4. **持續監控**：
   - 追蹤輸出質量指標
   - 監控新的錯誤模式
   - 定期更新提示策略
</iteration>

## 與其他技術組合

<combinations>
JSON輸出控制可以與以下技術組合使用：

1. **系統提示詞**：
   - 在系統級別定義JSON專家角色
   - 提供持續的格式約束
   - 確保一致的輸出行為

2. **預填回應技術**：
   - 核心組合，強制JSON格式
   - 跳過Claude的友好前言
   - 提高輸出一致性

3. **多樣本提示**：
   - 提供多個JSON範例
   - 展示各種情況的處理方式
   - 增強格式理解

4. **XML標籤結構化**：
   - 使用XML標籤組織提示
   - 清晰分離指令和內容
   - 提高提示的可讀性

5. **工具使用技術**：
   - 定義JSON schema作為工具輸入
   - 強制結構化輸出
   - 支援複雜的數據驗證

6. **鏈式提示**：
   - 分階段處理複雜JSON結構
   - 先提取基本信息再詳細處理
   - 提高複雜任務的成功率

7. **測試案例開發**：
   - 創建JSON格式的測試用例
   - 驗證輸出格式的正確性
   - 支援自動化測試流程
</combinations>

## 官方參考連結

- [Control Output Format (JSON mode) - Anthropic](https://docs.anthropic.com/claude/docs/control-output-format)
- [Increase Output Consistency - Anthropic](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/increase-consistency)
- [Prefill Claude's Response - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response)
- [Extracting Structured JSON - Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook/blob/main/tool_use/extracting_structured_json.ipynb)
- [Structured Outputs Course - Anthropic](https://github.com/anthropics/courses/blob/master/tool_use/03_structured_outputs.ipynb)