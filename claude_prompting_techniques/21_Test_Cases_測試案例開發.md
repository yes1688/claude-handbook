# 21. 測試案例開發(Test Cases)

## 官方定義與說明

<definition>
基於 Anthropic 官方文檔，測試案例開發是一個系統化的評估框架，用於定義成功標準並創建實證評估方法來測試 Claude 的提示詞效能。Anthropic 將 Claude 優化描述為「實證科學和持續改進的過程」，其中精心設計的評估系統對成功至關重要。
</definition>

## 核心原理

<principle>
測試案例開發基於三個核心原理：
1. **任務特定性**：測試案例必須反映真實世界任務分佈，包含邊緣情況
2. **可測量性**：建立明確、具體的成功標準和評估指標
3. **迭代性**：通過持續測試和評估來改進提示詞效能，形成閉環優化循環

核心工作流程：定義成功標準 → 開發測試案例 → 實施評估 → 分析結果 → 優化提示詞
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，測試案例開發提供以下效益：
- 提供客觀、可量化的效能評估標準
- 防止過度擬合特定測試案例，促進泛化性能
- 支援自動化評估，提高測試效率和規模
- 建立持續改進循環，系統化地優化提示詞效能
- 確保模型在各種情境下的穩定表現
- 提供跨版本和跨配置的效能比較基準
</benefits>

## 實作格式

### 基本結構
```xml
<test_case_framework>
  <success_criteria>
    <metric>準確度|一致性|延遲|成本</metric>
    <target_value>具體數值或描述</target_value>
  </success_criteria>
  
  <test_cases>
    <case id="1">
      <input>測試輸入</input>
      <expected_output>預期輸出</expected_output>
      <evaluation_method>評估方法</evaluation_method>
    </case>
  </test_cases>
</test_case_framework>
```

### 完整結構
```xml
<comprehensive_test_framework>
  <project_definition>
    <task_description>任務描述</task_description>
    <success_criteria>
      <primary_metrics>主要指標</primary_metrics>
      <secondary_metrics>次要指標</secondary_metrics>
      <edge_case_handling>邊緣情況處理</edge_case_handling>
    </success_criteria>
  </project_definition>

  <test_suite>
    <baseline_tests>
      <test_case category="基本功能">
        <input>{{input_variable}}</input>
        <expected_output>{{expected_variable}}</expected_output>
        <grading_method>exact_match|cosine_similarity|llm_grading</grading_method>
      </test_case>
    </baseline_tests>
    
    <edge_case_tests>
      <test_case category="邊緣情況">
        <scenario>具體情境描述</scenario>
        <input>邊緣情況輸入</input>
        <expected_behavior>預期行為</expected_behavior>
      </test_case>
    </edge_case_tests>
    
    <stress_tests>
      <test_case category="壓力測試">
        <volume>大量輸入</volume>
        <complexity>複雜情境</complexity>
        <performance_requirements>效能要求</performance_requirements>
      </test_case>
    </stress_tests>
  </test_suite>

  <evaluation_methods>
    <automated_grading>
      <code_based>程式碼評分</code_based>
      <llm_based>LLM評分</llm_based>
      <similarity_metrics>相似度指標</similarity_metrics>
    </automated_grading>
    
    <manual_evaluation>
      <human_review>人工審查</human_review>
      <quality_assessment>品質評估</quality_assessment>
    </manual_evaluation>
  </evaluation_methods>
</comprehensive_test_framework>
```

## 實作範例

### 範例1：情感分析測試案例
```xml
<sentiment_analysis_test>
  <success_criteria>
    <accuracy>≥ 90%</accuracy>
    <consistency>同類型文本結果一致</consistency>
    <edge_case_handling>處理諷刺、混合情感</edge_case_handling>
  </success_criteria>

  <test_cases>
    <case id="negative_clear">
      <input>This movie was a total waste of time. 👎</input>
      <expected_output>negative</expected_output>
      <evaluation_method>exact_match</evaluation_method>
    </case>
    
    <case id="positive_enthusiastic">
      <input>The new album is 🔥! Been on repeat all day.</input>
      <expected_output>positive</expected_output>
      <evaluation_method>exact_match</evaluation_method>
    </case>
    
    <case id="sarcastic_negative">
      <input>I just love it when my flight gets delayed for 5 hours. #bestdayever</input>
      <expected_output>negative</expected_output>
      <evaluation_method>llm_grading</evaluation_method>
      <grading_rubric>
        評估是否正確識別諷刺語氣背後的負面情感
      </grading_rubric>
    </case>
    
    <case id="mixed_sentiment">
      <input>The movie's plot was terrible, but the acting was phenomenal.</input>
      <expected_output>mixed</expected_output>
      <evaluation_method>llm_grading</evaluation_method>
      <grading_rubric>
        評估是否正確識別並平衡正負面元素
      </grading_rubric>
    </case>
  </test_cases>
</sentiment_analysis_test>
```

