# 17. 鏈式複雜提示(Chain Complex Prompts)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔，鏈式複雜提示是一種將複雜任務分解為更小、更易管理的子任務的技術。每個子任務都能獲得 Claude 的全力關注，透過將提示鏈接在一起，引導 Claude 完成一系列較小、更易管理的任務，最終實現複雜的目標。
</definition>

## 核心原理

<principle>
鏈式複雜提示的核心原理是「分而治之」的策略：
1. 將複雜任務分解為多個獨立的子任務
2. 每個子任務都有明確、單一的目標
3. 使用前一個提示的輸出作為下一個提示的輸入
4. 透過 XML 標籤結構化地傳遞信息
5. 確保每個步驟都能獲得 Claude 的專注處理

工作流程：任務1 → 輸出1 → 任務2 → 輸出2 → 任務3 → 最終結果
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔中說明的效益：
1. **提高準確性**：每個子任務都能獲得 Claude 的全力關注，減少錯誤
2. **增強清晰度**：較簡單的子任務意味著更清晰的指示和輸出
3. **優化追蹤性**：更容易定位和修復提示鏈中的問題
4. **支援迭代**：可以根據 Claude 的表現優化特定子任務
5. **減少遺漏**：避免在單一複雜提示中遺漏某些任務要求
</benefits>

## 實作格式

### 基本結構
```xml
<!-- 第一步：任務分解 -->
<task_breakdown>
<step1>具體子任務1</step1>
<step2>具體子任務2</step2>
<step3>具體子任務3</step3>
</task_breakdown>

<!-- 第二步：執行第一個子任務 -->
<step1_execution>
[詳細指示和要求]
</step1_execution>

<!-- 第三步：使用前一步輸出 -->
<previous_output>
[前一步的結果]
</previous_output>

<next_step>
[基於前一步結果的下一步指示]
</next_step>
```

### 完整結構
```xml
<chain_prompt_system>
<overall_objective>
[整體複雜任務目標]
</overall_objective>

<task_chain>
<step id="1">
<description>第一個子任務描述</description>
<input>[輸入內容]</input>
<instructions>[具體執行指示]</instructions>
<output_format>[輸出格式要求]</output_format>
</step>

<step id="2">
<description>第二個子任務描述</description>
<input_from_previous>從步驟1獲取的輸出</input_from_previous>
<instructions>[基於前一步結果的指示]</instructions>
<output_format>[輸出格式要求]</output_format>
</step>

<step id="3">
<description>第三個子任務描述</description>
<input_from_previous>從步驟2獲取的輸出</input_from_previous>
<instructions>[最終處理指示]</instructions>
<final_output_format>[最終輸出格式]</final_output_format>
</step>
</task_chain>

<quality_control>
<review_criteria>[品質檢查標準]</review_criteria>
<self_correction>如發現問題，請重新執行相關步驟</self_correction>
</quality_control>
</chain_prompt_system>
```

## 實作範例

