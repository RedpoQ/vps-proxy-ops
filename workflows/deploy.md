# Deploy — smallest stable working proxy

## Goal

On a VPS that the user has accepted, establish **one working, verifiable path**. Do not install a collection of protocols, WARP, CDN features or a static exit by default.

## 1. Determine the actual environment

- Confirm target host and the user's authorization; reuse existing OpenSSH config/agent without copying key material.
- Identify OS, current networking, existing services, ports, firewall/provider port mappings, control-plane ownership and rollback/recovery access.
- Detect whether the host already has 3X-UI / Xray, and distinguish persistent panel state, generated configuration, running Xray and client subscription.
- If no SSH access exists, guide a minimal public-key bootstrap. Only the public key goes to the VPS. Verify access **before** relying on it.
- This repository documents 3X-UI/Xray as its current control-plane reference; do not pretend there is a tested generic sing-box/other-panel deployment adapter.

## 2. Propose the minimum plan

Choose an inbound transport from tested needs, not a universal ranking. HY2 requires a working UDP path; TCP/REALITY can be an alternative if actual conditions justify it. Select a client-supported baseline and compatible TLS/authentication strategy.

Default path:

~~~text
Client → one inbound → VPS → Direct
~~~

WARP, fixed SOCKS5, multiple protocols, custom CDN routing and extra domains are optional branches that require a concrete user need, compatible upstream, and separate validation. Never assume a specific exit improves account standing or bypasses a service's country rules.

## 3. Change only with permission

Before any production write:

1. Identify the control-plane source of truth and the exact objects/ports/services to change.
2. Capture the current baseline and complete sensitive backups without publishing them.
3. Prepare rollback independent of the Agent's connection when changing its own route, proxy, DNS or access path.
4. State the exact proposed mutation, likely downtime, and ask for approval.
5. Use the actual installed product/version's supported interface; do not blindly overwrite a 3X-UI-generated Xray config with a standalone template.
6. Apply one main change, parse/check, confirm runtime/listeners and client projection, then run real client-origin end-to-end smoke tests.

If any prerequisite is missing, output BLOCKED with one concrete next step; do not invent deployment success.

## 4. Verify and stop

Verify protocol handshake, usable application traffic, intended exit, client routing/DNS as applicable, unchanged existing paths, and a workable rollback. Use [3X-UI/Xray state](../skills/xui-xray-stack/SKILL.md), [client governance](../skills/proxy-client-governance/SKILL.md), [Shadow Testing](../skills/proxy-transport-lab/SKILL.md), and [acceptance](../skills/proxy-stack-acceptance/SKILL.md) only as needed.

~~~text
DEPLOYMENT=DEPLOYED|BLOCKED|NOT_STARTED
WORKING_PATH=
EVIDENCE=
ROLLBACK_READY=
OPTIONAL_COMPONENTS_SKIPPED=
NEXT_ACTION=
~~~

A guide or planned command is not proof that deployment occurred.
