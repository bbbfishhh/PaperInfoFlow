# 【论文速读】分层提示词分类

分层提示分类：一种与人类认知原则一致的大型语言模型通用评估框架

Hierarchical Prompting Taxonomy


## 总结先行

老实说，没太看明白（笑哭），求助知乎，b站也没有人读过。以我浅显的认知理解：
提示词分层：

【Q】:为啥提示词要分层？
【A】：应用人类认知的四大过程。为了杀鸡不用牛刀，什么层次的问题对应什么层次的提示词就够。

1. 进一步解决幻觉问题  2. 大模型普惠全民的工程问题，如何又好又便宜



| 简化版表述       | 原文对应内容                                                                                                                                                                                                 |
|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1. 记住事实      | **Basic Recall and Reproduction**：<br>"基本回忆和再现：在没有分析的情况下记忆和再现事实信息，仅涉及从记忆中识别或检索知识（Anderson等，2014）"                                                              |
| 2. 理解意思      | **Understanding and Interpretation**：<br>"理解和解释：用自己语言解释信息，总结要点或澄清意义（Bloom 1956第二层级），需要信息处理而非单纯检索"                                                             |
| 3. 分析规律      | **Analysis and Reasoning**：<br>"分析和推理：比较、对比、理解深层原则（Bloom分析阶段），需审视结构并识别模式与联系"                                                                                        |
| 4. 解决问题      | **Application of Knowledge and Execution**：<br>"知识应用与执行：完成多步骤任务、解决复杂问题（Bloom应用与评估阶段），需综合信息并实践决策"                                                                |

## 本文的5种prompting
1. Role Prompting：简单角色扮演，你问我答In&Out
对应论文：
	Kong A, Zhao S, Chen H, et al. Better zero-shot reasoning with role-play prompting[J]. arXiv preprint arXiv:2308.07702, 2023.

2. Zero-shot Chain of Thought prompting：零样本思维链条
【就是不给你例子，自己去显式生成中间推理步骤 step by step】
Kojima T, Gu S S, Reid M, et al. Large language models are zero-shot reasoners[J]. Advances in neural information processing systems, 2022, 35: 22199-22213.

3. Three-shot Chain of Thought prompting：三样本思维链条
【给你三个例子，可以去模仿示例生成推理链了】
Wei J, Wang X, Schuurmans D, et al. Chain-of-thought prompting elicits reasoning in large language models[J]. Advances in neural information processing systems, 2022, 35: 24824-24837.

4. Least to Most prompting【就是通过“分而治之”策略克服CoT的简单】
Least-to-Most Prompting（渐进式分解提示法）是一种通过问题拆解与链式推理解决复杂任务的提示策略。其核心思想是：将难题分解为多个简单子问题，按顺序逐步解决，每个子问题的答案作为后续步骤的基础，最终组合出全局解决方案。“分步拆解→链式解决”的机制，中间结合COT
Least-to-most prompting enables complex reasoning in large language models
Zhou D, Schärli N, Hou L, et al. Least-to-most prompting enables complex reasoning in large language models[J]. arXiv preprint arXiv:2205.10625, 2022.

5. Generated Knowledge Prompting (GKP) 【就是先搜资料再回答】
Generated knowledge prompting for commonsense reasoning
Liu J, Liu A, Lu X, et al. Generated knowledge prompting for commonsense reasoning[J]. arXiv preprint arXiv:2110.08387, 2021.


