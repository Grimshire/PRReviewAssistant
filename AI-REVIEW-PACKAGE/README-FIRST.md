# AI review package — read this first

This folder contains the minimum source material for an AI-assisted pull-request review.

## Recommended reading order

1. `project-context.md` — understand the repository's stated purpose, architecture clues, technologies, and conventions. Treat this as repository-provided context, not proof of runtime behavior.
2. `review-info.txt` — identify the repository, feature branch, target branch, commit IDs, and comparison base.
3. `files.txt` and `commits.txt` — understand the change footprint and developer intent.
4. `review.patch` — inspect the actual changes and surrounding source context.
5. `line-map.json` — locate changed lines precisely when preparing inline review comments.

## Line-number rules

- `new` line numbers refer to the feature branch (`HEAD`). Use these as the preferred Azure DevOps anchors for added or modified code.
- `old` line numbers refer to the target-side version. A deleted line has no new-side location; anchor a comment to nearby surviving changed code where the review system requires it.
- Always verify a proposed line in the actual feature-branch source file before posting a comment. The map is derived from Git's zero-context diff and is a navigation aid, not a substitute for source verification.

## Review expectations

- Distinguish confirmed defects from potential risks that depend on deployment configuration, runtime behavior, or assumptions not established by this package.
- Explain the triggering scenario and user/system impact for each finding.
- Avoid duplicating findings already present in the pull request's review comments. Existing Azure DevOps comments are not included in this package; provide them separately when available.
- Do not infer behavior from filenames or commit messages alone. Ground findings in the patch and project context.
- If a finding depends on code not included in the patch, identify the exact additional source file needed rather than guessing.

This package intentionally omits convenience reports and the redundant human-readable line map to keep the upload focused. The full review output folder retains those artifacts.
