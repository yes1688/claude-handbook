# 8. 使用修飾詞增強(Modifiers)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，修飾詞(Modifiers)是指在指令中添加特定的詞語或短語，用以提高 Claude 輸出的品質、詳細程度和表現力。修飾詞能夠引導 Claude 提供更全面、更深入的回應，避免過於簡潔或表面的輸出。這是一種透過語言增強來改善 AI 表現的技術。
</definition>

## 核心原理

<principle>
修飾詞技術的核心原理是透過特定的語言暗示來激發 Claude 的潛力。當接收到包含修飾詞的指令時，Claude 會調整其回應模式，提供更豐富、更詳細的內容。修飾詞作為質量導向的提示，能夠：

1. 激勵 Claude 提供超出基本要求的內容
2. 引導 Claude 思考更多細節和可能性
3. 促使 Claude 展現更高水準的創作能力
4. 確保輸出符合專業標準和期望

修飾詞本質上是一種心理暗示機制，通過正面的語言框架來提升 AI 的表現標準。
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔指出，使用修飾詞可以帶來以下效益：

1. **提升輸出品質**：鼓勵 Claude 產生更高品質、更詳細的回應
2. **增加內容豐富度**：促使 Claude 提供更全面的資訊和洞察
3. **提高專業水準**：引導 Claude 展現領域專業知識和技能
4. **改善創作表現**：激發 Claude 的創意和想像力
5. **確保完整性**：避免過於簡潔或不完整的輸出
6. **增強用戶滿意度**：提供超出期望的回應品質

官方特別強調，修飾詞是一種簡單但有效的技術，能夠在不增加複雜性的情況下顯著改善 AI 表現。
</benefits>

## 實作格式

### 基本結構
```xml
<instruction>
[基本指令] + [品質修飾詞]
</instruction>
```

### 完整結構
```xml
<instruction>
[具體任務描述]
</instruction>

<modifiers>
<quality_enhancement>
[品質提升修飾詞]
</quality_enhancement>

<scope_expansion>
[範圍擴展修飾詞]
</scope_expansion>

<performance_boost>
[表現增強修飾詞]
</performance_boost>
</modifiers>

<context>
[背景資訊和期望]
</context>
```

## 實作範例

### 範例1：程式碼開發增強
```xml
<instruction>
創建一個分析儀表板的 React 組件
</instruction>

<modifiers>
<quality_enhancement>
包含盡可能多的相關功能和交互效果
</quality_enhancement>

<scope_expansion>
超越基本需求，創建一個功能完整的實作
</scope_expansion>

<performance_boost>
展現出色的網頁開發能力，創建令人印象深刻的演示
</performance_boost>

<design_principles>
應用設計原則：層次、對比、平衡和動態效果
</design_principles>
</modifiers>

<context>
這個儀表板將用於企業級數據分析，需要專業的外觀和豐富的功能
</context>
```

### 範例2：內容創作增強
```xml
<instruction>
撰寫一篇關於永續能源的文章
</instruction>

<modifiers>
<quality_enhancement>
提供深入的分析和見解，不要保留
</quality_enhancement>

<scope_expansion>
涵蓋技術、經濟、環境和社會各個面向
</scope_expansion>

<performance_boost>
展現專業知識和批判性思維，全力以赴
</performance_boost>

<detail_level>
包含具體數據、案例研究和未來趨勢預測
</detail_level>
</modifiers>

<context>
文章將發表在學術期刊上，讀者是能源政策專家
</context>
```

### 範例3：設計任務增強
```xml
<instruction>
設計一個行動應用程式的用戶介面
</instruction>

<modifiers>
<quality_enhancement>
創建精美且直觀的設計，追求卓越
</quality_enhancement>

<scope_expansion>
加入貼心的細節如懸停狀態、轉場動畫和微互動
</scope_expansion>

<performance_boost>
展現頂尖的 UI/UX 設計能力，超越期望
</performance_boost>

<innovation_focus>
融入創新的設計概念和最新的設計趨勢
</innovation_focus>
</modifiers>

<context>
這是一個金融科技應用，需要平衡專業性和易用性
</context>
```

## API實作方式

```python
import anthropic

client = anthropic.Client(api_key="your-api-key")

def enhanced_prompt_with_modifiers(task, modifiers_list, context=""):
    """
    使用修飾詞增強提示詞的函數
    
    Args:
        task: 基本任務描述
        modifiers_list: 修飾詞列表
        context: 額外的背景資訊
    """
    
    # 基本修飾詞模板
    quality_modifiers = [
        "全力以赴",
        "不要保留",
        "展現專業水準",
        "超越期望",
        "追求卓越"
    ]
    
    scope_modifiers = [
        "包含盡可能多的相關功能",
        "超越基本需求",
        "提供全面的解決方案",
        "涵蓋所有重要面向",
        "深入探討細節"
    ]
    
    # 建構增強提示詞
    enhanced_prompt = f"""
    <instruction>
    {task}
    </instruction>
    
    <modifiers>
    <quality_enhancement>
    {' '.join(quality_modifiers[:2])}
    </quality_enhancement>
    
    <scope_expansion>
    {' '.join(scope_modifiers[:2])}
    </scope_expansion>
    
    <performance_boost>
    {' '.join(modifiers_list)}
    </performance_boost>
    </modifiers>
    
    <context>
    {context}
    </context>
    """
    
    return enhanced_prompt

# 使用範例
task = "創建一個資料視覺化圖表"
modifiers = ["展現數據分析專業知識", "創建互動式體驗", "應用最佳實踐"]
context = "圖表用於高階主管簡報，需要清晰且有說服力"

enhanced_prompt = enhanced_prompt_with_modifiers(task, modifiers, context)

response = client.messages.create(
    model="claude-3-sonnet-20240229",
    max_tokens=4000,
    temperature=0.7,
    messages=[{
        "role": "user", 
        "content": enhanced_prompt
    }]
)

print(response.content)
```

