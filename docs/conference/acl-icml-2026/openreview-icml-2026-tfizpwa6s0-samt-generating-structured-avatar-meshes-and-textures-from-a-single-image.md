---
title: "SAMT: Generating Structured Avatar Meshes and Textures from a Single Image"
title_zh: SAMT：从单张图像生成结构化头像网格和纹理
authors: "Muyu Wang, Jianzhe Gao, Xingping Dong, Yujia Wang, Wenguan Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0c354edf4bdb07276a70786a2459e2f8ef75d03f.pdf"
tags: ["query:sketch-to-d"]
score: 5.0
evidence: 从单张图像生成三维头像
tldr: 针对单张图像生成高保真三维头像的难题，提出SAMT框架，第一阶段用潜在三维扩散模型生成面部网格，第二阶段通过多视角感知纹理策略合成视图一致性纹理。在大规模头像数据集上微调后，方法能产生具有身份细节和精细纹理的三维面部资产，对绘画等非真实感输入有潜在适用性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 单图像三维头像生成需同时保持面部微结构特征和多视角纹理一致性，现有方法常难以兼具。
method: 两阶段框架：预训练潜在三维扩散模型生成面部网格，结合多视角先验与网格几何进行多视角感知纹理合成。
result: 在35K三维头像模型上微调后，能生成高质量几何与视图一致性纹理。
conclusion: SAMT为单图像三维头像生成提供了结构化且纹理精细的解决方案，可潜在应用于角色原画等绘画输入。
---

## Abstract
Despite rapid progress in generative 3D creation, producing high-fidelity 3D face assets from a single image remains challenging, as it requires both identity-critical facial micro-structures and fine-grained view-consistent textures. To address this, we present a two-stage framework named **SAMT** for monocular 3D avatar generation and texture synthesis. Specifically, a latent 3D diffusion model for facial mesh generation is pretrained and then further adapted to generate high-quality facial geometry through large-scale domain-specific finetuning on 35K curated 3D avatar models. Subsequently, the generated facial mesh is textured through a multi-view-aware texturing strategy. It incorporates multi-view facial priors along with the mesh geometry to guide a 2D texturing diffusion, enabling cross-view consistent and mesh-aligned texture synthesis. Extensive experiments demonstrate that SAMT improves over existing baselines by producing more coherent facial geometry together with more fine-grained and view-consistent textures. Project page is available at https://github.com/muyuWang/SAMT.

---

## 论文详细总结（自动生成）

### 摘要核心信息
本文提出 **SAMT** 框架，用于从单张图像生成高保真三维头像（包含结构化网格与纹理）。该方法采用两阶段设计：先用潜在三维扩散模型生成面部网格，再通过多视角感知策略合成视图一致的纹理。在大规模（35K）三维头像数据上微调后，能够产生兼顾身份细节和精细纹理的三维面部资产，并可能适用于绘画等非真实感输入。

---

### 1. 论文的核心问题与整体含义
- **研究背景**：生成式三维建模虽进展迅速，但从单张图像直接创建高保真三维头像仍面临巨大挑战。
- **核心痛点**：需要同时捕捉与身份强相关的面部微结构（如法令纹、睑缘）和保证纹理在多视角下的细粒度一致性，现有方法往往难以兼得。
- **研究动机**：解决单图像三维头像生成中“几何细节丢失”与“纹理视图漂移”的固有矛盾，为游戏、影视角色制作等提供更透明的创作工具。
- **潜在价值**：桥接单目二维感知与三维资产生产，降低传统手工建模和贴图的人力成本。

### 2. 论文提出的方法论
- **整体框架**：命名为 **SAMT** 的两阶段流水线。
- **第一阶段：几何生成**
  - 预训练一个 **潜在三维扩散模型**，专门用于面部网格的生成。
  - 在35,000个精心筛选的三维头像模型上进行大规模领域微调，使模型能输出高质量、结构连贯的面部几何（保留微结构特征）。
