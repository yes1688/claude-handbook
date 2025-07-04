# 3. 多樣本提示(Multishot Prompting)

## 官方定義與說明

<definition>
多樣本提示(Multishot Prompting)是一種提示工程技術，透過在單一提示中提供3-5個多樣化且相關的範例，來引導Claude的行為和輸出生成。此技術也被稱為少樣本提示(Few-shot Prompting)，是透過示例來教導模型如何執行特定任務的有效方法。
</definition>

## 核心原理

<principle>
多樣本提示的核心原理基於機器學習中的少樣本學習(Few-shot Learning)概念：
1. **模式識別**：透過多個範例，Claude能夠識別輸入與輸出之間的模式
2. **上下文學習**：利用範例建立上下文，使模型理解任務的具體要求
3. **結構化學習**：範例展示了預期的輸出格式和結構
4. **邊界條件理解**：多樣化的範例幫助模型理解任務的邊界和變化
</principle>

## 官方效益

<benefits>
根據Anthropic官方文檔，多樣本提示提供以下效益：
- **提高準確性**：顯著減少指令誤解，提升輸出精確度
- **強化一致性**：確保統一的輸出結構和風格
- **增強複雜任務處理**：對於複雜任務，更多範例帶來更好的性能表現
- **減少變異性**：降低生成回應的不一致性
- **改善理解**：幫助Claude理解細微的任務要求
</benefits>

## 實作格式

### 基本結構
```xml
<examples>
<example>
Input: [輸入範例]
Output: [輸出範例]
</example>
</examples>

[實際任務]
Input: [真實輸入]
```

### 完整結構
```xml
<task>
[任務描述]
</task>

<examples>
<example>
Input: [輸入範例1]
Output: [輸出範例1]
</example>

<example>
Input: [輸入範例2]
Output: [輸出範例2]
</example>

<example>
Input: [輸入範例3]
Output: [輸出範例3]
</example>
</examples>

<instructions>
請根據上述範例，處理以下實際任務：
</instructions>

Input: [實際輸入]
```

## 實作範例

### 範例1：客戶反饋分析
```xml
<task>
分析客戶反饋，並分類為正面、負面或中性，同時提取關鍵主題。
</task>

<examples>
<example>
Input: "產品質量很好，但是運送時間太長了。"
Output: {
  "sentiment": "中性",
  "themes": ["產品質量", "運送時間"],
  "details": "正面：產品質量好；負面：運送時間長"
}
</example>

<example>
Input: "客服態度很差，完全不幫忙解決問題。"
Output: {
  "sentiment": "負面",
  "themes": ["客服態度", "問題解決"],
  "details": "客服服務品質不佳，未能提供有效協助"
}
</example>

<example>
Input: "這個APP界面設計很直觀，功能也很實用。"
Output: {
  "sentiment": "正面",
  "themes": ["界面設計", "功能實用性"],
  "details": "用戶對產品的使用體驗表示滿意"
}
</example>
</examples>

<instructions>
請根據上述範例格式，分析以下客戶反饋：
</instructions>

Input: "價格合理，但是說明書寫得不夠清楚。"
```

### 範例2：程式碼註解生成
```xml
<task>
為Python函數生成專業的文檔字符串註解。
</task>

<examples>
<example>
Input: 
```python
def add_numbers(a, b):
    return a + b
```
Output:
```python
def add_numbers(a, b):
    """
    將兩個數字相加並返回結果。
    
    Args:
        a (int/float): 第一個數字
        b (int/float): 第二個數字
        
    Returns:
        int/float: 兩個數字的和
        
    Example:
        >>> add_numbers(3, 5)
        8
    """
    return a + b
```
</example>

<example>
Input:
```python
def find_max(numbers):
    if not numbers:
        return None
    return max(numbers)
```
Output:
```python
def find_max(numbers):
    """
    在數字列表中找到最大值。
    
    Args:
        numbers (list): 包含數字的列表
        
    Returns:
        int/float/None: 列表中的最大值，如果列表為空則返回None
        
    Example:
        >>> find_max([1, 3, 2, 8, 5])
        8
        >>> find_max([])
        None
    """
    if not numbers:
        return None
    return max(numbers)
