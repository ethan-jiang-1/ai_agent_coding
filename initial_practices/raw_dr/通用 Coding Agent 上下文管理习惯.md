# **编码智能体通用上下文管理习惯与工作流深度研究报告**

## **编码智能体时代的上下文工程范式转移**

随着人工智能在软件工程领域的深度渗透，自主与半自主编码智能体（Coding Agents）——包括 Cursor、Claude Code、Aider、Continue、Cline、Roo Code、Trae、Windsurf 以及 Gemini CLI 等——已经从根本上改变了代码编写的生命周期。在早期的大语言模型（LLM）交互中，开发者的核心技能被称为“提示词工程（Prompt Engineering）”，即如何精准地构建单次请求。然而，在现代编码智能体的应用语境下，这一核心技能已经演进为“上下文工程（Context Engineering）”。上下文工程被定义为对智能体所处信息环境的战略性主动管理，它要求开发者将“上下文管理”理解为一系列连续的用户动作，而非仅仅关注模型底层的注意力机制。这涉及到决定智能体应该看到什么、记住什么、被禁止访问什么，以及在何种特定时刻必须清空其记忆以防止认知污染 1。

本报告的深度分析表明，在人工智能辅助开发中最常见的失败模式——从幻觉依赖、代码逻辑漂移到巨大的 Token 成本超支——其根源往往并非底层模型推理能力的匮乏，而是用户在上下文管理方面的不良习惯 3。当用户将编码智能体视作具备人类记忆特征的资深开发者，期望它们能够在不断变化的任务参数中隐式地保持长期的情境感知时，智能体不可避免地会遭遇“上下文腐烂（Context Rot）”或“上下文漂移（Context Drift）” 5。由于 LLM 本质上是无状态的函数映射器，它们会倾向于优化“看起来完整”的响应，而不是验证绝对的正确性，这最终导致代码质量下降和技术债务的急剧增加 7。

为了彻底解决这一问题，本报告对几乎适用于所有主流编码智能体的通用上下文管理习惯进行了详尽的剖析。通过对官方使用指南、产品团队的公开经验、高水平开发者的工作流分享以及架构设计模式进行综合梳理，本文明确界定了哪些习惯能够跨越特定产品的壁垒，成为既能大幅节省 API 成本，又能显著减少返工率的通用最佳实践。

## **核心反模式：识别最常见且最昂贵的用户坏习惯**

在深入探讨高水平用户的稳定好习惯之前，必须首先对阻碍智能体发挥效能的核心反模式进行解构。这些坏习惯不仅广泛存在于初级用户群体中，甚至许多经验丰富的传统开发者在初次接触 Agent 时代时也会陷入这些陷阱。

第一个且最具破坏性的坏习惯是“大杂烩式会话（Kitchen Sink Sessions）”。受传统聊天软件界面的影响，用户倾向于在一个单一的、不断滚动的聊天窗口中处理数天的开发工作。在这个窗口中，用户可能刚刚让智能体修改了前端的 CSS 样式，紧接着又要求其重构后端的数据库迁移脚本，随后又开始配置 CI/CD 流水线 3。这种习惯会导致极其严重的后果。随着项目规模的扩大，智能体会通过读取文件来构建上下文，但当文件数量增加时，智能体无法读取所有内容，只能猜测哪些文件是重要的，而这种猜测往往是错误的。在一个包含 50 个文件的项目中，一个简单的问题可能会拉取高达 18,000 个 Token 的无关代码，这就像是要求水管工修理水槽，而对方却开始阅读整栋房屋的建筑蓝图 4。此外，随着 Token 数量逼近模型的上下文窗口极限（如 200k Token），系统不仅会产生高昂的财务成本，还会为了腾出空间而强行截断早期的重要对话，导致不可预测的“失忆症（Amnesia）” 11。

第二个普遍的坏习惯是“对话式追加纠错”而非“修改原提示词”。当智能体生成了错误的代码时，人类的自然反应是在聊天框中追加一条斥责或纠正的回复，例如：“不对，你忘记处理空指针异常了，重新写”。这种做法迫使底层模型同时处理初始的模糊提示词、智能体自身生成的错误代码以及用户追加的仓促纠正 13。随着这种多轮纠错的进行，上下文窗口中充满了“僵尸代码”和失败的推理过程。由于 LLM 会从其上下文中的所有信息中学习，保留这些错误的尝试实际上构成了负面的少样本提示（Few-shot Prompting），使得模型陷入逻辑循环，不仅无法修复问题，反而会引入新的 Bug 14。

第三个反模式是“蛮力粘贴大型外部材料”。当开发者需要使用新的框架或 API 时，常常会将几十页的官方文档直接复制并粘贴到聊天框中。这种做法不仅瞬间消耗掉大量的上下文预算，而且由于粘贴的文本通常包含大量无结构的 HTML 标签或导航栏噪音，严重干扰了模型的注意力机制，导致模型在提取关键技术规范时产生幻觉 16。

