# Decision intent for the security lead

The proposed policy allows iam:GetRole as the control.

It explicitly denies five privileged role mutations:

- iam:DeleteRole
- iam:DeleteRolePermissionsBoundary
- iam:UpdateAssumeRolePolicy
- iam:PutRolePolicy
- iam:TagRole

It does not allow iam:ListUsers.

It proposes iam:CreatePolicyVersion against the boundary policy. The unbounded control should allow that proposal. After the boundary is applied, the required evidence is AllowedByPermissionsBoundary false.