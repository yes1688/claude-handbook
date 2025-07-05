# 23. 程式碼生成優化(Code Generation)

## 官方定義與說明

<definition>
基於 Anthropic 官方文檔，程式碼生成優化是指利用 Claude 4 的先進程式碼理解能力，透過精確的提示工程技術來生成高品質、可維護且符合最佳實踐的程式碼。Claude Opus 4 和 Sonnet 4 是當前最先進的程式碼生成模型，在複雜程式碼庫理解和 SWE-bench 評測中表現卓越，Sonnet 4 達到了 72.7% 的成績。
</definition>

## 核心原理

<principle>
程式碼生成優化的核心原理包括：
1. **上下文感知生成**：Claude 能夠理解整個專案結構和程式碼架構
2. **精確指令遵循**：Claude 4 模型經過訓練，能夠更精確地遵循明確的指令
3. **通用性優先**：生成適用於所有有效輸入的解決方案，而非僅針對特定測試案例
4. **最佳實踐整合**：自動應用軟體設計原則和程式設計最佳實踐
5. **平行工具執行**：能夠同時執行多個獨立操作以提高效率
6. **角色專業化**：透過系統提示詞將 Claude 轉化為特定領域的虛擬專家
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔中說明的程式碼生成優化效益：
- 提高開發效率：透過自然語言指令快速生成程式碼
- 程式碼品質提升：生成符合最佳實踐的高品質程式碼
- 全專案理解：維持對整個專案架構的感知能力
- 多語言支援：支援多種程式語言和框架
- 除錯和測試：自動執行和修復測試，進行程式碼檢查
- 版本控制整合：協助 Git 操作，創建提交和拉取請求
- 安全性保障：直接連接 Anthropic API，無中介伺服器
- 即時操作：能夠執行實際的檔案編輯和專案操作
</benefits>

## 實作格式

### 基本結構
```xml
<system_prompt>
你是一位專業的 [程式語言] 開發者，專精於 [特定領域]。
請遵循以下原則：
- 實作適用於所有有效輸入的通用解決方案
- 遵循 [程式語言] 的最佳實踐
- 提供清晰的程式碼註解
- 確保程式碼的可維護性和可擴展性
</system_prompt>

<task>
請幫我實作 [具體功能描述]
</task>

<requirements>
- [需求1]
- [需求2]
- [需求3]
</requirements>
```

### 完整結構
```xml
<system_prompt>
你是一位資深的全端開發者，具備以下專業技能：
- 熟練掌握 [程式語言列表]
- 深度理解軟體設計原則和架構模式
- 精通測試驅動開發和程式碼審查
- 具備豐富的效能優化經驗

請在程式碼生成時遵循以下指導原則：
- 實作高品質、通用的解決方案，適用於所有有效輸入
- 遵循 SOLID 原則和設計模式
- 編寫清晰的程式碼註解和文檔
- 考慮效能、安全性和可維護性
- 包含適當的錯誤處理和邊界情況
</system_prompt>

<context>
專案背景：[專案描述]
技術堆疊：[使用的技術]
架構風格：[架構描述]
</context>

<task>
具體任務：[詳細任務描述]
</task>

<requirements>
功能需求：
- [功能需求1]
- [功能需求2]

非功能需求：
- [效能需求]
- [安全需求]
- [可維護性需求]
</requirements>

<constraints>
- [約束條件1]
- [約束條件2]
</constraints>

<output_format>
請按照以下格式提供輸出：
1. 程式碼實作
2. 測試程式碼
3. 使用說明
4. 設計決策說明
</output_format>
```

## 實作範例

### 範例1：前端 React 元件開發

#### 範例A：基本版（不用 XML）
```
你是一位專業的前端開發者，精通 React、TypeScript 和現代前端開發實踐。

任務：創建一個用戶個人資料卡片元件，包含頭像、姓名、職位、社交媒體連結和聯絡資訊。

要求：
- 響應式設計
- 支援深淺色主題
- 包含編輯功能
- 社交媒體連結驗證
- 無障礙功能支援

請提供：
1. TypeScript React 元件程式碼
2. CSS-in-JS 樣式
3. 使用範例
4. 基本測試程式碼

不要保留任何功能，全力以赴創建豐富的互動式設計。包含盡可能多的相關功能和互動，使用現代 React 最佳實踐。
```

