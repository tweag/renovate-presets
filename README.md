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

### Why are updates to the root `MODULE.bazel` ignored?

With Bazel modules enabled (bzlmod), Bazel will evaluate the requirements for the workspace's
transitive dependencies selecting the latest compatible version for each Bazel module. For direct
dependencies of the client's workspace, this can lead to a version being selected that is more 
recent than the client has specified. This results in a warning message. It could also lead to
unexpected behavior for the client. 

To minimize the risks from this type of upgrade, Bazel rulesets are encouraged to maintain the
lowest viable version for their external dependencies. As a corollary, client workspaces are
encouraged to maintain the highest viable version for their external dependencies.

For additional details, please see [this discussion](https://github.com/bazel-contrib/SIG-rules-authors/discussions/82).
