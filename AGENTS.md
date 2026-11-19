# 📌 Obsidian Knowledge Base — Agent Guidelines & Auto-Push

## 🚨 Git Push on Modification Rule

Whenever you create, update, rename, or delete files within this repository:
1. Stage all changed files: `git add .`
2. Commit with conventional commit message: `git commit -m "<type>(<scope>): <message>"`
3. Push changes immediately to GitHub: `git push origin main`
4. Remote URL: `https://github.com/erythroai-droid/RAG.git`

## 🧩 Installed Plugins & Skills Registry Rule

Whenever any AI agent (Antigravity, Cursor, Claude Code, Codex, Grok) installs, updates, or removes a plugin or skill (globally or per-project):
1. Record the change in `06-Installed-Plugins-Skills/plugins-skills-registry.md` (agent, version/commit, source repo/marketplace, skills included, check date).
2. Follow the update check and safe-upgrade protocol in `06-Installed-Plugins-Skills/plugin-update-policy.md` (check updates periodically and before major tasks in the plugin's domain).
3. Commit and push the Knowledge Base update per the rule above.