#### 範例B：進階版（使用 XML）
```xml
<system_prompt>
你是一位專業的前端開發者，精通 React、TypeScript 和現代前端開發實踐。
不要保留任何功能！全力以赴創建豐富的互動式設計。
請遵循以下原則：
- 包含盡可能多的相關功能和互動
- 添加細緻的懸停狀態、過渡效果和微互動
- 應用設計原則：層次結構、對比、平衡和動態
- 使用現代 React 最佳實踐
</system_prompt>

<task>
創建一個用戶個人資料卡片元件，包含頭像、姓名、職位、社交媒體連結和聯絡資訊。
</task>

<requirements>
- 響應式設計
- 支援深淺色主題
- 包含編輯功能
- 社交媒體連結驗證
- 無障礙功能支援
</requirements>

<output_format>
請提供：
1. TypeScript React 元件
2. CSS-in-JS 樣式
3. 使用範例
4. 測試程式碼
</output_format>
```

### 範例2：後端 API 開發

#### 範例A：基本版（不用 XML）
```
你是一位資深的後端開發者，專精於 Node.js、Express 和資料庫設計。

任務：設計並實作一個用戶認證系統的 RESTful API，包含註冊、登入、密碼重設和權限管理功能。

技術要求：
- JWT 權杖認證
- 密碼加密和鹽化
- 電子郵件驗證
- 角色和權限管理
- 請求速率限制
- 完整的 API 文檔

技術約束：
- 使用 MongoDB 作為資料庫
- 支援橫向擴展
- 符合 GDPR 資料保護要求

請提供一個健壯、可擴展的解決方案，適用於所有有效輸入場景。包含完整的錯誤處理和輸入驗證，確保程式碼的可維護性和可測試性。

輸出內容：
1. Express 路由和中間件
2. 資料庫模型定義
3. 驗證和授權邏輯
4. API 文檔
5. 單元測試和整合測試
```

#### 範例B：進階版（使用 XML）
```xml
<system_prompt>
你是一位資深的後端開發者，專精於 Node.js、Express 和資料庫設計。
請實作一個健壯、可擴展的解決方案，適用於所有有效輸入場景。
遵循以下原則：
- 理解問題需求並實作正確的演算法
- 應用軟體設計原則和最佳實踐
- 包含完整的錯誤處理和輸入驗證
- 確保程式碼的可維護性和可測試性
</system_prompt>

<task>
設計並實作一個用戶認證系統的 RESTful API，包含註冊、登入、密碼重設和權限管理功能。
</task>

<requirements>
- JWT 權杖認證
- 密碼加密和鹽化
- 電子郵件驗證
- 角色和權限管理
- 請求速率限制
- 完整的 API 文檔
</requirements>

<constraints>
- 使用 MongoDB 作為資料庫
- 支援橫向擴展
- 符合 GDPR 資料保護要求
</constraints>

<output_format>
請提供：
1. Express 路由和中間件
2. 資料庫模型定義
3. 驗證和授權邏輯
4. API 文檔
5. 單元測試和整合測試
</output_format>
```

### 範例3：資料結構和演算法優化

#### 範例A：基本版（不用 XML）
```
你是一位電腦科學專家，精通資料結構、演算法設計和效能優化。

任務：實作一個高效的 LRU (Least Recently Used) 快取系統，支援 get 和 put 操作。

技術要求：
- O(1) 時間複雜度的 get 和 put 操作
- 支援泛型鍵值對
- 執行緒安全
- 容量限制和自動淘汰
- 完整的單元測試覆蓋

實作約束：
- 使用 Python 實作
- 記憶體使用要高效
- 提供使用統計資訊

請實作一個高效、通用的解決方案，能夠處理所有有效輸入範圍。分析時間和空間複雜度，考慮邊界情況和異常處理，提供清晰的演算法說明。

輸出內容：
1. LRU 快取類別實作
2. 複雜度分析
3. 測試程式碼
4. 使用範例和效能基準測試
```

