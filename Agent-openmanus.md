# AI Agent 测玩

## 场景1 

```python
$ python main.py
INFO     [browser_use] BrowserUse logging setup complete with level info
INFO     [root] Anonymized telemetry enabled. See https://docs.browser-use.com/development/telemetry for more information.
Enter your prompt: webbrowse a youtube video called stella the labrador
2025-03-21 16:51:19.414 | WARNING  | __main__:main:15 - Processing your request...
2025-03-21 16:51:19.415 | INFO     | app.agent.base:run:140 - Executing step 1/20     
2025-03-21 16:51:19.414 | WARNING  | __main__:main:15 - Processing your request...
2025-03-21 16:51:19.415 | INFO     | app.agent.base:run:140 - Executing step 1/20
2025-03-21 16:51:23.690 | INFO     | app.llm:update_token_count:250 - Token usage: Input=1490, Completion=13, Cumulative Input=1490, Cumulative Completion=13, Total=1503, Cumulative Total=1503  
2025-03-21 16:51:23.691 | INFO     | app.agent.toolcall:think:80 - ✨ Manus's thoughts:
2025-03-21 16:51:23.692 | INFO     | app.agent.toolcall:think:81 - 🛠️ Manus selected
1 tools to use
2025-03-21 16:51:23.693 | INFO     | app.agent.toolcall:think:85 - 🧰 Tools being prep
ared: ['browser_use']
2025-03-21 16:51:23.694 | INFO     | app.agent.toolcall:think:88 - 🔧 Tool arguments: {"action":"web_search","query":"stella the labrador youtube video"}
2025-03-21 16:51:23.695 | INFO     | app.agent.toolcall:execute_tool:179 - 🔧 Activating tool: 'browser_use'...
2025-03-21 16:51:27.366 | INFO     | app.tool.web_search:_try_all_engines:104 - 🔎 Attempting search with Google...
2025-03-21 16:51:57.640 | INFO     | app.agent.toolcall:act:149 - 🎯 Tool 'browser_use' completed its mission! Result: Observed output of cmd `browser_use` executed:
Searched for 'stella the labrador youtube video' and navigated to first result: https://www.youtube.com/watch?v=Tu3HN-MmJc4
All results:https://www.youtube.com/watch?v=Tu3HN-MmJc4
https://www.youtube.com/@DogNamedStella
https://www.youtube.com/watch?v=s_RmXuV2Sqs
https://www.youtube.com/watch?v=5PPF9I8zcOQ
https://www.youtube.com/watch?v=X5zFJCD-LGY
https://www.youtube.com/watch?v=FXY0HQyJFcU
https://www.youtube.com/watch?v=AYYNv9YKHM4&pp=0gcJCfcAhR29_xXO
https://www.youtube.com/watch?v=-hLRse7cdKI&pp=ygUMI2RvZ2lubGVhdmVz
https://www.youtube.com/watch?v=_HR2XnYshrQ
https://www.youtube.com/watch?v=pSMSQ2xpQiE
2025-03-21 16:51:57.643 | INFO     | app.agent.base:run:140 - Executing step 2/20
---
error
```
tu1

