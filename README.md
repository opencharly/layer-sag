# sag

The [ElevenLabs](https://elevenlabs.io) text-to-speech CLI for OpenCharly images.

The `sag` candy go-installs `github.com/steipete/sag` into the user's GOPATH bin
(`~/go/bin`) and appends that directory to `PATH`, so `sag` is invokable by the
container user. On Fedora the `alsa-lib-devel` headers are pulled in for the cgo
audio backend.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `sag` |
| Binary | `~/go/bin/sag` |
| Install | `go install github.com/steipete/sag/cmd/sag@latest` |
| Requires | [`layer-golang`](https://github.com/opencharly/layer-golang) |
| Env | `GOPATH=~/go`, PATH append `~/go/bin` |
| Fedora package | `alsa-lib-devel` (cgo audio backend) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-tts-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-sag:v2026.243.0409'
```

Then, inside the built image (or on a dev host):

```bash
sag --help     # the ElevenLabs TTS CLI
```

Supply `ELEVENLABS_API_KEY` via `charly secrets` at runtime.

The candy's `plan:` asserts the binary is present, resolves on `PATH` from the Go
bin directory, and that the Fedora `alsa-lib-devel` package is installed.

## Layout

- `charly.yml` — the `sag:` candy entity (the `require:`, the `distro.fedora:`
  package section, the `env:`/`path_append:`, the `go install` step, and the
  `check:` probes) and the embedded `sag-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-tools:sag`
- Dependency: `/charly-coder:golang`
- Sibling TTS: `/charly-tools:sherpa-onnx` (offline)
- Speech-to-text: `/charly-tools:whisper`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