```
</example>

<example>
Input:
```python
def calculate_area(length, width):
    return length * width
```
Output:
```python
def calculate_area(length, width):
    """
    計算矩形面積。
    
    Args:
        length (int/float): 矩形的長度
        width (int/float): 矩形的寬度
        
    Returns:
        int/float: 矩形的面積
        
    Raises:
        ValueError: 如果長度或寬度為負數
        
    Example:
        >>> calculate_area(5, 3)
        15
    """
    return length * width
```
</example>
</examples>

<instructions>
請為以下函數生成專業的文檔字符串註解：
</instructions>

Input:
```python
def validate_email(email):
    import re
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None
```
```

### 範例3：會議記錄摘要
```xml
<task>
將會議記錄整理成結構化摘要，包含重點決議、行動項目和負責人。
</task>

<examples>
<example>
Input: "今天討論了新產品上市計劃。市場部張三建議在第三季度推出，研發部李四說還需要兩個月完成測試。最終決定九月份正式上市，由王五負責協調各部門。"
Output: {
  "會議主題": "新產品上市計劃",
  "重點決議": [
    "新產品將於九月份正式上市"
  ],
  "行動項目": [
    {
      "任務": "完成產品測試",
      "負責人": "研發部李四",
      "期限": "兩個月內"
    },
    {
      "任務": "協調各部門上市準備",
      "負責人": "王五",
      "期限": "九月份前"
    }
  ],
  "參與部門": ["市場部", "研發部"]
}
</example>

<example>
Input: "討論預算分配問題。財務部報告Q2預算使用率80%，建議Q3控制在70%以內。各部門需要在下周提交預算調整方案。"
Output: {
  "會議主題": "預算分配",
  "重點決議": [
    "Q3預算使用率需控制在70%以內"
  ],
  "行動項目": [
    {
      "任務": "提交預算調整方案",
      "負責人": "各部門",
      "期限": "下周"
    }
  ],
  "重要數據": [
    "Q2預算使用率：80%",
    "Q3目標使用率：70%"
  ]
}
</example>

<example>
Input: "安全培訓會議。人事部陳六介紹新的安全規定，所有員工必須完成線上培訓。IT部門負責建置培訓系統，預計下個月上線。"
Output: {
  "會議主題": "安全培訓",
  "重點決議": [
    "所有員工必須完成新的安全規定培訓"
  ],
  "行動項目": [
    {
      "任務": "建置線上培訓系統",
      "負責人": "IT部門",
      "期限": "下個月"
    },
    {
      "任務": "推廣安全規定培訓",
      "負責人": "人事部陳六",
      "期限": "系統上線後"
    }
  ],
  "參與部門": ["人事部", "IT部門"]
}
</example>
</examples>

<instructions>
請將以下會議記錄整理成結構化摘要：
</instructions>

Input: "產品優化會議。用戶反饋系統響應速度慢，技術部經理建議升級服務器。預算需要50萬，需要董事會批准。預計升級後性能提升30%。"
```

## API實作方式

```python
import anthropic

client = anthropic.Client(api_key="your-api-key")

# 多樣本提示範例：情感分析
def multishot_sentiment_analysis(text):
    prompt = """
<task>
分析文本的情感傾向，並提供信心分數(0-1)。
</task>

<examples>
<example>
Input: "今天天氣真好，心情很愉快！"
Output: {
  "sentiment": "正面",
  "confidence": 0.9,
  "keywords": ["天氣真好", "心情愉快"]
}
</example>

<example>
Input: "這個服務還算可以，不過有改進空間。"
Output: {
  "sentiment": "中性",
  "confidence": 0.7,
  "keywords": ["還算可以", "改進空間"]
}
</example>

<example>
Input: "完全不滿意，浪費時間和金錢。"
Output: {
  "sentiment": "負面",
  "confidence": 0.95,
  "keywords": ["不滿意", "浪費時間", "浪費金錢"]
}
</example>
</examples>

<instructions>
請分析以下文本的情感傾向：
</instructions>

Input: """ + text
    
    response = client.messages.create(
        model="claude-3-5-sonnet-20241022",
        max_tokens=1000,
        messages=[
            {"role": "user", "content": prompt}
        ]
    )
    
    return response.content[0].text

# 使用範例
result = multishot_sentiment_analysis("服務品質還不錯，但是價格有點貴。")
print(result)
```

