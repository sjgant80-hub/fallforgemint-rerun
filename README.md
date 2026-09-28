# fallforgemint-rerun

**▶ LIVE: https://sjgant80-hub.github.io/fallforgemint/#rerun** — make a scorecard, download its re-run bundle, and read how the rail works.

Re-run a [FallForge Mint](https://sjgant80-hub.github.io/fallforgemint/) scorecard on a **clean GitHub runner**, in the open, without trusting whoever made it.

## Use it

1. Press **Use this template** to make your own copy of this repo.
2. Add the re-run bundle you were sent, or the one you downloaded from the page's **Download re-run bundle** button. Put it at `bundle.json` (or anywhere, and give its path in step 3).
3. Actions → **rerun** → **Run workflow**, with the bundle's path.
4. Read the verdict in the run's summary. The `rerun-result` artifact holds a self-hashed attestation bound to that run's URL. Send the run link: it is the proof.

The `bundle.json` already here is a real scorecard, minted in CI, so you can try the rail before you have your own.

**Mint in CI instead.** Run the workflow with **mode: mint** and a spec (`spec.json` here is one: a task, examples, how many to hold out, a base model). The runner mints the model, scores it on the held-out examples, signs the scorecard and uploads its bundle. The scorecard's `rerun` link is that run. Because this repo only calls the rail, in one job, at a commit that checks out its own code, the rail can later confirm that run made that exact scorecard. Keep the workflow to that one job, or it cannot.

## What the runner does

1. **Re-verify.** It recomputes every recorded number from the bundle: the fingerprints, the rebuilt Modelfile's hash, the task and evidence hashes, the re-graded scores, the held-out disjointness and the signature. If the scorecard names a CI run, it looks that run up on GitHub and checks the run made this exact scorecard (GitHub keeps the run's upload about 90 days; after that it confirms only that the run was the rail and was running at the time). Any mismatch is **TAMPERED**: the job fails and nothing is re-executed.
2. **Re-execute.** It installs Ollama (pinned), rebuilds the minted model, and runs every held-out example through the base and the minted model again. It then grades the results.
   - **Same runtime and model digest:** the hits must match exactly (**REPRODUCED**).
   - **Different runtime** (a scorecard made in a browser): the verdict must hold (**AGREES**).
   - **Otherwise:** **DID_NOT_REPRODUCE**, and the job fails.

The rail runs from [sjgant80-hub/fallforgemint](https://github.com/sjgant80-hub/fallforgemint). Its judgements live in a mutation-gated kernel, and this repo only calls it. The rail always runs the code of the commit you call, so to fix it at one version, replace `@main` in `.github/workflows/rerun.yml` with a commit SHA.

<!-- RAIL-RUNS -->
**Proven from this repo, on real runs.** The `bundle.json` here, re-run through this template: its rerun link is **BOUND** (the run it names made this exact scorecard) and it is **REPRODUCED**, so the job passes: [run 36439663756](https://github.com/sjgant80-hub/fallforgemint-rerun/actions/runs/36439663756). A scorecard **minted** here from `spec.json` ([run 36439678449](https://github.com/sjgant80-hub/fallforgemint-rerun/actions/runs/36439678449)) names that run as its link, and the rail confirmed the run made it: [run 36440212447](https://github.com/sjgant80-hub/fallforgemint/actions/runs/36440212447). An earlier bundle with one borderline row failed here, loudly, when that row flipped on this runner's CPU: [run 36427951661](https://github.com/sjgant80-hub/fallforgemint-rerun/actions/runs/36427951661). The forged and tampered proofs, and the runs that fail them, are listed in [the rail's README](https://github.com/sjgant80-hub/fallforgemint#proof-runs).

These runs are from a repo in the same account as the rail. Any repo can call a public repo's reusable workflow, but a run from a different account has not been shown here yet.
<!-- /RAIL-RUNS -->

**What it shows, and what it doesn't.** Every recorded number is recomputed, and the held-out set is run again on that runner. It does not attest the machine that made the original, and it says nothing about what the base model saw in its own training.

Published by AI-Native Solutions. MIT.
