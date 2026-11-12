# Test case format

A test case describes one business task and the exact end state that proves it was done.
It is written so a program, not a person, decides whether a bot or AI agent succeeded.
See `example.yaml` for a complete case.

| Field | Required | Meaning |
| --- | --- | --- |
| `id` | yes | Unique name for the case, lowercase with dashes |
| `environment` | yes | Which test system the case runs in (for example `odoo17`, `osticket`) |
| `surface` | yes | How the system is used: `web`, `desktop`, `citrix` or `terminal` |
| `start_url` | yes | Where the bot or agent starts |
| `login` | yes | The name of a stored login to use. Passwords are never written in the case |
| `seed` | yes | The starting data. `baseline` means the environment's clean snapshot |
| `instruction` | yes | The task, in plain business language, exactly as a person would be told |
| `max_steps` | yes | Most actions allowed before the attempt counts as failed |
| `max_cost_usd` | yes | Most money (model usage) allowed before the attempt counts as failed |
| `forbidden_screens` | yes | Screens the bot or agent must never open; visiting one fails the attempt |
| `expected.db` | yes | Database checks. Each has a `query` and the value it must `equals` |
| `expected.ui` | no | Checks on what the screen shows at the end |
| `expected.files` | no | Checks on files that must exist at the end |
| `variants` | no | Versions of the same case, for example after a software upgrade |

## How a case is scored

Every case is run several times (k runs) from the same clean starting state.
A run passes only if every check in `expected` passes and no forbidden screen was visited.
**pass^k** is the share of cases where all k runs passed. It measures whether
something works every time, not just once.

## How a case is proven before use

Each case must:
1. fail its checks on the clean starting state,
2. pass them after a person does the task by hand,
3. fail them again after the environment is reset.

A case that does not do all three is not used.
EOF
