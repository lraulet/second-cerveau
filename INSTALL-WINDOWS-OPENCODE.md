# Installation pas à pas avec OpenCode (Windows)

De zéro jusqu'à ton second cerveau opérationnel, avec **OpenCode** à la place de Claude Code. Le guide [`INSTALL-WINDOWS.md`](INSTALL-WINDOWS.md) reste disponible pour Claude Code. Les deux agents partagent le même vault, les mêmes skills et les mêmes instructions (`CLAUDE.md`).

> Pas besoin d'abonnement Claude : tu choisis ton fournisseur et ton modèle dans OpenCode. Les coûts dépendent du fournisseur.

| Phase | Qui fait | Quoi |
|---|---|---|
| 0-2 | toi | Git, OpenCode, connexion d'un fournisseur |
| 3 | toi ou l'agent | Obsidian, Node, le vault |
| 4 | toi | Obsidian : ouvrir le coffre et ses plugins |
| 5-10 | vous deux | L'agent prend le relais |

---

## Phase 0 - Avant de commencer

- [ ] Un compte chez un fournisseur de modèles pris en charge par OpenCode (clé API ou abonnement compatible)
- [ ] Droits d'installation de logiciels (pas besoin d'être administrateur)
- [ ] Ta matière prête dans un dossier

**Ouvrir PowerShell :** touche Windows, taper `powershell`, Entrée.

---

## Phase 1 - Git

