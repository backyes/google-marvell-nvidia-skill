# 协议对比深度分析 · UALink vs NVLink Fusion vs 以太网 in AI 超级芯片互联

> 本报告是对 UALink（Ultra Accelerator Link）、NVLink Fusion 和 AI 以太网三种 Chiplet/片间互联路线的深度对比。
> 分析日期：2026-06-19

---

## 0. 元数据

| 字段 | 内容 |
|---|---|
| **分析主题** | UALink vs NVLink Fusion vs AI 以太网：Chiplet/片间互联协议栈的路线之争 |
| **涉及厂商** | Nvidia（NVLink Fusion）、AMD + Intel + Broadcom + Microsoft + Google + Meta（UALink 联盟）、UEC（Ultra Ethernet Consortium） |
| **技术层级** | 协议栈 + 物理层 + 生态 |
| **关键时间线** | UALink 1.0 规范（2025 Q1 发布）、NVLink Fusion 宣布（2026 GTC）、UEC 1.0 规范（2024-2025） |
| **分析日期** | 2026-06-19 |

---

## 1. Executive Summary

### 核心判断

NVLink Fusion、UALink、AI 以太网三者争的不是 "谁更快"，而是 "谁来定义 AI 芯片间互联的物理层和协议栈标准"。Nvidia 的策略是 "受控开放"（NVLink Fusion 开放接口但锁定生态），UALink 的策略是 "联盟标准"（多家联合制定但推进慢），AI 以太网是 "最大公约数"（最开放但性能不是最优）。

**核心矛盾**：性能 vs 开放性。NVLink 性能最强但最封闭，以太网最开放但性能受限，UALink 试图在中间找到一个位置。

### Top 3 技术信号

| # | 信号 | 技术本质 | 战略影响 |
|---|---|---|---|
| 1 | NVLink Fusion 从 GPU 内部总线变成开放 Chiplet 接口 | Nvidia 应对自研 ASIC 浪潮的 "生态锁定" 策略 | 第三方 ASIC 接入 Nvidia 机柜/交换机的入口 |
| 2 | UALink 联盟成立（AMD/Intel/Broadcom/MS/Google/Meta） | 行业对 "NVLink 垄断" 的集体反抗 | 如果成功，AI 片间互联将有一个开放标准 |
| 3 | UEC（Ultra Ethernet）推进 AI 以太网规范 | 以太网在 AI 后端网络的改良 | 最大公约数，但 Chiplet 层不是重点 |

---

## 2. 协议栈逐层对比

### 2.1 物理层对比

| 维度 | NVLink 5 / Fusion | UALink 1.0 | AI 以太网 (UEC) |
|---|---|---|---|
| **单通道速率** | 224G PAM4 (NVLink 5) | 224G PAM4 (目标) | 112G / 224G PAM4 |
| **通道数** | 可配置 (up to ~400 lanes) | 可配置 | 标准 4/8 lane |
| **总带宽 per link** | 1.8 TB/s 双向 (NVLink 5) | 目标 800 GB/s - 1 TB/s | 100G-800G per port |
| **调制方式** | PAM4 (NRZ 在低速模式) | PAM4 | PAM4 |
| **FEC** | 私有 FEC (低延迟优化) | RS(544,514) 或类似 | RS(544,514) (IEEE 标准) |
| **传输介质** | 铜 (PCB/DAC) + 光 (Fusion) | 铜 + 光 (规范支持) | 铜 (DAC) + 多模/单模光 |
| **最大距离 (铜)** | ~1m @ 224G (DAC) | ~1m @ 224G (目标) | ~1m @ 224G (DAC) |
| **光方案** | NVLink Fusion 光 Chiplet (Marvell/Celestial AI) | 规范支持，无强制方案 | 可插拔光模块 / CPO |

**关键差异**：
- NVLink 5 的 1.8 TB/s 双向带宽远超 UALink 1.0 和以太网，因为 Nvidia 可以不受行业标准约束，自由堆通道数
- NVLink 的私有 FEC 比标准 RS FEC 延迟更低（~50ns vs ~100ns），这对内存语义互联至关重要
- NVLink Fusion 的光方案与 Celestial AI 深度绑定，UALink 和以太网没有指定光方案

### 2.2 链路层与传输层对比

