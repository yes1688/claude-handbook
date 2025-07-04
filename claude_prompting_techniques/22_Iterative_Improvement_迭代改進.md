# 22. 迭代改進(Iterative Improvement)

## 官方定義與說明

<definition>
迭代改進是 Claude 提示工程的核心方法論，基於 Anthropic 官方文檔，這是一個持續性的優化過程，涉及多個循環的測試和改進。提示工程本質上是迭代的，這種方法類似於軟體開發中的測試和除錯過程，使開發者能夠快速嘗試各種方法、調整提示並立即看到結果。成功的提示工程不在於第一次就做對，而在於持續的改進和精進。
</definition>

## 核心原理

<principle>
迭代改進技術基於以下核心原理：
1. **系統性測試**：通過多種場景測試提示的效果
2. **漸進式優化**：每次迭代都基於上一次的結果進行改進
3. **數據驅動決策**：使用客觀的評估指標來指導改進方向
4. **邊界案例探索**：測試極端情況和意外輸入
5. **持續性循環**：建立測試-分析-改進的連續循環流程
6. **快速反饋**：利用提示工程比微調更快速的特性進行快速實驗
</principle>

## 官方效益

<benefits>
根據 Anthropic 官方文檔，迭代改進技術提供以下效益：
1. **效能提升**：官方測試顯示提示改進器使多標籤分類測試的準確率提升 30%
2. **格式遵循**：摘要任務的字數遵循度達到 100%
3. **快速迭代**：比微調等方法更快速，能在更短時間內產生性能飛躍
4. **成本效益**：比微調更具成本效益，同時保持模型的通用知識
5. **透明度**：提供模型互動的透明度，便於理解和調試
6. **靈活性**：允許快速嘗試各種方法並立即看到結果
</benefits>

## 實作格式

### 基本結構
```xml
<iteration_cycle>
  <test_phase>
    <input_scenario>測試場景描述</input_scenario>
    <expected_output>期望結果</expected_output>
  </test_phase>
  
  <evaluation_phase>
    <performance_metrics>性能指標</performance_metrics>
    <gap_analysis>差距分析</gap_analysis>
  </evaluation_phase>
  
  <improvement_phase>
    <modification_plan>改進計劃</modification_plan>
    <implementation>實施步驟</implementation>
  </improvement_phase>
</iteration_cycle>
```

### 完整結構
```xml
<iterative_improvement_framework>
  <success_criteria>
    <primary_goals>主要目標列表</primary_goals>
    <measurement_metrics>測量指標</measurement_metrics>
    <acceptance_threshold>接受門檻</acceptance_threshold>
  </success_criteria>
  
  <testing_methodology>
    <test_cases>
      <happy_path>正常路徑測試</happy_path>
      <edge_cases>邊界案例測試</edge_cases>
      <error_scenarios>錯誤情境測試</error_scenarios>
    </test_cases>
    
    <evaluation_criteria>
      <accuracy>準確性評估</accuracy>
      <consistency>一致性評估</consistency>
      <completeness>完整性評估</completeness>
    </evaluation_criteria>
  </testing_methodology>
  
  <improvement_strategies>
    <technique_application>
      <clarity_enhancement>清晰度增強</clarity_enhancement>
      <example_addition>範例添加</example_addition>
      <structure_optimization>結構優化</structure_optimization>
    </technique_application>
    
    <systematic_refinement>
      <issue_identification>問題識別</issue_identification>
      <solution_implementation>解決方案實施</solution_implementation>
      <validation_testing>驗證測試</validation_testing>
    </systematic_refinement>
  </improvement_strategies>
</iterative_improvement_framework>
```

## 實作範例

