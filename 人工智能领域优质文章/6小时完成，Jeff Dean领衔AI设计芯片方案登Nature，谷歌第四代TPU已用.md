# 6小时完成，Jeff Dean领衔AI设计芯片方案登Nature，谷歌第四代TPU已用

> 将芯片的布局规划看作一个深度强化学习问题，谷歌大脑团队希望用 AI 来提升芯片设计效率。基于 AI 的最新设计方案可以在数小时内完成人类设计师耗费数月才能完成的芯片布局，这将有可能引领一场新的芯片效率革命。

2020 年 4 月，包括 Google AI 负责人 Jeff Dean 在内的谷歌大脑研究者描述了一种基于 AI 的芯片设计方法，该方法可以从过往经验中学习并随时间推移不断改进，从而能够更好地生成不可见（unseen）组件的架构。据他们表示，这种基于 AI 的方法平均可以在 6 小时内完成设计，这要比人类专家所需要的数周时间快得多。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184041.png)

*左为人类设计的微芯片布局，右为机器学习系统设计的微芯片布局。图源：Nature*

近日，谷歌大脑团队联合斯坦福大学的研究者对这一基于 AI 的芯片设计方法进行了改进，并将其应用于不久前 *Google I/O 2021 大会*上正式发布的、下一代张量处理单元（TPU v4）加速器的产品中。谷歌此前表示，TPUv4 可以在目标检测、图像分类、自然语言处理、机器翻译和推荐基准等工作负载上优于上一代 TPU 产品。

相关论文研究已经在 Nature 上发表，Jeff Dean 为核心作者之一。据介绍，在不到六小时的时间内，谷歌 AI 芯片设计方法自动生成的芯片布局在功耗、性能和芯片面积等所有关键指标上都优于或媲美人类，而工程师需要耗费数月的艰苦努力才能达到类似效果。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184108.png)

论文地址：[https://www.nature.com/articles/s41586-021-03544-w](https://www.nature.com/articles/s41586-021-03544-w)

这项基于强化学习的快速芯片设计方法对于资金紧张的初创企业大有裨益，如果谷歌公开相关技术的话，这些初创企业可以开发自己的 AI 和其他专用芯片。并且，这种方法有助于缩短芯片设计周期，从而使得硬件可以更好地适应快速发展的技术研究。

**技术详解**

芯片布局是设计计算机芯片物理布局的一项重要工程任务。在电子设计自动化（EDA）出现之前，设计人员必须手工完成集成电路的设计、布线等工作，到了 1970 年代中期，开发人员尝试将整个设计过程自动化。此后，第一个电路布局布线工具研发成功，设计自动化研讨会（Design Automation Conference）在这一时期被创立。电子设计自动化发展的下一个重要阶段以卡弗尔 · 米德（Carver Mead）和琳 · 康维于 1980 年发表的论文《超大规模集成电路系统导论》，提出了通过编程语言来进行芯片设计的新思想。从 1981 年开始，电子设计自动化逐渐开始商业化。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184142.png)

尽管历经了 50 年的相关研究，芯片布局仍与自动化背道而驰，需要物理设计工程师数月的艰苦努力才能生产出可制造的布局。基于此，谷歌研究者提出了一种用于芯片布局设计的深度强化学习（RL）方法。就其效果而言，**在不到六小时的时间内，谷歌设计方法自动生成的芯片布局在功耗、性能和芯片面积等所有关键指标上都优于或媲美人类**。

具体而言，为了实现这一目标，研究者将芯片布局作为一个强化学习问题，开发了一种基于边缘、能够学习芯片丰富且可迁移表示的图卷积神经网络架构。这种方法能够更好地利用过往的经验，从而更好更快地解决问题的新实例，使得芯片设计由比任何人类设计师具备更多经验的人工智能体执行。

此外，这种方法可被用于设计谷歌下一代人工智能加速器，并且有可能为它们节省数千小时的人力。研究者相信，更强大的 AI 设计的硬件将推动人工智能领域的进步，并在这两个领域之间建立一种共生关系。

**设计域 - 自适应策略**

为芯片布局规划开发域 - 自适应策略是非常具有挑战性的，因为该问题类似于具有不同棋子、棋盘和获胜条件的游戏，元件是「棋子」（例如，网表拓扑、宏计数、宏大小和纵横比）、放置元件的画布是「棋盘」（画布大小和长宽比）、赢的条件（不同的评估指标或不同的密度和路由拥塞约束的相对重要性）。即使是游戏的一个实例（将一个特定的网表放到一个特定的画布上）也有一个巨大的状态 - 动作空间，对全局形成影响。为了应对这个挑战，研究者首先集中学习状态空间的丰富表示。

