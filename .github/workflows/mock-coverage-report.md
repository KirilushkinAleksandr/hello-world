---
description: Adds a mock test coverage report commit to pull requests, triggered manually or automatically on PR creation
on:
  pull_request:
    types: [opened]
  workflow_dispatch:
    inputs:
      pr_number:
        description: 'Pull request number to add the coverage report to'
        required: true
        type: string
permissions:
  contents: read
  pull-requests: read
  issues: read
tools:
  github:
    toolsets: [default]
safe-outputs:
  push-to-pull-request-branch:
    max: 1
    target: "*"
  add-comment:
    max: 1
---

# Mock Test Coverage Report

You are an AI agent that adds a mock test coverage report to pull requests. This helps demonstrate coverage tracking and provides a starting template for real coverage integration.

## Your Task

1. **Identify the target PR**:
   - If triggered by `pull_request` event: use the current PR (`${{ github.event.pull_request.number }}`)
   - If triggered by `workflow_dispatch`: use the `pr_number` input (`${{ inputs.pr_number }}`)

2. **Generate a mock test coverage report** at `coverage/COVERAGE_REPORT.md` with realistic-looking data:

   ```markdown
   # Test Coverage Report

   **PR:** #<pr-number>
   **Generated:** <current-date-time>
   **Branch:** <pr-branch-name>

   ## Summary

   | Metric | Coverage | Change |
   |--------|----------|--------|
   | Lines | 87.3% | +1.2% |
   | Branches | 78.5% | +0.8% |
   | Functions | 92.1% | +2.5% |
   | Statements | 86.9% | +1.4% |

   ## File Breakdown

   | File | Lines | Branches | Functions |
   |------|-------|----------|-----------|
   | src/main.js | 95.2% | 88.0% | 100% |
   | src/utils.js | 82.1% | 71.4% | 90.0% |
   | src/api.js | 78.5% | 65.0% | 85.7% |

   ## Uncovered Lines

   - `src/utils.js`: Lines 42-47 (error handling branches)
   - `src/api.js`: Lines 103-110 (timeout retry logic)

   ## Recommendations

   - [ ] Add tests for error handling in `src/utils.js`
   - [ ] Cover timeout edge cases in `src/api.js`
   - [ ] Maintain coverage above 85% threshold

   ---
   *This is a mock report for demonstration purposes.*
   ```

3. **Commit the report to the PR branch** using the `push-to-pull-request-branch` safe output:
   - Target the PR's head branch
   - Commit message: `Add mock test coverage report for PR #<pr-number>`
   - File path: `coverage/COVERAGE_REPORT.md`

4. **Add a comment to the PR** using the `add-comment` safe output, summarizing what was added:
   - Mention coverage metrics
   - Link to the new file
   - Note that this is a mock report

## Guidelines

- **Randomize values slightly** to make each report unique (vary coverage percentages between 75-95%)
- **Use current timestamp** in the report
- **Keep formatting consistent** with GitHub-flavored markdown
- **Be transparent** that this is a mock report for demonstration

## Safe Outputs

When you complete the task:

1. Use `push-to-pull-request-branch` to add the coverage report file to the PR branch
2. Use `add-comment` to post a summary on the PR
3. If you cannot determine the target PR or any error occurs, do NOT push or comment — explain the issue in your output

## Important Notes

- This is a **demonstration workflow** — the data is synthetic
- The workflow will fail gracefully if the PR cannot be found
- All commits are signed by GitHub Actions to maintain audit trail
