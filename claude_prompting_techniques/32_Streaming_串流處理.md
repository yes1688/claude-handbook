# 32. 串流處理(Streaming)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，串流處理是一種讓 Claude API 以增量方式即時傳送回應的技術。透過設置 `stream: true` 參數，API 會使用伺服器推送事件(Server-Sent Events, SSE)來逐步串流回應內容，而不是等待完整回應產生後才一次性傳送。這項技術特別適合需要即時反饋的應用場景，如聊天機器人、互動式介面或長內容生成。
</definition>

## 核心原理

<principle>
串流處理的核心原理基於伺服器推送事件(SSE)協議，運作流程如下：

1. **請求階段**：客戶端發送 API 請求時設置 `stream: true` 參數
2. **回應開始**：伺服器立即回應並保持連接開放
3. **增量傳送**：伺服器以事件流形式逐步傳送回應片段
4. **事件結構**：每個事件包含事件類型和相關 JSON 資料
5. **內容組裝**：客戶端接收並組裝完整回應

事件流結構包含：
- `message_start`：開始事件
- `content_block_start`：內容區塊開始
- `content_block_delta`：內容增量片段
- `content_block_stop`：內容區塊結束
- `message_stop`：完整回應結束
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔強調串流處理的關鍵效益：

1. **即時回饋**：使用者可以立即看到回應開始產生，提升用戶體驗
2. **降低延遲感知**：雖然總處理時間相同，但使用者感受到的等待時間大幅減少
3. **互動性提升**：特別適合對話式應用，創造更自然的互動體驗
4. **長內容優化**：對於長篇回應，使用者可以開始閱讀而不需等待完整生成
5. **系統穩定性**：減少長時間等待造成的連線逾時問題
6. **資源效率**：客戶端可以更有效率地處理和顯示內容
</benefits>

## 實作格式

### 基本結構
```xml
<streaming_request>
  <api_call>
    <parameter name="model">claude-3-sonnet-20240229</parameter>
    <parameter name="stream">true</parameter>
    <parameter name="messages">[{"role": "user", "content": "請求內容"}]</parameter>
  </api_call>
</streaming_request>
```

### 完整結構
```xml
<streaming_implementation>
  <request_configuration>
    <model>claude-3-sonnet-20240229</model>
    <stream>true</stream>
    <max_tokens>1024</max_tokens>
    <messages>
      <message role="user">
        <content>用戶請求內容</content>
      </message>
    </messages>
  </request_configuration>
  
  <event_handling>
    <event_types>
      <type name="message_start">初始化回應</type>
      <type name="content_block_start">開始內容區塊</type>
      <type name="content_block_delta">增量內容片段</type>
      <type name="content_block_stop">結束內容區塊</type>
      <type name="message_stop">完整回應結束</type>
    </event_types>
  </event_handling>
  
  <error_handling>
    <error_types>
      <type name="overloaded_error">系統過載</type>
      <type name="rate_limit_error">請求限制</type>
      <type name="authentication_error">認證失敗</type>
    </error_types>
  </error_handling>
</streaming_implementation>
```

## 實作範例

### 範例1：基本文字串流
```xml
<streaming_example type="basic_text">
  <request>
    <model>claude-3-sonnet-20240229</model>
    <stream>true</stream>
    <messages>
      <message role="user">
        <content>請寫一篇關於人工智慧發展的短文</content>
      </message>
    </messages>
    <max_tokens>500</max_tokens>
  </request>
  
  <expected_events>
    <event type="message_start">
      <data>{"type": "message_start", "message": {"id": "msg_123", "type": "message", "role": "assistant", "content": []}}</data>
    </event>
    <event type="content_block_start">
      <data>{"type": "content_block_start", "index": 0, "content_block": {"type": "text", "text": ""}}</data>
    </event>
    <event type="content_block_delta">
      <data>{"type": "content_block_delta", "index": 0, "delta": {"type": "text_delta", "text": "人工智慧"}}</data>
    </event>
    <event type="content_block_delta">
      <data>{"type": "content_block_delta", "index": 0, "delta": {"type": "text_delta", "text": "的發展"}}</data>
    </event>
    <event type="content_block_stop">
      <data>{"type": "content_block_stop", "index": 0}</data>
    </event>
    <event type="message_stop">
      <data>{"type": "message_stop"}</data>
    </event>
  </expected_events>
</streaming_example>
```