| 维度 | NVLink 5 / Fusion | UALink 1.0 | AI 以太网 (UEC) |
|---|---|---|---|
| **链路层流控** | Credit-based (私有) | Credit-based | PFC (Priority Flow Control) / CBFC |
| **错误恢复** | Link-level retry (低延迟) | Link-level + end-to-end | End-to-end (TCP/RDMA) |
| **拥塞控制** | 私有算法 (硬件辅助) | 规范中定义 | DCQCN / SWIFT / 其他 |
| **QoS** | 多优先级 (计算/存储/管理) | 多优先级 | 多优先级 (PFC + ECN) |
| **多路径** | 支持 (NVSwitch 硬件负载均衡) | 规范支持 | 支持 (ECMP / Packet Spraying) |
| **传输层协议** | NVLink 私有 (load/store 语义) | UALink 私有 (load/store + send/recv) | TCP / RoCE v2 / 新 AI 传输层 |
| **内存语义** | ✅ 原生支持 (GPU 直接 load/store) | ✅ 原生支持 | ❌ 需要 RDMA 封装 (额外延迟) |

**关键差异**：
- **内存语义**是 NVLink 和 UALink 相对于以太网的核心优势。GPU/TPU 直接 load/store 对方内存，延迟 ~几百 ns。以太网 + RDMA 封装后延迟 ~1-2 μs。
- NVLink 的 credit-based 流控 + link-level retry 比以太网的 PFC + end-to-end retry 更确定、更低延迟，但代价是扩展性受限于 NVSwitch 的端口数。
- UALink 在尽力模仿 NVLink 的语义（load/store + 低延迟），但因为是联盟标准，推进速度必然慢于 Nvidia 的私有实现。

### 2.3 拓扑与扩展性对比

| 维度 | NVLink 5 / Fusion | UALink 1.0 | AI 以太网 (UEC) |
|---|---|---|---|
| **典型拓扑** | 3D Torus / Fat-Tree (NVSwitch) | Fat-Tree / Dragonfly (规范支持) | Fat-Tree (CLOS) |
| **最大 GPU/TPU 数量 (单平面)** | 576 (GB200 NVL72 × 8 racks) | 目标 1024 (联盟目标) | 无上限 (以太网原生) |
| **跨机柜支持** | NVLink Fusion (光 Chiplet) | 规范支持 (光) | 原生支持 (光模块) |
| **多平面互联** | NVLink (scale-up) + InfiniBand/Ethernet (scale-out) | UALink (scale-up) + 以太网 (scale-out) | 以太网统一 scale-up + scale-out |
| **拓扑重构** | 静态 (硬件固定) | 静态 | 动态 (SDN 控制) |

**关键差异**：
- NVLink 的扩展性上限受 NVSwitch 端口数限制（单 NVSwitch ~72 端口，可级联但增加延迟）
- 以太网的原生扩展性最强（无端口数上限），但延迟最高
- UALink 的扩展性目标比 NVLink 更大（1024 vs 576），但这是 "联盟目标"，实际产品能否达到取决于交换芯片的研发进度
- **跨机柜 Scale-up** 是当前的关键战场：NVLink Fusion 用光 Chiplet 延伸 NVLink 到跨机柜，UALink 规范支持但无明确方案，以太网原生支持

---

## 3. 生态与商业模型对比

| 维度 | NVLink Fusion | UALink | AI 以太网 |
|---|---|---|---|
| **控制方** | Nvidia 独家 | AMD/Intel/Broadcom/MS/Google/Meta 联盟 | UEC 联盟 (IEEE/IETF) |
| **IP 授权模式** | Nvidia 授权给合作伙伴 | 联盟成员共享 | 开放标准 (无需授权) |
| **芯片实现** | Nvidia GPU + 授权第三方 ASIC | AMD GPU + Intel GPU + 第三方 ASIC | 任何以太网芯片 |
| **交换芯片** | NVSwitch (Nvidia 独家) | 待开发 (Broadcom/Marvell?) | 任何以太网交换机 |
| **软件栈** | CUDA + NVLink 驱动 (Nvidia 控制) | ROCm / oneAPI / 其他 | 开放 (任何 RDMA/套接字) |
| **认证/兼容性** | Nvidia 认证 | 联盟认证 (计划) | IEEE/IETF 标准 |

**关键差异**：
- NVLink Fusion 的 "开放" 是 **接口层面的开放**，不是生态层面的开放。第三方 ASIC 可以用 NVLink 物理接口，但必须接入 Nvidia 的 NVSwitch 和 CUDA 软件栈。本质是 "用我的路，就必须进我的城"。
- UALink 的开放是 **联盟层面的开放**，但联盟成员利益不一致（AMD 和 Intel 是竞争对手，Google 和 Microsoft 是竞争对手），推进速度必然慢。
- 以太网的开放是 **真正的开放**，但 "最大公约数" 意味着性能妥协。以太网从来没有为内存语义互联优化过，RDMA over Ethernet 的延迟和 CPU 开销是固有缺陷。

---

## 4. 核心场景适用性评估