第四个典型的坏习惯是“工具与联网权限的默认全开”。现代编码智能体具有强大的自主性，可以执行终端命令、浏览实时互联网以及通过 MCP（模型上下文协议）服务器与第三方系统交互。然而，许多用户在处理简单的局部逻辑修改时，也默认开启网络搜索工具。这导致智能体在遇到微小的语法疑问时，自主发起网络搜索，从而将大量非结构化的网页文本和潜在的过时 StackOverflow 答案引入上下文中，破坏了其对本地代码库的专注度 4。

## **会话生命周期管理：边界划分与状态传递**

跨越所有编码智能体产品线，高水平用户最稳定的好习惯是对会话生命周期进行极其严格的纪律管理。他们深刻理解，上下文窗口是一种极其昂贵且易耗的计算资源，必须像管理计算机内存一样对其进行主动的垃圾回收和状态重置。

### **确立严格的会话边界**

高水平用户从不依赖单个超长会话，而是将工作划分为离散的、目标明确的微小周期。通用的黄金法则是：**任务边界即会话边界** 5。这意味着每当一个具体的功能模块开发完成、一个 Bug 被成功修复并提交到版本控制系统后，当前的会话就必须被终止，后续的工作应当在一个全新的会话中展开 13。

在处理复杂任务时，开发者会遵循“事不过三（The Third Correction Rule）”原则。如果智能体连续三次未能正确理解意图、产生幻觉或重复同样的错误，这表明当前的上下文已经被严重污染。此时，最节省时间和成本的习惯不是继续追加第四次纠正，而是立即果断地结束对话，撤销工作区中不正确的代码更改，重新梳理思路，并开启一个全新的干净会话 5。经验表明，一个拥有更优质初始提示词的干净会话，其输出质量永远优于一个积累了大量纠错历史的冗长会话 22。

此外，重大重构之后的立即重置也是不可或缺的习惯。当开发者在会话中途对代码库的底层架构或核心组件进行了大规模重组后，智能体上下文缓存中原有的文件结构和逻辑路径已经完全失效。如果此时继续在原会话中提问，智能体会基于过时的信息进行推理，从而给出南辕北辙的建议。因此，基于状态变化或基于时间（例如每 60 到 90 分钟）进行定期重置，是维持智能体敏锐度的通用法则 5。

| 会话状态 / 触发条件 | 用户应对动作 | 认知与经济学原理 |
| :---- | :---- | :---- |
| **单一功能/任务完成** | 使用 /clear 或开启新聊天窗口 | 释放被占用的 Token 预算，防止不同特性的逻辑模式发生交叉污染。 |
| **连续三次修正失败** | 撤销代码，重开会话并优化提示词 | 阻断模型从自身错误输出中学习的负反馈循环（Negative Few-Shot）。 |
| **进行大规模文件重构后** | 立即终止当前会话，重新索引 | 消除缓存中的过时抽象语法树（AST）和路径信息，避免幻觉。 |
| **上下文容量达到 70%-80%** | 触发上下文摘要并强行开启新会话 | 防止模型因底层自动截断机制导致的意外“失忆”和推理能力断崖式下跌。 |

### **Memento 工作流：跨会话的记忆延续与摘要机制**

既然长会话是破坏性的，开发者如何在一个由数百个文件组成的大型项目中保持连贯性？答案在于实施被业界称为“Memento（记忆碎片）”的工作流 23。这一命名源自同名电影，主角由于缺乏短期记忆，必须依靠不断记录笔记来向未来的自己传递信息。高水平的 AI 开发者正是采用这种方式与无状态的 LLM 协作。

在这个工作流中，开发者将大约 80% 的精力投入到文档记录和状态管理上，而不是直接编写代码。当一个开发阶段接近尾声，或者上下文容量即将达到安全阈值（如 70%）时，开发者会主动触发摘要机制。在 Claude Code 中，这通常表现为使用 /compact 命令压缩历史记录，或者要求智能体生成一份 session-handoff.md（会话交接文档） 22。在 Roo Code 中，则可以通过点击“智能上下文压缩”按钮来实现 11。这份交接文档会精炼地记录刚刚完成的架构决策、已解决的关键问题、当前的代码状态以及下一个会话需要解决的核心任务。

当开启新的会话时，开发者不会让智能体盲目地重新扫描整个代码库。相反，新会话的第一个动作是引导智能体阅读这份轻量级的交接文档，或者读取项目中统一维护的进度记录。通过这种习惯，一段原本消耗了 50,000 Token 的试错对话，被压缩成了仅含几百个单词的高密度上下文指令 25。这种做法不仅使得跨线程的持久化记忆成为可能，而且在根本上消除了重构过程中的方向迷失，其效果远远优于任何试图扩大模型原生上下文窗口的底层技术努力 6。

## **交互微操：编辑替代追加，规划先行于执行**

在微观的单次交互层面，通用习惯的差异决定了代码产出的稳定性和后续的维护成本。高水平用户在发送任务和处理错误反馈时，展现出了高度一致的纪律性。

### **回答方向不对时：坚决修改原任务**

如前文所述，在聊天框中无休止地追加纠正指令是典型的坏习惯。当面对智能体偏离方向的回答时，高水平用户最稳定的微操习惯是：**回退、编辑并重新运行（Edit and Rerun）** 28。

