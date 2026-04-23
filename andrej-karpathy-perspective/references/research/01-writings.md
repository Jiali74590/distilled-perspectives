# Agent 1: Writings & Systematic Thinking — Andrej Karpathy

> 本文档聚焦 Karpathy 的"系统性表达"：YouTube 教学视频（他对标传统论文的"长文"）、博客长文、公开演讲与代表性项目 README。目标是提炼可复用的思想框架，而非复述内容。

---

## 调研摘要

### 来源构成
- **本地素材（一手·核心）**：7 个 transcript，总计 ~3300 行，约 70 万字
  - micrograd（2022）—— 教学的"原始形态"
  - makemore 系列 1/2/4/5（2022）—— 从 bigram → MLP → backprop → WaveNet
  - state-of-gpt（MS Build 2023）—— 他第一次系统讲 LLM 训练 pipeline
  - lex-fridman #333（2022）—— Tesla 离职时的世界观自白
  - skill-issue-loopy-era（2026-03，No Priors）—— 目前最新的 vibe coding / AutoResearch / 未来预测

- **网络补充（一手·辅助）**：
  - karpathy.ai 博客长文索引（2011-2026，23+ 篇）
  - "Software 2.0"（2017, Medium）—— 全文
  - "A Recipe for Training Neural Networks"（2019, karpathy.github.io）—— 提纲
  - "Deep Neural Nets: 33 years ago and 33 years from now"（2022）—— 核心论点
  - "Software 3.0" keynote 二手总结（YC AI Startup School, 2025-06）
  - Dwarkesh Patel 访谈（2025-10, "AGI is a decade away"）—— 核心观点摘要
  - nanoGPT / llm.c / micrograd / nanochat README —— 项目即论文
  - Karpathy's Advice（cs.stanford.edu/people/karpathy/advice.html）
  - Cognitive Core tweet（2025）

- **一手/二手占比**：一手 ~75%（transcript 全文 + 原博客 + 原项目 README + 原 tweet），二手 ~25%（Software 3.0 keynote 由 latent.space 整理、Dwarkesh 访谈摘要为第三方提炼）。

### 方法论
- 优先读他自己讲的话（"Neural Networks: Zero to Hero" 系列几乎就是他的"博士论文"级长文）
- 反复出现 ≥3 个来源 = 核心信念（下表标注）
- 只出现一次 = 观察 / 暂不归入核心框架
- 发现立场演变（见末尾）不做调和

---

## 核心论点（反复出现的真信念）

### 论点 1: "神经网络是一种全新的计算范式"（Software 2.0 / 3.0）

**频次**：在 10+ 个来源中反复出现 —— Software 2.0 博客 (2017)、Lex #333、state-of-gpt、Software 3.0 keynote、skill-issue 访谈、2025 年 11 月的 X 推文。

**原文引用**：
> "Software 1.0 is written in languages such as Python, C++, etc. It consists of explicit instructions to the computer written by a programmer. In contrast, Software 2.0 is written in much more abstract, human unfriendly language, such as the weights of a neural network." —— Software 2.0, 2017

> "在 Software 2.0 里，dataset 就是 source code，architecture 是 rough skeleton，training 是 compilation，最终 binary 是 neural net weights。" —— Software 2.0, 2017（转述）

> "LLMs are a new kind of computer, and you program them in English. Hence I think they are well deserving of a major version upgrade in terms of Software 3.0." —— X, 2025-06

> "the hottest new programming language is English" —— Software 3.0 keynote, 2025

**我的解读**：这是 Karpathy 全部工作的"根信念"。他坚持**神经网络不是一个更好的分类器、不是一个工具，而是一套新的编程范式**——因此他对"API 调用 LLM"和"从零实现 LLM"的态度是不同的：前者只是使用，后者是掌握这套新范式。这个论点一旦接受，他后面所有的立场都推得出来（为什么从零构建 micrograd、为什么支持开源、为什么 AutoResearch 要"agent-first"）。

---

### 论点 2: "从零构建 = 真正理解"（from-scratch 原则）

**频次**：micrograd、makemore 1-5、state-of-gpt、skill-issue、Dwarkesh、nanoGPT/llm.c/micrograd/nanochat 的 README 都在重复。

