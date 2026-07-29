# Codex workflows

Reusable GitHub Actions workflows that turn labeled issues into PRs and respond to reviews requesting changes.

## Contents

- `solve-labeled-issue.yml`: implements an open issue when it receives a label. It creates or updates a draft PR and reuses an existing branch if the PR failed.
- `address-requested-review.yml`: responds only to a formal GitHub review with the **Request changes** status. It reads inline comments, modifies the PR branch, and leaves a response.
- `examples/`: minimal dispatcher workflows to copy into each consuming repository.

## Publish this repository

1. Create a private repository named `autonomous-prs` in your GitHub account.
2. Upload the contents of this folder.
3. In the central repository, go to **Settings → Actions → General** and allow private repositories in your account to access its reusable workflows.
4. Create and publish a version tag, initially `v1`, then publish a new version tag for every workflow change.

The central repository does not need `OPENAI_API_KEY`.

## Configure a consuming repository

1. In the consuming repository, add the `OPENAI_API_KEY` Actions secret.
2. In **Settings → Actions → General → Workflow permissions**, allow GitHub Actions to create pull requests.
3. Copy one or both files from `examples/` to `.github/workflows/` in the consuming repository.
4. Copy `examples/ISSUE_TEMPLATE/autonomous-prs-task.yml` to `.github/ISSUE_TEMPLATE/` to give product owners an
   English task form that applies `ready-for-ai` automatically. Create the `ready-for-ai` label in the repository first.
5. Replace `marcomilon/autonomous-prs@v5` if you use a different owner, repository, or version.

The dispatcher workflows pass only the API key required by the workflow. The `GITHUB_TOKEN` retains the consuming repository's permissions; those permissions are declared in each dispatcher.
Each workflow run is capped at 20 minutes.

## Usage

- Add the `ready-for-ai` label to an open issue to trigger `solve-labeled-issue`. The workflow adds
  `ai-in-progress`, `needs-clarification`, and `ready-for-review` as it progresses.
- When the workflow requests clarification, a repository owner, member, or collaborator can reply with
  `/autonomous retry` to run it again.
- For a PR originating from the same repository, submit a **Request changes** review to trigger `address-requested-review`.

The workflows never merge. The review workflow ignores approvals and regular comments to avoid unnecessary API usage.