当开发者发现智能体生成的组件遗漏了某个边界条件，他们不会发送“你忘了处理 null 值”的新消息。相反，他们会在 IDE 中丢弃生成的代码，向上滚动到最初的提示词，点击编辑按钮，在原有的指示中补充道：“请确保涵盖所有边界情况，特别是 null 值的处理”，然后重新提交这个被优化的完整提示词。这种做法保持了对话树的极度扁平化。对于智能体而言，它看到的是一个经过深思熟虑、条件完备的一次性指令，而不是一个充满争吵和妥协的冗长辩论过程。这种习惯几乎不依赖于特定产品，无论是在 Aider 的终端界面，还是在 Cursor 的可视化侧边栏中，保持聊天历史的纯洁性都是减少模型认知负荷、降低重复返工率的最直接手段 22。

### **第一轮合并说明：探索与执行的强制分离**

初级用户往往期望通过一句简短的命令让智能体立刻吐出数百行完美的生产级代码。然而，让智能体在没有充分探索项目结构的情况下直接跳入代码编写阶段，极易导致其解决错误的问题或破坏现有的架构设计 22。

为了避免这种情况，高水平用户采用的第一轮交互习惯是“规划与执行强制分离”。在 Cursor 中，这体现为使用 Shift+Tab 切换到“Plan Mode（规划模式）”；在 Claude Code 中，体现为使用 /explore 命令；在 Cline 或 Roo Code 中，同样有明确的规划与执行阶段划分 13。

在第一轮交互中，开发者会合并说明所有相关背景，但明确禁止智能体编写任何实现代码。智能体被要求首先利用 grep、AST 树解析或语义搜索来广泛探索代码库，提出需要澄清的疑问，并输出一份结构化的实施计划（通常以 Markdown 格式包含受影响的文件路径和变更大纲） 13。这种习惯相当于设置了一道低成本的验证门槛。开发者可以快速审查这份计划，纠正智能体在架构理解上的偏差。只有当计划被明确批准后，智能体才被允许进入第二轮的实际代码编写阶段。通过这种方式，大规模的复杂重构被拆解成了模型更容易处理的块状结构，不仅极大降低了后续代码评审的难度，也避免了因方向性错误而导致的大面积代码回滚 30。

## **静态背景管理：长效规则与项目记忆的架构设计**

为了让编码智能体适应特定的企业级代码规范和项目架构，开发者必须引入长期的项目记忆。目前，几乎所有主流平台都支持通过特定文件（如 Cursor 的 .cursorrules、Claude Code 的 CLAUDE.md、Gemini CLI 的 GEMINI.md、GitHub Copilot 的 .github/copilot-instructions.md）来定义全局静态上下文 16。然而，如何使用这些文件，区分了普通用户与上下文工程专家。

### **稳定背景移出聊天框，警惕规则膨胀**

最基础的好习惯是：**永远不要在每次对话的开头重复输入你的编码偏好**。如果一个项目要求使用 TypeScript、禁止使用传统的类的生命周期组件、且需要严格遵循某种目录结构，这些稳定的背景信息应当彻底移出聊天框，固化到项目根目录的规则文件中。每次会话启动时，系统会自动静默加载这些规则，确保规范的一致性 26。

然而，这极易引发另一个极端反模式：规则膨胀（Rule Bloat）。许多用户倾向于将公司整本的开发手册、所有可能的边缘情况处理指南、甚至第三方的完整 API 文档全部塞进全局规则文件中。由于全局规则在每一次的微小交互中都会被无条件加载，这种做法不仅在后台默默燃烧着巨额的 Token 预算，还会严重稀释模型对当前具体任务的注意力（Attention Dilution） 16。

### **高效规则的编写与作用域控制**

高水平用户的规则文件通常极其精简、极具可操作性，并且严格控制作用域：

1. **采用负面约束策略**：在引导 LLM 行为时，明确的禁令往往比冗长的建议更有效。例如，写入“绝对不要使用基于类的视图（Do NOT use class-based views）”，只需耗费极少的 Token 就能彻底封杀一种过时的编码模式，这比长篇大论地解释应该使用什么模式要高效得多 16。  
2. **利用 Glob 模式进行精准匹配**：优秀的规则管理必须做到按需加载。在 Cursor 等现代 IDE 中，用户可以通过配置特定的 Glob 模式（如 \*\*/\*.py），确保 Python 的代码格式化规则只在智能体编辑 Python 文件时才被注入上下文，而绝不会在编辑 CSS 文件时造成干扰 16。  
3. **指向性引用优于直接嵌入**：不要在规则文件中大段复制现有的代码结构。相反，应该使用指向性语言，例如：“关于数据库连接池的实现规范，请严格参考 @/src/db/connection\_template.ts 中的标准示例”。智能体具有自主阅读文件的能力，通过提供规范锚点，既能保持全局规则文件的轻量化，又能防止由于代码库迭代导致的规则过时 22。

### **将复杂工作流剥离为技能（Skills）与子智能体**

当面对一些高度专业化、但非高频触发的任务时，将其放入全局规则是一种极大的浪费。跨平台的通用解决方案是利用“技能（Skills）”或“子智能体（Sub-agents）”架构进行上下文隔离 5。

