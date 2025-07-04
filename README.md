# Claude Handbook

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Documentation](https://img.shields.io/badge/docs-complete-brightgreen)](./claude_prompting_techniques/)
[![Techniques](https://img.shields.io/badge/techniques-32-blue)](./claude_prompting_techniques/)
[![Status](https://img.shields.io/badge/status-archived-orange)](#-專案狀態--project-status)
[![Fork](https://img.shields.io/badge/welcome-fork-success)](#-歡迎-fork--welcome-to-fork)

> 完整的 Claude AI 工程提示詞技術指南與實用手冊

## ⚠️ 專案狀態 | Project Status
🏁 **本專案已完成開發，現進入 Archive 模式。歡迎 Fork 接續維護！**  
🏁 **This project is complete and now in Archive mode. Welcome to Fork and continue!**

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

## ⚠️ 重要聲明與免責條款

### 📋 內容性質說明
- **僅供參考**：本手冊的所有內容僅供學習和參考使用，不構成官方指導
- **官方文檔優先**：如有疑問，請以 [Anthropic 官方文檔](https://docs.anthropic.com/claude/docs) 為準
- **AI 生成內容**：本專案部分內容由 AI 協助生成，可能存在幻覺或錯誤
- **謹慎判斷**：使用者應謹慎判斷內容的準確性，並在實際應用前進行驗證

### 🔍 使用建議
1. **驗證為先**：所有技術建議在生產環境使用前請先測試驗證
2. **持續更新**：Claude API 和最佳實踐可能會變化，請關注官方更新
3. **適當調整**：根據您的具體需求調整和優化提示技術
4. **安全考量**：特別注意安全性相關的技術實作

---

## 🌐 分享與推廣 | Sharing & Promotion

### 中文社群 | Chinese Community
歡迎在以下平台分享本專案：
- 🐦 **微博**：分享時請註明來源和免責聲明
- 💬 **微信群/QQ群**：技術交流群組
- 📖 **知乎/CSDN**：技術文章平台
- 🔄 **掘金/思否**：開發者社群

### English Community
Feel free to share this project on:
- 🐦 **Twitter/X**: #ClaudeAI #PromptEngineering #AnthropicClaude
- 💬 **Discord**: Anthropic Community, AI Development servers
- 📱 **Reddit**: r/ClaudeAI, r/PromptEngineering, r/MachineLearning
- 💼 **LinkedIn**: Professional AI/ML communities

### 📤 分享模板 | Sharing Template

**中文版本：**
```
🚀 發現一個超詳細的 Claude 提示工程指南！

📚 Claude Handbook - 包含32個完整技術指南
✅ 基於官方文檔驗證
💻 附完整代碼範例
🎯 涵蓋從基礎到進階所有技術

⚠️ 注意：內容僅供參考，請以官方文檔為準，AI生成內容可能有誤，請謹慎判斷

GitHub: [您的項目連結]
#ClaudeAI #提示工程 #人工智能
```

**English Version:**
```
🚀 Found an incredibly detailed Claude Prompt Engineering Guide!

📚 Claude Handbook - 32 comprehensive technique guides
✅ Verified against official documentation  
💻 Complete code examples included
🎯 Covers everything from basics to advanced techniques

⚠️ Disclaimer: Content for reference only. Please refer to official docs. AI-generated content may contain errors - use with caution.

GitHub: [Your project link]
#ClaudeAI #PromptEngineering #AI #Anthropic
```

---

## 📦 專案狀態 | Project Status

### 🎯 **專案已完成 | Project Completed**

本專案為 **完整版技術指南**，包含 32 個 Claude 提示工程技術，**已完成開發並進入 Archive 模式**。

This project is a **complete technical guide** with 32 Claude prompt engineering techniques. **Development is finished and the project is now in Archive mode.**

### 🍴 **歡迎 Fork | Welcome to Fork**

- ✅ **自由 Fork**：歡迎任何人 Fork 本專案進行改進或擴展
- ✅ **自主維護**：Fork 後可以自由修改、添加、改進內容  
- ✅ **無需許可**：不需要通知作者，想改就改
- ✅ **保留版權**：請在 Fork 版本中保留原作者聲明即可

**原作者不再主動維護此專案，但歡迎社群接力！**

**The original author will no longer actively maintain this project, but community continuation is welcome!**

### 🌟 **如何接續專案 | How to Continue the Project**

1. **Fork 本倉庫**
2. **添加新內容或改進現有內容**
3. **建立自己的維護分支**
4. **歡迎在您的版本中說明改進內容**

### 🔄 **社群版本 | Community Versions**

如果您創建了改進版本，歡迎在 Issues 中分享連結，我們會在此列出：

If you create an improved version, feel free to share the link in Issues, and we'll list it here:

- *目前暫無社群版本 | No community versions yet*

---

⭐ **如果這個專案對您有幫助，請給我們一個 Star！**  
⭐ **If this project helps you, please give us a Star!**

## 📜 最終免責聲明 | Final Disclaimer

本專案作者不對使用本手冊所產生的任何直接或間接損失承擔責任。使用者應自行承擔使用風險，並在關鍵應用中諮詢專業意見。

The authors of this project do not assume responsibility for any direct or indirect losses resulting from the use of this handbook. Users should assume their own risks and consult professional advice for critical applications.

*最後更新 | Last Updated：2024年12月 | December 2024*