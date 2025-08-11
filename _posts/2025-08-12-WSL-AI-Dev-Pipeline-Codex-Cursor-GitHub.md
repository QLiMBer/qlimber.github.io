---
layout: post
title: "WSL AI Dev Pipeline (Codex CLI + Cursor + GitHub)"
date: 2025-08-10
categories: [development]
tags: [ai, codex, cursor, github]
---

Use this guide to set up an end‑to‑end WSL AI development pipeline that keeps your repositories in WSL for performance and makes Codex CLI fully functional, while you edit from Cursor on Windows and version in Git from WSL.

- Keep repos on WSL’s ext4 for speed and reliable tooling.
- Run Codex CLI in WSL; authenticate using the WSL workflow below.
- Open and edit the same repo from Cursor on Windows via Remote - WSL.
- Commit, branch, and push from WSL with SSH keys.
- Bonus: Codex + Cursor is a great combo; currently, OpenAI and Cursor are offering GPT‑5 testing in both with separate usage limits.

**Target:** Ubuntu 22.04 in WSL2

---

## **1️⃣ Install core packages**

```bash
# Update apt
sudo apt update && sudo apt upgrade -y

# Remove any old Node/npm
sudo apt remove -y nodejs npm libnode-dev
sudo apt autoremove -y
sudo apt clean

# Install Node 20 LTS from NodeSource
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs git

# Verify
node -v   # should be v20.x
npm -v    # should be v10.x
```

---

## **2️⃣ Install Codex CLI**

```bash
sudo npm install -g @openai/codex
codex --version  # should show codex-cli 0.20.0 or similar
```

### WSL authentication (headless browser) workaround

If `codex login` cannot open a browser inside WSL, use this SSH tunnel method from Windows. See the project notes about Windows/WSL in the Codex CLI repo: [openai/codex](https://github.com/openai/codex).

1. Install and start SSH server in WSL (Ubuntu):

   ```bash
   sudo apt install -y openssh-server
   sudo service ssh start
   ```

2. From Windows PowerShell or Command Prompt, forward port 1455 to WSL and keep this window open:

   ```powershell
   ssh -L 1455:localhost:1455 your-wsl-username@localhost
   ```

3. In your WSL terminal, run login and follow the URL in your Windows browser:

   ```bash
   codex login
   # Auth server listens on http://localhost:1455 and prints an https://auth.openai.com/... URL
   ```

4. Complete auth in Windows browser. It should redirect to `http://localhost:1455/success`. The WSL terminal will show "Successfully logged in".

5. Verify and clean up:

   ```bash
   codex "print('Hello')"  # quick test
   ```

   - Close the Windows SSH tunnel window when done.
   - Optionally stop SSH in WSL: `sudo service ssh stop` (or remove `openssh-server` if no longer needed).

---

## **3️⃣ Create SSH key for GitHub**

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
# Press Enter for defaults, set a passphrase for safety

# Start ssh-agent automatically
echo 'eval "$(ssh-agent -s)"' >> ~/.bashrc

# Auto-load key on first Git use per session
cat << 'EOF' >> ~/.bashrc

# Auto-load SSH key when Git is first used
git() {
    ssh-add -l >/dev/null 2>&1 || ssh-add ~/.ssh/id_ed25519
    command git "$@"
}
EOF
source ~/.bashrc

# Show public key to add to GitHub
cat ~/.ssh/id_ed25519.pub
```

**→ Copy & paste** into GitHub:
*Settings → SSH and GPG keys → New SSH key*

---

## **4️⃣ Configure SSH for GitHub**

```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh
nano ~/.ssh/config
```

Paste:

```sshconfig
Host *
    ForwardAgent no
    IdentitiesOnly yes

Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
```

```bash
chmod 600 ~/.ssh/config
```

---

## **5️⃣ Clone repos into WSL filesystem**

```bash
mkdir -p ~/projects
cd ~/projects
git clone git@github.com:YourUser/your-repo.git
```

✅ Keeps everything on ext4 for speed and avoids CRLF issues.

---

## **6️⃣ Use VS Code/Cursor in WSL mode**

- Install **Remote - WSL** extension.
- From WSL terminal:

  ```bash
  code ~/projects/your-repo
  ```

  *(Replace `code` with `cursor` if available)*
- Status bar should show: **\[WSL: Ubuntu]**

---

## **📅 First-use checklist after reboot**

```bash
# 1. Open WSL terminal (ssh-agent starts automatically from .bashrc)
# 2. Run any git command (first use will prompt for passphrase)
```

After that, GitHub SSH will work without asking again until you close WSL or reboot.

---

Now you’ve got:

- Latest Node.js + npm
- Codex CLI
- Fast, secure GitHub SSH access with 1 passphrase per session
- Projects on WSL’s native filesystem
- Seamless IDE integration