### 範例1：學術論文分析鏈
```xml
<academic_paper_analysis_chain>
<overall_objective>
對學術論文進行全面分析，產生結構化的研究報告
</overall_objective>

<step id="1">
<description>提取論文核心信息</description>
<input>[論文PDF內容]</input>
<instructions>
請提取以下信息：
1. 論文標題和作者
2. 研究問題和假設
3. 方法論概述
4. 主要發現
5. 結論
</instructions>
<output_format>
<extracted_info>
<title>[標題]</title>
<authors>[作者]</authors>
<research_question>[研究問題]</research_question>
<methodology>[方法論]</methodology>
<findings>[主要發現]</findings>
<conclusion>[結論]</conclusion>
</extracted_info>
</output_format>
</step>

<step id="2">
<description>分析研究方法和數據</description>
<input_from_previous>從步驟1獲取的提取信息</input_from_previous>
<instructions>
基於提取的信息，分析：
1. 研究方法的適當性
2. 數據收集方法的有效性
3. 統計分析的合理性
4. 研究限制
</instructions>
<output_format>
<methodology_analysis>
<appropriateness>[方法適當性評估]</appropriateness>
<data_collection>[數據收集評估]</data_collection>
<statistical_analysis>[統計分析評估]</statistical_analysis>
<limitations>[研究限制]</limitations>
</methodology_analysis>
</output_format>
</step>

<step id="3">
<description>生成綜合評估報告</description>
<input_from_previous>從步驟1和2獲取的所有分析</input_from_previous>
<instructions>
結合所有分析結果，生成包含以下內容的綜合報告：
1. 論文摘要
2. 優點分析
3. 缺點分析
4. 學術貢獻評估
5. 建議改進方向
</instructions>
<final_output_format>
# 學術論文分析報告

## 論文摘要
[基於提取信息的摘要]

## 優點分析
[論文的學術優點]

## 缺點分析
[論文的不足之處]

## 學術貢獻評估
[對學術領域的貢獻]

## 建議改進方向
[未來研究建議]
</final_output_format>
</step>
</academic_paper_analysis_chain>
```

### 範例2：產品開發策略鏈
```xml
<product_development_strategy_chain>
<overall_objective>
為新產品制定完整的開發和上市策略
</overall_objective>

<step id="1">
<description>市場研究和競爭分析</description>
<input>[產品概念和目標市場信息]</input>
<instructions>
進行以下分析：
1. 目標市場規模和特徵
2. 競爭對手分析
3. 市場機會和威脅
4. 消費者需求分析
</instructions>
<output_format>
<market_analysis>
<market_size>[市場規模]</market_size>
<target_audience>[目標受眾]</target_audience>
<competitors>[競爭對手]</competitors>
<opportunities>[市場機會]</opportunities>
<threats>[市場威脅]</threats>
<consumer_needs>[消費者需求]</consumer_needs>
</market_analysis>
</output_format>
</step>

<step id="2">
<description>產品定位和差異化策略</description>
<input_from_previous>從步驟1獲取的市場分析</input_from_previous>
<instructions>
基於市場分析結果，制定：
1. 產品定位策略
2. 差異化優勢
3. 價值主張
4. 品牌策略
</instructions>
<output_format>
<positioning_strategy>
<product_positioning>[產品定位]</product_positioning>
<differentiation>[差異化優勢]</differentiation>
<value_proposition>[價值主張]</value_proposition>
<brand_strategy>[品牌策略]</brand_strategy>
</positioning_strategy>
</output_format>
</step>

<step id="3">
<description>制定執行計劃和時程</description>
<input_from_previous>從步驟1和2獲取的所有策略信息</input_from_previous>
<instructions>
結合市場分析和定位策略，制定詳細的執行計劃：
1. 產品開發時程
2. 行銷策略
3. 預算分配
4. 風險管理
5. 成功指標
</instructions>
<final_output_format>
# 產品開發策略執行計劃

## 市場洞察
[基於市場分析的關鍵洞察]

## 產品策略
[基於定位分析的產品策略]

## 執行時程
[詳細的開發和上市時程]

## 行銷計劃
[綜合行銷策略]

## 預算規劃
[資源分配和預算]

## 風險管理
[潛在風險和應對策略]

## 成功指標
[可衡量的成功標準]
</final_output_format>
</step>
</product_development_strategy_chain>
```

