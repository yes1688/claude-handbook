# Claude Handbook

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Documentation](https://img.shields.io/badge/docs-complete-brightgreen)](./claude_prompting_techniques/)
[![Techniques](https://img.shields.io/badge/techniques-32-blue)](./claude_prompting_techniques/)

> 完整的 Claude AI 工程提示詞技術指南與實用手冊

## 📖 專案簡介

**Claude Handbook** 是一個全面且實用的 Claude AI 工程提示詞技術指南集合。本專案包含 32 個獨立的詳細技術指南文件，每個文件都基於 Anthropic 官方文檔驗證，提供完整的技術說明、實作範例和最佳實踐。

### ✨ 專案特色

- **📚 完整覆蓋**：涵蓋所有重要的 Claude 提示工程技術
- **🔗 官方驗證**：所有內容基於 Anthropic 官方文檔驗證
- **💻 實用導向**：每個技術都包含可直接使用的代碼範例
- **🎯 結構化**：使用標準化的文件結構，易於查找和學習
- **🌟 高品質**：專業格式，無表情符號干擾，適合技術文檔使用

## 🚀 快速開始

### 📁 專案結構

```
claude-handbook/
├── README.md                          # 專案說明（本文件）
├── claude_prompting_techniques/       # 技術指南主目錄
│   ├── README.md                      # 技術指南索引
│   ├── 01_System_Prompts_系統提示詞與角色提示.md
│   ├── 02_Clear_Direct_清晰直接的指示.md
│   ├── ...                           # 其他 30 個技術指南
│   └── 32_Streaming_串流處理.md
└── CLAUDE.md                          # 專案開發記錄
```

### 🎯 使用建議

#### 對於初學者
1. 從[核心基礎技術](./claude_prompting_techniques/#🔥-核心基礎技術必學)開始學習
2. 重點掌握前 5 個最重要的技術
3. 逐步進階到應用技術

#### 對於開發者
1. 直接查看感興趣的[技術分類](./claude_prompting_techniques/#快速查找指南)
2. 複製相關的 API 代碼範例
3. 根據需求組合多種技術

#### 對於企業用戶
1. 重點關注[安全性考量](./claude_prompting_techniques/25_Safety_Considerations_安全性考量.md)
2. 學習[效能優化](./claude_prompting_techniques/#🎯-專業優化技術高階)相關技術
3. 建立標準化的提示詞範本

## 📋 技術目錄

### 🔥 核心基礎技術（必學）
- [**System Prompts**](./claude_prompting_techniques/01_System_Prompts_系統提示詞與角色提示.md) - 定義角色和行為基礎
- [**Clear Direct**](./claude_prompting_techniques/02_Clear_Direct_清晰直接的指示.md) - 清晰指示的根本原則
- [**XML Tags**](./claude_prompting_techniques/05_XML_Tags_XML標籤結構化.md) - 結構化提示的核心工具
- [**Multishot Prompting**](./claude_prompting_techniques/03_Multishot_Prompting_多樣本提示.md) - 範例學習的標準方法
- [**Chain of Thought**](./claude_prompting_techniques/04_Chain_of_Thought_讓Claude思考.md) - 複雜推理的關鍵技術

### 📈 進階應用技術
- [**Claude4 Instruction Following**](./claude_prompting_techniques/06_Claude4_Instruction_Following_精確指令遵循.md) - 精確控制指令遵循
- [**Output Formatting**](./claude_prompting_techniques/09_Output_Formatting_輸出格式控制.md) - 輸出格式精確控制
- [**Prefilling**](./claude_prompting_techniques/10_Prefilling_預填回應技術.md) - 預填回應優化
- [**Tool Use**](./claude_prompting_techniques/12_Parallel_Tool_Use_平行工具執行.md) - 工具使用相關技術

### 🎯 專業優化技術
- [**Long Context**](./claude_prompting_techniques/16_Long_Context_Tips_長上下文技巧.md) - 長上下文處理技巧
- [**Latency Reduction**](./claude_prompting_techniques/29_Reducing_Latency_減少延遲.md) - 效能優化
- [**Safety Considerations**](./claude_prompting_techniques/25_Safety_Considerations_安全性考量.md) - 安全性防護

> 📖 **查看完整技術列表** → [進入技術指南主目錄](./claude_prompting_techniques/)

## 🎯 應用場景

### 🎭 角色扮演應用
適用技術：[System Prompts](./claude_prompting_techniques/01_System_Prompts_系統提示詞與角色提示.md) + [Keep Character](./claude_prompting_techniques/28_Keep_Character_保持角色一致.md)

### 📝 內容創作應用
適用技術：[Clear Direct](./claude_prompting_techniques/02_Clear_Direct_清晰直接的指示.md) + [Multishot Prompting](./claude_prompting_techniques/03_Multishot_Prompting_多樣本提示.md) + [Output Formatting](./claude_prompting_techniques/09_Output_Formatting_輸出格式控制.md)

### 🤔 複雜推理應用
適用技術：[Chain of Thought](./claude_prompting_techniques/04_Chain_of_Thought_讓Claude思考.md) + [Extended Thinking](./claude_prompting_techniques/14_Extended_Thinking_延伸思考提示.md)

### 💻 程式碼生成應用
適用技術：[Code Generation](./claude_prompting_techniques/23_Code_Generation_程式碼生成優化.md) + [Tool Reflection](./claude_prompting_techniques/13_Tool_Reflection_工具反思技術.md)

## 📊 品質保證

每個技術指南文件都包含：

- ✅ **官方驗證**：基於 Anthropic 官方文檔的準確資訊
- ✅ **完整結構**：標準化的 11 個章節結構
- ✅ **實作範例**：至少 3 個不同情境的實用範例
- ✅ **代碼模板**：完整的 Python API 實作範例
- ✅ **最佳實踐**：詳細的指導原則和常見錯誤對比
- ✅ **測試方法**：評估和優化的具體建議
- ✅ **技術組合**：與其他技術的協同使用方式

## 🔗 相關資源

### 官方文檔
- [Anthropic Claude API 文檔](https://docs.anthropic.com/claude/docs)
- [Claude 提示工程指南](https://docs.anthropic.com/claude/docs/prompt-engineering)
- [Claude 最佳實踐](https://docs.anthropic.com/claude/docs/best-practices)

### 社群資源
- [Anthropic Community](https://community.anthropic.com/)
- [Claude AI Discord](https://discord.gg/anthropic)

## 📄 授權聲明

本專案採用 MIT 授權條款，您可以自由使用、修改和分發本專案的內容。

## 🤝 貢獻

歡迎提交 Issue 或 Pull Request 來改進這個專案。如果您發現任何錯誤或有改進建議，請隨時聯繫我們。

## 📞 聯繫方式

如有任何問題或建議，歡迎透過以下方式聯繫：

- 📧 Email: [您的聯繫方式]
- 🐛 Issues: [GitHub Issues 連結]

---

⭐ **如果這個專案對您有幫助，請給我們一個 Star！**

*最後更新：2024年12月*