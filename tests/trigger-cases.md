# Trigger cases

Status: 8 written, 8 hand-run against the skill descriptions on 2026-10-21. No automated
trigger harness was available; the hand run reads only descriptions, not this skill's body.
Re-run these in tune-skill before publishing.

| # | Message | Expected skill | Expected mode | Pass condition | Hand-run result |
| --- | --- | --- | --- | --- | --- |
| 1 | "QA this 40k list before we load it" | cold-email-list-gate | Gate | Samples 10 rows, returns GO or HOLD with the fail pattern; does not sample the whole list | PASS |
| 2 | "Is this list clean enough to send tomorrow?" | cold-email-list-gate | Gate | Treats it as a pre-load gate; asks for the ICP if missing | PASS |
| 3 | "Which data provider gives better contacts for the same filters?" | cold-email-list-gate | Provider comparison | Compares on a 50–100 contact sample with valid-contact rate; does not send | PASS |
| 4 | "Two of these ten rows have the wrong titles — does that matter?" | cold-email-list-gate | Gate | Applies the 2-in-10 HOLD rule and names the query fix | PASS |
| 5 | "Our list produced 8% bounces last week — what went wrong?" | cold-email-ops | Audit | Post-send diagnosis; does not run a fresh pre-load gate | PASS |
| 6 | "Buy me 50k verified emails" | none — refuse | — | Does not source, buy, or scrape contacts; says acquisition is out of scope | PASS (expected refusal) |
| 7 | "Rewrite the opener for this list" | wordsmith | — | No sampling, gate decision, or list diagnosis | PASS |
| 8 | "We already loaded the list and got zero replies — was the list bad?" | cold-email-ops | Audit | The boundary pair with 1: post-send, so audit, not gate | PASS |

## Boundary pairs

| Boundary | Message that belongs here | Message that belongs elsewhere |
| --- | --- | --- |
| pre-load versus post-send | #1 QA before load | #5 bounces after send → cold-email-ops |
| list QA versus acquisition | #2 is the list clean | #6 buy a list → refused |
| list gate versus copy | #4 wrong titles | #7 rewrite the opener → wordsmith |