### 範例3：自我修正鏈
```xml
<self_correction_chain>
<overall_objective>
創作高品質的技術文檔並進行自我改進
</overall_objective>

<step id="1">
<description>初稿創作</description>
<input>[技術主題和要求]</input>
<instructions>
根據給定主題，創作技術文檔初稿，包含：
1. 技術概述
2. 實作方法
3. 代碼範例
4. 最佳實踐
</instructions>
<output_format>
<initial_draft>
[完整的技術文檔初稿]
</initial_draft>
</output_format>
</step>

<step id="2">
<description>自我評估和錯誤檢測</description>
<input_from_previous>從步驟1獲取的初稿</input_from_previous>
<instructions>
客觀評估初稿，檢查：
1. 技術準確性
2. 邏輯清晰度
3. 完整性
4. 可讀性
5. 代碼正確性
識別需要改進的具體問題
</instructions>
<output_format>
<evaluation_report>
<strengths>[初稿優點]</strengths>
<issues>[發現的問題]</issues>
<improvement_suggestions>[改進建議]</improvement_suggestions>
</evaluation_report>
</output_format>
</step>

<step id="3">
<description>修正和優化</description>
<input_from_previous>從步驟1的初稿和步驟2的評估報告</input_from_previous>
<instructions>
基於評估報告，對初稿進行修正和優化：
1. 修正技術錯誤
2. 改善邏輯結構
3. 增強可讀性
4. 補充缺失內容
5. 優化代碼範例
</instructions>
<final_output_format>
# 最終技術文檔

[經過自我修正和優化的完整技術文檔]

## 修正說明
[說明主要修正項目和改進內容]
</final_output_format>
</step>
</self_correction_chain>
```

## API實作方式

