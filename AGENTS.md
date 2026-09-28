# AGENTS.md — layer-check-base-layer

Standalone candy repo for the `check-base-layer` fixture — the first layer of the
`check-pod` stack. It writes `/etc/check-base-marker`; the layer composed on top
asserts the marker survives, proving composition order. It ships no service and
carries **no `skill:` entity**.

Canonical files:

- `charly.yml` — the `check-base-layer:` candy entity (a `write:` run step plus
  two `file:` `check:` probes; no `skill:` entity).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-check:check` — the owning family skill: the check plan authoring
  reference, the disposable beds, the R10 change classes, and the probe verbs.
  Load before editing any `plan:` step.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.
- **Missing owning skill:** this fixture has no `skill:` entity, so no
  `/charly-check-base-layer:*` page is projected for it. The gap is recorded
  against the named batch
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has no
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The candy's own proof is its `plan:` — the `write:` step plus the two `file:`
  `check:` probes — exercised inside the combined `check-pod` bed
  (`charly check run check-pod`).

## Modify this repo

- Keep the marker path and content stable: the composed
  `layer-check-stack-layer` asserts both, so a change here is a cross-repo
  change.
- The plan's `check:` probes are the acceptance test; keep at least one
  deterministic probe and never weaken it without a replacement.
- If an owning skill is authored, add the `skill:` entity here and update this
  signpost and the README in the same change.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
