# Instructions OpenCode

**`CLAUDE.md` à la racine est la source unique des instructions de ce vault** : identité de l'IA, structure IPCRA, phases de vie, règles d'interaction. OpenCode le charge automatiquement via `instructions` dans `.opencode/opencode.json`. Si ce n'était pas le cas, lis `CLAUDE.md` avant toute action et applique-le intégralement.

Ne duplique jamais ces règles ici : toute modification des consignes (y compris le nom de l'IA et le prénom posés par `/initialisation`) se fait dans `CLAUDE.md`.

Ce fichier ne contient que la traduction des notions propres à Claude Code vers OpenCode.

## Skills et commandes

- Les skills sont dans `.claude/skills/<id>/SKILL.md` (lus par Claude Code et OpenCode). Chacun a une commande OpenCode du même nom dans `.opencode/commands/<id>.md`, donc `/initialisation`, `/done`, etc. fonctionnent comme dans Claude Code.
- `.opencode/skills/` contient les variantes propres à OpenCode ; elles priment sur le skill de même identifiant dans `.claude/skills/` (c'est le cas de `connect-mcp`).
- Quand un skill est créé, crée aussi sa commande dans `.opencode/commands/` (voir le skill `create-skill`).

## Correspondance des outils

Quand un skill cite un outil de Claude Code, utilise l'équivalent OpenCode :

| Claude Code | OpenCode |
|---|---|
| `Read`, `Edit` (`replace_all`), `Write`, `Glob`, `Grep` | `read`, `edit` (`replaceAll`), `write`, `glob`, `grep` |
| `Bash` | `shell` (PowerShell sous Windows : adapte `ls`, `find`, `sort -V`, `tail`, etc.) |
| `Agent` / sous-agents | outil `subagent` (agent `general` pour les lots en parallèle) |
| `WebSearch`, `WebFetch` | `websearch`, `webfetch` |
| `mcp__firecrawl-mcp__firecrawl_*` | outils du serveur MCP `firecrawl` déclaré dans la config OpenCode |
| `claude mcp add`, `~/.claude.json` | configuration `mcp.servers` d'OpenCode (skill `connect-mcp`) |
| `/exit` puis relancer pour charger un skill ou un MCP | inutile : OpenCode recharge config, skills, commandes et MCP à chaud |
| plan mode / build mode | agents `plan` / `build` d'OpenCode |

## Fichiers propres à Claude Code

`.claude/settings.json` (permissions) et `.claude/statusline.js` (barre de statut) ne sont pas lus par OpenCode. Leur équivalent : les `permissions` de `.opencode/opencode.json`. Ne les modifie pas pour OpenCode.
