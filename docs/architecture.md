# Architecture — decision first, minimum moving parts

The product has three user-facing jobs:

1. **Evaluate**: decide whether a new or existing VPS is worth keeping for the user's ISP, applications, budget and deadline.
2. **Deploy**: install or integrate the smallest stable, verifiable proxy path on an accepted VPS.
3. **Repair**: diagnose and restore the failing path without disturbing healthy paths.

Entry: [AGENTS.md](../AGENTS.md). Workflows: [Evaluate](../workflows/evaluate.md), [Deploy](../workflows/deploy.md), [Repair](../workflows/repair.md).

## Runtime concept

~~~text
User intent
    → choose one task workflow
    → discover actually accessible native shell / OpenSSH / service APIs
    → collect the minimum decisive evidence
    → decide / propose authorized change
    → verify result and stop
~~~

No separate VPS-side AI, custom SSH protocol, mandatory Cloudflare MCP, mandatory WARP, universal benchmark, persistent local profile or new runtime dependency is needed for these documentation-first workflows. Use OpenSSH's existing config and credential storage. A future helper should be justified by an actual repeated failure or measurable reduction in manual work.

## Existing Skills are specialist references, not eight mandatory steps

The original eight Skills remain available and their content is preserved for agents with Skill integration. Select only those relevant to the user's task: VPS path, OS baseline, 3X-UI/Xray, protocol experiment, special exit, client rules, and final acceptance. The lifecycle Skill remains a fallback for an unknown topology, not a requirement for every narrow fault.

A reference can guide an Agent but does not grant execution permissions, make a tool installed, or prove that any deployment has run.

## Evidence and recovery

- User goals and explicit authorization define the requested outcome and allowed mutations.
- SSH/config/API access identifies what the current Agent can read, not what it may change.
- For 3X-UI, preserve persistent → generated → Xray runtime → subscription projection distinctions.
- Current observed traffic is stronger than a historical success claim when assessing current function; measure it at the correct origin.
- Collect only tests that can affect a keep/refund, deployment or repair decision. Stop once evidence is sufficient.
- Confirm authorization, backup and independent rollback before production mutations affecting connectivity.

See [evidence hierarchy](methodology/evidence-hierarchy.md), [rollback](methodology/rollback.md), [secret handling](methodology/secret-handling.md) and [legacy lifecycle](lifecycle.md).

## Verified scope vs intent

This repository presently ships knowledge and workflows with a static repository checker and text-based routing/boundary cases. It does **not** bundle a general autonomous installer, a portable Agent activation guarantee, or a remotely executed production acceptance suite. Statements of deployment or repair success require tool-derived evidence from that specific environment.
