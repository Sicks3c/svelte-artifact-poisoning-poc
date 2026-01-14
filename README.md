# Artifact Poisoning POC

This repository demonstrates a GitHub Actions artifact poisoning vulnerability similar to the one found in sveltejs/svelte.

## Vulnerability

The `post-comment.yml` workflow:
1. Triggers on `workflow_run` completion of `build-artifact.yml`
2. Downloads artifact created by the triggering workflow
3. **Trusts artifact content** to determine which issue/PR to comment on
4. **Trusts artifact content** for the comment body

## Attack Vector

When a PR from a **fork** triggers `build-artifact.yml`:
- The **fork's version** of the workflow runs
- The fork can create a **malicious artifact**
- The `post-comment.yml` (running in main repo context with write permissions) downloads and trusts it

## To Test

1. Fork this repository
2. Modify `.github/workflows/build-artifact.yml` in your fork
3. Open a PR to this repository
4. Observe the comment posted based on your artifact content