- **第二阶段：纹理合成**
  - 设计 **多视角感知的纹理化策略**。
  - 融合 **多视角面部先验** 与上一阶段生成的 **网格几何**，共同指导一个二维纹理扩散过程。
  - 该策略的核心优势在于能实现 **跨视角一致** 且 **与网格精确对齐** 的纹理合成，避免传统方法中常出现的纹理拉伸或视角间颜色不一致。
- **关键技术点**：
  - 潜在扩散模型保障了生成效率与质量。
  - 跨阶段耦合：纹理合成严格依赖几何先验，将三维几何作为二维纹理扩散的硬约束。

### 3. 实验设计
- **训练 / 微调数据**：使用一个包含 **35,000 个三维头像模型** 的专用数据集进行大规模微调（几何阶段）。该数据集的构建和筛选标准在完整论文中有详细说明，摘要未提及具体来源。
- **评估基准与对比方法**：
  - 与 **现有基线方法** 进行对比（摘要泛称“existing baselines”，未列出具体名称）。
  - 评估维度聚焦于几何的连贯性、纹理的细粒度以及视图一致性。
- **潜在泛化测试**：依据论文动机和结论，声称对绘画（如角色原画）等非真实感输入有适用性，但摘要中未提供针对此类输入的定量或定性实验描述。

### 4. 资源与算力
- **明确程度**：本文所提供的 **摘要及元数据中未提及任何关于算力的信息**，例如 GPU 型号、数量、单次训练时长或总消耗时间等。完整算力详情需查阅全文。

### 5. 实验数量与充分性
- **实验量级**：摘要仅提及“大量实验验证”，未给出实验组的具体数量（如消融实验个数、跨数据集对比数）。根据常规论文经验，此类工作通常包含：
  - 与多个主流框架的定量对比。
  - 针对几何生成模块和纹理策略的消融研究。
  - 对真实人像、绘画等不同输入域的定性展示。
- **充分性与客观性**：摘要中声称提升明显（更连贯的几何、更细粒度且一致的纹理），但因缺少具体指标（如FID、PSNR、身份保持度）和标准数据集名称，基于当前信息无法严谨评估实验是否充分、客观。需以全文为准。
- **公平性说明**：摘要未提及对比方法复现与调参细节，因此无法判断对比的公平性。

### 6. 论文的主要结论与发现
- **SAMT 有效解决单图像三维头像生成难题**，能产生结构上更 **连贯的面部几何** 和 **更精细、视角一致的纹理**。
- **两阶段分工明确且效果叠加**：预训练加领域微调保证了几何的身份保真度，多视角感知纹理化解耦了光照与材质并使纹理稳固贴合。
- **应用延展性**：该方法对非真实感输入（如绘画）具备潜在适用性，向风格化角色生成扩展提供了可能。

### 7. 优点（亮点）
- **问题聚焦明确**：专门针对微结构和视图一致性这两项长期痛点，而非泛泛的三维重建。
- **跨模态融合设计**：将三维扩散模型（几何）与二维纹理扩散通过多视角先验巧妙耦合，实现了“几何到纹理”的强制对齐。
- **规模化微调策略**：在35K级别三维头像数据上微调，体现了利用大规模私有数据提升零样本生成质量的思路。
- **结构化输出**：直接生成可编辑的三维网格与纹理贴图，而非隐式表示，更利于下游生产流程对接。

### 8. 不足与局限
- **实验细节缺失（基于提供内容）**：算力成本、精确指标体系、所用基线方法列表均未在摘要中披露，难以评估实用代价与感知质量提升的客观幅度。
- **泛化边界不明确**：虽然提到绘画输入，但缺乏实验支撑；对极端姿态、遮挡、配饰（眼镜）的情况是否鲁棒未知。
- **数据集偏差风险**：微调所用35K头像模型可能存在风格、种族、年龄分布偏差，生成的多样性受制于该数据集。
- **方法局限合理推断**：两阶段解耦可能会使几何错误不可逆地传导至纹理阶段（摘要未评估对几何误差的容忍度）。
- **真实感与细节保留**：尽管声称保留微结构，但传统单图三维重建固有的高频细节丢失问题是否被彻底解决仍需查证细致度对比。

（完）
