# 11. 具體性勝過模糊性(Specificity)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，具體性是指在提示中提供明確、詳細的指令，而不是模糊或含糊的要求。Claude 最適合處理清晰、直接的指令，具體性原則要求將 Claude 視為「一位聰明但健忘的新員工」，需要明確的指導來完成任務。模糊的提示會產生模糊的結果，而具體的提示能夠獲得精確的回應。
</definition>

## 核心原理

<principle>
具體性原則基於 Claude 的工作機制：Claude 沒有你的工作背景、風格指南或偏好方式的上下文。當提示越具體，Claude 就越能準確理解你的期望並產生符合要求的輸出。這個原則的核心是消除歧義：通過提供明確的指令、約束條件、格式要求和期望結果，讓 Claude 能夠精確地執行任務。具體性不僅包括任務描述，還包括輸出格式、目標受眾、語調風格和成功標準。
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔強調具體性帶來的效益：
1. 顯著提高輸出品質和準確性
2. 減少需要多次迭代修正的情況
3. 獲得更一致和可預測的結果
4. 降低產生不相關或偏離主題內容的風險
5. Claude 4 模型特別對明確指令反應良好
6. 能夠實現更精準的指令遵循
7. 減少歧義導致的錯誤理解
</benefits>

## 實作格式

### 基本結構
```xml
<task>
明確的任務描述（具體動作 + 明確目標）
</task>

<constraints>
- 特定限制條件
- 格式要求
- 長度限制
</constraints>

<context>
任務背景和目的
</context>
```

### 完整結構
```xml
<task>
具體任務描述（包含明確動作詞彙）
</task>

<audience>
目標受眾的具體特徵
</audience>

<format>
輸出格式的詳細要求
</format>

<constraints>
- 具體的限制條件
- 數字化的要求（字數、項目數等）
- 禁止或必須包含的元素
</constraints>

<context>
任務背景、目的、使用場景
</context>

<examples>
期望輸出的具體示例
</examples>
```

## 實作範例

### 範例1：內容創作任務
```xml
<task>
為永續水瓶產品撰寫行銷文案
</task>

<audience>
25-40歲環保意識強烈的都市專業人士
</audience>

<format>
- 標題：不超過10字
- 內容：3段落，每段50-70字
- 結尾：明確的行動呼籲
</format>

<constraints>
- 強調環保特色和實用性
- 使用積極正面的語調
- 避免過度技術性術語
- 必須包含產品的3個核心優勢
</constraints>

<context>
這是針對新產品發布的主要行銷文案，將用於社群媒體和官網首頁，目標是提高品牌知名度並促進預購
</context>

<examples>
期望的語調範例：「讓每一口水都充滿環保意識」
</examples>
```

### 範例2：程式碼生成任務
```xml
<task>
創建一個 Python 函數來驗證電子郵件地址格式
</task>

<format>
- 函數名稱：validate_email
- 輸入：字串類型的電子郵件地址
- 輸出：布林值（True/False）
- 包含詳細的文檔字串
</format>

<constraints>
- 使用正則表達式進行驗證
- 包含基本格式檢查：@符號、網域名稱、頂級網域
- 加入錯誤處理機制
- 程式碼風格遵循 PEP 8 標準
- 包含至少3個測試案例
</constraints>

<context>
此函數將整合到用戶註冊系統中，用於前端表單驗證，需要快速且準確的驗證功能
</context>

<examples>
有效格式：user@example.com
無效格式：user@, @example.com, user.example.com
</examples>
```

### 範例3：資料分析任務
```xml
<task>
分析提供的銷售數據並生成月度報告摘要
</task>

<audience>
高階管理層（需要戰略層面的洞察）
</audience>

<format>
- 執行摘要：100字以內
- 關鍵指標：表格形式，包含本月vs上月比較
- 趨勢分析：2-3個重點發現
- 建議行動：3項具體可執行的建議
</format>

<constraints>
- 專注於收入、成長率、客戶獲取成本
- 使用百分比和具體數字
- 避免過度技術性統計術語
- 每項建議需包含預期影響和時間框架
</constraints>

<context>
這是每月董事會議的重要參考資料，需要幫助管理層做出戰略決策
</context>

<examples>
期望的表達方式：「本月收入較上月成長15%（從$50,000增至$57,500）」
</examples>
```

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

# 具體性提示範例
def create_specific_prompt(task, audience, format_req, constraints, context):
    prompt = f"""
<task>
{task}
</task>

<audience>
{audience}
</audience>

<format>
{format_req}
</format>

<constraints>
{constraints}
</constraints>

<context>
{context}
</context>

請根據以上具體要求完成任務。
"""
    
    response = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1500,
        temperature=0.7,
        messages=[
            {"role": "user", "content": prompt}
        ]
    )
    
    return response.content[0].text

