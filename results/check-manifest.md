# Required check manifest

1. results/01-caller.json: administrative caller, not account root
2. results/02-baseline.json: widening allowed without boundary
3. results/03-explicit-denies.json: five explicit denies
4. results/04-implicit-deny.json: missing identity allow
5. results/05-boundary-deny.json: AllowedByPermissionsBoundary is false
6. results/06-allowed-control.json: allowed read control

All six files contain complete pasted command output before teardown. The final tag was created only after teardown evidence existed.

