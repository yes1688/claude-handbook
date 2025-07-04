# 7. 提供動機和背景(Context Motivation)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，提供動機和背景是指在提示中包含任務的背景資訊、目的說明以及成功完成標準的定義。這種技術幫助 Claude 更好地理解任務目標，並提供更精確、更符合期望的回應。官方建議將 Claude 視為"一位聰明但全新的員工（患有失憶症）"，需要明確的指令和充分的背景資訊。
</definition>

## 核心原理

<principle>
Context Motivation 技術的核心原理基於認知心理學中的情境理解理論。當 Claude 獲得充分的背景資訊時，它能夠：
1. 理解任務的具體目標和期望
2. 調整回應的語調、風格和詳細程度
3. 優先考慮最重要的資訊
4. 避免不相關或不適當的輸出
5. 產生更具針對性的解決方案
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔中說明的效益包括：
1. 顯著提高回應的精確度和相關性
2. 減少模糊性和誤解
3. 增強任務完成的成功率
4. 改善輸出品質，特別是複雜任務
5. 使 Claude 能夠更好地理解和利用外部內容
6. 提供更符合特定領域或情境需求的回應
</benefits>

## 實作格式

### 基本結構
```xml
<context>
[背景資訊說明]
</context>

<task>
[具體任務描述]
</task>
```

### 完整結構
```xml
<context>
[背景資訊]
</context>

<purpose>
[任務目的和重要性]
</purpose>

<audience>
[目標受眾]
</audience>

<constraints>
[限制條件]
</constraints>

<task>
[具體任務描述]
</task>

<success_criteria>
[成功標準定義]
</success_criteria>
```

## 實作範例

### 範例1：商業報告撰寫
```xml
<context>
你是一位資深商業分析師，正在為公司高階主管準備季度業績報告。這份報告將在董事會會議上使用，影響未來的戰略決策。
</context>

<purpose>
報告需要清楚展示公司當前的財務狀況、市場表現，並提供具體的改善建議，以便管理層做出明智的決策。
</purpose>

<audience>
董事會成員和高階主管，他們需要快速理解關鍵資訊，並基於數據做出戰略決策。
</audience>

<constraints>
- 報告長度不超過 10 頁
- 必須包含具體的數據支撐
- 語調專業、客觀
- 避免技術術語，使用商業語言
</constraints>

<task>
基於以下財務數據，撰寫一份專業的季度業績報告，重點分析營收變化、成本控制效果，並提供下季度的具體建議。
</task>

<success_criteria>
報告應該能讓讀者在 5 分鐘內掌握公司現況，並清楚了解需要採取的行動。
</success_criteria>
```

### 範例2：教育內容設計
```xml
<context>
你是一位經驗豐富的線上課程設計師，正在為一個針對完全初學者的程式設計課程創建學習材料。學生大多是職場轉換者，沒有任何程式設計背景。
</context>

<purpose>
課程目標是讓學生在 12 週內掌握基礎程式設計概念，並能夠獨立完成簡單的程式專案。建立學生的信心和持續學習的動機非常重要。
</purpose>

<audience>
25-40 歲的職場轉換者，學習時間有限，需要實用且容易理解的內容。
</audience>

<constraints>
- 每個概念的解釋不超過 200 字
- 必須提供實際的程式碼範例
- 避免使用複雜的技術術語
- 包含互動式練習
</constraints>

<task>
設計一個關於「變數和資料類型」的課程單元，包括概念說明、程式碼範例和練習題。
</task>

<success_criteria>
學生完成這個單元後，能夠理解什麼是變數、如何宣告變數，並能夠在簡單的程式中正確使用不同的資料類型。
</success_criteria>
```

### 範例3：客戶服務回應
```xml
<context>
你是一位專業的客戶服務代表，正在處理一位對產品功能不滿意的重要客戶投訴。這位客戶是公司的長期合作夥伴，維護關係對公司非常重要。
</context>

<purpose>
需要妥善處理客戶不滿，提供有效的解決方案，同時維護公司聲譽和客戶關係。
</purpose>

<audience>
一位重視效率和結果的企業客戶，期望得到快速、專業的回應。
</audience>

<constraints>
- 保持專業和同理心的語調
- 提供具體的解決方案
- 避免推卸責任
- 回應長度控制在 300 字以內
</constraints>

<task>
針對客戶反映的「軟體載入速度過慢」問題，撰寫一份客戶服務回應，包括問題確認、解決方案和後續跟進計劃。
</task>

<success_criteria>
客戶感受到公司的重視，對提供的解決方案感到滿意，並願意繼續合作。
</success_criteria>
```

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

