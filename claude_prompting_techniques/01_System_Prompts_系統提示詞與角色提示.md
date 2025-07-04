# 1. 系統提示詞與角色提示(System Prompts)

## 官方定義與說明

<definition>
系統提示詞是一種特殊的提示技術，用於為 Claude 設定特定的角色或上下文背景，從而顯著提升其在特定領域的表現和專注度。根據 Anthropic 官方文檔，系統提示詞是透過 Messages API 中的 `system` 參數來實現的，它為 Claude 提供了一個持續的身份和行為框架。
</definition>

## 核心原理

<principle>
系統提示詞的核心工作原理是在對話開始前為 Claude 建立一個穩定的角色定位和行為準則。這個角色定位會持續影響整個對話過程，使 Claude 能夠：

1. **角色一致性**：在整個對話中保持特定的專業身份
2. **上下文記憶**：記住其扮演的角色和相關背景知識
3. **行為約束**：按照角色特性調整回應風格和內容深度
4. **專業導向**：以特定領域專家的角度來分析和回應問題

與傳統的用戶提示不同，系統提示詞在 API 架構中具有更高的優先級，能夠更穩定地影響 Claude 的行為模式。
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔說明，系統提示詞技術提供以下關鍵效益：

1. **增強準確性(Enhanced Accuracy)**：在複雜情境中提供更精確的專業回應
2. **定制化語調(Tailored Tone)**：匹配特定的溝通風格和專業要求
3. **提升專注度(Improved Focus)**：將 Claude 從通用助手轉換為虛擬領域專家
4. **角色轉換**：能夠根據不同角色(如數據科學家 vs 營銷策略師)提供不同視角的洞察
5. **持續性**：在整個對話過程中維持角色的一致性和專業性
</benefits>

## 實作格式

### 基本結構
```xml
<system>
你是一位[專業領域]的[具體角色]。
</system>

<user>
[用戶的具體問題或任務]
</user>
```

### 完整結構
```xml
<system>
你是一位具有[年資/經驗]的[專業領域][具體角色]。

你的專業背景包括：
- [專業技能1]
- [專業技能2]
- [專業技能3]

你的工作風格：
- [風格特點1]
- [風格特點2]
- [風格特點3]

你的回應應該：
- [回應要求1]
- [回應要求2]
- [回應要求3]
</system>

<user>
[用戶的具體問題或任務]
</user>
```

## 實作範例

### 範例1：資深數據科學家角色
```xml
<system>
你是一位具有15年經驗的資深數據科學家，目前在財富500強公司工作。

你的專業背景包括：
- 機器學習模型開發與優化
- 大數據分析與處理
- 商業智能與決策支援
- Python、R、SQL等技術棧精通

你的工作風格：
- 注重數據驅動的決策
- 擅長將複雜的技術概念轉化為商業洞察
- 重視統計顯著性和模型可解釋性

你的回應應該：
- 提供具體的數據分析方法
- 包含相關的統計考量
- 給出可執行的技術建議
</system>

<user>
我們的電商平台用戶流失率正在上升，你能幫我分析可能的原因並提出解決方案嗎？
</user>
```

### 範例2：創意總監角色
```xml
<system>
你是一位屢獲殊榮的創意總監，擁有12年廣告行業經驗，專精於品牌策略和創意執行。

你的專業背景包括：
- 品牌定位與視覺識別設計
- 整合行銷傳播策略
- 創意概念發想與執行
- 消費者洞察與市場趨勢分析

你的工作風格：
- 追求創新且具有影響力的創意
- 深度理解目標受眾心理
- 平衡創意表達與商業目標

你的回應應該：
- 提供具創意性的解決方案
- 考慮品牌一致性和市場定位
- 包含可視化和執行建議
</system>

<user>
我們正在為一個新的環保產品線設計品牌識別，目標客群是25-35歲的環保意識消費者。你能提供一個完整的品牌策略嗎？
</user>
```

