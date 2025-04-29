# Function Call 之 DeepResearch

LLM + 意图识别可以去做 deep Research，比如 Gemini 2.5 pro-exp 可以做 deep Research 特别深入，能涉及上百个网页来源。

而一般普通的联网搜索能力，比如刚上的 QWen3 的联网搜索就是 8 个来源。

## deep research

以下这个老哥讲解的很深入：
如何看待OpenAI最新发布的deep research？ - theigrams的回答 - 知乎
https://www.zhihu.com/question/11164351940/answer/98306087114

Deep Research 是一个 “deep” 的Agent

本质上：

1.搜索引擎 2.web crawler 网页抓取 3.LLM

结合三者，再加上 CoT 多轮迭代


OpenDeepResearch：
https://github.com/dzhng/deep-research


关键就是有多深？
迭代多少轮？

## 国内外对比：

国内好像没有一个大厂在做 ...

https://liuwei.blog/2025/04/03/%E5%9B%BD%E5%86%85%E4%BA%94%E4%B8%AAdeep-research%E5%B7%A5%E5%85%B7%E8%AF%84%E6%B5%8B/

目前国内存在的 deep Research 只有：

研学智得AI：由中国知网开发，专注于学术文献，资源丰富，定价可能与机构订阅相关。

Reportify：专注于投资研究，北京积沙成塔科技有限公司（哈佛老徐），定价未公开，可能适合金融分析师。

智谱AutoGLM沉思：由智谱AI推出的一款集深度研究与实际操作能力于一体的AI智能体。

小白研报：“问小白”的一个功能，由元石科技开发，免费使用，但需要排队申请，适合生成和分析研究报告。

OpenDeepResearcher：开源工具，由mshumer开发，免费但需技术设置，适合技术用户。【实际上是 github上外国开发者】

### 最大的应该是 autoglm 沉思

https://autoglm-research.zhipuai.cn/

autoglm 自己的研究员介绍：
1. 模仿Chatgpt o3 的训练方式，强化学习推理模型，自主规划、动态决策，不需要提前设计工作流

2. 我看了，和国外的 deep research 不同，autoglm 喜欢大量用 browser-use， 基本上都大部分网页第一时间用的是 browser-use。就是 Agent 模仿人看网页一样一个一个看。

优点：Browser-Use 是面向交互与视觉理解的高级浏览器自动化工具，适合 AI Agent 进行类人操作；而传统 Web Crawler 更适合专注于数据提取、批量处理等静态场景。 

缺点：性能开销大，用到视觉ocr等，估计 tokens 消耗也高，最重要的是太慢了，同样时间 web crawler 可以获取的信息量更大。

建议还是以第一性的文本信息为主。



## gemini 使用例子