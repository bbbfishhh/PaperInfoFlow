# Function_Call

1. google gemini sdk 从 google-generativeai 变成了 google-genai

https://pypi.org/project/google-genai/

所以 pip install google-genai

2. 模型本身不能执行函数工具，而只是理解意图，去调用真正的 tool function

3. 模型也是Agent， 属于人与机械中的中介，更精确直接地理解人的意图并执行

4. 暴论：最好的Agent是模型本身，模型作为“代理”，感知 + 认知 + 行动。


## 大模型为什么能识别到 function_call 意图？

大模型收到 prompt后没有直接回复，而是走向“调用函数”，需要专门的微调：
| 输入 | 期望输出 |
|------|---------|
| 用户问：“帮我查一下今天上海的天气” | `{"function_call": {"name": "get_weather", "arguments": {"location": "上海"}}}` |
| 用户问：“2加3等于几” | `{"function_call": {"name": "add", "arguments": {"a": 2, "b": 3}}}` |

通常还会混合很多“不需要调用函数的对话”，让模型学会何时不用调用函数。

比如， Qwen-QwQ-32B 利用奖励机制整合工具调用。


| 阶段 | 作用 | 是否专注 function_call |
|------|------|----------------------|
| 预训练 | 建立语言和代码理解能力 | ❌（非专门训练） |
| 指令微调 | 学习何时调用函数、怎么调用 | ✅（核心训练方式） |
| RLHF | 优化调用策略的准确性 | ✅（策略调优） |

所以，function_call 的识别能力并不是“规则写死的”，而是通过“训练让模型自己学会判断什么时候调用”，而这种能力的本质就是它的语言推理和意图理解能力的延伸。



## Gemini 执行天气查询的 tool_use 用例

官方文档：https://ai.google.dev/gemini-api/docs/function-calling?hl=zh-cn&example=weather

1. 定义函数声明

就是把写成大模型看得懂的，有规则的函数 String。

主要是三部分，name 、 description、 parameters


```
# Define the function declaration for the model
weather_function = {
    "name": "get_current_temperature",
    "description": "Gets the current temperature for a given location.",
    "parameters": {
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "The city name, e.g. San Francisco",
            },
        },
        "required": ["location"],
    },
}
```

**定义实际函数 :**
```
# This is the actual function that would be called based on the model's suggestion

def get_current_temperature(location):
    """Get the actual temperature from actual application, e.g. 调用 OpenWeatherMap API 获取指定地点的当前天气信息。
    """
    return {"Temperature": , "Weather": }

```

2. 使用上述函数声明，调用模型

模型可能会直接回复，或者返回一个函数调用。

如果调用了函数，响应对象将包含函数调用建议。
```
client = genai.Client(api_key=os.getenv("GEMINI_API_KEY"))
tools = types.Tool(function_declarations=[weather_function])
config = types.GenerateContentConfig(tools=[tools])

user_query = "What's the temperature in ShangHai?"

response = client.models.generate_content(
    model="gemini-2.0-flash",
    contents=user_query, # 发送用户查询
    config=config,
)

print(response.candidates[0].content.parts[0].function_call)
```

3. 执行 get_current_temperature 函数代码

从模型响应返回的对象 function_call 中，提取函数调用详细信息，并真正执行 tool：get_current_temperature 函数。

```
tool_call = response.candidates[0].content.parts[0].function_call

function_args = function_call.args

function_result = get_current_temperature(**function_args)
```

4. 使用函数结果创建用户友好的回答，然后再次调用模型

最后，将函数执行结果发送回模型，以便模型将此信息纳入其对用户的最终回答中。

```
contents_with_tool_result = [
    {"role": "user", "parts": [{"text": user_query}]}, # 用户的原始查询
    {"role": "model", "parts": [first_part]}, # 模型的函数调用响应
    {"role": "tool", "parts": [{"function_response": {"name": function_call.name, "response": function_result}}]} # 函数执行结果
]

response_with_result = client.models.generate_content(
    model="gemini-2.0-flash",
    contents=contents_with_tool_result, # 发送包含历史和结果的消息
    config=config, # 仍然需要提供工具定义，尽管这次模型是根据结果生成回复
)

print(response_with_result.candidates[0].content.parts[0].text)
```

