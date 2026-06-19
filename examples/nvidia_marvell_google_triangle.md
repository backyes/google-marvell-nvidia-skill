# 示例分析报告：Nvidia-Marvell-Google 三角关系深度分析

> 本报告是对 2026 年 Nvidia / Marvell / Google 三方在 AI 超级芯片互联与硅光子领域合作的综合深度分析。
> 分析日期：2026-06-19 · 覆盖周期：2026 Q1-Q2

---

## 0. 元数据

| 字段 | 内容 |
|---|---|
| **分析主题** | Nvidia-Marvell-Google 三方在 AI 超级芯片互联与硅光子领域的战略三角关系 |
| **涉及厂商** | Nvidia、Google、Marvell、Broadcom、Intel Foundry、MediaTek、Celestial AI |
| **技术层级** | 综合（互连 + 封装 + PHY + 供应链） |
| **关键事件** | 1. Nvidia 向 Marvell 注资 $2B（2026.03）<br>2. Marvell 完成收购 Celestial AI $3.25B（2026.02）<br>3. Google TPU v8e 网络 ASIC 交 Marvell 设计，Intel 18A 工艺（2026.06） |
| **分析日期** | 2026-06-19 |

---

## 1. Executive Summary

### 今日最重要变化

Marvell 通过三件事（Celestial AI 收购 + Nvidia $2B 注资 + Google TPU v8e 网络 ASIC 订单）在 2026 上半年完成了从 "Broadcom 替代品" 到 "AI 互联物理层核心枢纽" 的战略跃迁。黄仁勋在 Computex 2026 亲自站台称其为 "下一家万亿美金市值公司"，这不仅是背书，更是对 "互连物理层 = AI 基础设施咽喉" 的判断。

### 方向

AI 基础设施的瓶颈从 "单芯片算力" 转向 "通信带宽与物理层功耗"，铜线在 224G PAM4 撞墙，硅光子从交换机向计算芯片渗透。

### 核心矛盾

**带宽-距离-功耗 不可能三角**：224G PAM4 铜线有效距离 ~1m，跨机柜 Scale-up 必须走光。谁掌握 224G+ 光互联的 DSP/SerDes/硅光子 IP，谁就扼住了下一代 AI 基础设施的咽喉。

### Top 5 技术信号

| # | 信号 | 涉及厂商 | 技术本质 | 战略影响 | 后续推演 |
|---|---|---|---|---|---|
| 1 | Nvidia 投 Marvell $2B | Nvidia, Marvell | NVLink Fusion 硅光子合作 | 开放 NVLink 生态，锁定第三方 ASIC | 第三方 ASIC 接入 Nvidia 机柜生态的入口 |
| 2 | Marvell 收购 Celestial AI $3.25B | Marvell, Celestial AI | 获取 3D 光子织物技术 | 获得硅光子 Chiplet 核心 IP | 光 Chiplet 成为 ASIC 互连标配的路线图加速 |
| 3 | Google TPU v8e 网络 ASIC → Marvell | Google, Marvell, Intel | ICI 从计算 die 解耦成独立网络芯片 | Google 供应链 "去 Broadcom 化" 里程碑 | 网络 ASIC 独立化成为行业模板 |
| 4 | Intel 18A 首次 AI 芯片应用 | Intel Foundry, Google, Marvell | 224G SerDes 在 Intel 18A 上量产验证 | 打破 TSMC 先进封装垄断 | 成败决定 Intel Foundry 在 AI 领域的未来 |
| 5 | MediaTek 加入 TPU v8e I/O 后端 | Google, MediaTek | Google "分而治之" 供应商策略 | 计算(Broadcom)+网络(Marvell)+I/O(MediaTek) 三家分工 | 多供应商模板可复制到 Amazon/Meta |

---

## 2. 三角关系的技术本质

### 2.1 Nvidia × Marvell：总线为名，硅光为实

