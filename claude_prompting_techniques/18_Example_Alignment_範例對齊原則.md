# 18. 範例對齊原則(Example Alignment)

## 官方定義與說明

<definition>
範例對齊原則是指確保提供給 Claude 的範例與您想要鼓勵的行為完全一致，同時最小化您想要避免的行為。根據 Anthropic 官方文檔，Claude 4 模型會密切關注細節和範例作為指令遵循的一部分，因此範例的品質和一致性直接影響輸出品質。
</definition>

## 核心原理

<principle>
範例對齊原則的核心在於透過精心設計的範例來引導 Claude 的行為模式。Claude 會從提供的範例中學習並模仿其結構、風格和邏輯。當範例與期望行為完全對齊時，Claude 能夠更準確地理解任務需求並產生一致的輸出。此原理建立在機器學習的模式識別基礎上，透過示範代替抽象說明來傳達複雜指令。
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，範例對齊原則能夠帶來以下效益：
1. 提高準確性：減少對指令的誤解
2. 確保一致性：強制執行統一的結構和格式
3. 增強效能：提升處理複雜任務的能力
4. 降低歧義：透過具體示範消除模糊性
5. 加速學習：使 Claude 更快理解複雜的任務需求
</benefits>

## 實作格式

### 基本結構
```xml
<system>
請按照以下範例的格式和風格來回應：

<example>
輸入：[示範輸入]
輸出：[期望輸出]
</example>
</system>

<user>
[實際任務內容]
</user>
```

### 完整結構
```xml
<system>
您是一位專業的 [角色定義]。請嚴格按照以下範例的格式、風格和邏輯來處理任務。

<examples>
<example>
情境：[情境描述]
輸入：[示範輸入]
輸出：[期望輸出]
說明：[為什麼這個輸出是正確的]
</example>

<example>
情境：[不同情境描述]
輸入：[示範輸入]
輸出：[期望輸出]
說明：[為什麼這個輸出是正確的]
</example>

<example>
情境：[邊界情況描述]
輸入：[示範輸入]
輸出：[期望輸出]
說明：[為什麼這個輸出是正確的]
</example>
</examples>

重要原則：
- 嚴格遵循範例的格式和結構
- 保持一致的風格和語調
- 確保邏輯推理過程與範例一致
</system>

<user>
[實際任務內容]
</user>
```

## 實作範例

### 範例1：客戶服務回應格式對齊
```xml
<system>
您是一位專業的客戶服務代表。請按照以下範例的格式和風格來回應客戶問題。

<examples>
<example>
客戶問題：我的訂單什麼時候到貨？
回應：
感謝您的查詢。

訂單狀態：正在處理中
預計到貨時間：3-5個工作天
追蹤碼：將在24小時內透過email發送

如有其他問題，請隨時聯繫我們。
祝您愉快！
</example>

<example>
客戶問題：我想要退換商品
回應：
感謝您的聯繫。

退換政策：購買後30天內可退換
所需文件：原始發票和商品包裝
處理時間：收到商品後7個工作天

請告知您的訂單編號，我們將協助您處理。
祝您愉快！
</example>
</examples>
</system>

<user>
客戶問題：我的帳戶被鎖定了，怎麼辦？
</user>
```

### 範例2：程式碼審查格式對齊
```xml
<system>
您是一位資深的程式碼審查專家。請按照以下範例的格式和風格來進行程式碼審查。

<examples>
<example>
程式碼：
```python
def calculate_total(items):
    total = 0
    for item in items:
        total += item.price
    return total
```

審查結果：
**功能性：** ✅ 正確
**效能：** ✅ 良好
**可讀性：** ✅ 清晰

**建議改進：**
- 考慮使用 sum() 函數簡化程式碼
- 添加型別提示增強可讀性
- 考慮處理空清單的邊界情況

**修改建議：**
```python
def calculate_total(items: List[Item]) -> float:
    return sum(item.price for item in items) if items else 0.0
```
</example>
</examples>
</system>

<user>
請審查以下程式碼：
```python
def validate_email(email):
    if "@" in email:
        return True
    return False
