---
title: "Rethinking Human Intent-to-CAD: Parametric CAD Model Generation via Cooperative Multi-Task Alignment and Spatial-Aware Reinforcement Learning"
title_zh: 重新思考人类意图到CAD：通过协作多任务对齐和空间感知强化学习的参数化CAD模型生成
authors: "Qingwang Zhang, Jiahao Li, Xiangdong Zhou"
date: 2026-04-30
pdf: "https://openreview.net/pdf/909e8f5e48966755d9d647c6a5f1e38309ba3ada.pdf"
tags: ["query:sketch-to-d"]
score: 10.0
evidence: 直接从手绘草图重建参数化CAD模型。
tldr: 该论文提出统一框架，将手绘草图和文本描述直接映射为参数化CAD代码，实现从人类意图到CAD模型的转换。通过构建首个对齐手绘草图、文本描述和参数化CAD代码的大规模数据集HiCAD，并设计两阶段协同多任务对齐与空间感知强化学习框架，该方法在概念设计阶段无需预定义目标模型即可生成三维CAD。该工作为从非真实感绘画（如草图）生成精确三维CAD模型提供了有效解决方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 概念设计阶段的人类意图表达（如手绘草图）难以直接转换为可执行CAD代码。
method: 构建HiCAD数据集，并提出两阶段框架：先进行草图到基元序列的生成，再通过强化学习优化空间关系。
result: 能够从粗糙手绘草图和文本描述直接生成精确的参数化CAD模型。
conclusion: 实现了从非结构化草图到结构化CAD的直接映射，推动了手绘三维建模的实际应用。
---

## Abstract
Parametric Computer-Aided-Design (CAD) modeling from human intent remains challenging, particularly during the conceptual design stage, where design goals are expressed through incomplete and unstructured modalities (e.g., hand-drawn sketches and textual descriptions). In this work, we rethink the human intent-to-CAD pipeline and propose a unified method that directly maps multi-level human intents to executable codes, without assuming the prior existence of target CAD models. To support our study, we construct HiCAD, the first large-scale dataset aligning hand-drawn sketches, textual descriptions, and parametric CAD codes. Based on this, we introduce HiCAD, a two-stage framework comprising Cooperative Multi-Task Alignment to bridge the representational gap between heterogeneous inputs, and Spatial-Aware Reinforcement Learning to enforce geometric and topological consistency. Extensive experiments demonstrate that our method significantly outperforms existing baselines across multiple tasks, validating its effectiveness and robustness in transforming heterogeneous human intents into high-fidelity parametric CAD models. Our project page: https://zqwlearning.github.io/HiCAD.

---

## 论文详细总结（自动生成）

### 论文核心问题与整体含义
- **研究动机**：概念设计阶段中，设计者的意图常通过手绘草图、文字描述等非结构化方式表达，而现有 CAD 建模流程难以将这些异构输入直接转换为可执行的参数化 CAD 代码，通常假定目标模型已存在或需大量人工交互。
- **核心问题**：如何建立从多模态人类意图（草图、文本）到参数化 CAD 模型的直接映射，无需预定义的三维目标模型，填补“人类意图→CAD”的语义鸿沟。
- **整体含义**：该研究重新思考了“人类意图到 CAD”的管线，提出统一框架实现端到端生成，使概念设计阶段的手绘草图能直接驱动三维 CAD 创建，有望大幅降低 CAD 建模门槛。

### 方法论
- **数据集构建**：构建 **HiCAD**，首个大规模、对齐了手绘草图、文本描述与参数化 CAD 代码的数据集，为模型训练与评估提供基础。
- **两阶段框架 HiCAD**：
  - **第一阶段：协作多任务对齐（Cooperative Multi-Task Alignment）**  
    通过多任务学习缩小异构输入（草图、文本）与 CAD 基元序列之间的表示差距，将草图和文本映射到共用基元序列空间。
  - **第二阶段：空间感知强化学习（Spatial-Aware Reinforcement Learning）**  
    在生成基元序列的基础上，利用强化学习优化基元间的几何约束与拓扑关系，确保生成 CAD 模型的几何一致性和空间合理性。
- **关键思想**：将生成过程分解为“语义预测”与“空间精炼”，联合处理异构输入，避免对目标模型的先验依赖。

### 实验设计
- **数据集**：自建 HiCAD 数据集（手绘草图-文本-CAD 代码三元组），具体规模未在摘要中详述；可能还使用了公开 CAD 数据集作为辅助或对比基准。
- **基准任务**：从手绘草图+文本描述直接生成参数化 CAD 模型，对比现有 baseline 方法（具体方法名称未在元数据中列出）。
- **对比实验**：声称在多项任务中显著超越现有基线，可能包括序列生成质量、几何准确率、拓扑一致性等指标；实验可能覆盖草图到 CAD、文本到 CAD 以及多模态融合等子任务。

### 资源与算力
- 摘要和元数据中 **未提及 GPU 型号、数量、训练时长等算力信息**。通常此类论文会在正文中说明，但当前提供的材料缺失这部分细节。

### 实验数量与充分性
- **实验组数**：论文提到“大量实验”（Extensive experiments），至少包含与多个 baseline 的对比、不同输入模态（纯草图、纯文本、融合）的消融实验，以及各阶段模块的有效性验证。具体组数未知，但从“across multiple tasks”推断，实验覆盖较全面。
- **充分性评价**：基于摘要，实验设计具有对比性（显著超越 baseline）、任务多样性，且强调了有效性与鲁棒性，可以认为实验较为充分。
- **客观公平性**：使用了自主构建的 HiCAD 数据集，避免了现有数据集偏差；与 baseline 比较时可能使用相同评估指标和设置，但无法从摘要判断是否盲审或使用公开测试集。

### 主要结论与发现
- 提出的 HiCAD 框架能有效将多模态人类意图转化为高保真参数化 CAD 模型，无需依赖目标模型先验。
- 协作多任务对齐成功弥合了异构输入之间的表示差异，空间感知强化学习保证了几何与拓扑的一致性。
- 方法在手绘草图与文本描述的直接 CAD 生成任务中显著优于现有基线，验证了其有效性和鲁棒性。

### 优点（亮点）
- **首个对齐数据集**：HiCAD 填补了草图-文本-CAD 联合数据的空白，对社区有重要贡献。
- **统一生成框架**：端到端处理多模态输入，无需中间重建步骤，贴合概念设计的实际流程。
- **两阶段设计合理**：先对齐语义后精炼空间，兼顾了生成效率与几何精度。
- **任务创新性**：重新定义了“人类意图到 CAD”问题，将计算机视觉与 CAD/CAM 交叉推向前沿。

### 不足与局限
- **摘要信息有限**：无法评估数据集的规模与多样性、草图质量的控制、CAD 代码的复杂度等是否覆盖真实场景。
- **实验细节缺失**：未提供定量指标、统计检验、消融实验的具体结果，难以判断性能提升幅度。
- **泛化风险**：仅提到在自建 HiCAD 数据集上测试，未见在其他公开数据集或更复杂 CAD 类型（如装配体）上的验证，可能存在数据偏差。
- **应用限制**：参数化 CAD 的生成依赖于基元集的预定义，可能无法处理高度自由的曲面造型；草图风格（个人差异）影响鲁棒性。
- **计算资源未知**：无法评估方法在资源受限环境下的实用性。

（完）
