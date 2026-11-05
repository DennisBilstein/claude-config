# claude-config

Meine globalen Regeln für Claude Code. Sie gelten für alle Projekte und stehen in `AGENTS.md`.

## Auf einem neuen Gerät einrichten

```bash
git clone https://github.com/DennisBilstein/claude-config.git ~/claude-config
mkdir -p ~/.claude
echo '@~/claude-config/AGENTS.md' >> ~/.claude/CLAUDE.md
```

## Änderungen synchronisieren

Regeln in `AGENTS.md` ändern, dann committen und `git push`. Auf dem anderen Gerät reicht danach `git pull`.

Keine Secrets in dieses Repo legen.
