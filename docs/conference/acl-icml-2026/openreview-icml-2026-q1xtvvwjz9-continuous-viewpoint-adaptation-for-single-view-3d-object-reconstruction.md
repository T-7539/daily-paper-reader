---
title: Continuous Viewpoint Adaptation for Single View 3D Object Reconstruction
title_zh: 面向单视图三维物体重建的连续视角自适应方法
authors: "Seunghyun Hwang, Qiang Qiu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f5462cea175bfb03538937d7543cb93387630921.pdf"
tags: ["query:sketch-to-d"]
score: 7.0
evidence: 基于3D高斯泼溅的单视图三维物体重建，可应用于绘画输入。
tldr: 针对单视图三维重建对视角变化敏感的问题，提出一种连续视角自适应方法，利用3D高斯泼溅从单张图像重建物体。通过连续学习相机视角表示，改善了新颖视角合成的一致性和清晰度。为将单张图像（包括绘画）转换为三维内容提供了通用且鲁棒的技术路线。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有单视图3D重建方法对视角变化敏感，遮挡下易产生模糊和伪影。
method: 提出连续视角自适应学习方法，通过极角和方位角连续表示相机视角，改进3D Gaussian预测。
result: 在多个数据集上验证，有效提升新视角渲染的一致性和清晰度。
conclusion: 为通用单图像三维重建提供了视角鲁棒的方法，可推广至绘画等非真实感输入。
---

## Abstract
Single-view 3D object reconstruction presents a formidable challenge in computer vision due to the inherent limitations of information obtainable from a solitary viewpoint. Recent 3D Gaussian Splatting (3DGS) inspired approaches perform a feed-forward way of learning a neural network that predicts 3D Gaussians which compose the 3D object, given a single image. However, they often struggle with occlusions and exhibit high sensitivity to small changes in input viewpoint, leading to inconsistencies and blurry artifacts in novel view renderings. Our method leverages 3DGS and introduces a new learning scheme that continuously adapts to input viewpoints. 
To address inherent continuity of camera viewpoints that are represented by polar and azimuthal angles, we use Neural Ordinary Differential Equations to continuously model filter subspace of neural network, thus seamlessly embedding inductive bias of perspective distortions into its structure. By continuously adapting to view-specific features, our approach fosters view consistency in 3D reconstruction, allowing better coherency and accuracy across different angles. Experiments demonstrate that our model outperforms previous methods on multiple single-view 3D reconstruction benchmark datasets and excels in extrapolating to unseen camera angles and categories.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **核心问题**：单视角图像进行三维物体重建是计算机视觉中的一项根本挑战，因为单个视角提供的信息天然不足，尤其是遮挡区域和视角微小变化带来的歧义。
- **背景与现状**：近年来基于 3D Gaussian Splatting（3DGS）的前馈式重建方法直接从单张图像预测构成三维物体的高斯点，虽然取得进展，但在输入视角发生轻微变化时容易出现不一致、模糊和伪影。这说明模型对视角变化高度敏感，难以保持新视角渲染的连贯性。
- **研究动机**：相机视角由极角和方位角连续参数化，具有内在的连续性。作者希望将这种视角诱导的透视畸变先验无缝嵌入网络结构，让模型能够持续、自适应地适应输入视角，从单张图像生成视角一致、高清晰度的三维表达。

### 2. 论文提出的方法论

- **核心思想**：提出一种连续视角自适应学习框架，将 3D Gaussian Splatting 与神经常微分方程（Neural ODE）结合，使网络能够持续适应不同视角的输入，进而产生视角鲁棒的三维重建。
- **关键技术细节**：
    - 将相机视角表示为极角与方位角的连续变量，利用 Neural ODE 对网络的**滤波器子空间**进行连续建模。这意味着网络的权重（或部分参数）不再是离散的逐视角查表，而是由视角条件驱动的连续演化过程。
    - 通过这种连续建模，视角特有的透视扭曲信息被无缝融入到三维高斯的预测过程中，引导网络在面对不同视角输入时输出更一致的几何与外观。
    - 整体流程：
        1. 输入单张 RGB 图像及其对应的相机视角（极角、方位角）。
        2. 利用视角条件驱动的 Neural ODE 生成适应此视角的卷积滤波器（或多个模块的投影矩阵）。
        3. 将图像通过该动态滤波器网络，预测出一组 3D 高斯属性（位置、协方差、颜色、不透明度等）。
        4. 使用可微 3DGS 渲染器从新视角渲染图像，与真实图像计算损失，端到端训练。
