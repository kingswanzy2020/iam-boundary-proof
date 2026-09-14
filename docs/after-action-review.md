# After-action review for the security lead

## Proven

- 8/8: exact prediction score
- Five explicit-deny decisions and returned source positions
- Ordinary implicit deny for iam:ListUsers
- AllowedByPermissionsBoundary false for iam:CreatePolicyVersion
- Allowed iam:GetRole control
- Role and policy NoSuchEntity results

## Left undone and tradeoff

- Live service authorization remains untested; traded for a no-side-effect simulator proof.
- Resource-based policies remain untested; traded for a focused role and identity-policy boundary case.
- Service control policies remain untested; traded for a single-account proof that every learner can run.
- A separate human console session remains untested; traded for one reproducible administrative CLI session.
- Cross-account trust remains untested; traded for a two-resource build that fits 45 minutes.
- Remote backup and collaboration remain unconfigured; traded for no code-host or ticket-account dependency.