无论是 Trae Skills 还是 Copilot Agent Skills，其核心理念都是将特定的操作规范（例如：“如何格式化并导出 CSV 数据分析报告”、“如何审查代码的安全漏洞”）封装成独立的文件或提示词模板。这些技能平时处于休眠状态，不消耗任何主会话的上下文预算。只有当用户明确使用特定的斜杠命令（如 /csv-export）唤醒时，相关的上下文才会被加载 37。同样，在 Claude Code 的工作流中，资深开发者习惯于派遣独立的子智能体去执行耗时且需要扫描大量文件的检索任务，子智能体在自己独立的沙箱上下文中完成阅读，并将最终的精炼摘要返回给主智能体。这种将繁杂的背景资料剥离至外部按需调用的习惯，是保持主控模型思维清晰的关键 26。

## **大规模材料与外部文档的动态挂载**

在现代软件开发中，集成全新的第三方库或处理未被模型训练数据覆盖的最新框架是家常便饭。这必然要求将大规模的外部文档引入智能体的视野。如何高效地完成这一动作，是上下文工程中极具挑战性的一环。

### **摒弃蛮力粘贴，拥抱动态引用（@ 提及与 MCP）**

正如前文在反模式中所述，将数百行的官方文档直接复制粘贴到当前聊天框中不仅显得笨拙，而且极其低效。这种习惯使得文本以非结构化的形式呈现，极易引发解析错误 40。

高效率且跨产品的通用习惯是：**大材料绝对不要每轮重贴，利用动态指针进行挂载**。无论是在 Windsurf、Cursor 还是 Continue 中，开发者都习惯通过 @ 符号（如 @Docs, @Web, @README.md）将外部资源或本地大型文件动态绑定到当前提示词中 32。当系统检测到这些标签时，它会在后台通过 RAG（检索增强生成）机制，智能地提取最相关的内容片段送入模型，而不是盲目地堆砌全文。

更进一步，为了应对 LLM 在处理大规模文档时的局限性，业界正在快速普及 llms.txt 协议标准 17。高水平的开发者在引入新框架时，不再去搜寻适合人类阅读的 HTML 页面，而是直接寻找该框架官方提供的 llms.txt 或 llms-full.txt 文件。这些文件是专门为机器阅读而优化的 Markdown 文本，剥离了所有视觉噪音。通过配置 MCP（模型上下文协议）服务器（如 Context7），开发者可以让智能体在需要时，通过标准化的接口安全、精确地查询这些外部文档字典。这不仅保证了文档引用的绝对准确性，也彻底解放了开发者手动搬运资料的双手 32。

| 处理大规模外部文档的方式 | 对上下文的影响 | 效率评估 | 适用场景 |
| :---- | :---- | :---- | :---- |
| **直接全文复制粘贴到输入框** | 极差。迅速耗尽 Token，污染历史记录，导致视觉与检索灾难。 | 极低，频繁触发幻觉 | 仅限不足 50 行的极小片段。 |
| **使用 @File 手动批量挂载文件** | 中等。若一次性挂载过多无关文件，会引发“注意力稀释”。 | 中等 | 需要跨越 2-3 个紧密相关的核心本地文件。 |
| **依赖本地 RepoMap（如 Aider）/ 语义搜索** | 优。仅检索高关联度片段，大幅压缩所需 Token。 | 高，节省预算 | 在庞大代码库中寻找未知依赖项或函数签名。 |
| **通过 MCP 服务器挂载 llms.txt** | 极优。提供完全结构化、无噪音、专为机器优化的标准知识库。 | 极高，消除时效性盲区 | 引入全新的第三方框架、复杂的 API 规范时。 |

### **谨慎看待全局库搜索的局限性**

尽管诸如 Aider 的 RepoMap 或 Continue 的语义搜索（Semantic Search）等底层技术大大缓解了代码发现的难题，但优秀的上下文管理者从不完全依赖于此 30。语义搜索有时无法捕捉到极其复杂的跨文件非显式依赖关系。因此，即使系统拥有全库搜索能力，高水平用户依然保持着主动筛选的习惯：如果明确知道问题发生在某三个文件中，他们会手动使用 @ 将这三个文件挂载至当前上下文，以此来强制收束模型的搜索空间，而不是放任模型在一个包含上万个文件的庞大知识图谱中漫无目的地自行摸索 16。

## **经济性控制、模型路由与自动化验证体系**

在编码智能体的深度使用中，财务成本与输出质量之间存在着微妙的平衡。聪明的上下文管理习惯不仅能提升代码质量，更能产生显著的经济效益。

### **小任务不要默认高价模型（智能模型路由）**

目前市面上的主流智能体大多集成了多家大模型底座。初级用户的一个通病是：无论是进行宏大的架构设计，还是仅仅修改一处拼写错误，都默认使用参数量最大、单价最昂贵的旗舰模型（如 Claude 3.5 Sonnet 或 GPT-4o）。鉴于上下文计费是按 Token 数量线性增长的，这种习惯在长线项目中将造成惊人的资金浪费 3。