## 那么实际已经用的 function_call 有哪些呢？

以各大模型平台为例子：

Grok、Gemini、Qwen、Chatgpt、DeepSeek

---

### 🌟 **Gemini（Google）**

> **状态：**已经支持 Function Calling（称为 *Tool Use*），用于自动决策是否调用外部工具（API）。

#### ✅ 示例：

1. **天气 API**
   - 用户问：“帮我查一下纽约今天的天气”
   - Gemini 自动调用 `getWeather(location: string)` 工具
   - 返回天气数据后继续生成自然语言答复

2. **日历工具**
   - 用户问：“下周三我有什么安排？”
   - Gemini 通过 Google Calendar 插件获取事件信息

3. **地图搜索**
   - 用户问：“附近有好吃的泰国菜吗？”
   - 自动调用 Google Maps 插件（如 `searchRestaurants(query, location)`）

#### 📌 技术说明：
- 使用 JSON schema 定义工具接口
- 调用通过 API 实现，有内建“判断是否需要调用工具”的逻辑（类似图中蓝线逻辑）

---

### 🧠 **Qwen（通义千问，阿里）**

> **状态：**Qwen-72B 和 Qwen-1.5 模型已开放 Function Call 能力

#### ✅ 示例：

1. **算术 API**
   - 用户输入：“请问 sqrt(144) 是多少？”
   - 返回调用：`{"name": "sqrt", "arguments": {"value": 144}}`

2. **插件系统**
   - 通义 App 集成多个插件工具：如查天气、翻译、单位换算
   - 提供自动识别和调用插件功能

#### 📌 技术说明：
- 使用类似 OpenAI 的 `function_call` schema
- 支持“多轮调用”与“结果后决策下一步调用”

---

### 💬 **ChatGPT（OpenAI）**

> **状态：**Function Calling 是 OpenAI 首创并率先开放的核心功能之一

#### ✅ 示例：

1. **航班搜索**
   - 用户问：“帮我查明天从北京到上海的机票”
   - 模型返回：
     ```json
     {
       "function_call": {
         "name": "search_flight",
         "arguments": {
           "from": "北京",
           "to": "上海",
           "date": "2025-04-30"
         }
       }
     }
     ```

2. **插件调用（旧版）**
   - Wolfram 插件：调用科学计算、图像绘制
   - Zapier 插件：连接 Gmail、Sheets 等服务

3. **代码执行（Code Interpreter）**
   - 内部也是一种“自动调用工具”的方式（例如上传文件后自动处理）

---

### 📘 **DeepSeek（深度求索）**

> **状态：**支持 Tool Call 和 Code Interpreter，重视开发者能力

#### ✅ 示例：

1. **函数调用：**
   - 用户输入数学问题或需要天气、翻译、汇率转换时，DeepSeek 会生成工具调用
   - 工具注册方式类似 OpenAI 的 function schema（JSON 格式）

2. **代码运行器：**
   - 在 DeepSeek-Coder 中嵌入代码执行环境：模型判断是否需要执行某段代码 → 自动运行 → 回传结果 → 再次生成最终答复

---

## 📌 汇总对比表：

| 平台       | 支持 Function Call | 技术名词        | 示例功能                             |
|------------|--------------------|------------------|--------------------------------------|
| Grok       | 🚫 暂无披露        | -                | -                                    |
| Gemini     | ✅ 已支持          | Tool Use         | 天气查询、地图检索、日历事件         |
| Qwen       | ✅ 已支持          | Function Call    | 翻译、单位换算、算术、查天气         |
| ChatGPT    | ✅ 已支持          | Function Call / Tools | 航班搜索、图表绘制、插件操作    |
| DeepSeek   | ✅ 已支持          | Tool Call        | 数学计算、代码执行、插件调用         |

---