![](https://fastly.jsdelivr.net/gh/bucketio/img8@main/2025/03/07/1741341962785-dad0a271-cc09-45a9-8ea2-6839baa93d6c.png)

**对应的其实就是人类认知里面的 快思考（系统1） 和 慢思考（系统2）**
原文翻译：
基于其认知复杂性，反映人类分配认知资源的方式。对于低认知需求的任务，HPF使用与System1思维平行的简单提示。这些任务，如事实回忆或基本分类，需要最少的推理，允许LLM快速高效地响应而无需大量计算。例如，让一个LLM“识别国家的首都”，类似于一个人使用System1检索熟悉的事实。

相比之下，认知需求高的任务涉及引导LLM进行复杂推理、抽象或多步骤问题解决的提示——类似于系统2思维。例如生成逻辑论证或解决复杂问题，需要刻意和资源密集的过程。正如当一个问题超出系统1的能力时会激活系统2一样，更高水平的HPF被调用以完成需要更深入分析的任务。

HPF明确地用HPI来衡量这种转变，评估每个任务所需的认知负荷。通过根据任务复杂性定制提示策略，HPF优化了LLM表现，就像人类根据情况在系统1和系统2之间进行适应性切换一样。这种并行突出了HPT如何将计算策略与类似人类的认知模型相结合，从而实现更细致的任务评估和资源分配。

数据集：
MMLU, GSM8k, HumanEval, BoolQ, CSQA, SamSum, and IWSLTen-fr covering areas such as reasoning, coding, mathematics, question-answering, summarization, and machine translation, to evaluate the framework’s robustness and applicability.
数据集也是很不错的之后去看看，囊括了基本要解决的所有问题。


## 通义千问翻译：
评估大型语言模型 (LLMs) 在执行不同任务中的有效性对于理解它们的优缺点至关重要。本文提出了层次提示分类法 (HPT)，它基于人类认知原则，旨在通过考察各种任务的认知需求来评估LMS。HPT运用了层次激励框架 (HPF)，该框架根据其对LLMs的认知需求，相对于人类的心理能力，按层次顺序构建了五种独特的激励策略。它使用分层激励指数 (HPI) 评估任务的复杂性，这展示了LLMs在不同数据集上的认知能力，并提供了对数据集对不同LLMs的认知需求的见解。这种方法能够全面评估lm的问题解决能力和数据集的复杂性，为任务复杂性提供标准化的度量。对多个数据集和LLLMS进行的广泛实验表明，与基准性能相比，HPF将LLM性能提高了2%-63%。GSM8k是感知和编码任务中最复杂的任务，平均HPI为3.20，确认了HPT的有效性。为了支持该领域未来的研究和再现性，HPT和HPF的实现可在这里获得。
这篇论文介绍了一种名为“层次提示分类法”的方法，用于评估大型语言模型在不同任务中的表现。该方法基于人类认知原则设计，并使用五个独特的提示策略构建了一个层次化的提示框架，以评估语言模型的认知需求和能力。
通过计算每个任务的层次化提示指数（HPI），可以确定其复杂性和数据集对不同语言模型的影响。实验结果表明，该方法能够提高语言模型的表现，为未来的相关研究提供了支持和标准化的指标。
本文的主要贡献如下：


分层提示分类法（HPT）：本文提出了一种规则集，将提示策略映射到人类认知原则，从而为LLMs提供了一个通用的任务复杂度衡量标准。
分层提示框架（HPF）：该框架旨在从五个不同的提示策略中选择最有效的提示，以优化LLMs在任务完成过程中的认知负荷。这个框架不仅有助于更准确地评估LLMs，而且还能提高它们的性能，提供更加透明的见解。
分层提示指数（HPI）：HPI1定量评估了LLMs在各种数据集上的任务复杂性，为每个任务对不同LLMs的认知需求提供了见解。
HPF可以有效地与 “开卷” 考试进行比较，如图2所示，其中问题代表任务，教科书作为提示策略。在这个类比中，试题的复杂性各不相同，从简单的事实回忆到复杂的分析性问题，类似于HPT中的任务，这些任务是根据认知需求进行评估的。同样地，教科书为解决这些问题提供了结构化的指导，就像HPF在不断增加的复杂性中组织提示以支持LLMs一样。例如，一个简单的术语表查找对应于一个低复杂度的任务，而解决一个需要综合概念的多步分析问题则代表一个高复杂度的任务。学生回答问题所投入的努力反映了HPI，它测量了lm的认知负荷。正如学生在结构化资源 (如教科书) 中表现更好一样，LLMs通过精心设计的分层提示策略来提高效率，使他们能够有效地处理逐步复杂的任务。
本文的其余部分结构如下：第2节回顾了LLMs中提示和评估的相关工作。第3节详细介绍了HPT及其相关框架。第4节概述了实验设置、结果和消融研究。第5节总结了论文。第6节讨论了这项工作的伦理影响。 
图2：将HPF与“开卷考试”方法论进行类比的框架。该图表展示了HPF组件（下方）如何映射传统教育评估元素（上方），任务复杂度级别、资源利用（提示/教科书）和性能指标（HPI/学生努力程度）之间存在平行关系。这种比较表明，LLM任务复杂性与教育评估复杂性相似，从简单的查找任务到复杂的综合问题。 

![](https://fastly.jsdelivr.net/gh/bucketio/img8@main/2025/03/07/1741342129615-1da40052-6a9c-4bb8-a453-0c83b158f228.png)
结果就是好，颜色越深提升效果越好。


![](https://fastly.jsdelivr.net/gh/bucketio/img10@main/2025/03/07/1741342142684-c185ed36-6a2d-4e7c-a2a2-c689c6904675.png)

提出一个新的HPI指数来表示大模型处理任务的复杂程度，越小越好。
【即】LLM无需调用深层推理模块，直接通过模式匹配即可解决（类似“条件反射”）。  
A lower HPI for a dataset suggests that the corresponding LLM is more adept at solving the task with fewer cognition processes.
来自deepseek的解释：


### **一句话解释**  
**HPI（Human Processing Index）值越低** → **任务认知需求越简单** → **大模型（LLM）解决问题所需“脑力步骤”越少，表现更高效**。  

---

### **逐层拆解**  
1. **HPI是什么？**  
   - 假设的**认知复杂度评分指标**，用于量化某个任务在HPT框架下对认知资源的需求程度。  
   - **HPI计算逻辑**：任务若需要更多高阶认知操作（如分析、综合），则HPI值更高。  

2. **“Lower HPI”的含义**  
   - **低分场景**：任务仅需基础认知操作（如记忆、简单理解）。  
   - **高分场景**：任务需复杂推理（如多步分析、跨领域知识应用）。  

3. **为何低HPI对应模型更高效？**  
   - **认知负荷低**：LLM无需调用深层推理模块，直接通过模式匹配即可解决（类似“条件反射”）。  
   - **示例对比**：  
     - **低HPI任务**：填空题“中国的首都是__”（只需记忆检索）。  
     - **高HPI任务**：应用题“如何用经济模型解释北京的城市化进程？”（需分析、知识整合）。  
   - **模型表现**：低HPI任务中，LLM响应更快且准确率更高。  

---

### **与HPT框架的关联**  
| HPT认知层级      | 典型HPI值 | 模型处理方式                  |  
|------------------|-----------|------------------------------|  
| **回忆/再现**    | 低HPI     | 直接提取训练数据中的答案模式   |  
| **理解/解释**    | 中低HPI   | 简单语义匹配与重组            |  
| **分析/推理**    | 中高HPI   | 激活逻辑推理模块               |  
| **应用/创新**    | 高HPI     | 多模块协同+外部知识调用        |  

---

### **实践意义**  
- **任务设计**：若希望LLM高效运行，应通过提示工程降低任务的HPI值（如拆解复杂问题为子问题链）。  
- **模型选择**：低HPI任务可用轻量模型（如TinyLlama），高HPI任务需大参数量模型（如GPT-4）。  

**案例**：  
- **低HPI优化**：将开放式问题“分析全球变暖的影响”改为选择题（选项限制认知路径）。  
- **高HPI需求**：学术论文写作需保持高HPI以激发LLM的深度推理能力。  

---
