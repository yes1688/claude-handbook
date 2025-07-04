# 16. 長上下文技巧(Long Context Tips)

## 官方定義與說明

<definition>
長上下文技巧是專門為 Claude 的大型上下文窗口（200K tokens）設計的提示工程技術。根據 Anthropic 官方文檔，這些技巧能夠顯著提升 Claude 在處理長文檔（約 20K+ tokens 或 500 頁內容）時的性能，通過文檔結構化、策略性內容放置、以及特定的提示技術來優化信息檢索和分析能力。
</definition>

## 核心原理

<principle>
長上下文技巧的核心原理基於以下四個關鍵要素：

1. **文檔位置策略**：將長文檔放置在提示的頂部，查詢和指令放在底部，利用位置效應提升性能
2. **結構化組織**：使用 XML 標籤系統化組織多個文檔，包含元數據和內容分隔
3. **引用驅動方法**：要求 Claude 首先提取相關引用，然後基於這些引用進行分析
4. **注意力引導**：通過特定提示技術引導 Claude 關注文檔中的關鍵信息

這些原理共同作用，幫助 Claude 在大型上下文中"切穿噪音"，準確定位和處理相關信息。
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔證實的效益包括：

1. **性能提升**：將查詢放在文檔末尾可提升回應質量達 30%
2. **準確性改善**：使用引用技術可將準確率從 27% 提升到 98%
3. **錯誤減少**：與 Claude 2.0 相比，錯誤答案減少 30%
4. **幻覺降低**：錯誤聲稱文檔支持某個觀點的比率降低 3-4 倍
5. **複雜任務處理**：能夠處理多文檔、數據豐富的複雜任務
6. **上下文容量**：充分利用 200K token 的上下文窗口（約 500 頁內容）
</benefits>

## 實作格式

### 基本結構
```xml
<documents>
  <document index="1">
    <source>document_name.pdf</source>
    <document_content>
      {{DOCUMENT_CONTENT}}
    </document_content>
  </document>
</documents>

請先找出與問題相關的引用，然後回答問題。

問題：{{QUESTION}}
```

### 完整結構
```xml
<documents>
  <document index="1">
    <source>{{SOURCE_1}}</source>
    <document_type>{{TYPE_1}}</document_type>
    <document_date>{{DATE_1}}</document_date>
    <document_content>
      {{CONTENT_1}}
    </document_content>
  </document>
  
  <document index="2">
    <source>{{SOURCE_2}}</source>
    <document_type>{{TYPE_2}}</document_type>
    <document_date>{{DATE_2}}</document_date>
    <document_content>
      {{CONTENT_2}}
    </document_content>
  </document>
</documents>

<instructions>
請仔細閱讀以上文檔，稍後將被問及相關問題。

在回答問題之前，請先：
1. 找出文檔中與問題最相關的句子或段落
2. 提供這些相關引用的完整內容
3. 基於這些引用來回答問題
</instructions>

<question>
{{SPECIFIC_QUESTION}}
</question>
```

## 實作範例

### 範例1：財務報告分析
```xml
<documents>
  <document index="1">
    <source>annual_report_2023.pdf</source>
    <document_type>財務報告</document_type>
    <document_date>2023-12-31</document_date>
    <document_content>
      公司在2023年第三季度實現營收增長15%，主要得益於新產品線的成功推出。
      管理層預計第四季度將面臨市場競爭加劇的挑戰，但對長期增長前景保持樂觀。
      研發投資占營收比例達到12%，創歷史新高。
      現金流量表顯示經營活動現金流為正，投資活動現金流為負主要由於新設備採購。
    </document_content>
  </document>
</documents>

<instructions>
請仔細閱讀財務報告，稍後將被問及相關問題。

在回答前，請先提供最相關的引用內容：
</instructions>

分析公司在2023年的研發投資情況及其對未來的影響。
```