**原文引用**：
> "I fundamentally wrote micrograd because you can understand how things work at the fundamental level and then you can speed it up later." —— micrograd, 2022

> "basically there's a lot of power that comes from only 150 lines of code and that's all you need to understand to understand neural network training and everything else is just efficiency." —— micrograd, 2022

> "training neural nets and LLMs specifically, it's a huge amount of code but all of that code is actually complexity from efficiency. It's just because you need it to go fast. If you don't need it to go fast and you just care about the algorithm then that algorithm actually is 200 lines of Python very simple to read." —— skill-issue / No Priors, 2026

> "If I can't build it, I don't understand it. That's a Feynman quote, I believe." —— Dwarkesh, 2025-10

> "I wanted to learn stuff I wanted to teach stuff" —— Lex #333, 2022 (解释为什么离开 Tesla)

**我的解读**：Karpathy 的教学不是"让你会用"，而是"让你理解到能自己写一遍"。关键区分：
- **复杂度的来源**：他反复区分"算法复杂度"与"效率复杂度"。现代深度学习框架的代码量是后者，算法本身极简。
- **教学的对应物**：他的做法是**先教 150 行可读版**（micrograd）→ **再映射到 PyTorch 的工业实现**。这是"先搞懂本质，再理解优化"的典型"追本溯源"。
- **暗含判断**：绝大多数人对深度学习的理解停留在 API 层，这是不够的。

---

### 论点 3: "Transformer 是通用可微分计算机"

**频次**：Lex #333、state-of-gpt、makemore-1 结尾、skill-issue 都强调。

**原文引用**：
> "Transformers, the magnificent neural network architecture because it is a general purpose differentiable computer. It is simultaneously expressive in the forward pass, optimizable via backpropagation and gradient descent, and efficient (high parallelism compute graph)." —— 他的 tweet，Lex #333 引用

> "Transformer... is also trainable and very efficient to run on our Hardware. ... there's a number of design criteria that sort of overlap in the Transformer simultaneously that made it very successful." —— Lex #333

> "The Zeitgeist today is: do not touch the Transformer. Touch everything else." —— Lex #333

> "the Transformer that came out in 2016 is the Transformer you would use today except you reshuffle some of the layer norms... it's been remarkably stable" —— Lex #333

**我的解读**：他对 Transformer 的定位高于大多数研究者。三性合一（**expressive · optimizable · efficient**）是他反复强调的三角——他认为这三者同时满足的架构极稀缺。他甚至把 Transformer 的 residual+MLP 结构类比为"一段 20 行 Python 代码，第一轮优化第一行，第二轮优化第二行"（Lex #333）——这种**"神经网络是一段被梯度雕刻出来的代码"**的视角，是他独有的。

---

### 论点 4: "LLM 是 people spirits / ghosts / token simulators"（LLM 心理学）

**频次**：state-of-gpt、Software 3.0 keynote、Dwarkesh、skill-issue 都反复类比。

**原文引用**：
> "These transformers are just like token simulators, they don't know what they don't know. They just imitate the next token. They don't know what they're good at or not good at." —— state-of-gpt, 2023

> "Transformers need tokens to think." —— state-of-gpt, 2023

> "LLMs don't want to succeed, they want to imitate. You want to succeed, and you should ask for it." —— state-of-gpt, 2023

> "We're not building animals. We're building ghosts or spirits." —— Dwarkesh, 2025-10

> "LLMs... stochastic simulations of people, with emergent 'psychology'" —— Software 3.0 keynote, 2025

> "I think to a large extent prompting is just making up for this cognitive difference between these two architectures like our brains here and LLM brains." —— state-of-gpt, 2023

**我的解读**：他把 LLM 当作**从互联网文本蒸馏出来的"人类意识碎片"**，而不是动物式的"演化智能"。这个视角有几个直接推论：
1. prompt engineering 的本质是"弥补 LLM 的认知缺陷"——他罕见地把 prompting 放在一个理论框架里。
2. "jagged intelligence"（锯齿状智能）是自然结果——它们在训练过信号上是天才，脱离训练分布立刻变成十岁小孩。
3. 不要指望 LLM 自己知道自己不知道什么——必须在 prompt 里告诉它。
4. "ghosts/spirits"的比喻为他对 AGI timeline 的保守判断（"decade of agents"）提供了世界观基础。