### 範例1：文章分類系統的迭代改進
```xml
<iterative_improvement_process>
  <iteration_1>
    <initial_prompt>
      請將以下文章分類為：技術、商業、娛樂、體育
      
      文章：{article_text}
    </initial_prompt>
    
    <test_results>
      <accuracy>65%</accuracy>
      <issues>
        <unclear_boundaries>技術與商業分類模糊</unclear_boundaries>
        <missing_context>缺乏分類標準說明</missing_context>
      </issues>
    </test_results>
  </iteration_1>
  
  <iteration_2>
    <improved_prompt>
      你是一個專業的文章分類專家。請仔細分析文章內容並分類為以下類別之一：
      
      <categories>
        <technology>技術：軟體開發、硬體創新、科技趨勢</technology>
        <business>商業：市場分析、企業策略、金融投資</business>
        <entertainment>娛樂：電影、音樂、遊戲、名人</entertainment>
        <sports>體育：運動賽事、運動員、體育產業</sports>
      </categories>
      
      <analysis_steps>
        1. 識別文章主要主題
        2. 分析內容重點
        3. 對照類別定義
        4. 選擇最符合的分類
      </analysis_steps>
      
      文章：{article_text}
      
      請以以下格式回應：
      分類：[類別名稱]
      理由：[簡要說明]
    </improved_prompt>
    
    <test_results>
      <accuracy>85%</accuracy>
      <improvements>
        <clear_definitions>類別定義明確</clear_definitions>
        <structured_analysis>分析步驟清晰</structured_analysis>
      </improvements>
    </test_results>
  </iteration_2>
  
  <iteration_3>
    <final_prompt>
      你是一個專業的文章分類專家。請使用以下步驟分析文章：
      
      <classification_framework>
        <categories>
          <technology>技術：軟體開發、硬體創新、科技趨勢、數位轉型</technology>
          <business>商業：市場分析、企業策略、金融投資、經濟政策</business>
          <entertainment>娛樂：電影、音樂、遊戲、名人、藝術文化</entertainment>
          <sports>體育：運動賽事、運動員、體育產業、健身養生</sports>
        </categories>
        
        <analysis_process>
          1. 閱讀全文並識別關鍵詞
          2. 分析文章的主要論述焦點
          3. 評估內容與各類別的相關性
          4. 選擇相關性最高的類別
          5. 提供分類理由
        </analysis_process>
        
        <edge_case_handling>
          - 如果文章涉及多個類別，選擇佔內容比重最大的類別
          - 如果內容不明確，選擇最相關的類別並說明不確定性
          - 對於新興或跨領域主題，參考核心內容進行分類
        </edge_case_handling>
      </classification_framework>
      
      文章：{article_text}
      
      <output_format>
        分類：[類別名稱]
        置信度：[高/中/低]
        關鍵指標：[支持分類的主要證據]
        理由：[詳細分類理由]
      </output_format>
    </final_prompt>
    
    <final_results>
      <accuracy>94%</accuracy>
      <consistency>92%</consistency>
      <edge_case_handling>88%</edge_case_handling>
    </final_results>
  </iteration_3>
</iterative_improvement_process>
```

### 範例2：客服回應品質的迭代改進
```xml
<customer_service_improvement>
  <baseline_version>
    <prompt>
      你是客服代表。請回應客戶的查詢。
      
      客戶問題：{customer_inquiry}
    </prompt>
    
    <issues_identified>
      <tone_inconsistency>語調不一致</tone_inconsistency>
      <missing_empathy>缺乏同理心</missing_empathy>
      <incomplete_solutions>解決方案不完整</incomplete_solutions>
    </issues_identified>
  </baseline_version>
  
  <iteration_1>
    <improved_prompt>
      你是一位友善且專業的客服代表。請遵循以下指導原則：
      
      <service_guidelines>
        <tone>友善、專業、有耐心</tone>
        <approach>
          1. 首先表達理解和同理心
          2. 清楚說明問題的解決方案
          3. 提供額外的幫助或建議
          4. 確認客戶滿意度
        </approach>
      </service_guidelines>
      
      客戶問題：{customer_inquiry}
      
      請提供完整的回應。
    </improved_prompt>
    
    <test_results>
      <empathy_score>75%</empathy_score>
      <solution_completeness>80%</solution_completeness>
      <tone_consistency>70%</tone_consistency>
    </test_results>
  </iteration_1>
  
  <iteration_2>
    <optimized_prompt>
      你是一位經驗豐富的客服專家。請使用以下結構回應客戶：
      
      <response_structure>
        <acknowledgment>
          - 感謝客戶聯絡
          - 表達對問題的理解
          - 展現同理心
        </acknowledgment>
        
        <solution_process>
          1. 分析問題核心
          2. 提供具體解決步驟
          3. 說明預期結果
          4. 提供替代方案（如需要）
        </solution_process>
        
        <follow_up>
          - 確認客戶理解
          - 提供額外資源
          - 邀請後續聯絡
        </follow_up>
      </response_structure>
      
      <tone_guidelines>
        - 使用正面、積極的語言
        - 避免技術術語，使用客戶能理解的語言
        - 保持專業但溫暖的語調
        - 展現主動幫助的態度
      </tone_guidelines>
      
      客戶問題：{customer_inquiry}
      
      <output_template>
        親愛的客戶，
        
        [表達理解和同理心]
        
        [提供解決方案]
        
        [確認和後續支援]
        
        祝您一切順利！
        客服團隊
      </output_template>
    </optimized_prompt>
    
    <final_metrics>
      <empathy_score>92%</empathy_score>
      <solution_completeness>95%</solution_completeness>
      <tone_consistency>94%</tone_consistency>
      <customer_satisfaction>89%</customer_satisfaction>
    </final_metrics>
  </iteration_2>
</customer_service_improvement>
```