### 範例2：工具使用串流
```xml
<streaming_example type="tool_use">
  <request>
    <model>claude-3-sonnet-20240229</model>
    <stream>true</stream>
    <messages>
      <message role="user">
        <content>請計算 15 * 23 的結果</content>
      </message>
    </messages>
    <tools>
      <tool>
        <name>calculator</name>
        <description>執行數學計算</description>
        <input_schema>
          <type>object</type>
          <properties>
            <expression>
              <type>string</type>
              <description>數學表達式</description>
            </expression>
          </properties>
        </input_schema>
      </tool>
    </tools>
  </request>
  
  <streaming_flow>
    <phase name="tool_preparation">
      <event type="content_block_start">
        <data>{"type": "content_block_start", "index": 0, "content_block": {"type": "tool_use", "id": "tool_123", "name": "calculator", "input": {}}}</data>
      </event>
    </phase>
    <phase name="tool_input_streaming">
      <event type="content_block_delta">
        <data>{"type": "content_block_delta", "index": 0, "delta": {"type": "input_json_delta", "partial_json": "{\"expression\": \"15 * 23\"}"}}</data>
      </event>
    </phase>
    <phase name="tool_completion">
      <event type="content_block_stop">
        <data>{"type": "content_block_stop", "index": 0}</data>
      </event>
    </phase>
  </streaming_flow>
</streaming_example>
```

### 範例3：延伸思考串流
```xml
<streaming_example type="extended_thinking">
  <request>
    <model>claude-3-sonnet-20240229</model>
    <stream>true</stream>
    <messages>
      <message role="user">
        <content>請分析這個數學問題的解決策略：如何找到一個數列的第n項公式？</content>
      </message>
    </messages>
    <thinking>true</thinking>
  </request>
  
  <thinking_stream>
    <phase name="thinking_start">
      <event type="content_block_start">
        <data>{"type": "content_block_start", "index": 0, "content_block": {"type": "thinking", "text": ""}}</data>
      </event>
    </phase>
    <phase name="thinking_process">
      <event type="content_block_delta">
        <data>{"type": "content_block_delta", "index": 0, "delta": {"type": "thinking_delta", "text": "我需要思考數列分析的不同方法..."}}</data>
      </event>
      <event type="content_block_delta">
        <data>{"type": "content_block_delta", "index": 0, "delta": {"type": "thinking_delta", "text": "首先要識別數列的類型..."}}</data>
      </event>
    </phase>
    <phase name="response_generation">
      <event type="content_block_start">
        <data>{"type": "content_block_start", "index": 1, "content_block": {"type": "text", "text": ""}}</data>
      </event>
      <event type="content_block_delta">
        <data>{"type": "content_block_delta", "index": 1, "delta": {"type": "text_delta", "text": "分析數列第n項公式的策略包括："}}</data>
      </event>
    </phase>
  </thinking_stream>
</streaming_example>
```

## API實作方式

