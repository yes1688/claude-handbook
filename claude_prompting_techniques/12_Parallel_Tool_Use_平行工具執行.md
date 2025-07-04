# 12. 平行工具執行(Parallel Tool Use)

## 官方定義與說明

<definition>
平行工具執行是 Claude 在單一回應中同時執行多個工具呼叫的能力。根據 Anthropic 官方文檔，預設情況下，Claude 可以使用多個工具來回答使用者查詢，這個功能讓 Claude 能夠同時執行多個工具呼叫，提高效率並減少來回互動的次數。開發者可以透過設定 `disable_parallel_tool_use=true` 來禁用此功能，確保 Claude 在每次回應中最多只使用一個工具。
</definition>

## 核心原理

<principle>
平行工具執行的核心原理是讓 Claude 在分析使用者請求後，識別出需要使用多個工具才能完成的任務，然後在單一回應中同時發起多個工具呼叫。這種機制基於以下工作流程：

1. 分析使用者查詢，識別所需的多個工具
2. 同時發起多個工具呼叫請求
3. 等待所有工具執行完成
4. 整合多個工具的結果
5. 提供綜合性回應

此機制特別適合需要從多個資料來源蒐集資訊或執行相互獨立的多個操作的場景。
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，平行工具執行提供以下效益：

1. **效率提升**：同時執行多個工具可以顯著減少整體執行時間
2. **減少延遲**：避免序列執行工具造成的累積延遲
3. **更好的使用者體驗**：減少使用者等待時間和來回互動次數
4. **資源優化**：更有效地利用系統資源和API呼叫配額
5. **增強功能性**：能夠處理更複雜的多步驟任務
6. **提高準確性**：同時獲取多個資料來源的資訊，提供更全面的回應
</benefits>

## 實作格式

### 基本結構
```python
# 基本平行工具執行 API 呼叫
client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1000,
    messages=[{"role": "user", "content": "需要多個工具的查詢"}],
    tools=[tool1, tool2, tool3],
    # 平行工具執行預設為啟用
)
```

### 完整結構
```python
# 完整平行工具執行設定
client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1000,
    messages=[{"role": "user", "content": "查詢內容"}],
    tools=[tool1, tool2, tool3],
    tool_choice={
        "type": "auto",
        "disable_parallel_tool_use": False  # 明確啟用平行工具執行
    }
)
```

### 禁用平行工具執行
```python
# 禁用平行工具執行
client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1000,
    messages=[{"role": "user", "content": "查詢內容"}],
    tools=[tool1, tool2, tool3],
    tool_choice={
        "type": "auto",
        "disable_parallel_tool_use": True  # 禁用平行工具執行
    }
)
```

## 實作範例

### 範例1：天氣和時間查詢
```python
import anthropic

# 定義工具
def get_weather(location):
    return f"The weather in {location} is 72°F and sunny."

def get_time(location):
    return f"The current time in {location} is 3:45 PM."

# 工具定義
weather_tool = {
    "name": "get_weather",
    "description": "Gets the current weather for a given location",
    "input_schema": {
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "The city and state, e.g. San Francisco, CA"
            }
        },
        "required": ["location"]
    }
}

time_tool = {
    "name": "get_time",
    "description": "Gets the current time in a given location",
    "input_schema": {
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "The city and state, e.g. San Francisco, CA"
            }
        },
        "required": ["location"]
    }
}

# 平行工具執行
client = anthropic.Anthropic(api_key="your_api_key")

response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1000,
    messages=[
        {"role": "user", "content": "What's the weather and time in New York?"}
    ],
    tools=[weather_tool, time_tool]
)

# Claude 會同時呼叫兩個工具
```

### 範例2：Claude 3.7 Sonnet 的批次工具解決方案
```python
# 針對 Claude 3.7 Sonnet 的批次工具解決方案
batch_tool = {
    "name": "batch_tool",
    "description": "Invoke multiple other tool calls simultaneously",
    "input_schema": {
        "type": "object",
        "properties": {
            "invocations": {
                "type": "array",
                "description": "The tool calls to invoke",
                "items": {
                    "type": "object",
                    "properties": {
                        "name": {"type": "string", "description": "Tool name"},
                        "arguments": {"type": "string", "description": "JSON string of arguments"}
                    },
                    "required": ["name", "arguments"]
                }
            }
        },
        "required": ["invocations"]
    }
}

def process_batch_tool(invocations):
    results = []
    for inv in invocations:
        if inv["name"] == "get_weather":
            args = json.loads(inv["arguments"])
            results.append(get_weather(args["location"]))
        elif inv["name"] == "get_time":
            args = json.loads(inv["arguments"])
            results.append(get_time(args["location"]))
    return results

# 使用批次工具
response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1000,
    messages=[
        {"role": "user", "content": "Get weather and time for San Francisco and New York"}
    ],
    tools=[weather_tool, time_tool, batch_tool]
)
```

