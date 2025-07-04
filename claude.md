# Claude 工程提示詞技術獨立文件製作任務

## 任務概述

為「Claude 工程提示詞遵循技術完整核心要點列表.md」中的每項技術，製作獨立的詳細指南文件。每項技術都需要搜尋官方文檔驗證，並製作成類似「XML 標籤系統提示詞與角色提示完整指南.md」品質的獨立文件。

## 文件命名規範

每個獨立文件使用以下命名格式：
```
{序號}_{技術英文名稱}_{中文名稱}.md
```

例如：
- `01_System_Prompts_系統提示詞與角色提示.md`
- `02_Clear_Direct_清晰直接的指示.md`
- `03_Multishot_Prompting_多樣本提示.md`

## 需要製作的32個獨立文件

<target_files>
1. 01_System_Prompts_系統提示詞與角色提示.md
2. 02_Clear_Direct_清晰直接的指示.md
3. 03_Multishot_Prompting_多樣本提示.md
4. 04_Chain_of_Thought_讓Claude思考.md
5. 05_XML_Tags_XML標籤結構化.md
6. 06_Claude4_Instruction_Following_精確指令遵循.md
7. 07_Context_Motivation_提供動機和背景.md
8. 08_Modifiers_使用修飾詞增強.md
9. 09_Output_Formatting_輸出格式控制.md
10. 10_Prefilling_預填回應技術.md
11. 11_Specificity_具體性勝過模糊性.md
12. 12_Parallel_Tool_Use_平行工具執行.md
13. 13_Tool_Reflection_工具反思技術.md
14. 14_Extended_Thinking_延伸思考提示.md
15. 15_Prompt_Templates_提示範本與變數.md
16. 16_Long_Context_Tips_長上下文技巧.md
17. 17_Chain_Complex_Prompts_鏈式複雜提示.md
18. 18_Example_Alignment_範例對齊原則.md
19. 19_Negative_Prompting_負面提示技術.md
20. 20_Success_Criteria_成功標準定義.md
21. 21_Test_Cases_測試案例開發.md
22. 22_Iterative_Improvement_迭代改進.md
23. 23_Code_Generation_程式碼生成優化.md
24. 24_Vision_Capabilities_視覺內容處理.md
25. 25_Safety_Considerations_安全性考量.md
26. 26_Output_Consistency_輸出一致性.md
27. 27_Reduce_Hallucinations_減少幻覺.md
28. 28_Keep_Character_保持角色一致.md
29. 29_Reducing_Latency_減少延遲.md
30. 30_Mitigate_Jailbreaks_防範越獄.md
31. 31_JSON_Output_Control_JSON輸出控制.md
32. 32_Streaming_串流處理.md
</target_files>

## 每個獨立文件的標準結構

每個 `.md` 文件必須包含以下標準結構：

```markdown
# [序號]. [技術中文名稱]([技術英文名稱])

## 官方定義與說明

<definition>
基於 Anthropic 官方文檔的技術定義
</definition>

## 核心原理

<principle>
技術的核心工作原理
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔中說明的效益
</benefits>

## 實作格式

### 基本結構
```xml
[基本XML格式範例]
```

### 完整結構
```xml
[完整XML格式範例]
```

## 實作範例

### 範例1：[情境描述]
```xml
[完整實作範例]
```

### 範例2：[情境描述]
```xml
[完整實作範例]
```

### 範例3：[情境描述]
```xml
[完整實作範例]
```

## API實作方式

```python
[Python API 範例]
```

## 最佳實踐

<guidelines>
[實作指導原則]
</guidelines>

## 常見錯誤

<mistakes>
[常見錯誤與避免方法]
</mistakes>

## 測試與優化

<evaluation>
[評估方法]
</evaluation>

<iteration>
[迭代改進方法]
</iteration>

## 與其他技術組合

<combinations>
[可組合使用的技術]
</combinations>

## 官方參考連結

[Anthropic 官方文檔連結]
```

## 執行步驟

### 第一步：建立工作目錄
```bash
mkdir claude_prompting_techniques
cd claude_prompting_techniques
```

### 第二步：逐一製作文件
對每項技術執行以下流程：

1. **搜尋官方文檔**
   ```
   web_search "[技術名稱] anthropic official documentation"
   ```

2. **獲取詳細內容**
   ```
   web_fetch [官方文檔URL]
   ```

3. **驗證技術細節**
   - 確認技術定義的準確性
   - 搜集官方範例
   - 了解最佳實踐建議

4. **製作獨立文件**
   - 按照標準結構創建文件
   - 包含3-5個實用範例
   - 提供錯誤示範對比
   - 添加API實作代碼

5. **品質檢查**
   - 確保所有資訊基於官方文檔
   - 驗證範例的正確性
   - 檢查XML標籤格式

### 第三步：建立索引文件
製作 `README.md` 包含：
- 所有32個技術的目錄
- 技術重要性排序說明
- 快速查找指南
- 技術組合建議

## 品質要求

### 內容品質
- 每個文件必須基於官方文檔驗證
- 至少包含3個不同情境的實作範例
- 提供可直接使用的代碼模板
- 包含正確與錯誤的對比示範

### 格式要求
- 嚴格遵循XML標籤結構
- 程式碼使用適當的語法高亮
- 無表情符號，保持專業格式
- 標題層次清晰

### 實用性要求
- 範例必須實用且可直接套用
- 提供測試驗證方法
- 包含常見問題解決方案
- 適合不同技能水平使用者

## 執行優先級

按照技術重要性排序執行：

<execution_priority>
**第一批（最重要）**：
1-5: 系統提示詞、清晰指示、多樣本提示、思維鏈、XML標籤

**第二批（重要）**：
6-15: Claude4特性、輸出控制、工具使用相關技術

**第三批（進階）**：
16-25: 複雜提示技術、專門應用技術

**第四批（專業）**：
26-32: 效能優化、安全性、特殊格式處理
</execution_priority>

## 最終檢查清單

每個文件完成後檢查：
- [ ] 基於官方文檔驗證
- [ ] 包含完整標準結構
- [ ] 至少3個實作範例
- [ ] API代碼範例正確
- [ ] XML格式規範
- [ ] 無表情符號
- [ ] 官方參考連結有效
- [ ] 可直接使用的模板

## 預期成果

32個獨立的高品質技術指南文件，每個文件都是該技術的完整參考手冊，使用者可以：

1. 單獨學習任一技術
2. 快速查找特定技術的使用方法
3. 獲得可直接使用的模板
4. 了解技術組合應用方式
5. 避免常見錯誤