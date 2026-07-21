---
title: "LaRI: Layered Ray Intersections for Single-view 3D Geometric Reasoning"
title_zh: LaRI：用于单视图三维几何推理的分层射线相交
authors: "Rui Li, Biao Zhang, Zhenyu Li, Federico Tombari, Peter Wonka"
date: 2026-04-30
pdf: "https://openreview.net/pdf/81e4c1ee701d165b0e030660757cf3a56b5dcd34.pdf"
tags: ["query:sketch-to-d"]
score: 8.0
evidence: 单视图完整场景重建方法，使用分层点图，适用于绘画和草图。
tldr: LaRI 提出一种全监督的单视图方法，通过预测分层点图来恢复被遮挡的多个表面，从而在单次前馈传递中从单张图像实现完整的三维场景重建。不同于传统深度估计仅关注可见表面，该技术能推理遮挡几何，为物体级和场景级任务提供高效且视角对齐的几何推理。这为从绘画、草图等单张二维内容生成三维几何提供了实用基础。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有单视图几何推理方法通常只恢复可见表面，无法处理遮挡几何。
method: 采用全监督方式预测分层射线相交点图，一次性输出多层点位置及射线停止索引。
result: 能够在一次前向传播中完成单视图完整场景重建，便于实时应用。
conclusion: 为单图像三维几何推理提供了高效视图对齐的方案，适合作为绘画等输入的三维几何生成组件。
---

## Abstract
We present Layered Ray Intersections (LaRI), a fully supervised method for occluded geometry reasoning from a single image. Unlike conventional depth estimation, which is limited to visible surfaces, LaRI predicts multiple surfaces intersected by the camera rays using layered point maps. Compared to the existing approaches that leverage neural implicit representations or iterative refinement, LaRI achieves complete scene reconstruction in one feed-forward pass, enabling efficient and view-aligned geometric reasoning to underpin both object-level and scene-level tasks. We further propose to predict the ray stopping index, which identifies valid intersecting pixels and layers from LaRI's output. To better underpin and evaluate this task, we build an annotation pipeline using rendering engines, construct annotations for five public datasets, including synthetic and real-world data covering 3D objects and scenes. As a generic method, LaRI's performance is validated in object-level and scene-level reconstruction tasks.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：从单张二维图像恢复完整的三维场景结构，尤其是重建被遮挡的几何表面。传统的深度估计方法只能恢复相机直接可见的第一层表面，无法推理遮挡关系。
- **研究动机**：在许多应用（如从绘画、草图生成三维内容）中，必须推断出图像中看不到的物体部分，因此需要一种能一次性处理多层表面、推理遮挡后几何信息的单视图方法。
- **整体含义**：LaRI 提出了一种全监督、单次前馈的解决方案，旨在高效且视角对齐地完成从单张图像到多层三维表面点的映射，为物体级和场景级的完整三维重建提供新的基础组件。

## 2. 论文提出的方法论
- **核心思想**：将三维几何推理建模为“分层射线相交”（Layered Ray Intersections）。对于每个像素对应的摄像机射线，预测该射线与场景中多个表面相交的三维点位置。
- **关键技术细节**：
  - **分层点图（Layered Point Maps）**：网络直接输出一个按层排列的3D点集合，每一层代表射线与不同深度表面的交点。
  - **射线停止索引（Ray Stopping Index）**：同时预测一个索引值，标识每条射线在哪些层存在有效的相交点（即哪些层真正击中了表面），从而过滤无效的预测。
  - **算法流程**：输入单张RGB图像，通过一个前馈神经网络一次性输出分层位置图和对应的停止索引，无需任何迭代优化或神经隐式场的逐点查询，实现端到端的快速推理。
  - **公式/流程简化说明**：给定图像 \(I\)，模型 \(f_\theta\) 输出 \(\{\{p_{i,j}\}, s_i\}\)，其中 \(p_{i,j}\) 是像素 \(i\) 在第 \(j\) 层的3D坐标，\(s_i\) 是有效相交层数（射线停止位置）。

## 3. 实验设计
- **数据集与场景**：
  - 为五个公共数据集构建了专门的分层几何标注，涵盖合成数据和真实数据，并覆盖三维物体和三维场景两个层面。
  - 具体数据集名称在摘要和元数据中未列出，但明确指出包含合成与真实两类，确保场景多样性。
- **评估基准与对比方法**：
  - 将单视图完整重建任务作为基准，评价指标预计包括点位置精度、表面完整度等（摘要未详述具体指标）。
  - 对比方法包括两类：一类是基于神经隐式表示的方法（如神经辐射场 NeRF 类），另一类是基于迭代优化的精细化方法。
- **任务设置**：分别在物体级重建和场景级重建任务上验证方法的有效性。

## 4. 资源与算力
- 所提供的摘要及元数据中**未明确说明**使用的 GPU 型号、数量、训练时长等计算资源细节。因此，无法总结该部分。

## 5. 实验数量与充分性
- **实验规模**：至少覆盖了五个数据集上的实验，涉及物体和场景两种粒度，并与两种主流范式的基线进行了比较。此外，预测射线停止索引的设计本身很可能有对应的消融实验（摘要中虽未明确列出，但属于方法论验证的常规部分）。
- **充分性与客观性**：
  - 覆盖合成与真实数据，任务多样，对比了隐式方法和迭代方法，实验设计维度较为全面。
  - 由于未给出具体数值和统计检验，仅从摘要判断，实验对比在范式层面是公平的，但更细致的公平性（如参数量、训练数据量对齐等）无法进一步确认。
  - 整体来看，实验设计能够支撑论文的核心主张，具备一定的充分性。

## 6. 论文的主要结论与发现
- **一次前馈完成完整重建**：LaRI 避免了隐式表示的耗时查询和迭代优化，仅通过单次前向传播即可输出多层表面点云。
- **有效处理遮挡几何**：通过预测分层交点，能够显式推理被前景遮挡的后层表面。
- **停止索引提升输出质量**：预测的射线停止索引可以准确地标识有效层，滤除冗余预测。
- **通用性得到验证**：方法在物体级和场景级数据上都表现出了可行的重建能力，适合作为从单张绘画或草图生成三维几何的实用组件。

## 7. 优点
- **高效且视角对齐**：单次前馈输出与输入视图空间对齐的点云，天然适合实时或交互式应用。
- **明确的遮挡建模**：相较于仅预测深度图，分层点图提供了对场景多层几何的直接编码。
- **方法简洁通用**：不需要复杂的多阶段或特定先验，可直接应用于物体和场景，跨度较大。
- **贡献了标注管道与数据集**：为五个公开数据集构建了分层相交标注，有助于推动该任务的研究。

## 8. 不足与局限
- **监督信号依赖强**：全监督训练需要分层几何标注，而这类标注严重依赖渲染引擎，真实场景的标注成本高且可能存在域偏差。
- **信息缺失**：摘要未提及计算资源需求，复现和资源评估存在不确定性；也未给出具体对比模型的准确名称及量化结果，难以深度评判其相对优势。
- **容量限制**：输出的层数可能是固定的，对于包含半透明或极复杂多层遮挡的场景，固定层数可能无法完整恢复所有细节。
- **缺乏与最新生成式基线的比较**：仅对比了隐式表示和迭代方法，未涉及基于扩散模型或大模型的单视图三维生成方法，对比维度可能存在不足。
- **应用限制**：方法输出为离散的点层，可能还需要后处理才能生成密实的网格或纹理表面，在直接用于高质量渲染时存在局限。

（完）
