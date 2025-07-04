# 1. 系統提示詞與角色提示(XML範例)

## 一、什麼是系統提示詞與角色提示

**系統提示詞與角色提示**是使用 `<role>` 標籤給 Claude 指定專業身份的技術，被認為是**最強大的提示工程技術**。

**核心原理**：將 Claude 從通用助手轉變為特定領域的專家。

## 二、為什麼重要

### 三大效益
1. **提升準確性**：在複雜場景中顯著提升表現
2. **客製化語調**：調整溝通風格匹配需求
3. **改善專注度**：專注於特定任務需求

## 三、基本 XML 格式

### 最簡結構
```xml
<role>
你是一位 [專業身份]，具備 [關鍵能力]
</role>

<task>
[具體任務描述]
</task>
```

### 完整結構
```xml
<role>
專業角色定義
</role>

<task>
具體任務指示
</task>

<context>
背景資訊
</context>

<format>
輸出格式要求
</format>
```

## 四、實作範例

### 範例 1：簡單角色設定

```xml
<role>
你是一位資深軟體工程師，專精於 Python 開發
</role>

<task>
請幫我審查這段程式碼並提出改進建議
</task>
```

### 範例 2：詳細角色設定

```xml
<role>
你是一位擁有10年經驗的數據科學家，專長包括：
- 統計分析和機器學習
- Python 和 R 程式設計
- 商業數據洞察分析
- 視覺化報表製作
</role>

<task>
分析銷售數據並找出影響業績的關鍵因素
</task>

<context>
這是一家零售公司過去兩年的月度銷售數據，包含地區、產品類別、季節等變數
</context>

<format>
請提供：
1. 數據摘要分析
2. 關鍵影響因素
3. 視覺化建議
4. 實際行動建議
</format>
```

### 範例 3：情境化角色

```xml
<role>
你是一位在新創公司工作的產品經理，負責 B2B SaaS 產品。
你習慣用數據驅動決策，關注用戶體驗和商業價值的平衡。
</role>

<task>
設計一個新功能的產品規格
</task>

<requirements>
- 提升用戶留存率
- 增加付費轉換
- 開發成本控制在 3 個月內
</requirements>

<format>
請提供產品需求文件（PRD）大綱
</format>
```

## 五、撰寫要點

### ✅ 正確做法
- **具體化角色**：「資深 React 開發者」而非「程式設計師」
- **列出專長**：明確技能和經驗範圍
- **設定風格**：說明溝通和回應偏好
- **提供背景**：相關的工作或專案情境

### ❌ 避免錯誤
- 角色太模糊：「你是專家」
- 缺乏專業性：沒有具體技能描述
- 過於複雜：一次設定太多角色
- 不切實際：超出 AI 能力範圍的角色

## 六、實用技巧

### 1. 角色漸進式設定
```xml
<!-- 基礎版 -->
<role>你是一位財務分析師</role>

<!-- 進階版 -->
<role>
你是一位擁有 CFA 認證的資深財務分析師，
專精於科技股投資評估和風險管理
</role>
```

### 2. 多重專長組合
```xml
<role>
你是一位技術主管，同時具備：
- 軟體開發背景（10年）
- 團隊管理經驗（5年）
- 產品策略思維
</role>
```

### 3. 特定情境設定
```xml
<role>
你是一位在醫療科技公司工作的資安專家，
特別注重 HIPAA 法規遵循和患者資料保護
</role>
```

## 七、測試與優化

### 評估方法
1. **專業度測試**：回應是否使用正確的專業術語
2. **實用性測試**：建議是否具體可執行
3. **一致性測試**：風格是否符合角色設定

### 迭代改進
- 從簡單角色開始測試
- 根據回應品質調整角色描述
- 逐步增加專業細節
- 記錄有效的角色設定模式

## 八、常用角色模板

### 技術類
```xml
<role>
你是一位資深 [技術領域] 工程師，具備：
- X年實務經驗
- [具體技術棧] 專精
- [特殊專長或認證]
</role>
```

### 商業類
```xml
<role>
你是一位 [職位名稱]，專長於 [業務領域]，
習慣用 [思考方式] 分析問題並提供實際可行的解決方案
</role>
```

### 諮詢類
```xml
<role>
你是一位專業的 [領域] 顧問，
擁有豐富的 [相關經驗]，擅長將複雜概念轉化為易懂的建議
</role>
```

---

**重點提醒**：角色設定要與任務需求匹配，越具體越能產生高品質的專業回應。