### 範例3：資料庫查詢與API呼叫的平行執行
```python
# 資料庫查詢工具
database_tool = {
    "name": "query_database",
    "description": "Query the customer database for information",
    "input_schema": {
        "type": "object",
        "properties": {
            "query": {
                "type": "string",
                "description": "SQL query to execute"
            }
        },
        "required": ["query"]
    }
}

# 外部API呼叫工具
api_tool = {
    "name": "call_external_api",
    "description": "Make a call to an external API service",
    "input_schema": {
        "type": "object",
        "properties": {
            "endpoint": {
                "type": "string",
                "description": "API endpoint URL"
            },
            "method": {
                "type": "string",
                "description": "HTTP method (GET, POST, etc.)"
            },
            "params": {
                "type": "object",
                "description": "Request parameters"
            }
        },
        "required": ["endpoint", "method"]
    }
}

# 平行執行多個資料來源查詢
response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1000,
    messages=[
        {"role": "user", "content": "Get customer data from database and pricing from external API for product ABC123"}
    ],
    tools=[database_tool, api_tool],
    tool_choice={"type": "auto"}  # 預設啟用平行工具執行
)
```

## API實作方式

```python
import anthropic
import json
import asyncio

class ParallelToolExecutor:
    def __init__(self, api_key):
        self.client = anthropic.Anthropic(api_key=api_key)
        self.tools = {}
        
    def register_tool(self, name, function, schema):
        """註冊工具函數"""
        self.tools[name] = {
            "function": function,
            "schema": schema
        }
    
    def get_tool_schemas(self):
        """獲取所有工具的模式定義"""
        return [tool["schema"] for tool in self.tools.values()]
    
    async def execute_tools(self, tool_calls):
        """平行執行多個工具呼叫"""
        tasks = []
        for tool_call in tool_calls:
            tool_name = tool_call["name"]
            if tool_name in self.tools:
                tool_function = self.tools[tool_name]["function"]
                tool_input = tool_call["input"]
                tasks.append(tool_function(**tool_input))
        
        results = await asyncio.gather(*tasks)
        return results
    
    def process_message(self, user_message):
        """處理使用者訊息並執行工具"""
        # 發送訊息給Claude
        response = self.client.messages.create(
            model="claude-3-5-sonnet-20241022",
            max_tokens=1000,
            messages=[{"role": "user", "content": user_message}],
            tools=self.get_tool_schemas(),
            tool_choice={"type": "auto"}
        )
        
        # 檢查是否需要執行工具
        if response.stop_reason == "tool_use":
            tool_calls = []
            for content in response.content:
                if content.type == "tool_use":
                    tool_calls.append({
                        "name": content.name,
                        "input": content.input,
                        "id": content.id
                    })
            
            # 平行執行工具
            results = asyncio.run(self.execute_tools(tool_calls))
            
            # 將結果回傳給Claude
            tool_results = []
            for i, result in enumerate(results):
                tool_results.append({
                    "type": "tool_result",
                    "tool_use_id": tool_calls[i]["id"],
                    "content": str(result)
                })
            
            # 獲取最終回應
            final_response = self.client.messages.create(
                model="claude-3-5-sonnet-20241022",
                max_tokens=1000,
                messages=[
                    {"role": "user", "content": user_message},
                    {"role": "assistant", "content": response.content},
                    {"role": "user", "content": tool_results}
                ]
            )
            
            return final_response.content[0].text
        
        return response.content[0].text

# 使用範例
executor = ParallelToolExecutor("your_api_key")

# 註冊工具
executor.register_tool("get_weather", get_weather, weather_tool)
executor.register_tool("get_time", get_time, time_tool)

# 執行查詢
result = executor.process_message("What's the weather and time in Tokyo?")
print(result)
```

## 最佳實踐

<guidelines>
實施平行工具執行時應遵循以下最佳實踐：