既省钱又减少返工的绝佳习惯是：**基于任务复杂度进行动态的模型路由** 12。当面临探索未知代码库、处理复杂的并发逻辑、或进行跨模块深度重构等需要极高推理能力的任务时，毫不犹豫地启用旗舰模型，以确保方向的正确性。然而，一旦核心逻辑被理清，并且所需的上下文已经被精心修剪和固化（例如，已经生成了详细的实现计划或摘要文件），面对后续的生成样板代码、补充单元测试用例、或者进行局部的语法转换等结构化明确的小任务时，高水平用户会果断切换至响应更快、成本极低的轻量级模型（如 Claude 3.5 Haiku、Gemini 1.5 Flash 或 DeepSeek Coder） 12。由于前期的上下文工程已经排除了所有的环境干扰，即使是轻量级模型也能在这种纯净的输入下产生完美的输出。

### **工具和联网权限的按需克制**

如前所提及，给予智能体过多自由往往适得其反。在进行基础的代码逻辑推演或架构梳理时，**坚决关闭 Web Search（网络搜索）等非必要工具权限**是一个极其重要却常被忽略的好习惯 18。

当联网工具处于默认开启状态时，模型一旦在本地代码中遇到细微的不确定性，就容易产生依赖心理，主动触发网页搜索。抓取回来的网页通常充满了冗余的导航菜单、广告脚本和与当前版本不符的过时代码片段。这不仅白白浪费了 API 调用成本，更严重的是，它稀释了模型原本集中在本地业务逻辑上的注意力。因此，稳定且高效的工作流要求：平时将智能体封锁在纯粹的本地代码阅读和编辑权限内；只有当明确诊断出模型缺乏某种特定的外部领域知识，且无法通过本地文档补全时，才短暂地为其开启特定的 MCP 接口或精确的搜索权限，获取所需信息后立即再次关闭 18。

### **构建自验证闭环以对抗漂移**

最后，无论上下文管理得多么精妙，随着对话深度的增加，LLM 固有的概率生成特性总有出错的可能。为了防止这种错误被悄然引入代码库，高水平的上下文管理范式必须延伸到外部验证环境，形成闭环。

单纯依靠智能体的内部推理来保证正确性是不可靠的（即“信任但不核实”的坏习惯）。卓越的习惯是：**为智能体提供独立验证其自身工作的手段** 7。 这通常通过以下几种方式实现： 首先是测试驱动开发（TDD）的强制推行。在请求功能代码之前，先命令智能体编写针对该功能的测试用例并执行。只有在测试明确失败后，智能体才被允许修改业务逻辑，直到所有测试通过为止。这种方式将模糊的需求上下文转化为绝对精确、可执行的代码契约 13。 其次，引入带有强阻断机制的 Post-Save Hooks（保存后钩子）。当智能体自动保存修改后的文件时，系统后台立即触发代码检查工具（Linter）或构建编译器。如果发现语法错误或违反全局规范，系统会返回非零的退出码（如 Exit Code 2），并强制阻止智能体继续下一步行动。智能体必须读取错误堆栈并修正代码，直到获得绿色的通行信号 54。这种利用外部确定性工具来勒紧智能体行为边界的做法，彻底根除了由于上下文漂移而产生的低级结构性破坏。

## **结论**

从上述深度剖析中可以清晰地得出结论：在当前的大语言模型技术范式下，真正区分顶尖 AI 开发者与普通用户的分水岭，绝非其掌握了多少所谓神奇的“提示词模板”，而是其是否建立了一套像管理服务器内存般严密的“上下文工程”纪律。

那些几乎适用于所有编码智能体的核心好习惯，本质上都是在与模型不可避免的“熵增”和“注意力耗散”作斗争。通过将大型任务切分为带有明确终止边界的短会话 5，利用摘要文件在不同会话间传递高密度的核心状态 25，开发者成功地绕过了长会话带来的灾难性认知污染。通过在修正错误时坚持回退并修改初始提示词，而不是顺延着对话流进行追加斥责，他们保持了输入信号的极度纯净 14。通过将庞杂的项目知识从每次的聊天输入中剥离，转而固化为轻量级的全局规则、动态调用的能力节点（Skills）以及基于 llms.txt 标准的机器优化文档 17，他们既赋予了智能体全景的视野，又守住了宝贵的上下文计算预算。最后，通过审慎地进行模型降级路由和严格钳制外部网络工具的滥用，开发者不仅实现了降本增效，更构建了一个稳定、可控且高效的人机协同编码体系 12。随着智能体自主性的进一步提升，这种对上下文环境的精密控制与收敛能力，将成为下一代软件工程师最不可或缺的核心竞争力。

#### **Works cited**

