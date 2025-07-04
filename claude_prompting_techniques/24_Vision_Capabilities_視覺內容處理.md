# 24. 視覺內容處理(Vision Capabilities)

## 官方定義與說明

<definition>
Claude 3 和 4 系列模型具備先進的視覺理解能力，能夠理解和分析圖像，為多模態互動開啟了令人興奮的可能性。Claude 可以處理 JPEG、PNG、GIF 和 WebP 格式的圖像，並在文本與圖像混合的對話中表現出色。這些視覺能力與其他領先模型相當，可以處理照片、圖表、圖形和技術圖表等多種視覺格式。
</definition>

## 核心原理

<principle>
Claude 的視覺處理基於深度學習的多模態架構，能夠同時理解文本和圖像內容。系統透過以下方式處理視覺資訊：

1. **圖像編碼**：將圖像轉換為 base64 編碼格式或直接上傳處理
2. **視覺理解**：利用訓練好的視覺模型分析圖像內容、結構和語義
3. **多模態融合**：結合文本提示詞與圖像資訊生成回應
4. **內容解釋**：提供圖像描述、數據提取、分析和洞察

Claude 採用 token 計算方式處理圖像，計算公式為：tokens = (width px × height px) / 750
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，Vision Capabilities 提供以下核心效益：

1. **多模態對話**：可在單一對話中處理文本和圖像，支援連續的多輪互動
2. **強大的視覺理解**：精準解讀圖表、圖形、技術圖表和文檔
3. **OCR 能力**：從不完美的圖像中準確轉錄文本內容
4. **批量處理**：支援單次請求處理多張圖像（API 最多 100 張）
5. **企業級應用**：適用於包含 PDF、流程圖、簡報等視覺內容的知識庫
6. **成本效益**：相比專門的 OCR 服務，提供更靈活的處理能力
7. **隱私保護**：上傳的圖像處理後自動刪除，不用於模型訓練
</benefits>

## 實作格式

### 基本結構
```xml
<vision_task>
    <image_input>
        <source_type>base64</source_type>
        <media_type>image/jpeg</media_type>
        <data>[base64_encoded_image_data]</data>
    </image_input>
    <instruction>
        [具體的分析指令]
    </instruction>
</vision_task>
```

### 完整結構
```xml
<vision_analysis>
    <context>
        <task_type>[文檔分析/圖表解讀/OCR提取/視覺問答]</task_type>
        <output_format>[JSON/表格/描述文字]</output_format>
        <focus_areas>[重點關注的圖像區域]</focus_areas>
    </context>
    
    <images>
        <image id="1">
            <source_type>base64</source_type>
            <media_type>image/png</media_type>
            <data>[base64_encoded_image_data]</data>
            <description>[圖像描述]</description>
        </image>
    </images>
    
    <instructions>
        <primary_task>[主要任務描述]</primary_task>
        <specific_requirements>
            - [具體要求1]
            - [具體要求2]
            - [具體要求3]
        </specific_requirements>
        <quality_checks>
            - [品質檢查項目1]
            - [品質檢查項目2]
        </quality_checks>
    </instructions>
</vision_analysis>
```

## 實作範例

### 範例1：發票 OCR 數據提取
```xml
<vision_analysis>
    <context>
        <task_type>OCR提取</task_type>
        <output_format>JSON</output_format>
        <focus_areas>發票關鍵資訊欄位</focus_areas>
    </context>
    
    <images>
        <image id="invoice">
            <source_type>base64</source_type>
            <media_type>image/jpeg</media_type>
            <data>iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNk+P+/HgAFhAJ/wlseKgAAAABJRU5ErkJggg==</data>
            <description>掃描的發票影像</description>
        </image>
    </images>
    
    <instructions>
        <primary_task>從發票圖像中提取結構化資訊</primary_task>
        <specific_requirements>
            - 提取發票號碼、日期、金額、稅額
            - 識別供應商和客戶資訊
            - 提取商品明細項目
            - 輸出為 JSON 格式
        </specific_requirements>
        <quality_checks>
            - 確認數字準確性
            - 檢查日期格式
            - 驗證金額計算
        </quality_checks>
    </instructions>
</vision_analysis>
```

### 範例2：圖表數據分析
```xml
<vision_analysis>
    <context>
        <task_type>圖表解讀</task_type>
        <output_format>分析報告</output_format>
        <focus_areas>趨勢分析和關鍵指標</focus_areas>
    </context>
    
    <images>
        <image id="chart">
            <source_type>base64</source_type>
            <media_type>image/png</media_type>
            <data>iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNk+P+/HgAFhAJ/wlseKgAAAABJRU5ErkJggg==</data>
            <description>銷售趨勢圖表</description>
        </image>
    </images>
    
    <instructions>
        <primary_task>分析圖表中的銷售趨勢和關鍵洞察</primary_task>
        <specific_requirements>
            - 識別並提取所有數據點
            - 分析趨勢模式（上升、下降、穩定）
            - 找出異常值或重要轉折點
            - 提供業務洞察和建議
        </specific_requirements>
        <quality_checks>
            - 確認數據讀取準確性
            - 驗證趨勢分析邏輯
            - 檢查結論的合理性
        </quality_checks>
    </instructions>
</vision_analysis>
```