### 範例3：程式碼審查的迭代改進
```xml
<code_review_improvement>
  <initial_approach>
    <prompt>
      請審查以下程式碼並提供建議：
      
      {code_snippet}
    </prompt>
    
    <problems_found>
      <generic_feedback>建議過於籠統</generic_feedback>
      <missing_priorities>缺乏優先級劃分</missing_priorities>
      <no_examples>沒有具體改進範例</no_examples>
    </problems_found>
  </initial_approach>
  
  <iteration_1>
    <enhanced_prompt>
      你是一位資深軟體工程師。請從以下角度審查程式碼：
      
      <review_aspects>
        <functionality>功能正確性</functionality>
        <performance>性能效率</performance>
        <security>安全性</security>
        <maintainability>可維護性</maintainability>
        <style>程式碼風格</style>
      </review_aspects>
      
      程式碼：{code_snippet}
      
      請提供具體的改進建議。
    </enhanced_prompt>
    
    <improvement_results>
      <coverage>審查覆蓋面更完整</coverage>
      <specificity>建議更具體</specificity>
      <remaining_issues>仍缺乏優先級和範例</remaining_issues>
    </improvement_results>
  </iteration_1>
  
  <iteration_2>
    <comprehensive_prompt>
      你是一位專業的程式碼審查專家。請使用以下系統化方法審查程式碼：
      
      <review_methodology>
        <analysis_framework>
          <critical_issues>
            - 安全漏洞
            - 功能錯誤
            - 性能問題
          </critical_issues>
          
          <improvement_opportunities>
            - 程式碼結構優化
            - 可讀性增強
            - 最佳實踐應用
          </improvement_opportunities>
          
          <style_considerations>
            - 命名規範
            - 格式一致性
            - 註釋品質
          </style_considerations>
        </analysis_framework>
        
        <review_process>
          1. 理解程式碼功能和目的
          2. 識別潛在問題和風險
          3. 評估效能和可維護性
          4. 檢查程式碼風格和規範
          5. 提供優先級化的建議
        </review_process>
      </review_methodology>
      
      程式碼：{code_snippet}
      
      <output_format>
        ## 審查摘要
        [整體評估]
        
        ## 關鍵問題 (必須修復)
        [高優先級問題列表]
        
        ## 改進建議 (建議修復)
        [中優先級建議列表]
        
        ## 風格建議 (可選修復)
        [低優先級建議列表]
        
        ## 改進範例
        [具體程式碼改進示例]
        
        ## 總結
        [整體建議和後續行動]
      </output_format>
    </comprehensive_prompt>
    
    <final_assessment>
      <usefulness>96%</usefulness>
      <actionability>94%</actionability>
      <comprehensiveness>92%</comprehensiveness>
      <developer_satisfaction>91%</developer_satisfaction>
    </final_assessment>
  </iteration_2>
</code_review_improvement>
```

## API實作方式

