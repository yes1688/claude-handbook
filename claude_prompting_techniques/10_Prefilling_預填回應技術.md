# 10. 預填回應技術(Prefilling)

## 官方定義與說明

<definition>
預填回應技術是在 Assistant 訊息中包含初始文本來引導 Claude 回應的技術。通過在 Assistant 角色的訊息中預先填入部分內容，Claude 會從預填內容的末尾繼續生成回應，實現對輸出方向、格式和角色一致性的精確控制。
</definition>

## 核心原理

<principle>
預填回應技術的核心原理是利用 Claude 的對話延續機制。當 API 請求中的最後一個訊息使用 assistant 角色時，Claude 會將該訊息的內容視為自己已經開始的回應，並從該內容的末尾繼續生成文本。這種機制允許開發者：

1. 控制回應的起始方向
2. 強制特定的輸出格式
3. 跳過不必要的前導語
4. 維持角色扮演的一致性
5. 提高回應的準確性和相關性
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，預填回應技術提供以下關鍵效益：

1. **增強可控性**：通過提供初始文本，可以將 Claude 的回應導向期望的方向
2. **格式控制**：精確指定輸出格式，特別適用於 JSON、XML 等結構化數據
3. **跳過前導語**：直接進入核心內容，避免不必要的解釋性文字
4. **角色一致性**：在長對話中維持特定角色的人格特徵
5. **效能提升**：當 Claude 回應不符合期望時，可通過預填進行修正
</benefits>

## 實作格式

### 基本結構
```python
{
    "role": "assistant", 
    "content": "[預填內容]"
}
```

### 完整結構
```python
messages = [
    {"role": "user", "content": "[使用者訊息]"},
    {"role": "assistant", "content": "[預填內容]"}
]
```

## 實作範例

### 範例1：JSON 格式強制輸出
```python
import anthropic

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-3-sonnet-20240229",
    max_tokens=1024,
    messages=[
        {
            "role": "user", 
            "content": "分析這個產品的優缺點：iPhone 15 Pro"
        },
        {
            "role": "assistant", 
            "content": "{"
        }
    ]
)

# Claude 會直接輸出 JSON 格式，跳過前導語
# 預期輸出：{"advantages": ["高效能處理器", "優秀相機系統"], "disadvantages": ["價格昂貴", "電池續航力"]}
```

### 範例2：角色扮演維持
```python
import anthropic

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-3-sonnet-20240229",
    max_tokens=1024,
    messages=[
        {
            "role": "system",
            "content": "你是一位經驗豐富的偵探夏洛克·福爾摩斯，以邏輯推理和觀察細節著稱。"
        },
        {
            "role": "user", 
            "content": "房間裡有打鬥的痕跡，你看到了什麼？"
        },
        {
            "role": "assistant", 
            "content": "[夏洛克·福爾摩斯]"
        }
    ]
)

# Claude 會以夏洛克·福爾摩斯的角色繼續回應
# 預期輸出：觀察這個房間，我注意到幾個關鍵細節...
```

### 範例3：XML 結構化輸出
```python
import anthropic

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-3-sonnet-20240229",
    max_tokens=1024,
    messages=[
        {
            "role": "user", 
            "content": "為「機器學習」這個主題建立一個學習計劃"
        },
        {
            "role": "assistant", 
            "content": "<learning_plan>\n<topic>機器學習</topic>\n<weeks>"
        }
    ]
)

# Claude 會繼續完成 XML 結構
# 預期輸出：<week1><focus>數學基礎</focus><tasks>線性代數、統計學</tasks></week1>...
```

## API實作方式

