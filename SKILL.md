---
name: superchip-analyzer
description: 超级芯片互联 / 内存控制器 / 先进封装 / SerDes / 硅光子 领域的"深度技术架构分析"生成器。
  当用户提到"超级芯片"、"NVLink"、"NVLink Fusion"、"UALink"、"CXL"、"芯片互联"、"内存控制器"、
  "SerDes"、"硅光子"、"CPO"、"CoWoS"、"EMIB"、"Chiplet"、"TPU 互联"、"ICI"、"光子织物"、
  "Nvidia Marvell"、"Google Marvell"、"Broadcom ASIC"、"超节点互联"、"光电共封装"、
  "224G"、"448G"、"PAM4"、"Photonic Fabric"、"Celestial AI"、"异构集成"、"2.5D/3D 封装"、
  "CXL 内存"、"内存池化"、"近存计算"、"KV Cache"、"Structera"、"MPU"、"内存扩展"、
  "CXL Switch"、"CXL Fabric"、"Astera Labs"、"DIMM Recycling"、"Composable Memory"时使用此技能。
  输出面向 AI Infra 架构师 / 芯片设计团队 / 互连系统工程师 / 投研团队，强调"物理层约束优先 + 解耦与分解方法论 + 多供应商博弈分析"。
---

# Superchip Analyzer Skill（超级芯片互联深度分析生成器）

把"分析 Nvidia / Google / Marvell / Broadcom 在超级芯片互联、内存控制器、先进封装、硅光子等领域的合作与竞争"这件事固化下来的 skill。核心方法论：**物理层约束优先于营销命名、解耦与分解（Decoupling & Disaggregation）优先于黑盒分析、技术 IP 优势 + 供应链地缘政治双维度评估**。

## 领域知识图谱（五大支柱）

### 支柱 1：高速互连与协议栈
- **NVLink 1.0–5.0 & NVLink Fusion**：1.8 TB/s 双向 per chiplet，Fusion 标志着 NVLink 从封闭总线走向开放 Chiplet 接口
- **UALink（Ultra Accelerator Link）** vs NVLink 生态竞争：协议封装、3D Torus / Fat-Tree 拓扑选择
- **CXL 3.0+**：内存池化、缓存一致性、跨机柜资源解耦
- **谷歌 ICI（Inter-Chip Interconnect）**：TPU v4–v8e 各代 ICI 演进路径，从片上 IP 到独立网络 ASIC 的解耦过程

### 支柱 2：物理层（PHY）与 SerDes 工程
- **PAM4 信令**：112G → 224G → 448G 三代演进，每代 SNR 预算、FEC 开销、功耗曲线
- **信号完整性**：铜背板 vs 光收发器的抖动/噪声抑制、多 GHz 能效指标
- **距离-带宽-功耗 不可能三角**：铜线在 224G+ 下的物理极限（~1m@224G PAM4），光互联的必然性拐点

### 支柱 3：先进封装与异构集成
- **TSMC CoWoS-S/R/L**：硅中介层逻辑，各变体面积上限与互连密度
- **Intel EMIB**：嵌入式多芯片互连桥，与 CoWoS 的成本/良率/灵活性对比
- **2.5D/3D Chiplet 堆叠**：UCIe 标准、混合键合（Hybrid Bonding）、热管理约束
- **CPO（Co-Packaged Optics）**：光引擎与交换 ASIC 共封装，功耗节省的理论上限与实际瓶颈
- **硅光子（SiPh）与光子织物（Photonic Fabric）**：Celestial AI 的 3D 垂直光路由技术，单片 16 Tbps 带宽，高热稳定性光 Chiplet 直接 off compute die 出光

