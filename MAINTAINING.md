# Maintaining Alien Defender

This is a reproducible local publication candidate, **NOT FOR DISTRIBUTION**. A successful build does not approve copy, pass a learner test, update GitHub, or refresh MakeCode's served version. See `RELEASE_MANIFEST.json` for the exact candidate, inputs, media, pending gates, and file hashes.

The canonical Step 5 text now states “Your ship is a sprite of kind **Player**.” before asking for the overlap behavior. The complete passage is `AGENT_REVIEWED` under the owner's standing source-grounded maintenance policy, not human-approved. The authoring record is `games/alien-defender/design/V5_AGENT_COPY_REVIEW_2026-09-04.json`; the reviewed learner-passage SHA-256 is `039cdb0c8126237c7919a014a231cc978b57edc1419cdd813f5905e0467c35db`. This review and cue integration do not pass either learner gate or update the served tutorial.

## Canonical ownership

The learning-game authoring repository owns the learner text, supplied world, asset definitions, tutorial assembler, demonstration recordings, publication inputs, and evidence. This repository's generated `README.md` must not become a hand-edited fork.

In the authoring repository, use:

```powershell
node games/alien-defender/tools/build-publication-project.mjs --plan
node games/alien-defender/tools/build-publication-project.mjs --build --output games/alien-defender/generated/publication/NEW-STAGING-NAME
node games/alien-defender/tools/build-publication-project.mjs --check games/alien-defender/generated/publication/NEW-STAGING-NAME
```

Choose a new destination for each build. The builder refuses an existing directory and performs no account writes. It calls the canonical tutorial assembler and changes only the media host mapping, then appends the canonical source/license comment. The empty `main.ts` is intentional: the tutorial starter, hidden world, simulator configuration, and image payload remain in the tutorial's existing `template`, `customts`, and `assetjson` fences.

GIF paths are content-addressed. Never overwrite an existing media-revision directory with different bytes. Binary GIFs are repository web assets, not TypeScript dependency files and not entries in `pxt.json`'s `files` array. The publication manifest binds every GIF to its original recording metadata. Staged raw-GitHub URLs will not serve the new files until an authorized publication uploads them; local candidate media use the corresponding local host mapping.

## Required release sequence

1. Complete the release-equivalent `FULL_LEARNER` run with a fresh-context learner agent and a verified new/reset tutorial test project. The existing supported browser may be reused; a new profile, Guest session, or computer is not required. Verify the original starter, first tutorial step, initial game state, and absence of carried-over learner solution/completion. Do not clear unrelated projects or storage. The learner then constructs and plays independently through the visible interface. Keep served-version promotion blocked until this local run passes.
2. Use the owner's standing authorization to update the existing independent `mrbrackebusch-code/alien-defender` project; routine scoped maintenance does not need another publication instruction or human-copy signoff. Review the intended files; never replace unrelated repository work. Preserve the eight GIFs and rights notices as well as the MakeCode project files. Source grounding, protected wording, provenance, rights, target accuracy and cohesion review remain required for subsequent changes.
3. Commit and push the project through MakeCode's GitHub integration. The staging `pxt.json` retains the previously published `1.0.0` baseline; it is not a new served version. Select and create the next version through MakeCode's GitHub integration. A GitHub-only tag/release does not refresh MakeCode's documented tutorial cloud cache. Let MakeCode generate its current `tutorial-info-cache.json`; do not copy the old cache into this candidate.
4. Verify the new served commit, release, protected text, and all eight media URLs. Then run a second fresh-context `FULL_LEARNER` journey in a verified fresh tutorial project on the exact stable public tutorial URL, using the same new/reset-project rule. Student distribution and any claim that the changed version is student-ready wait for that public-surface pass.

Record and retain the previous serving commit and release before updating. Never force-push, rewrite shared history, move a published tag, delete a prior release, or overwrite an old staging package. Recover through a new restoration/revert commit or a new version. Before the local learner pass, progress may be preserved on a clearly identified candidate ref only after verifying that ref cannot advance the student route or trigger a deployment; a “candidate” label on the serving branch is not protection. If the public test fails after promotion, preserve the failure, repair and retest, or restore the prior served version through new history. Withholding the link does not hide a change to an already distributed stable URL.

The authoring policies are `docs/game-production/PUBLICATION_STANDARD.md`, `docs/game-production/LEARNER_PLAYTEST_STANDARD.md`, and the September 4 standing-maintenance and fresh-project-testing owner decisions. Superseded packages and blocked test reports remain historical records; they are not silently updated or retroactively passed.

The stable identity is `https://arcade.makecode.com/#tutorial:https://github.com/mrbrackebusch-code/alien-defender/README`. The explicit `/README` is deliberate. Keeping this identity does not mean the live site already contains this candidate. Anonymous or persistent MakeCode Share tutorial carriers are prohibited.

Official workflow references: [user tutorials and caching](https://makecode.com/writing-docs/user-tutorials), [MakeCode GitHub commit/push](https://arcade.makecode.com/github/getting-started), and [MakeCode releases](https://arcade.makecode.com/github/release).

GitHub versions the tutorial, not individual students' unfinished programs. Student work needs the permitted MakeCode local/cloud or encoded-project-download workflow; it does not require Share or write access to the tutorial repository.