### 範例3：多圖像比較分析
```xml
<vision_analysis>
    <context>
        <task_type>視覺問答</task_type>
        <output_format>比較分析報告</output_format>
        <focus_areas>產品差異和改進建議</focus_areas>
    </context>
    
    <images>
        <image id="before">
            <source_type>base64</source_type>
            <media_type>image/jpeg</media_type>
            <data>iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNk+P+/HgAFhAJ/wlseKgAAAABJRU5ErkJggg==</data>
            <description>產品改進前</description>
        </image>
        <image id="after">
            <source_type>base64</source_type>
            <media_type>image/jpeg</media_type>
            <data>iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNk+P+/HgAFhAJ/wlseKgAAAABJRU5ErkJggg==</data>
            <description>產品改進後</description>
        </image>
    </images>
    
    <instructions>
        <primary_task>比較兩張產品圖像的差異並提供改進評估</primary_task>
        <specific_requirements>
            - 識別視覺上的主要差異
            - 評估改進效果
            - 分析使用者體驗影響
            - 提供進一步優化建議
        </specific_requirements>
        <quality_checks>
            - 確認差異識別完整性
            - 驗證評估的客觀性
            - 檢查建議的實用性
        </quality_checks>
    </instructions>
</vision_analysis>
```

## API實作方式

```python
import anthropic
import base64
import httpx

# 初始化 Claude 客戶端
client = anthropic.Anthropic(api_key="your_api_key")

# 方法1：從 URL 載入圖像
def load_image_from_url(image_url):
    """從 URL 載入圖像並轉換為 base64"""
    response = httpx.get(image_url)
    image_data = base64.b64encode(response.content).decode("utf-8")
    return image_data

# 方法2：從本地文件載入圖像
def load_image_from_file(file_path):
    """從本地文件載入圖像並轉換為 base64"""
    with open(file_path, "rb") as image_file:
        image_data = base64.b64encode(image_file.read()).decode("utf-8")
    return image_data

# 基本視覺分析
def analyze_image(image_data, prompt, media_type="image/jpeg"):
    """分析單張圖像"""
    message = client.messages.create(
        model="claude-3-5-sonnet-20241022",
        max_tokens=1024,
        messages=[
            {
                "role": "user",
                "content": [
                    {
                        "type": "image",
                        "source": {
                            "type": "base64",
                            "media_type": media_type,
                            "data": image_data
                        }
                    },
                    {
                        "type": "text",
                        "text": prompt
                    }
                ]
            }
        ]
    )
    return message.content[0].text

# 多圖像比較分析
def compare_images(image_data_list, prompt):
    """比較多張圖像"""
    content = []
    
    # 添加所有圖像
    for i, image_data in enumerate(image_data_list):
        content.append({
            "type": "image",
            "source": {
                "type": "base64",
                "media_type": "image/jpeg",
                "data": image_data
            }
        })
    
    # 添加提示文本
    content.append({
        "type": "text",
        "text": prompt
    })
    
    message = client.messages.create(
        model="claude-3-5-sonnet-20241022",
        max_tokens=2048,
        messages=[
            {
                "role": "user",
                "content": content
            }
        ]
    )
    return message.content[0].text

# OCR 文本提取
def extract_text_from_image(image_data):
    """從圖像中提取文本"""
    prompt = """
    請從這張圖像中提取所有可見的文本內容。
    要求：
    1. 保持原始文本的格式和結構
    2. 如果有表格，請保持表格格式
    3. 標注任何不清楚或可能錯誤的文本
    4. 以 JSON 格式輸出結果
    """
    return analyze_image(image_data, prompt)

# 圖表數據分析
def analyze_chart(image_data):
    """分析圖表數據"""
    prompt = """
    請分析這張圖表並提供以下資訊：
    1. 圖表類型（柱狀圖、線圖、餅圖等）
    2. 主要數據點和數值
    3. 趨勢分析
    4. 關鍵洞察
    5. 以結構化格式輸出分析結果
    """
    return analyze_image(image_data, prompt)

# 使用範例
if __name__ == "__main__":
    # 載入圖像
    image_data = load_image_from_file("path/to/your/image.jpg")
    
    # 基本分析
    result = analyze_image(image_data, "請描述這張圖像的內容")
    print("圖像分析結果:", result)
    
    # OCR 提取
    ocr_result = extract_text_from_image(image_data)
    print("OCR 結果:", ocr_result)
    
    # 圖表分析
    chart_result = analyze_chart(image_data)
    print("圖表分析結果:", chart_result)
```

## 最佳實踐

<guidelines>
1. **圖像品質優化**
   - 使用高解析度、清晰的圖像（避免小於 200 像素）
   - 確保圖像方向正確，避免旋轉或顛倒
   - 選擇適當的圖像格式（JPEG、PNG、GIF、WebP）

