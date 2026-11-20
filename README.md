PR REVIEW ASSISTANT v2.1 — NATIVE GIT BASH
==========================================

WHAT'S NEW
----------
1. First-run project discovery creates a source-grounded project-context.md.
   It captures README and repository instructions, common dependency/build
   manifests, selected architecture/API documentation, and a shallow
   directory map. Sensitive .env files are intentionally excluded.
2. Project context is stored outside the repository and reused until one of
   the discovered source files or the directory structure changes.
3. Every PR package includes line-map.txt and line-map.json with exact old-side
   and new-side line numbers derived from Git's zero-context diff.
4. REVIEW-START-HERE.md explains the complete review output and line references.
5. diff-check.txt records the result of git diff --check.
6. AI-REVIEW-PACKAGE/ contains a focused set of files for upload to an AI:
   review.patch, line-map.json, project-context.md, review-info.txt, files.txt,
   commits.txt, and README-FIRST.md.
7. AI-REVIEW-PACKAGE.zip packages that folder for one-step upload.

INSTALL / UPGRADE
-----------------
The v2.0.0 VS Code extension can continue to launch this standalone script.
If you use the v2.1.0 VSIX, it includes this script automatically.

1. To use the standalone script, replace your existing Generate-PRReview.sh.
2. Confirm Git Bash, Git, and Python 3 are available.
3. In VS Code, run Developer: Reload Window, then click PR Review.

DEFAULT PATHS
-------------
Project scan: ~/Documents/Projects/cloned
Output:       ~/Documents/Projects/PR-Review-Output

Optional environment overrides:
  PR_REVIEW_PROJECT_ROOT
  PR_REVIEW_OUTPUT_ROOT
  PR_REVIEW_MAX_REPO_DEPTH
  PR_REVIEW_MAX_CONTEXT_FILES
  PR_REVIEW_MAX_CONTEXT_FILE_BYTES
  PR_REVIEW_MAX_CONTEXT_TOTAL_BYTES

OUTPUT LAYOUT
-------------
PR-Review-Output/<repository>/<feature-branch>/
  REVIEW-START-HERE.md
  project-context.md
  project-context-sources.txt
  review-info.txt
  summary.txt
  commits.txt
  files.txt
  review.patch
  line-map.patch
  line-map.txt
  line-map.json
  diff-check.txt
  AI-REVIEW-PACKAGE/
    README-FIRST.md
    review.patch
    line-map.json
    project-context.md
    review-info.txt
    files.txt
    commits.txt
  AI-REVIEW-PACKAGE.zip

AI REVIEW WORKFLOW
------------------
Upload AI-REVIEW-PACKAGE.zip to the AI assistant. The folder is also available
for uploading individual files if needed. The package README gives the reading
order and explains line-number conventions. Existing Azure DevOps comments are
not captured automatically; provide screenshots or their text separately to
avoid duplicate findings.

LINE NUMBERS
------------
- `new` line numbers refer to the feature branch (HEAD); use these for comments
  on added or modified code.
- `old` line numbers refer to the target-side file. Deleted lines have no new
  side line number; anchor comments to nearby surviving changed code.
- Verify the selected line in the actual feature-branch source before posting.

NOTES
-----
- Comparison remains origin/<target>...HEAD (merge-base semantics).
- Uncommitted working-tree changes are excluded and reported as a warning.
- Review output is written outside the repository so it does not dirty the PR.
- Project context is a curated source snapshot, not an AI-generated summary.