```python
import anthropic

class ClaudePrefillHandler:
    def __init__(self, api_key):
        self.client = anthropic.Anthropic(api_key=api_key)
    
    def json_prefill(self, user_message, model="claude-3-sonnet-20240229"):
        """強制 JSON 輸出的預填方法"""
        response = self.client.messages.create(
            model=model,
            max_tokens=1024,
            messages=[
                {"role": "user", "content": user_message},
                {"role": "assistant", "content": "{"}
            ]
        )
        return "{" + response.content[0].text
    
    def role_prefill(self, user_message, role_name, model="claude-3-sonnet-20240229"):
        """角色扮演預填方法"""
        response = self.client.messages.create(
            model=model,
            max_tokens=1024,
            messages=[
                {"role": "user", "content": user_message},
                {"role": "assistant", "content": f"[{role_name}]"}
            ]
        )
        return response.content[0].text
    
    def format_prefill(self, user_message, prefill_content, model="claude-3-sonnet-20240229"):
        """通用格式預填方法"""
        response = self.client.messages.create(
            model=model,
            max_tokens=1024,
            messages=[
                {"role": "user", "content": user_message},
                {"role": "assistant", "content": prefill_content}
            ]
        )
        return prefill_content + response.content[0].text

# 使用範例
handler = ClaudePrefillHandler("your-api-key")

# JSON 輸出
json_result = handler.json_prefill("分析產品優缺點")

# 角色扮演
role_result = handler.role_prefill("描述犯罪現場", "夏洛克·福爾摩斯")

# 自訂格式
custom_result = handler.format_prefill("建立學習計劃", "<plan>\n<subject>")
```

## 最佳實踐

<guidelines>
1. **簡潔有效**：預填內容應該簡潔明確，通常幾個字符或一句話即可達到效果

2. **格式一致性**：確保預填內容與期望的最終格式保持一致

3. **角色標籤**：使用 [角色名稱] 格式維持角色扮演的一致性

4. **結構化輸出**：對於 JSON、XML 等結構化數據，使用開放性標籤作為預填

5. **避免尾隨空白**：預填內容不能以空白字符結尾，會導致 API 錯誤

6. **測試驗證**：在實際應用前測試預填效果，確保達到期望的輸出控制

7. **漸進調整**：根據實際效果調整預填內容的長度和具體內容
</guidelines>

## 常見錯誤

<mistakes>
1. **尾隨空白錯誤**
   - 錯誤：`"content": "As an AI assistant, I "`（末尾有空格）
   - 正確：`"content": "As an AI assistant, I"`（無尾隨空白）

2. **過度預填**
   - 錯誤：預填大量內容，限制 Claude 的創造性回應
   - 正確：預填關鍵起始部分，保留生成空間

3. **格式不一致**
   - 錯誤：預填 JSON 開頭但期望其他格式
   - 正確：預填內容與期望輸出格式保持一致

4. **角色混亂**
   - 錯誤：在不同對話中使用相同角色標籤但不同人格
   - 正確：保持角色標籤與人格設定的一致性

5. **忽略上下文**
   - 錯誤：預填內容與對話上下文不符
   - 正確：確保預填內容與整個對話流程自然銜接
</mistakes>

## 測試與優化

<evaluation>
評估預填效果的方法：

1. **輸出格式檢查**：驗證輸出是否符合預期格式
2. **內容相關性**：確認回應內容與預填方向一致
3. **角色一致性**：檢查角色扮演是否保持穩定
4. **效率測試**：比較有無預填的回應品質和速度
5. **邊界測試**：測試極端情況下的預填效果
</evaluation>

<iteration>
迭代改進策略：

1. **A/B 測試**：比較不同預填內容的效果
2. **長度調整**：測試不同長度預填的影響
3. **內容優化**：根據實際效果調整預填用詞
4. **格式微調**：針對特定輸出格式優化預填模板
5. **回饋循環**：根據使用者反饋持續改進預填策略
</iteration>

## 與其他技術組合

<combinations>
1. **系統提示詞 + 預填**
   - 系統提示詞設定角色和背景
   - 預填維持角色一致性
   - 提升角色扮演的穩定性

2. **XML 標籤 + 預填**
   - XML 標籤結構化輸入
   - 預填控制輸出格式
   - 實現精確的格式控制

3. **思維鏈 + 預填**
   - 思維鏈引導邏輯思考
   - 預填控制思考格式
   - 標準化推理過程輸出

4. **多樣本提示 + 預填**
   - 多樣本提供範例格式
   - 預填強化格式遵循
   - 提高輸出一致性

5. **工具呼叫 + 預填**
   - 工具呼叫執行特定功能
   - 預填格式化工具輸出
   - 優化工具結果呈現
</combinations>

## 官方參考連結

- [Anthropic 官方文檔：預填 Claude 回應](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response)
- [Anthropic 官方文檔：保持 Claude 角色一致性](https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/keep-claude-in-character)
- [Anthropic Messages API 文檔](https://docs.anthropic.com/en/docs/upgrading-to-the-messages-api)