# Plugin Marketplace Maintenance

## Adding a Native Skill

1. Create the skill directory under `skills/<category>/<skill-name>/`
2. Add the skill entry to `sources.yaml`
3. Update `AGENTS.md` with the skill description and usage
4. Commit and open a PR

## Syncing External Skills

External skills are synced automatically via `.github/workflows/sync.yaml`.
To trigger a manual sync: `npm run sync`
