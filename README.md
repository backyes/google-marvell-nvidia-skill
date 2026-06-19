# Superchip Analyzer — Nvidia / Google / Marvell 超级芯片互联与内存控制器深度分析 Skill

> 一个面向 AI Infra 架构师、芯片设计团队、互连系统工程师和投研团队的 Claude Code Skill，
> 用于深度分析 Nvidia、Google、Marvell 在超级芯片互联、内存控制器、SerDes、硅光子、CXL 内存池化等领域的合作与竞争。

## 核心方法论

| 原则 | 说明 |
|---|---|
| **物理层约束优先** | 先问"铜线能跑多远？功耗预算是多少？"，再谈协议和生态 |
| **KV Cache 容量缺口量化** | 先算 `模型 × 上下文 × Batch Size` 的内存需求，再论证 CXL 池化价值 |
| **解耦与分解** | 按"计算 → 通信/路由 → I/O 后端 → 内存子系统"四层分解架构演进 |
| **双维度供应商评分** | 技术 IP 优势 + 供应链地缘政治 两个独立维度 |

## 领域覆盖（五大支柱）

1. **高速互连与协议栈** — NVLink 1.0–5.0 / NVLink Fusion / UALink / CXL 3.0+ / Google ICI
2. **物理层与 SerDes 工程** — 112G→224G→448G PAM4 / 铜线物理极限 / 光互联拐点
3. **先进封装与异构集成** — CoWoS / EMIB / 2.5D/3D Chiplet / CPO / 硅光子 / Celestial AI Photonic Fabric
4. **内存控制器与 CXL 池化** — KV Cache 内存墙 / Structera 家族 (A/X/S) / 近存计算 / MPU 解耦
5. **超大规模 ASIC 系统解构** — Google TPU v4–v8e / Amazon Trainium / Meta MTIA / Microsoft Maia

## 文件结构

```
├── SKILL.md                         # 主入口（方法论 + 触发词 + 工作流）
├── prompts/                         # 分析 prompt 库
│   ├── superchip_interconnect.md    # 超级芯片互联主分析
│   ├── vendor_dynamics.md           # 供应商动态与供应链
│   ├── physical_layer.md            # 物理层/SerDes/硅光子（含 Celestial AI ~130行深度）
│   ├── memory_controller.md         # 内存控制器/CXL池化/近存计算
│   └── cross_vendor_synthesis.md    # 跨厂商横向主线（8 条产业主线）
├── templates/                       # 输出模板
│   ├── analysis_report_template.md  # 10 节深度报告模板
│   └── vendor_profile_template.md   # 8 节厂商档案模板
├── examples/                        # 成品示例
│   ├── nvidia_marvell_google_triangle.md   # 三角关系综合分析
│   ├── broadcom_profile.md                 # Broadcom 厂商深度档案
│   ├── ualink_vs_nvlink_fusion.md          # 三大互联协议逐层对比
│   └── cxl_memory_pooling_kv_cache.md      # CXL 内存池化与 KV Cache 量化
└── scripts/
    └── patent_search.py             # USPTO 专利检索脚本（预留）
```

## 使用方式

将此仓库克隆到 `~/.claude/skills/superchip-analyzer/`，然后在 Claude Code 中直接提及以下关键词即可触发：

- **芯片互联**："NVLink Fusion"、"UALink"、"CXL"、"ICI"、"超节点互联"
- **物理层**："224G PAM4"、"硅光子"、"CPO"、"Photonic Fabric"、"Celestial AI"
- **内存**："CXL 内存池化"、"KV Cache"、"Structera"、"MPU"、"近存计算"
- **供应商**："Nvidia Marvell"、"Google Marvell"、"Broadcom ASIC"
- **对比**："对比 Nvidia 和 Google 的互联策略"

或直接说："用 superchip-analyzer 分析 XXX"。

## 示例输出

- [Nvidia-Marvell-Google 三角关系深度分析](examples/nvidia_marvell_google_triangle.md)
- [Broadcom 厂商深度档案](examples/broadcom_profile.md)
- [UALink vs NVLink Fusion vs AI 以太网](examples/ualink_vs_nvlink_fusion.md)
- [CXL 内存池化与 KV Cache 量化分析](examples/cxl_memory_pooling_kv_cache.md)

## 关键约束

- 每个量化数字必须带来源（URL + 日期）
- 重大判断 ≥ 2 个独立信源，至少 1 个 T0/T1
- 每条信号带"技术本质 / 战略影响 / 后续推演"三段式判断
- 物理层可实现性优先于营销命名
- 保留技术术语原文（SerDes、PAM4、CoWoS 等）

---

*仓库创建于 2026-06-19*