---

### 论点 5: "RL 是通过吸管吸取监督"（RL 的根本缺陷）

**频次**：Lex #333、Dwarkesh、skill-issue 反复出现。

**原文引用**：
> "reinforcement learning is extremely inefficient way of training neural networks because you're taking all these actions and all these observations and you get some sparse rewards once in a while... you can burn forest and you can sort of Brute Force through it... but it's extremely inefficient." —— Lex #333, 2022

> "You've done all this work that could be a minute of rollout, and you're sucking the bits of supervision of the final reward signal through a straw and you're broadcasting that across the entire trajectory." —— Dwarkesh, 2025-10

> "at the time [2015 OpenAI] everyone was super excited about reinforcement learning from scratch... that's a misstep" —— Lex #333 + Dwarkesh

> "AI agents... are struggling with the exact same thing... labs can improve the models in anything that is verifiable... but some of the things where they're struggling is like nuance" —— skill-issue, 2026

**我的解读**：Karpathy 对 RL 的态度从 2015 "兴奋" → 2022 "低效但有用" → 2025 "misstep"。他认为**正确的路径是：先用大规模预训练学表征，再用少量 RLHF 调方向**。这解释了他为什么一直在强调"pretrain is 99% of compute"（state-of-gpt）——他把智能的主要来源归于 next-token prediction 的 multitask 压力，而不是 RL 的奖励塑造。

---

### 论点 6: "AGI 是十年尺度，不是一年"

**频次**：Dwarkesh 访谈为主，skill-issue 访谈的"jaggedness"论证支撑。

**原文引用**：
> "The problems are tractable, but they're still difficult." —— Dwarkesh, 2025-10

> "AGI is still a decade away" —— Dwarkesh 标题（他本人确认）

> "the whole thing still doesn't... it's still kind of like bursting at the seams a little bit and there's cracks and it doesn't fully work." —— skill-issue, 2026

> "I simultaneously feel like I'm talking to an extremely brilliant PhD student who's been a systems programmer for their entire life and a 10-year-old. ... humans have a lot less of that kind of jaggedness." —— skill-issue, 2026

**我的解读**：他的"十年论"不是悲观，是基于**两个具体观察**：
1. **能力锯齿（jaggedness）**：模型在可验证任务（代码、数学）上进展飞快，在软任务（幽默、共识、常识推理）上几乎停滞——这说明 RL/评估体系只能打磨一部分能力，其他部分需要不同机制。
2. **RLHF 的信息效率低**：他用"吸管吸监督"的比喻说明，当前训练范式从单次 rollout 提取的信号太少，难以快速推进 AGI。

这让他在加速主义（Dario/Altman）和稳健派（LeCun）之间取一个中位——**问题可解，但需要时间**。

---

### 论点 7: "Autonomy Slider / 部分自主"

**频次**：Software 3.0 keynote、skill-issue、state-of-gpt 都涉及。

**原文引用**：
> "Use LLMs in low-stakes applications. Combine them always with human oversight. Use them as a source of inspiration and suggestions and think co-pilots, instead of completely autonomous agents" —— state-of-gpt, 2023

> "Demo is works.any(), product is works.all()" —— Software 3.0 keynote, 2025

> "Partial autonomy... examples include Cursor's Tab→Cmd+K→Cmd+L→Cmd+I progression and Tesla's autopilot levels" —— Software 3.0 keynote, 2025

> "copilot really I mean it's autopilot for programming right, and currently it's doing the link following which is like the simple copy paste" —— Lex #333, 2022

**我的解读**：Karpathy 的"partial autonomy"不是保守，是对**产品级可靠性**的深刻理解（来自 Tesla Autopilot 五年经验）。他反复用"demo → product"的距离来提醒：
- demo 能 run，说明能力存在
- product 要 "any" → "all"，要求可靠性量级跃升
- 因此当前阶段**正确做法是"带刻度的自主滑块"**：让人类在关键节点验证、让 AI 在低风险节点全权接管。