```
</user>
```

### 範例3：學術論文摘要格式對齊
```xml
<system>
您是一位學術寫作專家。請按照以下範例的格式和風格來撰寫論文摘要。

<examples>
<example>
論文標題：機器學習在醫療診斷中的應用
摘要：
【背景】隨著人工智慧技術的快速發展，機器學習在醫療診斷領域展現出巨大潛力。【目的】本研究旨在評估深度學習模型在醫學影像診斷中的準確性和實用性。【方法】採用卷積神經網絡（CNN）分析10,000張X光片，建立肺部疾病診斷模型。【結果】模型在測試集上達到95.2%的準確率，敏感度為93.8%，特異度為96.5%。【結論】機器學習技術能夠有效輔助醫師進行診斷，提高診斷效率和準確性。

關鍵詞：機器學習、醫療診斷、深度學習、醫學影像
</example>
</examples>
</system>

<user>
請為以下研究撰寫摘要：
研究主題：區塊鏈技術在供應鏈管理中的應用
研究發現：提高透明度40%，減少成本15%，增強信任度
</user>
```

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your_api_key")

# 範例對齊原則的API實作
def create_aligned_prompt(examples, task_description):
    # 構建範例結構
    examples_text = "<examples>\n"
    for i, example in enumerate(examples, 1):
        examples_text += f"<example>\n"
        examples_text += f"輸入：{example['input']}\n"
        examples_text += f"輸出：{example['output']}\n"
        if 'explanation' in example:
            examples_text += f"說明：{example['explanation']}\n"
        examples_text += f"</example>\n\n"
    examples_text += "</examples>"
    
    system_prompt = f"""
您需要嚴格按照以下範例的格式、風格和邏輯來處理任務。

{examples_text}

重要原則：
- 完全遵循範例的結構和格式
- 保持一致的風格和語調
- 確保輸出品質與範例一致
"""
    
    return system_prompt

# 使用範例
examples = [
    {
        "input": "撰寫一份產品介紹：智能手錶",
        "output": "**智能手錶 - 科技與時尚的完美結合**\n\n✨ 主要特色：\n- 全天候健康監測\n- 50+ 運動模式\n- 7天超長續航\n- IP68防水設計\n\n💡 適用族群：運動愛好者、商務人士、健康管理者\n\n🎯 核心價值：讓科技成為生活的智慧助手",
        "explanation": "使用結構化格式、表情符號增加吸引力、突出核心賣點"
    },
    {
        "input": "撰寫一份產品介紹：藍牙耳機",
        "output": "**藍牙耳機 - 無線音樂新體驗**\n\n✨ 主要特色：\n- 主動降噪技術\n- HiFi音質表現\n- 30小時總續航\n- 快充5分鐘用2小時\n\n💡 適用族群：音樂愛好者、通勤族、商務人士\n\n🎯 核心價值：沉浸式音樂體驗，隨時隨地享受純淨音質",
        "explanation": "保持相同的結構格式，突出音質特色，維持一致的表達風格"
    }
]

system_prompt = create_aligned_prompt(examples, "產品介紹撰寫")

# 發送請求
response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1000,
    system=system_prompt,
    messages=[
        {"role": "user", "content": "撰寫一份產品介紹：無線充電器"}
    ]
)

