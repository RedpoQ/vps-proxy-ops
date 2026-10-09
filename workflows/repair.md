# Repair — restore the failing path

## Goal

Restore existing service quickly **without breaking the paths that still work**. Start from the symptom and shortest discriminating test, not a mandatory full lifecycle audit.

## 1. Triage before running commands

- What exactly fails, since when, on which client/network and protocol?
- Which nodes, transports or destinations still work?
- Can this Agent actually access local shell, SSH and relevant service APIs?
- Does the Agent's session or the user's only management access depend on the failing proxy?

If a live control path depends on the component to be changed, require independent recovery before a disruptive operation.

## 2. Make one hypothesis falsifiable

Read only the relevant layers:

| Symptom | First differentiating evidence |
| --- | --- |
| Only one transport fails | Client handshake / protocol transport vs other working inbound |
| Cloudflare-fronted entry fails | Same origin/transport tested with and without Cloudflare, keeping variables fixed |
| 3X-UI says healthy, node fails | Persistent state → generated state → Xray runtime/listeners → subscription/client |
| Browser fails but node works | Mihomo active profile, DNS, rule match, TUN/system proxy, observed connection |
| AI-only/static exit fails | Routing rule, special outbound health, real exit, ordinary-route negative control |
| VPS itself unreachable | Local-network path vs provider console/SSH, route, DNS and host correctness |

Do not infer current fault from historical tests. A successful HTTP request is not a protocol proof, and an HTTP 403 after successful transport is not automatically a transport failure.

## 3. Collect and compare safely

Use native shell/SSH and authenticated read APIs, if actually available. Redact credentials, subscription URLs, full configs and public host details in shared reports.

Only add extra tools/tests after a bounded first probe leaves a meaningful ambiguity. A remote script, sustained benchmark, production fail-closed sabotage or unapproved node switch is not a diagnostic shortcut.

For experiments use independent configuration, temporary processes/ports and safe cleanup where supported. See [transport lab](../skills/proxy-transport-lab/SKILL.md), [source of truth](../skills/xui-xray-stack/references/source-of-truth.md), and [route isolation](../skills/special-egress-routing/references/route-isolation.md).

## 4. Repair only the demonstrated fault

Present root-cause confidence, exact proposed change, impact, baseline, backup, independent rollback and verification; obtain explicit approval for production changes. If root cause cannot be demonstrated, say UNKNOWN and propose the next discriminating test.

After an authorized change, recheck the failing path plus at least one previously working control path. Roll back when the intended acceptance fails.

~~~text
STATUS=FIXED|ISOLATED|UNKNOWN|BLOCKED
WORKING_CONTROLS=
FAILING_LAYER_AND_EVIDENCE=
CAUSE_CONFIDENCE=
PRODUCTION_CHANGE=NONE|PROPOSED|AUTHORIZED_AND_VERIFIED
NEXT_SMALLEST_ACTION=
~~~
