---
title: "PointDiT: Pixel-Space Diffusion for Monocular Geometry Estimation"
title_zh: PointDiT：像素空间扩散用于单目几何估计
authors: "Haofei Xu, Rundi Wu, Philipp Henzler, Nikolai Kalischek, Michael Oechsle, Fabian Manhardt, Marc Pollefeys, Andreas Geiger, Federico Tombari, Michael Niemeyer"
date: 2026-04-30
pdf: "https://openreview.net/pdf/859969c4505c940b506d06cb01ee1bce1e5d07d0.pdf"
tags: ["query:sketch-to-d"]
score: 7.0
evidence: 通过像素空间扩散从单图生成3D点图
tldr: 现有单图三维重建依赖复杂架构或潜在扩散，PointDiT提出在像素空间直接扩散点图块的极简方案，使用纯ViT和DINOv3条件，无需点图分词器，在多个基准上取得最优或可比性能，证明简洁设计可高效解决单目几何估计。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有单图三维重建方法架构复杂，需潜在压缩，限制了效率和简化。
method: 提出基于纯ViT的像素空间扩散Transformer，直接在点图块上操作，以DINOv3图像token为条件。
result: 在多个数据集上达到或超越现有方法，同时架构更简洁。
conclusion: 极简扩散模型设计可有效用于单目几何估计，无需潜在空间和复杂损失。
---

## Abstract
State-of-the-art single-image 3D reconstruction methods often rely on complex hybrid architectures and loss functions (e.g., MoGe), or necessitate compressing geometry into latent spaces (e.g., GeometryCrafter) to leverage pre-trained latent diffusion models. In this work, we demonstrate that such architectural overhead and intricate loss formulations are unnecessary. We introduce a minimalist pixel-space Diffusion Transformer built on a plain ViT, which operates directly on raw 3D point map patches and is conditioned on image tokens from a pre-trained DINOv3. Unlike existing latent diffusion-based approaches, we train our diffusion backbone entirely from scratch, eliminating the need for point map tokenizers. We show that this streamlined approach yields results superior to complex latent-based diffusion models while remaining significantly simpler than hybrid alternatives. Notably, our model produces sharper geometric structures and achieves significantly better results on highly ambiguous regions, such as transparent objects.

---

## 论文详细总结（自动生成）

好的，我们基于所提供的信息对论文《PointDiT: Pixel-Space Diffusion for Monocular Geometry Estimation》进行结构化总结。请注意，由于原文是 OpenReview 的元数据与摘要，部分细节可能未涵盖，总结将依据现有材料展开。

---

### 1. 论文的核心问题与整体含义

- **核心问题**：从单目图像中进行三维几何估计（即单视图重建）是计算机视觉的核心任务。现有方法往往依赖复杂的混合架构与损失函数（如 MoGe），或需要将几何信息压缩到潜在空间中（如 GeometryCrafter），以便利用预训练的潜在扩散模型。这种架构上的复杂性和设计负担限制了方法的简洁性、可解释性和效率。
- **整体含义**：本工作质疑“复杂架构和潜在压缩是必需的”这一前提，试图证明一个极简的像素空间扩散模型可以直接在原始三维点云图上运作，并达到甚至超越现有方法的性能，从而为单目几何估计提供一种更简单且更高效的解决方案。

### 2. 论文提出的方法论

- **核心思想**：直接在**像素空间**对三维点图进行扩散，跳过向潜在空间的压缩步骤，同时采用纯粹的视觉 Transformer（ViT）架构，避免额外的设计复杂性。
- **关键技术细节**：
  - **输入与表示**：在**点图块（point map patches）** 上直接操作，即直接将三维点坐标视为可处理的 token 序列，无需点图分词器（point map tokenizer）。
  - **模型架构**：一个基于简单 ViT 的扩散 Transformer（Diffusion Transformer, DiT），从零开始训练（无预训练扩散权重）。
  - **条件机制**：使用预训练的 **DINOv3** 模型提取的图像 token 作为条件信号，引导扩散过程。
  - **训练与推理**：采用标准的扩散模型去噪训练目标，直接从噪声点图块还原干净的几何结构，整个流程保持在像素空间。