## 最佳實踐

<guidelines>
1. **選擇適當的修飾詞**：根據任務類型選擇最相關的修飾詞，避免過度使用

2. **平衡激勵與指導**：修飾詞應該既激勵 Claude 又提供明確的方向

3. **考慮輸出長度**：使用修飾詞時要考慮可能增加的輸出長度和複雜度

4. **保持一致性**：在同一個對話中使用一致的修飾詞風格和強度

5. **測試效果**：不同的修飾詞組合可能產生不同的效果，需要測試找出最佳組合

6. **避免過度**：過多或過於強烈的修飾詞可能適得其反，要適度使用

7. **結合其他技術**：修飾詞與其他提示技術（如 XML 標籤、範例）結合使用效果更佳

8. **針對性使用**：根據具體任務和期望輸出調整修飾詞的類型和強度
</guidelines>

## 常見錯誤

<mistakes>
1. **過度使用修飾詞**
   - 錯誤：使用過多激勵性語言，如"絕對最好"、"完美無缺"、"史上最強"
   - 正確：使用適中的修飾詞，如"盡力而為"、"提供高品質"、"超越基本要求"

2. **修飾詞與任務不匹配**
   - 錯誤：對簡單任務使用過於強烈的修飾詞
   - 正確：根據任務複雜度選擇適當強度的修飾詞

3. **缺乏具體指導**
   - 錯誤：只使用空洞的激勵詞語，如"做得更好"
   - 正確：提供具體的改進方向，如"增加更多互動功能"

4. **忽視上下文**
   - 錯誤：不考慮任務背景就使用通用修飾詞
   - 正確：根據具體使用場景選擇相關的修飾詞

5. **修飾詞衝突**
   - 錯誤：同時使用相互矛盾的修飾詞
   - 正確：確保所有修飾詞指向一致的目標

6. **缺乏測試**
   - 錯誤：不測試修飾詞的實際效果
   - 正確：透過 A/B 測試比較有無修飾詞的輸出差異
</mistakes>

## 測試與優化

<evaluation>
評估修飾詞效果的方法：

1. **品質比較**：比較使用修飾詞前後的輸出品質
2. **詳細程度評估**：檢查輸出的詳細程度和全面性
3. **專業水準測試**：評估輸出是否達到專業標準
4. **用戶滿意度**：收集使用者對輸出品質的反饋
5. **完成度檢查**：確認輸出是否完整滿足需求
6. **創新性評估**：檢查輸出是否具有創新性和獨特性

評估指標：
- 內容豐富度（1-10分）
- 專業程度（1-10分）
- 創新性（1-10分）
- 完整性（1-10分）
- 實用性（1-10分）
</evaluation>

<iteration>
迭代改進修飾詞使用的方法：

1. **收集反饋**：記錄每次使用修飾詞的效果
2. **分析模式**：找出最有效的修飾詞組合
3. **調整強度**：根據任務類型調整修飾詞的強度
4. **優化組合**：測試不同修飾詞的組合效果
5. **建立範本**：為不同任務類型建立修飾詞範本
6. **持續測試**：定期測試新的修飾詞和組合
7. **記錄最佳實踐**：建立修飾詞使用的最佳實踐資料庫

改進流程：
初始修飾詞 → 測試輸出 → 分析效果 → 調整修飾詞 → 再次測試 → 確定最佳組合
</iteration>

## 與其他技術組合

<combinations>
修飾詞技術可以與以下技術有效組合：

1. **系統提示詞**：在系統提示中加入修飾詞設定 Claude 的基本表現標準
2. **XML 標籤**：使用 XML 標籤結構化修飾詞，提高清晰度
3. **多樣本提示**：在範例中展示修飾詞的效果
4. **思維鏈**：結合思維鏈讓 Claude 思考如何達到修飾詞的要求
5. **輸出格式控制**：指定輸出格式時加入品質修飾詞
6. **角色扮演**：為特定角色設定相應的修飾詞標準
7. **長上下文技巧**：在長文本中持續使用修飾詞維持品質
8. **工具使用**：在工具使用任務中加入修飾詞提升執行品質

組合示例：
```xml
<role>你是一位資深的技術文檔撰寫專家</role>

<instruction>
撰寫 API 文檔
</instruction>

<modifiers>
<quality>展現專業技術寫作能力</quality>
<scope>涵蓋所有重要的技術細節</scope>
<usability>創建易於理解和使用的文檔</usability>
</modifiers>

<examples>
[提供高品質文檔範例]
</examples>

<thinking>
讓我思考如何創建最優秀的 API 文檔...
</thinking>
```
</combinations>

## 官方參考連結

- [Claude 4 Prompt Engineering Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Prompt Engineering Overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Anthropic Claude API Documentation](https://docs.anthropic.com/claude/docs)
- [Claude Prompt Library](https://docs.anthropic.com/claude/page/prompts)