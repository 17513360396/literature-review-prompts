# 📚 文献综述 AI Prompt 高推荐指数合集

> 本合集从 GitHub 上多个高星学术写作 Prompt 仓库中搜集整理，专注于**毕业论文/期刊论文文献综述**场景。
> 收录来源均为 GitHub 上经过社区验证的高质量项目，涵盖文献检索、论文筛选、综述框架设计、主体撰写、质量检查全流程。

---

## 📖 收录来源

| 来源仓库 | Stars | 特点 | 协议 |
|----------|-------|------|------|
| [ahmetbersoz/chatgpt-prompts-for-academic-writing](https://github.com/ahmetbersoz/chatgpt-prompts-for-academic-writing) | ~4,600+ | 英文学术写作全流程，含 Literature Review Generator 定制GPT | - |
| [bohyy/academic-ai-prompt](https://github.com/bohyy/academic-ai-prompt) | ~900+ | 中文研究生专用，89+ Prompt完整覆盖选题→写作→发表 | MIT |
| [ChenZiHong-Gavin/paper-prompt](https://github.com/ChenZiHong-Gavin/paper-prompt) | ~58 | 中文论文全生命周期，含文献调研/综述/审稿回复 Prompt | - |
| [drshahizan/Generative-AI-Playground](https://github.com/drshahizan/Generative-AI-Playground) | 持续维护 | 20+文献综述专用Prompt，覆盖全学术写作流程 | - |
| [ccastilloz/chatgpt-prompts](https://github.com/ccastilloz/chatgpt-prompts) | - | 英文学术写作Prompt合集，含文献综述多场景 | - |

---

## 一、文献检索与论文筛选

### 1.1 确定研究领域顶级期刊与会议

> 来源：bohyy/academic-ai-prompt

```
我的研究方向是：[你的研究方向/选题]

我需要你帮我找出这个领域最权威的期刊和会议。

请给我：

【第一部分：顶级期刊（International Level）】
列出5-8个该领域的顶级国际期刊，包括：
- 期刊名称（中英文）
- 影响因子（IF）或类似指标
- 主要关注的研究方向
- 出刊周期
- 审稿周期
- 该期刊在领域中的地位和特点

【第二部分：优质期刊（High Quality）】
列出5-8个该领域的优质但可能稍次于顶级的期刊

【第三部分：顶级会议（A类会议）】
列出3-5个该领域最重要的国际会议，包括：
- 会议名称（中英文 + 缩写）
- 举办地点和周期
- 接收论文方向
- 影响力（H-index或引用情况）
- 录取率

【第四部分：选择建议】
根据我的研究方向，推荐我应该重点关注的3-5个期刊/会议，并说明理由
```

### 1.2 生成论文搜索策略

> 来源：bohyy/academic-ai-prompt

```
我的研究选题是：[你的选题名称]

请帮我制定一个搜索策略，包括：

【第一部分：核心关键词】
列出10-15个最相关的核心关键词/短语（英文）：
- 3-5个最核心的关键词
- 5-7个相关但稍次要的关键词
- 2-3个可能相关的拓展关键词
并标注每个关键词的中文翻译和常见同义词

【第二部分：搜索式组合】
给我5-8个具体的搜索式，可以直接复制到Google Scholar：
例如：
- 搜索式1："[关键词1]" AND "[关键词2]"
- 搜索式2：[关键词3] OR [关键词4]

【第三部分：时间范围建议】
- 应该查最近几年的论文？
- 是否需要查经典论文（5-10年前）？

【第四部分：筛选标准】
找到论文后，用什么标准判断是否值得读？
- 期刊/会议等级
- 引用次数
- 作者知名度
```

### 1.3 快速筛选相关论文（评分系统）

> 来源：bohyy/academic-ai-prompt

```
我找到了一些论文，需要快速判断哪些值得深入阅读。

请给我一个"论文筛选清单"：

【筛选标准1：来源质量】
☐ 期刊/会议是否是Top期刊/会议？
☐ 作者是否来自知名研究机构？
☐ 作者是否是该领域的知名学者？

【筛选标准2：相关性】
☐ 题目和摘要是否直接相关我的研究？
☐ 是否解决了我关心的问题？

【筛选标准3：影响力】
☐ 被引用的次数多吗？
☐ 发表时间是最近的吗？

【快速评分系统】
根据以上标准给论文评分（满分10分）：
- 9-10分：必读，深入阅读
- 7-8分：应该读，了解观点
- 5-6分：可以读，补充知识
- <5分：可能不需要重点关注
```

### 1.4 学术检索与综述助手（System Prompt）

> 来源：ChenZiHong-Gavin/paper-prompt

```
# 角色定义：学术检索与综述助手
当用户要求搜索"相关论文（Related Works）"、"文献综述"或提供选题时，
请扮演资深学术导师，遵循以下规范进行检索与输出。

# 概念对齐
- 核心/基线 (Baseline)：该领域开创性、高被引或被广泛对比的经典文献。
- 前沿/SOTA：近 3-5 年内，发表在顶会/顶刊上的最新进展。
- 对标分析：对比已有文献与用户选题在研究对象、方法或理论上的异同，
  寻找"研究空白（Research Gap）"。

# 响应结构
## 1. 核心对标与必读推荐 (Top 3-5)
- [年份] 标题 | 作者 | 期刊/会议名称 (附带链接)
- 核心贡献：一句话概括其使用了什么方法，发现了什么。
- 对标与启示：说明这篇论文与用户选题的相似度，以及为什么必须优先阅读。

## 2. 结构化文献库（分类展示）
按"理论/方法/应用"或"时间线"进行分类，推荐使用 Markdown 表格：
| 论文标题 | 年份 | 研究类型/方法 | 核心发现 | 相关性评分(1-5) |

## 3. 研究空白与阅读建议
- 研究空白：总结当前领域还有哪些未解决的问题
- 阅读路径：给出从浅入深的阅读顺序建议

# 检索规则
1. 宁缺毋滥：必须推荐真实存在的文献，严禁捏造（幻觉）
2. 灵活变通：当找不到完全匹配的论文时，主动推荐方法论相似的跨界启发文献
```

---

## 二、综述框架设计

### 2.1 为综述设计最优框架

> 来源：bohyy/academic-ai-prompt

```
我要写一篇关于[你的研究领域/具体选题]的文献综述。

我已经收集了[50/100]篇论文，主要涉及以下几个方面：
- 方面1：[描述]
- 方面2：[描述]
- 方面3：[描述]

请帮我设计一个最优的综述框架，要求：
1. 逻辑清晰、层次分明
2. 能够清楚地展示该领域的发展脉络
3. 容易突出创新点和研究空白
4. 适合期刊发表（3000-8000字）

请给出：
- 综述的核心框架（5-7个主要部分）
- 每个部分的内容安排
- 各部分之间的逻辑关系
- 预计字数分配
```

### 2.2 分析论文之间的关系

> 来源：bohyy/academic-ai-prompt

```
请帮我分析这[数量]篇论文之间的关系和演进过程。

论文列表：
1. [论文1] - [作者] - [年份] - [核心内容]
2. [论文2] - [作者] - [年份] - [核心内容]
[...]

请识别出：
1. 哪些论文是该领域的开创性工作？
2. 研究方向的主要演进路线？
3. 存在的研究空白和机会？
4. 不同研究方向之间的联系？
5. 按照论文之间的逻辑关系重新排序

最后给出一个推荐的综述框架，使得论文的介绍顺序能够最好地展现该领域的发展脉络。
```

### 2.3 识别关键论文和研究分支

> 来源：bohyy/academic-ai-prompt

```
我有一个研究领域的论文列表。请帮我：

1. 识别该领域的关键论文（必读文献）
   - 开创性工作
   - 被广泛引用的论文
   - 最新的突破性成果

2. 将论文分组成不同的研究分支
   每个分支应该代表该领域的一个重要研究方向

3. 为每个分支命名并简要说明其研究内容

4. 指出各分支之间的关系和融合点

请以表格形式输出：
| 分支名称 | 关键论文 | 论文数量 | 主要内容 | 与其他分支的关系 |
```

### 2.4 生成综述大纲

> 来源：bohyy/academic-ai-prompt

```
基于以下信息，为我生成一个详细的文献综述大纲：

研究领域：[描述]
研究问题：[你要解决的主要问题]
已有论文：[数量]篇
目标期刊/出版物：[期刊名称或学位论文]
字数要求：[如5000字]

请生成一个包含以下内容的详细大纲：
1. 绪论（介绍研究背景、问题和意义）
2. 主体部分（按研究分支组织，每个小节清晰）
3. 存在的问题与挑战
4. 未来研究方向
5. 结论

要求：
- 每个二级标题下要有三级标题
- 标注每个部分的预计字数
- 标注关键论文出现的位置
```

### 2.5 对比不同的综述角度

> 来源：bohyy/academic-ai-prompt

```
我正在考虑从不同角度写这篇文献综述。请比较以下几种写法的优缺点：

角度1：[描述第一种组织方式]
角度2：[描述第二种组织方式]
角度3：[描述第三种组织方式]

我的目标是：[你的具体目标，如发表在顶刊、突出创新点等]

请帮我分析：
1. 每种角度对应的框架结构
2. 每种角度的优点和缺点
3. 哪种角度最适合我的目标
4. 为什么选择这个角度更好
```

---

## 三、论文内容提炼与分析

### 3.1 快速提炼单篇论文核心信息

> 来源：bohyy/academic-ai-prompt

```
请帮我快速提炼以下论文的核心信息：

论文标题：[标题]
作者：[作者]
年份：[年份]
期刊/会议：[出版地]
论文摘要：[粘贴摘要或论文内容摘要]

请提炼出：
1. 论文的核心研究问题是什么？
2. 论文提出了什么新的理论/方法/发现？
3. 采用的主要研究方法是什么？
4. 主要发现/结论是什么？
5. 这篇论文对该领域的贡献是什么？
6. 这篇论文存在的局限是什么？
7. 一句话总结这篇论文的核心价值

输出格式：
- 核心问题：
- 创新点：
- 方法：
- 主要发现：
- 领域贡献：
- 局限：
- 一句话总结：
```

### 3.2 对比多篇论文的异同

> 来源：bohyy/academic-ai-prompt

```
请帮我对比以下这些论文的异同：

论文1：[标题] - [作者] - [年份]
论文2：[标题] - [作者] - [年份]
论文3：[标题] - [作者] - [年份]

请分析：
1. 这些论文研究的是同一个问题吗？有什么区别？
2. 采用的研究方法有什么不同？
3. 主要发现有什么相同和不同之处？
4. 它们的理论框架是什么？
5. 这些差异反映了该领域的什么发展趋势？
6. 这些论文之间是否存在引用关系？

请用表格形式呈现对比结果：
| 论文 | 研究问题 | 方法 | 主要发现 | 创新点 | 局限 |
```

### 3.3 提炼论文的方法论特点

> 来源：bohyy/academic-ai-prompt

```
我要强调所综述论文采用的多样化方法论。请帮我分析这些论文使用的方法：

论文列表：[粘贴论文标题和使用的方法]

请帮我：
1. 总结所有论文采用的主要研究方法（分类）
2. 每种方法的应用情况（用了哪些论文）
3. 不同方法的优缺点对比
4. 该领域在方法论上的发展趋势
5. 未来研究可以尝试的新方法
```

### 3.4 整理多篇论文的共同观点与分歧

> 来源：bohyy/academic-ai-prompt

```
我想比较和整理以下几篇论文的观点：

论文1：[标题]（[年份]）- [作者]
论文2：[标题]（[年份]）- [作者]
...

请帮我：

【第一部分：共同观点】
这些论文在以下方面有共识：
- 观点1：[描述] → 论文1、2、3都提到了这一点

【第二部分：观点分歧】
这些论文在以下方面存在分歧：
- 分歧1：
  └─ 论文1的观点：[观点]
  └─ 论文2的观点：[观点]
  └─ 我的分析：[哪个观点更合理？为什么？]

【第三部分：理论演进】
时间线：
- [年份]：[理论/发现]
- [年份]：[理论/发现]

【第四部分：总体评价】
- 这个领域的主流观点是什么？
- 还有什么没有被解决的问题？
- 未来可能的研究方向是什么？
```

---

## 四、综述主体写作

### 4.1 撰写综述绪论部分

> 来源：bohyy/academic-ai-prompt

```
请帮我撰写文献综述的绪论部分。

研究领域：[描述]
研究意义：[为什么这个研究很重要]
研究问题：[要解决的主要问题]
综述范围：[时间范围、地理范围等]
目标读者：[期刊名称/论文类型]

绪论应该包含：
1. 研究背景的吸引人的开场（100-150字）
2. 问题的提出（说明为什么这个问题很重要）
3. 已有研究的总体状况简述
4. 综述的主要研究问题
5. 综述的主要贡献
6. 综述的组织结构

请帮我生成一个[800-1000]字的高质量绪论。
```

### 4.2 撰写综述主体部分

> 来源：bohyy/academic-ai-prompt

```
请帮我撰写文献综述主体部分的第[第几个]个小节。

小节标题：[标题]
核心内容：[这个小节要讲什么]
相关论文：
- 论文1：[标题] - [核心贡献]
- 论文2：[标题] - [核心贡献]
[...]

这个小节应该：
1. 清晰地表述该研究方向的核心问题
2. 介绍该方向上的主要理论框架
3. 总结主要研究发现
4. 指出该方向的进展和局限
5. 为下一个小节做铺垫

请生成[1000-1500]字的高质量内容，包含：
- 开场句：引入该研究方向
- 理论框架的介绍
- 主要研究成果的总结
- 存在的问题和不足
- 过渡句：连接到下一部分
```

### 4.3 撰写综述比较分析段落

> 来源：bohyy/academic-ai-prompt

```
请帮我撰写一段对比分析段落，比较这些论文的异同：

要对比的论文：
- 论文1：[标题] - [关键特点]
- 论文2：[标题] - [关键特点]
- 论文3：[标题] - [关键特点]

对比维度：
1. [维度1]
2. [维度2]
3. [维度3]

请写一段[500-800]字的对比分析，要求：
1. 找出这些论文的共同点
2. 强调不同之处
3. 分析这些差异的原因
4. 指出这些差异反映的领域发展趋势
5. 用具体数据/例子支撑论述
```

### 4.4 撰写综述"问题与挑战"部分

> 来源：bohyy/academic-ai-prompt

```
请帮我撰写文献综述的"存在的问题"或"研究挑战"部分。

该领域的主要问题包括：
1. [问题1]
2. [问题2]
3. [问题3]

这个部分应该：
1. 系统地指出该领域存在的主要问题
2. 分析问题产生的原因
3. 说明这些问题的重要性
4. 为解决这些问题的研究做铺垫

请生成[800-1200]字的内容，包含：
- 问题的分类和整理
- 每个问题的具体表现和例证
- 问题之间的关联
- 这些问题对未来研究的启示
```

### 4.5 撰写综述结论部分

> 来源：bohyy/academic-ai-prompt

```
请帮我撰写文献综述的结论部分。

综述的主要发现：
1. [发现1]
2. [发现2]
3. [发现3]

存在的主要问题：[列举]

未来研究方向：[你认为有前景的研究方向]

这个结论部分应该：
1. 总结综述的主要发现
2. 阐述这些发现的意义
3. 指出存在的主要问题
4. 提出未来的研究方向
5. 留下深刻的印象

请生成[600-1000]字的高质量结论。
```

---

## 五、英文学术写作 Prompt（Literature Review）

### 5.1 文献综述核心 Prompt

> 来源：ahmetbersoz/chatgpt-prompts-for-academic-writing (~4,600 Stars)

```
Conduct a literature review on [TOPIC SENTENCE] and provide review paper references
```

```
Summarize the scholarly literature, including in-text citations on [PARAGRAPHS]
```

```
Identify gaps in the literature on [TOPIC SENTENCE]
```

```
Compare and contrast [THEORY1] and [THEORY2] in the context of [RESEARCH DOMAIN]
```

```
Provide me with references and links to papers in [PARAGRAPH]
```

> ⚠️ 注意：ChatGPT 可能生成虚假参考文献，务必人工核实！

### 5.2 PDF 文献综述（GPT-4 文档功能）

> 来源：ahmetbersoz/chatgpt-prompts-for-academic-writing

```
# 上传多篇论文PDF后使用：
Considering these documents, write a literature review in paragraphs 
that include in-text references of the documents in APA format.
```

### 5.3 系统性文献综述 Prompt 合集（20条）

> 来源：drshahizan/Generative-AI-Playground

| # | Prompt（英文） | 用途 |
|---|---------------|------|
| 1 | `Conduct a literature review on [TOPIC], summarizing key findings and insights from relevant research studies.` | 基础文献综述 |
| 2 | `Analyze and synthesize the existing literature on [TOPIC], identifying the major theories, concepts, and methodologies used in previous research.` | 理论框架梳理 |
| 3 | `Review a selection of scholarly articles on [TOPIC], critically evaluating their methodologies, strengths, and limitations.` | 方法论评价 |
| 4 | `Identify and discuss the key themes and trends that emerge from the literature on [TOPIC], highlighting areas of consensus and controversy.` | 趋势与争议识别 |
| 5 | `Conduct a systematic literature review on [TOPIC], organizing and categorizing the relevant research papers based on their research questions, methodologies, and findings.` | 系统性文献综述 |
| 6 | `Provide an overview of the theoretical frameworks and conceptual models that have been employed in the literature on [TOPIC].` | 理论框架概览 |
| 7 | `Analyze the gaps and limitations in the existing literature on [TOPIC], suggesting areas for further research and exploration.` | 研究空白分析 |
| 8 | `Compare and contrast the findings and conclusions from different research studies on [TOPIC], highlighting variations and inconsistencies.` | 结论对比 |
| 9 | `Examine the impact and implications of the research findings in the literature on [TOPIC], considering their relevance and applicability.` | 影响力分析 |
| 10 | `Discuss the methodological approaches used in the literature on [TOPIC], evaluating their effectiveness.` | 方法论评估 |
| 11 | `Identify the key researchers and scholars who have contributed significantly to the field of [TOPIC], citing their influential works.` | 关键学者识别 |
| 12 | `Summarize and critique the methodologies employed in the literature on [TOPIC], assessing their strengths and weaknesses.` | 方法论综述 |
| 13 | `Analyze the research design and data collection methods used in the literature on [TOPIC], evaluating their appropriateness and reliability.` | 研究设计评估 |
| 14 | `Provide a critical assessment of the quality and rigor of the research studies reviewed in the literature on [TOPIC].` | 质量评估 |
| 15 | `Discuss the theoretical and practical implications of the research findings in the literature on [TOPIC].` | 理论与实践意义 |
| 16 | `Synthesize the key findings and conclusions from the reviewed literature on [TOPIC], highlighting gaps and areas for future research.` | 综合与空白 |
| 17 | `Discuss the limitations and challenges encountered in the reviewed research studies on [TOPIC], suggesting strategies for improvement.` | 局限与改进 |
| 18 | `Evaluate the credibility and relevance of the sources used in the literature on [TOPIC].` | 来源可信度 |
| 19 | `Provide a comprehensive bibliography of the review papers and research studies included in the literature review on [TOPIC].` | 参考文献汇编 |
| 20 | `Summarize the major contributions and insights from the reviewed literature on [TOPIC], highlighting their significance.` | 贡献总结 |

### 5.4 学术润色与语言改进

> 来源：ahmetbersoz/chatgpt-prompts-for-academic-writing

```
Rewrite this paragraph in an academic language: [PARAGRAPH]
```

```
Paraphrase the text using more academic and scientific language. 
Use a neutral tone and avoid repetitions of words and phrases. [PARAGRAPH]
```

```
Improve the clarity and coherence of my writing [PARAGRAPHS]
```

```
Analyze the text below for style, voice, and tone. Using NLP, 
create a prompt to write a new article in the same style, voice, and tone: [PARAGRAPHS]
```

```
Write a transition sentence to connect the following two paragraphs: 
[PARAGRAPH1] [PARAGRAPH2]
```

---

## 六、顶会/顶刊 Related Work 撰写指南

> 来源：ChenZiHong-Gavin/paper-prompt

### 6.1 核心原则

```
你是一名顶会（如 ACL, NeurIPS, CVPR, ICLR）论文写作专家，专精 Related Work 章节。

写作原则：
1. 综述而非罗列：每篇引用的论文都要说明它与本文的关系（相似点 + 关键差异）
2. 为自己的工作服务：每个小节结束时应自然过渡到本文的定位
3. 公平客观：不要刻意贬低已有工作来抬高自己
4. 覆盖充分：不遗漏直接相关的重要工作
```

### 6.2 三种组织方式

```
方式 A: 按主题分组（最常用）
将相关工作按技术主题分为 2-4 个小节，每个小节覆盖一类方法。
适用于: 方法涉及多个技术领域的交叉研究

方式 B: 按问题维度分组
围绕要解决的问题的不同方面来组织。
适用于: 问题本身是多维度的

方式 C: 按方法演进分组
按时间或技术迭代顺序梳理。
适用于: 领域发展脉络清晰的综述性工作
```

### 6.3 每个小节的写作模板（3-5句）

```
1. 开门见山：一句话定义这个子方向
   "X methods aim to address Y by Z."

2. 代表性工作：介绍 2-4 篇关键论文，概括其核心思路

3. 共性局限：指出这类方法的共同不足
   "However, most existing approaches assume... / are limited to..."

4. 过渡到本文：自然点明本文如何弥补这个空白
   "In contrast, our work..." / "Unlike these approaches, we..."
```

### 6.4 句式参考

| 场景 | 句式 |
|------|------|
| 区分已有工作 | `Unlike [X] which focuses on ..., our method addresses ...` |
| 指出空白 | `To the best of our knowledge, no prior work has explored ...` |
| 承认相似性 | `Our work shares a similar spirit with [X], but differs in ...` |
| 强调互补性 | `Our approach is orthogonal to [X] and can be combined with ...` |

### 6.5 自查清单

```
- [ ] 是否遗漏了审稿人可能认为必须引用的直接相关工作？
- [ ] 每个小节是否都有到本文的过渡？（避免"孤立的综述"）
- [ ] 是否避免了对已有工作的不公平贬低？
- [ ] 引用格式是否全文一致？
- [ ] 篇幅是否合理？（顶会通常 0.75-1.5 页，期刊可更长）
```

---

## 七、综述优化与质量检查

### 7.1 建立综述论述逻辑

> 来源：bohyy/academic-ai-prompt

```
请帮我检查综述稿件的逻辑流畅性。

当前结构：[粘贴综述的各小节标题和主要内容概括]

请分析：
1. 各小节之间的逻辑关系是否清晰？
2. 信息流是否自然流畅？
3. 有没有重复或遗漏的地方？
4. 过渡段落是否充分？
5. 是否能清楚地引导读者理解研究脉络？

如果有问题，请：
- 指出具体是哪里有问题
- 建议改进的方式
- 提供改进后的过渡句或段落
```

### 7.2 增强综述创新性和学术价值

> 来源：bohyy/academic-ai-prompt

```
请帮我提高这篇文献综述的创新性和学术价值。

目前的综述框架：[描述当前框架]
目前的主要贡献：[列举当前的亮点]

请帮我：
1. 提出一个新的、有趣的综述角度
2. 设计一个新的分析框架，将论文进行新的分类组织
3. 建议一些新的对比维度或分析方式
4. 提出一些有洞察力的论点或结论
5. 指出这个综述相比其他综述的独特贡献
```

### 7.3 检查综述常见问题

> 来源：bohyy/academic-ai-prompt

```
请检查这篇文献综述是否存在以下常见问题：

综述内容：[粘贴综述全文或主要内容]

请检查是否存在：
1. ❌ 简单堆砌论文摘要，缺乏深入分析
2. ❌ 过度强调某些论文，忽略其他重要工作
3. ❌ 逻辑不清，各部分独立，缺乏有机联系
4. ❌ 结论太宽泛，没有突出主要发现
5. ❌ 对不同观点的处理不公平
6. ❌ 缺乏对研究空白和未来方向的讨论
7. ❌ 学术规范不符合要求
8. ❌ 表述不够学术或不够清晰

对于存在的每个问题，请：
- 指出具体是哪里有问题
- 给出改进的建议
- 提供改进后的文本示例
```

### 7.4 全面检查综述学术质量

> 来源：bohyy/academic-ai-prompt

```
请帮我检查这篇文献综述的学术质量。

请从以下几个方面进行评估：

1. 文献的代表性
   - 是否包含了该领域的关键论文？
   - 时间范围是否合适？
   - 是否有地理或学科的偏差？

2. 分析的深度
   - 是否只是简单地总结论文，还是进行了深入分析？
   - 是否指出了研究的空白和问题？
   - 是否提出了新的见解或框架？

3. 论述的逻辑
   - 各部分之间的逻辑关系是否清晰？
   - 是否有明显的矛盾或不连贯？

4. 学术规范
   - 引用格式是否正确？
   - 语言表达是否学术化？

请给出一份详细的质量评估报告。
```

### 7.5 改进综述可读性

> 来源：bohyy/academic-ai-prompt

```
请帮我改进这篇文献综述的可读性和吸引力。

原文内容：[粘贴综述的关键段落]

请帮我：
1. 改进表述的清晰度（简化复杂句子、消除模糊表述）
2. 增强逻辑性（添加过渡句、调整段落顺序）
3. 增加可读性（适当段落长度、使用小标题或要点列表）
4. 提高学术性（确保表述学术化、用词精确）

给出改进后的版本。
```

---

## 八、完整综述写作流程

### 推荐工作流（总耗时 12-22 小时 vs 传统方式 40-60 小时）

```
第1步：框架设计（1-2小时）
→ 使用 Prompt 2.1-2.5 设计综述框架

第2步：内容提炼（2-4小时）
→ 使用 Prompt 3.1-3.4 提炼论文信息

第3步：主体写作（6-10小时）
→ 使用 Prompt 4.1-4.5 逐部分写作

第4步：逻辑优化（2-3小时）
→ 使用 Prompt 7.1-7.2 优化论述

第5步：质量检查（1-2小时）
→ 使用 Prompt 7.3-7.5 检查质量
```

### 针对不同场景的建议

| 场景 | 重点使用 Prompt | 预计字数 |
|------|----------------|----------|
| 发表在期刊的综述 | 所有框架设计 + 高级技巧 + 质量检查 | 3000-8000字 |
| 学位论文综述章节 | 2.1-2.3框架 + 4.1-4.5写作 + 7.1检查 | 8000-15000字 |
| 研究提案文献综述 | 简化框架，重点突出创新（7.2） | 2000-3000字 |

---

## 九、使用注意事项

### ⚠️ 关键提醒

1. **参考文献须人工核实**：AI 可能生成看似合理但实际不存在的论文引用
2. **保持批判性思维**：AI 的建议是辅助而非替代你的学术判断
3. **避免简单堆砌**：好的综述是分析而非罗列，要体现你的洞察和立场
4. **反复修改打磨**：AI 初稿需要人工完善，加入你自己的见解
5. **遵守学术规范**：确保引用格式、语言表达符合目标期刊/学校要求

### 💡 最佳实践

- 🎯 始终以你的研究问题为导向
- 🎯 在使用 AI 的同时保持批判性思维
- 🎯 用自己的语言和见解深化 AI 的建议
- 🎯 反复修改和打磨，直到满意为止
- 🎯 AI 擅长：规划、归纳、分类、对比
- 🎯 人工擅长：最终判断、创意、深度思考
- 🎯 结合使用：最高效率

---

## 📎 更多资源

| 资源 | 链接 | 说明 |
|------|------|------|
| Literature Review Generator (Custom GPT) | ChatGPT GPT Store | 上传PDF自动提取主题并生成综述 |
| PROMPTHEUS | [GitHub](https://github.com/joaopftorres/PROMPTHEUS) | 自动化SLR流程（论文发表于MDPI Information） |
| AutoRelatedWork | [GitHub](https://github.com/windszzlang/AutoRelatedWork) | GPT自动生成带真实引用的Related Work |
| MetaScreener | [GitHub](https://github.com/ChaokunHong/MetaScreener) | 多LLM集成的系统综述筛选工具 |
| stepwise | [GitHub](https://github.com/melek/stepwise) | PRISMA合规的自主SLR工具（Claude Code插件） |

---

> 整理日期：2025-06-04
> 收录协议：各源仓库均采用 MIT 或同等开放协议
> 如有遗漏或建议，欢迎提 Issue
