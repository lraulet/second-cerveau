# Instructions du vault pour OpenCode

Ce dossier est à la fois le vault Obsidian et le répertoire de travail de l'agent. Tous les chemins ci-dessous sont relatifs à sa racine.

## Structure

- `0 INBOX/` : matière à traiter
- `1 PROJETS/` : projets actifs, avec une note de contexte par projet
- `2 CASQUETTES/` : responsabilités continues et contexte personnel
- `3 RESSOURCES/` : références et inspirations
- `4 TOOLS/` : templates et processus
- `5 ARCHIVE/` : contenu terminé ou inactif
- `_Import/` : imports à reclasser
- `.claude/skills/` : skills partagés avec Claude Code et découverts par OpenCode

## Consulter le contexte

- Quand une demande concerne un projet ou une casquette, lis d'abord sa note de contexte principale.
- Pour les demandes personnelles, consulte `2 CASQUETTES/Sur ma vie/Moi.md`.
- Pour le journaling, les bilans et les objectifs, détecte la phase de vie active dans `2 CASQUETTES/Sur ma vie/Life Phases/` au lieu de supposer son numéro.
- Les logs de session vont dans la phase active, selon le skill `done`.

## Règles de travail

- Réponds en français, directement et avec concision ; garde les accents et n'utilise pas de tiret cadratin.
- Utilise les `[[wikilinks]]` dans les notes Obsidian lorsque c'est pertinent.
- Préserve la structure existante et le travail de l'utilisateur ; ne supprime pas ou ne réinitialise pas de données sans demande explicite.
- Ne pousse jamais vers un dépôt distant sans demande explicite.
- Les skills de ce vault restent dans `.claude/skills/` afin de conserver leur compatibilité avec Claude Code. Les skills OpenCode spécifiques peuvent être placés dans `.opencode/skills/`.
- Les skills partagés peuvent contenir des exemples de commandes Bash ou citer les outils de Claude Code. Adapte les commandes au shell et au système réellement utilisés, et utilise les outils de lecture/édition disponibles dans l'agent courant.
- Lors de l'initialisation, enregistre l'identité de l'agent dans le fichier lu par l'agent courant : `AGENTS.md` pour OpenCode, `CLAUDE.md` pour Claude Code. Ne modifie pas l'autre fichier au nom de l'agent.

## Fichiers spécifiques aux agents

- OpenCode V2 lit `AGENTS.md` ; il ne charge pas automatiquement `CLAUDE.md`.
- `CLAUDE.md`, `.claude/settings.json` et `.claude/statusline.js` sont conservés pour Claude Code et ne remplacent pas la configuration OpenCode.