### 支柱 4：内存控制器与 CXL 内存池化（NEW）
- **大模型推理的 KV Cache 内存墙**：KV Cache 容量需求 vs 单卡 HBM 剩余空间 → CXL 池化的刚需量化
- **CXL 全栈四层模型**：内存控制器（Controller）→ 内存扩展（Expansion）→ 近存计算（Near-Memory Computing）→ CXL 交换与池化（Switching & Pooling）
- **Structera 产品家族**：A 系列（近存计算加速器，ARM V2 核心）、X 系列（内存扩展控制器，4-6TB 容量）、S 系列（CXL 3.0 交换芯片，260-lane / 4 TB/s / <460ns）
- **近存计算（Near-Memory Computing）**：在 CXL 控制器侧集成 ARM 核心 + 硬件加速引擎，卸载 GPU/TPU 的 KV Cache 压缩/解压、向量检索、数据预处理任务
- **DIMM Recycling**：旧代 DDR4 内存循环利用，降低 CXL 内存池的 CapEx
- **Composable Memory Pooling**：多主机动态共享 CXL 内存池，按需分配/回收

### 支柱 5：超大规模 ASIC 系统解构
- **Google TPU v4–v8e**：计算 die、I/O die、网络 ASIC、**MPU（Memory Processing Unit）** 的逐代分解图
- **Amazon Trainium/Inferentia**：自研芯片的互连选型（EFA/Nitro 与芯片内网络的关系）
- **Meta MTIA**：推理芯片的互联策略
- **Microsoft Maia**：CXL 与片间互联的角色

## 分析方法论（严格约束）

### 原则 1：物理层约束优先
> 永远先问"铜线能跑多远？功耗预算是多少？"再谈协议和生态。

- 任何 "下一代互联" 的声明，先验证其在 224G/448G PAM4 下的物理可实现性
- 距离超过 1m 的 224G+ 电互联，必须论证信号调理方案（retimer / redriver / DSP）
- 跨机柜互联默认走光，除非有明确证据证明电方案可行

### 原则 2：KV Cache 容量缺口量化优先（内存子系统专用原则）
> 在评估任何 CXL 内存池化方案前，先算清楚 "KV Cache 需要多少 GB？单卡 HBM 还剩多少？"

- 公式：`KV Cache 需求 = 模型层数 × 每层 KV/token × 上下文长度 × Batch Size`
- 如果 `KV Cache 需求 > 单卡 HBM 剩余空间`，CXL 池化的刚需就成立
- 容量缺口越大，CXL 的价值越大（替代方案 = 多卡拆分 → GPU 利用率崩塌）

### 原则 3：解耦与分解（Decoupling & Disaggregation）
> 分析架构演进时，按"计算逻辑 → 通信/路由逻辑 → I/O 后端 → 内存子系统"四层分解。

- 当通信功能从片上 IP 变成独立 ASIC（如 TPU v8e 的 ICI → 独立网络芯片），这是范式级变化
- **当内存子系统从计算 die 解耦成独立 MPU**（如 Google TPU v8e 的 MPU → Marvell 定制），这是更底层的范式变化 — 它意味着"内存"不再是计算的附属品，而是独立可扩展的资源池
- 每一层解耦的驱动力是什么？（制程选择自由度 / IP 复用 / 供应链分权 / 良率 / 功耗分区）
- 解耦后新的瓶颈在哪？（chip-to-chip 接口带宽 / 延迟 / 功耗 overhead）

### 原则 3：多供应商博弈双维度评估
> 评估 Broadcom / Marvell / MediaTek / Intel 等供应商动态时，同时打分两张表。

| 维度 | 评估项 |
|---|---|
| **技术 IP 优势** | SerDes 速率 / 功耗 (pJ/bit) / 信号完整性 / DSP 算法 / 量产成熟度 |
| **供应链地缘政治** | 多源策略 / 制程锁定 / 客户冲突 / 地缘风险 / 第二供应商价值 |

## 何时启用

用户说出以下关键词时启用：

- **芯片互联类**："超级芯片"、"NVLink"、"NVLink Fusion"、"UALink"、"CXL"、"芯片互联"、"ICI"、"超节点互联"
- **物理层类**："SerDes"、"PAM4"、"224G"、"448G"、"硅光子"、"CPO"、"光电共封装"、"光子织物"、"Photonic Fabric"
- **封装类**："CoWoS"、"EMIB"、"Chiplet"、"2.5D 封装"、"3D 封装"、"异构集成"、"先进封装"
- **内存类**："内存控制器"、"HBM"、"CXL 内存"、"内存池化"
- **供应商类**："Nvidia Marvell"、"Google Marvell"、"Broadcom ASIC"、"Celestial AI"、"Marvell 分析"
- **对比分析类**："对比 Nvidia 和 Google 的互联策略"、"Marvell 在两家之间的角色"

