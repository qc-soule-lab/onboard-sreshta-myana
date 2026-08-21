# Orientation — Sreshta Myana

**Goal:** bring Sreshta Myana's environment up to the lab standard and get them working in their project repo.

- Project repo: **https://github.com/qc-soule-lab/neftali-kratc-project**
- Azure container (their own space): **timeseries-sreshta**
- GitHub org: **qc-soule-lab**

Guide the member through these steps **one at a time**, confirming each before the next.

### 1. Confirm they're authenticated
They launched `claude`, so their `qc-soule-lab` Team seat is working. Have them run `/status` — it should show the Team account. If they hit an auth error or a login prompt that won't complete (the Hub's localhost browser callback often can't connect), the fix is a token: quit, run `claude setup-token` in a terminal, approve in the browser, `export CLAUDE_CODE_OAUTH_TOKEN=<token>`, then relaunch `claude`. If `/status` shows no seat at all, their Team invite isn't active yet — stop and have Dr. Soule check.

### 2. Clone the project repo and enter it
```bash
cd ~ && git clone https://github.com/qc-soule-lab/neftali-kratc-project
cd "$(basename https://github.com/qc-soule-lab/neftali-kratc-project .git)"
```

### 3. Run the lab bootstrap (from inside the project repo)
This installs uv, the lab Claude config (skills incl. `ethical-check`, hooks, the global rules), SpecKit, the project's Python deps (`uv sync`), a Jupyter kernel, and the `azure_lake` CLI:
```bash
git clone https://github.com/qc-soule-lab/claude-config.git ~/repos/claude-config 2>/dev/null || true
bash ~/repos/claude-config/bootstrap-student.sh
```
Explain what it's doing as it runs; it may take a few minutes on first sync.

### 4. Set up Azure access
Dr. Soule securely sends them the SAS for **their own** container `timeseries-sreshta` (never anyone else's — per-person isolation). Newcomers hit several traps saving it, so use this **exact** method:

1. **Make the directory FIRST** — editors/`nano` fail to save if it doesn't exist (bootstrap also creates it, but confirm):
   ```bash
   mkdir -p ~/.azure && chmod 700 ~/.azure
   ```
2. **Create the file with `nano`** — NOT the JupyterLab "Save As" dialog (it can't write hidden dotfolders like `.azure`), and NOT a pasted `read -s … && …` one-liner (a stray newline in the paste breaks it and can echo the secret):
   ```bash
   nano ~/.azure/timeseries-sreshta.env
   ```
   In nano, make **one line** — type the wrapper and paste the URL **between** the quotes:
   ```
   export AZURE_BLOB_SAS_URL='<paste the SAS URL here>'
   ```
   Paste in a browser terminal = **Ctrl+Shift+V** (or right-click → Paste, or **Cmd+V** on Mac). Save = **Ctrl-O, Enter**; exit = **Ctrl-X**.
3. **Lock down + verify:**
   ```bash
   chmod 600 ~/.azure/timeseries-sreshta.env
   source ~/.azure/timeseries-sreshta.env && azure_lake info && azure_lake ls && echo OK
   ```
   `azure_lake info` should show container `timeseries-sreshta` + `racwdl` + expiry; `ls` empty + `OK` confirms it reaches Azure. If `info` errors, the SAS got mangled on paste — redo step 2.

**Never let the SAS land in chat, a commit, or shared output.** It showing in their own `nano`/terminal while saving is fine; if it appears anywhere shared, tell Dr. Soule to rotate it. (Common newbie slip: typing the file *path* alone at the prompt → "permission denied" because bash tries to *run* the file — they need to `nano`/open it, not execute it.)

### 5. Verify SpecKit
```bash
science-specify check
```
If the project repo isn't SpecKit-scaffolded and should be, run `science-specify init` (ask first).

### 6. Walk them through the rules
Open the project repo's `CLAUDE.md` and summarize the non-negotiables in plain language:
- Run the **`ethical-check`** skill before new data/literature, prose for humans, or committing/sharing.
- **Never commit** secrets (SAS URLs, keys) or raw data the lab doesn't own.
- **Branch + PR** — never push to `main`; `uv run pytest` before every commit.
- Big/derived data → **Azure via `azure_lake`** (not git); small tables/figures/scripts → git.
- Shared host: keep parallel jobs **≤24 workers**.

### 7. First branch → PR (prove the workflow)
Help them create a throwaway branch, make a trivial change, commit (pytest first), push, and open a PR — so they've done one full cycle before real work.

**End state:** bootstrapped env, project repo cloned, `azure_lake info` works, `science-specify check` passes, they understand the rules, and they've completed one branch→PR.

### 8. Hand off to the onramp (the science curriculum)
Before project work, they take the lab's guided curriculum — "From the map to the tidal signal":
```bash
cd ~ && git clone -b 001-coding-onramp-v1 https://github.com/qc-soule-lab/student-onramp.git
cd student-onramp && uv sync && claude
```
In that Claude session: `/model claude-sonnet-4-6`, then type **`assess me`**. It assesses, builds
them a personal plan, and teaches the slice (it saves progress — `continue` resumes any time).
When the onramp's bridge note lands, they're ready for real work in the **project repo**.