## 场景2 搜集旅行信息
```
Failed to extract content: RetryError[<Future at 0x1de7ba0fef0 state=finished raised BadRequestError>]
# 大多数网站都提取失败

2025-03-21 17:04:20.793 | INFO     | app.agent.base:run:140 - Executing step 12/20
2025-03-21 17:04:24.128 | INFO     | app.llm:update_token_count:250 - Token usage: Input=9626, Completion=30, Cumulative Input=63094, Cumulative Completion=929, Total=9656, Cumulative Total=64023
2025-03-21 17:04:24.131 | INFO     | app.agent.toolcall:think:80 - ✨ Manus's thoughts: The conten
t extraction failed again. I will try scrolling down the page to see if that helps.

2025-03-21 17:04:24.132 | INFO     | app.agent.toolcall:think:81 - 🛠️ Manus selected 1 tools to  
use
2025-03-21 17:04:24.134 | INFO     | app.agent.toolcall:think:85 - 🧰 Tools being prepared: ['bro 
wser_use']
2025-03-21 17:04:24.135 | INFO     | app.agent.toolcall:think:88 - 🔧 Tool arguments: {"action":"scroll_down","scroll_amount":500}
2025-03-21 17:04:24.136 | INFO     | app.agent.toolcall:execute_tool:179 - 🔧 Activating tool: 'browser_use'...
2025-03-21 17:04:24.152 | INFO     | app.agent.toolcall:act:149 - 🎯 Tool 'browser_use' completed its mission! Result: Observed output of cmd `browser_use` executed:
Scrolled down by 500 pixels
2025-03-21 17:04:24.154 | INFO     | app.agent.base:run:140 - Executing step 13/20
2025-03-21 17:04:29.020 | INFO     | app.llm:update_token_count:250 - Token usage: Input=9922, Completion=47, Cumulative Input=73016, Cumulative Completion=976, Total=9969, Cumulative Total=73992
2025-03-21 17:04:29.021 | INFO     | app.agent.toolcall:think:80 - ✨ Manus's thoughts: Now that I've scrolled down, I'll try extracting the content again.

2025-03-21 17:04:29.022 | INFO     | app.agent.toolcall:think:81 - 🛠️ Manus selected 1 tools to use
2025-03-21 17:04:29.023 | INFO     | app.agent.toolcall:think:85 - 🧰 Tools being prepared: ['bro wser_use']
2025-03-21 17:04:29.023 | INFO     | app.agent.toolcall:think:88 - 🔧 Tool arguments: {"action":"extract_content","goal":"Extract restaurant names, locations, types of food, and other relevant details for creating a Tokyo food map."}
2025-03-21 17:04:29.024 | INFO     | app.agent.toolcall:execute_tool:179 - 🔧 Activating tool: 'browser_use'...


2025-03-21 17:04:54.051 | INFO     | app.agent.toolcall:act:149 - 🎯 Tool 'browser_use' completed its mission! Result: Observed output of cmd `browser_use` executed:
Searched for 'popular tokyo dishes' and navigated to first result: https://www.japan-guide.com/e/e3075.html
All results:https://www.japan-guide.com/e/e3075.html
https://www.reddit.com/r/Tokyo/comments/o59tim/what_foods_does_tokyo_do_exceptionally_well/      
https://www.japancentre.com/en/page/156-30-must-try-japanese-foods?srsltid=AfmBOoqKzB0WfT1aEv3LgjHwD7kos9JCI64uAzmFUMQK0ZKUkfNzMoxA
https://guide.michelin.com/us/en/article/travel/inspector_must_have_dishes_tokyo_en
https://www.byfood.com/blog/travel-tips/japanese-traditional-foods
https://www.tripadvisor.com/Articles-l9FJq60Wj8m0-Tokyo_food.html
https://www.gotokyo.org/en/see-and-do/drinking-and-dining/tokyo-local-food/index.html
https://www.quora.com/What-are-some-must-try-foods-and-snacks-in-Tokyo-Japan-that-offer-a-unique-experience-or-serve-a-special-purpose
https://www.recipetineats.com/top-10-best-foods-to-eat-in-tokyo/
https://girleatworld.net/tokyo-food-guide/

2025-03-21 17:05:56.582 | INFO     | app.agent.base:run:140 - Executing step 19/20
2025-03-21 17:06:01.595 | INFO     | app.llm:update_token_count:250 - Token usage: Input=15191, Completion=280, Cumulative Input=152196, Cumulative Completion=1687, Total=15471, Cumulative Total=153883
2025-03-21 17:06:01.599 | INFO     | app.agent.toolcall:think:80 - ✨ Manus's thoughts: The conten
t extraction continues to fail. Since I am having trouble extracting the information automatically, I will manually create a report based on the search results.

**Tokyo Food Guide**

Based on the search results, here are some popular Tokyo dishes:

*   **Sushi:** A classic Japanese dish consisting of vinegared rice and various toppings, such as seafood and vegetables.
*   **Ramen:** A noodle soup dish with a rich broth, noodles, and toppings like pork, eggs, and seaweed.
*   **Tempura:** Deep-fried seafood and vegetables.
*   **Yakitori:** Grilled chicken skewers.
*   **Udon:** Thick wheat flour noodles, often served in a broth with various toppings.
*   **Soba:** Thin buckwheat noodles, served hot or cold.
*   **Okonomiyaki:** A savory pancake containing various ingredients.
*   **Monjayaki:** A pan-fried batter dish similar to okonomiyaki.
*   **Unagi:** Grilled eel.
*   **Tonkatsu:** Breaded and deep-fried pork cutlet.

I am unable to provide specific restaurant recommendations or a detailed map due to the limitations of the content extraction tool. However, these dishes are a great starting point for exploring the diverse culinary scene in Tokyo.

I will now terminate the interaction.
```

五张图

## 总结
- 太费token了，token粉碎机。一次web-browse甚至能70w tokens
- 基础的文本返回，没有成功过
- 给的 prompt 没做到正常拆解
  - 比如要求至多3个网站（within 3 websites），但是强行执行了十几个
  - 比如说 google 搜索换成 bing 或者百度，必须用代码，prompt没办法执行
- 固定报错：多个模型投入，但是报错显示没法回溯是哪一个。并且是哪一段
```
Authentication failed. Check API key.
2025-03-21 15:08:14.723 | ERROR    | app.llm:ask_tool:708 - OpenAI API error: Error code: 401 - {'error': {'message': 'Authentication Fails (no such user)', 'type': 'authentication_error', 'param': None, 'code': 'invalid_request_error'}}
```
- 官方 Github 给了唯一的实例，prompt：run a through SEO audit report of Andrej Karpathy(https://karpathy.ai/)
1. 给了具体的网站网址
2. 具体的任务
3. 返回了简单的信息（用ai爬虫也差不多

- 期待后续的黑客松能尽快有推进吧

#### 群友：
Token usage: Input=8079, Completion=426, Cumulative Input=339688, Cumulative Completion=9411, Total=8505, Cumulative Total=349099

349099 + 339688 = 35w*2 = 70w

程序跑完 第一个任务 开发坦克大战游戏 只出一个方块网页 没有操作逻辑 

第二个任务做一个app 跑完只是打开浏览器 搜索 然后读取内容 持续尝试 20步之后 没有生成任何文件。
生成文件失败，基础的文字输出也失败了。

## Manus 官方用例
1. 旅行信息搜索并推荐

图1

2. YC 公司列表搜集

图2

## 字节 UI-TARS 官方用例
1. Agent 自己谷歌搜天气并且返回文字 
2. Agent 自己登录 twitter 并且发送 ”hello world“ 
两图