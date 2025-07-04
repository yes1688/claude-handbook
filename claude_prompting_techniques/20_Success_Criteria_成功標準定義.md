# 20. 成功標準定義(Success Criteria)

## 官方定義與說明

<definition>
成功標準定義是在開始提示工程之前建立清晰、可衡量的性能指標，用於評估 Claude 在特定任務上的表現。根據 Anthropic 官方文檔，成功標準應該是「具體的、可衡量的、可達成的、相關的」(Specific, Measurable, Achievable, Relevant)，並且需要配合實證評估方法來驗證 Claude 的輸出品質。
</definition>

## 核心原理

<principle>
成功標準定義的核心原理是在提示工程開始之前建立客觀的評估框架。它基於以下原則：
1. 先定義成功標準，再進行提示工程
2. 使用量化指標來衡量性能
3. 建立實證評估方法來驗證結果
4. 考慮多維度的評估標準
5. 結合自動化和人工評估方法
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，明確定義成功標準帶來以下效益：
- 提供量化的性能測量方法
- 允許追蹤進展和識別問題
- 支援數據驅動的決策制定
- 確保提示工程的目標明確
- 促進系統性的評估和優化
- 避免過度擬合特定測試案例
- 支援生產環境的持續監控
</benefits>

## 實作格式

### 基本結構
```xml
<success_criteria>
    <dimension>任務保真度</dimension>
    <metric>準確率</metric>
    <target>≥ 85%</target>
    <measurement_method>自動化評分</measurement_method>
</success_criteria>
```

### 完整結構
```xml
<success_criteria>
    <primary_metrics>
        <metric>
            <name>任務保真度</name>
            <measurement>F1 分數</measurement>
            <target>≥ 0.85</target>
            <priority>高</priority>
        </metric>
        <metric>
            <name>一致性</name>
            <measurement>相似輸入的回應一致性</measurement>
            <target>≥ 90%</target>
            <priority>高</priority>
        </metric>
    </primary_metrics>
    
    <secondary_metrics>
        <metric>
            <name>回應時間</name>
            <measurement>平均延遲</measurement>
            <target>< 2 秒</target>
            <priority>中</priority>
        </metric>
        <metric>
            <name>成本效益</name>
            <measurement>每次查詢成本</measurement>
            <target>< $0.01</target>
            <priority>低</priority>
        </metric>
    </secondary_metrics>
    
    <evaluation_methods>
        <method>程式碼自動評分</method>
        <method>LLM 基礎評分</method>
        <method>人工評估</method>
    </evaluation_methods>
</success_criteria>
```

## 實作範例

### 範例1：客戶服務分類任務
```xml
<success_criteria>
    <task>客戶服務查詢分類</task>
    
    <primary_metrics>
        <metric>
            <name>分類準確率</name>
            <measurement>正確分類比例</measurement>
            <target>≥ 95%</target>
            <evaluation_method>程式碼自動評分</evaluation_method>
        </metric>
        <metric>
            <name>回應一致性</name>
            <measurement>相似查詢的分類一致性</measurement>
            <target>≥ 90%</target>
            <evaluation_method>餘弦相似度</evaluation_method>
        </metric>
    </primary_metrics>
    
    <secondary_metrics>
        <metric>
            <name>回應語調</name>
            <measurement>專業友善度評分</measurement>
            <target>4.0/5.0</target>
            <evaluation_method>LLM 基礎評分</evaluation_method>
        </metric>
    </secondary_metrics>
    
    <edge_cases>
        <case>模糊查詢處理</case>
        <case>多語言支援</case>
        <case>敏感資訊保護</case>
    </edge_cases>
</success_criteria>
```

### 範例2：文件摘要生成任務
```xml
<success_criteria>
    <task>技術文件摘要生成</task>
    
    <primary_metrics>
        <metric>
            <name>內容相關性</name>
            <measurement>ROUGE-L 分數</measurement>
            <target>≥ 0.7</target>
            <evaluation_method>自動化評分</evaluation_method>
        </metric>
        <metric>
            <name>關鍵資訊保留</name>
            <measurement>重要概念覆蓋率</measurement>
            <target>≥ 85%</target>
            <evaluation_method>關鍵詞匹配</evaluation_method>
        </metric>
    </primary_metrics>
    
    <quality_metrics>
        <metric>
            <name>可讀性</name>
            <measurement>Flesch Reading Ease</measurement>
            <target>60-70</target>
            <evaluation_method>程式碼計算</evaluation_method>
        </metric>
        <metric>
            <name>結構完整性</name>
            <measurement>段落組織評分</measurement>
            <target>≥ 4.0/5.0</target>
            <evaluation_method>LLM 評分</evaluation_method>
        </metric>
    </quality_metrics>
    
    <constraints>
        <constraint>摘要長度：100-300 字</constraint>
        <constraint>處理時間：< 5 秒</constraint>
        <constraint>保留專業術語準確性</constraint>
    </constraints>
</success_criteria>
```

