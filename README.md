# Deep Watermark Remover — GitHub Actions version

No server needed. GitHub's runner does the heavy Deep (SynthID) work.

## Setup (once)

1. Create a **new repo** (see the public/private note below).
2. Add the workflow: put `clean.yml` at `.github/workflows/clean.yml` in the repo.
3. Create an empty `inbox/` folder in the repo (add a blank `inbox/.gitkeep` file so Git keeps it).

## Use (every time, works from the phone browser/app)

1. Open the repo → `inbox/` → **Add file → Upload files** → pick your Gemini image → commit.
2. Go to the **Actions** tab → **Deep Watermark Remover** → **Run workflow** → pick the mode → Run.
3. Wait for the run to finish. Your cleaned image appears in the **`cleaned/`** folder
   (the inbox copy is deleted automatically). It is also attached to the run as the
   `cleaned-images` artifact for 7 days as a backup.

Modes: **fast** = visible logo + metadata (under a minute) · **deep** = SynthID regen
only · **both** = fast then deep.

## The first Deep run is slow, later runs are faster

The first Deep run downloads ~7 GB of SDXL models. The workflow caches them
(`wmr-models-v1`), so later runs skip the download. The cache is ~7 GB of the
10 GB per-repo cache limit — do not add other big caches to this repo. If the
cache is not used for 7 days GitHub may evict it and the next run downloads again.

## Public vs private repo — pick knowingly

| | Public repo | Private repo |
|---|---|---|
| Runner | 4 vCPU / 16 GB RAM | 2 vCPU / 8 GB RAM |
| Minutes | Free, unlimited (standard runners) | 2,000 free minutes/month on the Free plan, then billed |
| Your images | **Visible to anyone** while they sit in the repo | Only you |

Deep mode on the private runner (8 GB RAM) is slower and tighter but works;
on the public runner it is the full machine. If the images are just AI
generations you do not mind being seen, public is the stronger free option.
Anything personal belongs in a private repo — expect roughly a few hundred
Deep cleans a month inside the free minutes.

## Limits

- One workflow job can run at most 6 hours; a single image takes minutes, so this is not a real limit here.
- The runner disk is only ~14 GB free — models (7 GB) + engine fit, but do not process huge batches of 4K images in one run.
- This is GitHub's computer, not a server: there is no always-on web page. Each clean is upload → run → download.
