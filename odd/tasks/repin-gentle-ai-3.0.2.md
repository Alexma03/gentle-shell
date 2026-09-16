# Repin gentle-ai v3.0.1 → v3.0.2 + merge upstream/main

## Objective
Merge `upstream/main` into `custom/main` (3 commits) and bump the packaged
gentle-ai pin from v3.0.1 to v3.0.2 (install-scripts hotfix, same provider
contract 1.2.0).

## Scope
- Merge upstream/main → custom/main (done: d2f610b8, clean, stash round-tripped).
- Repin surface (mirrors #1118): `scripts/gentle-ai-installer.mjs`
  (INSTALLER_VERSION, 4 asset rows, Windows SumDB checksum, comments),
  `lib/native-review-cli.ts` + `runtime/native-review-cli.mjs` (contracts row),
  `scripts/verify-package-files.mjs` (messages), docs mentions,
  mirrored digests in `tests/gentle-ai-installer.test.ts`.
- No push without explicit user approval.

## Evidence rules
- Archive sha256 from minisign-signed checksums.txt of the published v3.0.2
  release; binary sha256 computed from extracted executables.
- Windows module checksum from `go mod download -json` with SumDB.
- Contracts row ground-truthed by diffing tags v3.0.1..v3.0.2 in gentle-ai clone.

## Tasks
- [x] T1 Merge upstream/main into custom/main (clean, no conflicts)
- [x] T2 Collect v3.0.2 digests (checksums.txt, 4 archives, SumDB)
- [x] T3 Apply pin edits across surface
- [x] T4 Run focused tests + typecheck
- [x] T5 Report; push only on approval

## Progress
- T1: merged d2f610b8; `git stash` round-trip of dirty
  tests/devbinary/pi-host-relay.devtest.ts clean; branch ahead of
  origin/custom/main by 14, unpushed.
- opencode2 check: no PR implements compat. Issue #3744 (feature,
  status:needs-review) + #4592 (automated defect, OPEN) track it.
- Repin done unpushed: INSTALLER_VERSION 3.0.2, 4 asset rows with fresh
  archive+binary sha256 (archives OK vs checksums.txt, binaries extracted
  locally), SumDB h1:UtealViTal0mOI4pdDExGTsv48/wlLJK1WWZphmQblI=,
  tag -> 9bf454d4, contracts v3.0.1..v3.0.2 zero bytes changed so the
  3.0.2 row repeats 3.0.1. Caveat: minisign signature present but not
  verified (pubkey only in upstream CI vars); authenticity rests on
  TLS-bound GitHub release + SumDB.
- Checks: focused 113 pass/0 fail; full suite 2666 tests, 2619 pass,
  0 fail, 47 skipped; provider-contract OK; typecheck no regressions.