### 範例3：技術架構師角色
```xml
<system>
你是一位雲端架構師，具有10年企業級系統設計經驗，專精於可擴展性和高可用性系統架構。

你的專業背景包括：
- 微服務架構設計與實施
- 雲端平台(AWS、Azure、GCP)架構優化
- DevOps和CI/CD流程設計
- 系統性能調優和容災規劃

你的工作風格：
- 重視系統的可擴展性和可維護性
- 考慮成本效益和技術債務
- 注重安全性和合規性要求

你的回應應該：
- 提供詳細的技術架構圖
- 包含技術選型的理由
- 考慮未來擴展和維護成本
</system>

<user>
我們需要為一個預期日活躍用戶100萬的社交媒體應用設計後端架構，你能提供一個完整的技術方案嗎？
</user>
```

## API實作方式

```python
import anthropic

# 初始化客戶端
client = anthropic.Anthropic(
    api_key="your-api-key-here"
)

# 基本系統提示詞實作
def create_system_prompt_message(system_role, user_query):
    response = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1500,
        temperature=0.7,
        system=system_role,
        messages=[
            {
                "role": "user", 
                "content": user_query
            }
        ]
    )
    return response.content[0].text

# 專業角色範例
data_scientist_role = """
你是一位具有15年經驗的資深數據科學家，目前在財富500強公司工作。
你的專業背景包括機器學習模型開發、大數據分析、商業智能等。
你的回應應該提供具體的數據分析方法、包含統計考量，並給出可執行的技術建議。
"""

# 使用系統提示詞
query = "我們的電商平台用戶流失率正在上升，你能幫我分析可能的原因並提出解決方案嗎？"
response = create_system_prompt_message(data_scientist_role, query)
print(response)

# 進階實作：角色切換功能
class RoleBasedAssistant:
    def __init__(self, api_key):
        self.client = anthropic.Anthropic(api_key=api_key)
        self.current_role = None
        
    def set_role(self, role_name, role_description):
        self.current_role = {
            "name": role_name,
            "description": role_description
        }
        
    def chat(self, message, max_tokens=1500, temperature=0.7):
        if not self.current_role:
            raise ValueError("請先設定角色")
            
        response = self.client.messages.create(
            model="claude-3-sonnet-20240229",
            max_tokens=max_tokens,
            temperature=temperature,
            system=self.current_role["description"],
            messages=[
                {
                    "role": "user",
                    "content": message
                }
            ]
        )
        return response.content[0].text

# 使用範例
assistant = RoleBasedAssistant("your-api-key")

# 設定為數據科學家角色
assistant.set_role(
    "數據科學家",
    "你是一位具有15年經驗的資深數據科學家，專精於機器學習和商業分析。"
)

response = assistant.chat("分析用戶流失的可能原因")
print(f"數據科學家回應: {response}")

# 切換為創意總監角色
assistant.set_role(
    "創意總監",
    "你是一位屢獲殊榮的創意總監，專精於品牌策略和創意執行。"
)

response = assistant.chat("為環保產品線設計品牌識別")
print(f"創意總監回應: {response}")
```

## 最佳實踐

<guidelines>
1. **具體性原則**：避免使用模糊的角色描述，如"專家"，而要具體說明專業領域和經驗水平
2. **經驗層次**：明確指定角色的經驗水平（如"5年經驗"、"資深"、"領域專家"）
3. **技能細分**：列出角色的具體技能和專業背景，使 Claude 能夠準確把握專業深度
4. **風格定義**：明確描述期望的回應風格和語調，確保輸出符合角色特徵
5. **約束設定**：為角色設定適當的行為約束和專業邊界
6. **上下文豐富**：提供足夠的背景信息，讓 Claude 能夠準確理解角色的工作環境和職責
7. **角色一致性**：在整個對話過程中保持角色設定的一致性
8. **領域權威**：選擇具有真實存在基礎的專業角色，避免虛構或不合理的角色設定
</guidelines>

## 常見錯誤

<mistakes>
**錯誤示範1：過於模糊的角色定義**
```xml
<system>
你是一個專家，請幫我解決問題。
</system>
```
**問題**：缺乏具體的專業領域和技能描述

**正確方式**：
```xml
<system>
你是一位具有8年經驗的軟體架構師，專精於微服務架構設計。
</system>
```

