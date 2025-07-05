# 2. 清晰直接的指示(Clear Direct Instructions)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，清晰直接的指示是指像對待一個聰明但沒有背景知識的新員工一樣對待 Claude，提供明確、具體、詳細的指示。越精確地解釋你想要什麼，Claude 的回應就越好。這是 Claude 4 模型特別強調的核心提示技術，這些模型經過專門訓練以更精確地遵循指示。
</definition>

## 核心原理

<principle>
清晰直接指示的核心原理基於「黃金法則」：將你的提示詞展示給一位對任務背景了解很少的同事，請他們按照指示執行。如果他們感到困惑，Claude 也很可能會困惑。這個原理強調：
1. 明確性勝過模糊性
2. 具體指示勝過抽象概念
3. 結構化指示勝過隨意表達
4. 背景資訊提供上下文理解
</principle>

## 官方效益

<benefits>
基於 Anthropic 官方文檔說明的效益包括：
- 減少誤解和歧義
- 提高回應準確性
- 幫助 Claude 理解細微要求
- 減少需要多次迭代的情況
- 獲得更符合預期的輸出結果
- 提升 Claude 4 模型的指令遵循能力
</benefits>

## 實作格式

### 基本結構
```xml
<instructions>
<task>清晰描述具體任務</task>
<context>提供必要背景資訊</context>
<requirements>列出具體要求</requirements>
<format>指定輸出格式</format>
</instructions>
```

### 完整結構
```xml
<instructions>
<task>詳細描述要完成的任務</task>
<context>
<background>任務背景和目的</background>
<audience>目標受眾</audience>
<workflow>工作流程背景</workflow>
</context>
<requirements>
<must_include>必須包含的內容</must_include>
<must_avoid>必須避免的內容</must_avoid>
<constraints>限制條件</constraints>
</requirements>
<format>
<structure>輸出結構</structure>
<style>寫作風格</style>
<length>長度要求</length>
</format>
<success_criteria>成功標準</success_criteria>
</instructions>
```

## 實作範例

### 範例1：內容創作指示

#### 範例A：基本版（不用 XML）
```
為科技新創公司撰寫一篇關於人工智慧影響的部落格文章。

背景：我們公司專注於企業AI解決方案，需要建立思想領導地位。目標讀者是中小企業的技術決策者和高階主管。這篇文章將用於官方部落格，並在LinkedIn上分享。

要求：
- 必須包含具體的企業AI應用案例
- 包含實際的ROI數據或統計
- 提供實施建議和行動步驟
- 討論風險考量和解決方案

格式：800-1200字，包含至少3個副標題，每段不超過150字。使用專業但易讀的語調，具有說服力。

目標：讀者能夠理解AI的商業價值並考慮採用。
```

#### 範例B：進階版（使用 XML）
```xml
<instructions>
<task>為科技新創公司撰寫一篇關於人工智慧影響的部落格文章</task>
<context>
<background>我們公司專注於企業AI解決方案，需要建立思想領導地位</background>
<audience>目標讀者是中小企業的技術決策者和高階主管</audience>
<workflow>這篇文章將用於官方部落格，並在LinkedIn上分享</workflow>
</context>
<requirements>
<must_include>
1. 具體的企業AI應用案例
2. 實際的ROI數據或統計
3. 實施建議和行動步驟
4. 風險考量和解決方案
</must_include>
<must_avoid>
1. 過於技術性的術語
2. 未經證實的預測
3. 競爭對手的負面評論
</must_avoid>
<constraints>
1. 字數：800-1200字
2. 包含至少3個副標題
3. 每段不超過150字
</constraints>
</requirements>
<format>
<structure>引言-主要論點-案例研究-行動建議-結論</structure>
<style>專業但易讀，具有說服力</style>
<length>800-1200字</length>
</format>
<success_criteria>讀者能夠理解AI的商業價值並考慮採用</success_criteria>
</instructions>
```

### 範例2：程式碼生成指示

#### 範例A：基本版（不用 XML）
```
建立一個用戶登入驗證系統的Python Flask API。

背景：這是為電子商務網站建立安全的用戶認證系統，API將被前端開發團隊使用，需要整合到現有的Flask應用程序中。

功能要求：
1. 用戶註冊端點（POST /register）
2. 用戶登入端點（POST /login）
3. JWT token生成和驗證
4. 密碼加密儲存
5. 錯誤處理和狀態碼
6. 輸入驗證

技術要求：
- 使用Flask-JWT-Extended
- 使用BCrypt加密密碼
- 包含完整的錯誤處理
- 添加適當的註釋
- 遵循PEP8規範

請提供完整的實作，包含所有必要功能，代碼可以直接運行並提供安全的用戶認證。
```

