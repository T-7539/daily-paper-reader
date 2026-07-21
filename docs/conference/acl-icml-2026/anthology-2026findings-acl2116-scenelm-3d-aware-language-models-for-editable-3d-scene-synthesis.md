---
title: "SceneLM: 3D-Aware Language Models for Editable 3D Scene Synthesis"
title_zh: SceneLM：面向可编辑3D场景合成的3D感知语言模型
authors: "Xingbo Yao, Xiaoyu Chen, Doudou Zhang, Mingzhi Sheng, Boyuan Cao, Ying-Cong Chen, Hui Xiong"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.2116.pdf"
tags: ["query:sketch-to-d"]
score: 6.0
evidence: 通过语言模型从单张RGB图像生成可编辑3D场景
tldr: 从单张RGB图像合成可编辑3D场景是内容创建的核心任务，现有几何管线难以交互编辑，SceneLM通过语言模型从图像恢复可执行度量3D布局，实现高保真重建与便捷交互式编辑，在场景合成和编辑任务上表现优异。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2116/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1635, \"height\": 552, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2116/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1652, \"height\": 559, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026findings-acl2116/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1629, \"height\": 906, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl2116/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1663, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl2116/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1664, \"height\": 385, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl2116/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 835, \"height\": 282, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026findings-acl2116/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 811, \"height\": 181, \"label\": \"Table\"}]"
motivation: 单图3D场景合成难兼顾重建质量和交互编辑。
method: 基于语言模型框架从单图恢复可执行度量三维布局。
result: 实现高保真场景重建并支持交互式编辑。
conclusion: 语言模型驱动度量3D理解可简化场景合成与编辑流程。
---

## Abstract
Synthesizing an editable 3D scene from a single RGB image is central to content creation, embodied-agent data generation, and AR/VR, yet remains challenging to achieve both high-fidelity reconstruction and convenient interactive editing. Existing geometry-based pipelines produce high-quality 3D results but are typically hard to refine without rerunning the full process, while LLM-driven procedural systems enable interactive tool use but are mostly text-driven and lack precise metric 3D understanding from images. We present SceneLM, a language-model-based framework that grounds 3D scene synthesis in visual evidence by recovering an executable metric 3D layout directly from a single image. Given an RGB image (and camera intrinsics when available), SceneLM outputs a JSON-form layout specifying each object’s category, 3D center, size, and discretized yaw, and then deterministically executes this layout with a tool suite to instantiate, place, and edit objects for iterative refinement. To train metric layout recovery at scale, we curate five datasets covering diverse indoor, outdoor, and tabletop scenes and convert heterogeneous 3D annotations into a unified instruction-tuning format. To improve numerical stability and metric accuracy while preserving the text interface, we augment autoregressive JSON generation with a lightweight geometry prediction branch and dual supervision. Experiments show that SceneLM substantially improves single-image 3D layout estimation over strong open and proprietary MLLM baselines, and yields higher-quality end-to-end scene generation in geometric consistency, physical plausibility, semantic alignment, and realism.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：从单张RGB图像合成可编辑的3D场景是内容生成、具身智能数据生成和AR/VR的核心问题。现有方法存在两大瓶颈：基于几何的重建管线（如布局生成、场景图）虽然输出质量高，但难以进行交互式编辑，修改需要重新运行整个流程；而基于大语言模型（LLM）的程序式系统虽然支持自然语言交互和工具调用，但大多仅为文本驱动，无法从图像中获取精确的度量（metric）级3D几何信息，生成的场景往往空间关系模糊、尺度失准。
- **整体含义**：SceneLM旨在弥合这两类方法的鸿沟，构建一个**以语言模型为框架、从单张图像直接恢复可执行度量3D布局**的系统，使场景合成既具备高保真几何重建能力，又天然支持便捷的交互式编辑。

### 2. 方法论
- **核心思想**：将单图3D场景合成转化为两个阶段——先预测一个对象级的**度量3D布局**（JSON结构化输出），再通过一系列确定性工具调用实例化、放置并编辑该布局。
- **关键技术细节**：
  - **模型架构**：
    - 视觉编码器：SigLIP-2 ViT 提取图像特征。
    - 视觉投影器：压缩特征为视觉 Token，与文本 prompt 拼接。
    - 语言模型：Qwen-2B（或 Qwen3-VL-2B-Instruct）自回归解码输出 JSON 列表，每个对象包含 `category`、`center_mm`、`size_mm`、`yaw_bin`（24方向离散化，每格15°）。
    - **几何感知辅助头**：在 LLM 最后一层隐藏状态上附加一个轻量 MLP，预测各对象的中心坐标、尺寸（Huber 损失）和朝向（交叉熵损失），与语言建模的交叉熵损失联合优化。公式为：  
      ```
      L = L_CE + λ·L_geom
      ```
      其中几何损失为：
      ```
      L_geom = Huber(ˆc, c) + Huber(ˆs, s) + CE(ˆy, y)
      ```
    - 推理时仍仅输出 JSON 文本，几何头仅用于训练阶段强化度量精度。
  - **可执行输出与工具使用**：生成的 JSON 布局由工具套件解析并转换为确定性函数调用，包括 `3d_object_generation`、`scene_arrangement`、`object_replacement` 等，实现对象的实例化、放置、替换及场景导出，支持通过更新布局字段进行迭代编辑。
  - **数据构建**：整合五个大规模数据集（Omni3D、SUN RGB-D、ARKitScenes、Hypersim、KITTI、Objectron），覆盖室内、室外、桌面场景，将异构3D标注统一为图像-相机内参-度量3D包围盒的指令微调格式。

