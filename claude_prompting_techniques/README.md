# Claude Handbook

## 專案概述

**Claude Handbook** 是一個完整的 Claude 工程提示詞技術指南集合，包含 32 個獨立的詳細技術指南文件。每個文件都基於 Anthropic 官方文檔驗證，提供完整的技術說明、實作範例和最佳實踐。

本手冊旨在成為 Claude AI 使用者的完整參考資源，從基礎概念到進階技術，涵蓋所有重要的提示工程技術。無論您是初學者還是資深開發者，都能在這裡找到實用的指導和可直接應用的範例。

## 完成狀態

### 已完成文件

#### 第一批（最重要技術）✅
1. [01_System_Prompts_系統提示詞與角色提示.md](./01_System_Prompts_系統提示詞與角色提示.md)
2. [02_Clear_Direct_清晰直接的指示.md](./02_Clear_Direct_清晰直接的指示.md)
3. [03_Multishot_Prompting_多樣本提示.md](./03_Multishot_Prompting_多樣本提示.md)
4. [04_Chain_of_Thought_讓Claude思考.md](./04_Chain_of_Thought_讓Claude思考.md)
5. [05_XML_Tags_XML標籤結構化.md](./05_XML_Tags_XML標籤結構化.md)

#### 第二批（重要技術）✅
6. [06_Claude4_Instruction_Following_精確指令遵循.md](./06_Claude4_Instruction_Following_精確指令遵循.md)
7. [07_Context_Motivation_提供動機和背景.md](./07_Context_Motivation_提供動機和背景.md)
8. [08_Modifiers_使用修飾詞增強.md](./08_Modifiers_使用修飾詞增強.md)
9. [09_Output_Formatting_輸出格式控制.md](./09_Output_Formatting_輸出格式控制.md)
10. [10_Prefilling_預填回應技術.md](./10_Prefilling_預填回應技術.md)
11. [11_Specificity_具體性勝過模糊性.md](./11_Specificity_具體性勝過模糊性.md)
12. [12_Parallel_Tool_Use_平行工具執行.md](./12_Parallel_Tool_Use_平行工具執行.md)
13. [13_Tool_Reflection_工具反思技術.md](./13_Tool_Reflection_工具反思技術.md)
14. [14_Extended_Thinking_延伸思考提示.md](./14_Extended_Thinking_延伸思考提示.md)
15. [15_Prompt_Templates_提示範本與變數.md](./15_Prompt_Templates_提示範本與變數.md)

#### 第三批（進階技術）✅
16. [16_Long_Context_Tips_長上下文技巧.md](./16_Long_Context_Tips_長上下文技巧.md)
17. [17_Chain_Complex_Prompts_鏈式複雜提示.md](./17_Chain_Complex_Prompts_鏈式複雜提示.md)
18. [18_Example_Alignment_範例對齊原則.md](./18_Example_Alignment_範例對齊原則.md)
19. [19_Negative_Prompting_負面提示技術.md](./19_Negative_Prompting_負面提示技術.md)
20. [20_Success_Criteria_成功標準定義.md](./20_Success_Criteria_成功標準定義.md)
21. [21_Test_Cases_測試案例開發.md](./21_Test_Cases_測試案例開發.md)
22. [22_Iterative_Improvement_迭代改進.md](./22_Iterative_Improvement_迭代改進.md)
23. [23_Code_Generation_程式碼生成優化.md](./23_Code_Generation_程式碼生成優化.md)
24. [24_Vision_Capabilities_視覺內容處理.md](./24_Vision_Capabilities_視覺內容處理.md)
25. [25_Safety_Considerations_安全性考量.md](./25_Safety_Considerations_安全性考量.md)

#### 第四批（專業技術）✅
26. [26_Output_Consistency_輸出一致性.md](./26_Output_Consistency_輸出一致性.md)
27. [27_Reduce_Hallucinations_減少幻覺.md](./27_Reduce_Hallucinations_減少幻覺.md)
28. [28_Keep_Character_保持角色一致.md](./28_Keep_Character_保持角色一致.md)
29. [29_Reducing_Latency_減少延遲.md](./29_Reducing_Latency_減少延遲.md)
30. [30_Mitigate_Jailbreaks_防範越獄.md](./30_Mitigate_Jailbreaks_防範越獄.md)
31. [31_JSON_Output_Control_JSON輸出控制.md](./31_JSON_Output_Control_JSON輸出控制.md)
32. [32_Streaming_串流處理.md](./32_Streaming_串流處理.md)