研究者训练了一个神经网络架构，能够预测新网表放置的奖励，最终目标是将此架构用作整个策略的编码层。

为了训练这种监督模型，需要大量的芯片放置数据集及其相应的奖励标签。因此，研究者创建了一个**包含 10000 个芯片放置的数据集**，其中输入是与给定放置相关的状态，标签是该放置的奖励。为了准确预测奖励标签，将其泛化到未看到的数据中，研究者提出了一种基于边缘的图神经网络架构，即 Edge-GNN（基于边缘图神经网络）。该网络的作用是将网表嵌入，将节点类型和连通性的信息提取到低维向量表示中，以用于下游任务。

监督模型通过回归进行训练，以最小化均方损失的加权和。监督任务使研究者能够找到在网表中推广奖励预测所需的特征和架构。为了将 Edge-GNN 合并到 RL 策略网络中，该研究移除了预测层，然后将其用作策略网络的编码器，如图所示。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184207.png)

*策略网络和价值网络体系架构。*

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184221.png)

*训练方法和训练方案。*

谷歌团队的系统从一个空芯片开始，按顺序放置组件，直到完成网表。为了指导系统选择先放置哪些组件，组件按大小递减进行排序，研究人员先放置大的组件，这样可以减少之后无法放置的可能性。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184247.png)

开源 RISC-V 处理器 Ariane 的宏布局随着训练进程的变化情况。左边：从头开始训练；右边：正在为这个芯片调整预训练策略。每个矩形代表一个单独的宏位置。

**自适应结果**

下图 3 中，研究人员比较了使用预训练策略生成的放置质量，以及通过从头开始训练策略生成的放置质量，训练数据集由 TPU 块和开源 Ariane RISC-V CPU 块组成，在每一个实验中，都对除目标块外的所有块预训练策略。

研究人员展示了零样本模式的结果，以及在特定设计上对预训练策略进行 2 小时和 12 小时微调后的结果。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184306.png)

从头开始训练的策略收敛时间要长得多，即使在 24 小时后（根据奖励函数评估），结果也比微调策略在 12h 内达到的效果还要差。这表明，在预训练期间接触许多不同的设计可以更快地为新的看不见的块生成更高质量的放置方案。

图 4 显示了从头开始训练与来自 Ariane RISC-V CPU30 的预训练策略网络的训练的收敛图。结果显示预训练策略不仅具有较低的放置成本，而且能比从头开始训练的策略收敛速度快 30 小时以上。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184325.png)

下图 5 展示了规模更大的训练集对性能的影响。研究者依次将训练集从 2 个 TPU 块增至 5 个，最后增至 20 个，策略网络在零样本和经过相同小时数的微调后均能生成更好的芯片布局。这表明，在将策略网络应用于更多不同类型的芯片设计过程中，它不易于出现过拟合，并且能够更好地泛化至新的未知芯片设计。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184341.png)

**与基线方法对比**

研究者将该方法与当前 SOTA 方法以及人类设计师团队完成的上一代 TPU 的产品设计进行了对比。为了公平起见，研究者确保所有方法使用相同的实验设置，包括相同的输入和 EDA 工具设置，并使用在最大数据集（20 个 TPU 块）上预先训练的 AI 策略，接着在 5 个目标不可见块上微调（时间少于 6 小时）。

结果如下表 1 所示，表明基于 AI 的方法在生成满足设计要求的高质量芯片布局方面是有效的，在面积、功率和线长方面均优于或媲美人类专家手动设计的效果。

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210612184357.png)

**为芯片设计过程的完全自动化奠定基础**

谷歌称其系统泛化和生成高质量解决方案的能力具有重大影响，为芯片设计过程的早期优化提供了机会。以前，大规模的架构探索是不可能的，因为评估一个给定的架构候选需要花费数月的努力。然而，谷歌团队指出，修改芯片设计可能会对性能产生巨大影响，并可能为芯片设计过程的完全自动化奠定基础。

此外，由于谷歌团队的系统只是学习将一个图的节点映射到一组资源上，因此它可能适用于包括城市规划、疫苗测试和分发以及大脑皮层映射在内的一系列应用。

研究人员在论文中写道：「（虽然）我们的方法已经在生产中被用于设计下一代谷歌 TPU…… 我们相信，（它）可以应用于芯片设计以外的有影响力的布局问题。」

![](https://maoxianxin1996.oss-accelerate.aliyuncs.com/codechina/20210608112105.png)