```python
import anthropic
import json
from typing import List, Dict, Any
import time

class IterativePromptImprover:
    def __init__(self, api_key: str):
        self.client = anthropic.Anthropic(api_key=api_key)
        self.iteration_history = []
        self.performance_metrics = []
    
    def create_test_suite(self, test_cases: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
        """創建測試套件"""
        return [
            {
                "input": test_case["input"],
                "expected_output": test_case.get("expected_output"),
                "test_type": test_case.get("test_type", "functional"),
                "priority": test_case.get("priority", "medium")
            }
            for test_case in test_cases
        ]
    
    def evaluate_prompt(self, prompt: str, test_suite: List[Dict[str, Any]]) -> Dict[str, Any]:
        """評估提示性能"""
        results = {
            "total_tests": len(test_suite),
            "passed": 0,
            "failed": 0,
            "scores": [],
            "detailed_results": []
        }
        
        for i, test_case in enumerate(test_suite):
            try:
                # 執行測試
                message = self.client.messages.create(
                    model="claude-3-sonnet-20240229",
                    max_tokens=1000,
                    messages=[{"role": "user", "content": prompt.format(**test_case["input"])}]
                )
                
                response = message.content[0].text
                
                # 評估結果
                score = self._evaluate_response(response, test_case.get("expected_output"))
                results["scores"].append(score)
                
                if score >= 0.7:  # 設定通過門檻
                    results["passed"] += 1
                else:
                    results["failed"] += 1
                
                results["detailed_results"].append({
                    "test_id": i,
                    "input": test_case["input"],
                    "output": response,
                    "expected": test_case.get("expected_output"),
                    "score": score,
                    "test_type": test_case["test_type"]
                })
                
            except Exception as e:
                results["failed"] += 1
                results["detailed_results"].append({
                    "test_id": i,
                    "error": str(e),
                    "score": 0.0
                })
        
        # 計算總體指標
        if results["scores"]:
            results["average_score"] = sum(results["scores"]) / len(results["scores"])
            results["pass_rate"] = results["passed"] / results["total_tests"]
        
        return results
    
    def _evaluate_response(self, response: str, expected: str = None) -> float:
        """評估回應品質"""
        if expected is None:
            # 使用 Claude 進行品質評估
            evaluation_prompt = f"""
            請評估以下回應的品質（0-1分）：
            
            回應：{response}
            
            評估標準：
            - 清晰度和準確性
            - 完整性
            - 相關性
            - 格式正確性
            
            請只回應數字分數（0.0-1.0）。
            """
            
            try:
                message = self.client.messages.create(
                    model="claude-3-haiku-20240307",
                    max_tokens=10,
                    messages=[{"role": "user", "content": evaluation_prompt}]
                )
                
                score_text = message.content[0].text.strip()
                return float(score_text)
            except:
                return 0.5  # 預設分數
        else:
            # 簡單的相似性評估
            return self._calculate_similarity(response, expected)
    
    def _calculate_similarity(self, response: str, expected: str) -> float:
        """計算相似性分數"""
        # 簡化的相似性計算
        response_words = set(response.lower().split())
        expected_words = set(expected.lower().split())
        
        if not expected_words:
            return 0.0
        
        intersection = response_words.intersection(expected_words)
        return len(intersection) / len(expected_words)
    
    def generate_improvement_suggestions(self, prompt: str, evaluation_results: Dict[str, Any]) -> List[str]:
        """生成改進建議"""
        # 分析失敗案例
        failed_cases = [
            result for result in evaluation_results["detailed_results"] 
            if result.get("score", 0) < 0.7
        ]
        
        improvement_prompt = f"""
        <analysis_request>
        請分析以下提示和測試結果，提供具體的改進建議：
        
        <current_prompt>
        {prompt}
        </current_prompt>
        
        <performance_metrics>
        通過率: {evaluation_results.get('pass_rate', 0):.2%}
        平均分數: {evaluation_results.get('average_score', 0):.2f}
        失敗案例數: {len(failed_cases)}
        </performance_metrics>
        
        <failed_cases>
        {json.dumps(failed_cases[:3], indent=2, ensure_ascii=False)}
        </failed_cases>
        </analysis_request>
        
        請提供5個具體的改進建議，每個建議都要包含：
        1. 問題識別
        2. 解決方案
        3. 實施步驟
        
        格式：
        建議1: [問題] -> [解決方案] -> [實施步驟]
        """
        
        try:
            message = self.client.messages.create(
                model="claude-3-sonnet-20240229",
                max_tokens=1500,
                messages=[{"role": "user", "content": improvement_prompt}]
            )
            
            suggestions = message.content[0].text.strip().split('\n建議')
            return [suggestion.strip() for suggestion in suggestions if suggestion.strip()]
            
        except Exception as e:
            return [f"自動改進建議生成失敗: {str(e)}"]
    
    def iterate_prompt(self, 
                      initial_prompt: str, 
                      test_suite: List[Dict[str, Any]], 
                      max_iterations: int = 5,
                      target_score: float = 0.8) -> Dict[str, Any]:
        """執行迭代改進流程"""
        
        current_prompt = initial_prompt
        iteration_count = 0
        
        while iteration_count < max_iterations:
            print(f"\n=== 迭代 {iteration_count + 1} ===")
            
            # 評估當前提示
            evaluation = self.evaluate_prompt(current_prompt, test_suite)
            
            # 記錄迭代歷史
            iteration_record = {
                "iteration": iteration_count + 1,
                "prompt": current_prompt,
                "evaluation": evaluation,
                "timestamp": time.time()
            }
            self.iteration_history.append(iteration_record)
            
            print(f"通過率: {evaluation.get('pass_rate', 0):.2%}")
            print(f"平均分數: {evaluation.get('average_score', 0):.2f}")
            
            # 檢查是否達到目標
            if evaluation.get('average_score', 0) >= target_score:
                print(f"達到目標分數 {target_score}！")
                break
            
            # 生成改進建議
            suggestions = self.generate_improvement_suggestions(current_prompt, evaluation)
            
            # 應用改進建議（簡化版本）
            current_prompt = self._apply_improvements(current_prompt, suggestions)
            
            iteration_count += 1
            
            # 添加延遲避免API限制
            time.sleep(1)
        
        return {
            "final_prompt": current_prompt,
            "iterations": iteration_count,
            "final_evaluation": evaluation,
            "history": self.iteration_history
        }
    
    def _apply_improvements(self, prompt: str, suggestions: List[str]) -> str:
        """應用改進建議"""
        # 使用 Claude 幫助應用改進建議
        improvement_prompt = f"""
        請根據以下建議改進提示：
        
        原提示：
        {prompt}
        
        改進建議：
        {chr(10).join(suggestions)}
        
        請提供改進後的提示，保持原有功能的同時融入改進建議。
        """
        
        try:
            message = self.client.messages.create(
                model="claude-3-sonnet-20240229",
                max_tokens=2000,
                messages=[{"role": "user", "content": improvement_prompt}]
            )
            
            return message.content[0].text.strip()
            
        except Exception as e:
            print(f"自動改進失敗: {str(e)}")
            return prompt
    
    def generate_report(self) -> str:
        """生成迭代改進報告"""
        if not self.iteration_history:
            return "沒有迭代歷史資料"
        
        report = "# 迭代改進報告\n\n"
        
        # 性能趨勢
        scores = [iter_data["evaluation"].get("average_score", 0) for iter_data in self.iteration_history]
        pass_rates = [iter_data["evaluation"].get("pass_rate", 0) for iter_data in self.iteration_history]
        
        report += "## 性能趨勢\n"
        report += f"初始分數: {scores[0]:.2f}\n"
        report += f"最終分數: {scores[-1]:.2f}\n"
        report += f"改進幅度: {(scores[-1] - scores[0]):.2f}\n\n"
        
        report += f"初始通過率: {pass_rates[0]:.2%}\n"
        report += f"最終通過率: {pass_rates[-1]:.2%}\n"
        report += f"通過率改進: {(pass_rates[-1] - pass_rates[0]):.2%}\n\n"
        
        # 迭代詳情
        report += "## 迭代詳情\n"
        for i, iter_data in enumerate(self.iteration_history):
            report += f"### 迭代 {i+1}\n"
            report += f"- 平均分數: {iter_data['evaluation'].get('average_score', 0):.2f}\n"
            report += f"- 通過率: {iter_data['evaluation'].get('pass_rate', 0):.2%}\n"
            report += f"- 通過測試: {iter_data['evaluation'].get('passed', 0)}/{iter_data['evaluation'].get('total_tests', 0)}\n\n"
        
        return report

# 使用範例
def main():
    # 初始化改進器
    improver = IterativePromptImprover("your-api-key-here")
    
    # 定義測試套件
    test_suite = [
        {
            "input": {"text": "這是一篇關於人工智能的文章"},
            "expected_output": "技術",
            "test_type": "classification",
            "priority": "high"
        },
        {
            "input": {"text": "股市今天大漲"},
            "expected_output": "商業",
            "test_type": "classification",
            "priority": "high"
        },
        {
            "input": {"text": "新電影上映"},
            "expected_output": "娛樂",
            "test_type": "classification",
            "priority": "medium"
        }
    ]
    
    # 初始提示
    initial_prompt = "請將以下文章分類：{text}"
    
    # 執行迭代改進
    result = improver.iterate_prompt(
        initial_prompt=initial_prompt,
        test_suite=test_suite,
        max_iterations=3,
        target_score=0.85
    )
    
    # 生成報告
    report = improver.generate_report()
    print(report)
    
    print(f"\n最終提示:\n{result['final_prompt']}")

if __name__ == "__main__":
    main()
```

