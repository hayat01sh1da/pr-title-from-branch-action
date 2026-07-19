# PR Title from Branch

[![Action - CI](https://github.com/hayat01sh1da/pr-title-from-branch-action/workflows/Action%20-%20CI/badge.svg)](https://github.com/hayat01sh1da/pr-title-from-branch-action/actions/workflows/action--ci.yml)

Set a pull request's title and labels derived from the topic branch name: open a PR from `{user}/{issue}/{category}/{summary}` and the title becomes `[category] Summary In Title Case`, with `{category}` (and `Hotfix`) available as labels.  
This action is a thin adapter over the [`spreen-pr`](https://pypi.org/project/spreen-pr/) PyPI package (also published [as a RubyGem](https://rubygems.org/gems/spreen-pr)): it runs the `pr-title` CLI on the head branch and applies the result to the pull request via the API.

## 1. Usage

```yaml
name: PR Title

on:
  pull_request:
    types: [opened]

permissions:
  pull-requests: write # required to set the title and add labels

jobs:
  pr-title:
    timeout-minutes: 5
    runs-on: ubuntu-latest
    steps:
      - uses: hayat01sh1da/pr-title-from-branch-action@v0.1.0
```

To also apply the labels (creating missing ones in the repository):

```yaml
permissions:
  pull-requests: write
  issues: write # required by create-label

# ...
      - uses: hayat01sh1da/pr-title-from-branch-action@v0.1.0
        with:
          apply-label: 'true'
          create-label: 'true'
```

The expected branch shape is `{user}/{issue}/{category}/{summary}` with the hotfix special case `{user}/{issue}/hotfix/{category}/{summary}`; the `{summary}` may be snake_case or kebab-case.  
Branches that do not match the shape fail the step with a clear error instead of producing a broken title — see the [spreen-pr README](https://github.com/hayat01sh1da/spreen-pr#3-branch-name-convention) for the full convention.

| Branch Name                                             | PR Title                             | Labels             |
|---------------------------------------------------------|--------------------------------------|--------------------|
| `hayat01sh1da/issue-89/service/improve-onboarding-flow` | `[service] Improve Onboarding Flow`  | `service`          |
| `hayat01sh1da/issue-90/hotfix/service/fix_login_crash`  | `[Hotfix][service] Fix Login Crash`  | `Hotfix`, `service` |

## 2. Inputs

| Input          | Default                          | Description                                                                                            |
|----------------|----------------------------------|--------------------------------------------------------------------------------------------------------|
| `branch-name`  | (the pull request's head branch) | Branch name to derive from. Mainly for compute-only usage outside `pull_request` events.               |
| `github-token` | `github.token`                   | Token used to update the pull request. See § 4.                                                        |
| `apply-title`  | `true`                           | Apply the derived title to the pull request. Set `false` for compute-only (outputs-driven) usage.      |
| `apply-label`  | `false`                          | Apply the derived labels to the pull request.                                                          |
| `create-label` | `false`                          | `apply-label` only: create labels missing from the repository instead of skipping them with a warning. |
| `overwrite`    | `false`                          | Apply the title on any `pull_request` event, not only `opened` — beware this clobbers manually edited titles on `synchronize` runs. |

## 3. Outputs

| Output   | Description                                                             |
|----------|-------------------------------------------------------------------------|
| `title`  | The derived pull request title, e.g. `[service] Improve Onboarding Flow`. |
| `labels` | The derived labels as a JSON array, e.g. `["Hotfix","service"]`.          |

## 4. Token & Permissions

- **Applying the title and existing labels (default)**: the built-in `GITHUB_TOKEN` works as long as the job declares `permissions: pull-requests: write` — no PAT needed.
- **Creating missing labels (`create-label: 'true'`)**: additionally declare `permissions: issues: write` (repository labels are managed through the Issues API).
- The title and labels are only applied on `pull_request` events (the `opened` action unless `overwrite` is `'true'`); on other events the action is compute-only and exposes the outputs.
- The action never prints the token; keep PATs in secrets.

## 5. Versioning

- Pin the exact tag (`@v0.1.0`) or a commit SHA for now; a floating major (`@v1`) arrives with `v1.0.0` after validation on a real pull request, and from then on pinning `@v1` receives fixes automatically.
- Each action release pins an exact `spreen-pr` package version internally, so existing tags keep their behaviour; package upgrades arrive via new action releases.

## 6. Development

The title/label derivation itself lives in [hayat01sh1da/spreen-pr](https://github.com/hayat01sh1da/spreen-pr) — file behaviour issues there, and action-specific issues here.  
The publishing plan and design record is [spreen-pr#89](https://github.com/hayat01sh1da/spreen-pr/issues/89).

## 7. License

[MIT](./LICENSE.txt)