```
Nvidia 的战略逻辑：
┌─────────────────────────────────────────────────────┐
│  问题：自研 ASIC 浪潮（Google/Meta/Amazon）在挑战    │
│        GPU 垄断，第三方芯片用 UALink/以太网自成体系  │
│                                                     │
│  方案：NVLink Fusion — 把 NVLink 开放成 Chiplet 接口 │
│        "你的自研芯片可以用 NVLink，但必须接入我的    │
│         机柜/交换机/软件栈"                          │
│                                                     │
│  为什么选 Marvell：                                  │
│  1. ASIC 设计能力（可以帮第三方接入 NVLink Fusion）   │
│  2. Celestial AI 光子织物（光 Chiplet 是 NVLink     │
│     跨机柜延伸的物理基础）                           │
│  3. 不是 Broadcom（避免进一步加强对手）              │
│                                                     │
│  $2B 投资 = 战略绑定费 + 硅光子联合开发费            │
└─────────────────────────────────────────────────────┘
```

**技术本质**：NVLink Fusion 不是 "开放标准"，是 "受控开放" — 接口开放给合作伙伴，但物理层（交换机/光模块/机柜）和软件栈（CUDA/网络管理）仍然由 Nvidia 控制。Marvell 的角色是帮 Nvidia 把这种 "受控开放" 从 GPU 扩展到第三方 ASIC。

### 2.2 Google × Marvell：解耦 ICI，获取 SerDes IP

```
Google 的战略逻辑：
┌─────────────────────────────────────────────────────┐
│  问题：TPU 全栈依赖 Broadcom（计算 die + 网络 + I/O） │
│        → 单一供应商风险 + 缺乏议价能力                │
│                                                     │
│  方案：TPU v8e "分而治之" 拆单                        │
│  ・ 计算 die → Broadcom（保留，核心算力）             │
│  ・ 网络 ASIC → Marvell（独立芯片，Intel 18A）       │
│  ・ I/O 后端 → MediaTek（低功耗集成）                 │
│                                                     │
│  为什么网络 ASIC 选 Marvell：                         │
│  1. 224G SerDes IP（核心需求）                       │
│  2. 不是 Broadcom（供应链分权）                       │
│  3. 可以同时服务 Nvidia 和 Google（已验证的冲突管理） │
│                                                     │
│  为什么用 Intel 18A：                                 │
│  1. 制程选择自由（脱离 TSMC 单一依赖）                │
│  2. EMIB 封装（与 CoWoS 竞争的第二方案）              │
│  3. Intel 需要标杆客户，可能给了更好的商业条件        │
└─────────────────────────────────────────────────────┘
```

**技术本质**：TPU v8e 的 ICI 从片上 IP 变成独立网络 ASIC，是范式级变化。这标志着 "通信子系统独立化" — 网络芯片不再被束缚在计算 die 的制程节点上，可以独立选择最优工艺和 SerDes IP。

### 2.3 三角关系的物理层主线：硅光子

```
        Celestial AI 光子织物 (3D 垂直光路由)
        ↓ Marvell 收购 ($3.25B)
        ↓
   Marvell 硅光子 IP 池
        ↓
   ┌────┴────┐
   ↓         ↓
Nvidia      Google
(光 Chiplet (TPU 外部
 用于 NVLink 光互联，
 Fusion)    网络 ASIC
            驱动 OCS)
```

**技术本质**：Nvidia 和 Google 都在押注同一个物理层方向 — 硅光子从交换机渗透到计算芯片。不同之处在于：Nvidia 走 "光 Chiplet 直连计算 die"（Celestial AI 路线），Google 走 "网络 ASIC 驱动外部 OCS"（光电路交换机路线）。两条路线最终可能收敛。

---

## 3. 物理层深度分析：224G PAM4 铜线撞墙

### 3.1 铜线物理极限

| 速率 | 调制 | Nyquist 频率 | 铜线有效距离 (DAC) | 插入损耗 @ Nyquist | 是否可行 |
|---|---|---|---|---|---|
| 112G PAM4 | PAM4 | 28 GHz | ~3m | ~15 dB | 成熟量产 |
| 224G PAM4 | PAM4 | 56 GHz | ~1m | ~25 dB | 当前主流 |
| 448G PAM4 | PAM4 | 112 GHz | ~0.3m (推测) | ~35 dB+ | 几乎不可行 |