#### 範例B：進階版（使用 XML）
```xml
<system_prompt>
你是一位電腦科學專家，精通資料結構、演算法設計和效能優化。
請實作一個高效、通用的解決方案，能夠處理所有有效輸入範圍。
遵循以下原則：
- 分析時間和空間複雜度
- 考慮邊界情況和異常處理
- 提供清晰的演算法說明
- 實作最優化的解決方案
</system_prompt>

<task>
實作一個高效的 LRU (Least Recently Used) 快取系統，支援 get 和 put 操作。
</task>

<requirements>
- O(1) 時間複雜度的 get 和 put 操作
- 支援泛型鍵值對
- 執行緒安全
- 容量限制和自動淘汰
- 完整的單元測試覆蓋
</requirements>

<constraints>
- 使用 Python 實作
- 記憶體使用要高效
- 提供使用統計資訊
</constraints>

<output_format>
請提供：
1. LRU 快取類別實作
2. 複雜度分析
3. 測試程式碼
4. 使用範例和效能基準測試
</output_format>
```

### 使用時機說明：

#### 🔸 **基本版適用情況：**
- **簡單程式碼生成**：功能需求明確單一，不需要複雜的結構化指導
- **快速原型開發**：需要快速生成程式碼原型，測試想法可行性
- **學習和練習**：初學者學習程式碼生成技術，容易理解和修改
- **單一功能實作**：專注於單一功能或模組的開發
- **個人專案使用**：小型個人專案，不需要嚴格的開發流程

#### 🔸 **進階版適用情況：**
- **企業級開發**：需要嚴格的程式碼品質標準和開發規範
- **複雜系統架構**：包含多個層面的技術要求和約束條件
- **團隊協作開發**：需要標準化的程式碼生成模板和流程
- **生產環境部署**：需要考慮效能、安全性、可維護性等多方面因素
- **專業諮詢服務**：為客戶提供高品質的程式碼解決方案

#### 🔸 **升級判斷標準：**
- **技術要求超過5個不同類型** → 考慮用XML
- **需要專業角色定位** → 考慮用XML
- **包含複雜約束條件** → 考慮用XML
- **需要標準化輸出格式** → 考慮用XML

## API實作方式

