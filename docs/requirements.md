# Security lead requirements brief

1. Deliver one project in 45 minutes including setup.
2. Use phase pins of setup 6, design 8, build 20, validation 7, and close-out 4.
3. Protect the full 20-minute build floor.
4. Install only AWS CLI; use the learner's existing Git, editor, and AI client.
5. Use exactly three AI prompts in design and none in build or validation.
6. Create only BoundaryProofCeiling and BoundaryProofRole.
7. Keep widened.json local, pass it to simulate-principal-policy, and never attach it.
8. Paste complete output for all six named checks.
9. Compare eight frozen predictions with eight actual decisions and report matches/8.
10. Treat AllowedByPermissionsBoundary false as the boundary evidence.
11. Do not claim the simulator returns a statement Sid.
12. Keep every graded acceptance criterion within IAM and the policy simulator.
13. Declare the graded path cost as 0.00 USD.
14. Remove the boundary before deleting its policy.
15. Prove role and policy absence with NoSuchEntity.
16. Run teardown before the final evidence commit and local tag.
17. Ship a self-contained HTML readout and keep all design artifacts.