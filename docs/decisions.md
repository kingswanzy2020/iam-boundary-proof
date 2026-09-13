# Decision records

## Direct AWS CLI calls

Decision: Use direct, single-line AWS CLI invocations and editor-authored JSON.
Alternative: Shell scripts and pipelines.
Tradeoff: More manual evidence capture in exchange for the same graded commands across Windows, macOS, and Linux.
Reversal trigger: A later project can add tested automation without changing this evidence model.

## Local Git only

Decision: Keep the handoff in a local repository with no remote.
Alternative: A code host and ticket account.
Tradeoff: No collaboration or off-device backup in exchange for zero new external accounts.
Reversal trigger: The security lead requests team review or durable remote storage.

## Simulator adjudication

Decision: Use simulate-principal-policy and retain raw outputs.
Alternative: Execute live service operations.
Tradeoff: Safe, reproducible modelled decisions instead of proof of live runtime behavior.
Reversal trigger: A controlled integration account and live test plan become available.

## Existing learner tools

Decision: Use whichever editor and AI client the learner already has.
Alternative: Require named vendors.
Tradeoff: Consistent cohort access instead of vendor-specific screenshots or prompts.
Reversal trigger: The cohort standardizes its workstation image.