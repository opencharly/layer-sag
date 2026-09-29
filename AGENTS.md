# AGENTS.md — layer-sag

Standalone candy repo for the `sag` layer — the ElevenLabs text-to-speech CLI
go-installed into the user's GOPATH bin. The candy lives in `charly.yml` at the
repo root: the `require:` on `layer-golang`, the Fedora `alsa-lib-devel` package,
the `env:`/`path_append:`, the `go install` step, the `check:` probes, and the
embedded `skill:` entity projected into the marketplace corpus as
`/charly-tools:sag`.

Canonical files:

- `charly.yml` — the `sag:` candy entity and the `sag-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-tools:sag` — the owning skill. The `go install` path, the GOPATH/PATH
  wiring, and the `ELEVENLABS_API_KEY` secret. Load before editing or
  troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `command:`/`check:`, `distro:` sections). Load before
  editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's `plan:` `check:` steps are the functional evidence: `~/go/bin/sag`
  exists, `command -v sag` resolves to it, and the Fedora `alsa-lib-devel`
  package is installed (the check is `exclude_distro`-gated to the RPM family).

## Modify this repo

- Edit the `sag:` candy entity AND the `sag-skill:` skill entity in `charly.yml`
  together. The skill is the projected usage source, so a package or path change
  not mirrored in the skill leaves the corpus stale.
- The Fedora `alsa-lib-devel` check is `exclude_distro`-scoped; keep that scope
  aligned with the `distro.fedora:` package section.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
