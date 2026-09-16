# IAM Permissions Boundary as a Role Ceiling

`BoundaryProofCeiling`, a customer-managed policy, is set as the permissions boundary on
`BoundaryProofRole`. That caps the role's maximum permissions, whatever identity policies it is given.
The IAM policy simulator evaluates the same proposed identity policy (`widened.json`) against the
role **before** and **after** the boundary is attached:

| Action | Identity policy | Ceiling | No boundary | Boundary attached |
|---|---|---|---|---|
| `iam:CreatePolicyVersion` | Allow | — | **allowed** | **implicitDeny**, `AllowedByPermissionsBoundary: false` |
| `iam:GetRole` | Allow | Allow | — | **allowed** |
| 5 role mutations | Deny | Allow | — | **explicitDeny** (the ceiling can't override a Deny) |
| `iam:ListUsers` | — | Allow | — | **implicitDeny** (the ceiling grants nothing on its own) |

The widening is only ever simulator input and is never attached to the role.

> AWS account IDs and IAM unique IDs are redacted to `111122223333`, `AIDACKCEVSQ6C2EXAMPLE`, and
> `AROADBQP57FF2AEXAMPLE` throughout this repository and its history. The replacements have the
> same length as the originals, so the simulator's `StartPosition`/`EndPosition` columns in `results/`
> still line up. Substitute your own account ID when you reproduce the commands.

## What's here

| Path | What it is |
|---|---|
| `intent.md` | The decision the security lead needs, in plain language |
| `trust.json` | Role trust policy (EC2 service principal, no instance profile) |
| `boundary.json` | `BoundaryProofCeiling`, the permissions boundary. It allows seven reviewed IAM actions and omits `iam:CreatePolicyVersion` |
| `widened.json` | The proposed widening, a JSON **array of policy strings** passed to `--policy-input-list`. It is never attached to anything |
| `predictions.csv` | Eight predicted decisions, frozen before the build |
| `results/01…06-*.json` | Complete simulator and STS output for the six required checks |
| `results/actual.csv` | Predicted vs actual decision, decision rule, boundary flag, and returned source position |
| `results/scored.md` | Exact match count (8/8) |
| `readout.html` | Self-contained one-page readout for the security lead |
| `docs/` | Requirements, decision records, topology, phase pins, source map, build record, after-action review, task list |

## Before and after, check by check

| # | Evidence | Decision |
|---|---|---|
| 1 | `01-caller.json` | Administrative IAM user, not account root |
| 2 | `02-baseline.json` | **Before:** `iam:CreatePolicyVersion` **allowed**. No boundary on the role, so the identity policy decides alone |
| 3 | `03-explicit-denies.json` | **After:** five role mutations → `explicitDeny`, matched at `1:237` |
| 4 | `04-implicit-deny.json` | `iam:ListUsers` → `implicitDeny` (inside the boundary, no identity allow) |
| 5 | `05-boundary-deny.json` | `iam:CreatePolicyVersion` → `implicitDeny`, **`AllowedByPermissionsBoundary: false`** |
| 6 | `06-allowed-control.json` | `iam:GetRole` → `allowed`, matched at `1:41` |

## Reproducing it

Only the AWS CLI is required, run as a non-root administrative identity. IAM and the simulator are free.

```bash
aws sts get-caller-identity

aws iam create-policy --policy-name BoundaryProofCeiling --policy-document file://boundary.json
aws iam create-role --role-name BoundaryProofRole --assume-role-policy-document file://trust.json

# Check 2: the unbounded baseline
aws iam simulate-principal-policy \
  --policy-source-arn arn:aws:iam::<ACCOUNT_ID>:role/BoundaryProofRole \
  --policy-input-list file://widened.json \
  --action-names iam:CreatePolicyVersion \
  --resource-arns arn:aws:iam::<ACCOUNT_ID>:policy/BoundaryProofCeiling

aws iam put-role-permissions-boundary --role-name BoundaryProofRole \
  --permissions-boundary arn:aws:iam::<ACCOUNT_ID>:policy/BoundaryProofCeiling

# Checks 3–6: rerun simulate-principal-policy with the same --policy-input-list
# for each action set in predictions.csv

# Teardown, in dependency order
aws iam delete-role-permissions-boundary --role-name BoundaryProofRole
aws iam delete-policy --policy-arn arn:aws:iam::<ACCOUNT_ID>:policy/BoundaryProofCeiling
aws iam delete-role --role-name BoundaryProofRole
aws iam get-role --role-name BoundaryProofRole                                          # NoSuchEntity
aws iam get-policy --policy-arn arn:aws:iam::<ACCOUNT_ID>:policy/BoundaryProofCeiling   # NoSuchEntity
```

`--policy-input-list` expects each policy as a **JSON string**, not a nested object. That is why
`widened.json` is an array holding one escaped document.

## Limits

- The simulator models decisions and makes no live service request. AWS notes that its results can differ from the live environment.
- Not tested: resource-based policies, service control policies, cross-account trust, or a separate human console session.
- The simulator returns the source position of a matched statement, not its `Sid`.
- **The `v1.0.0` tag sits one commit early.** It points at `test: adjudicate eight frozen predictions`, not the teardown commit. The tag already existed when the close-out commit ran, so `git tag -a` failed. That contradicts the last line of `results/check-manifest.md`. The tag is left where it landed rather than moved after the fact.
- The two `NoSuchEntity` teardown reads were captured as terminal screenshots (in the write-up). They were not pasted into a committed file.

## Write-up

Full write-up — animated architecture diagram, the before/after simulation of `iam:CreatePolicyVersion`,
and how identity policy and ceiling combine for each case —
lives in my portfolio repo:
**[Projects / aws / iam-boundary-proof](https://github.com/kingswanzy2020/Projects/tree/main/aws/iam-boundary-proof)**.
