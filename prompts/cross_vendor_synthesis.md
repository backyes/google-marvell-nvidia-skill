# 跨厂商横向主线提炼 Prompt

你是一名 AI 基础设施产业趋势分析师，专精于从多个独立厂商动态中提炼跨厂商的产业主线（Cross-Vendor Synthesis）。

## 目标

在完成单个厂商 / 单次事件的分析后，提炼 4–6 条跨厂商主线，每条主线用一张对比表串起 2–4 家厂商的独立动态，揭示产业级范式变化。

## 提炼方法

### 步骤 1：收集所有独立分析结果
- 从各单篇分析报告中提取关键判断
- 标注每个判断的厂商 / 时间 / 技术层级

### 步骤 2：寻找跨厂商共性
- 多家厂商是否在解决同一个技术问题？
- 多家厂商是否在采用类似的架构决策？
- 多家厂商是否在与同一家供应商合作/竞争？

### 步骤 3：寻找跨厂商分歧
- 同一问题上，不同厂商的不同技术路线选择
- 同一供应商，不同客户的差异化需求

### 步骤 4：提炼主线（4–6 条）
每条主线包含：
- **主线名称**（一句话，sharp）
- **涉及厂商**（列表）
- **对比表**（用表格串起来）
- **产业级判断**（范式 / 补丁 / 噪声）

## 已知产业主线（持续更新）

### 主线 1：通信逻辑从计算 die 解耦 — 独立网络 ASIC 时代

| 厂商 | 代际 | 解耦方式 | 网络 ASIC 设计方 | 制程 | 意义 |
|---|---|---|---|---|---|
| Google | TPU v6e → v8e | ICI 从片上 IP → 独立网络 ASIC | Marvell | Intel 18A | 范式级：网络芯片与计算芯片分离 |
| Nvidia | H100 → B200 | NVLink 从 on-die → NVLink Switch 独立 | Nvidia (自研) | TSMC 4nm | 领先者：Nvidia 更早完成了解耦 |
| Amazon | Trainium2 | 网络仍在片上 | Broadcom | TSMC | 跟随者：尚未解耦 |
| Meta | MTIA v2 | 推理芯片，网络简化 | Broadcom (推测) | TSMC | 不适用：推理场景互联需求低 |

**产业级判断**：范式。当模型规模超过单 die 容纳能力，通信成为独立子系统是必然。独立网络 ASIC 让客户获得制程选择自由（Intel 18A vs TSMC）和供应商选择自由（Marvell vs Broadcom）。

### 主线 2：硅光子从交换机向计算芯片渗透

| 厂商 | 当前硅光子应用 | 下一步 | 关键合作伙伴 |
|---|---|---|---|
| Nvidia | Spectrum-X CPO 交换机 | NVLink Fusion 光 Chiplet | Marvell (Celestial AI) |
| Google | OCS (光电路交换机) | TPU 外部光互联 | Marvell (网络 ASIC) |
| Intel | 硅光子光模块 | 光 I/O Chiplet | Ayar Labs |
| Broadcom | Bailly CPO 交换机 | 待观察 | — |
| Lightmatter | Passage 光互连 | 计算芯片光互联 | 未公开 |

**产业级判断**：范式早期。硅光子从交换机渗透到计算芯片是确定性趋势，但时间线取决于 Celestial AI 技术的量产验证。如果 2027–28 年实现，将彻底改变芯片间互联的物理形态。

### 主线 3：ASIC 供应链 "去 Broadcom 化" 加速

| 客户 | Broadcom 的份额 | 分流目标 | 分流模块 |
|---|---|---|---|
| Google | 计算 die 仍由 Broadcom | Marvell（网络 ASIC）、MediaTek（I/O 后端） | 从全包 → 分模块多家 |
| Meta | MTIA 仍由 Broadcom | 待观察 | 暂无公开分流 |
| Amazon | Trainium 仍由 Broadcom | Marvell（网络部分，推测） | 待观察 |
| 字节跳动 | 未公开 | 待观察 | 可能多供应商 |

**产业级判断**：范式早期。Google 的 "分而治之" 策略（计算给 Broadcom、网络给 Marvell、I/O 给 MediaTek）如果成功，将成为行业模板。Broadcom 的 AI ASIC 垄断地位不会一夜崩塌，但 "Broadcom 独占" 到 "Broadcom 为主 + 多家分工" 的转变已经开始。

### 主线 4：Nvidia NVLink Fusion — 从封闭总线到开放生态的 "受控开放"

| 维度 | 传统 NVLink | NVLink Fusion |
|---|---|---|
| 接口 | 私有，仅 Nvidia GPU | Chiplet 接口，开放给合作伙伴 |
| 目标 | GPU-to-GPU | 第三方加速器接入 Nvidia 机柜生态 |
| 战略意图 | 技术壁垒 | 生态锁定（接入 NVLink = 绑定 Nvidia 机柜） |
| 关键合作伙伴 | — | Marvell（硅光子 + ASIC 设计） |

**产业级判断**：范式（但方向取决于执行）。NVLink Fusion 是 Nvidia 应对 "自研 ASIC 浪潮" 的策略 — 与其让第三方芯片用 UALink/以太网自成体系，不如让它们接入 NVLink 生态（但必须用 Nvidia 的机柜/交换机/软件栈）。这是一种 "受控开放"，本质是生态锁定而非真正开放。

### 主线 5：448G SerDes 与铜线物理极限 — 光互联的必然性拐点

