# renovate-config

Account-wide Renovate inheritance config and shared preset for `corrupt952`.

## What this repo does

The Mend-hosted Renovate App auto-discovers `<owner>/renovate-config/org-inherited-config.json` and applies it to every Renovate run across the account — no per-repo setup required.

This repo also hosts the shared `default.json5` preset that every onboarded repo extends.

```
       Mend                       this repo                       each repo
  ┌────────────┐            ┌─────────────────────┐         ┌──────────────────────┐
  │            │            │ org-inherited-      │         │                      │
  │  Renovate  │ ──reads──▶ │   config.json       │         │  .github/            │
  │  App       │            │                     │         │    renovate.json5    │
  │  (hosted)  │            │ default.json5       │◀extends─│                      │
  │            │            │  (shared preset)    │         │                      │
  └─────┬──────┘            └─────────────────────┘         └──────────┬───────────┘
        │                                                              ▲
        └──── onboards (opens initial Renovate onboarding PR) ─────────┘
```

When onboarded, each repo's `.github/renovate.json5` is auto-generated with:

```json5
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>corrupt952/renovate-config:default.json5"],
}
```

Changes to `default.json5` are picked up on the next Renovate run on each repo (no per-repo action required).

## Conventions (Mend-hosted, non-negotiable)

- Repo name must be `renovate-config`
- File name must be `org-inherited-config.json`
- This repo does not need a Renovate onboarding PR
- The Renovate App must still be installed on this repo (otherwise Mend cannot read it)

## Notes

- `corrupt952` is a GitHub User account, not an Org, but Renovate's `parentOrg` resolution is a plain path split (`lib/workers/global/index.ts`), so `<owner>/renovate-config` resolves the same way regardless of account type.
- Onboarding auto-detection only looks for `default.json` / `renovate.json` (`lib/config/presets/util.ts`), not `default.json5`. Since this repo uses `default.json5`, the reference must stay explicit via `onboardingConfig.extends` above.

## References

- [Mend-hosted Apps Configuration](https://docs.mend.io/integrations/renovate/getting-started-with-mend-hosted-renovate-app)
- [Renovate Configuration Overview](https://docs.renovatebot.com/configuration-options/)
- [Inherited Config Schema](https://docs.renovatebot.com/renovate-inherited-schema.json)
- [`config:best-practices` preset](https://docs.renovatebot.com/presets-config/#configbest-practices)
