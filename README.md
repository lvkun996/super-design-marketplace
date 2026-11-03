# Super UI Design Marketplace

This repository distributes the `super-ui-design` Codex plugin. It includes `super-ui-design` for general product UI work, `super-ios-ui-design` for compact iOS-inspired mobile pages with continuous flat content, `super-ios-glass-tabbar` for floating translucent mobile navigation, and `super-codex-standard-skill` for personal development and delivery constraints.

## Included personal skills

The plugin bundles all seven custom skills from the local personal skills directory, together with the existing `super-ui-design` skill:

- `apple-design`: Apple-style web interfaces and fluid interactions.
- `stardew-style-image`: Stardew-style pixel image prompts.
- `super-codex-standard-skill`: Personal development and delivery standards; deployment prioritizes the project's own custom scripts.
- `super-download-wecom-card`: Super Download WeCom video-card continuation, with its historical checkpoint reference.
- `super-ios-glass-tabbar`: Floating translucent mobile navigation.
- `super-ios-ui-design`: Compact iOS-inspired mobile pages.
- `super-note-ui-design`: Super Note desktop interface design.

## Personal development standard

Invoke `$super-codex-standard-skill` to apply the project standard: avoid redundant builds, tests and verification; use uni-app + Vue 3 + TypeScript for mini-programs, React + antd for Admin, and Vue 3 + TypeScript with the bundled iOS design skills for H5. Reuse existing build/deployment scripts, supply one-click scripts for non-mini-program projects when missing, and reference the established Spring subservice architecture. Deployment scripts may configure usernames and passwords in plaintext; published templates must contain placeholders rather than real credentials. Creating a deployment script does not itself authorize deploying a project.

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