**关键判断**：224G PAM4 是铜线最后一个舒适的节点。在 448G，铜线距离缩短到 ~0.3m，连机柜内互联都难以覆盖。这意味着 **光互联从 "nice-to-have" 变成 "must-have" 的拐点就在 2027-2028**。

### 3.2 SerDes 功耗分析

假设一个 100-lane 224G SerDes 配置：

| SerDes 功耗 (pJ/bit) | 100-lane 总功耗 (W) | 占 1000W TDP 比例 | 是否可行 |
|---|---|---|---|
| 10 (当前典型) | 224W | 22.4% | 勉强 |
| 7 (先进) | 157W | 15.7% | 可行 |
| 5 (目标) | 112W | 11.2% | 理想 |
| 3 (理论极限) | 67W | 6.7% | 非常难 |

**关键判断**：SerDes 功耗是互联扩展的硬约束。如果 224G SerDes 功耗不能降到 7 pJ/bit 以下，100+ lane 的配置会吃掉整个芯片 20%+ 的功耗预算。光互联在 224G 以上开始有功耗竞争力，因为光纤本身的传输损耗远低于铜线（虽然激光器和调制器有固定功耗开销）。

---

## 4. 供应商双维度评分

| 供应商 | 技术 IP 优势 (1-5) | 评分理由 | 供应链地缘政治 (1-5) | 评分理由 | 综合判断 |
|---|---|---|---|---|---|
| **Marvell** | 4 | 224G SerDes + 光子织物 IP + ASIC 设计能力，但 448G 尚未验证 | 5 | "Not Broadcom" 战略价值极高，同时服务 Nvidia 和 Google，制程多源（TSMC + Intel） | **核心枢纽** |
| **Broadcom** | 5 | 224G SerDes 性能最强，AI ASIC 客户最多，Tomahawk/Jericho 交换机垄断 | 2 | 垄断地位让客户焦虑，地缘政治风险（台湾/中国），客户冲突（服务直接竞争对手） | **被挑战的王者** |
| **MediaTek** | 3 | 低功耗 I/O 集成强，但 SerDes 不是核心优势 | 4 | 台湾供应链，政治风险中等，但作为 "第三选项" 价值高 | **I/O 层补充** |
| **Intel Foundry** | 3 | 18A 工艺潜力大，EMIB 封装有竞争力，但模拟/混合信号性能未验证 | 4 | 美国本土制造，地缘政治加分，但需要客户验证 | **高风险高回报** |
| **TSMC** | 5 | 先进制程 + CoWoS 封装垄断，良率最高 | 3 | 台湾地缘政治风险是最大隐忧，客户在积极寻找第二来源 | **垄断但脆弱** |

---

## 5. 6-24 个月路线图推演

| 时间窗 | 预期里程碑 | 风险因素 |
|---|---|---|
| **T+6 个月 (2026 Q3-Q4)** | Marvell 完成 Celestial AI 技术整合，首个 Photonic Fabric 原型 | 硅光子热稳定性验证、Intel 18A SerDes 测试芯片回片 |
| **T+12 个月 (2027 Q1-Q2)** | Google TPU v8e 网络 ASIC 流片（Intel 18A），NVLink Fusion 首个第三方 ASIC 客户公布 | Intel 18A 良率、Marvell 224G SerDes 在 18A 上的性能 |
| **T+24 个月 (2028)** | 光 Chiplet 首次在计算芯片上量产（Nvidia Rubin? Google TPU v9?），448G SerDes 预研 | 硅光子量产良率、448G 铜线物理极限确认、行业标准竞争 |

---

## 6. 关键风险