Identique à la Phase 1 de [`INSTALL-WINDOWS.md`](INSTALL-WINDOWS.md) (téléchargement direct sur https://git-scm.com/download/win, éditeur **Notepad**, branche initiale **main**).

- [ ] Vérif : `git --version` renvoie un numéro de version

---

## Phase 2 - OpenCode

### 2.1 Installation

1. Télécharge l'archive CLI Windows depuis la [documentation OpenCode V2](https://opencode.ai/v2/docs/) (**x64** pour la plupart des PC, **ARM64** pour les PC ARM).
2. Décompresse `opencode.exe` dans `%LOCALAPPDATA%\Programs\OpenCode`.
3. Ajoute ce dossier au PATH utilisateur (pas besoin d'administrateur) :

```powershell
[Environment]::SetEnvironmentVariable("Path", [Environment]::GetEnvironmentVariable("Path","User") + ";$env:LOCALAPPDATA\Programs\OpenCode", "User")
```

- [ ] **Ferme et rouvre PowerShell**
- [ ] Vérif : `opencode --version` renvoie un numéro de version

> Alternative : l'application **OpenCode Desktop** pour Windows (même page). Ouvre alors le dossier du vault comme projet.

### 2.2 Connexion à un fournisseur

```powershell
opencode
```

Dans OpenCode : `/connect`, choisis ton fournisseur et suis l'authentification, puis `/models` pour choisir le modèle.

- [ ] Fournisseur connecté, modèle choisi
- [ ] Quitter (`Ctrl+C`)

### 2.3 Pas de raccourci `cc` nécessaire

Avec Claude Code, `cc` servait à supprimer les demandes de confirmation. OpenCode fonctionne déjà ainsi pour ce vault : `.opencode/opencode.json` autorise les actions courantes sans question et **bloque** `rm`, `Remove-Item`, `del`, `git push`, `git reset --hard` et la lecture des `.env`, comme `.claude/settings.json` le faisait pour Claude Code. Tu lances simplement `opencode`.

---

## Phase 3 - Obsidian, Node, et le vault

### 3.1 Obsidian et Node

1. **Obsidian** : https://obsidian.md → « Download for Windows ». N'ouvre pas encore Obsidian.
2. **Node.js LTS** : https://nodejs.org (requis pour les MCP lancés via `npx`, dont Firecrawl).

### 3.2 Le vault

```powershell
git clone https://github.com/lraulet/second-cerveau.git "$HOME\Documents\Second Cerveau"
```

> Pour utiliser la version adaptée à OpenCode tant qu'elle n'est pas fusionnée dans `main`, ajoute `-b opencode` à la commande.

Ou laisse l'agent le faire : lance `opencode` dans `Documents` et colle :

```
Clone le vault : git clone https://github.com/lraulet/second-cerveau.git "$HOME/Documents/Second Cerveau"
Vérifie qu'il contient 0 INBOX, 1 PROJETS, CLAUDE.md, AGENTS.md, .claude/skills (14 skills)
et .opencode/commands (14 commandes). Obsidian et Node sont déjà installés : ne les
réinstalle pas. Termine par un récapitulatif franc de ce qui a marché ou échoué.
```

- [ ] **Fermer et rouvrir PowerShell**, puis vérif : `node --version`

---

## Phase 4 - Obsidian et ses plugins

Identique à la Phase 4 de [`INSTALL-WINDOWS.md`](INSTALL-WINDOWS.md) : ouvrir le dossier `Second Cerveau` comme coffre, désactiver le mode restreint, installer **Lean Terminal**, **Excalidraw**, **Hidden Folders Access**, et régler le shell de Lean Terminal sur `powershell.exe`.

- [ ] Dans le terminal d'Obsidian, `dir` liste `0 INBOX`, `1 PROJETS`, `CLAUDE.md`, `AGENTS.md`...

> Le plugin fourni **Lean Terminal - Escape Fix** peut rester activé : il laisse la touche Échap au terminal intégré.

---

## Phase 5 - Lancer l'agent

Dans le terminal d'Obsidian :

```powershell
opencode
```

- [ ] OpenCode démarre dans le vault
- [ ] Vérif : taper `/` et voir `initialisation`, `create-skill`, `done`, `connect-mcp` dans la liste

**Instructions :** OpenCode charge `AGENTS.md` et, via la config du projet, `CLAUDE.md`. C'est le même fichier d'instructions que pour Claude Code : une seule source.

**Contexte :** la barre de statut Claude (`.claude/statusline.js`) n'est pas utilisée par OpenCode, qui a sa propre interface. Pas de quotas `5h`/`7j` (spécifiques à l'abonnement Claude).

> **Règle à retenir :** au-delà de 40 % de contexte utilisé, lance `/done` et ouvre une nouvelle session.

---

## Phase 6 - Initialisation du vault

```
/initialisation
```

- [ ] Conversation terminée
- [ ] Vérif : tes notes de contexte sont créées, et `CLAUDE.md` contient le nom de ton IA et ton prénom

> Attention : `/initialisation`, pas `/init`.

---

## Phase 7 - Ta matière

- [ ] Copier tes fichiers dans `0 INBOX\`, puis :

```
/inbox-processor
```

---

## Phase 8 - Firecrawl

### 8.1 Clé API

Compte gratuit sur https://firecrawl.dev, clé commençant par `fc-`.

### 8.2 Brancher le connecteur

Le plus simple, dans OpenCode :

```
/connect-mcp firecrawl
```

À la main : enregistre la clé dans ton environnement utilisateur (elle ne finit jamais dans le dépôt) :

```powershell
[Environment]::SetEnvironmentVariable("FIRECRAWL_API_KEY", "fc-ta-cle", "User")
opencode service restart
```

Puis ajoute dans ta config globale `%USERPROFILE%\.config\opencode\opencode.json` (crée-la si besoin, conserve ses autres réglages) :

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "firecrawl": {
        "type": "local",
        "command": ["npx", "-y", "firecrawl-mcp"],
        "environment": { "FIRECRAWL_API_KEY": "{env:FIRECRAWL_API_KEY}" }
      }
    }
  }
}
```

- [ ] Vérif : `opencode mcp list` affiche `firecrawl` connecté (ou `/mcps` dans OpenCode)
- [ ] Vérif : demander de scraper une page publique et de la résumer

> Pas besoin de relancer OpenCode après une modification de config. Seul l'ajout d'une **variable d'environnement** demande `opencode service restart`.

---

## Phase 9 - Ton premier skill

Identique à la Phase 9 de [`INSTALL-WINDOWS.md`](INSTALL-WINDOWS.md), avec `/create-skill`. Le skill est écrit dans `.claude\skills\<nom>\SKILL.md` et sa commande dans `.opencode\commands\<nom>.md`. Pas besoin de relancer : tape directement `/<nom-du-skill>`.

---

## Phase 10 - Clôturer

```
/done
```

---

## Dépannage rapide

| Symptôme | Correctif |
|---|---|
| `opencode` non reconnu | Rouvrir le terminal ; vérifier `Test-Path "$env:LOCALAPPDATA\Programs\OpenCode\opencode.exe"` et le PATH (Phase 2.1) |
| Aucune commande `/initialisation` | OpenCode lancé hors du vault : `dir` doit montrer `CLAUDE.md` et `.opencode` |
| L'agent ignore les règles du vault | Vérifier que `.opencode/opencode.json` contient `"instructions": ["CLAUDE.md"]` |
| Aucun modèle disponible | `/connect` puis `/models` |
| Firecrawl absent | `opencode mcp list` ; si la clé vient d'être ajoutée, `opencode service restart` |
| L'agent part en vrille | Contexte saturé : `/done` puis nouvelle session |

## Mémo des commandes

| Commande | Quand |
|---|---|
| `opencode` | Lancer l'agent, depuis le terminal d'Obsidian |
| `/initialisation` | Une seule fois, au démarrage |
| `/inbox-processor` | Après dépôt dans `0 INBOX` |
| `/create-skill` | Transformer un process en commande |
| `/connect-mcp <outil>` | Brancher un outil externe |
| `/daily-review`, `/weekly-review` | Journaling et bilans |
| `/done` | Fin de session, toujours |
| `/connect`, `/models`, `/mcps` | Fournisseur, modèle, connecteurs |