### 3. 实验设计
- **任务与数据集**：
  - **3D布局估计**：从单图预测对象度量3D包围盒，测试集从上述五个数据集中均匀采样1000张图像（GPT等闭源模型用100张子集）。
  - **端到端3D场景生成**：从单图生成完整可编辑场景，使用常用场景生成评估标准。
- **评估指标**：
  - 布局估计：`F1@IoU_3D`（阈值0.25和0.5）。
  - 场景生成：`Volumetric IoU`（体素交并比）、`Collided Pairs`（碰撞对象对数）、`CLIP Score`（语义对齐）、`FID`（真实感）。
- **对比方法**：
  - 布局估计：Qwen3-VL 不同规模指令模型（2B/8B/30B-A3B，同样输出JSON）、闭源 MLLM（GPT-5.2、Gemini 3 Pro）。
  - 场景生成：前馈模型 SceneGen，以及 Qwen3-VL-30B-A3B-Instruct 使用相同工具链执行布局预测后的结果。

### 4. 资源与算力
- **训练配置**：基于 Qwen3-VL-2B-Instruct 进行微调，单次训练使用 **4块 NVIDIA A800 (80GB)** GPU。
- **训练细节**：BF16精度，20个epoch，最大序列长度2048，micro-batch size为1，梯度累积步数16（等效总batch size约64）。
- **微调策略**：进行全参数微调，与 LoRA 微调进行对比。

### 5. 实验数量与充分性
- **实验组数概览**：
  - 主要结果表2组：布局估计多模型对比（含多个参数量的开源与闭源模型）。
  - 场景生成表1组：与SceneGen、Qwen30B+工具链对比。
  - 消融实验表1组：移除几何头与双重监督、移除朝向离散化，以及与纯JSON交叉熵训练的基线对比。
  - 微调策略对比表1组：全微调 vs. LoRA。
  - 定性分析：多场景（室内、室外、桌面）可视化比较。
- **充分性与公平性**：实验覆盖了布局估计和场景合成两个层面，对比范包含开源、闭源大模型以及专用生成模型；消融实验清晰验证了几何头、朝向离散化和双重监督的贡献。对比时使用相同工具链执行，控制变量，较为公平。闭源模型仅在子集评估可能稍显不足，但考虑到调用成本尚可理解。

### 6. 主要结论与发现
- SceneLM 在单张图像3D布局估计上显著优于所有对比的开放和商用MLLM，特别是在高精度IoU（0.5）下优势明显，证明几何辅助头有效提升度量准确性。
- 在端到端场景生成中，SceneLM 在几何一致性（Volumetric IoU）、物理合理性（低碰撞对数）、语义对齐（CLIP Score）和真实感（FID）上均取得最佳表现，且能生成更完整的物体集合。
- 对朝向进行离散化分类（24 bins）是稳定自回归训练的关键，纯数值回归容易导致不稳定。
- 全参数微调在精确3D布局任务中明显优于LoRA，建议在有条件时使用完整微调。

### 7. 优点
- **创新地融合语言模型与度量3D理解**：通过辅助几何头在保持语言接口的前提下强化数值精度，为LLM赋予可执行的3D感知能力。
- **完整的可编辑场景合成流水线**：从布局预测到工具调用，支持灵活编辑和迭代优化，区别于传统“一次生成”的几何管线。
- **大规模异构数据统一与训练**：整合并标准化五个数据集，形成可扩展的指令微调范式。
- **实验充分**：涵盖多个基准、消融、定性分析，结论说服力强。

### 8. 不足与局限
- **依赖相机内参**：尽管未提供时应使用默认值，但默认参数会引入绝对尺度和尺寸估计的偏差。
- **生成质量受外部资产与工具制约**：最终场景的逼真度取决于 3D 资产生成（如Hunyuan3D）的质量，碰撞处理等也非模型直接控制。
- **复杂场景下仍可能出现 JSON 格式错误或不完整输出**，文中提及但未详细量化该故障率。
- **闭源模型对比规模受限**：GPT和Gemini仅在100张图像上评估，更大规模对比可能更具说服力。
- **场景类型通用性有待验证**：数据虽涵盖多域，但极端复杂或罕见场景的泛化性尚不明确。

（完）