- **与现有方法的区别**：
  - 相较于潜在扩散方法（如 GeometryCrafter），无需训练点图的自编码器（分词器），避免了潜在空间量化误差和额外训练。
  - 相较于混合架构（如 MoGe），无需设计精巧的混合模块或多阶段损失函数，结构更统一。

### 3. 实验设计

- **数据集使用**：原文未在摘要中列举具体数据集，但根据通用基准，单目几何估计常用数据集包括 **ScanNet、NYU Depth V2、KITTI、Matterport3D** 等。文中提到“在多个基准上取得最优或可比性能”，并特别提及在透明物体等高难度区域表现突出，暗示实验覆盖了包含透明物体的测试场景（如 ClearPose 或 TransCG 等）。
- **对比方法**：明确提到的对比对象有 **MoGe**（混合架构）和 **GeometryCrafter**（潜在扩散模型），说明实验集中比较了代表性复杂架构与潜在扩散方法。
- **评估维度**：着重展示几何结构的清晰度，以及在**高不确定性区域**（如透明表面）的性能优势。

### 4. 资源与算力

- **GPU 信息**：所提供的摘要和元数据中**未提及** GPU 型号、数量或训练时长。因此，无法推断具体的算力需求。若需此信息，只能查阅完整论文。

### 5. 实验数量与充分性

- **实验组数**：由于仅有摘要，无法统计具体实验数量。但基于典型扩散模型论文，应当包括：
  - 不同数据集上的定量对比（PSNR, SSIM 等 3D 指标）。
  - 与多种 SOTA 方法的横向比较。
  - 消融实验（例如：验证像素空间 vs. 潜在空间、纯 ViT vs. 混合架构、DINOv3 条件对性能的贡献）。
  - 定性可视化（如透明物体重建结果）。
- **充分性与公平性**：根据摘要宣称，该方法在简洁度提升的同时仍能取得优越结果，且在透明物体等难点上优势明显，若这些对比是在公开基准上以统一度量进行，则实验具有客观性和公平性。但由于信息有限，无法确认是否包含了所有必要的基线或统计检验。

### 6. 论文的主要结论与发现

- 复杂的架构设计（混合模块、多损失函数）或潜在空间压缩不是单目几何估计的必要条件。
- 一个极简的、直接在像素空间操作的扩散 Transformer（PointDiT），配合 DINOv3 图像条件，就能从单张图像生成高质量的三维点图。
- 该方法在几何结构的锐利度和对高模糊区域（如透明对象）的重建上，显著优于复杂的潜在扩散模型和混合模型。
- 整体结论：**“少即是多”——在单目几何估计中，简洁的像素空间扩散设计可以高效替代现有沉重范式。**

### 7. 优点

- **架构极简**：纯 ViT 扩散主干，无需点图自编码器或复杂混合模块，训练和推理流程清晰。
- **训练经济**：直接在像素空间端到端训练，省去了预训练潜在扩散模型或分阶段训练的额外成本。
- **性能优异**：在透明物体等挑战性场景下取得更好结果，表明像素空间操作保留了更精细的几何细节。
- **易于复现与扩展**：设计上的简化降低了实现门槛，并为社区提供了新的基线。

### 8. 不足与局限

- **算力信息缺失**：摘要未提供训练所需的 GPU 资源和时间，难以评估其实际计算开销（像素空间扩散通常比潜在空间扩散更耗资源于高分辨率数据）。
- **实验覆盖未知**：具体使用的数据集、评估指标、消融项均未披露；透明物体之外的泛化性（如动态场景、大规模户外场景）未讨论。
- **潜在偏差**：DINOv3 作为条件网络，其性能受限于其预训练数据的分布；该方法在显著偏离该分布的图像上可能退化。
- **应用限制**：直接操作高分辨率点图可能面临内存和计算挑战，扩展性未曾验证；另外，单目重建固有的尺度歧义性未在摘要中说明如何处理。
- **比较公平性**：与 MoGe 等混合方法的比较是否在相同训练数据、相同增强条件下进行，摘要未澄清。

---

（完）
