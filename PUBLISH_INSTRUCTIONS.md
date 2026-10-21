# Publish ECS Custom Secure Websites to GitHub

The ZIP is structured with `README.md` at the top level. Extract **into** the existing local folder, not beside it.

## 1. Extract the completed repository package

```bash
mkdir -p ~/Downloads/GitHub/custom-secure-websites
cd ~/Downloads/GitHub/custom-secure-websites
unzip -o ~/Downloads/ECS_Custom_Secure_Websites_GitHub_20261022.zip
```

The ZIP includes this instruction file; everything else is ready to commit. Original image bytes are preserved under `assets/`.

## 2. Confirm the files

```bash
find . -maxdepth 2 -type f | sort
```

## 3. Initialize and publish if this local folder has no Git repository

```bash
cd ~/Downloads/GitHub/custom-secure-websites

git init
git branch -M main
git remote add origin https://github.com/elite-cybersecurity-specialist/custom-secure-websites.git

git add README.md SECURITY.md .gitignore SHA256SUMS.txt PUBLISH_INSTRUCTIONS.md docs/ assets/
git commit -m "Publish ECS custom secure websites architecture and hardening documentation"
git push -u origin main
```

If `origin` already exists, do **not** repeat `git remote add origin`; instead inspect `git remote -v`. To update the URL intentionally, run:

```bash
git remote set-url origin https://github.com/elite-cybersecurity-specialist/custom-secure-websites.git
```

If you have already initialized the folder and committed files, simply use `git add -A`, `git commit`, and `git push -u origin main` as applicable. Git might respond `nothing to commit` if there are no changes.

## 4. Verify the public result

```bash
git status
git remote -v
git log -1 --oneline
```

Open https://github.com/elite-cybersecurity-specialist/custom-secure-websites to check that all three images render and that documentation links resolve.

**Note:** GitHub will require your own authenticated Git credentials (for example, SSH key or supported credential manager). No credentials are included in this archive.