## 技術重要性排序

### 🔥 核心基礎技術（必學）
1. **System Prompts**：定義角色和行為基礎
2. **Clear Direct**：清晰指示的根本原則
3. **XML Tags**：結構化提示的核心工具
4. **Multishot Prompting**：範例學習的標準方法
5. **Chain of Thought**：複雜推理的關鍵技術

### 📈 進階應用技術（重要）
- **Claude4 Instruction Following**：精確控制指令遵循
- **Output Formatting**：輸出格式精確控制
- **Prefilling**：預填回應優化
- **Tool Use**：工具使用相關技術

### 🎯 專業優化技術（高階）
- **Long Context**：長上下文處理技巧
- **Latency Reduction**：效能優化
- **Safety Considerations**：安全性防護

## 快速查找指南

### 按應用場景分類

#### 🎭 角色扮演場景
- [系統提示詞與角色提示](./01_System_Prompts_系統提示詞與角色提示.md)
- [保持角色一致](./28_Keep_Character_保持角色一致.md)

#### 📝 內容創作場景
- [清晰直接的指示](./02_Clear_Direct_清晰直接的指示.md)
- [多樣本提示](./03_Multishot_Prompting_多樣本提示.md)
- [輸出格式控制](./09_Output_Formatting_輸出格式控制.md)

#### 🤔 複雜推理場景
- [讓Claude思考](./04_Chain_of_Thought_讓Claude思考.md)
- [延伸思考提示](./14_Extended_Thinking_延伸思考提示.md)
- [鏈式複雜提示](./17_Chain_Complex_Prompts_鏈式複雜提示.md)

#### 💻 程式碼生成場景
- [程式碼生成優化](./23_Code_Generation_程式碼生成優化.md)
- [工具反思技術](./13_Tool_Reflection_工具反思技術.md)

#### 🔧 工具使用場景
- [平行工具執行](./12_Parallel_Tool_Use_平行工具執行.md)
- [工具反思技術](./13_Tool_Reflection_工具反思技術.md)

## 技術組合建議

### 基礎組合
```xml
System Prompts + Clear Direct + XML Tags
```

### 進階組合
```xml
System Prompts + Multishot Prompting + Chain of Thought + XML Tags
```

### 專業組合
```xml
System Prompts + Extended Thinking + Tool Reflection + Output Formatting
```

## 文件結構說明

每個技術指南文件都包含以下標準結構：

- **官方定義與說明**：基於 Anthropic 官方文檔的技術定義
- **核心原理**：技術工作原理的深入解析
- **官方效益**：Anthropic 官方文檔中說明的效益
- **實作格式**：基本和完整的結構範例
- **實作範例**：3-5 個不同情境的實用範例
- **API實作方式**：Python 代碼實作範例
- **最佳實踐**：實作指導原則
- **常見錯誤**：常見錯誤與避免方法
- **測試與優化**：評估和迭代改進方法
- **與其他技術組合**：技術組合應用建議
- **官方參考連結**：Anthropic 官方文檔連結

## 品質保證

所有文件都經過以下品質檢查：

- ✅ 基於官方文檔驗證
- ✅ 包含完整標準結構
- ✅ 至少 3 個實作範例
- ✅ API 代碼範例正確
- ✅ XML 格式規範
- ✅ 無表情符號，保持專業格式
- ✅ 官方參考連結有效
- ✅ 提供可直接使用的模板

## 使用建議

### 新手學習路徑
1. 從核心基礎技術開始（文件 1-5）
2. 選擇適合自己應用場景的技術
3. 實際練習各種技術組合

### 進階使用者路徑
1. 重點關注專業優化技術
2. 學習複雜的技術組合應用
3. 根據具體需求選擇特定技術

### 開發者路徑
1. 重點學習 API 實作部分
2. 關注工具使用相關技術
3. 學習效能優化技術

## 官方參考

所有技術內容都基於 Anthropic 官方文檔：
- [Anthropic Prompt Engineering](https://docs.anthropic.com/claude/docs/prompt-engineering)
- [Claude API Documentation](https://docs.anthropic.com/claude/reference)
- [Best Practices Guide](https://docs.anthropic.com/claude/docs/best-practices)

---

**製作說明**：本指南集合使用 Claude Code 工具製作，每個技術文件都由獨立的 Claude Sonnet 4 實例基於官方文檔驗證製作，確保內容準確性和實用性。