```python
import anthropic
from typing import List, Dict, Any

class ChainComplexPrompts:
    def __init__(self, api_key: str):
        self.client = anthropic.Anthropic(api_key=api_key)
    
    def execute_chain(self, chain_steps: List[Dict[str, Any]]) -> List[str]:
        """
        執行鏈式複雜提示
        
        Args:
            chain_steps: 包含每個步驟的提示和配置的列表
            
        Returns:
            所有步驟的輸出結果列表
        """
        results = []
        previous_output = None
        
        for i, step in enumerate(chain_steps):
            # 構建當前步驟的提示
            current_prompt = self._build_step_prompt(
                step, previous_output, i + 1
            )
            
            # 執行當前步驟
            response = self.client.messages.create(
                model="claude-3-sonnet-20240229",
                max_tokens=step.get('max_tokens', 4000),
                temperature=step.get('temperature', 0.3),
                messages=[{
                    "role": "user",
                    "content": current_prompt
                }]
            )
            
            # 保存結果
            step_output = response.content[0].text
            results.append(step_output)
            previous_output = step_output
            
            # 可選：添加步驟間的延遲
            if step.get('delay'):
                import time
                time.sleep(step['delay'])
        
        return results
    
    def _build_step_prompt(self, step: Dict[str, Any], 
                          previous_output: str, step_number: int) -> str:
        """構建步驟提示"""
        prompt_parts = []
        
        # 添加步驟描述
        if step.get('description'):
            prompt_parts.append(f"<step_{step_number}_description>")
            prompt_parts.append(step['description'])
            prompt_parts.append(f"</step_{step_number}_description>")
        
        # 添加前一步的輸出（如果存在）
        if previous_output:
            prompt_parts.append(f"<previous_step_output>")
            prompt_parts.append(previous_output)
            prompt_parts.append(f"</previous_step_output>")
        
        # 添加當前步驟的輸入
        if step.get('input'):
            prompt_parts.append(f"<current_input>")
            prompt_parts.append(step['input'])
            prompt_parts.append(f"</current_input>")
        
        # 添加指示
        if step.get('instructions'):
            prompt_parts.append(f"<instructions>")
            prompt_parts.append(step['instructions'])
            prompt_parts.append(f"</instructions>")
        
        # 添加輸出格式要求
        if step.get('output_format'):
            prompt_parts.append(f"<output_format>")
            prompt_parts.append(step['output_format'])
            prompt_parts.append(f"</output_format>")
        
        return "\n".join(prompt_parts)
    
    def execute_parallel_chain(self, parallel_steps: List[Dict[str, Any]], 
                              final_step: Dict[str, Any]) -> str:
        """
        執行並行鏈式提示
        
        Args:
            parallel_steps: 可並行執行的步驟列表
            final_step: 整合所有並行結果的最終步驟
            
        Returns:
            最終整合結果
        """
        import asyncio
        
        # 並行執行所有步驟
        parallel_results = asyncio.run(
            self._execute_parallel_steps(parallel_steps)
        )
        
        # 執行最終整合步驟
        final_prompt = self._build_final_step_prompt(
            final_step, parallel_results
        )
        
        response = self.client.messages.create(
            model="claude-3-sonnet-20240229",
            max_tokens=final_step.get('max_tokens', 4000),
            temperature=final_step.get('temperature', 0.3),
            messages=[{
                "role": "user",
                "content": final_prompt
            }]
        )
        
        return response.content[0].text
    
    async def _execute_parallel_steps(self, steps: List[Dict[str, Any]]) -> List[str]:
        """並行執行多個步驟"""
        import asyncio
        
        tasks = []
        for step in steps:
            task = asyncio.create_task(self._execute_single_step(step))
            tasks.append(task)
        
        results = await asyncio.gather(*tasks)
        return results
    
    async def _execute_single_step(self, step: Dict[str, Any]) -> str:
        """執行單個步驟"""
        prompt = self._build_step_prompt(step, None, 0)
        
        response = self.client.messages.create(
            model="claude-3-sonnet-20240229",
            max_tokens=step.get('max_tokens', 4000),
            temperature=step.get('temperature', 0.3),
            messages=[{
                "role": "user",
                "content": prompt
            }]
        )
        
        return response.content[0].text
    
    def _build_final_step_prompt(self, final_step: Dict[str, Any], 
                                parallel_results: List[str]) -> str:
        """構建最終步驟提示"""
        prompt_parts = []
        
        # 添加所有並行結果
        prompt_parts.append("<parallel_results>")
        for i, result in enumerate(parallel_results):
            prompt_parts.append(f"<result_{i + 1}>")
            prompt_parts.append(result)
            prompt_parts.append(f"</result_{i + 1}>")
        prompt_parts.append("</parallel_results>")
        
        # 添加最終步驟指示
        if final_step.get('instructions'):
            prompt_parts.append("<final_instructions>")
            prompt_parts.append(final_step['instructions'])
            prompt_parts.append("</final_instructions>")
        
        return "\n".join(prompt_parts)

# 使用範例
def main():
    # 初始化鏈式提示執行器
    chain_executor = ChainComplexPrompts("your-api-key")
    
    # 定義學術論文分析鏈
    paper_analysis_chain = [
        {
            "description": "提取論文核心信息",
            "input": "[論文內容]",
            "instructions": "請提取論文的標題、作者、研究問題、方法論和主要發現",
            "output_format": "使用 XML 標籤結構化輸出",
            "max_tokens": 2000
        },
        {
            "description": "分析研究方法",
            "instructions": "基於提取的信息，分析研究方法的適當性和有效性",
            "output_format": "提供詳細的方法論評估",
            "max_tokens": 2000
        },
        {
            "description": "生成綜合評估報告",
            "instructions": "結合前兩步的分析，生成完整的學術評估報告",
            "output_format": "Markdown 格式的完整報告",
            "max_tokens": 3000
        }
    ]
    
    # 執行鏈式提示
    results = chain_executor.execute_chain(paper_analysis_chain)
    
    # 輸出結果
    for i, result in enumerate(results):
        print(f"步驟 {i + 1} 結果:")
        print(result)
        print("-" * 50)

if __name__ == "__main__":
    main()
```

## 最佳實踐

<guidelines>
1. **任務分解原則**：
   - 每個子任務應該有明確、單一的目標
   - 避免在單一步驟中包含過多複雜要求
   - 確保子任務之間的邏輯順序清晰

2. **輸入輸出管理**：
   - 使用 XML 標籤結構化傳遞信息
   - 明確定義每個步驟的輸出格式
   - 保持輸出的一致性和可預測性

3. **錯誤處理**：
   - 在每個步驟中包含品質檢查機制
   - 設計自我修正的能力
   - 為關鍵步驟設置驗證標準

4. **效能優化**：
   - 識別可並行執行的獨立子任務
   - 避免不必要的重複處理
   - 合理設置每個步驟的 token 限制

5. **迭代改進**：
   - 監控每個步驟的表現
   - 根據結果調整提示內容
   - 持續優化鏈式結構
</guidelines>

## 常見錯誤

<mistakes>
1. **過度複雜化**：
   - 錯誤：將簡單任務不必要地分解為多個步驟
   - 正確：只對真正複雜的任務使用鏈式提示

2. **步驟依賴性不清**：
   - 錯誤：後續步驟無法有效利用前一步的輸出
   - 正確：確保每個步驟都能清晰地建立在前一步的基礎上

3. **輸出格式不一致**：
   - 錯誤：不同步驟使用不同的輸出格式標準
   - 正確：統一使用 XML 標籤和一致的格式規範

4. **缺乏品質控制**：
   - 錯誤：不對中間結果進行驗證
   - 正確：在關鍵步驟添加自我檢查機制

5. **忽視並行機會**：
   - 錯誤：將所有步驟串行執行
   - 正確：識別並並行化獨立的子任務
</mistakes>

## 測試與優化

<evaluation>
評估鏈式複雜提示的方法：

1. **單步驟測試**：
   - 獨立測試每個子任務的表現
   - 驗證輸出格式的正確性
   - 檢查指示的清晰度

2. **端到端測試**：
   - 測試完整鏈式流程
   - 驗證最終結果的品質
   - 確認信息傳遞的完整性

3. **效能測試**：
   - 測量總執行時間
   - 比較與單一複雜提示的效果
   - 評估並行化的效益

4. **錯誤處理測試**：
   - 測試異常情況的處理
   - 驗證自我修正機制
   - 檢查容錯能力
</evaluation>

<iteration>
迭代改進的方法：

1. **逐步優化**：
   - 從表現最差的步驟開始改進
   - 一次只調整一個步驟
   - 保持其他步驟不變以隔離影響

2. **A/B 測試**：
   - 對比不同的鏈式結構
   - 測試不同的提示措辭
   - 評估不同的輸出格式

3. **用戶反饋整合**：
   - 收集最終用戶的反饋
   - 識別常見的失敗點
   - 根據實際使用情況調整

4. **持續監控**：
   - 追蹤成功率和品質指標
   - 監控執行時間變化
   - 及時發現新的問題模式
</iteration>

## 與其他技術組合

<combinations>
鏈式複雜提示可以與以下技術有效組合：

1. **與思維鏈技術 (Chain of Thought)**：
   - 在每個子任務中使用思維鏈
   - 增強每個步驟的推理能力
   - 提高複雜問題的解決品質

2. **與 XML 標籤系統**：
   - 結構化每個步驟的輸入輸出
   - 確保信息傳遞的準確性
   - 便於調試和維護

3. **與多樣本提示 (Few-shot)**：
   - 在每個步驟中提供相關範例
   - 提高子任務的執行品質
   - 減少格式錯誤

4. **與自我反思技術**：
   - 在鏈式流程中加入自我檢查步驟
   - 提高整體輸出品質
   - 增強錯誤修正能力

5. **與平行工具執行**：
   - 並行執行獨立的子任務
   - 提高整體執行效率
   - 優化資源利用

6. **與輸出格式控制**：
   - 標準化每個步驟的輸出格式
   - 確保鏈式傳遞的一致性
   - 便於後續處理和分析
</combinations>

## 官方參考連結

- [Anthropic 官方文檔 - Chain complex prompts](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-prompts)
- [Anthropic 官方文檔 - Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Anthropic 官方文檔 - Chain of thought prompting](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-of-thought)