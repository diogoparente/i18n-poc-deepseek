# Implementation Plan: i18n Automation with DeepSeek & GitHub Actions

This plan outlines the automated translation system using Shell scripts and GitHub Actions, leveraging DeepSeek to update target locale files.

## Architecture Overview
The system is entirely contained within the GitHub Actions workflow, avoiding the need for a dedicated runtime environment or external scripts. It uses standard tools (`curl`, `jq`, `git`) to manage translations.

## Phase 1: GitHub Actions Configuration (`.github/workflows/translate.yml`)
- [x] **Trigger Logic:**
    - [x] Configure `on: push: paths` to trigger the workflow only on changes to `locales/en.json`.
- [x] **Environment Setup:**
    - [x] Use `ubuntu-latest` runner (git, curl, jq pre-installed).
- [x] **Git Configuration:**
    - [x] Setup git user identity. Authentication handled automatically by `actions/checkout` and the built-in `GITHUB_TOKEN`.

## Phase 2: Translation Logic (Shell Script)
- [x] **Context Gathering:**
    - [x] Use `git show HEAD~1:locales/en.json` to get the previous version for diffing context.
    - [x] Load current target locale files (e.g., `pt.json`, `fr.json`) to provide style and existing translation context.
- [x] **DeepSeek Integration:**
    - [x] Construct a structured prompt for DeepSeek acting as a JSON diff and translation engine.
    - [x] Use `jq` to safely escape the prompt and construct the API payload.
    - [x] Call the DeepSeek API via `curl`.
- [x] **Response Handling:**
    - [x] Extract and validate the JSON response using `jq`.
    - [x] Overwrite the target locale files with the updated content.

## Phase 3: Automated Commits
- [x] **Commit Logic:**
    - [x] Detect changes in the `locales/` directory.
    - [x] Commit updated files with `[ci skip]` to prevent pipeline loops.
    - [x] Push changes back to the origin repository.

## Phase 4: Verification & Maintenance
- [ ] **Secrets Management:**
    - [ ] Ensure `DEEPSEEK_API_KEY` is configured in GitHub repository settings (Settings → Secrets and variables → Actions).
- [ ] **Testing:**
    - [ ] Add a new key to `en.json` and verify automated creation of `pt.json` and `fr.json`.
    - [ ] Modify an existing key in `en.json` and verify targeted translation updates.

## Success Criteria
- [ ] Fully automated i18n workflow with zero local dependencies.
- [ ] Accurate translations that respect existing JSON structure and style.
- [ ] Seamless integration with GitHub version control.