## 工作流（强制顺序）

### 阶段 1：确认分析范围

如果用户没指定，用 AskUserQuestion 问三件事：

| 问题 | 默认选项 |
|---|---|
| 分析哪个/哪些厂商？ | Nvidia + Google + Marvell（三角关系） |
| 聚焦哪个技术层？ | 全栈（互连 + 封装 + PHY + 供应链） |
| 输出深度？ | 深度报告（~500 行，含量化数字 + 源码/专利/新闻取证） |

### 阶段 2：拉取信源（Tier 分级）

按以下分级拉取信源，**至少覆盖 T0+T1**：

| Tier | 信源 | 用途 |
|---|---|---|
| **T0 一手原文** | 官方新闻稿（Marvell/Google/Nvidia 官网）、SEC/FERC filing（投资金额/条款）、专利数据库（USPTO SerDes/硅光子专利）、GitHub（开源互联 IP）、IEEE Xplore（HOTI/Micro 论文） | 不可否认的事实基础 |
| **T1 独立深度** | SemiAnalysis、Next Platform、Ian Cutress/AnandTech、ServeTheHome、Semiconductor Engineering | 独立技术判断 |
| **T2 供应链** | DigiTimes、TrendForce、Wccftech（交叉验证用）、供应链泄漏路线图 | 制程/封装路线图 |
| **T3 社区** | r/hardware、Hacker News、X 上 Dylan Patel/@chiakokhua 等 | 群体验证 |

### 阶段 3：按模板填充

读 `prompts/superchip_interconnect.md` 拿主分析 prompt，读 `templates/analysis_report_template.md` 拿报告模板。

### 阶段 4：输出报告

- 默认写到 `./output/<topic>_<YYYYMMDD>.md`
- 如涉及多厂商对比，追加 `prompts/cross_vendor_synthesis.md` 做横向主线

## 关键约束（必须遵守）

1. **物理层优先**：任何互联分析必须先论证物理可实现性，再谈协议/生态
2. **数字必须带来源**：每个量化数字（带宽、功耗、距离、金额）必须注明出处（URL + 日期）
3. **解耦四层框架**：每个架构分析都要按"计算 / 通信 / I/O / 内存"四层分解
4. **双维度评分**：每个供应商变动都要从"技术 IP + 供应链地缘"两个维度打分
5. **页码/段落引用**：引用专利/论文/新闻时必须注明具体位置
6. **不要做新闻复述**：每条信息要带"技术本质 / 战略影响 / 后续推演"三段式判断
7. **避免单一信源**：重大判断 ≥ 2 个独立信源，至少 1 个 T0/T1
8. **保留技术术语原文**：SerDes、PAM4、CoWoS 等保留英文，中文语境下首次出现时加括号注释

## 核心分析框架（三段式判断法）

每条技术信号或合作动态，按以下三段输出：

```
1. 技术本质：这个变化在物理层/协议层/架构层改变了什么？
   → 例：TPU v8e ICI 解耦 = 通信逻辑从计算 die 剥离成独立网络 ASIC，
      获取独立的制程选择自由度（Intel 18A vs TSMC）和 SerDes IP 复用

2. 战略影响：谁会立即受益？谁受冲击？
   → 例：Marvell 获得 Google 网络 ASIC 订单 →
      受益：Marvell（验证 224G SerDes 在 Intel 18A 上的量产能力）、
            Intel Foundry（获得 AI 芯片代工标杆客户）
      受冲击：Broadcom（失去 Google TPU 网络部分的独占地位）

3. 后续推演：6-24 个月内的二级/三级效应
   → 例：Google 多供应商策略验证成功后 →
      Amazon/Meta 可能跟进 → Marvell 成为 ASIC 互联层"瑞士军刀" →
      倒逼 Broadcom 降价/开放更多 SerDes IP
```

## 文件结构

