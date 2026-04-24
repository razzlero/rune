# Working in the claude-code plugin

This directory is the Claude Code plugin side of rune. Rules here apply only to the plugin, not to the pi package or other parts of the repo.

## Bump the plugin version when skills change

This is a personal-use plugin updated frequently — keep version bumping low-effort:

- **Patch bump only by default** (e.g. 0.1.0 → 0.1.1) for any skill/primer/config change.
- **Bump all three versions together** — `.claude-plugin/plugin.json` (`version`) and `.claude-plugin/marketplace.json` (both `metadata.version` and `plugins[0].version`) should stay in sync.
- **Skip the bump if the version is already changed uncommitted** — one bump per commit is plenty; don't stack them.
- **Leave minor bumps to the user.** Don't promote to a minor (e.g. 0.1.x → 0.2.0) unless explicitly asked.
