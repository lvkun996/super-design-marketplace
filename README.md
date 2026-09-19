# Super UI Design Marketplace

This repository distributes the `super-ui-design` Codex plugin. The plugin contains a reusable product UI design skill covering visual direction, spacing, typography, radius, elevation, color, interaction states, dark mode, and responsive behavior.

## Install on another device

After publishing this repository as `lvkun996/super-ui-design-marketplace`, run:

```bash
codex plugin marketplace add lvkun996/super-ui-design-marketplace
codex plugin add super-ui-design@personal
```

Start a new Codex task after installation so the new skill is loaded. Invoke it explicitly with `$super-ui-design`, or let Codex select it for relevant product UI work.

## Publish this local repository

Create an empty GitHub repository named `super-ui-design-marketplace`, then add it as the remote for this directory and push the `main` branch.

## Update across devices

Update the skill in `plugins/super-ui-design/skills/super-ui-design`, bump the plugin version, commit, and push. On each device, run:

```bash
codex plugin marketplace upgrade personal
codex plugin add super-ui-design@personal
```

Then start a new Codex task.
