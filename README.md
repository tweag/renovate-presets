# Renovate Presets

This repository contains [shared Renovate presets](https://docs.renovatebot.com/config-presets/)
used by the Tweag organization.

1. [ruleset_base.json](ruleset_base.json): This preset contains the settings that are useful
   for maintaining a Bazel ruleset. Renovate will ignore the root `MODULE.bazel` file, but will 
   offer updates for any `MODULE.bazel` files found in sub-directories.
1. [mergify.json](mergify.json): This preset contains the settings to automatically merge the
   Renovate pull requests.

## Usage

To configure a repository to use the base ruleset preset and the Mergify preset, add the following
in [the Renovate JSON file](https://docs.renovatebot.com/configuration-options/) for the target
repository.

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>tweag/renovate-presets:ruleset_base",
    "github>tweag/renovate-presets:mergify"
  ]
}
```

## Ruleset Base Preset

This preset contains all of the presets defined in Renovate's
[config:base](https://github.com/renovatebot/renovate/blob/main/lib/config/presets/internal/config.ts),
except the `:ignoreModulesAndTests` preset. It also contains a package rule that disables Renovate
for the root `MODULE.bazel`.

The
[:ignoreModulesAndTests](https://github.com/renovatebot/renovate/blob/main/lib/config/presets/internal/default.ts#L291)
preset is excluded because it defines `ignorePaths` that prevent the discovery of `MODULE.bazel`
files in select sub-directories (e.g. `examples`, `test`, `tests`).