## 最佳實踐

<guidelines>
1. **範例數量**：建議使用3-5個範例，過多可能導致上下文過長
2. **範例多樣性**：確保範例涵蓋不同情況和邊界條件
3. **範例相關性**：所有範例都應與實際任務直接相關
4. **結構一致性**：所有範例應使用相同的格式和結構
5. **使用XML標籤**：用`<example>`標籤清楚分隔每個範例
6. **範例品質**：確保範例展示正確和理想的輸出
7. **漸進複雜度**：從簡單到複雜安排範例順序
8. **評估機制**：讓Claude評估範例的相關性、多樣性和清晰度
</guidelines>

## 常見錯誤

<mistakes>
1. **範例不一致**：
   - 錯誤：範例使用不同的輸出格式
   - 正確：保持所有範例的格式統一

2. **範例不相關**：
   - 錯誤：使用與實際任務無關的範例
   - 正確：選擇直接相關的範例

3. **範例品質低**：
   - 錯誤：範例包含錯誤或不理想的輸出
   - 正確：確保範例展示最佳實踐

4. **範例過多**：
   - 錯誤：提供過多範例導致上下文混亂
   - 正確：限制在3-5個高質量範例

5. **缺乏多樣性**：
   - 錯誤：所有範例都非常相似
   - 正確：涵蓋不同情況和邊界條件

6. **標籤不清**：
   - 錯誤：範例之間界限不明
   - 正確：使用清晰的XML標籤分隔
</mistakes>

## 測試與優化

<evaluation>
評估多樣本提示效果的方法：
1. **一致性測試**：使用相同類型的輸入測試多次，檢查輸出一致性
2. **覆蓋率測試**：測試不同類型的輸入，確保範例涵蓋度足夠
3. **邊界測試**：測試極端情況，驗證範例的指導效果
4. **對比測試**：比較有無範例的輸出差異
5. **人工評估**：由專家評估輸出品質和準確性
</evaluation>

<iteration>
迭代改進策略：
1. **範例優化**：根據錯誤案例調整或增加範例
2. **範例多樣化**：識別未涵蓋的情況並添加相應範例
3. **格式調整**：優化範例的結構和格式
4. **數量平衡**：調整範例數量以達到最佳效果
5. **品質提升**：替換低質量範例為更好的示例
6. **讓Claude參與**：要求Claude評估和改進範例集合
</iteration>

## 與其他技術組合

<combinations>
多樣本提示可以與以下技術有效組合：

1. **系統提示詞**：在系統級別設定角色，用多樣本提示具體化任務
2. **思維鏈提示**：在範例中展示推理過程
3. **XML結構化**：使用XML標籤組織範例和輸出
4. **輸出格式控制**：透過範例展示特定的輸出格式
5. **角色提示**：結合角色設定和任務範例
6. **負面提示**：在範例中包含應避免的情況
7. **鏈式提示**：將多樣本提示用於複雜任務的各個階段

組合使用範例：
```xml
<role>你是一位專業的數據分析師</role>

<examples>
<example>
Input: [數據集]
Thinking: 首先分析數據分佈，然後識別異常值...
Output: [分析結果]
</example>
</examples>
```
</combinations>

## 官方參考連結

[Anthropic 官方文檔 - Use examples (multishot prompting)](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/multishot-prompting)

[Anthropic 提示工程概述](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)

[AWS Claude v3 少樣本提示範例](https://github.com/aws-samples/prompt-engineering-with-anthropic-claude-v-3/blob/main/07_Using_Examples_Few-Shot_Prompting.ipynb)