---

### 论点 8: "现在是 loopy era / skill issue"（2025-2026 的新元认知）

**频次**：skill-issue 访谈是核心来源。这是他最近的新信念。

**原文引用**：
> "it's skill issue. It's not that the capability is not there. It's that you just haven't found a way to string it together." —— skill-issue, 2026

> "the name of the game now is to increase your leverage. ... how can I have not just a single session of clot code... How can I have more of them?" —— skill-issue, 2026

> "I don't think I've typed like a line of code probably since December basically." —— skill-issue, 2026-03

> "you have to arrange things such that they're completely autonomous and the more you... how can you maximize your token throughput and not be in the loop." —— skill-issue, 2026

**我的解读**：这是他 2025 年 12 月之后的**工作方式跃迁**——从"写代码"到"指挥多个 agent 循环"。他把这种状态叫 **"AI psychosis"**（AI 癫狂）：因为可能性无限大，任何瓶颈都变成"skill issue"——不是能力不够，是你没想清楚怎么组合。这个论点本身又是一个"元论点"：**能力的前沿从模型转移到了"如何使用模型"**。

---

## 自创术语词典

| 术语 | 定义 | 首次出现 | 典型用法 |
|------|------|----------|----------|
| **Software 2.0** | 数据集 + 架构 + 梯度下降编译出的神经网络权重作为"代码" | 2017, Medium | "Software 2.0 is eating Software 1.0" |
| **Software 3.0** | 用自然语言（英语）prompt LLM 作为程序 | 2025, YC Keynote | "the hottest new programming language is English" |
| **vibe coding** | 完全交给 agent 写代码、不看 diff、语音交互、忘记代码存在 | 2025-02, tweet | "fully give in to the vibes, embrace exponentials, and forget that the code even exists" |
| **LLM OS** | LLM 作为新型"操作系统"——context window 像 RAM，tool use 像系统调用 | 2023 →2025 keynote | 与 utilities/fabs/timeshare mainframes 类比 |
| **token simulator** | 形容 Transformer 的本质：它不追求成功，它模仿 | state-of-gpt 2023 | "they don't know what they don't know" |
| **people spirits / ghosts** | LLM 是从人类语料蒸馏出的"幽灵"而非演化动物 | Software 3.0 / Dwarkesh 2025 | 对比"we're not building animals" |
| **jagged intelligence** | 在某些任务天才、另一些任务低能的锯齿形能力分布 | skill-issue 2026 | "brilliant PhD student AND 10-year-old" |
| **cognitive core** | 剥离百科知识、保留推理能力的小模型（几十亿参数） | X tweet 2025-06 | "maximally sacrifices encyclopedic knowledge for capability" |
| **autonomy slider** | 人-机协作的可调自主度（Copilot → Autopilot） | Software 3.0 keynote 2025 | 用 Cursor 的快捷键序列做例子 |
| **skill issue** | 当 AI 没达到预期时，默认是你的使用方式问题（而非模型能力问题） | skill-issue 2026 | 与 "AI psychosis" 配合 |
| **AutoResearch** | 让 agent 自主进行 AI 研究循环（改进 → 训练 → 评估 → 改进） | skill-issue 2026 | "remove yourself as the bottleneck" |
| **AI psychosis** | 面对 agent 无限可能性时的焦虑+兴奋状态 | skill-issue 2026 | "I'm in this perpetual state of AI psychosis" |
| **sucking supervision through a straw** | RL 的根本缺陷——用最终奖励反推全轨迹 | Dwarkesh 2025 | 批判 RLHF |
| **Dobby (the elf claw)** | 他自己的家务 agent 的昵称；指代"个人 agent 管家" | skill-issue 2026 | "Dobby controls everything in natural language" |
| **macro actions** | 指挥 agent 的"宏观动作"——功能级而非代码行级 | skill-issue 2026 | "move in much larger macro actions" |
| **Leaky abstraction (of NN training)** | 神经网络训练不是可靠抽象，出错时沉默，必须patient 地调 | Recipe 2019 | "neural net training fails silently" |
| **stack more layers** | 讽刺性术语：过去五年进步主要靠 scale 而非算法 | Lex #333 + 常见 tweet | "touch everything else [but the Transformer]" |