```python
import anthropic
import json

class CodeGenerationOptimizer:
    def __init__(self, api_key: str):
        self.client = anthropic.Anthropic(api_key=api_key)
    
    def generate_code(self, 
                     language: str, 
                     task_description: str, 
                     requirements: list = None,
                     constraints: list = None,
                     context: str = None) -> str:
        """
        使用 Claude 4 生成優化的程式碼
        """
        
        # 構建系統提示詞
        system_prompt = f"""
        你是一位專業的 {language} 開發者，精通軟體設計原則和最佳實踐。
        請遵循以下原則：
        - 實作適用於所有有效輸入的通用解決方案
        - 遵循 {language} 的最佳實踐和慣例
        - 提供清晰的程式碼註解和文檔
        - 確保程式碼的可維護性、可擴展性和可測試性
        - 包含適當的錯誤處理和邊界情況
        """
        
        # 構建任務描述
        task_content = f"""
        <task>
        {task_description}
        </task>
        """
        
        # 添加需求
        if requirements:
            requirements_text = "\n".join([f"- {req}" for req in requirements])
            task_content += f"""
        <requirements>
        {requirements_text}
        </requirements>
        """
        
        # 添加約束條件
        if constraints:
            constraints_text = "\n".join([f"- {const}" for const in constraints])
            task_content += f"""
        <constraints>
        {constraints_text}
        </constraints>
        """
        
        # 添加上下文
        if context:
            task_content += f"""
        <context>
        {context}
        </context>
        """
        
        # 添加輸出格式要求
        task_content += """
        <output_format>
        請提供：
        1. 完整的程式碼實作
        2. 程式碼註解和文檔
        3. 使用範例
        4. 測試程式碼
        5. 設計決策說明
        </output_format>
        """
        
        try:
            response = self.client.messages.create(
                model="claude-3-5-sonnet-20241022",  # 使用最新的 Claude 模型
                max_tokens=4000,
                temperature=0.2,  # 較低的溫度以確保程式碼的一致性
                system=system_prompt,
                messages=[
                    {
                        "role": "user",
                        "content": task_content
                    }
                ]
            )
            
            return response.content[0].text
            
        except Exception as e:
            return f"程式碼生成失敗: {str(e)}"
    
    def optimize_existing_code(self, 
                              code: str, 
                              language: str,
                              optimization_goals: list = None) -> str:
        """
        優化現有程式碼
        """
        
        system_prompt = f"""
        你是一位資深的 {language} 程式碼審查專家，專精於程式碼優化和重構。
        請分析提供的程式碼並提供優化建議。
        """
        
        goals_text = ""
        if optimization_goals:
            goals_text = "\n".join([f"- {goal}" for goal in optimization_goals])
        
        task_content = f"""
        <code_to_optimize>
        {code}
        </code_to_optimize>
        
        <optimization_goals>
        {goals_text if goals_text else "- 提高程式碼可讀性\n- 優化效能\n- 增強可維護性\n- 減少程式碼重複"}
        </optimization_goals>
        
        <output_format>
        請提供：
        1. 優化後的程式碼
        2. 優化說明和理由
        3. 效能改進分析
        4. 可能的風險和注意事項
        </output_format>
        """
        
        try:
            response = self.client.messages.create(
                model="claude-3-5-sonnet-20241022",
                max_tokens=4000,
                temperature=0.2,
                system=system_prompt,
                messages=[
                    {
                        "role": "user",
                        "content": task_content
                    }
                ]
            )
            
            return response.content[0].text
            
        except Exception as e:
            return f"程式碼優化失敗: {str(e)}"
    
    def generate_tests(self, 
                      code: str, 
                      language: str,
                      test_framework: str = None) -> str:
        """
        為程式碼生成測試
        """
        
        system_prompt = f"""
        你是一位專業的測試工程師，精通 {language} 的測試框架和最佳實踐。
        請為提供的程式碼編寫全面的測試。
        """
        
        framework_text = f"使用 {test_framework} 測試框架" if test_framework else ""
        
        task_content = f"""
        <code_to_test>
        {code}
        </code_to_test>
        
        <testing_requirements>
        - 提供全面的測試覆蓋
        - 包含正常情況和邊界情況
        - 測試異常處理
        - 提供清晰的測試說明
        {framework_text}
        </testing_requirements>
        
        <output_format>
        請提供：
        1. 完整的測試程式碼
        2. 測試案例說明
        3. 測試覆蓋率分析
        4. 執行指令
        </output_format>
        """
        
        try:
            response = self.client.messages.create(
                model="claude-3-5-sonnet-20241022",
                max_tokens=4000,
                temperature=0.2,
                system=system_prompt,
                messages=[
                    {
                        "role": "user",
                        "content": task_content
                    }
                ]
            )
            
            return response.content[0].text
            
        except Exception as e:
            return f"測試生成失敗: {str(e)}"

# 使用範例
if __name__ == "__main__":
    # 初始化程式碼生成優化器
    optimizer = CodeGenerationOptimizer("your-api-key")
    
    # 生成 Python 快速排序演算法
    result = optimizer.generate_code(
        language="Python",
        task_description="實作快速排序演算法",
        requirements=[
            "支援泛型比較",
            "包含效能分析",
            "提供迭代和遞歸版本"
        ],
        constraints=[
            "平均時間複雜度 O(n log n)",
            "空間複雜度優化"
        ]
    )
    
    print("生成的程式碼:")
    print(result)
```

## 最佳實踐

<guidelines>
程式碼生成優化的實作指導原則：

1. **角色專業化**
   - 使用系統提示詞將 Claude 設定為特定領域的專家
   - 明確指定所需的技能和知識領域
   - 提供相關的背景脈絡和專業標準

2. **明確的指令和鼓勵**
   - 對於前端開發，使用"不要保留任何功能！全力以赴"等鼓勵性語言
   - 明確要求"包含盡可能多的相關功能和互動"
   - 指定設計原則：層次結構、對比、平衡和動態

3. **通用性優先**
   - 強調"實作適用於所有有效輸入的解決方案"
   - 避免硬編碼特定值或僅適用於測試案例的解決方案
   - 要求"理解問題需求並實作正確的演算法"