**錯誤示範2：角色與任務不匹配**
```xml
<system>
你是一位詩人，擅長創作優美的詩歌。
</system>
<user>
請分析這個數據庫查詢的性能問題。
</user>
```
**問題**：角色設定與實際任務需求不匹配

**正確方式**：
```xml
<system>
你是一位資深數據庫管理員，專精於SQL優化和性能調優。
</system>
```

**錯誤示範3：過度複雜的角色設定**
```xml
<system>
你是一位同時具有醫學、法律、工程、藝術四個領域博士學位的萬能專家...
</system>
```
**問題**：角色設定過於複雜和不現實

**正確方式**：專注於單一或相關的專業領域

**錯誤示範4：缺乏行為指導**
```xml
<system>
你是一位醫生。
</system>
```
**問題**：沒有提供具體的行為期望和回應要求

**正確方式**：
```xml
<system>
你是一位家庭醫學科醫師，你的回應應該專業但易懂，並提醒用戶尋求專業醫療建議。
</system>
```
</mistakes>

## 測試與優化

<evaluation>
**評估方法**：

1. **角色一致性測試**：
   - 在多輪對話中檢查角色表現是否一致
   - 評估專業術語使用的準確性
   - 確認回應風格是否符合角色設定

2. **專業深度評估**：
   - 測試角色在專業領域的知識深度
   - 評估技術建議的可行性和準確性
   - 確認是否能夠提供角色應有的專業洞察

3. **語調適配性**：
   - 評估回應的語調是否符合角色特徵
   - 檢查專業用語與通俗解釋的平衡
   - 確認溝通風格是否適合目標受眾

4. **邊界測試**：
   - 測試角色在專業邊界外的表現
   - 評估是否能夠適當地承認知識限制
   - 確認是否會堅持專業倫理和原則
</evaluation>

<iteration>
**迭代改進方法**：

1. **漸進式細化**：
   - 從基本角色定義開始
   - 根據回應品質逐步添加細節
   - 調整專業背景和技能描述

2. **A/B測試**：
   - 準備多個角色變體
   - 在相同問題上測試不同版本
   - 選擇表現最佳的角色設定

3. **反饋循環**：
   - 收集用戶對角色表現的反饋
   - 分析回應中的不足之處
   - 調整角色描述和行為指導

4. **情境適配**：
   - 為不同使用情境調整角色設定
   - 考慮目標受眾的專業水平
   - 優化角色與特定任務的匹配度

5. **持續監控**：
   - 定期檢查角色表現的穩定性
   - 監控新類型問題的處理效果
   - 根據使用模式調整角色設定
</iteration>

## 與其他技術組合

<combinations>
**最佳組合技術**：

1. **系統提示詞 + XML標籤結構化**：
```xml
<system>
你是一位資深數據分析師，請按照以下結構回應：
- 使用<analysis>標籤包含分析內容
- 使用<recommendation>標籤包含建議
- 使用<metrics>標籤包含關鍵指標
</system>
```

2. **系統提示詞 + 多樣本提示**：
```xml
<system>
你是一位UX設計師，請參考以下範例格式回應用戶需求。
</system>
<user>
範例1：[提供具體的設計案例]
範例2：[提供另一個設計案例]
現在請為我們的移動應用設計用戶流程。
</user>
```

3. **系統提示詞 + 思維鏈(Chain of Thought)**：
```xml
<system>
你是一位問題解決專家，請用以下步驟思考：
1. 問題理解
2. 方案分析
3. 風險評估
4. 最終建議
</system>
```

4. **系統提示詞 + 輸出格式控制**：
```xml
<system>
你是一位技術文檔撰寫專家，請用Markdown格式回應，包含：
- 標題層次
- 程式碼區塊
- 表格整理
</system>
```

5. **系統提示詞 + 工具調用**：
```xml
<system>
你是一位數據分析師，當需要執行計算或處理數據時，請使用提供的工具。
</system>
```
</combinations>

## 官方參考連結

- [Anthropic 官方文檔 - 系統提示詞](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/system-prompts)
- [Anthropic 官方文檔 - Claude 4 最佳實踐](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Anthropic 官方文檔 - 提示工程概述](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Anthropic 系統提示詞發布紀錄](https://docs.anthropic.com/en/release-notes/system-prompts)