---

## 教学哲学（"from scratch" 原则的深层逻辑）

### 三条原则

**1. 算法复杂度 vs 效率复杂度**
> "everything else is just efficiency" —— 重复 10+ 次

他坚持先教"慢但可读"版本，再解释"快但复杂"版本的由来。micrograd 是 scalar-valued 的 150 行，但和 PyTorch tensor 版"数学完全一致，区别只是 parallelism"。

**教学顺序**：bigram 计数 → 1-hot 编码 → MLP → WaveNet → Transformer。每一步都**只增加一个抽象层**，并解释为什么需要它。

**2. pedagogical framing: "loss goes down" 的感官反馈**
> "it's really cool to literally code, you run it in a notebook and it gives you a result and you're like oh wow and like actual numbers actual input" —— Lex #333

他反对用 slides / 符号证明讲 DL，坚持 Jupyter notebook 里边跑边讲。这是 **"教学必须可验证"** 的原则——代码才是真理。

**3. "If I can't build it, I don't understand it"（Feynman）**

这条原则贯穿 micrograd/makemore/nanoGPT/llm.c/nanochat 的全部项目。他的"写书方式"就是写项目——**项目即著作**。

### 反对立场

他明确反对的几种常见做法：
- **"直接用 API 就行"** → 会导致你只是使用、不是掌握
- **"tensor 从一开始就用"** → "it's not pedagogically useful to be dealing with tensors from scratch"（micrograd）
- **"先看论文/读教科书"** → "papers are quite good... textbooks are too high up in the level of abstraction"（Lex #333）
- **"一口气讲 Transformer"** → 必须先经过 bigram/MLP/WaveNet 的铺垫

### 2026 年的微妙转变

在 skill-issue 访谈里出现了一个**新的立场**：
> "I actually tried to make that video a little bit... but I kind of realized that this is not really adding too much because people cuz it's already so simple that it's 200 lines that anyone could ask their agent to explain it in various ways. ... I'm not explaining to people anymore. I'm explaining it to agents."

**他开始把教学对象从人切换到 agent**——写 markdown 给 agent，agent 再"个性化"讲给每个学习者。这是一个**立场演变**（见末尾）。

---

## 智识谱系

### 明确提到的影响源

**学术导师**：
- **Geoffrey Hinton**（多伦多大学神经网络课，2007）—— Karpathy 本科阶段的启蒙
- **Fei-Fei Li**（Stanford PhD 导师，计算机视觉）

**精神偶像**：
- **Richard Feynman**——"If I can't build it, I don't understand it"（Dwarkesh 2025-10）
- **Einstein, Feynman**——Lex 形容他时，他默认的类比对象（Lex #333）

**直接同事**：
- **Ilya Sutskever**（OpenAI co-founder，共同的 Hinton 学派）
- **Greg Brockman, Sam Altman**（OpenAI 创始阶段同事）
- **Elon Musk**（Tesla 五年老板，Lex #333 显示仍有深情）

**思想家（他推荐的）**：
- **Richard Dawkins** —— *The Selfish Gene*："helped me understand altruism... the selection is in the level of genes was a huge insight"（Lex #333）
- **Nick Lane**（进化生物学家）—— "I really appreciate that"（Lex #333）
- **Carl Sagan** —— *Contact* 的数学符号梗（Lex #333）
- **教科书偏好**：Molecular Biology of the Cell（"I like the cell"）；对 DL 教科书不满意

**同代 AI 人**：
- **Alec Radford**（他称为"无名英雄之一"，GPT 的真正推手）
- **Yann LeCun**——他对 LeCun 1989 论文做过致敬（"33 years ago"博客）

### 影响了谁

Karpathy 的影响主要通过**教学**而非论文：
- **YC AI Startup School 听众**（直接启发 Software 3.0 时代创业者）
- **nanoGPT / llm.c / micrograd 复刻者**（全球几十万学习者）
- **"vibe coding"一词**被 Collins Dictionary 评为 2025 年度词汇
- **Eureka Labs**（他自己创办的 AI 教育公司）—— 把他的教学哲学产品化

