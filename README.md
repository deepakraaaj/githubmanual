
# 🔐 SSH User Manual for GitHub and Bitbucket (Multi-Account + Workflow Guide)

---

## 📌 Part 1: Creating and Configuring a Custom SSH Key

### ✅ Generate SSH Key
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
When prompted for file location, enter:
```bash
~/.ssh/personalgithub
```

### ✅ Add to SSH Agent
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/personalgithub
```

### ✅ Add Key to GitHub
```bash
cat ~/.ssh/personalgithub.pub
```
Then go to **GitHub → Settings → SSH and GPG Keys → New SSH Key**, paste it and save.

---

## ✅ SSH Config for Multi-Account

Edit SSH config:
```bash
nano ~/.ssh/config
```

Add:
```ini
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/personalgithub
  IdentitiesOnly yes
```

Then use:
```bash
git clone git@github-personal:username/repo.git
```

---

## 📌 Part 2: Switching Repositories & Handling Bitbucket Workflow

### ✅ Change Remote from GitHub to Bitbucket

Check current remote:
```bash
git remote -v
```

Update remote to Bitbucket:
```bash
git remote set-url origin git@bitbucket.org:kritilabs/kl-erp.git
```

Confirm:
```bash
git remote -v
```

---

## ✅ Pull from a different branch (`development`) and handle divergence

```bash
git pull origin development
```

If you see:
```
fatal: Need to specify how to reconcile divergent branches.
```

Resolve it by setting your pull behavior:
```bash
git config pull.rebase false  # Use merge (recommended)
```

If Git refuses to merge:
```
fatal: refusing to merge unrelated histories
```

Force sync:
```bash
git fetch origin development
git reset --hard origin/development
```

---

## ✅ Handle Untracked Files and Commit Your Work

Check untracked files:
```bash
git status
```

If new files exist, switch to a new branch:
```bash
git checkout -b report-updates
```

Then add and commit:
```bash
git add .
git commit -m "Time Spent report"
```

---

## ✅ Push New Branch to Bitbucket

If this is the first push:
```bash
git push --set-upstream origin report-updates
```

This command:
- Pushes your new branch
- Links it to `origin/report-updates`
- Generates a Bitbucket pull request link, like:
  ```
  https://bitbucket.org/kritilabs/kl-erp/pull-requests/new?source=report-updates
  ```

---

## 🔁 Common Commands Reference

| Task | Command |
|------|---------|
| List all SSH keys | `ls ~/.ssh` |
| Add SSH key to agent | `ssh-add ~/.ssh/your-key` |
| Configure git pull behavior | `git config pull.rebase false` |
| Change remote URL | `git remote set-url origin <url>` |
| Check branch status | `git status` |
| Force sync with remote branch | `git fetch origin <branch>` + `git reset --hard origin/<branch>` |
| Create and push new branch | `git checkout -b <branch>` + `git push --set-upstream origin <branch>` |