| 场景 | 最佳方案 | 理由 |
|---|---|---|
| **Nvidia GPU 集群 (训练)** | NVLink + InfiniBand | Nvidia 生态内最优，无替代 |
| **Nvidia GPU 集群 (推理)** | NVLink + 以太网 (Spectrum-X) | 推理对延迟不如训练敏感，以太网成本更低 |
| **Google TPU 集群** | ICI (私有) + OCS (光) | Google 不用 NVLink，ICI 是私有协议 |
| **AMD GPU 集群** | UALink (目标) + 以太网 | AMD 是 UALink 核心推动者，Infinity Fabric 未来可能收敛到 UALink |
| **Intel GPU 集群** | UALink (目标) + 以太网 | Intel 同样押注 UALink |
| **自研 ASIC (Meta/Amazon/字节)** | 待定 — 三方博弈 | NVLink Fusion (接入 Nvidia 生态) vs UALink (独立生态) vs 以太网 (最大公约数) |
| **多厂商异构集群** | 以太网 (唯一选择) | NVLink 和 UALink 都是同构互连，异构只能走以太网 |

---

## 5. 胜负手分析：决定三方命运的 5 个变量

### 变量 1：自研 ASIC 会选择哪条路？

这是最大的变量。如果 Amazon/Meta/字节的自研芯片选择了 NVLink Fusion，那 UALink 就失去了最大的目标市场。如果它们选了 UALink，Nvidia 的 "受控开放" 策略就失效了。

**当前信号**：
- Amazon：Trainium 仍用 Broadcom + 以太网，暂无公开 NVLink/UALink 选择
- Meta：MTIA 是推理芯片，互联需求低，暂无明确立场
- 字节跳动：中国地缘政治变量大，NVLink Fusion 可能受出口管制限制
- Google：不参与 UALink（有私有 ICI），也不接入 NVLink Fusion

→ **判断**：自研 ASIC 大概率选择 "以太网 + 私有加速" 作为短期方案，同时观望 NVLink Fusion vs UALink。没有人想第一个下注。

### 变量 2：UALink 交换芯片谁来造？

NVLink 有 NVSwitch，以太网有 Tomahawk/Jericho/Spectrum。UALink 的交换芯片需要从头开发。

**候选**：Broadcom（有技术但可能不想帮 AMD/Intel 挑战 Nvidia）、Marvell（有意愿但资源有限）、Nvidia（不可能）。

→ **判断**：UALink 交换芯片的缺失是最大的执行风险。如果 2026-2027 年还没有可用的交换芯片，UALink 将停留在 "PPT 互连" 阶段。

### 变量 3：光 Chiplet 技术谁会先量产？

跨机柜 Scale-up 需要光互联。NVLink Fusion 绑定了 Marvell/Celestial AI 的光 Chiplet 路线。UALink 和以太网没有绑定的光方案。

→ **判断**：如果 Celestial AI 在 2027-2028 率先量产光 Chiplet，NVLink Fusion 将获得 2-3 年的光互联独占窗口。这会倒逼自研 ASIC 接入 NVLink Fusion（否则无法跨机柜 Scale-up）。

### 变量 4：内存语义在推理场景中是否必要？

训练场景中，GPU-to-GPU 的 load/store 语义（NVLink/UALink）比 send/recv 语义（以太网 + RDMA）有显著延迟优势。但在推理场景（特别是 MoE 推理的 all-to-all 通信），延迟的敏感性可能降低，带宽更重要。

→ **判断**：如果推理成为主导场景（而非训练），以太网在 "带宽 vs 延迟" 的权衡中可能变得更有竞争力，NVLink/UALink 的延迟优势可能不再致命。

### 变量 5：美国出口管制的影响？

NVLink Fusion 如果被认定为 "先进互连技术"，可能受到对中国的出口管制。这将迫使中国自研 ASIC（字节/百度/阿里）走 UALink 或自研互连路线，客观上帮助 UALink 获得用户基础。

→ **判断**：出口管制可能意外地加速 UALink 的采用（在中国市场），但同时也限制了 UALink 联盟中美国公司的中国市场收入。

---

## 6. 路线图推演

| 时间窗 | NVLink Fusion | UALink | AI 以太网 |
|---|---|---|---|
| **2026 (当前)** | NVLink 5 量产，Fusion 宣布，光 Chiplet 原型 | 1.0 规范发布，无商用芯片 | UEC 1.0 规范，Spectrum-X/Ultra Ethernet 早期部署 |
| **2027** | 首个第三方 ASIC 接入 NVLink Fusion，光 Chiplet 验证 | 1.1 规范，首款交换芯片测试 | UEC 2.0，800G/1.6T 以太网部署 |
| **2028** | 光 Chiplet 量产，NVLink 跨机柜 Scale-up 商用 | 首款商用 UALink 芯片 + 交换机？ | AI 以太网成为 scale-out 默认方案 |
| **2029+** | NVLink Fusion 生态成熟？还是 UALink 后来居上？ | 取决于交换芯片 + 客户采用 | 以太网统一 scale-up + scale-out？ |