#### 範例B：進階版（使用 XML）
```xml
<instructions>
<task>建立一個用戶登入驗證系統的Python Flask API</task>
<context>
<background>為電子商務網站建立安全的用戶認證系統</background>
<audience>這個API將被前端開發團隊使用</audience>
<workflow>需要整合到現有的Flask應用程序中</workflow>
</context>
<requirements>
<must_include>
1. 用戶註冊端點（POST /register）
2. 用戶登入端點（POST /login）
3. JWT token生成和驗證
4. 密碼加密儲存
5. 錯誤處理和狀態碼
6. 輸入驗證
</must_include>
<must_avoid>
1. 明文儲存密碼
2. 缺少輸入驗證
3. 不安全的token處理
</must_avoid>
<constraints>
1. 使用Flask-JWT-Extended
2. 使用BCrypt加密密碼
3. 包含完整的錯誤處理
4. 添加適當的註釋
</constraints>
</requirements>
<format>
<structure>導入模組-配置-路由定義-輔助函數-主程式</structure>
<style>遵循PEP8規範，清晰註釋</style>
<length>包含所有必要功能的完整實作</length>
</format>
<success_criteria>代碼可以直接運行並提供安全的用戶認證</success_criteria>
</instructions>
```

### 範例3：數據分析指示

#### 範例A：基本版（不用 XML）
```
分析過去6個月的銷售數據並提供業務建議。

背景：公司銷售增長放緩，需要識別問題和機會。分析結果將向執行團隊報告，用於制定下季度的銷售策略。

分析要求：
1. 銷售趨勢分析（按月份、產品類別、地區）
2. 關鍵績效指標對比
3. 問題識別和根因分析
4. 具體的改善建議
5. 預期影響評估

注意事項：
- 基於提供的CSV銷售數據
- 包含視覺圖表
- 重點關注可控制的因素
- 避免沒有數據支持的推測

格式：10-15頁報告，包含執行摘要、數據概覽、詳細分析、建議和下一步行動。使用簡潔專業、以數據為導向的風格。

目標：高階主管能夠基於分析結果做出明智的策略決策。
```

#### 範例B：進階版（使用 XML）
```xml
<instructions>
<task>分析過去6個月的銷售數據並提供業務建議</task>
<context>
<background>公司銷售增長放緩，需要識別問題和機會</background>
<audience>分析結果將向執行團隊報告</audience>
<workflow>分析將用於制定下季度的銷售策略</workflow>
</context>
<requirements>
<must_include>
1. 銷售趨勢分析（按月份、產品類別、地區）
2. 關鍵績效指標對比
3. 問題識別和根因分析
4. 具體的改善建議
5. 預期影響評估
</must_include>
<must_avoid>
1. 沒有數據支持的推測
2. 過於複雜的統計術語
3. 缺乏可行性的建議
</must_avoid>
<constraints>
1. 使用提供的CSV銷售數據
2. 包含視覺圖表
3. 重點關注可控制的因素
</constraints>
</requirements>
<format>
<structure>執行摘要-數據概覽-詳細分析-建議-下一步行動</structure>
<style>簡潔專業，以數據為導向</style>
<length>10-15頁報告</length>
</format>
<success_criteria>高階主管能夠基於分析結果做出明智的策略決策</success_criteria>
</instructions>
```

### 使用時機說明：

#### 🔸 **基本版適用情況：**
- **簡單明確的任務**：要求straightforward，沒有太多層次
- **快速開發測試**：需要快速測試想法，不想被標籤分散注意力
- **初學者學習**：剛開始學習提示工程，先掌握基本表達
- **內容創作任務**：大部分的文章、總結、翻譯任務
- **單一目標任務**：只有一個主要輸出要求

#### 🔸 **進階版適用情況：**
- **複雜多層次任務**：包含多個不同類型的要求（背景+約束+格式+評估標準）
- **需要精確控制**：對輸出的每個方面都有具體要求
- **團隊協作場景**：需要讓其他人快速理解提示結構
- **生產環境使用**：需要穩定、可預測的輸出品質
- **系統整合需求**：提示會被程式碼調用，需要結構化解析

#### 🔸 **升級判斷標準：**
- **要求數量超過5個不同類型** → 考慮用XML
- **有明顯的層次結構** → 考慮用XML
- **需要經常修改特定部分** → 考慮用XML
- **多人協作維護** → 考慮用XML

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