### 範例2：程式碼生成測試案例
```xml
<code_generation_test>
  <success_criteria>
    <functionality>程式碼功能正確性</functionality>
    <robustness>處理所有有效輸入</robustness>
    <code_quality>可讀性和可維護性</code_quality>
  </success_criteria>

  <test_cases>
    <case id="basic_function">
      <input>撰寫一個計算兩個數字最大公約數的函數</input>
      <expected_behavior>
        <functionality>正確計算GCD</functionality>
        <edge_cases>處理零值、負數、相同數字</edge_cases>
        <algorithm>使用有效演算法（如歐幾里得演算法）</algorithm>
      </expected_behavior>
      <evaluation_method>code_execution</evaluation_method>
      <test_inputs>
        <input_set>gcd(12, 18) → 6</input_set>
        <input_set>gcd(17, 13) → 1</input_set>
        <input_set>gcd(0, 5) → 5</input_set>
      </test_inputs>
    </case>
    
    <case id="robust_implementation">
      <input>實作一個解決方案，適用於所有有效輸入，而不僅僅是測試案例</input>
      <expected_behavior>
        避免硬編碼值，實現通用解決方案
      </expected_behavior>
      <evaluation_method>code_review</evaluation_method>
      <quality_metrics>
        <generality>通用性</generality>
        <maintainability>可維護性</maintainability>
        <documentation>文檔完整性</documentation>
      </quality_metrics>
    </case>
  </test_cases>
</code_generation_test>
```

### 範例3：客戶服務對話測試案例
```xml
<customer_service_test>
  <success_criteria>
    <helpfulness>解決客戶問題</helpfulness>
    <politeness>保持禮貌語氣</politeness>
    <accuracy>提供正確資訊</accuracy>
    <consistency>保持角色一致性</consistency>
  </success_criteria>

  <test_cases>
    <case id="standard_inquiry">
      <input>
        <customer_message>我想了解退貨政策</customer_message>
        <context>一般產品退貨詢問</context>
      </input>
      <expected_behavior>
        <information_accuracy>提供準確的退貨政策資訊</information_accuracy>
        <tone>友善、專業</tone>
        <completeness>包含時限、條件、流程</completeness>
      </expected_behavior>
      <evaluation_method>llm_grading</evaluation_method>
    </case>
    
    <case id="complex_complaint">
      <input>
        <customer_message>我的訂單延遲了三週，客服一直說在處理，我要求退款！</customer_message>
        <context>憤怒客戶投訴</context>
      </input>
      <expected_behavior>
        <empathy>表達同理心</empathy>
        <solution_focus>提供具體解決方案</solution_focus>
        <de_escalation>緩解客戶情緒</de_escalation>
        <follow_up>確認後續行動</follow_up>
      </expected_behavior>
      <evaluation_method>human_review</evaluation_method>
    </case>
    
    <case id="edge_case_handling">
      <input>
        <customer_message>你們的產品讓我的寵物生病了，我要起訴你們！</customer_message>
        <context>嚴重投訴威脅</context>
      </input>
      <expected_behavior>
        <professional_response>保持專業</professional_response>
        <escalation>適當升級處理</escalation>
        <liability_awareness>避免承認責任</liability_awareness>
      </expected_behavior>
      <evaluation_method>expert_review</evaluation_method>
    </case>
  </test_cases>
</customer_service_test>
```

## API實作方式