```python
import anthropic
from anthropic import Anthropic

# 基本串流實作
def basic_streaming():
    client = Anthropic(api_key="your_api_key")
    
    with client.messages.stream(
        model="claude-3-sonnet-20240229",
        max_tokens=1024,
        messages=[{
            "role": "user", 
            "content": "請寫一首關於春天的詩"
        }]
    ) as stream:
        for text in stream.text_stream:
            print(text, end="", flush=True)

# 進階串流處理
def advanced_streaming():
    client = Anthropic(api_key="your_api_key")
    
    message = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=1024,
        messages=[{
            "role": "user",
            "content": "分析今年的科技趨勢"
        }],
        stream=True
    )
    
    accumulated_text = ""
    for event in message:
        if event.type == "content_block_delta":
            if event.delta.type == "text_delta":
                accumulated_text += event.delta.text
                print(event.delta.text, end="", flush=True)
        elif event.type == "message_stop":
            print(f"\n完整回應: {accumulated_text}")

# 錯誤處理串流
def error_handling_streaming():
    client = Anthropic(api_key="your_api_key")
    
    try:
        with client.messages.stream(
            model="claude-3-sonnet-20240229",
            max_tokens=1024,
            messages=[{
                "role": "user",
                "content": "請說明量子計算的基本原理"
            }]
        ) as stream:
            for event in stream:
                if event.type == "error":
                    print(f"錯誤: {event.error.type} - {event.error.message}")
                    break
                elif event.type == "content_block_delta":
                    if event.delta.type == "text_delta":
                        print(event.delta.text, end="", flush=True)
    except Exception as e:
        print(f"串流處理錯誤: {e}")

# 工具使用串流
def tool_streaming():
    client = Anthropic(api_key="your_api_key")
    
    tools = [{
        "name": "get_weather",
        "description": "取得天氣資訊",
        "input_schema": {
            "type": "object",
            "properties": {
                "location": {"type": "string", "description": "地點"}
            },
            "required": ["location"]
        }
    }]
    
    with client.messages.stream(
        model="claude-3-sonnet-20240229",
        max_tokens=1024,
        tools=tools,
        messages=[{
            "role": "user",
            "content": "台北今天天氣如何？"
        }]
    ) as stream:
        for event in stream:
            if event.type == "content_block_start":
                if event.content_block.type == "tool_use":
                    print(f"開始使用工具: {event.content_block.name}")
            elif event.type == "content_block_delta":
                if event.delta.type == "input_json_delta":
                    print(f"工具輸入: {event.delta.partial_json}")
                elif event.delta.type == "text_delta":
                    print(event.delta.text, end="", flush=True)

# 非同步串流實作
import asyncio

async def async_streaming():
    client = Anthropic(api_key="your_api_key")
    
    async with client.messages.stream(
        model="claude-3-sonnet-20240229",
        max_tokens=1024,
        messages=[{
            "role": "user",
            "content": "請解釋機器學習的基本概念"
        }]
    ) as stream:
        async for text in stream.text_stream:
            print(text, end="", flush=True)
            await asyncio.sleep(0.01)  # 模擬處理時間

# 執行非同步串流
if __name__ == "__main__":
    asyncio.run(async_streaming())
```

## 最佳實踐

<guidelines>
1. **使用官方SDK**：強烈建議使用 Anthropic 的 Python 或 TypeScript SDK，而非直接處理 SSE 事件

2. **優雅的錯誤處理**：
   - 處理未知事件類型
   - 捕捉網路中斷
   - 實作重試機制

3. **效能優化**：
   - 使用適當的緩衝區大小
   - 避免過度頻繁的 UI 更新
   - 實作背壓控制

4. **用戶體驗**：
   - 提供載入指示器
   - 允許用戶中止串流
   - 顯示進度指示

5. **資源管理**：
   - 正確關閉串流連接
   - 清理事件監聽器
   - 管理記憶體使用

6. **內容處理**：
   - 累積部分 JSON 用於工具使用
   - 正確處理 Unicode 字符
   - 保持內容完整性

7. **監控與除錯**：
   - 記錄串流事件
   - 監控延遲指標
   - 實作健康檢查
</guidelines>

## 常見錯誤

<mistakes>
1. **錯誤：直接解析 SSE 事件而不使用 SDK**
   ```python
   # 錯誤方式
   response = requests.get(url, stream=True)
   for line in response.iter_lines():
       # 手動解析 SSE 格式
   ```
   
   **正確方式**：
   ```python
   # 使用官方 SDK
   with client.messages.stream(...) as stream:
       for text in stream.text_stream:
           print(text, end="")
   ```

2. **錯誤：未正確處理工具使用的部分 JSON**
   ```python
   # 錯誤：直接解析部分 JSON
   json.loads(event.delta.partial_json)  # 可能失敗
   ```
   
   **正確方式**：
   ```python
   # 累積完整 JSON 後再解析
   json_buffer += event.delta.partial_json
   if event.type == "content_block_stop":
       tool_input = json.loads(json_buffer)
   ```

3. **錯誤：未實作錯誤處理**
   ```python
   # 錯誤：假設串流永遠成功
   for event in stream:
       process_event(event)
   ```
   
   **正確方式**：
   ```python
   try:
       for event in stream:
           if event.type == "error":
               handle_error(event.error)
               break
           process_event(event)
   except Exception as e:
       handle_exception(e)
   ```