### 技术风险
1. **Intel 18A 模拟/混合信号性能未验证**：高速 SerDes 对工艺的模拟特性要求极高，Intel 18A 是否胜任 224G SerDes 量产是 TPU v8e 网络 ASIC 最大的单一技术风险。
2. **硅光子热稳定性**：Celestial AI 声称通过材料创新解决了热稳定性问题，但量产环境下的温度波动远比实验室严格。
3. **448G PAM4 铜线物理极限**：如果 448G 铜线距离真的短到 ~0.3m，跨机柜光互联成为必选项，而光方案的功耗和成本仍然是瓶颈。

### 供应链风险
1. **Marvell 的客户冲突**：同时服务 Nvidia 和 Google，两家在 AI 芯片上是直接竞争对手。如何管理信息隔离和 IP 归属是核心挑战。
2. **Intel Foundry 的交付能力**：Intel 18A 的量产时间表多次推迟，Google TPU v8e 的时间窗口是否与 18A 量产节奏匹配是未知数。
3. **台海地缘政治**：TSMC 的不可替代性在下降（CoWoS → EMIB 分流，TSMC → Intel 18A 分流），但短期内仍是 AI 芯片制造的核心瓶颈。

### 生态风险
1. **NVLink Fusion 的 "开放" 程度**：如果第三方 ASIC 接入 NVLink Fusion 后过度依赖 Nvidia 软件栈，实际上是换了一种形式的锁定。
2. **标准战争**：UALink vs NVLink Fusion vs 以太网在 Chiplet 互连层的竞争尚未分出胜负，选边有风险。

---

## 7. 一句话点评

> **Marvell 在 2026 年同时被 Nvidia 和 Google 寄予厚望，不是因为它比 Broadcom 技术更强（目前还不是），而是因为它是唯一一个同时拥有 224G SerDes + 硅光子 IP + "Not Broadcom" 战略价值的公司 — 在 AI 基础设施的 "互连物理层 = 咽喉" 时代，这个组合的价值超过了单纯的技术领先。**

---

## 附录 A：信源清单

| # | Tier | 来源 | 标题/摘要 | URL | 日期 |
|---|---|---|---|---|---|
| 1 | T0 | Marvell 官方 | Marvell Completes Acquisition of Celestial AI | https://www.marvell.com/company/newsroom/marvell-completes-acquisition-of-celestial-ai.html | 2026-02-02 |
| 2 | T1 | optics.org | NVIDIA invests $2 billion in Marvell Technology in silicon photonics partnership | https://optics.org/news/nvidia-invests-2-b-in-marvell-technology-in-new-partnership | 2026-04-02 |
| 3 | T2 | Pulse 2.0 | NVIDIA: $2 Billion Investment In Marvell To Expand AI Infrastructure Partnership | https://pulse2.com/nvidia-2-billion-investment-in-semiconductor-company-marvell-to-expand-ai-infrastructure-partnership/ | 2026-03-31 |
| 4 | T2 | Wccftech | Marvell Scores Another Win, As Google Hands It A Custom Networking Chip For The TPUv8e On Intel's 18A/18AP Process | https://wccftech.com/marvell-scores-another-win-after-nvidia-ceos-trillion-dollar-praise-as-google-hands-it-a-custom-networking-chip-for-the-tpuv8e-on-intels-18a-18ap-process/ | 2026-06-03 |
| 5 | T2 | Photonics Spectra | Marvell to Acquire Celestial AI for $3.3B | https://www.photonics.com/Articles/Marvell-to-Acquire-Celestial-AI-for-33B/a71734 | 2026-02 |

## 附录 B：后续追踪清单

- [ ] Intel 18A SerDes 测试芯片回片结果（预期 2026 Q3-Q4）
- [ ] Marvell Celestial AI 整合后的首个 Photonic Fabric 原型 demo
- [ ] NVLink Fusion 首个第三方 ASIC 客户公布
- [ ] Google TPU v8e 网络 ASIC 流片消息
- [ ] Broadcom 是否降价或发布更先进 SerDes 以应对 Marvell 竞争
- [ ] Amazon/Meta 是否跟进 Google 的多供应商 "分而治之" 策略

---

*生成时间：2026-06-19 · 分析框架：Superchip Analyzer v1.0*