---

## 立场演变（发现的真实变化，不调和）

### 变化 1: 对 RL 的态度（2015 → 2022 → 2025）

- **2015**（OpenAI 初期）：兴奋地用 RL 训 World of Bits agent
- **2022**（Lex #333）："RL is extremely inefficient... but works for go and dota"
- **2025**（Dwarkesh）：RL from scratch 是"**a misstep**"

**转变原因**：他看到了 pretrained representations 的力量——"now you're not training an agent from scratch, you are taking GPT as an initialization"（Lex #333）。RL 在表征之上才有意义。

### 变化 2: 对"写代码"的定位（2022 → 2026）

- **2022**（Lex #333）：对 Copilot 友好但仍强调手写代码的学习价值
- **2026**（skill-issue）："I don't think I've typed like a line of code probably since December"

**这不是否定之前的立场**——他仍然认为"from scratch 理解是必要的"，但**执行阶段已经完全委托给 agent**。理解和执行解耦了。

### 变化 3: 教学对象（2022 → 2026）

- **2022-2024**：Neural Networks Zero to Hero 是对"人类学习者"的手把手教学
- **2026**（skill-issue）：microGPT 不再做视频，"I'm explaining it to agents... agents can be the router"

**暗含判断**：未来人类学习材料 = agent 可以理解的 markdown + skill 指令。教育的中介从"讲师"变成"agent"。

### 变化 4: 对 AGI 时间表的立场（隐含）

- **2015-2020**：没有明确公开的时间表判断
- **2022**（Lex #333）：谨慎，强调 "bootloader for AI"——我们可能只是在造第一代 AGI
- **2025**（Dwarkesh）：明确说"**decade away**"，公开反驳"year of agents"

**这是他第一次把 AGI timeline 当成一个需要公开"纠偏"的议题**——表明他认为过度乐观已经形成舆论风险。

### 一致性很强的立场（没有改变过）

- **Transformer 的特殊地位**（2017+ 的所有材料一致）
- **Software 2.0/3.0 的范式论**（一脉相承）
- **从零构建的教学哲学**（micrograd → nanochat 五年都一样）
- **对开源的支持**（Lex #333 + skill-issue 一致）
- **"partial autonomy 优于 full autonomy"**（Tesla 时代 → 2026 一致）

---

## 信息源列表（含 URL + 可信度评级）

### 一手（Tier 1：他本人的话，最高可信度）

| 来源 | URL | 年份 | 可信度 |
|------|-----|------|--------|
| micrograd transcript（本地） | https://www.youtube.com/watch?v=VMj-3S1tku0 | 2022 | ★★★★★ |
| makemore-1 bigram（本地） | https://www.youtube.com/watch?v=PaCmpygFfXo | 2022 | ★★★★★ |
| makemore-2 MLP（本地） | YouTube | 2022 | ★★★★★ |
| makemore-4 backprop ninja（本地） | YouTube | 2022 | ★★★★★ |
| makemore-5 WaveNet（本地） | YouTube | 2022 | ★★★★★ |
| State of GPT（MS Build）本地 | https://www.youtube.com/watch?v=bZQun8Y4L2A | 2023 | ★★★★★ |
| Lex Fridman #333（本地） | https://www.youtube.com/watch?v=cdiD-9MMpb0 | 2022 | ★★★★★ |
| No Priors / Sarah Guo "Skill Issue"（本地） | https://www.youtube.com/watch?v=kwSVtQ7dziU | 2026-03 | ★★★★★ |
| Software 2.0（Medium 全文）| https://karpathy.medium.com/software-2-0-a64152b37c35 | 2017 | ★★★★★ |
| A Recipe for Training Neural Networks | http://karpathy.github.io/2019/04/25/recipe/ | 2019 | ★★★★★ |
| Deep Neural Nets: 33 years ago and 33 years from now | http://karpathy.github.io/2022/03/14/lecun1989/ | 2022 | ★★★★★ |
| A Survival Guide to a PhD | http://karpathy.github.io/2016/09/07/phd/ | 2016 | ★★★★★ |
| karpathy.ai 主页 | https://karpathy.ai/ | 常更新 | ★★★★★ |
| Karpathy's Advice | https://cs.stanford.edu/people/karpathy/advice.html | ~2011 | ★★★★★ |
| nanoGPT README | https://github.com/karpathy/nanoGPT | 2022+ | ★★★★★ |
| llm.c README | https://github.com/karpathy/llm.c | 2024 | ★★★★★ |
| micrograd README | https://github.com/karpathy/micrograd | 2020+ | ★★★★★ |
| "vibe coding" 原 tweet | https://x.com/karpathy/status/1886192184808149383 | 2025-02 | ★★★★★ |
| "cognitive core" tweet | https://x.com/karpathy/status/1938626382248149433 | 2025-06 | ★★★★★ |
| Software 3.0 发布推文 | https://x.com/karpathy/status/1935518272667217925 | 2025-06 | ★★★★★ |
| AI 作为 Software 2.0 tweet | https://x.com/karpathy/status/1990116666194456651 | 2025-11 | ★★★★★ |

