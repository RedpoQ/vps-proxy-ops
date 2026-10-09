# VPS Proxy Ops — 用 Agent 判断 VPS 去留、快速部署、修复故障

**买了 VPS，先判断值不值得留；值得留，再部署最简单的稳定代理；出了故障，让 Agent 尽量自己检查并恢复。**

面向个人自建代理的轻量 Agent 工作流，复用本地 Shell、现成 SSH 连接、3X-UI / Xray 与 Clash Verge / Mihomo。项目重点是**决策和实际验证**，而不是新的 VPS 跑分软件、节点大全或八阶段必走流程。

## 从你的问题开始

| 你想解决什么 | Agent 工作流 | 最终得到 |
| --- | --- | --- |
| 刚买 VPS，担心线路差、晚高峰不稳、退款来不及 | [Evaluate：VPS 去留](workflows/evaluate.md) | KEEP / RETEST / REFUND_CANDIDATE，附决定性证据 |
| VPS 值得留下，想尽快有一条稳定节点 | [Deploy：最小部署](workflows/deploy.md) | 已验证的最小工作路径，或明确阻塞项 |
| XHTTP、HY2、Cloudflare、面板或客户端突然异常 | [Repair：故障恢复](workflows/repair.md) | 故障层、证据、经授权的修复与复测 |

已有线路、运营商、协议、Shadow Test 和出口分流经验都保留在 [8 个专门 Skills](skills/) 中，**需要哪部分才读哪部分**，不必每次执行完整生命周期。

## 快速开始：让 Agent 尽量自己做

~~~bash
git clone https://github.com/RedpoQ/vps-proxy-ops.git
cd vps-proxy-ops
~~~

在**能使用本机 Shell/SSH 的 Agent** 中打开这个仓库。对于能够识别仓库级 AGENTS.md 的客户端，会读取 [AGENTS.md](AGENTS.md) 中的执行原则；其他 Agent 请先显式要求它读取该文件。不同 Agent 的自动读取/Plugin 安装行为尚未逐一验证。

例如直接提出：

> 我刚买了一台 VPS，SSH 别名已经配置好。请根据 AGENTS.md 和 workflows/evaluate.md，先只读检查我的实际运营商线路、UDP、使用需求与退款时间，判断值得保留还是需要复测。能通过工具执行的检查请自己执行，不要让我反复复制输出。

或：

> 其他节点正常，只有 XHTTP 经过 Cloudflare 不通。请按 workflows/repair.md 进行最小差异诊断，不要修改现有节点。

**本地密钥怎么用？** Agent 优先复用已有 ~/.ssh/config 和 ssh-agent，通过 OpenSSH 调用主机别名；私钥和 API Token 不放入本项目、聊天或公开仓库。若 Agent 在云端且无权访问你的本机，无法凭文档自动连接 VPS，必须先建立实际授权通道。

## 如何判断 VPS 去留？

**不为跑分而跑分。** 先核对真实 ISP、应用需求、价格与退款期限，然后按成本从低到高收集决定性证据：

1. 硬条件：VPS 权限、价格、流量、运营商政策、地址和 UDP 需求。
2. 用户真实网络 → VPS 的延迟、丢包、路由与协议可达性；VPS → 目标网站另行测量。
3. 只有数据不足时，再使用已安装的 mtr、iperf3、NextTrace、BackTrace、ECS 或 YABS；安装、重负载测试或外部上传前确认影响与授权。
4. 依真实用途给出 KEEP、RETEST 或 REFUND_CANDIDATE。未测晚高峰时不能假装证明晚高峰稳定，也不能保证服务商退款。

示例（**格式示意，非实测**）：

~~~text
DECISION=RETEST
DECISIVE_FACTS=基础资源满足需求，日间 TCP 可达
UNVERIFIED_OR_CONTAMINATED=真实用户 ISP 的晚高峰与 HY2 握手尚未验证
NEXT_SMALLEST_ACTION=在退款期内从实际使用网络完成一次定向复测
~~~

## 技术选择：够用就停止

~~~text
客户端 → 单条经验证的入站协议 → VPS / Xray → Direct
                                           └→ 可选 WARP / 固定 SOCKS5
~~~

HY2、REALITY、TUIC、XHTTP 是按实际情况选择或比较的协议，不是必须全部安装。WARP 与固定 SOCKS5 是**可选出口**，不会自动让 IP 更干净；它们不解决所有入口协议故障。3X-UI 是 Xray 的管理面板，sing-box 是另一种内核，不能把两者当作同一层替代品。

部署与生产修复遵循：明确目标 → 当前基线 → 备份与独立回滚 → **用户批准具体变更** → 验证 → 停止。特别保护正在承载 Agent 连接的节点，不做破坏性 Fail-Closed 测试。

## 已有专业资料（按需阅读）

| 需要深入时 | 对应资料 |
| --- | --- |
| 状态不明、协调任务 | [proxy-stack-lifecycle](skills/proxy-stack-lifecycle/SKILL.md) |
| VPS 地区、ISP 与路由测量 | [vps-region-fit](skills/vps-region-fit/SKILL.md) |
| Linux 系统与 DNS | [vps-baseline](skills/vps-baseline/SKILL.md) |
| 3X-UI、Xray、订阅与运行时 | [xui-xray-stack](skills/xui-xray-stack/SKILL.md) |
| HY2 / REALITY / TUIC / XHTTP 隔离比较 | [proxy-transport-lab](skills/proxy-transport-lab/SKILL.md) |
| WARP / 静态 SOCKS5 等特殊出口决策 | [special-egress-routing](skills/special-egress-routing/SKILL.md) |
| Clash Verge / Mihomo 的 DNS、TUN、分流 | [proxy-client-governance](skills/proxy-client-governance/SKILL.md) |
| 真实端到端结果、停止调优 | [proxy-stack-acceptance](skills/proxy-stack-acceptance/SKILL.md) |

## 目前能做什么，尚不能保证什么？

- **已有：** 面向 Agent 的任务说明、8 个专业 Skills、静态仓库检查、合成案例、完整的证据与回滚方法。适用于 Agent 在实际拥有 Shell/SSH 等工具时，指导它执行授权操作。
- **尚未证明：** 任意 Agent 的自动安装和自动激活、全部平台的无人干预部署、真实 VPS 自动故障修复、跨 ISP 的普适协议性能排名。
- **支持范围：** 3X-UI / Xray、Windows Clash Verge/Mihomo 和 Linux VPS 有特定环境参考；Linux 客户端为部分参考，macOS 与 OpenWrt 自动化未做完整生产验证。
- **外部工具：** ECS、NextTrace、YABS、mtr 等**不是本项目内置的依赖**。安装和联网测速可能消耗流量、磁盘/CPU或向第三方发送测试信息。

运行现有只读静态检查：

~~~bash
python tests/check_repository.py
~~~

它检验仓库结构、相对链接及一些敏感信息模式，**不会**连接 VPS 或证明自动部署和 Skill 激活成功。

详见 [架构](docs/architecture.md)、[证据处理](docs/evidence-inventory.md)、[安全说明](SECURITY.md)、[贡献指南](CONTRIBUTING.md)。英文版：[README.en.md](README.en.md)。本项目当前仍为 v0.1 工作流项目，不应被理解为已发布的全自动运维软件。

[MIT License](LICENSE)。
