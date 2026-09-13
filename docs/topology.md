# Evidence topology

## Local evidence

- docs/TASKS.md
- docs/
- intent.md
- trust.json
- boundary.json
- widened.json
- predictions.csv
- results/
- readout.html
- .git/

## Temporary IAM resources

- BoundaryProofCeiling: customer managed policy used as the role's permissions boundary during validation
- BoundaryProofRole: role used as the policy-source ARN

## Ordered flow

1. Save the non-root caller response.
2. Freeze design documents, policies, and predictions.
3. Create the policy and role.
4. Pass widened.json to the simulator before the boundary and save the allowed baseline.
5. Attach BoundaryProofCeiling to BoundaryProofRole as the boundary.
6. Pass widened.json to four guarded simulations and score eight decisions.
7. Remove the boundary, clear dependencies, delete policy, delete role, and prove absence.
8. Commit and tag only after teardown.

widened.json is never attached to any identity.