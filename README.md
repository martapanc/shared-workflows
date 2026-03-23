# Shared Workflows

Add `.github/workflows/release.yml`:
```yaml
on:
  push:
    branches: [master]

jobs:
  release:
    uses: martapanc/shared-workflows/.github/workflows/release-please.yml@master
    with:
      release-type: node  # or "simple", "python", etc.
```

For more control, add a `release-please-config.json` at the root:

```json
{
  "release-type": "node",
  "changelog-sections": [
    { "type": "feat", "section": "✨ Features" },
    { "type": "fix", "section": "🐛 Bug Fixes" },
    { "type": "docs", "section": "📖 Documentation" },
    { "type": "chore", "hidden": true }
  ],
  "bump-minor-pre-major": true
}
```

And a `.release-please-manifest.json`:

```json
{ ".": "0.1.0" }
```

### Commit conventions cheatsheet

| Prefix                   | Effect             | Example                    |
|--------------------------|--------------------|----------------------------|
| feat:                    | minor bump         | feat: add dark mode        |
| fix:                     | patch bump         | fix: broken logo on mobile |
| feat!: / BREAKING CHANGE | major bump         | feat!: new config format   |
| docs:, chore:, refactor: | no bump (can hide) | chore: update deps         |

### Optional: enforce commit style locally

Add commitlint + husky to each project so you catch bad commit messages before they reach CI:

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional husky
npx husky init
echo "npx --no -- commitlint --edit \$1" > .husky/commit-msg
```

`commitlint.config.js`:

```js
export default { extends: ['@commitlint/config-conventional'] };
```