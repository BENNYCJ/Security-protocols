# **GitHub Security & Operations Protocol (Phase 1)**
**Authored by:** CISO - Benedict Obiomachukwu Amadi <br>
**Status:** Active <br>
**Scope:** All Developers & Contributors

---

## I. Core Philosophy (The "Golden Rules")
1.  **`main` is Sacred:** The `main` branch represents Production code. It must always be stable and deployment-ready.
2.  **No Direct Pushes:** No one—not even the Administrator—pushes code directly to `main`.
3.  **The Parallel Universe:** All work happens in temporary "Feature Branches."
4.  **Four-Eyes Principle:** No code enters `main` without a Pull Request (PR) and at least one approval.

---

## II. The Security Infrastructure (One-Time Setup)

These controls are "set and forget" but must be verified for every new repository.

### 1. The Shield (`.gitignore`)
**Purpose:** Prevents secret keys from entering the history.
**Rule:** Every repository must contain a `.gitignore` file at the root with these entries minimum:


 ```text
*.json      # Blocks Google Cloud Service Keys
*.skey      # Blocks Cardano Signing Keys
.env        # Blocks local environment variables (DB passwords)
node_modules/
dist/
 ```
### 2. The Lock (Branch Protection Rules)
**Purpose:** Enforces the "No Direct Push" policy via software.
**Location:** Settings > Branches > Add Rule.
**Pattern:** main
**Mandatory Settings:**

- [x] Require a pull request before merging.
- [x] Require approvals (Min: 1).
- [x] Require status checks to pass.
- [x] Do not allow bypassing the above settings (Crucial for Admins).

## III. The Daily Workflow (The "Professional Cycle")
Every task follows this exact 5-step sequence.

Step 1: Sync & Branch
Always start with the latest version of the code.

```bash
  git checkout main
  git pull                            # Download updates from cloud
  git checkout -b feature/task-name   # Create your sandbox
```
Naming Convention: Use `feature/` for new stuff, `fix/` for bugs, `security/` for audit findings.

Step 2: The Save Sequence
Saving in Git is a 3-part process:

  1. Stage (The Envelope): Select files to save.
  ```bash
    git add .
  ```
  2. Commit (The Stamp): Seal the envelope with a message.
  ```bash
    git commit -m "Action-oriented message describing the change"
  ```
  3. Push (The Delivery): Send to the cloud.
  ```bash
    git push -u origin feaure/task-name
  ```
Step 3: The Review (Pull Request)
Go to GitHub.com.

Open a Pull Request from your branch to main.

## IV. Emergency Protocols
**Scenario A:** "I accidentally committed to main locally."
**Symptom:** You typed git commit while on (main) and now you can't push because of Branch Protection. The Fix (The Rescue Mission):

**Teleport:** Move your accidental work to a legal branch.

**Self-Audit:** Check the "Files Changed" tab for accidental keys or bad code before asking others to review.

Wait for approval and Merge.
  ```bash
     git checkout -b feature/saved-work
     git push -u origin feature/saved-work
  ```
Reset: Clean your local main to match the server.
  ```bash
    git checkout main
    git reset --hard origin/main
  ```

Scenario B: "I suspect I leaked a key."
Symptom: You realized `key.json` was not in `.gitignore.`

Do NOT just delete the file. It is still in the history.

Action: Rotate the key immediately (Generate a new one in GCP/Cardano).

Action: The old key is now considered burned/compromised.

## V. **Command Cheat Sheet**

| Command | Plain English Translation |
| :--- | :--- |
| `git status` | "Where am I?" Always run this first. Shows tracked/untracked files. |
| `git clone [url]` | "Download." Gets the repo from the cloud. |
| `git checkout -b [name]` | "New Workspace." Creates and switches to a branch. |
| `git checkout [name]` | "Switch Workspace." Jumps between branches. |
| `git add .` | "Stage." Prepares files for saving. |
| `git commit -m "msg"` | "Save." Records a permanent snapshot. |
| `git push` | "Upload." Sends commits to GitHub. |
| `git pull` | "Update." Downloads new changes from teammates. |

---
<p align="center">
  &copy; 2025 CyberForearm Team. All Rights Reserved. <br>
  Confidential & Proprietary.
</p>