### 範例2：多文檔法律分析
```xml
<documents>
  <document index="1">
    <source>contract_A.pdf</source>
    <document_type>商業合同</document_type>
    <document_date>2023-06-15</document_date>
    <document_content>
      第3條規定：乙方應在收到甲方書面通知後30日內完成產品交付。
      第8條規定：如因不可抗力因素導致延遲，交付期可適當延長。
      第12條規定：違約方需承擔對方因此遭受的直接經濟損失。
    </document_content>
  </document>
  
  <document index="2">
    <source>contract_B.pdf</source>
    <document_type>商業合同</document_type>
    <document_date>2023-08-20</document_date>
    <document_content>
      第2條規定：供應商須在簽約後45日內完成首批產品交付。
      第7條規定：因政府政策變化導致的延遲不視為違約。
      第10條規定：違約金按合同總額的5%計算。
    </document_content>
  </document>
</documents>

<instructions>
請仔細閱讀這兩份合同，稍後將就交付條款進行比較分析。

回答問題前，請先提供相關條款的完整引用：
</instructions>

比較分析兩份合同在交付期限和違約責任方面的差異。
```

### 範例3：研究文獻綜述
```xml
<documents>
  <document index="1">
    <source>machine_learning_survey_2023.pdf</source>
    <document_type>學術論文</document_type>
    <document_date>2023-09-10</document_date>
    <document_content>
      近年來，深度學習在自然語言處理領域取得了顯著突破。
      Transformer架構的出現徹底改變了機器翻譯的性能表現。
      研究顯示，大規模預訓練模型在多項NLP任務中都達到了人類水準。
      然而，這些模型的計算成本和環境影響仍然是重要考慮因素。
      未來研究方向包括模型壓縮、高效微調、以及多模態學習。
    </document_content>
  </document>
</documents>

<instructions>
請仔細閱讀這份機器學習綜述論文，稍後將被問及相關問題。

請先找出文檔中與問題最相關的句子：
</instructions>

總結論文中提到的深度學習在NLP領域的主要成就和未來挑戰。
```

## API實作方式

```python
import anthropic

client = anthropic.Anthropic(api_key="your_api_key")

def long_context_analysis(documents, question):
    """
    使用長上下文技巧進行文檔分析
    """
    # 構建文檔結構
    document_content = "<documents>\n"
    for i, doc in enumerate(documents, 1):
        document_content += f"""  <document index="{i}">
    <source>{doc['source']}</source>
    <document_type>{doc.get('type', 'document')}</document_type>
    <document_date>{doc.get('date', 'N/A')}</document_date>
    <document_content>
      {doc['content']}
    </document_content>
  </document>
  
"""
    document_content += "</documents>\n\n"
    
    # 構建完整提示
    prompt = f"""{document_content}<instructions>
請仔細閱讀以上文檔，稍後將被問及相關問題。

在回答問題之前，請先：
1. 找出文檔中與問題最相關的句子或段落
2. 提供這些相關引用的完整內容
3. 基於這些引用來回答問題

請以以下格式回答：
相關引用：[引用內容]
分析回答：[基於引用的分析]
</instructions>

<question>
{question}
</question>"""
    
    response = client.messages.create(
        model="claude-3-sonnet-20240229",
        max_tokens=4000,
        temperature=0.3,
        messages=[
            {"role": "user", "content": prompt}
        ]
    )
    
    return response.content[0].text

# 使用範例
documents = [
    {
        "source": "annual_report_2023.pdf",
        "type": "財務報告",
        "date": "2023-12-31",
        "content": "公司在2023年第三季度實現營收增長15%，主要得益於新產品線的成功推出..."
    }
]

question = "分析公司2023年的營收增長情況"
result = long_context_analysis(documents, question)
print(result)
```

## 最佳實踐

<guidelines>
1. **文檔放置策略**
   - 將長文檔（20K+ tokens）放在提示頂部
   - 查詢和指令放在底部
   - 這種結構可提升性能達30%

2. **結構化組織**
   - 使用XML標籤清晰分隔文檔
   - 包含source、type、date等元數據
   - 為每個文檔分配唯一索引

3. **引用優先方法**
   - 總是要求Claude先提取相關引用
   - 基於引用內容進行分析
   - 使用"請先找出最相關的句子"引導