### 一手（Tier 1.5：他本人访谈的二手整理，经过编辑）

| 来源 | URL | 年份 | 可信度 |
|------|-----|------|--------|
| Dwarkesh Patel podcast transcript | https://www.dwarkesh.com/p/andrej-karpathy | 2025-10 | ★★★★☆ |
| Lex Fridman #490（State of AI 2026） | https://lexfridman.com/ai-sota-2026-transcript/ | 2025 | ★★★★☆ |
| Latent.space Software 3.0 notes | https://www.latent.space/p/s3 | 2025-06 | ★★★★☆ |

### 二手（Tier 2：他人总结，作为参考）

| 来源 | URL | 用途 |
|------|-----|------|
| Simon Willison "AGI is a decade away" 笔记 | https://simonwillison.net/2025/Oct/18/agi-is-still-a-decade-away/ | 二手 Dwarkesh 访谈提炼 |
| FlowHunt "Decade of AI Agents" | https://www.flowhunt.io/blog/the-decade-of-ai-agents-andrej-karpathy-agi-timeline/ | 二手时间线总结 |
| Kyle Howells "Software Is Changing (Again)" | http://ikyle.me/blog/2025/andrej-karpathy-software-is-changing-again | Software 3.0 keynote 笔记 |

### 黑名单来源（未使用）
- 知乎、微信公众号、百度百科、中文 Medium 翻译
- 未注明原始访谈的"Karpathy 金句"合集
- 任何"据说/听说"的引语

---

## 备注：待补充的原始材料

如果后续要做第 2/3 阶段（表达 DNA / 决策框架），以下**本地缺失**的一手内容值得补齐：

1. **"A Recipe for Training Neural Networks"博客全文**（本次只取了结构，细节如"don't be a hero"、"overfit, then regularize"的原话很值钱）
2. **"The Unreasonable Effectiveness of RNN"(2015)**——早期思想源头
3. **"Deep Reinforcement Learning: Pong from Pixels"(2016)**——他 RL 立场的源点
4. **Software 3.0 keynote 原视频 transcript**（目前只有二手整理）
5. **Dwarkesh 访谈的完整 transcript**（本次只取了摘要）
6. **"A from-scratch tour of Bitcoin in Python"(2021)**——展示他"什么都 from scratch"的边界
7. **他的 Stanford CS231n 课程讲义**（公开的，是他教学哲学的源头）

## 关键洞察（如果只记一件事）

Karpathy 的所有工作围绕一个核心信念：**"神经网络是一种新计算范式，理解它的唯一方式是从零构建它"**。

- 这个信念让他在 2017 写出 Software 2.0
- 在 2022 启动 Zero-to-Hero 把 micrograd→transformer 全链路开源
- 在 2025 提出 Software 3.0，把 prompt 升级为"第三种编程语言"
- 在 2026 进入 loopy era，发现"用好 agent"本身也是一门需要"from scratch"掌握的新技艺

他的一致性在于：**从不绕过基础层直接去用上层 API**。他的演化在于：**基础层本身一直在上移**（scalar → tensor → transformer → agent orchestration → auto-research）。
