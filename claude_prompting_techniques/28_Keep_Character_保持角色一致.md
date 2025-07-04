# 28. 保持角色一致(Keep Character)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，保持角色一致是一種確保 Claude 在整個對話過程中維持特定角色設定的技術。這包括使用系統提示詞定義角色、回應預填技術加強角色認同，以及在長對話中通過情境強化來防止角色漂移。此技術是角色提示的進階應用，專門解決長時間對話中的角色一致性問題。
</definition>

## 核心原理

<principle>
保持角色一致的核心原理基於以下三個層面：

1. **角色定義層**：通過系統提示詞建立清晰的角色身份、背景、特質和行為模式
2. **強化機制層**：使用回應預填技術定期提醒模型當前角色身份
3. **情境維持層**：在對話中定期重申角色設定，特別是在長對話或複雜情境中

這種多層次的角色一致性維護機制確保 Claude 能夠在各種情境下保持穩定的角色表現。
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔中說明的效益包括：

- **溝通風格客製化**：根據不同角色需求調整語調和表達方式
- **專業領域聚焦**：確保 Claude 在特定專業領域內保持一致的專業水準
- **沉浸式體驗**：提供更自然、連貫的角色互動體驗
- **長對話穩定性**：防止在長時間對話中出現角色漂移現象
- **情境適應性**：在不同情境下保持角色核心特質的同時靈活適應
</benefits>

## 實作格式

### 基本結構
```xml
<system>
<role>
你是 [角色名稱]，一位 [職業/身份描述]。
你的特質包括：[核心特質列表]
你的說話風格：[語調和表達方式]
</role>
</system>
```

### 完整結構
```xml
<system>
<role>
你是 [角色名稱]，[詳細背景描述]。

<personality>
- 核心特質：[特質1]、[特質2]、[特質3]
- 說話風格：[具體描述]
- 行為模式：[典型行為描述]
</personality>

<expertise>
- 專業領域：[領域1]、[領域2]
- 經驗背景：[相關經驗]
- 知識重點：[重要知識點]
</expertise>

<interaction_guidelines>
- 始終保持 [角色名稱] 的身份
- 用 [特定語調] 回應
- 當不確定時，請說：[標準回應]
- 避免：[不符合角色的行為]
</interaction_guidelines>
</role>
</system>

<prefill>
[角色名稱]：
</prefill>
```

## 實作範例

### 範例1：AI職涯顧問
```xml
<system>
<role>
你是 Joe，一位來自 AI Career Coach Co. 的專業AI職涯顧問。你有15年的職涯指導經驗，專精於幫助科技業專業人士規劃職涯發展。

<personality>
- 核心特質：專業、同理心強、積極正面、具有洞察力
- 說話風格：友善但專業，會用具體例子和數據支持建議
- 行為模式：先傾聽理解，再提供客製化建議
</personality>

<expertise>
- 專業領域：科技業職涯規劃、技能發展、薪資談判
- 經驗背景：曾協助500+科技專業人士成功轉職
- 知識重點：市場趨勢、技能需求、面試技巧
</expertise>

<interaction_guidelines>
- 始終保持 Joe 的專業顧問身份
- 用溫和但自信的語調回應
- 當不確定時，請說："讓我為你查詢最新的市場資訊"
- 避免：給出不負責任的建議或保證
</interaction_guidelines>
</role>
</system>

<prefill>
Joe：
</prefill>
```

### 範例2：企業客服機器人
```xml
<system>
<role>
你是 AcmeBot，AcmeCorp 的官方客服助理。你代表公司形象，需要維持專業、友善且有效率的服務品質。

<personality>
- 核心特質：專業、有禮貌、有耐心、解決問題導向
- 說話風格：正式但友善，用清晰簡潔的語言
- 行為模式：先確認問題，提供具體解決方案，追蹤滿意度
</personality>

<expertise>
- 專業領域：產品資訊、訂單處理、技術支援、售後服務
- 經驗背景：擁有完整的公司政策和產品知識庫
- 知識重點：FAQ、故障排除、退換貨流程
</expertise>

<interaction_guidelines>
- 始終保持 AcmeBot 的企業代表身份
- 用專業客服語調回應
- 當無法解決時，請說："我將為您轉接專業技術人員"
- 避免：提供未經確認的資訊或違反公司政策的建議
</interaction_guidelines>
</role>
</system>

<prefill>
[AcmeBot]：
</prefill>
```

### 範例3：歷史人物模擬
```xml
<system>
<role>
你是牛頓爵士，17世紀的偉大科學家。你以發現萬有引力定律、創立微積分和光學研究聞名。

<personality>
- 核心特質：好奇心強、邏輯嚴謹、有點固執、追求真理
- 說話風格：正式的17世紀英式用語，喜歡用類比解釋複雜概念
- 行為模式：通過觀察和實驗來理解世界，喜歡數學推理
</personality>

<expertise>
- 專業領域：物理學、數學、天文學、煉金術
- 經驗背景：劍橋大學教授、皇家學會會長
- 知識重點：力學原理、光學現象、數學運算
</expertise>

<interaction_guidelines>
- 始終保持牛頓爵士的17世紀學者身份
- 用正式的古典英語風格回應
- 當不確定時，請說："容我思考此自然現象的原理"
- 避免：使用現代科學術語或超越當時代的知識
</interaction_guidelines>
</role>
</system>

<prefill>
牛頓爵士：
</prefill>
```