```
google-marvell-nvidia-skill/
├── README.md                                   # 仓库总览
├── SKILL.md                                    # 主入口 + 五大支柱 + 方法论
├── prompts/
│   ├── superchip_interconnect.md               # 超级芯片互联主分析 prompt
│   ├── vendor_dynamics.md                      # 供应商动态与供应链分析 prompt
│   ├── physical_layer.md                       # 物理层/SerDes/硅光子 深度分析 prompt
│   ├── memory_controller.md                    # 内存控制器/CXL池化/近存计算 深度分析 prompt
│   └── cross_vendor_synthesis.md               # 跨厂商横向主线提炼 prompt
├── templates/
│   ├── analysis_report_template.md             # 深度分析报告模板（10 节）
│   └── vendor_profile_template.md              # 单厂商深度档案模板
├── examples/
│   ├── nvidia_marvell_google_triangle.md        # Nvidia-Marvell-Google 三角关系分析
│   ├── broadcom_profile.md                      # Broadcom 厂商深度档案
│   ├── ualink_vs_nvlink_fusion.md               # UALink vs NVLink Fusion vs 以太网
│   └── cxl_memory_pooling_kv_cache.md           # CXL 内存池化与 KV Cache 分析
└── scripts/
    └── patent_search.py                        # USPTO 专利检索辅助脚本（预留）
```

## 使用示例

```
用户：分析 Nvidia 投 Marvell 20 亿美元的战略意图
→ 读 prompts/vendor_dynamics.md
→ WebSearch "NVIDIA Marvell $2 billion NVLink Fusion silicon photonics"
→ WebSearch "Celestial AI Marvell acquisition photonic fabric"
→ WebSearch "NVLink Fusion open chiplet interface"
→ 按三段式判断法输出分析
→ 写文件到 ./output/nvidia_marvell_20b_20260619.md
```

```
用户：对比 Google TPU v8e 和 Nvidia GB200 的互联架构
→ 读 prompts/superchip_interconnect.md
→ 并行拉取两边信源（T0+T1）
→ 按四层分解框架（计算/通信/I/O/内存）逐层对比
→ 读 prompts/cross_vendor_synthesis.md 做横向主线
→ 写文件到 ./output/tpu_v8e_vs_gb200_interconnect_20260619.md
```

```
用户：分析 Marvell Structera 家族在大模型推理中的价值
→ 读 prompts/memory_controller.md
→ 先量化 KV Cache 容量缺口（模型 × 上下文 × Batch Size）
→ 拆解 Structera A/X/S 各自解决什么问题
→ 对比 Astera Labs / Samsung / SK Hynix 的 CXL 方案
→ 写文件到 ./output/marvell_structera_analysis_20260619.md
```

```
用户：CXL 内存池化能解决大模型推理的什么瓶颈？
→ 读 prompts/memory_controller.md
→ 算一个具体场景的 KV Cache 需求 vs HBM 容量
→ 量化 CXL 池化前后的吞吐/TTFT/TCO 对比
→ 写文件到 ./output/cxl_pooling_kv_cache_20260619.md
```

## 已知陷阱

- **"NVLink Fusion 是开放标准"** — 要区分"开放给合作伙伴"和"开放标准"。Fusion 目前是 Nvidia 控制的 Chiplet 接口规范，不是 IEEE/行业标准。
- **"硅光子已量产"** — 要区分"CPO 在交换机量产"和"硅光子 Chiplet 在计算芯片量产"。后者（Celestial AI 路线）仍在早期。
- **"XX 公司赢了"** — AI 互联没有单一赢家。铜/光、电/光、SerDes/硅光 是互补技术栈，不同距离/带宽场景有不同最优解。
- **"工艺节点 = 性能"** — 高速 SerDes 对工艺的模拟/混合信号特性要求远高于数字逻辑。Intel 18A 能否胜任 224G SerDes 量产，是 TPU v8e 网络 ASIC 的最大技术风险。
- **不要忽视"安静的大事"** — 有时最重要的信号是"Broadcom 在某客户处丢单"、"某 SerDes IP 供应商被收购"、"某标准组织投票结果"。显式扫描 absent signal。
