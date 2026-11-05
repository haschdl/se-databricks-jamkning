# Databricks Sweden — Jämkning Skill

An [Agent Skill](https://agentskills.io/) to help employees of Databricks Sweden understand and prepare a Swedish **jämkning** application (adjustment of preliminary income-tax withholding).

Jämkning changes how much preliminary tax is withheld during the year; it does **not** change final tax liability.

## What the skill covers

- Decide whether jämkning is worth investigating, including the risk of **kvarskatt** (tax due) if withholding is too low.
- Reconcile Skatteverket's year-to-date income records with Databricks payslips.
- Account for RSU vestings, Shareworks reports, commissions, benefits, and **engångsskatt** (one-time payment withholding).
- Assess employee tender offers and Swedish share acquisition-cost calculations.
- Collect mortgage-interest, ISK, other capital-income, and tax-reduction information.
- Model uncertain income and review fields in the Skatteverket application.
- Check the decision's application to future payroll and RSU events.

The skill deliberately distinguishes sourced tax-year rules from items still **UNVERIFIED**. It should not be used to apply one year's tax rates or form codes to another year without checking Skatteverket.

## Try it

**Ask an agent to inspect the repository** using:

> Read https://github.com/haschdl/se-databricks-jamkning, including SKILL.md and its referenced files. Explain whether Swedish jämkning might be useful for a Databricks Sweden employee with RSUs, and tell me what evidence you need before making a calculation.

For reliable repeated use, **install the skill** rather than relying on a one-time URL read.

### Claude Code (from GitHub)

With Node.js available, use the [skills CLI](https://skills.sh/docs) in your working project:

```bash
npx skills add haschdl/se-databricks-jamkning
```

Follow the installer prompts and select Claude Code. The installer copies the skill and its supporting files into a location your agent can discover. Inspect the source before installing any third-party skill.

Alternatively, clone the repo and copy the skill root and `references/` together into a named Claude Code skill directory (for example `~/.claude/skills/databricks-sweden-jamkning/`). A `SKILL.md` without its referenced files is incomplete.

### Claude.ai

If your Claude plan supports custom skills, upload a ZIP containing **one skill folder** with `SKILL.md` and the `references/` directory. See [Anthropic's skills repository](https://github.com/anthropics/skills) for examples and documentation.

### ChatGPT

On eligible ChatGPT workspaces, open **Plugins → Skills → Create → Upload from your computer** and upload the packaged skill. Availability depends on plan and workspace settings; see [Skills in ChatGPT](https://help.openai.com/en/articles/20001066-skills-in-chatgpt).

If native skill installation is unavailable, you can still ask ChatGPT to read the public repository and its references for a particular conversation. That is **not** the same as permanently installing the skill.

## Repository layout

```text
.
├── README.md                          # Human-facing overview and reuse instructions
├── SKILL.md                           # Agent-facing workflow and trigger description
├── references/
│   ├── 2026-tax-constants.md          # Verified tax-year rules and unresolved values
│   ├── databricks-payroll.md          # Payslip, Shareworks and tender-offer data
│   ├── skatteverket-form-fields.md    # E-service field mappings
│   └── checklists.md                 # Evidence, reconciliation and submission checks
└── evals/                             # Synthetic behavioral evaluation prompts
```

Start with [SKILL.md](SKILL.md). Supporting details are deliberately kept in `references/` so the core workflow is short and easier to maintain.

## Validate before using

For a quick independent scan of this public repository:

- [CheckSkills](https://checkskills.dev/check/) — paste the GitHub URL for an automated grade.
- [skillemall.ai](https://skillemall.ai/?lang=en) — scan the full skill and optionally evaluate model behavior with and without it.

These are third-party services, not authoritative validators of Swedish tax law. The `evals/` directory contains invented scenarios for checking RSU withholding and tender-offer reasoning; it is **not** an automated CI test suite.

## Behavioral evaluation

The repository includes a [SkillTest](https://github.com/archplg/skilltest) contract in [`spec.yaml`](spec.yaml) and six **fictional** requests with expected behavior in [`evals/evals.json`](evals/evals.json). The earlier narrative scenarios remain in `evals/*.md`.

Run locally after reviewing the inputs and any external model costs:

```bash
npx skilleval run . --dry-run
npx skilleval run . --budget 3
```

The contract enables `run.include_files: [auto]` so model evaluations also see the tax, form, and payroll references. It also checks a short first-turn triage against a structured pre-submission review. Out-of-scope phrases test whether this **skill activates**, not whether a general-purpose assistant must refuse legitimate unrelated requests. A static scan or a dry run does not establish behavioral correctness. No paid model run is enabled automatically by GitHub Actions.

## Privacy and limitations

**Do not commit employee financial records or application data to this repository.** Do not include actual payslips, Shareworks statements, compensation letters, brokerage exports, names, account numbers, addresses, or completed Skatteverket forms. Use synthetic inputs in examples and evaluations.

The workflow is an aid to understanding, estimating, and preparing an application—not financial, legal, or tax advice. Verify current rules with [Skatteverket](https://www.skatteverket.se/jamkning), and use its e-service or calculator for the final-tax estimate where possible.

Some items are intentionally unresolved, including how the jämkning e-service applies the ISK tax-free allowance and the numbering of the paper SKV 4302 form. See the references for current status.

## Maintenance

Tax-year constants and their sources belong in `references/2026-tax-constants.md`, and form field mappings in `references/skatteverket-form-fields.md`. Update or create year-specific references before supporting another tax year. Keep examples fictional and review the `evals/` scenarios after behavioral changes.