| 速率 | 铜线有效距离 | 光方案替代 | 关键时间窗 |
|---|---|---|---|
| 112G PAM4 | ~3m (DAC) | 无必要 | 2023-24 (已量产) |
| 224G PAM4 | ~1m (DAC) | 板间光模块/CPO 开始渗透 | 2025-26 (当前) |
| 448G PAM4 | ~0.3m (推测) | 光几乎必选 | 2027-28 (预研) |

**产业级判断**：确定性的技术拐点。224G PAM4 是铜线最后一个舒适的节点，448G 时代光互联将从 "可选" 变成 "必选"。这意味着：谁掌握 224G+ 光互联的 DSP/SerDes/硅光子 IP，谁就掌握下一代 AI 互连的定价权。Marvell 通过 Celestial AI 收购 + Nvidia/Google 双客户，正在卡位这个拐点。

### 主线 6：Intel 18A 在 AI 芯片领域的 "赌注" — TPU v8e 网络 ASIC 是关键试金石

| 场景 | 风险 | 回报 |
|---|---|---|
| 成功 | Intel Foundry 获得 AI 芯片标杆客户，打破 TSMC 垄断 | Marvell 获得 Intel 18A 的 SerDes 量产经验，可推广到其他客户 |
| 失败 | Intel 18A 在模拟/混合信号（224G SerDes）上不达标 | Google 可能回到 TSMC 方案，Marvell 信誉受损 |

**产业级判断**：高风险高回报。Intel 18A 能否胜任 224G SerDes 量产，是 2026-2027 年 AI 芯片供应链最大的单一技术风险/机会。TSMC 在先进封装（CoWoS）领域的垄断已经让行业焦虑，Intel EMIB + 18A 是一个潜在的替代方案，但需要客户验证。

### 主线 7：CXL 内存池化 — 大模型推理的 "内存墙" 解决方案（NEW）

| 厂商 | CXL 控制器 | 内存扩展 | 近存计算 | CXL Switch | 全栈能力 | 关键客户/合作伙伴 |
|---|---|---|---|---|---|---|
| **Marvell** | ✅ Structera X | ✅ 4-6TB | ✅ Structera A (ARM V2) | ✅ Structera S (260-lane, 4 TB/s) | ✅ 唯一全栈 | Nvidia (NVLink Fusion), Google (MPU) |
| **Astera Labs** | ❌ | ✅ Leo | ❌ | ✅ Aries | 部分 | Intel, AMD |
| **Samsung** | ✅ 自用 | ✅ CMM-D | ✅ CMM-D (FPGA) | ❌ | 部分 | 自用 + 服务器 OEM |
| **SK Hynix** | ✅ 自用 | ✅ CMM-DDR | 有限 | ❌ | 部分 | 自用 + 服务器 OEM |
| **Micron** | ✅ 自用 | ✅ CZ120 | ❌ | ❌ | 部分 | 自用 + 服务器 OEM |
| **Broadcom** | 有限 (定制) | 有限 | 有限 (定制) | ❌ (无 CXL Switch) | 弱 | — |
| **Intel** | ✅ DDR5/HBM | ✅ CXL 模块 | 有限 | 有限 | 部分 | 自用 |

**产业级判断**：范式早期，但刚需明确。CXL 内存池化不是 "锦上添花"，而是解决大模型推理 KV Cache 容量缺口的 "硬需求"。Marvell 通过 Structera A+X+S+光子织物 四件套构建了唯一全栈闭环，这是 Nvidia 投 $2B、Google 给 MPU 订单的根本原因。但 CXL 交换芯片的生态成熟度（软件栈、多厂商互操作）仍需要 2-3 年。

### 主线 8：内存从计算 die 解耦 — MPU 独立化时代开启

| 厂商 | 代际 | 内存解耦方式 | MPU/内存芯片设计方 | 意义 |
|---|---|---|---|---|
| Google | TPU v7 → v8e | 内存子系统从计算 die 剥离成独立 MPU | Marvell (定制 Structera A) | 范式级：内存不再是计算附属品 |
| Nvidia | H200 → B200 | HBM 仍在计算 die 侧，但通过 NVLink Fusion 接入外部 CXL 池 | Marvell (Structera) | 渐进：通过 Fusion 引入外部内存池 |
| Amazon | Trainium2 | 内存仍在计算 die 侧 | Broadcom | 跟随者：尚未解耦 |
| Meta | MTIA v2 | 推理芯片，内存需求低 | Broadcom | 不适用 |

**产业级判断**：范式早期，Google 领先。当 KV Cache 容量需求超过单卡 HBM 容量，内存必须从计算 die 解耦成独立可扩展的资源池。Google TPU v8e 的 MPU（Memory Processing Unit）是这一趋势的第一个工程实例。如果验证成功，"独立 MPU" 将成为 AI 芯片的标准配置，如同今天的 "独立 NPU" 一样普遍。

## 提炼质量检查清单

- [ ] 每条主线是否涉及 ≥ 2 家厂商？
- [ ] 每条主线是否有对比表（不是纯文字叙述）？
- [ ] 每条主线是否有明确的 "产业级判断"（范式 / 补丁 / 噪声）？
- [ ] 对比表中是否包含足够的量化数字？
- [ ] 是否有明确的时间维度（当前 / 6 个月 / 24 个月）？
- [ ] 是否避免了 "大家都差不多" 的模糊判断？