### 範例3：程式碼生成評估
```xml
<success_criteria>
    <task>Python 程式碼生成</task>
    
    <functional_metrics>
        <metric>
            <name>語法正確性</name>
            <measurement>程式碼可執行率</measurement>
            <target>≥ 98%</target>
            <evaluation_method>語法解析器</evaluation_method>
        </metric>
        <metric>
            <name>功能完整性</name>
            <measurement>測試案例通過率</measurement>
            <target>≥ 90%</target>
            <evaluation_method>單元測試</evaluation_method>
        </metric>
    </functional_metrics>
    
    <quality_metrics>
        <metric>
            <name>程式碼品質</name>
            <measurement>PEP 8 遵循度</measurement>
            <target>≥ 95%</target>
            <evaluation_method>靜態分析工具</evaluation_method>
        </metric>
        <metric>
            <name>註解完整性</name>
            <measurement>函數註解覆蓋率</measurement>
            <target>≥ 80%</target>
            <evaluation_method>程式碼解析</evaluation_method>
        </metric>
    </quality_metrics>
    
    <performance_metrics>
        <metric>
            <name>執行效率</name>
            <measurement>時間複雜度評估</measurement>
            <target>符合需求規格</target>
            <evaluation_method>效能測試</evaluation_method>
        </metric>
    </performance_metrics>
</success_criteria>
```

## API實作方式

```python
import anthropic
from typing import Dict, List, Any
import json

class SuccessCriteriaEvaluator:
    def __init__(self, client: anthropic.Anthropic):
        self.client = client
        self.criteria = {}
        self.evaluation_results = []
    
    def define_criteria(self, criteria_config: Dict[str, Any]):
        """定義成功標準"""
        self.criteria = criteria_config
        return self
    
    def create_evaluation_prompt(self, task: str, criteria: Dict[str, Any]) -> str:
        """創建評估提示詞"""
        prompt = f"""<success_criteria>
        <task>{task}</task>
        
        <evaluation_dimensions>
        {self._format_criteria(criteria)}
        </evaluation_dimensions>
        
        <evaluation_instructions>
        請根據以下標準評估回應品質：
        1. 為每個維度提供 1-5 分的評分
        2. 提供具體的評分理由
        3. 指出需要改進的地方
        4. 總結整體表現
        </evaluation_instructions>
        </success_criteria>
        
        待評估的回應：
        {"{response}"}
        
        請提供結構化的評估結果："""
        
        return prompt
    
    def _format_criteria(self, criteria: Dict[str, Any]) -> str:
        """格式化評估標準"""
        formatted = []
        for metric_name, metric_config in criteria.items():
            formatted.append(f"""
            <metric>
                <name>{metric_name}</name>
                <target>{metric_config.get('target', 'N/A')}</target>
                <measurement>{metric_config.get('measurement', 'N/A')}</measurement>
                <priority>{metric_config.get('priority', 'medium')}</priority>
            </metric>""")
        return "\n".join(formatted)
    
    def evaluate_response(self, response: str, reference: str = None) -> Dict[str, Any]:
        """評估回應品質"""
        evaluation_prompt = self.create_evaluation_prompt(
            "Response Evaluation", 
            self.criteria
        ).format(response=response)
        
        if reference:
            evaluation_prompt += f"\n\n參考答案：{reference}"
        
        message = self.client.messages.create(
            model="claude-3-5-sonnet-20241022",
            max_tokens=1000,
            messages=[
                {"role": "user", "content": evaluation_prompt}
            ]
        )
        
        return {
            "evaluation": message.content[0].text,
            "timestamp": str(datetime.now()),
            "criteria_used": self.criteria
        }
    
    def batch_evaluate(self, test_cases: List[Dict[str, str]]) -> List[Dict[str, Any]]:
        """批次評估多個測試案例"""
        results = []
        
        for i, test_case in enumerate(test_cases):
            result = self.evaluate_response(
                test_case["response"],
                test_case.get("reference")
            )
            result["test_case_id"] = i
            result["input"] = test_case.get("input", "")
            results.append(result)
        
        return results
    
    def generate_performance_report(self, results: List[Dict[str, Any]]) -> str:
        """生成性能報告"""
        report_prompt = f"""<performance_analysis>
        <evaluation_results>
        {json.dumps(results, indent=2, ensure_ascii=False)}
        </evaluation_results>
        
        <analysis_requirements>
        請基於評估結果提供：
        1. 整體性能摘要
        2. 各維度表現分析
        3. 常見問題識別
        4. 改進建議
        5. 下一步行動項目
        </analysis_requirements>
        </performance_analysis>
        
        請提供詳細的性能分析報告："""
        
        message = self.client.messages.create(
            model="claude-3-5-sonnet-20241022",
            max_tokens=1500,
            messages=[
                {"role": "user", "content": report_prompt}
            ]
        )
        
        return message.content[0].text

# 使用範例
client = anthropic.Anthropic(api_key="your-api-key")
evaluator = SuccessCriteriaEvaluator(client)

# 定義成功標準
criteria = {
    "accuracy": {
        "target": "≥ 90%",
        "measurement": "正確回答比例",
        "priority": "high"
    },
    "relevance": {
        "target": "≥ 4.0/5.0",
        "measurement": "內容相關性評分",
        "priority": "high"
    },
    "clarity": {
        "target": "≥ 4.0/5.0",
        "measurement": "表達清晰度評分",
        "priority": "medium"
    }
}

evaluator.define_criteria(criteria)

# 評估單個回應
result = evaluator.evaluate_response(
    "這是 Claude 的回應內容...",
    "這是參考答案..."
)

# 批次評估
test_cases = [
    {
        "input": "測試輸入1",
        "response": "Claude 回應1",
        "reference": "參考答案1"
    },
    {
        "input": "測試輸入2", 
        "response": "Claude 回應2",
        "reference": "參考答案2"
    }
]

batch_results = evaluator.batch_evaluate(test_cases)
performance_report = evaluator.generate_performance_report(batch_results)
```