4. **品質標準**
   - 要求"高品質、通用的解決方案"
   - 強調遵循軟體設計原則和最佳實踐
   - 確保程式碼的健壯性、可維護性和可擴展性

5. **平行工具執行**
   - 使用提示詞："為了最大效率，當需要執行多個獨立操作時，請同時調用所有相關工具，而不是順序執行"
   - 鼓勵同時進行多個任務以提高效率

6. **環境優化**
   - 利用 CLAUDE.md 檔案提供專案特定的指導
   - 在 .claude/commands 資料夾中儲存重複工作流程的提示範本
   - 使用斜線指令選單快速存取常用範本

7. **測試和驗證**
   - 要求生成對應的測試程式碼
   - 包含邊界情況和異常處理測試
   - 提供測試覆蓋率分析

8. **文檔和註解**
   - 要求提供清晰的程式碼註解
   - 包含使用範例和設計決策說明
   - 提供 API 文檔和使用指南
</guidelines>

## 常見錯誤

<mistakes>
程式碼生成優化中的常見錯誤與避免方法：

1. **過度關注測試通過**
   - 錯誤：僅針對特定測試案例編寫解決方案
   - 正確：要求"實作適用於所有有效輸入的通用解決方案"
   - 避免：硬編碼值或僅適用於特定輸入的解決方案

2. **缺乏上下文資訊**
   - 錯誤：提供模糊或不完整的任務描述
   - 正確：提供詳細的專案背景、技術堆疊和架構資訊
   - 避免：假設 Claude 了解隱含的需求或約束

3. **忽視非功能需求**
   - 錯誤：只關注功能實作，忽略效能、安全性、可維護性
   - 正確：明確指定效能要求、安全標準和維護性目標
   - 避免：生成不符合生產環境標準的程式碼

4. **角色設定不當**
   - 錯誤：使用通用的系統提示詞
   - 正確：設定具體的專業角色和技能要求
   - 避免：缺乏領域專業知識的程式碼生成

5. **輸出格式不清楚**
   - 錯誤：沒有指定期望的輸出格式和內容
   - 正確：明確要求程式碼、測試、文檔和使用範例
   - 避免：不完整或格式混亂的輸出

6. **缺乏錯誤處理**
   - 錯誤：生成沒有異常處理的程式碼
   - 正確：要求包含完整的錯誤處理和邊界情況
   - 避免：在生產環境中產生未處理的異常

7. **忽視程式碼風格**
   - 錯誤：沒有指定程式碼風格和命名慣例
   - 正確：明確要求遵循特定的程式碼風格指南
   - 避免：不一致的程式碼風格影響可讀性

8. **臨時檔案清理**
   - 錯誤：生成過程中產生的臨時檔案沒有清理
   - 正確：指示 Claude "清理任何臨時新檔案、腳本或輔助檔案"
   - 避免：專案目錄中殘留不必要的檔案
</mistakes>

## 測試與優化

<evaluation>
程式碼生成優化的評估方法：

1. **功能正確性測試**
   - 驗證生成的程式碼是否正確實作了所需功能
   - 測試所有有效輸入範圍，不僅限於特定測試案例
   - 檢查邊界情況和異常處理

2. **程式碼品質評估**
   - 使用程式碼品質工具（如 SonarQube、CodeClimate）
   - 評估程式碼複雜度、可讀性和可維護性
   - 檢查是否遵循最佳實踐和設計原則

3. **效能基準測試**
   - 測量執行時間和記憶體使用情況
   - 比較不同實作方案的效能
   - 驗證時間和空間複雜度分析

4. **安全性審查**
   - 檢查是否存在常見的安全漏洞
   - 驗證輸入驗證和清理機制
   - 評估權限和存取控制實作

5. **可維護性評估**
   - 評估程式碼的模組化程度
   - 檢查註解和文檔的完整性
   - 測試程式碼的可擴展性

6. **測試覆蓋率分析**
   - 使用程式碼覆蓋率工具測量測試覆蓋率
   - 確保關鍵路徑和邊界情況都有測試
   - 評估測試品質和有效性
</evaluation>

<iteration>
程式碼生成優化的迭代改進方法：

