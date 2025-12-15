# Sorbet Error Reproduction

This repository was created to reproduce the Dependabot Sorbet error when updating GHCR images via docker-compose ecosystem.

## Issue Reference
https://github.com/dependabot/dependabot-core/issues/13553

## Reproduction Status
⚠️ **UNABLE TO REPRODUCE** - Dependabot successfully created [PR #1](https://github.com/a-schur/sorbet-error/pull/1) bumping `goauthentik/server` from `2025.10.0` to `2025.10.2` without any Sorbet errors.

## Original Problem Description
The issue reported that Dependabot crashes with an internal Sorbet runtime error during the UpdateAllVersions / GroupUpdateAllVersions stage when trying to update the `ghcr.io/goauthentik/server` Docker image.

## Files

- `.github/dependabot.yml` - Dependabot configuration
- `compose.yaml` - Docker Compose file with the image reference

## Expected Behavior
Dependabot should successfully evaluate available image tags and either:
- Open a PR with the updated tag, or
- Determine that no update is required

## Actual Behavior
Dependabot crashes with a Sorbet runtime error:
```
ERROR <job_xxx> /home/dependabot/dependabot-updater/vendor/ruby/3.4.0/gems/sorbet-runtime-0.6.12544/lib/types/private/methods/call_validation_2_7.rb:687:in 'block in Dependabot::Updater::Operations::UpdateAllVersions#create_validator_procedure_fast1'
```

All network calls to GHCR succeed (manifest lookups, tags list, token retrieval), but Dependabot fails after retrieving metadata.

## How to Reproduce

### Option 1: On GitHub
1. Create a GitHub repository with these files
2. Enable Dependabot on the repository
3. Wait for Dependabot to run (or trigger it manually)
4. Observe the Sorbet runtime error in the Dependabot logs

### Option 2: Locally with dependabot-cli
1. Clone the [dependabot-cli](https://github.com/dependabot/cli) repository
2. Set your GitHub token:
   ```bash
   export LOCAL_GITHUB_ACCESS_TOKEN=<your-github-token>
   ```
3. From the dependabot-cli directory, run:
   ```bash
   script/dependabot update docker a-schur/sorbet-error --debug
   ```

The error occurs when Dependabot attempts to update the `ghcr.io/goauthentik/server` image from version `2025.10.0`.

**Note**: The package ecosystem is `docker` (for docker-compose files), not composer.