print(response.content[0].text)
```

## 最佳實踐

<guidelines>
1. **範例數量原則**：提供3-5個多樣化的相關範例，更多範例通常能改善表現
2. **多樣性確保**：範例應涵蓋邊界情況和潛在挑戰
3. **結構一致性**：所有範例應遵循相同的格式和結構
4. **品質標準**：每個範例都應代表您期望的最高品質輸出
5. **相關性驗證**：確保範例與實際使用情境密切相關
6. **XML標籤規範**：使用 `<example>` 和 `<examples>` 標籤清晰包裝範例
7. **解釋說明**：在複雜情況下，為範例提供解釋說明
8. **負面範例避免**：避免展示您不希望看到的行為模式
9. **逐步驗證**：測試範例是否真正引導出期望的行為
10. **持續優化**：根據實際效果不斷調整和改進範例
</guidelines>

## 常見錯誤

<mistakes>
1. **範例不一致**：提供的範例格式、風格或邏輯不一致
   - 錯誤：有些範例使用正式語調，有些使用非正式語調
   - 正確：所有範例保持一致的語調和風格

2. **範例過於簡單**：提供的範例無法涵蓋實際複雜情況
   - 錯誤：只提供基本情況的範例
   - 正確：包含邊界情況和複雜情境的範例

3. **負面範例展示**：意外展示了不希望的行為
   - 錯誤：在範例中包含錯誤的處理方式
   - 正確：只展示期望的正確行為

4. **範例與任務不符**：範例與實際任務需求不匹配
   - 錯誤：提供文章寫作範例但任務是數據分析
   - 正確：確保範例完全符合任務需求

5. **格式不規範**：未使用適當的XML標籤結構
   - 錯誤：範例混雜在普通文字中
   - 正確：使用 `<example>` 標籤清晰包裝

6. **缺乏多樣性**：所有範例都很相似
   - 錯誤：提供三個幾乎相同的範例
   - 正確：確保範例涵蓋不同情境和挑戰

7. **過度複雜化**：範例過於複雜難以理解
   - 錯誤：提供包含過多細節的複雜範例
   - 正確：保持範例清晰簡潔但完整

8. **缺少關鍵細節**：範例遺漏重要的格式或內容要素
   - 錯誤：範例不完整，缺少重要部分
   - 正確：確保範例包含所有必要元素
</mistakes>

## 測試與優化

<evaluation>
評估範例對齊效果的方法：

1. **一致性測試**：使用相同類型的多個輸入測試，檢查輸出是否保持一致的格式和風格
2. **邊界情況測試**：測試極端或不常見的情況，確保範例能夠處理各種情境
3. **格式遵循測試**：驗證輸出是否嚴格遵循範例的結構和格式
4. **品質對比測試**：比較使用範例前後的輸出品質差異
5. **專家評估**：邀請領域專家評估輸出是否符合專業標準
6. **使用者反饋**：收集實際使用者對輸出品質的反饋
7. **A/B測試**：比較不同範例組合的效果差異
</evaluation>

<iteration>
迭代改進範例對齊的方法：

1. **範例品質提升**：根據測試結果持續改進範例的品質和完整性
2. **多樣性增強**：添加更多涵蓋不同情境的範例
3. **結構優化**：調整範例的組織結構和展示方式
4. **說明補充**：為複雜範例添加解釋說明
5. **負面篩選**：識別並移除可能引導不良行為的範例
6. **格式統一**：確保所有範例遵循統一的格式標準
7. **效果追蹤**：建立指標體系追蹤範例對齊的長期效果
8. **反饋循環**：建立持續的反饋收集和改進機制
</iteration>

## 與其他技術組合

<combinations>
範例對齊原則可以與以下技術有效組合：

1. **系統提示詞**：在系統提示中定義角色，在範例中展示該角色的行為模式
2. **多樣本提示**：範例對齊是多樣本提示的核心原理
3. **XML標籤結構化**：使用XML標籤清晰組織和展示範例
4. **輸出格式控制**：透過範例精確控制輸出格式
5. **思維鏈推理**：在範例中展示完整的思維推理過程
6. **具體性原則**：提供具體詳細的範例而非抽象說明
7. **清晰直接指示**：結合明確指示和具體範例
8. **成功標準定義**：範例即是成功標準的具體展示
9. **測試案例開發**：使用範例作為測試案例的基礎
10. **迭代改進**：根據測試結果持續優化範例品質
</combinations>

## 官方參考連結

- [Anthropic - Use examples (multishot prompting) to guide Claude's behavior](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/multishot-prompting)
- [Anthropic - Claude 4 prompt engineering best practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Anthropic - Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Anthropic - Be clear, direct, and detailed](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct)