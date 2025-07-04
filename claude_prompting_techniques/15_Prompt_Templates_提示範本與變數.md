# 15. 提示範本與變數(Prompt Templates)

## 官方定義與說明

<definition>
根據 Anthropic 官方文檔定義，提示範本是一種結合固定內容和變數內容的結構化方法，使用占位符（通常是 {{雙括號}}）來表示可在不同互動中動態變化的元素。範本將靜態指令與動態輸入分離，提供一致的提示結構，同時允許高效的內容替換。
</definition>

## 核心原理

<principle>
提示範本的核心原理是將提示分為兩個部分：
1. 固定內容：保持不變的靜態指令、上下文或結構
2. 變數內容：根據不同請求動態變化的元素，如用戶輸入、檢索內容、對話上下文等

範本使用占位符標記變數位置，在實際使用時替換為具體值。這種分離機制確保了提示的一致性和可重複性，同時提供了靈活的客製化能力。
</principle>

## 官方效益

<benefits>
Anthropic 官方文檔指出提示範本具有以下效益：
1. 確保提示結構的一致性
2. 允許高效的內容替換和測試
3. 簡化提示管理和版本控制
4. 提供易於測試不同輸入的機制
5. 支援規模化應用開發
6. 便於提示的迭代優化和追蹤
7. 分離核心提示邏輯與動態輸入
</benefits>

## 實作格式

### 基本結構
```xml
<template>
  <fixed_content>
    {靜態指令和上下文}
  </fixed_content>
  <variable_content>
    {{variable_name}}
  </variable_content>
</template>
```

### 完整結構
```xml
<system_prompt>
  <role>你是一位專業的{{domain}}專家</role>
  <task>分析用戶提供的{{content_type}}並提供{{output_format}}建議</task>
</system_prompt>

<user_input>
  <context>
    {{context}}
  </context>
  <content>
    {{user_content}}
  </content>
  <requirements>
    {{specific_requirements}}
  </requirements>
</user_input>

<output_format>
  <format>{{desired_format}}</format>
  <length>{{response_length}}</length>
</output_format>
```

## 實作範例

### 範例1：多語言翻譯範本
```xml
<translation_template>
  <system_prompt>
    你是一位專業的翻譯專家，專精於{{source_language}}到{{target_language}}的翻譯。
    請提供準確、自然且符合目標語言文化背景的翻譯。
  </system_prompt>
  
  <user_input>
    <source_text>
      {{text_to_translate}}
    </source_text>
    <context>
      文本類型：{{text_type}}
      目標受眾：{{target_audience}}
      語調要求：{{tone_requirement}}
    </context>
  </user_input>
  
  <output_format>
    <translation>{{translated_text}}</translation>
    <explanation>{{translation_notes}}</explanation>
  </output_format>
</translation_template>
```

### 範例2：程式碼審查範本
```xml
<code_review_template>
  <system_prompt>
    你是一位資深的{{programming_language}}開發者，專精於程式碼審查。
    請根據以下標準進行分析：程式碼品質、效能、安全性、可維護性。
  </system_prompt>
  
  <code_analysis>
    <code_snippet>
      {{code_to_review}}
    </code_snippet>
    <project_context>
      專案類型：{{project_type}}
      程式語言：{{programming_language}}
      團隊規模：{{team_size}}
    </project_context>
  </code_analysis>
  
  <review_criteria>
    <focus_areas>{{review_focus}}</focus_areas>
    <severity_level>{{required_severity}}</severity_level>
  </review_criteria>
</code_review_template>
```

### 範例3：內容審核範本
```xml
<content_moderation_template>
  <system_prompt>
    你是一位專業的內容審核專家。請根據以下政策審核用戶提交的內容。
  </system_prompt>
  
  <content_policy>
    {{content_policy}}
  </content_policy>
  
  <content_to_review>
    <user_content>
      {{user_submitted_content}}
    </user_content>
    <content_metadata>
      平台：{{platform}}
      內容類型：{{content_type}}
      用戶類型：{{user_type}}
    </content_metadata>
  </content_to_review>
  
  <review_output>
    <classification>{{content_classification}}</classification>
    <reasoning>{{moderation_reasoning}}</reasoning>
    <action_required>{{suggested_action}}</action_required>
  </review_output>
</content_moderation_template>
```

## API實作方式

```python
import os
from anthropic import Anthropic

class PromptTemplateManager:
    def __init__(self, api_key=None):
        self.client = Anthropic(api_key=api_key or os.environ.get("ANTHROPIC_API_KEY"))
        self.templates = {}
    
    def create_template(self, name, system_prompt, user_prompt, variables):
        """創建提示範本"""
        template = {
            "system_prompt": system_prompt,
            "user_prompt": user_prompt,
            "variables": variables
        }
        self.templates[name] = template
        return template
    
    def render_template(self, template_name, **kwargs):
        """渲染範本並替換變數"""
        template = self.templates.get(template_name)
        if not template:
            raise ValueError(f"範本 '{template_name}' 不存在")
        
        # 替換系統提示中的變數
        system_prompt = template["system_prompt"]
        for var, value in kwargs.items():
            system_prompt = system_prompt.replace(f"{{{{{var}}}}}", str(value))
        
        # 替換用戶提示中的變數
        user_prompt = template["user_prompt"]
        for var, value in kwargs.items():
            user_prompt = user_prompt.replace(f"{{{{{var}}}}}", str(value))
        
        return system_prompt, user_prompt
    
    def execute_template(self, template_name, model="claude-3-sonnet-20240229", **kwargs):
        """執行範本並獲取回應"""
        system_prompt, user_prompt = self.render_template(template_name, **kwargs)
        
        response = self.client.messages.create(
            model=model,
            system=system_prompt,
            messages=[{"role": "user", "content": user_prompt}],
            max_tokens=1000
        )
        
        return response.content[0].text

# 使用範例
template_manager = PromptTemplateManager()

# 創建翻譯範本
translation_template = template_manager.create_template(
    name="translation",
    system_prompt="""你是一位專業的翻譯專家，專精於{{source_language}}到{{target_language}}的翻譯。
    請提供準確、自然且符合目標語言文化背景的翻譯。""",
    user_prompt="""<source_text>
{{text_to_translate}}
</source_text>

<context>
文本類型：{{text_type}}
目標受眾：{{target_audience}}
</context>

請提供翻譯並簡要說明翻譯選擇的原因。""",
    variables=["source_language", "target_language", "text_to_translate", "text_type", "target_audience"]
)

# 執行範本
result = template_manager.execute_template(
    "translation",
    source_language="英文",
    target_language="繁體中文",
    text_to_translate="The quick brown fox jumps over the lazy dog.",
    text_type="範例句子",
    target_audience="一般讀者"
)

print(result)
```

## 最佳實踐

<guidelines>
1. 變數命名規範：使用清晰、描述性的變數名稱，避免縮寫和模糊命名
2. 結構化組織：將相關變數組織在XML標籤中，增強可讀性
3. 預設值設定：為可選變數提供合理的預設值
4. 驗證機制：實作變數驗證確保輸入符合期望格式
5. 版本控制：為範本建立版本控制系統，追蹤變更歷史
6. 文檔化：為每個範本提供詳細的變數說明和使用範例
7. 測試覆蓋：為範本創建測試案例，確保各種變數組合正常工作
8. 效能考量：避免在範本中使用過多巢狀變數，影響處理效率
</guidelines>

## 常見錯誤

<mistakes>
1. 變數命名衝突：使用相同名稱的變數導致意外替換
   - 錯誤：{{text}} 在多個地方使用但含義不同
   - 正確：{{source_text}} 和 {{target_text}} 明確區分

2. 缺少變數驗證：未檢查變數是否存在或格式正確
   - 錯誤：直接使用 {{undefined_variable}} 導致範本錯誤
   - 正確：實作變數存在性檢查和格式驗證

3. 過度複雜的範本：在單一範本中包含過多邏輯
   - 錯誤：一個範本處理多種完全不同的任務
   - 正確：將複雜任務拆分為多個專門的範本

4. 不當的XML結構：XML標籤不匹配或格式錯誤
   - 錯誤：<context>{{variable}</context> 標籤錯誤
   - 正確：<context>{{variable}}</context> 標籤正確匹配

5. 變數作用域混淆：在錯誤的範本部分使用變數
   - 錯誤：在system_prompt中使用僅適用於user_prompt的變數
   - 正確：確保變數在正確的範本部分使用
</mistakes>

## 測試與優化

<evaluation>
1. 變數覆蓋測試：測試所有變數組合，確保範本在各種情況下正常工作
2. 效能測試：評估範本渲染和執行速度，特別是對於大量變數的情況
3. 輸出品質測試：比較不同變數值對輸出品質的影響
4. 邊界條件測試：測試空值、特殊字符、過長內容等極端情況
5. 使用者接受度測試：收集實際使用者對範本輸出的反饋
</evaluation>

<iteration>
1. 監控範本使用情況：追蹤最常使用的變數組合和範本
2. 收集用戶反饋：定期收集使用者對範本效果的評價
3. A/B測試：比較不同範本版本的效果差異
4. 效能優化：基於使用數據優化範本結構和變數設計
5. 持續改進：根據新的使用案例和需求更新範本
</iteration>

## 與其他技術組合

<combinations>
1. 與多樣本提示結合：在範本中包含{{examples}}變數，支援動態範例
2. 與思維鏈結合：使用{{reasoning_steps}}變數控制推理過程
3. 與輸出格式控制結合：通過{{output_format}}變數指定回應格式
4. 與角色提示結合：使用{{role}}和{{expertise}}變數定義角色特徵
5. 與鏈式提示結合：在複雜工作流程中使用範本作為各個階段的標準化輸入
6. 與工具使用結合：在範本中包含{{tool_params}}變數配置工具使用
7. 與上下文管理結合：使用{{context}}變數動態調整上下文內容
8. 與安全性控制結合：通過{{safety_guidelines}}變數加強安全性檢查
</combinations>

## 官方參考連結

- [Anthropic 官方提示範本文檔](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-templates-and-variables)
- [Anthropic 提示生成器](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-generator)
- [Anthropic 提示庫](https://docs.anthropic.com/en/resources/prompt-library/library)
- [Anthropic 提示工程概覽](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)