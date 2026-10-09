# Evaluate — should this VPS stay?

## Goal

Help the user decide **KEEP**, **RETEST**, or **REFUND_CANDIDATE** as early as the evidence permits. The user usually values a stable connection for their real ISP and applications more than peak synthetic benchmark scores. A recommendation is not a provider's refund guarantee.

## Gather only decision-changing context

- Real client network / ISP and the applications that matter (AI, web, video, games, hosting).
- Actual plan price, renewal cost, traffic cap, and any refund/renewal deadline.
- Whether a working VPS and SSH access already exist; use an existing SSH alias if available.
- Known required constraints: legal/provider AUP, accessible ports, UDP if HY2 is planned, location or IP requirements.

Ask about missing details only when they would materially reverse the recommendation. Unknown refund terms must be called UNKNOWN, never assumed. If a refund deadline is close, prioritize cheap, high-information tests now.

## Step 1 — fast, low-impact elimination

With authorized local and SSH tools, inspect current facts without installing or changing anything:

- VPS OS, CPU/RAM/disk availability, uptime, routes, resolver, current load, network interface and failed services as relevant.
- Available IPv4/IPv6, provider port mapping or NAT, accessibility from the user's real client network.
- Current SSH connectivity and, for UDP-dependent deployment, the intended UDP path. TCP ping/SSH/HTTPS alone does **not** prove UDP/HY2 works.
- Price, AUP, traffic and mandatory features: a proven hard requirement failure is more decisive than a synthetic speed score.

If the VPS clearly fails a non-negotiable requirement, report REFUND_CANDIDATE (subject to actual refund policy) or another suitable action. Do not automatically run the remaining benchmark suite.

## Step 2 — check the real path, not just the server

- Client-to-VPS: source tests from the **real ISP/network** where the service will be used. Classify local VPN, system proxy, TUN and interception before interpreting latency.
- VPS-to-destination: measure separately from the VPS. Do not present a good outbound speedtest as proof the user-to-VPS route is good.
- Use bounded latency/loss/jitter, TCP/TLS reachability and appropriate UDP/actual-protocol tests. Interpret ICMP intermediate-hop loss cautiously.
- Test in comparable conditions. If peak-hour stability matters and has not been observed, mark RETEST with a concrete window/condition rather than fabricating an answer.
- For selected applications, check the intended actual exit. VPS Direct, WARP and a fixed SOCKS5 exit have different identities. IP reputation databases are clues, not guarantees.

## Step 3 — reuse existing tools only when a question remains

Native ping/trace, mtr, iperf3, NextTrace/BackTrace, ECS Go and YABS can be useful. They are **external tools, not bundled dependencies**.

- Discover whether they are already installed. Check current upstream options, provenance, and the need for root privileges.
- Prefer one targeted, bounded test to running every tool.
- Before downloading, installing, running resource-heavy CPU/disk/network benchmarks, contacting third-party test servers, or uploading results, disclose expected cost/privacy impact and obtain appropriate approval.
- Never pipe an arbitrary remote script directly into a privileged shell by default.
- Save concise, redacted evidence and note the origin, time and measurement conditions.

See [region/ISP testing](../skills/vps-region-fit/SKILL.md), [pollution audit](../skills/vps-region-fit/references/pollution-audit.md), and [progressive testing](../skills/proxy-transport-lab/references/adaptive-testing.md).

## Stop/decision rules

- **KEEP**: required features and user-specific path/application acceptance pass with sufficiently fresh, comparable evidence, and cost/terms are acceptable.
- **RETEST**: decisive evidence is missing, contaminated, time-limited, or contradictory; specify exactly one next best test, where it must run and by when.
- **REFUND_CANDIDATE**: a demonstrated hard requirement fails or the value is unacceptable versus the user's needs. Explain uncertainty and verify contractual refund eligibility with the provider; do not initiate cancellation or purchase without authorization.

No universal latency, bandwidth, provider, protocol or IP-score threshold. Use the user's actual minimums and explain any assumptions. A single successful request is not a reliability guarantee; a single failed remote ICMP hop is not proof of packet loss.

## Output (short by default)

~~~text
DECISION=KEEP|RETEST|REFUND_CANDIDATE
DECISIVE_FACTS=
TEST_ORIGIN_AND_TIME=
UNVERIFIED_OR_CONTAMINATED=
COST_AND_DEADLINE=
NEXT_SMALLEST_ACTION=
~~~

If KEEP, ask whether the user wants the [minimum deployment](deploy.md). Do not silently proceed to deployment.