1. **提示詞精煉**
   - 基於初始結果調整系統提示詞
   - 增加更具體的技術要求和約束
   - 優化角色設定和專業領域描述

2. **範例引導改進**
   - 提供高品質的程式碼範例作為參考
   - 使用多樣本提示技術展示期望的輸出格式
   - 包含正面和負面範例對比

3. **漸進式複雜度**
   - 從簡單任務開始，逐步增加複雜度
   - 將複雜任務分解為多個較小的子任務
   - 使用鏈式提示處理複雜的多步驟任務

4. **回饋循環建立**
   - 收集程式碼審查和測試結果的回饋
   - 分析常見錯誤模式並調整提示策略
   - 建立程式碼品質指標的追蹤機制

5. **上下文最佳化**
   - 優化專案上下文資訊的提供方式
   - 使用 CLAUDE.md 檔案標準化專案規範
   - 建立重複使用的提示範本庫

6. **工具整合改進**
   - 評估平行工具執行的效果
   - 優化工具呼叫的順序和組合
   - 建立自動化的程式碼生成流程

7. **持續學習**
   - 定期更新提示策略以適應新的模型能力
   - 學習和採用最新的程式碼生成最佳實踐
   - 建立知識庫記錄成功的提示模式
</iteration>

## 與其他技術組合

<combinations>
程式碼生成優化可與以下技術組合使用：

1. **系統提示詞與角色提示**
   - 設定專業開發者角色
   - 定義技術專長和經驗水準
   - 提供專案特定的背景脈絡

2. **XML 標籤結構化**
   - 使用 `<code>` 標籤包裝程式碼區塊
   - 用 `<requirements>` 標籤明確需求
   - 透過 `<context>` 標籤提供專案背景

3. **思維鏈提示**
   - 要求 Claude 解釋程式碼設計決策
   - 包含演算法分析和複雜度推理
   - 提供問題解決的思考過程

4. **多樣本提示**
   - 提供多個程式碼範例作為參考
   - 展示不同情境下的實作方式
   - 包含正確和錯誤的範例對比

5. **平行工具執行**
   - 同時生成程式碼和測試
   - 平行執行程式碼檢查和文檔生成
   - 協調多個相關的開發任務

6. **輸出格式控制**
   - 指定程式碼格式和風格
   - 控制註解和文檔的結構
   - 定義測試程式碼的組織方式

7. **具體性勝過模糊性**
   - 明確指定程式語言和版本
   - 詳細描述技術需求和約束
   - 提供具體的效能和品質標準

8. **成功標準定義**
   - 明確程式碼品質指標
   - 定義測試通過的標準
   - 設定效能和可維護性目標

9. **測試案例開發**
   - 自動生成對應的測試程式碼
   - 包含單元測試和整合測試
   - 提供測試覆蓋率分析

10. **迭代改進**
    - 基於程式碼審查結果進行優化
    - 持續改進程式碼品質和效能
    - 建立程式碼重構的最佳實踐

組合使用範例：
```xml
<system_prompt>
你是一位資深的 Python 開發者，專精於 Web 開發和 API 設計。
</system_prompt>

<thinking>
讓我分析這個 RESTful API 的設計需求：
1. 需要考慮的核心功能
2. 資料庫設計和關聯性
3. 安全性和權限控制
4. 效能和可擴展性
</thinking>

<code_requirements>
- 使用 FastAPI 框架
- 實作 JWT 認證
- 包含完整的 CRUD 操作
- 提供 OpenAPI 文檔
</code_requirements>

<output_format>
請同時提供：
1. API 路由和處理函數
2. 資料庫模型定義
3. 認證和授權邏輯
4. 完整的測試套件
5. 使用範例和文檔
</output_format>
```
</combinations>

## 官方參考連結

- [Claude 4 Best Practices - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Claude Code Overview - Anthropic](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Claude Code Best Practices - Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices)
- [System Prompts - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/system-prompts)
- [Prompt Engineering Overview - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Claude API Documentation - Anthropic](https://docs.anthropic.com/en/api/getting-started)
- [Anthropic Academy - Build with Claude](https://www.anthropic.com/learn/build-with-claude)