4. **注意力引導**
   - 使用"仔細閱讀文檔，稍後將被問及問題"
   - 明確指出回答格式要求
   - 強調基於文檔內容回答

5. **多文檔處理**
   - 清晰標記每個文檔的來源
   - 使用統一的結構格式
   - 在問題中明確指出需要比較的文檔

6. **上下文優化**
   - 充分利用200K token的容量
   - 避免在單個文檔中放置無關信息
   - 保持文檔內容的相關性
</guidelines>

## 常見錯誤

<mistakes>
1. **位置錯誤**
   - 錯誤：將查詢放在文檔前面
   - 正確：將查詢放在文檔後面
   - 影響：性能下降30%

2. **結構混亂**
   - 錯誤：不使用XML標籤組織文檔
   - 正確：使用清晰的XML結構
   - 影響：Claude難以區分不同文檔

3. **缺少引用要求**
   - 錯誤：直接要求回答問題
   - 正確：要求先提供相關引用
   - 影響：增加幻覺風險

4. **元數據缺失**
   - 錯誤：只提供文檔內容
   - 正確：包含來源、類型、日期等信息
   - 影響：缺乏上下文理解

5. **指令模糊**
   - 錯誤：指令不明確或過於簡單
   - 正確：提供詳細的處理步驟
   - 影響：回答質量下降

6. **忽略模型特性**
   - 錯誤：不考慮Claude的處理特點
   - 正確：使用"這是最相關的句子"等引導語
   - 影響：錯過98%準確率的機會
</mistakes>

## 測試與優化

<evaluation>
1. **準確性測試**
   - 使用已知答案的文檔進行測試
   - 檢查引用的正確性和完整性
   - 評估回答與文檔內容的一致性

2. **效率測試**
   - 測試不同文檔大小的處理時間
   - 評估多文檔處理的性能
   - 比較不同結構的效果

3. **魯棒性測試**
   - 測試包含噪音信息的文檔
   - 評估對不完整信息的處理
   - 檢查對矛盾信息的處理能力

4. **對比測試**
   - 比較使用和不使用長上下文技巧的效果
   - 測試不同提示格式的性能差異
   - 評估引用技術的實際效果
</evaluation>

<iteration>
1. **性能迭代**
   - 根據測試結果調整文檔結構
   - 優化引用要求的措辭
   - 改進元數據的組織方式

2. **格式迭代**
   - 測試不同的XML標籤結構
   - 優化文檔索引系統
   - 改善指令的清晰度

3. **策略迭代**
   - 調整文檔放置策略
   - 優化查詢的位置和措辭
   - 改進多文檔處理流程

4. **品質迭代**
   - 持續監控回答質量
   - 收集使用者反饋
   - 根據實際使用情況調整技術
</iteration>

## 與其他技術組合

<combinations>
1. **與XML標籤結合**
   - 使用XML標籤進一步結構化文檔內容
   - 標記重要段落和關鍵信息
   - 提供更精細的內容組織

2. **與思維鏈結合**
   - 在引用提取後加入推理步驟
   - 要求Claude解釋引用的相關性
   - 提供更深入的分析過程

3. **與多樣本提示結合**
   - 提供相同文檔的多個問答範例
   - 使用上下文相關的範例
   - 提升特定任務的性能

4. **與輸出格式控制結合**
   - 指定引用和分析的格式
   - 使用結構化的回答模板
   - 確保輸出的一致性

5. **與工具使用結合**
   - 結合文檔搜索工具
   - 使用文檔解析工具
   - 整合外部知識庫

6. **與角色提示結合**
   - 設定專業分析師角色
   - 指定特定領域的專業知識
   - 提升分析的專業性
</combinations>

## 官方參考連結

- [Long context prompting tips - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/long-context-tips)
- [Prompt engineering for Claude's long context window - Anthropic](https://www.anthropic.com/news/prompting-long-context)
- [Long context prompting for Claude 2.1 - Anthropic](https://www.anthropic.com/news/claude-2-1-prompting)
- [Context windows - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/context-windows)