
# 🔒 Enforced Release Workflow: Only Allow `release` → `master` Pull Requests

This repository uses a GitHub Actions workflow that **strictly controls which branches can merge into `master`**.
Only the **`release`** branch is allowed to create Pull Requests targeting `master`.
Any other branch attempting to merge into `master` will be **automatically blocked**.

This ensures a clean, maintainable release cycle and prevents accidental merges into production.

---

## 🚀 Workflow Overview

The GitHub Actions workflow (`.github/workflows/release-to-master.yml`) triggers whenever a Pull Request is opened, edited, synchronized, or reopened **against the `master` branch**.

It contains **two jobs**:

---

### ✅ 1. Allow Valid PRs (`release` ➜ `master`)

If the source branch is **`release`** and the target is **`master`**, the workflow approves the PR:

```yaml
check-source-branch:
  if: github.base_ref == 'master' && github.head_ref == 'release'
  runs-on: ubuntu-latest
  steps:
    - name: "Confirm release to master PR"
      run: echo "This is a valid release ➜ master pull request."
```

✔ This means the PR is allowed and CI continues normally.

---

### ❌ 2. Block Invalid PRs (any-other-branch ➜ `master`)

If any branch **other than `release`** attempts to merge into `master`, GitHub Actions will **fail the workflow**, blocking the
