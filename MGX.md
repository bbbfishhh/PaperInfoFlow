# 用 MGX 一键做了个 APP


## 总结先行

- 适用于前端页面设计，小游戏生成，小型APP设计
- 功能还是要迭代，一次生成错误很多，好在它可以人为调整
- 基于 claude-sonnet，credits 基本能免费让你生成一次

一句话 Prompt 自动通过多 Agent 生成了一个微习惯培养的 MVP

Prompt 如下：

Create a prototype for an app focused on developing micro-habits with the following requirements:（根据以下要求做一个培养微习惯的模型）
1. Minimalist design（极简风格设计）
2. A gamified experience similar to Duolingo（模仿多邻国的游戏设计模式）
3. Micro-habit formation based on the Fogg Behavior Model（基于fogg行为模型设计微习惯范式）



结果展示表示：
1. 经验条--符合游戏设计
2. 设计习惯 + 添加锚定时刻 + 微小庆祝 -- 符合福格行为模型
3. 风格极简

缺点：
1. 第一次生成功能缺失：比如经验条不动，新习惯添加失败

## 流程如下：

### Agent Mike Team Leader 负责分配任务

### Agent Emma Product Manager 
负责写 PRD 产品需求文档（.md .mermaid）

1. 理解 B = MAP 行为模型设计
- CoT
- Reviewed 14 web pages 查阅了 14 个网页
- 生成

2. 然后去查阅理解了 多邻国重复上述步骤
3. 又查阅理解了 Minimalist Design重复上述步骤
4. 最后把生成的 文档位置，解决的任务告诉了 MIKE




### Agent Bob Architect

#### 负责生成 system-design-doc
- 技术栈选取 + 开源框架
- 数据结构和接口
- 程序调用流



## Agent Alex Engineer

Mike： Please develop a prototype of the micro-habits app using React, JavaScript, and Tailwind CSS based on the system design created by Bob. The system architecture documents are located at:

- System Design Document: '/data/chats/i6ki8/workspace/docs/micro_habits_app_system_design.md'
- Class Diagram: '/data/chats/i6ki8/workspace/docs/micro_habits_app_class_diagram.mermaid'
- Sequence Diagram: '/data/chats/i6ki8/workspace/docs/micro_habits_app_sequence_diagram.mermaid'

The PRD is located at '/data/chats/i6ki8/workspace/docs/micro_habits_app_prd.md'.

Please focus on implementing the minimalist design, gamification features similar to Duolingo, and habit formation based on the Fogg Behavior Model as specified in these documents. Create a functional prototype that demonstrates the core features of the app.@ Alex

最后在 MGX 官方的远程虚拟环境中运行

## 看看其他有趣的应用：


### oiiai meme cat

### MBTI show



### CRM System