```python
import anthropic
import csv
import json
from typing import List, Dict, Any
from dataclasses import dataclass

@dataclass
class TestCase:
    id: str
    input_text: str
    expected_output: str
    evaluation_method: str
    category: str = "general"

class ClaudeTestFramework:
    def __init__(self, api_key: str):
        self.client = anthropic.Anthropic(api_key=api_key)
        self.test_cases = []
        self.results = []
    
    def add_test_case(self, test_case: TestCase):
        """添加測試案例"""
        self.test_cases.append(test_case)
    
    def load_test_cases_from_csv(self, csv_path: str):
        """從CSV文件載入測試案例"""
        with open(csv_path, 'r', encoding='utf-8') as file:
            reader = csv.DictReader(file)
            for row in reader:
                test_case = TestCase(
                    id=row['id'],
                    input_text=row['input'],
                    expected_output=row['expected_output'],
                    evaluation_method=row['evaluation_method'],
                    category=row.get('category', 'general')
                )
                self.add_test_case(test_case)
    
    def run_test_suite(self, system_prompt: str, user_prompt_template: str):
        """執行完整測試套件"""
        results = []
        
        for test_case in self.test_cases:
            # 格式化提示詞
            user_prompt = user_prompt_template.format(input=test_case.input_text)
            
            # 執行Claude請求
            response = self.client.messages.create(
                model="claude-3-5-sonnet-20241022",
                max_tokens=1000,
                system=system_prompt,
                messages=[
                    {"role": "user", "content": user_prompt}
                ]
            )
            
            actual_output = response.content[0].text
            
            # 評估結果
            score = self.evaluate_response(
                test_case.expected_output,
                actual_output,
                test_case.evaluation_method
            )
            
            result = {
                'test_id': test_case.id,
                'category': test_case.category,
                'input': test_case.input_text,
                'expected': test_case.expected_output,
                'actual': actual_output,
                'score': score,
                'passed': score >= 0.8  # 80% 閾值
            }
            
            results.append(result)
            
        self.results = results
        return results
    
    def evaluate_response(self, expected: str, actual: str, method: str) -> float:
        """評估回應品質"""
        if method == "exact_match":
            return 1.0 if expected.strip().lower() == actual.strip().lower() else 0.0
        
        elif method == "cosine_similarity":
            # 簡化版本，實際應用需要使用嵌入模型
            from difflib import SequenceMatcher
            return SequenceMatcher(None, expected, actual).ratio()
        
        elif method == "llm_grading":
            return self.llm_grade_response(expected, actual)
        
        else:
            return 0.0
    
    def llm_grade_response(self, expected: str, actual: str) -> float:
        """使用LLM評分回應"""
        grading_prompt = f"""
        請評估以下回應的品質，給出0到1之間的分數：
        
        預期回應：{expected}
        實際回應：{actual}
        
        評分標準：
        - 準確性：回應是否正確
        - 完整性：回應是否完整
        - 相關性：回應是否切題
        
        只需回應數字分數（0.0到1.0）：
        """
        
        response = self.client.messages.create(
            model="claude-3-5-sonnet-20241022",
            max_tokens=10,
            system="你是一位專業的回應評估者，只需提供數字分數。",
            messages=[
                {"role": "user", "content": grading_prompt}
            ]
        )
        
        try:
            score = float(response.content[0].text.strip())
            return max(0.0, min(1.0, score))  # 確保分數在0-1範圍內
        except:
            return 0.0
    
    def generate_test_cases(self, task_description: str, num_cases: int = 10) -> List[TestCase]:
        """自動生成測試案例"""
        generation_prompt = f"""
        為以下任務生成 {num_cases} 個測試案例：
        
        任務描述：{task_description}
        
        請為每個測試案例提供：
        1. 輸入內容
        2. 預期輸出
        3. 測試類別（基本功能、邊緣情況、壓力測試）
        
        以JSON格式回應：
        ```json
        [
          {{
            "id": "test_1",
            "input": "測試輸入",
            "expected_output": "預期輸出",
            "category": "基本功能"
          }}
        ]
        ```
        """
        
        response = self.client.messages.create(
            model="claude-3-5-sonnet-20241022",
            max_tokens=2000,
            system="你是一位專業的測試案例設計師，專門為AI模型創建綜合測試案例。",
            messages=[
                {"role": "user", "content": generation_prompt}
            ]
        )
        
        try:
            # 提取JSON內容
            content = response.content[0].text
            json_start = content.find('[')
            json_end = content.rfind(']') + 1
            json_str = content[json_start:json_end]
            
            test_data = json.loads(json_str)
            
            test_cases = []
            for item in test_data:
                test_case = TestCase(
                    id=item['id'],
                    input_text=item['input'],
                    expected_output=item['expected_output'],
                    evaluation_method="llm_grading",
                    category=item.get('category', 'general')
                )
                test_cases.append(test_case)
            
            return test_cases
            
        except Exception as e:
            print(f"生成測試案例時發生錯誤: {e}")
            return []
    
    def analyze_results(self) -> Dict[str, Any]:
        """分析測試結果"""
        if not self.results:
            return {}
        
        total_tests = len(self.results)
        passed_tests = sum(1 for r in self.results if r['passed'])
        average_score = sum(r['score'] for r in self.results) / total_tests
        
        # 按類別分析
        category_stats = {}
        for result in self.results:
            category = result['category']
            if category not in category_stats:
                category_stats[category] = {'total': 0, 'passed': 0, 'scores': []}
            
            category_stats[category]['total'] += 1
            if result['passed']:
                category_stats[category]['passed'] += 1
            category_stats[category]['scores'].append(result['score'])
        
        # 計算各類別平均分
        for category in category_stats:
            scores = category_stats[category]['scores']
            category_stats[category]['average_score'] = sum(scores) / len(scores)
        
        return {
            'overall_pass_rate': passed_tests / total_tests,
            'average_score': average_score,
            'total_tests': total_tests,
            'passed_tests': passed_tests,
            'category_breakdown': category_stats,
            'failed_tests': [r for r in self.results if not r['passed']]
        }
    
    def export_results(self, filename: str):
        """匯出測試結果"""
        with open(filename, 'w', encoding='utf-8', newline='') as file:
            if not self.results:
                return
            
            fieldnames = ['test_id', 'category', 'input', 'expected', 'actual', 'score', 'passed']
            writer = csv.DictWriter(file, fieldnames=fieldnames)
            writer.writeheader()
            writer.writerows(self.results)

# 使用範例
def main():
    # 初始化測試框架
    test_framework = ClaudeTestFramework(api_key="your_api_key_here")
    
    # 定義系統提示詞
    system_prompt = """
    你是一位專業的情感分析專家。
    請分析輸入文本的情感色彩，並給出以下其中一個分類：
    - positive（正面）
    - negative（負面）
    - mixed（混合）
    - neutral（中性）
    
    請只回應分類結果，不需要額外解釋。
    """
    
    # 自動生成測試案例
    generated_cases = test_framework.generate_test_cases(
        "情感分析任務：分析文本的情感色彩",
        num_cases=5
    )
    
    for case in generated_cases:
        test_framework.add_test_case(case)
    
    # 手動添加特定測試案例
    test_framework.add_test_case(TestCase(
        id="manual_negative",
        input_text="This movie was terrible!",
        expected_output="negative",
        evaluation_method="exact_match",
        category="manual"
    ))
    
    # 執行測試套件
    user_prompt_template = "請分析以下文本的情感：{input}"
    results = test_framework.run_test_suite(system_prompt, user_prompt_template)
    
    # 分析結果
    analysis = test_framework.analyze_results()
    print("測試結果分析：")
    print(f"通過率：{analysis['overall_pass_rate']:.2%}")
    print(f"平均分數：{analysis['average_score']:.2f}")
    print(f"總測試數：{analysis['total_tests']}")
    
    # 匯出結果
    test_framework.export_results("test_results.csv")

if __name__ == "__main__":
    main()
```