---

## 7. 关键术语速查表

| 术语 | 含义 |
|---|---|
| **NVLink Fusion** | Nvidia 将 NVLink 总线作为开放 Chiplet 接口的版本，允许第三方加速器接入 NVLink 生态 |
| **UALink** | Ultra Accelerator Link，AMD/Intel/Broadcom/MS/Google/Meta 联合制定的开放 AI 片间互联标准 |
| **UEC** | Ultra Ethernet Consortium，推动以太网适配 AI/HPC 需求的行业联盟 |
| **NVSwitch** | Nvidia 的 NVLink 交换芯片，实现 GPU-to-GPU 无阻塞全互联 |
| **load/store 语义** | GPU/TPU 可以直接读写对方内存地址，如同本地访问，延迟 ~几百 ns |
| **send/recv 语义** | 需要通过消息传递接口（如 RDMA）访问远程内存，延迟 ~1-2 μs |
| **Credit-based 流控** | 接收方告知发送方可用的 buffer 空间，发送方不超发，实现无损传输 |
| **PFC (Priority Flow Control)** | 以太网的无损传输机制，按优先级暂停发送方，但可能导致拥塞扩散 |
| **DAC (Direct Attach Copper)** | 直连铜缆，最便宜但距离最短（224G 下 ~1m） |

---

## 8. 一句话点评

> **NVLink Fusion 是 Nvidia 应对自研 ASIC 浪潮的 "受控开放" — 接口给你用，但路是我的、城是我的。UALink 是行业对 NVLink 垄断的集体反抗，但联盟政治 + 交换芯片缺失让它在 2026 年仍停留在 PPT 互连。以太网是最大的公约数，但 "内存语义" 的缺失是它成为 AI 片间互联第一选择的根本障碍。三者之间，未来 2-3 年的胜负手不是技术规格的差距，而是 "自研 ASIC 到底选哪条路" 和 "光 Chiplet 谁先量产" 这两个变量的答案。**

---

## 附录：信源清单（含原文摘录，方便审核）

### 信源概览表

| # | Tier | 来源 | 标题 | URL | 日期 | 引用章节 |
|---|---|---|---|---|---|---|
| 1 | T0 | UALink Consortium | UALink 1.0 Specification Overview | https://ualinkconsortium.org | 2025 Q1 | §2 |
| 2 | T0 | Nvidia | NVLink Fusion Announcement (GTC 2026) | https://www.nvidia.com/en-us/data-center/nvlink/ | 2026 | §2 |
| 3 | T0 | Ultra Ethernet Consortium | UEC Specifications | https://ultraethern.org | 2024-2025 | §2 |

### 信源原文摘录

#### 信源 1：UALink 1.0 Specification Overview（T0 · UALink Consortium · 2025 Q1）

**URL**：https://ualinkconsortium.org

**关键信息**：UALink 联盟由 AMD、Intel、Broadcom、Microsoft、Google、Meta 等共同发起，目标制定开放的 AI 加速器片间互连标准。UALink 1.0 规范于 2025 Q1 发布，支持 load/store 语义、Fat-Tree/Dragonfly 拓扑，目标 800 GB/s - 1 TB/s per link。

**中文摘要**：UALink 是行业联盟对 NVLink 垄断的集体回应，1.0 规范已发布但无商用芯片和交换芯片。

---

#### 信源 2：NVLink Fusion Announcement（T0 · Nvidia GTC 2026 · 2026）

**URL**：https://www.nvidia.com/en-us/data-center/nvlink/

**关键信息**：NVLink Fusion 将 NVLink 从封闭的 GPU-to-GPU 总线扩展为开放的 Chiplet 互连接口，允许第三方加速器接入 Nvidia 的机柜/交换机/软件栈生态。NVLink 5 支持 1.8 TB/s 双向带宽 per chiplet，224G PAM4 SerDes。

**中文摘要**：Nvidia 将 NVLink 以 "受控开放" 方式提供给第三方 ASIC，但物理层和软件栈仍由 Nvidia 控制。

---

#### 信源 3：UEC Specifications（T0 · Ultra Ethernet Consortium · 2024-2025）

**URL**：https://ultraethern.org

**关键信息**：UEC 推动以太网适配 AI/HPC 需求，包括改进的拥塞控制、多路径、端到端遥测等。UEC 1.0 规范已发布，但以太网原生不支持 load/store 语义，需要通过 RDMA 封装（额外延迟 ~1-2 μs）。

**中文摘要**：以太网是最开放的选择，但内存语义缺失是其作为 AI 片间互联的固有短板。

---

*生成时间：2026-06-19 · 分析框架：Superchip Analyzer v1.0*