2. **提示詞策略**
   - 將圖像放在文本之前以獲得最佳效果
   - 提供具體、明確的分析指令
   - 指定期望的輸出格式（JSON、表格、描述等）

3. **性能優化**
   - 合理調整圖像大小以平衡品質和 token 使用
   - 批量處理時考慮 API 限制（最多 100 張圖像）
   - 使用快取機制避免重複處理相同圖像

4. **結構化輸出**
   - 使用 XML 標籤組織複雜的分析任務
   - 定義清晰的輸出格式和資料結構
   - 包含品質檢查和驗證步驟

5. **多模態整合**
   - 結合文本上下文增強圖像理解
   - 在對話中逐步建立圖像分析結果
   - 使用圖像作為後續分析的基礎
</guidelines>

## 常見錯誤

<mistakes>
1. **圖像品質問題**
   - 錯誤：使用模糊、低解析度或旋轉的圖像
   - 解決：確保圖像清晰、高解析度且方向正確

2. **格式支援錯誤**
   - 錯誤：使用不支援的圖像格式或損壞的文件
   - 解決：使用 JPEG、PNG、GIF 或 WebP 格式的完整圖像

3. **提示詞不當**
   - 錯誤：提供模糊或過於寬泛的分析指令
   - 解決：使用具體、明確的指令和期望輸出格式

4. **隱私安全問題**
   - 錯誤：嘗試識別圖像中的特定人物
   - 解決：了解 Claude 不會識別或命名圖像中的人物

5. **token 使用不當**
   - 錯誤：使用過大的圖像導致 token 消耗過多
   - 解決：根據需求調整圖像大小，使用 token 計算公式估算成本

6. **API 限制誤解**
   - 錯誤：嘗試使用 URL 直接引用圖像
   - 解決：使用 base64 編碼或直接上傳圖像文件

7. **輸出期望不實際**
   - 錯誤：期望 Claude 生成、編輯或修改圖像
   - 解決：了解 Claude 只能分析和理解圖像，不能生成視覺內容
</mistakes>

## 測試與優化

<evaluation>
1. **準確性測試**
   - 使用已知答案的圖像測試 OCR 準確性
   - 比較不同解析度圖像的分析結果
   - 驗證圖表數據提取的正確性

2. **效能評估**
   - 測量不同圖像大小的處理時間
   - 評估 token 使用量和成本效益
   - 比較批量處理與單張處理的效率

3. **品質指標**
   - 文本識別準確率
   - 圖表數據提取完整性
   - 分析結果的實用性和相關性

4. **使用者體驗**
   - 回應時間是否滿足需求
   - 輸出格式是否符合期望
   - 錯誤處理是否適當
</evaluation>

<iteration>
1. **基於回饋改進**
   - 收集使用者對分析結果的回饋
   - 根據錯誤模式調整提示詞策略
   - 優化圖像預處理流程

2. **持續優化**
   - 定期更新提示詞範本
   - 測試新的圖像處理技術
   - 監控 API 使用模式並優化

3. **版本控制**
   - 記錄不同版本提示詞的效果
   - 維護成功案例的提示詞庫
   - 建立最佳實踐的知識庫

4. **性能調優**
   - 分析處理時間瓶頸
   - 優化圖像大小與品質平衡
   - 改進批量處理策略
</iteration>

## 與其他技術組合

<combinations>
1. **與 Chain of Thought 結合**
   - 使用思維鏈分析複雜的視覺內容
   - 逐步解析圖像中的多個元素
   - 建立邏輯推理過程

2. **與 Output Formatting 搭配**
   - 定義結構化的視覺分析輸出格式
   - 使用 JSON Schema 規範數據提取結果
   - 創建一致的報告格式

3. **與 Multishot Prompting 整合**
   - 提供多個圖像分析範例
   - 展示不同類型圖像的處理方式
   - 建立標準化的分析流程

4. **與 Tool Use 結合**
   - 整合外部圖像處理工具
   - 結合 OCR 服務進行增強處理
   - 使用資料庫儲存分析結果

5. **與 Context Motivation 搭配**
   - 提供圖像分析的業務背景
   - 解釋分析目的和期望結果
   - 增強分析的針對性和準確性

6. **與 Prefilling 技術結合**
   - 預填充分析結果的開始部分
   - 引導特定的分析方向
   - 確保輸出格式的一致性
</combinations>

## 官方參考連結

- [Anthropic Vision Documentation](https://docs.anthropic.com/en/docs/build-with-claude/vision)
- [Anthropic Cookbook - Vision Examples](https://github.com/anthropics/anthropic-cookbook/tree/main/multimodal)
- [Claude 3.5 Sonnet Vision Capabilities](https://www.anthropic.com/news/claude-3-5-sonnet)
- [Best Practices for Vision](https://github.com/anthropics/anthropic-cookbook/blob/main/multimodal/best_practices_for_vision.ipynb)
- [Getting Started with Vision](https://github.com/anthropics/anthropic-cookbook/blob/main/multimodal/getting_started_with_vision.ipynb)