## 最佳實踐

<guidelines>
1. **任務特定化設計**
   - 設計反映真實世界任務分佈的測試案例
   - 包含常見情境和邊緣情況
   - 確保測試案例覆蓋各種可能的輸入類型

2. **評估方法選擇**
   - 優先使用程式碼評分（最快速、可靠）
   - 使用LLM評分處理複雜情境（靈活、可擴展）
   - 人工評估僅用於最關鍵案例（成本高、規模小）

3. **測試案例品質**
   - 優先考慮測試案例的數量而非單一案例品質
   - 自動化評分流程以支援大規模測試
   - 建立清晰、詳細的評分標準

4. **避免過度擬合**
   - 使用「為所有有效輸入實作解決方案，而非僅針對測試案例」的指導
   - 避免硬編碼值或僅適用於特定測試輸入的解決方案
   - 保留一組從未在開發過程中使用的測試集進行最終評估

5. **持續改進循環**
   - 建立系統化的測試流程
   - 定期分析失敗案例並改進提示詞
   - 追蹤不同版本之間的效能變化
</guidelines>

## 常見錯誤

<mistakes>
1. **測試案例設計錯誤**
   - 錯誤：僅測試成功案例，忽略失敗和邊緣情況
   - 正確：包含各種情境，特別是可能導致失敗的情況

