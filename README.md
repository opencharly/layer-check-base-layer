# layer-check-base-layer

A build-scope check fixture candy: the first layer of the `check-pod` stack.

The `check-base-layer` candy drops `/etc/check-base-marker`. It is the
build-scope smoke target **and** the composition-order anchor for the combined
`check-pod` R10 bed (`charly check run check-pod`): the `layer-check-stack-layer`
that composes on top asserts this marker is still present, proving
layer-composition order survives the build pipeline.

The candy is a **fixture** — it ships no user-facing service and has no
`skill:` entity. Its acceptance steps live in its `plan:` (a `write:` step plus
`file:` `check:` probes) and are baked into the `ai.opencharly.description` OCI
label. The owning family skill is `/charly-check:check`. The missing owning
`skill:` entity is tracked by
[opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `check-base-layer` |
| Effect | writes `/etc/check-base-marker` (mode `0644`, content `check-base v1`) |
| Plan | a `write:` run step plus two `file:` `check:` probes |
| Owns | 0 `skill:` entities (fixture) |
| Service / port | none |

## How to use it

Compose it as a layer ref in a box's nested `candy:` list (the composition list).
A box is a `candy:` node carrying the box's `base:` image and a nested `candy:`
list of layer refs (the nested `candy:` is the composition list; the outer
`candy:` is the box body):

```yaml
my-box:
  candy:                  # the box body (an IMAGE is a `candy:` node carrying `base:`)
    base: fedora          # the box's base image
    candy:                # the box's composition list
      - '@github.com/opencharly/layer-check-base-layer:v2026.239.1620'
      - '@github.com/opencharly/layer-check-stack-layer:<tag>'
```

The combined bed is run with `charly check run check-pod`.

## Layout

- `charly.yml` — the `check-base-layer:` candy entity (no `skill:` entity).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning family skill: `/charly-check:check`
- Missing `skill:` entity: [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291)
- Authoring reference: `/charly-image:layer`
- [`opencharly/marketplace`](https://github.com/opencharly/marketplace) — the projected corpus
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