## 最佳實踐

<guidelines>
1. **先定義標準再優化**：在開始提示工程之前就建立清晰的成功標準
2. **使用量化指標**：盡可能使用具體的數字目標而非模糊的描述
3. **多維度評估**：考慮準確性、相關性、一致性、語調等多個維度
4. **優先順序排序**：為不同的評估維度設定重要性等級
5. **自動化優先**：優先使用程式碼自動評分，其次是 LLM 評分，最後才是人工評估
6. **建立基準線**：設定可接受的最低性能標準
7. **持續監控**：在生產環境中持續追蹤性能指標
8. **版本化管理**：記錄不同版本的成功標準和評估結果
</guidelines>

## 常見錯誤

<mistakes>
1. **標準過於模糊**：避免使用「好」、「不錯」等主觀描述
   - 錯誤：「回應品質要好」
   - 正確：「相關性評分 ≥ 4.0/5.0」

2. **缺乏量化指標**：沒有設定具體的數字目標
   - 錯誤：「準確率要高」
   - 正確：「準確率 ≥ 85%」

3. **忽略邊界情況**：沒有考慮特殊或困難的測試案例
   - 錯誤：只測試典型情況
   - 正確：包含邊界情況和異常輸入

4. **過度依賴人工評估**：對大規模評估來說不可持續
   - 錯誤：所有評估都依賴人工判斷
   - 正確：結合自動化和人工評估

5. **標準設定不切實際**：設定無法達成的目標
   - 錯誤：「準確率 100%」
   - 正確：「準確率 ≥ 90%」

6. **缺乏優先順序**：所有標準都同等重要
   - 錯誤：不區分重要性
   - 正確：設定高、中、低優先級

7. **忽略成本考量**：沒有考慮延遲和成本限制
   - 錯誤：只關注準確性
   - 正確：平衡準確性、速度和成本
</mistakes>

## 測試與優化

<evaluation>
評估方法：
1. **A/B 測試**：比較不同提示版本的性能
2. **基準測試**：與既有解決方案或人工表現比較
3. **交叉驗證**：使用多個測試集驗證結果
4. **統計顯著性檢驗**：確保性能改進是統計上顯著的
5. **用戶反饋**：收集實際使用者的回饋意見
6. **邊界測試**：測試極端情況和邊界條件
7. **長期監控**：追蹤生產環境中的性能變化
</evaluation>

<iteration>
迭代改進方法：
1. **建立基準線**：記錄初始性能水準
2. **識別瓶頸**：分析哪些維度需要優先改進
3. **假設驗證**：提出改進假設並進行測試
4. **漸進式改進**：一次只改變一個變數
5. **性能追蹤**：持續監控關鍵指標
6. **回歸測試**：確保新改進不會影響其他功能
7. **文檔更新**：記錄所有改進過程和結果
</iteration>

## 與其他技術組合

<combinations>
成功標準定義可以與以下技術組合使用：

1. **多樣本提示 (Multishot Prompting)**：
   - 使用成功標準來篩選高品質的示例
   - 評估不同示例組合的效果

2. **鏈式思考 (Chain of Thought)**：
   - 評估推理過程的邏輯性和正確性
   - 測量思考步驟的完整性

3. **輸出格式控制**：
   - 檢查輸出格式的一致性
   - 驗證結構化輸出的完整性

4. **工具反思技術**：
   - 評估工具選擇和使用的準確性
   - 測量問題解決的有效性

5. **測試案例開發**：
   - 與成功標準緊密結合
   - 確保測試覆蓋所有評估維度

6. **迭代改進**：
   - 使用成功標準指導改進方向
   - 量化改進效果

7. **提示範本與變數**：
   - 評估範本的穩定性和一致性
   - 測試變數替換的效果
</combinations>

## 官方參考連結

- [Create strong empirical evaluations - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/develop-tests)
- [Empirical performance evaluations - Anthropic](https://docs.anthropic.com/claude/docs/empirical-performance-evaluations)
- [Prompt engineering overview - Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Evaluate prompts in the developer console - Anthropic](https://www.anthropic.com/news/evaluate-prompts)