# Welcome to qc-soule-lab — onboarding for Sreshta Myana

This little repo gets your computing environment set up.

## Step 0 — one-time setup (do this first)

Your JupyterHub container may not have these yet. In a Hub terminal:

**a) GitHub access** (needed even to clone — these repos are private):

    gh auth login            # GitHub.com → HTTPS → login with a web browser (device code)

If `gh` is **not installed** ("command not found"), use a token instead:

    git config --global credential.helper store
    # When a later `git clone` asks for a password, paste a GitHub Personal Access Token
    # (github.com → Settings → Developer settings → Personal access tokens (classic) → scope: repo).
    # Your username is your GitHub login; the "password" is the token.

**b) Claude Code installed:**

    claude --version         # if "command not found":
    curl -fsSL https://claude.ai/install.sh | bash
    export PATH="$HOME/.local/bin:$PATH"

## Then — three steps

1. **Clone this repo:**

       git clone https://github.com/qc-soule-lab/onboard-sreshta-myana.git
       cd onboard-sreshta-myana

2. **Launch Claude Code:**

       claude

   First launch asks you to log in. **If the browser login hangs** (common on the Hub), quit it and run `claude setup-token` instead — approve in your browser, paste the token back, then `claude` again. `/status` should show your Team account.

3. **Type exactly this:**

       Onboard me — read ORIENTATION.md and walk me through it one step at a time.

Claude guides the rest (tools, your project repo, Azure access, the lab rules), pausing after each step so you can keep up.

**Have ready:** the Azure credential (SAS) Dr. Soule will send you securely. If anything looks wrong, stop and message Dr. Soule.