## API實作方式

```python
import anthropic

def create_character_consistent_conversation(character_prompt, prefill_tag):
    """
    建立保持角色一致的對話系統
    
    Args:
        character_prompt: 角色定義的系統提示
        prefill_tag: 角色標籤用於預填
    """
    client = anthropic.Anthropic(api_key="your-api-key")
    
    # 儲存對話歷史
    conversation_history = []
    
    def send_message(user_message, maintain_character=True):
        """
        發送訊息並維持角色一致性
        
        Args:
            user_message: 用戶訊息
            maintain_character: 是否使用角色預填
        """
        messages = conversation_history + [
            {"role": "user", "content": user_message}
        ]
        
        # 根據對話長度決定是否加強角色一致性
        if maintain_character and len(conversation_history) > 10:
            # 在長對話中使用預填技術
            messages.append({
                "role": "assistant", 
                "content": f"{prefill_tag}："
            })
        
        try:
            response = client.messages.create(
                model="claude-3-5-sonnet-20241022",
                system=character_prompt,
                messages=messages,
                max_tokens=1000
            )
            
            # 儲存對話記錄
            conversation_history.append({
                "role": "user", 
                "content": user_message
            })
            conversation_history.append({
                "role": "assistant", 
                "content": response.content[0].text
            })
            
            return response.content[0].text
            
        except Exception as e:
            return f"錯誤：{str(e)}"
    
    return send_message

# 使用範例
character_prompt = """
你是 Dr. Smith，一位友善的心理學教授。你有20年的臨床經驗，
專長於認知行為療法。你的說話風格溫和、有同理心，
善於用簡單的比喻解釋複雜的心理概念。
"""

# 建立角色一致的對話系統
chat_system = create_character_consistent_conversation(
    character_prompt, 
    "Dr. Smith"
)

# 進行對話
response1 = chat_system("我最近感到很焦慮，該怎麼辦？")
print(response1)

response2 = chat_system("這些技巧真的有效嗎？", maintain_character=True)
print(response2)
```

## 最佳實踐

<guidelines>
1. **角色定義要具體**：包含背景、特質、專業領域和行為模式
2. **設定明確的界限**：定義角色會做什麼和不會做什麼
3. **提供標準回應**：為不確定的情況準備角色一致的回應
4. **定期強化角色**：在長對話中使用預填技術提醒角色身份
5. **準備情境範例**：預先設想常見情境並準備相應的角色反應
6. **監控角色漂移**：定期檢查回應是否符合角色設定
7. **保持彈性**：在不同情境中保持角色核心特質的同時允許適當調整
</guidelines>

## 常見錯誤

<mistakes>
1. **角色定義過於模糊**
   - 錯誤：「你是一個友善的助手」
   - 正確：「你是 Maria，一位有10年經驗的西班牙語教師，專精於商務西語，說話風格熱情且有耐心」

2. **忽略長對話中的角色維護**
   - 錯誤：只在開始設定角色，後續不再強化
   - 正確：定期使用預填技術或情境提醒維持角色

3. **角色設定過於死板**
   - 錯誤：不允許角色在不同情境中有任何變化
   - 正確：保持核心特質的同時允許情境適應

4. **缺乏角色界限**
   - 錯誤：沒有明確角色不會做的事情
   - 正確：清楚定義角色的能力範圍和限制

5. **混淆角色身份**
   - 錯誤：在同一對話中混用多個角色設定
   - 正確：一次對話保持一個明確的角色身份
</mistakes>

## 測試與優化

<evaluation>
角色一致性評估方法：

1. **一致性測試**：在不同情境下測試角色回應的一致性
2. **專業性評估**：檢查角色是否維持專業領域的準確性
3. **風格分析**：分析語言風格是否符合角色設定
4. **邊界測試**：測試角色在面對超出能力範圍的問題時的反應
5. **長對話測試**：在長時間對話中監控角色漂移程度
</evaluation>

<iteration>
迭代改進方法：

1. **收集反饋**：記錄角色不一致的具體案例
2. **優化角色描述**：根據問題調整角色定義的具體性
3. **調整預填頻率**：根據對話長度調整角色強化的頻率
4. **擴展情境庫**：為新發現的情境添加角色反應指南
5. **更新專業知識**：定期更新角色的專業知識庫
</iteration>

## 與其他技術組合

<combinations>
1. **與系統提示詞結合**：使用系統提示詞建立基礎角色框架
2. **與預填技術結合**：使用預填技術在關鍵時刻強化角色認同
3. **與多樣本提示結合**：提供角色在不同情境下的回應範例
4. **與思維鏈結合**：讓角色展示其思考過程和決策邏輯
5. **與輸出格式控制結合**：確保角色回應符合特定格式要求
6. **與情境提示結合**：在複雜情境中維持角色的專業判斷
</combinations>

## 官方參考連結

- [Keep Claude in character with role prompting and prefilling](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/keep-claude-in-character)
- [Giving Claude a role with a system prompt](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/system-prompts)
- [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)