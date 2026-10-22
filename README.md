# cold-email-list-gate

An agent skill that gates a cold-email lead list before it loads. It samples ten rows against your ICP and returns **GO** or **HOLD**, with the fail pattern, its cause, the change required, and the condition for a fresh sample.

The list breaks before deliverability does. A bad filter can reach 20,000 inboxes before anyone notices; ten rows read before load catch it while the fix is still one query change.

## Install

```
npx skills add arm-pz/cold-email-list-gate --skill cold-email-list-gate
```

For Qoder, copy the folder into the user skills directory:

```
cp -r cold-email-list-gate ~/.qoder/skills/
```

## Ask it things like

- "Is this list safe to load?"
- "Sample ten rows and tell me if these companies fit our ICP."
- "Compare Apollo and our current provider on the same filter set."
- "We fixed the title filter. Re-check the list before we upload."

## The gate

| Sample result | Decision |
|---|---|
| Two or more wrong in ten | HOLD. Fix the query or provider; do not patch individual rows |
| One wrong in ten | GO, with the bad row named in the report |
| Zero wrong in ten | GO |
| Fewer than ten rows | Sample all of them and return INCONCLUSIVE |

- Sample at random, and record the row itself rather than a summary.
- A HOLD is never lifted on the old sample. Re-run a fresh ten after the fix.
- Name the shared fail pattern: wrong filter, stale source, wrong seniority, invalid mailboxes. The pattern tells the owner what to change.
- For a provider comparison, use the same criteria and 50 to 100 contacts per provider. Email is judged on valid-contact rate; phone on dials per in-profile person.

## What it needs

Three inputs: the list itself, the ICP definition, and the query or provider that built it. Without an ICP definition it can only check mailbox validity, and it returns INCONCLUSIVE rather than guessing at fit.

## What it will not do

- Buy, scrape, or enrich contacts.
- Write the email copy. That is a [wordsmith](https://github.com/arm-pz/wordsmith) job.
- Diagnose a list that already sent. That is [cold-email-ops](https://github.com/arm-pz/cold-email-ops); this skill gates before load only.
- Invent sample rows or a GO it did not earn.

## Where the rules come from

The sample size and hold threshold are adapted from Will Breton's published account of running cold-email operations at volume ([source](https://x.com/bretonwill/status/2111690982777323895)). They are labelled as source benchmarks in `references/gate-rules.md`, not as client results. This is an independent implementation and is not affiliated with the author.

## Contents

- `SKILL.md` — the gate workflow and boundaries.
- `references/gate-rules.md` — row rubric, threshold table, and the report template.
- `tests/trigger-cases.md` — 8 trigger cases, hand-run.

Validated with [tune-skill](https://github.com/arm-pz/tune-skill): `check_skill.py` reports 0 fail, 0 warn.

## License

MIT. See [LICENSE](LICENSE).