# 清晰直接指示範例
def clear_direct_prompt(task, context, requirements, format_spec):
    prompt = f"""
<instructions>
<task>{task}</task>
<context>{context}</context>
<requirements>{requirements}</requirements>
<format>{format_spec}</format>
</instructions>

請按照上述指示完成任務。
"""
    
    response = client.messages.create(
        model="claude-3-5-sonnet-20241022",
        max_tokens=4000,
        temperature=0.3,
        messages=[
            {
                "role": "user",
                "content": prompt
            }
        ]
    )
    
    return response.content[0].text

# 使用範例
task = "撰寫一份產品規格文件"
context = "這是一個新的智慧手錶產品，目標用戶是健身愛好者"
requirements = "必須包含技術規格、功能特色、使用場景和競爭優勢"
format_spec = "使用標題結構，每個部分包含3-5個要點"

result = clear_direct_prompt(task, context, requirements, format_spec)
print(result)

# Claude 4 增強版本 - 使用修飾詞
def claude4_enhanced_prompt(base_task):
    enhanced_prompt = f"""
<instructions>
<task>{base_task}</task>
<enhancement>
盡可能包含更多相關功能和細節。不要保留，創建一個完整、全面的實作。
包含所有必要的功能，超越基本要求。
</enhancement>
</instructions>

請按照上述指示完成任務，充分發揮你的能力。
"""
    
    response = client.messages.create(
        model="claude-3-5-sonnet-20241022",
        max_tokens=4000,
        temperature=0.3,
        messages=[
            {
                "role": "user",
                "content": enhanced_prompt
            }
        ]
    )
    
    return response.content[0].text
```

## 最佳實踐

<guidelines>
1. **使用「黃金法則」測試**：將提示詞給同事閱讀，確保他們能理解並執行
2. **提供充分背景**：說明任務目的、目標受眾、使用場景
3. **使用結構化指示**：採用編號列表或項目符號組織指示
4. **指定具體輸出格式**：明確說明所需的結構、風格、長度
5. **包含成功標準**：定義如何評估結果是否滿足要求
6. **運用 Claude 4 增強技術**：使用修飾詞鼓勵更詳細的輸出
7. **避免模糊語言**：用具體要求取代抽象描述
8. **逐步分解複雜任務**：將大任務分解為清晰的子任務
</guidelines>

## 常見錯誤

<mistakes>
**錯誤示範：**
```
寫一些關於氣候變化的內容
```

**正確示範：**
```xml
<instructions>
<task>撰寫一篇關於氣候變化對沿海社區影響的文章</task>
<context>文章將發布在環境保護網站，讀者是關心環境議題的一般大眾</context>
<requirements>
- 重點關注過去2年的最新研究
- 包含至少3個具體案例
- 提供可行的解決方案建議
- 長度：800-1000字
</requirements>
<format>使用副標題結構，每段不超過120字</format>
</instructions>
```

**常見錯誤類型：**
1. 指示過於籠統或模糊
2. 缺乏必要的背景資訊
3. 沒有指定輸出格式
4. 未提供成功標準
5. 混合多個不相關的任務
6. 使用否定語言而非正向指示
</mistakes>

## 測試與優化

<evaluation>
**評估清晰度的方法：**
1. **同事測試**：請同事按照指示執行，檢查是否產生預期結果
2. **要素檢查**：確認包含任務、背景、要求、格式四個基本要素
3. **具體性評估**：檢查每個指示是否具體可執行
4. **歧義檢測**：識別可能產生多種解釋的語句
5. **完整性驗證**：確認所有必要資訊都已提供
</evaluation>

<iteration>
**迭代改進流程：**
1. 記錄 Claude 的輸出與預期的差異
2. 分析差異原因（背景不足、指示不清、格式不明等）
3. 針對性地增強相應部分
4. 重新測試並評估改進效果
5. 建立指示模板以供重複使用
</iteration>

## 與其他技術組合

<combinations>
**與系統提示詞結合：**
- 在系統提示詞中定義 Claude 的角色
- 在用戶提示詞中提供清晰直接的任務指示

**與多樣本提示結合：**
- 使用清晰的指示描述期望的行為模式
- 提供符合指示的正確範例

**與 XML 標籤結合：**
- 使用 XML 標籤結構化清晰指示
- 標記指示的不同部分（任務、背景、要求、格式）

**與思維鏈結合：**
- 在清晰指示中要求 Claude 展示思考過程
- 指導 Claude 如何進行步驟性推理

**與輸出格式控制結合：**
- 在清晰指示中精確定義輸出格式
- 使用具體的格式範例和模板
</combinations>

## 官方參考連結

- [Anthropic 官方文檔：Be clear, direct, and detailed](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct)
- [Anthropic 官方文檔：Claude 4 prompt engineering best practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Anthropic 官方文檔：Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Anthropic Interactive Prompt Engineering Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)