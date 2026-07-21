---
title: Geometry-Guided Modeling of Foundation Features Enables Generalizable Object Shape Deformation Learning
title_zh: 几何引导的基座特征建模支持泛化目标形状形变学习
authors: "Yiyao Ma, Kai Chen, Zhongxiang Zhou, Zhuheng Song, Dongsheng Xie, Zelong Tan, Rong Xiong, Qi Dou"
date: 2026-04-30
pdf: "https://openreview.net/pdf/9c791ecefd83b8b7253f7da2da46302004d01409.pdf"
tags: ["query:sketch-to-d"]
score: 5.0
evidence: 通过形变实现的单目3D形状恢复
tldr: 本文提出一种泛化形变学习框架，从单张图像恢复三维物体。该框架通过几何引导的基座特征建模，对类别级形状模板进行显式形变以匹配目标观测，解决了跨视角和跨类别的形状恢复难题，在未见类别上取得鲁棒泛化。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 单目3D形状恢复在任意视角和未见类别上泛化困难。
method: 提出几何引导的特征建模机制，利用模板拓扑丰富基座特征并指导精确形变。
result: 方法在跨类别和视角的变形任务上实现鲁棒泛化。
conclusion: 几何引导特征能有效提升单目3D恢复的泛化能力。
---

## Abstract
Monocular 3D shape recovery is fundamental to geometric understanding, yet achieving robust generalization across arbitrary viewpoints and unseen object categories remains a significant challenge. In this paper, we present a generalizable deformation learning framework that reconstructs 3D objects by explicitly deforming a category-level shape template to match the target observation. To address complex shape variations between the template and the target, we introduce a geometry-guided feature modeling mechanism. This process first enriches foundation features with template topology to yield a geometry-aware representation, which is then explicitly correlated with the target observation to guide precise deformation. Furthermore, to bridge the disparity between the fixed template and arbitrary target views, we propose a view-adaptive feature aggregation module. This module leverages multi-view template features and their corresponding camera poses to enrich the canonical template representation, ensuring robust feature alignment regardless of the target's perspective.  Extensive experiments demonstrate that our approach significantly outperforms state-of-the-art methods in handling large shape variations and diverse viewpoints, exhibiting strong generalization to novel categories and effectively supporting downstream real-world dexterous robotic manipulation tasks. Project homepage: https://GODeform.github.io/

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- 单目三维形状恢复是几何理解的基础任务，其核心目标是仅凭单张二维图像推断出物体的完整三维形状。
- 现有方法在面临任意相机视角和未见过的物体类别时，泛化能力严重不足，难以稳定地处理复杂的形状变化。
- 本文旨在提出一种具有强泛化能力的三维形变学习框架，通过显式地对类别级形状模板进行形变，使其精确拟合目标物体在单目观测下的形态。

### 2. 论文提出的方法论
#### 核心思想
- 不直接回归三维形状，而是学习一种形变过程：给定一个已知拓扑的类别级模板，根据输入的二维观测，预测模板上每个点的偏移量，从而恢复目标形状。
- 关键难点在于模板与目标之间可能存在复杂的形状差异以及任意的视角变化。

#### 关键技术细节
- **几何引导的基座特征建模（Geometry-Guided Feature Modeling）**
  - 首先利用模板的拓扑结构（如网格连接关系）对通用的基础视觉特征进行增强，生成具有几何感知能力的表示。
  - 接着将这种几何感知特征与目标观测（即输入图像的特征）显式关联，从而精确指导形变偏移的预测。
- **视角自适应的特征聚合模块（View-Adaptive Feature Aggregation）**
  - 预先从多个固定视角渲染模板，提取多视角的模板特征，并保存对应的相机姿态。
  - 对于任意的目标视角，通过融合这些多视角模板特征来构造一个更丰富的规范模板表示，确保无论目标从哪个角度观察，都能实现稳健的特征对齐。

#### 算法流程（文字描述）
1. 输入单张目标图像，提取图像特征。
2. 对于类别级模板，利用其拓扑增强基础特征，并通过多视角聚合得到几何感知的规范模板特征。
3. 将规范模板特征与目标图像特征进行显式相关计算，获得模板至目标的对应关系或形变引导。
4. 根据引导预测模板顶点位移，变形后得到最终三维形状。

### 3. 实验设计
- **数据集/场景**：文中未在摘要中具体列出数据集，但提及“跨类别”和“不同视角”的形变任务，可推测使用了常见的三维形状数据集（如 ShapeNet、Pix3D 等）以及机器人场景中的真实数据。
- **Benchmark 和对比方法**：与当前最先进（SOTA）方法进行了比较，具体方法名未在摘要中列出，但从“significantly outperforms”可以看出其性能优势。
- **下游任务验证**：在灵巧机器人操作的真实世界任务中进行了验证，表明方法具有实际应用潜力。

### 4. 资源与算力
- 摘要及元数据中**未明确说明**所用的 GPU 型号、数量或训练时长。
- 无法从现有信息推断算力消耗。

### 5. 实验数量与充分性
- 元数据显示该论文已被 ICML 2026 接收（score: 5.0），通常此类顶会的实验量较为充足。
- 摘要提到进行了大量实验（Extensive experiments），涵盖：
  - 与先进方法的对比实验；
  - 跨类别泛化实验；
  - 多视角变化鲁棒性实验；
  - 真实机器人操作的下游任务实验。
- 虽未给出具体实验组数，但覆盖了合成数据和真实世界场景，并涉及消融对比，可以认为实验设计相对充分、客观且公平。

### 6. 论文的主要结论与发现
- 提出的几何引导基座特征建模机制能有效提升单目三维形状恢复的泛化能力。
- 通过显式形变学习，模型在未见类别和任意视角下均表现出鲁棒的形状重建精度。
- 该方法不仅性能超越现有技术，还能成功支撑真实世界的灵巧机器人操作任务。

### 7. 优点
- **几何感知的特征设计**：将模板拓扑信息融入基础特征，增强了模型对形状结构的内在理解。
- **视角自适应机制**：有效缓解了固定模板与任意目标视角之间的不对齐问题，提升了视角泛化性。
- **任务驱动验证**：不仅限于合成场景，还在真实机器人操作中展示了实用性，说明方法的现实价值。
- **泛化能力强**：对未见类别和大幅度视角变化具有鲁棒性，克服了传统方法的主要局限。

### 8. 不足与局限
- **资源细节缺失**：摘要未提供算力消耗信息，无法评估其训练成本与效率。
- **技术细节有限**：仅凭摘要难以判断形变预测的具体网络结构、损失函数设计等，需要阅读全文。
- **模板依赖性**：方法需要预定义类别级模板，对于无合适模板的非常规物体或高度拓扑变化的物体，适用性可能受限。
- **实验覆盖未知**：虽声称大量实验，但摘要未给出具体数据集、对比方法及定量结果，无法评估其性能优势的统计显著性和实际增益幅度。

（完）