1. Stingy Context: 18:1 Code compression for LLM auto-coding (arXiv) : r/LLMDevs \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/LLMDevs/comments/1qzaq2l/stingy\_context\_181\_code\_compression\_for\_llm/](https://www.reddit.com/r/LLMDevs/comments/1qzaq2l/stingy_context_181_code_compression_for_llm/)  
2. Context Engineering: The Bottleneck to Better Continuous AI, accessed on April 7, 2026, [https://blog.continue.dev/context-engineering-the-bottleneck-to-better-continuous-ai](https://blog.continue.dev/context-engineering-the-bottleneck-to-better-continuous-ai)  
3. How do you manage AI Agent costs? Blew $135 in a week and need some pro tips. \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/cursor/comments/1o5k1ck/how\_do\_you\_manage\_ai\_agent\_costs\_blew\_135\_in\_a/](https://www.reddit.com/r/cursor/comments/1o5k1ck/how_do_you_manage_ai_agent_costs_blew_135_in_a/)  
4. Why your AI agent gets worse as your project grows (and how I fixed it) \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/vibecoding/comments/1rfh64q/why\_your\_ai\_agent\_gets\_worse\_as\_your\_project/](https://www.reddit.com/r/vibecoding/comments/1rfh64q/why_your_ai_agent_gets_worse_as_your_project/)  
5. Context Rot: Why Your AI Agent Gets Worse Over Time \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/Trae\_ai/comments/1rqt4lf/context\_rot\_why\_your\_ai\_agent\_gets\_worse\_over\_time/](https://www.reddit.com/r/Trae_ai/comments/1rqt4lf/context_rot_why_your_ai_agent_gets_worse_over_time/)  
6. Anthropic just showed how to make AI agents work on long projects without falling apart, accessed on April 7, 2026, [https://www.reddit.com/r/LocalLLaMA/comments/1p7siuu/anthropic\_just\_showed\_how\_to\_make\_ai\_agents\_work/](https://www.reddit.com/r/LocalLLaMA/comments/1p7siuu/anthropic_just_showed_how_to_make_ai_agents_work/)  
7. Our Agent Rebuilt Itself in 26 Hours. AMA : r/ChatGPTCoding \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/ChatGPTCoding/comments/1qo3se2/our\_agent\_rebuilt\_itself\_in\_26\_hours\_ama/](https://www.reddit.com/r/ChatGPTCoding/comments/1qo3se2/our_agent_rebuilt_itself_in_26_hours_ama/)  
8. Forget Prompt Engineering. Protocol Engineering is the Future of Claude Projects. : r/ClaudeAI \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/ClaudeAI/comments/1lrp4b0/forget\_prompt\_engineering\_protocol\_engineering\_is/](https://www.reddit.com/r/ClaudeAI/comments/1lrp4b0/forget_prompt_engineering_protocol_engineering_is/)  
9. Vibe Coding Services \- instinctools, accessed on April 7, 2026, [https://www.instinctools.com/vibe-coding-services/](https://www.instinctools.com/vibe-coding-services/)  
10. Is vibe coding just a beautiful trap? Built nothing for months and I can't stop starting over : r/codex \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/codex/comments/1rj7mfv/is\_vibe\_coding\_just\_a\_beautiful\_trap\_built/](https://www.reddit.com/r/codex/comments/1rj7mfv/is_vibe_coding_just_a_beautiful_trap_built/)  
11. Intelligent Context Condensing | Roo Code Documentation, accessed on April 7, 2026, [https://docs.roocode.com/features/intelligent-context-condensing](https://docs.roocode.com/features/intelligent-context-condensing)  
12. The End of Context Amnesia: Cline's Visual Solution to Context Management \- Cline Blog, accessed on April 7, 2026, [https://cline.bot/blog/understanding-the-new-context-window-progress-bar-in-cline](https://cline.bot/blog/understanding-the-new-context-window-progress-bar-in-cline)  
13. Best practices for coding with agents · Cursor, accessed on April 7, 2026, [https://cursor.com/blog/agent-best-practices](https://cursor.com/blog/agent-best-practices)  
14. Aider \- A new Gemini pro 2.5 just ate sonnet 3.7 thinking like a snack ;-) : r/LocalLLaMA, accessed on April 7, 2026, [https://www.reddit.com/r/LocalLLaMA/comments/1jjv68r/aider\_a\_new\_gemini\_pro\_25\_just\_ate\_sonnet\_37/](https://www.reddit.com/r/LocalLLaMA/comments/1jjv68r/aider_a_new_gemini_pro_25_just_ate_sonnet_37/)  
15. Recently, I have been trying out LLMs for coding and they are surprisingly bad for anything even a tiny bit complex? : r/webdev \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/webdev/comments/1h7wmcg/recently\_i\_have\_been\_trying\_out\_llms\_for\_coding/](https://www.reddit.com/r/webdev/comments/1h7wmcg/recently_i_have_been_trying_out_llms_for_coding/)  
16. Context Management Strategies for Cursor: A Complete Guide to the AI-Native Code Editor, accessed on April 7, 2026, [https://datalakehousehub.com/blog/2026-03-context-management-cursor/](https://datalakehousehub.com/blog/2026-03-context-management-cursor/)  
17. Best llms.txt implementation platforms for AI-discoverable APIs in January 2026 \- Fern, accessed on April 7, 2026, [https://buildwithfern.com/post/best-llms-txt-implementation-platforms-ai-discoverable-apis](https://buildwithfern.com/post/best-llms-txt-implementation-platforms-ai-discoverable-apis)  
18. EVMbench: Evaluating AI Agents on Smart Contract Security \- arXiv, accessed on April 7, 2026, [https://arxiv.org/html/2603.04915v1](https://arxiv.org/html/2603.04915v1)  
19. EVMbench: Evaluating AI Agents on Smart Contract Security \- OpenAI, accessed on April 7, 2026, [https://cdn.openai.com/evmbench/evmbench.pdf](https://cdn.openai.com/evmbench/evmbench.pdf)  
20. Web search with the Responses API \- Azure \- Microsoft Learn, accessed on April 7, 2026, [https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/web-search](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/web-search)  
21. Best Practices for Context Management when Generating Code with AI Agents, accessed on April 7, 2026, [https://docs.digitalocean.com/products/gradient-ai-platform/concepts/context-management/](https://docs.digitalocean.com/products/gradient-ai-platform/concepts/context-management/)  
22. Best Practices for Claude Code \- Claude Code Docs, accessed on April 7, 2026, [https://code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices)  
23. Windsurf best practices : r/Codeium \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/Codeium/comments/1h2psgy/windsurf\_best\_practices/](https://www.reddit.com/r/Codeium/comments/1h2psgy/windsurf_best_practices/)  
24. Read a software engineering blog if you think vibe coding is the future : r/vibecoding \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/vibecoding/comments/1kprxpl/read\_a\_software\_engineering\_blog\_if\_you\_think/](https://www.reddit.com/r/vibecoding/comments/1kprxpl/read_a_software_engineering_blog_if_you_think/)  
25. Beyond Prompts: 4 Context Engineering Secrets for Claude Code | The road \- kane.mx, accessed on April 7, 2026, [https://kane.mx/posts/2025/context-engineering-secrets-claude-code/](https://kane.mx/posts/2025/context-engineering-secrets-claude-code/)  
26. How are you guys managing context in Claude Code? 200K just ain't cutting it. \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/ClaudeAI/comments/1rrkv0h/how\_are\_you\_guys\_managing\_context\_in\_claude\_code/](https://www.reddit.com/r/ClaudeAI/comments/1rrkv0h/how_are_you_guys_managing_context_in_claude_code/)  
27. I read 17 papers on agentic AI workflows. Most Claude Code advice is measurably wrong : r/ClaudeAI \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/ClaudeAI/comments/1s8mbqm/i\_read\_17\_papers\_on\_agentic\_ai\_workflows\_most/](https://www.reddit.com/r/ClaudeAI/comments/1s8mbqm/i_read_17_papers_on_agentic_ai_workflows_most/)  
28. Department of Defense Gateway Information System (DGIS) Users' Guide \- DTIC, accessed on April 7, 2026, [https://apps.dtic.mil/sti/tr/pdf/ADA270140.pdf](https://apps.dtic.mil/sti/tr/pdf/ADA270140.pdf)  
29. I X USER'S MANUAL \- Bitsavers.org, accessed on April 7, 2026, [https://bitsavers.org/pdf/usenix/Usenix\_BSD\_Manuals/4.2\_2nd\_printing\_198412/Unix\_Users\_Manual\_Supplementary\_Documents\_198403.pdf](https://bitsavers.org/pdf/usenix/Usenix_BSD_Manuals/4.2_2nd_printing_198412/Unix_Users_Manual_Supplementary_Documents_198403.pdf)  
30. Tips \- Aider, accessed on April 7, 2026, [https://aider.chat/docs/usage/tips.html](https://aider.chat/docs/usage/tips.html)  
31. Context Management : r/CLine \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/CLine/comments/1i6oevd/context\_management/](https://www.reddit.com/r/CLine/comments/1i6oevd/context_management/)  
32. Context Providers | Continue Docs, accessed on April 7, 2026, [https://docs.continue.dev/customize/deep-dives/custom-providers](https://docs.continue.dev/customize/deep-dives/custom-providers)  
33. Mastering Cursor IDE: 10 Best Practices (Building a Daily Task Manager App) \- Medium, accessed on April 7, 2026, [https://medium.com/@roberto.g.infante/mastering-cursor-ide-10-best-practices-building-a-daily-task-manager-app-0b26524411c1](https://medium.com/@roberto.g.infante/mastering-cursor-ide-10-best-practices-building-a-daily-task-manager-app-0b26524411c1)  
34. Rules | Cursor Docs, accessed on April 7, 2026, [https://cursor.com/docs/rules](https://cursor.com/docs/rules)  
35. Use custom instructions in VS Code, accessed on April 7, 2026, [https://code.visualstudio.com/docs/copilot/customization/custom-instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)  
36. Manage context and memory | Gemini CLI, accessed on April 7, 2026, [https://geminicli.com/docs/cli/tutorials/memory-management/](https://geminicli.com/docs/cli/tutorials/memory-management/)  
37. What differentiates on Custom Agents, Agent Skills and Custom Instructions in Copilot CLI · community · Discussion \#183962 \- GitHub, accessed on April 7, 2026, [https://github.com/orgs/community/discussions/183962](https://github.com/orgs/community/discussions/183962)  
38. Best Practices for Agent Skills in TRAE | TRAE \- Collaborate with ..., accessed on April 7, 2026, [https://www.trae.ai/blog/trae\_tutorial\_0115?v=1](https://www.trae.ai/blog/trae_tutorial_0115?v=1)  
39. Mastering Context Management in Claude Code: 8 Tips That Changed My Workflow | by manav ghosh | Medium, accessed on April 7, 2026, [https://medium.com/@manavghosh/mastering-context-management-in-claude-code-8-tips-that-changed-my-workflow-9ce88ca4db39](https://medium.com/@manavghosh/mastering-context-management-in-claude-code-8-tips-that-changed-my-workflow-9ce88ca4db39)  
40. The Smarter Way to Code: Stop Copy-Pasting and Start Reusing \- DEV Community, accessed on April 7, 2026, [https://dev.to/nisarthpatel/the-smarter-way-to-code-stop-copy-pasting-and-start-reusing-1mk1](https://dev.to/nisarthpatel/the-smarter-way-to-code-stop-copy-pasting-and-start-reusing-1mk1)  
41. How to Give Windsurf the Right Context for Smarter AI Coding \- Kaizen Softworks, accessed on April 7, 2026, [https://www.kzsoftworks.com/blog/how-to-give-windsurf-the-right-context-for-smarter-ai-coding](https://www.kzsoftworks.com/blog/how-to-give-windsurf-the-right-context-for-smarter-ai-coding)  
42. Our Guide to Master Context in Trae : r/Trae\_ai \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/Trae\_ai/comments/1l3lvno/our\_guide\_to\_master\_context\_in\_trae/](https://www.reddit.com/r/Trae_ai/comments/1l3lvno/our_guide_to_master_context_in_trae/)  
43. Working with llms.txt | Platform Overview \- Mastercard Developers, accessed on April 7, 2026, [https://developer.mastercard.com/platform/documentation/agent-toolkit/working-with-llmstxt/](https://developer.mastercard.com/platform/documentation/agent-toolkit/working-with-llmstxt/)  
44. What Is LLMs.txt? The Guide To AI Search & GEO \- Yotpo, accessed on April 7, 2026, [https://www.yotpo.com/blog/what-is-llms-txt/](https://www.yotpo.com/blog/what-is-llms-txt/)  
45. Claude Code Works Better When You Do This, accessed on April 7, 2026, [https://www.youtube.com/watch?v=D5bRTv6GhXk](https://www.youtube.com/watch?v=D5bRTv6GhXk)  
46. What is llms.txt? Breaking down the skepticism \- Mintlify, accessed on April 7, 2026, [https://www.mintlify.com/blog/what-is-llms-txt](https://www.mintlify.com/blog/what-is-llms-txt)  
47. How to expose any documentation to any LLM agent \- Eric J. Ma's Personal Site \- Eric Ma, accessed on April 7, 2026, [https://ericmjl.github.io/blog/2025/10/19/how-to-expose-any-documentation-to-any-llm-agent/](https://ericmjl.github.io/blog/2025/10/19/how-to-expose-any-documentation-to-any-llm-agent/)  
48. How do you provide documentation to your AI? : r/ChatGPTCoding \- Reddit, accessed on April 7, 2026, [https://www.reddit.com/r/ChatGPTCoding/comments/1jr9atv/how\_do\_you\_provide\_documentation\_to\_your\_ai/](https://www.reddit.com/r/ChatGPTCoding/comments/1jr9atv/how_do_you_provide_documentation_to_your_ai/)  
49. Beyond file trees: why AI coding assistants need smarter context \- Nuanced.dev, accessed on April 7, 2026, [https://www.nuanced.dev/blog/beyond-file-trees](https://www.nuanced.dev/blog/beyond-file-trees)  
50. LocAgent: Graph-Guided LLM Agents for Code Localization \- arXiv, accessed on April 7, 2026, [https://arxiv.org/html/2503.09089v2](https://arxiv.org/html/2503.09089v2)  
51. Cutting Through the Noise: Smarter Context Management for LLM-Powered Agents, accessed on April 7, 2026, [https://blog.jetbrains.com/research/2025/12/efficient-context-management/](https://blog.jetbrains.com/research/2025/12/efficient-context-management/)  
52. How I Save Over 50% of My Claude Code Context (12 Rules), accessed on April 7, 2026, [https://www.youtube.com/watch?v=7nP2wjGcIXs](https://www.youtube.com/watch?v=7nP2wjGcIXs)  
53. Metaprompts: Your Secret Superpower to Prompt Engineering \- jeffreybowdoin.com, accessed on April 7, 2026, [https://jeffreybowdoin.com/metaprompt-101/](https://jeffreybowdoin.com/metaprompt-101/)  
54. Automated Code Validation: How Post-Save Hooks Turn AI Into a Reliable Coding Partner, accessed on April 7, 2026, [https://medium.com/@peterphonix/automated-code-validation-how-post-save-hooks-turn-ai-into-a-reliable-coding-partner-567beb5bca1c](https://medium.com/@peterphonix/automated-code-validation-how-post-save-hooks-turn-ai-into-a-reliable-coding-partner-567beb5bca1c)