## 最佳實踐

<guidelines>
1. **建立明確的成功標準**
   - 定義具體、可測量的目標
   - 設定數值化的評估指標
   - 確立可接受的性能門檻

2. **系統化測試方法**
   - 創建全面的測試案例
   - 包含正常路徑和邊界案例
   - 測試不同類型的輸入情境

3. **結構化改進流程**
   - 一次只改進一個方面
   - 記錄每次改進的原因和結果
   - 保持改進的可追溯性

4. **利用官方工具**
   - 使用 Anthropic Console 的提示改進器
   - 利用評估功能進行客觀測試
   - 善用範例管理功能

5. **持續監控和調整**
   - 定期重新評估提示性能
   - 關注新的使用情境
   - 適時調整測試案例

6. **文檔化最佳實踐**
   - 記錄成功的改進策略
   - 整理常見問題和解決方案
   - 建立提示模板庫
</guidelines>

## 常見錯誤

<mistakes>
1. **缺乏明確的測試標準**
   - 錯誤：沒有設定具體的成功標準
   - 正確：定義明確的評估指標和接受門檻

2. **改進過於激進**
   - 錯誤：一次改變太多內容
   - 正確：漸進式改進，每次專注一個方面

3. **忽略邊界案例**
   - 錯誤：只測試理想情況
   - 正確：包含各種異常和邊界情況

