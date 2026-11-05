# Second Cerveau — template Catapulte

Un second cerveau personnel dans un vault Obsidian local, piloté par un agent (OpenCode ou Claude Code). Capture ta matière, structure-la, et lance des skills qui produisent de vrais livrables (synthèse de veille, revues, notes de projet).

Base : template second cerveau d'[Eliott Meunier](https://github.com/EliottMeunierFluid/templatesecondcerveau) (MIT), généralisé et outillé par Studio Catapulte.

## Ce qu'il y a dedans

Un seul dossier : c'est à la fois ton vault Obsidian et le répertoire depuis lequel l'agent travaille.

```
<ton second cerveau>/
├── .claude/skills/      # skills partagés : Claude Code et OpenCode
├── .opencode/           # configuration et skills spécifiques à OpenCode
├── .obsidian/           # config Obsidian (plugins, daily notes, templates)
├── 0 INBOX/             # capture rapide
├── 1 PROJETS/           # projets actifs
├── 2 CASQUETTES/        # responsabilités continues (vie, business, santé...)
├── 3 RESSOURCES/        # références, inspirations (brut)
├── 4 TOOLS/             # templates et process documentés
├── 5 ARCHIVE/           # terminé
├── _Import/             # notes à reclasser
├── AGENTS.md            # instructions d'OpenCode
└── CLAUDE.md            # instructions de Claude Code
```

Tu ouvres ce dossier dans Obsidian et tu lances l'agent depuis ce même dossier (`opencode` ou `claude`). Pas de niveau intermédiaire, pas de sous-dossier à ne pas confondre.

Les skills partagés vivent dans `.claude/skills/`. OpenCode les détecte aussi depuis cet emplacement de compatibilité. C'est un dossier caché, donc invisible par défaut dans l'explorateur Obsidian : le plugin **Hidden Folders Access** (dans la liste ci-dessous) te permet de les voir et de les éditer comme n'importe quelle note. Les skills spécifiques à OpenCode peuvent vivre dans `.opencode/skills/`.

## Skills fournis

- **`initialisation`** — configure le vault avec toi au démarrage (ton nom, ton contexte).
- **`create-skill`** — construire TES propres skills. Un bon premier exercice : ton skill de veille (mapper ton process de veille, puis le figer en commande réutilisable).
- **`daily-review`, `weekly-review`, `new-life-phase`** — journaling et revues.
- **`inbox-processor`, `import`, `map-process`** — traitement et structuration.
- **`new-automation`, `connect-mcp`** — étendre l'agent (brancher des outils/MCP, automatiser).
- **`done`, `thinking-partner`, `research-assistant`, `de-ai-ify`** — clôture de session, réflexion, recherche, nettoyage de style.

Dans Claude Code, ces skills s'invoquent avec `/nom-du-skill`. Dans OpenCode, mentionne `@nom-du-skill` dans ton message (par exemple `@initialisation`).

## Démarrage

**Sur Windows :** choisis le guide correspondant à l'agent : [`INSTALL-WINDOWS-OPENCODE.md`](INSTALL-WINDOWS-OPENCODE.md) pour OpenCode, ou [`INSTALL-WINDOWS.md`](INSTALL-WINDOWS.md) pour Claude Code. Le guide OpenCode utilise l'URL de ce dépôt ; l'ancien guide Claude est conservé tel quel.

**Sur macOS ou Linux :**

1. **Clone** ce repo en local :
   ```
   git clone <url du repo> "Second Cerveau"
   cd "Second Cerveau"
   ```
2. Ouvre **ce dossier** comme vault dans **Obsidian**.
3. Active les plugins communautaires (désactive le mode restreint), puis installe **Lean Terminal**, **Excalidraw** et **Hidden Folders Access**. Calendar et le patch Lean Terminal Escape Fix sont déjà fournis.
4. Ouvre un terminal (Lean Terminal dans Obsidian, ou ton terminal habituel dans ce dossier) et lance **`opencode`** ou **`claude`**.
5. Avec OpenCode, lance le skill en mentionnant **`@initialisation`** dans ton message ; avec Claude Code, utilise **`/initialisation`**.
6. Dépose ta matière (veille, notes, docs de rôle) dans `0 INBOX/`, puis lance `@inbox-processor` dans OpenCode ou `/inbox-processor` dans Claude Code.

Le repo est un **template** : après le clone, c'est ton vault. Tu n'as pas besoin de compte GitHub pour t'en servir (git local suffit).

## Prérequis outils

- **OpenCode** avec un fournisseur/modèle configuré, ou **Claude Code** (abonnement Claude Pro ou Max).
- **Obsidian**, avec **Lean Terminal** (terminal intégré), **Excalidraw** (schémas) et **Hidden Folders Access** (voir `.claude/skills/`).
- **Firecrawl** (MCP, plan gratuit) pour scraper le web (utile quand tu construiras ta veille), voir le skill `connect-mcp`.
- **Node.js** (LTS), requis par les MCP qui tournent via `npx` — dont Firecrawl.

> Le plugin **Lean Terminal - Escape Fix** est fourni dans `.obsidian/plugins/`. Il corrige le comportement d'Échap pour Claude Code ; il n'est pas requis par OpenCode.

## Notes

- Structure **à plat** assumée : le vault et le répertoire de travail de l'agent sont le même dossier. La variante imbriquée (un WORKSPACE parent pouvant héberger d'autres projets à côté du vault) est conservée sur la branche `structure-imbriquee`.
- Les instructions sont dans `CLAUDE.md` pour Claude Code et `AGENTS.md` pour OpenCode V2 (`CLAUDE.md` n'est pas chargé automatiquement par OpenCode).
- `.claude/settings.json` règle les permissions de Claude Code. Les permissions OpenCode sont configurées séparément dans `.opencode/opencode.json`.