# 使用範例
task = "撰寫產品介紹文案"
audience = "25-35歲的科技愛好者"
format_req = "標題 + 3段內容 + 特色列表 + 行動呼籲"
constraints = "每段不超過50字，強調創新技術，使用親近的語調"
context = "新款智慧手錶的電商頁面文案，目標提高轉換率"

result = create_specific_prompt(task, audience, format_req, constraints, context)
print(result)

# 對比範例：模糊 vs 具體
def compare_prompts():
    # 模糊提示
    vague_prompt = "寫一些關於產品的內容"
    
    # 具體提示
    specific_prompt = """
    <task>為智慧手錶撰寫電商頁面產品介紹</task>
    <audience>25-35歲科技愛好者，注重功能性和設計</audience>
    <format>
    - 吸引人的標題（8-12字）
    - 產品描述（100-150字）
    - 5個核心功能特色
    - 購買行動呼籲
    </format>
    <constraints>
    - 強調健康監測和運動追蹤功能
    - 使用親切但專業的語調
    - 包含具體的電池續航時間
    </constraints>
    """
    
    return vague_prompt, specific_prompt
```

## 最佳實踐

<guidelines>
1. 黃金法則：將你的提示給同事看，如果他們困惑，Claude 也會困惑
2. 使用具體的數字：「2-3句話」而非「簡潔」
3. 明確指定輸出格式：如果只要程式碼，就明確說明
4. 提供上下文：解釋任務的目的和使用場景
5. 設定具體約束：字數限制、語調要求、必須包含的元素
6. 使用循序漸進的指令：用編號或條列式清單
7. 定義目標受眾：明確說明內容的接收對象
8. 提供範例：展示期望的輸出風格或格式
9. 避免假設：不要依賴 Claude 推測你的意圖
10. 量化要求：使用可測量的標準而非主觀描述
</guidelines>

## 常見錯誤

<mistakes>
1. 錯誤：「寫得好一點」
   正確：「使用專業但易懂的語調，每段不超過50字」

2. 錯誤：「分析這個數據」
   正確：「計算月度成長率，識別前三大收入來源，並提供改善建議」

3. 錯誤：「創建一個網站」
   正確：「創建響應式網站，包含導覽列、英雄區塊、產品展示區、聯絡表單」

4. 錯誤：「讓內容更有趣」
   正確：「加入2-3個相關的實際案例，使用對話式語調，包含提問句」

5. 錯誤：「優化這個流程」
   正確：「識別流程中的3個主要瓶頸，提供具體的改善步驟和預期時間節約」

6. 錯誤：使用多重模糊指令
   正確：拆分成具體的子任務，每個任務有明確的輸出要求
</mistakes>

## 測試與優化

<evaluation>
評估具體性效果的方法：
1. 一致性測試：同樣的提示重複執行，檢查輸出的一致性
2. 同事測試：讓他人根據你的提示完成任務，看是否產生期望結果
3. 完整性檢查：確認輸出包含所有要求的元素
4. 格式驗證：檢查輸出是否符合指定格式
5. 目標對齊：評估輸出是否達成原始目標
6. 錯誤率測量：計算需要重新生成的比例
</evaluation>

<iteration>
迭代改進具體性的方法：
1. 記錄不滿意的輸出，分析缺少的具體指令
2. 建立常用任務的具體提示模板
3. 收集最佳實踐的具體措辭
4. 定期檢視並更新約束條件
5. 建立具體性檢查清單
6. A/B測試不同具體性程度的提示
7. 建立反饋循環，持續優化提示具體性
</iteration>

## 與其他技術組合

<combinations>
1. 具體性 + 系統提示：在系統提示中建立具體的角色和行為準則
2. 具體性 + 多樣本提示：提供具體的正面和負面範例
3. 具體性 + 思維鏈：要求具體的推理步驟和中間結果
4. 具體性 + XML標籤：使用具體的標籤結構化複雜指令
5. 具體性 + 輸出格式：詳細指定輸出的結構和樣式
6. 具體性 + 預填技術：提供具體的開始文字引導方向
7. 具體性 + 測試案例：包含具體的測試情境和期望結果
8. 具體性 + 迭代改進：根據具體的評估標準持續優化
</combinations>

## 官方參考連結

- [Anthropic 提示工程概述](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [清晰直接的指示](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct)
- [Claude 4 最佳實踐](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [提示範本和變數](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-templates-and-variables)
- [Anthropic 互動式提示工程教學](https://github.com/anthropics/prompt-eng-interactive-tutorial)