1. **工具獨立性**：確保同時執行的工具之間沒有依賴關係
2. **詳細描述**：為每個工具提供極其詳細的描述，包括預期輸入和輸出
3. **錯誤處理**：實作健全的錯誤處理機制，以應對部分工具失敗的情況
4. **超時控制**：設定合理的超時時間，避免長時間等待
5. **資源管理**：監控API配額和系統資源使用情況
6. **結果驗證**：驗證工具執行結果的正確性和一致性
7. **日誌記錄**：記錄所有工具呼叫和結果，便於調試和監控
8. **選擇性禁用**：在不需要平行執行或順序很重要的場景下，適當禁用平行工具執行
9. **模型選擇**：根據模型特性選擇適合的實作方式（如Claude 3.7 Sonnet需要批次工具解決方案）
10. **性能優化**：根據實際使用情況調整並發數量和執行策略
</guidelines>

## 常見錯誤

<mistakes>
實作平行工具執行時常見的錯誤包括：

1. **工具依賴性錯誤**：
   - 錯誤：讓相互依賴的工具並行執行
   - 解決：確保平行執行的工具之間沒有依賴關係

2. **資源競爭**：
   - 錯誤：多個工具同時存取同一資源導致衝突
   - 解決：實作適當的資源鎖定機制

3. **錯誤處理不當**：
   - 錯誤：未處理部分工具失敗的情況
   - 解決：實作健全的錯誤處理和降級機制

4. **超時設定不當**：
   - 錯誤：設定過短或過長的超時時間
   - 解決：根據工具特性設定合理的超時時間

5. **模型限制忽略**：
   - 錯誤：忽略Claude 3模型不支援平行工具呼叫的限制
   - 解決：針對不同模型採用適當的實作策略

6. **工具描述不完整**：
   - 錯誤：工具描述過於簡略
   - 解決：提供詳細的工具描述和使用場景

7. **結果整合錯誤**：
   - 錯誤：未正確整合多個工具的執行結果
   - 解決：實作適當的結果合併邏輯

8. **配額管理不當**：
   - 錯誤：未考慮API配額限制
   - 解決：實作配額監控和速率限制機制
</mistakes>

## 測試與優化

<evaluation>
評估平行工具執行效果的方法：

1. **性能測試**：
   - 比較平行執行與序列執行的時間差異
   - 測量總體回應時間和使用者滿意度

2. **準確性測試**：
   - 驗證平行執行結果的正確性
   - 比較與單獨執行工具的結果一致性

3. **穩定性測試**：
   - 測試高並發情況下的系統穩定性
   - 評估錯誤率和恢復能力

4. **資源使用測試**：
   - 監控CPU、記憶體和網路資源使用情況
   - 評估API配額消耗情況

5. **使用者體驗測試**：
   - 收集使用者回饋和滿意度
   - 測量實際使用場景中的效果
</evaluation>

<iteration>
持續改進平行工具執行的方法：

1. **監控指標**：
   - 追蹤工具執行時間、成功率和錯誤率
   - 監控使用者查詢模式和需求變化

2. **A/B測試**：
   - 比較不同平行執行策略的效果
   - 測試不同工具組合的性能表現

3. **回饋循環**：
   - 收集使用者回饋和改進建議
   - 根據實際使用情況調整工具設定

4. **性能調優**：
   - 優化工具執行順序和並發數量
   - 調整超時設定和重試機制

5. **技術更新**：
   - 跟進Anthropic的最新技術更新
   - 適應新模型的特性和能力變化
</iteration>

## 與其他技術組合

<combinations>
平行工具執行可以與以下技術組合使用：

1. **系統提示詞**：
   - 設定工具使用的整體策略和優先順序
   - 定義工具選擇的決策邏輯

2. **思維鏈（Chain of Thought）**：
   - 在複雜任務中引導Claude的推理過程
   - 幫助決定何時使用平行工具執行

3. **XML標籤結構化**：
   - 使用XML標籤組織工具輸入和輸出
   - 提供結構化的工具執行結果

4. **輸出格式控制**：
   - 統一多個工具結果的輸出格式
   - 確保結果的一致性和可讀性

5. **預填回應技術**：
   - 預設工具執行的結果格式
   - 引導Claude生成預期的回應結構

6. **提示範本與變數**：
   - 使用範本管理複雜的工具組合
   - 支援動態工具選擇和配置

7. **長上下文技巧**：
   - 在長對話中保持工具執行的上下文
   - 管理大量工具執行歷史

8. **測試案例開發**：
   - 開發全面的測試用例
   - 驗證不同工具組合的效果
</combinations>

## 官方參考連結

- [Anthropic Tool Use Documentation](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)
- [How to implement tool use](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/implement-tool-use)
- [Anthropic Cookbook - Parallel Tools Example](https://github.com/anthropics/anthropic-cookbook/blob/main/tool_use/parallel_tools_claude_3_7_sonnet.ipynb)
- [Claude Tool Use GA Announcement](https://www.anthropic.com/news/tool-use-ga)