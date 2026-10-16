# Security policy

Verity holds bonds and stake in Soroban contracts, and its verdicts are meant
to gate real payouts. **The contracts are unaudited and deployed on testnet
only.** Do not treat any current deployment as safe for real value.

## Reporting a vulnerability

Please report privately. Do **not** open a public issue or pull request for a
security problem.

Use GitHub's private vulnerability reporting:
<https://github.com/DevQwinB/Verity/security/advisories/new>

Include what you can of:

- the component (`escrow-gate`, `zk-verifier-registry`, backend, re-executor
  agent, dashboard) and the commit or deployment you tested
- steps or a transaction sequence that reproduces it
- what an attacker gains: stolen or locked funds, a wrong verdict, an honest
  party slashed, denial of service, or exposed secrets
- a suggested fix, if you have one

You can expect an acknowledgement, a status update as we investigate, and
credit in the fix unless you ask otherwise. Please give us reasonable time to
fix an issue before disclosing it, and do not exploit it beyond what is needed
to demonstrate it.

Testing against the shared dev deployment (`deployments/testnet.json`) is fine.
Do not test against a deployment marked `"public": true` that other people are
using, other than reading from it.

## What matters most

Reports in these areas are the highest value:

| Area | Examples |
|---|---|
| Verdict integrity | Any path to a verdict that is not derived from bonded re-execution, an unchallenged window, or an unresolved challenge expiring. Any way to make a ZK result appear (the ZK method must be rejected). Bypassing the challenge window. |
| Funds | Draining or freezing agent bonds, challenger bonds or re-executor stake. Withdrawing stake ahead of a slash. Double settlement (`finalize()` must be idempotent). |
| Slashing | Slashing an honest voter, or letting a wrong-side voter escape. Choosing the slash amount. Admin discretion over a verdict (the admin must not be able to set, change or override one). |
| Voter set | Capturing the voter set where the allow-list is meant to hold; Sybil attacks on assignment. |
| Sandbox | Escaping the WebAssembly replay sandbox, reading the host's files or environment, or exhausting a re-executor's host without hitting a limit. |
| Backend | Authentication bypass (SEP-10, JWT), the relay forwarding a transaction it was not built for, SSRF through webhook URLs or retrieval targets, the keeper key being used for anything beyond keeper calls. |
| Secrets | Any leak of keeper, re-executor, SEP-10 or JWT secrets in code, logs, images or API responses. |

## Out of scope

- Findings that only apply to testnet-specific behaviour, such as free stake.
  The README documents why deployments run with `reexecutor_allowlist` on.
- Limitations the README already lists under "Known limits" (for example, a
  verdict only covers consistency with the artifacts the agent declared;
  assignment is uniform; webhooks have no retry).
- Phase 2 features that do not exist yet, such as ZK proving.
- Vulnerabilities in third-party dependencies with no demonstrated impact on
  Verity. Please report those upstream.
- Denial of service by sheer traffic volume.

## Supported versions

Only the latest commit on `main` is supported. There are no tagged releases
yet.