4. **缺乏系統性記錄**
   - 錯誤：沒有記錄改進歷史
   - 正確：詳細記錄每次迭代的結果

5. **過度依賴主觀評估**
   - 錯誤：僅憑感覺判斷改進效果
   - 正確：使用客觀指標進行評估

6. **忽視性能退化**
   - 錯誤：沒有檢查改進是否影響其他功能
   - 正確：全面測試確保整體性能不退化
</mistakes>

## 測試與優化

<evaluation>
**評估方法**：
1. **A/B測試**：比較不同版本的提示性能
2. **交叉驗證**：使用多個測試集驗證改進效果
3. **性能基準**：建立基準分數進行比較
4. **使用者反饋**：收集實際使用者的反饋
5. **自動化測試**：建立自動化測試流程
6. **長期監控**：追蹤長期性能趨勢

**評估指標**：
- 準確率、精確率、召回率
- 回應時間和穩定性
- 使用者滿意度
- 任務完成率
- 錯誤率和異常情況處理能力
</evaluation>

<iteration>
**迭代改進方法**：
1. **階段性目標**：設定短期和長期改進目標
2. **版本控制**：管理不同版本的提示
3. **回歸測試**：確保新改進不影響既有功能
4. **漸進式部署**：逐步推出改進版本
5. **反饋循環**：建立持續反饋機制
6. **知識累積**：將改進經驗轉化為最佳實踐

**改進策略**：
- 從最大的問題開始改進
- 優先處理高頻使用場景
- 結合多種提示工程技術
- 定期重新評估改進需求
- 保持改進的可維護性
</iteration>

## 與其他技術組合

<combinations>
1. **與系統提示詞結合**
   - 在系統提示詞中定義改進框架
   - 使用角色設定支持迭代過程

2. **與XML標籤結合**
   - 使用XML結構化改進流程
   - 標記不同版本的提示元素

3. **與範例對齊結合**
   - 通過範例展示改進效果
   - 使用範例指導改進方向

4. **與測試案例開發結合**
   - 建立完整的測試框架
   - 系統化驗證改進效果

5. **與鏈式複雜提示結合**
   - 對複雜提示鏈進行分段改進
   - 優化各個環節的性能

6. **與輸出格式控制結合**
   - 改進輸出格式的一致性
   - 優化結構化輸出的品質

7. **與成功標準定義結合**
   - 建立明確的改進目標
   - 量化改進效果
</combinations>

## 官方參考連結

- [Anthropic 提示工程概述](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Anthropic 提示改進器](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-improver)
- [Claude 4 最佳實踐指南](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Anthropic 互動式提示工程教程](https://github.com/anthropics/prompt-eng-interactive-tutorial)
- [鏈式複雜提示指南](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-prompts)