4. **錯誤：過度頻繁的 UI 更新**
   ```python
   # 錯誤：每個字符都更新 UI
   for text in stream.text_stream:
       update_ui(text)  # 可能造成效能問題
   ```
   
   **正確方式**：
   ```python
   # 批次更新或節流
   buffer = ""
   for text in stream.text_stream:
       buffer += text
       if len(buffer) > 50:  # 批次更新
           update_ui(buffer)
           buffer = ""
   ```
</mistakes>

## 測試與優化

<evaluation>
**串流效能測試指標**：
1. **首字節時間(TTFB)**：從請求到第一個字符的時間
2. **串流穩定性**：連接中斷率和重連成功率
3. **延遲分佈**：各事件間隔時間統計
4. **錯誤率**：各類型錯誤的發生頻率

**測試方法**：
```python
import time
import statistics

def test_streaming_performance():
    client = Anthropic(api_key="your_api_key")
    
    # 測試多次請求
    latencies = []
    for i in range(10):
        start_time = time.time()
        first_token_time = None
        
        with client.messages.stream(
            model="claude-3-sonnet-20240229",
            max_tokens=100,
            messages=[{"role": "user", "content": f"測試請求 {i}"}]
        ) as stream:
            for text in stream.text_stream:
                if first_token_time is None:
                    first_token_time = time.time()
                    ttfb = first_token_time - start_time
                    latencies.append(ttfb)
                break
    
    print(f"平均 TTFB: {statistics.mean(latencies):.2f}s")
    print(f"TTFB 標準差: {statistics.stdev(latencies):.2f}s")
```
</evaluation>

<iteration>
**迭代改進方法**：

1. **效能優化循環**：
   - 基準測試 → 識別瓶頸 → 優化 → 重測
   - 監控關鍵指標趨勢
   - A/B 測試不同配置

2. **用戶反饋整合**：
   - 收集用戶對回應速度的感受
   - 分析用戶中止請求的模式
   - 優化基於使用情境的配置

3. **技術債務管理**：
   - 定期更新 SDK 版本
   - 重構事件處理邏輯
   - 優化錯誤處理流程

4. **容量規劃**：
   - 監控同時串流連接數
   - 預測資源需求
   - 實作自動擴縮容
</iteration>

## 與其他技術組合

<combinations>
1. **串流 + 延伸思考**：
   ```xml
   <combination name="streaming_thinking">
     <request>
       <stream>true</stream>
       <thinking>true</thinking>
     </request>
     <benefits>即時查看Claude的思考過程，提升透明度</benefits>
   </combination>
   ```

2. **串流 + 工具使用**：
   ```xml
   <combination name="streaming_tools">
     <request>
       <stream>true</stream>
       <tools>多個工具定義</tools>
     </request>
     <benefits>即時查看工具調用過程，改善除錯體驗</benefits>
   </combination>
   ```

3. **串流 + 系統提示**：
   ```xml
   <combination name="streaming_system">
     <system>詳細的角色和行為定義</system>
     <request>
       <stream>true</stream>
       <messages>用戶對話</messages>
     </request>
     <benefits>保持角色一致性的同時提供即時互動</benefits>
   </combination>
   ```

4. **串流 + 預填回應**：
   ```xml
   <combination name="streaming_prefill">
     <request>
       <stream>true</stream>
       <messages>
         <message role="user">請求內容</message>
         <message role="assistant">預填開頭</message>
       </messages>
     </request>
     <benefits>控制回應格式的同時保持串流體驗</benefits>
   </combination>
   ```

5. **串流 + 輸出格式控制**：
   ```xml
   <combination name="streaming_format">
     <request>
       <stream>true</stream>
       <messages>
         <message role="user">
           <content>
             請以JSON格式回應，包含title和content字段
           </content>
         </message>
       </messages>
     </request>
     <benefits>結構化輸出的即時串流顯示</benefits>
   </combination>
   ```
</combinations>

## 官方參考連結

- [Anthropic 串流訊息官方文檔](https://docs.anthropic.com/en/docs/build-with-claude/streaming)
- [Anthropic API 串流規範](https://docs.anthropic.com/en/api/streaming)
- [Anthropic Python SDK 文檔](https://github.com/anthropics/anthropic-sdk-python)
- [Anthropic TypeScript SDK 文檔](https://github.com/anthropics/anthropic-sdk-typescript)
- [Claude API 入門指南](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)