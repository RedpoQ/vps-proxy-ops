# VPS Proxy Ops — evaluate, deploy and repair with an Agent

**Decide whether to keep a VPS first. Deploy one stable proxy path only if it is worth keeping. Diagnose failures without breaking working nodes.**

This is a lightweight, decision-first workflow for personal self-hosted proxy setups. It reuses tools the Agent actually has: local shell, existing OpenSSH, 3X-UI/Xray and Clash Verge/Mihomo. It does not introduce another benchmark engine, mandatory WARP, or an eight-stage checklist that every task must follow.

## Pick one job

| User intent | Workflow | Result |
| --- | --- | --- |
| New VPS, route/ISP quality, refund deadline | [Evaluate](workflows/evaluate.md) | KEEP / RETEST / REFUND_CANDIDATE with evidence |
| Accepted VPS, need a working personal proxy | [Deploy](workflows/deploy.md) | One validated working path, or a concrete blocker |
| Broken protocol, Xray, Cloudflare or client route | [Repair](workflows/repair.md) | Fault isolation, authorized change, verified recovery |

## Quick start

~~~bash
git clone https://github.com/RedpoQ/vps-proxy-ops.git
cd vps-proxy-ops
~~~

Open the repository in an Agent with **actual shell/SSH access**. Clients that recognize repository-level [AGENTS.md](AGENTS.md) can follow its guidance; other clients must be instructed to read that file explicitly. Portable Plugin installation and autonomous Skill activation have not been verified across clients.

Example user prompt:

> My VPS is new and my SSH alias is ready. Follow AGENTS.md and workflows/evaluate.md. Use the tools you actually have to inspect the VPS and my real ISP path without making changes. Decide KEEP, RETEST or REFUND_CANDIDATE, noting the refund deadline and missing evidence. Do not ask me to paste output you can retrieve yourself.

For failures:

> Other nodes still work, but XHTTP behind Cloudflare fails. Follow workflows/repair.md. Compare the failing path with a working control; do not change production without approval.

The Agent should reuse the user's SSH configuration/agent, **not copy or request private keys**. A cloud-only chat without local access cannot discover your computer's SSH credentials automatically.

## Method: minimal evidence, decisive result

Evaluate actual client-to-VPS performance separately from VPS-to-target performance. Look at the user's ISP, application goals, plan cost and refund terms before selecting tests. Reuse installed native tools and, where relevant, mtr, NextTrace, BackTrace, iperf3, ECS or YABS. These are **not bundled tools**; installers, heavy benchmarks and third-party uploads need appropriate review and approval.

A good result may stop after a hard requirement failure. If the important peak-hour or real protocol test has not been run, report RETEST, not a fictitious performance verdict.

For deployment prefer one verified inbound through Xray with VPS Direct egress. Additional inbound protocols, WARP or fixed SOCKS5 are optional responses to demonstrated requirements. The current documented control-plane reference is 3X-UI + Xray; sing-box is a separate core, not another name for 3X-UI.

## Existing specialist Skills

All eight original Skills remain available as optional technical references: [lifecycle](skills/proxy-stack-lifecycle/SKILL.md), [region/ISP](skills/vps-region-fit/SKILL.md), [Linux baseline](skills/vps-baseline/SKILL.md), [3X-UI/Xray](skills/xui-xray-stack/SKILL.md), [transport experiments](skills/proxy-transport-lab/SKILL.md), [special egress](skills/special-egress-routing/SKILL.md), [client routing](skills/proxy-client-governance/SKILL.md), and [acceptance](skills/proxy-stack-acceptance/SKILL.md). Load only what the current task requires.

## Production boundaries and scope

- Read-only, bounded evidence collection first. Explain changes, obtain scoped approval, preserve backups and an independent rollback before touching production routes, DNS, firewall, proxies or services.
- The documented Windows client, Linux VPS and 3X-UI/Xray workflows have environment-specific evidence, **not** universal platform coverage. Linux desktop integration is partial; macOS/OpenWrt automated mutation is not validated.
- The repository ships documentation, Skills and a static checker, **not** a verified autonomous installer, Cloudflare MCP, local SSH credential store, or universal production repair tool.
- Native Agent tools execute work; docs cannot magically grant access. Never expose provider passwords, API tokens, private keys or subscriptions.

Run the static checker from the repository root:

~~~bash
python tests/check_repository.py
~~~

It checks repository structure and limited secret patterns; it does not run real Agent activation or VPS tests.

See [AGENTS.md](AGENTS.md), [architecture](docs/architecture.md), [security](SECURITY.md), [Chinese README](README.md) and [license](LICENSE).
