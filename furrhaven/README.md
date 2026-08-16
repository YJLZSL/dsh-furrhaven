# Furrhaven fork layer

Furrhaven Studio is a fork of the official DeepSeek Harness framework, not a
separate desktop stack. This directory is the fork's additive layer: every
Furrhaven asset lives here so upstream merges stay clean. The official
framework sources above (`apps/`, `packages/`, `vendor/`) are unmodified
upstream files; no Furrhaven code is patched into them yet.

## Layout

```
furrhaven/
  presets/card-forge/          write-card agent preset (agent.cordis.yml + router .mjs)
  plugins/fh-tools/            host plugin: 9 fh_* tools (tsc build → lib/)
  skills/furrhaven-card/       write-card workflow discipline
  theme/furrhaven-theme.css    "gold-paper workshop" overlay theme for the DSH web client
```

## Install (profile `web`, development)

```powershell
# 1. preset → user preset root (discovered without restart)
Copy-Item furrhaven\presets\card-forge $env:USERPROFILE\.dsh\.agent-presets\ -Recurse -Force

# 2. skill → user skill root
Copy-Item furrhaven\skills\furrhaven-card $env:USERPROFILE\.dsh\skills\ -Recurse -Force

# 3. plugin → build against a DSH checkout, then super-injector or plugin add
cd furrhaven\plugins\fh-tools
DSH_CHECKOUT=<checkout> bash scripts/build.sh
# super-injector: dev_inject_plugin <dir>
```

## Fork policy

- `upstream` = `https://github.com/deepseek-ai/deepseek-harness.git` (official).
- `origin` = the private Furrhaven mirror. Branch `furrhaven` carries this layer.
- Never edit upstream files to add Furrhaven behavior without an Agent Note
  (repo convention). Prefer additive packages/plugins.
- Sync: see `../scripts/sync-dsh.ps1` in the parent repository.

## Status

- v0.1: preset/skill/plugin/theme shipped as additive files; plugin built and
  hot-injected against DSH 0.1.0-rc.5.
- Next: register `fh-tools` as a workspace package and wire a Furrhaven client
  theme into `packages/client/ui-theme` (slot/store discipline applies).
