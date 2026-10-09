# Agent guide — VPS Proxy Ops

Purpose: help a person **decide whether to keep a VPS**, **deploy the smallest working proxy**, or **restore a broken setup**. Success means a useful decision or a verified result with minimal human effort, not completion of every checklist.

## Start here

Choose exactly one starting workflow from the user's intent:

| Intent | Entry |
| --- | --- |
| New VPS, route/ISP comparison, keep-or-refund decision | [Evaluate](workflows/evaluate.md) |
| Set up a working personal proxy on an accepted VPS | [Deploy](workflows/deploy.md) |
| Existing node, client, Cloudflare or egress failure | [Repair](workflows/repair.md) |

Do not force all eight legacy Skills or every lifecycle stage into a narrow task. The existing [Skills](skills/) and their references provide optional specialist knowledge; load only what is relevant. If any instructions conflict, keep the stricter authorization, secret-handling and rollback requirement. A conceptual question needs no environment probes.

## Execution rules

1. Discover capabilities that are **actually available** in the current Agent session. Use authorized local shell, OpenSSH, existing authenticated APIs/connectors, and local proxy-controller read access directly where possible.
2. Do not ask the user to copy command output that the Agent can retrieve. Ask only for a missing decision or access step; if the Agent is chat-only or cloud-based without access to the user's devices, say so and provide the smallest manual alternative.
3. Reuse the user's existing SSH alias, SSH agent and ~/.ssh/config. Never read, print, commit, request or transfer SSH private keys, API tokens, subscription credentials, or provider passwords. Never disable host-key checking to make an SSH connection work.
4. Start with non-disruptive, bounded evidence collection. Check whether the Agent's own connectivity depends on the proxy or VPS being diagnosed. Keep client-origin measurements distinct from probes originated on the VPS; avoid TUN/system-proxy contamination.
5. Do not run an unreviewed curl-to-shell installer, intrusive benchmark, large download, paid service, or outside upload by default. Check the tool's current source/version and resource/privacy costs; ask for permission when an installation, traffic-heavy test or external data submission is needed.
6. Persistent configuration changes, service restarts, route/DNS/firewall modifications and production node switching require an explicit, scoped approval after explaining impact, backup, independent rollback and validation. A previous general request to diagnose is not a write authorization.
7. Treat panel/database configuration, generated files, running services, client configuration and observed traffic as distinct evidence. A historical PASS is not current health. If the Agent cannot test a layer, report UNKNOWN.
8. Stop when enough evidence supports the user's decision or an agreed acceptance gate. Do not add protocols, WARP, CDN layers or tests merely because they exist.

## Shared references

- [VPS paths](skills/vps-region-fit/SKILL.md) and [Linux correctness](skills/vps-baseline/SKILL.md)
- [3X-UI/Xray state](skills/xui-xray-stack/SKILL.md) and [client rules](skills/proxy-client-governance/SKILL.md)
- [Shadow experiments](skills/proxy-transport-lab/SKILL.md) and [special egress](skills/special-egress-routing/SKILL.md)
- [Acceptance](skills/proxy-stack-acceptance/SKILL.md), [evidence hierarchy](docs/methodology/evidence-hierarchy.md), [rollback](docs/methodology/rollback.md), [secrets](docs/methodology/secret-handling.md)

## Response shape

Lead with the action-oriented result (KEEP / RETEST / REFUND_CANDIDATE; DEPLOYED / BLOCKED; FIXED / ISOLATED / UNKNOWN). Follow with decisive evidence, limitations, and the next smallest action. Do not invent a score or guarantee a refund. Never present example commands as executed operations.