# Context Motivation 提示範例
def create_context_motivated_prompt(context, purpose, audience, task, constraints=None, success_criteria=None):
    prompt_parts = [
        f"<context>\n{context}\n</context>",
        f"<purpose>\n{purpose}\n</purpose>",
        f"<audience>\n{audience}\n</audience>"
    ]
    
    if constraints:
        prompt_parts.append(f"<constraints>\n{constraints}\n</constraints>")
    
    prompt_parts.append(f"<task>\n{task}\n</task>")
    
    if success_criteria:
        prompt_parts.append(f"<success_criteria>\n{success_criteria}\n</success_criteria>")
    
    return "\n\n".join(prompt_parts)

# 使用範例
context = "你是一位資深的產品經理，正在為新產品發布準備市場策略。"
purpose = "制定一個全面的市場進入策略，確保產品成功上市。"
audience = "公司執行團隊和市場部門同事。"
task = "基於市場研究數據，制定產品發布的行銷策略。"
constraints = "預算限制在 100 萬元以內，發布時間為 3 個月後。"
success_criteria = "策略應該能夠在 6 個月內達到 10% 的市場佔有率。"

prompt = create_context_motivated_prompt(
    context=context,
    purpose=purpose,
    audience=audience,
    task=task,
    constraints=constraints,
    success_criteria=success_criteria
)

message = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1000,
    messages=[
        {"role": "user", "content": prompt}
    ]
)

print(message.content)
```

## 最佳實踐

<guidelines>
1. **具體性原則**：提供具體而非抽象的背景資訊
2. **相關性原則**：確保所有背景資訊都與任務直接相關
3. **分層結構**：按重要性排序背景資訊
4. **受眾導向**：明確說明目標受眾的特徵和需求
5. **目標明確**：清楚定義成功的標準和期望
6. **限制明確**：說明任何限制條件或約束
7. **驗證原則**：將你的提示給同事看，如果他們感到困惑，Claude 也可能會困惑
8. **循序漸進**：對於複雜任務，提供階段性的背景資訊
</guidelines>

## 常見錯誤

<mistakes>
1. **背景資訊過於籠統**
   - 錯誤：「為公司寫一份報告」
   - 正確：「作為財務分析師，為董事會撰寫第三季度業績分析報告」

2. **缺乏目的說明**
   - 錯誤：只描述任務內容
   - 正確：解釋為什麼需要完成這個任務

3. **忽略受眾特徵**
   - 錯誤：不說明目標讀者
   - 正確：明確描述受眾的背景、需求和期望

4. **過度複雜的背景**
   - 錯誤：提供過多不相關的背景資訊
   - 正確：只包含與任務直接相關的背景

5. **缺乏成功標準**
   - 錯誤：沒有定義什麼是成功的結果
   - 正確：明確說明期望的結果和衡量標準
</mistakes>

## 測試與優化

<evaluation>
評估 Context Motivation 技術效果的方法：
1. **相關性測試**：檢查回應是否針對提供的背景資訊
2. **完整性測試**：驗證是否滿足所有成功標準
3. **適切性測試**：確認語調和風格是否符合受眾需求
4. **效率測試**：比較有無背景資訊時的回應品質差異
5. **同儕審查**：讓其他人評估背景資訊的清晰度和充分性
</evaluation>

<iteration>
迭代改進的方法：
1. **A/B 測試**：比較不同背景資訊詳細程度的效果
2. **增量改進**：逐步增加背景資訊的具體性
3. **反饋收集**：收集目標受眾對輸出品質的反饋
4. **模板優化**：基於成功案例建立可重複使用的背景模板
5. **持續監控**：追蹤長期使用效果並調整策略
</iteration>

## 與其他技術組合

<combinations>
Context Motivation 可以與以下技術有效組合：
1. **系統提示詞**：在系統層級設定角色和基本背景
2. **多樣本提示**：提供具體的輸出範例
3. **思維鏈**：引導 Claude 逐步思考問題
4. **XML 標籤**：結構化組織背景資訊
5. **輸出格式控制**：指定特定的回應格式
6. **具體性技術**：提供詳細的任務要求
7. **負面提示**：明確說明要避免的行為
8. **成功標準定義**：設定明確的評估標準
</combinations>

## 官方參考連結

- [Anthropic 官方提示工程指南](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [清晰直接的指示](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct)
- [Claude 4 最佳實踐](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [系統提示詞指南](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/system-prompts)