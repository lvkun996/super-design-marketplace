# Super UI Design Marketplace

This repository distributes the `super-ui-design` Codex plugin. It includes `super-ui-design` for general product UI work, `super-ios-ui-design` for compact iOS-inspired mobile pages with continuous flat content, and `super-ios-glass-tabbar` for floating translucent mobile navigation.

## Install on another device

After publishing this repository as `lvkun996/super-design-marketplace`, run:

```bash
codex plugin marketplace add lvkun996/super-design-marketplace
codex plugin add super-ui-design@personal
```

Start a new Codex task after installation so the new skill is loaded. Invoke `$super-ui-design` for the general style, `$super-ios-ui-design` for the mobile page style, or `$super-ios-glass-tabbar` for tab-bar work. Codex can also select the relevant skill automatically.

## Publish this local repository

The repository is published as `lvkun996/super-design-marketplace` on GitHub.

## Update across devices

Update the relevant skill under `plugins/super-ui-design/skills/`, bump the plugin version, commit, and push. On each device, run:

```bash
codex plugin marketplace upgrade personal
codex plugin add super-ui-design@personal
```

Then start a new Codex task.