2. **評估標準模糊**
   - 錯誤：使用主觀或不明確的評估標準
   - 正確：建立具體、可量化的成功標準

3. **過度擬合測試案例**
   - 錯誤：針對特定測試案例優化提示詞
   - 正確：確保提示詞在未見過的案例上也能良好表現

4. **測試規模不足**
   - 錯誤：僅使用少量測試案例
   - 正確：使用大量多樣化的測試案例確保可靠性

5. **忽略自動化**
   - 錯誤：依賴手動評估，導致測試瓶頸
   - 正確：優先實施自動化評估流程

6. **缺乏版本控制**
   - 錯誤：不追蹤提示詞變更對測試結果的影響
   - 正確：建立版本控制系統追蹤效能變化
</mistakes>

## 測試與優化

<evaluation>
測試案例開發的評估方法：

1. **覆蓋率評估**
   - 功能覆蓋率：測試案例是否涵蓋所有主要功能
   - 情境覆蓋率：是否包含各種使用情境
   - 邊緣案例覆蓋率：是否測試異常和邊界情況

2. **預測性評估**
   - 測試結果是否能準確預測實際使用效能
   - 失敗案例分析是否能指導改進方向
   - 測試通過率與實際使用品質的相關性

3. **效率評估**
   - 測試執行時間和成本
   - 自動化程度
   - 結果分析的便利性

4. **穩定性評估**
   - 測試結果的可重現性
   - 不同環境下的一致性
   - 跨時間的穩定性
</evaluation>

<iteration>
測試案例開發的迭代改進方法：

1. **分析失敗模式**
   - 識別常見失敗類型
   - 分析失敗原因
   - 設計針對性測試案例

2. **擴展測試覆蓋**
   - 根據實際使用回饋添加新測試案例
   - 定期審查測試案例的相關性
   - 移除過時或重複的測試案例

3. **優化評估方法**
   - 改進評分標準的準確性
   - 調整評估權重
   - 引入新的評估指標

4. **自動化改進**
   - 增加自動化評估的比例
   - 優化測試執行效率
   - 改進結果分析工具

5. **持續監控**
   - 建立測試結果儀表板
   - 設定效能警報
   - 定期效能報告
</iteration>

## 與其他技術組合

<combinations>
測試案例開發可與以下技術有效組合：

1. **與成功標準定義結合**
   - 使用明確定義的成功標準設計測試案例
   - 確保測試案例與業務目標一致
   - 建立可測量的績效指標

2. **與提示範本結合**
   - 為提示範本建立標準化測試套件
   - 測試不同變數組合的效果
   - 驗證範本的通用性

3. **與迭代改進結合**
   - 使用測試結果指導提示詞改進
   - 建立版本控制和A/B測試流程
   - 追蹤改進效果

4. **與XML標籤結構結合**
   - 測試XML格式的正確性
   - 驗證結構化輸出的一致性
   - 確保標籤使用的有效性

5. **與工具反思技術結合**
   - 測試工具使用的準確性
   - 評估反思過程的品質
   - 驗證錯誤檢測和修正能力

6. **與輸出格式控制結合**
   - 測試格式化輸出的一致性
   - 驗證不同格式要求的遵循度
   - 確保輸出結構的穩定性
</combinations>

## 官方參考連結

- [Anthropic Prompt Engineering Overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Create Strong Empirical Evaluations](https://docs.anthropic.com/en/docs/build-with-claude/develop-tests)
- [Using the Evaluation Tool](https://docs.anthropic.com/en/docs/test-and-evaluate/eval-tool)
- [Empirical Performance Evaluations](https://docs.anthropic.com/en/docs/empirical-performance-evaluations)
- [Interactive Prompt Engineering Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)