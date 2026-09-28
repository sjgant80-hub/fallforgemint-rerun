# fallforgemint-rerun

**▶ LIVE: https://sjgant80-hub.github.io/fallforgemint/#rerun** — make a scorecard, download its re-run bundle, and read how the rail works.

Re-run a [FallForge Mint](https://sjgant80-hub.github.io/fallforgemint/) scorecard on a **clean GitHub runner**, in the open, without trusting whoever made it.

## Use it

1. Press **Use this template** to make your own copy of this repo.
2. Add the re-run bundle you were sent, or the one you downloaded from the page's **Download re-run bundle** button. Put it at `bundle.json` (or anywhere, and give its path in step 3).
3. Actions → **rerun** → **Run workflow**, with the bundle's path.
4. Read the verdict in the run's summary. The `rerun-result` artifact holds a self-hashed attestation bound to that run's URL. Send the run link: it is the proof.

The `bundle.json` already here is a real scorecard, minted in CI, so you can try the rail before you have your own.

## What the runner does

1. **Re-verify.** It recomputes every recorded number from the bundle: the fingerprints, the rebuilt Modelfile's hash, the task and evidence hashes, the re-graded scores, the held-out disjointness and the signature. Any mismatch is **TAMPERED**: the job fails and nothing is re-executed.
2. **Re-execute.** It installs Ollama (pinned), rebuilds the minted model, and runs every held-out example through the base and the minted model again. It then grades the results.
   - **Same runtime and model digest:** the hits must match exactly (**REPRODUCED**).
   - **Different runtime** (a scorecard made in a browser): the verdict must hold (**AGREES**).
   - **Otherwise:** **DID_NOT_REPRODUCE**, and the job fails.

The rail runs from [sjgant80-hub/fallforgemint](https://github.com/sjgant80-hub/fallforgemint). Its judgements live in a mutation-gated kernel, and this repo only calls it. To fix the rail at one version, set both `@main` and `rail-ref` in `.github/workflows/rerun.yml` to the same commit SHA.

<!-- RAIL-RUNS -->
<!-- /RAIL-RUNS -->

**What it shows, and what it doesn't.** Every recorded number is recomputed, and the held-out set is run again on that runner. It does not attest the machine that made the original, and it says nothing about what the base model saw in its own training.

Published by AI-Native Solutions. MIT.