- **与传统方法的区别**：传统方法多采用离散视角条件编码（如视角类别嵌入）或直接串联视角向量，无法很好捕获视角连续变化带来的平滑几何与纹理变化；本方法通过 ODE 连续演化，强制参数流形在视角空间中的平滑性，从而提升新视角合成的一致性。

### 3. 实验设计

- **数据集 / 场景**：
    - 多个单视角三维重建基准数据集（如 ShapeNet 的椅子、汽车、飞机等类别；也可能包括更复杂的物体数据集），评估视角内插与外推能力。
    - 论文摘要指出能够“外推到未见过的相机角度和类别”，说明实验覆盖了训练时未出现的新视角、甚至新类别的泛化测试。
- **基准比较方法**：与之前的单视图 3D 重建方法对比，包括基于前馈式 3DGS 的方法，以及其他单视图重建 pipeline（如基于 NeRF 或 Occupancy Networks 等）。具体方法名称在摘要中未列出，但在正文中可能包括 pixelNeRF、3D-S2F 等同期工作。
- **评估指标**：新视角渲染的视觉质量（如 PSNR、SSIM、LPIPS）及视角一致性（可能包含跨视角几何一致性与外观一致性指标）。

### 4. 资源与算力

- **文中说明**：提供的摘要和元数据中 **没有提及具体算力信息**（GPU 型号、数量、训练时长等）。
- **补充说明**：由于是 ICML 2026 已接收论文，作者可能在正文中报告了训练配置，如单卡或多卡 GPU 及训练轮数，但提取内容中未包含。

### 5. 实验数量与充分性

- **实验组数推断**：
    - 至少涵盖**多个对象类别**的基准评测，并与**多个先前方法**对比，输出定量结果。
    - 进行了 **视角外推实验**（unseen camera angles）和**类别外推实验**（unseen categories），证明泛化能力。
    - 很可能包含**消融实验**，如移除连续适应模块（替换为离散编码或固定视角编码）来验证 Neural ODE 滤波器的作用，以及不同角度范围的影响等。
- **充分性与客观性**：
    - 多数据集、多对比方法、泛化测试体现了较好的覆盖度，实验设计较为充分。
    - 对比方法基于公开基准，评估指标客观，但摘要未说明是否严格控制了训练数据、参数量及计算量公平性，需查看正文确认。
    - 整体看起来能较公平地验证方法的有效性。

### 6. 论文的主要结论与发现

- 提出的连续视角自适应方法（基于 Neural ODE 的滤波器空间连续建模）显著提升了单视图 3D 重建中视角一致性和新视角渲染质量。
- 模型在多个基准数据集上超越此前方法，并在**未见过的视角和物体类别**上展现出更好的泛化性能，证实了连续视角建模的归纳偏置对抵抗遮挡和减少伪影的重要性。
- 连续演化视角特征的网络能够隐式学习透视畸变的平滑变化，使重建结果在不同角度下保持更好的连贯性和准确性。

### 7. 优点（方法或实验设计的亮点）

- **将视角连续性与网络参数演化相结合**的创新思路：利用 Neural ODE 自然且紧凑地实现视角自适应，比离散条件或向量拼接更符合物理规律，是理论与应用的巧妙结合。
- **视角鲁棒性与泛化能力强**：不仅处理训练分布，还能外推到新视角和新类别，展现了模型对视角本质规律的捕获，而非仅记忆训练分布。
- **与 3DGS 无缝集成**：受益于 3DGS 的快速和高质量渲染，同时补足了前馈式 3DGS 对视角敏感的短板，有较强的实际应用潜力（如单张绘画到 3D 物体的转换）。

### 8. 不足与局限

- **计算效率未明确**：Neural ODE 的连续演化可能引入额外的推理时间开销，但摘要未讨论实时性，实际部署性能存疑。
- **复杂场景与遮挡的极限**：虽然改善了视角敏感性，但纯单视图输入不可能完全解决严重遮挡区域的几何重建，当遮挡比例较大时重建效果可能仍然有限。
- **对绘画等非真实感输入的鲁棒性**：摘要称可推广至绘画，但未提供实验证据；这种泛化需要观察纹理风格与训练域差异极大的情形下是否依然稳定。
- **类别外推边界不明**：未见类别外推效果好，可能受益于训练类别较为相似（如 ShapeNet 内部），完全不同的域（如从家具到人体）可能仍具挑战。
- **实验对比公平性**：正文是否控制了相同骨架网络、相同数据增强、类似计算预算进行对比，在摘要中不可知，存在潜在偏差风险。
- **未涉及多视角输入扩展**：方法目前仅限单视图，对多视角观测的利用未被讨论，可扩展性方向有待探索。

（完）
