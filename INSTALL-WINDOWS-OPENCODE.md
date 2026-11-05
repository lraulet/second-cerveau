# Installation pas à pas avec OpenCode (Windows)

Ce guide configure le vault avec **OpenCode**, sans installer ni lancer Claude Code. Le guide [`INSTALL-WINDOWS.md`](INSTALL-WINDOWS.md) reste disponible pour ceux qui souhaitent utiliser Claude Code.

> OpenCode est l'agent utilisé ici. Tu peux choisir le fournisseur et le modèle dans OpenCode ; un abonnement Claude n'est pas requis. Les frais et modalités dépendent du fournisseur choisi.

## 0. Prérequis

- Droits permettant d'installer des logiciels et d'ajouter un dossier au PATH utilisateur.
- Git, pour cloner le vault et enregistrer les changements.
- Obsidian, pour consulter et modifier le vault.
- Node.js LTS, seulement nécessaire aux MCP installés avec `npx`, comme Firecrawl.
- Une méthode d'authentification auprès d'un fournisseur pris en charge par OpenCode (clé API ou authentification proposée par le fournisseur).

Ouvre **PowerShell** (pas PowerShell ISE ni `cmd.exe`).

## 1. Git et le vault

Vérifie Git :

```powershell
git --version
```

Le dépôt de ce guide est `https://github.com/lraulet/second-cerveau.git`. Le guide Claude d'origine clone une autre URL ; ne reprends pas cette URL pour installer ce vault.

Si tu as déjà cloné ce dépôt, réutilise ce dossier et ne le clone pas une seconde fois. Sinon :

```powershell
git clone https://github.com/lraulet/second-cerveau.git "$HOME\Documents\Second Cerveau"
```

Le dossier cloné est à la fois le coffre Obsidian et le répertoire de travail d'OpenCode.

## 2. Installer OpenCode

### Option recommandée : CLI dans le terminal d'Obsidian

1. Depuis la [documentation officielle OpenCode V2](https://opencode.ai/v2/docs/), télécharge l'archive CLI Windows correspondant à l'architecture de la machine (x64 pour la plupart des PC, ARM64 pour les PC ARM).
2. Décompresse `opencode.exe` dans un dossier utilisateur, par exemple `%LOCALAPPDATA%\Programs\OpenCode`.
3. Ajoute ce dossier au **PATH utilisateur** dans les variables d'environnement Windows, puis ferme et rouvre PowerShell.
4. Vérifie :

```powershell
opencode --version
```

L'application [OpenCode Desktop](https://opencode.ai/v2/docs/) est également disponible pour Windows. Si tu l'utilises, ouvre le dossier du vault comme projet.

> Le guide V2 propose un binaire Windows autonome. Ne suis pas la Phase 2 de `INSTALL-WINDOWS.md` : elle installe Claude Code et configure `cc` pour le lancer.

## 3. Installer Obsidian et Node.js

Si ces applications ne sont pas déjà installées :

1. Installe Obsidian depuis [obsidian.md](https://obsidian.md).
2. Installe la version LTS de Node.js depuis [nodejs.org](https://nodejs.org), nécessaire aux MCP qui utilisent `npx`.

Ferme et rouvre PowerShell, puis vérifie si tu comptes utiliser Firecrawl :

```powershell
node --version
npx --version
```

## 4. Ouvrir le vault dans Obsidian

Dans Obsidian, choisis **Ouvrir un dossier comme coffre** et sélectionne le dossier du dépôt. Dans l'explorateur, tu dois voir notamment `0 INBOX`, `1 PROJETS`, `2 CASQUETTES` et `4 TOOLS`. Les fichiers d'instructions des agents peuvent être masqués dans l'explorateur Obsidian.

Active les plugins communautaires si nécessaire, puis installe et active :

- **Lean Terminal** (LeanProductivity), pour ouvrir OpenCode depuis le vault ;
- **Excalidraw**, pour les schémas ;
- **Hidden Folders Access**, si tu veux consulter ou modifier les skills, qui sont dans des dossiers cachés.

Calendar et le patch `Lean Terminal - Escape Fix` sont fournis dans le dépôt. Le patch Escape Fix concerne le comportement d'interruption de Claude Code ; il n'est pas requis par OpenCode.

Dans les réglages de Lean Terminal, sélectionne `powershell.exe` comme shell Windows. Ouvre ensuite un terminal intégré et vérifie que son dossier courant est la racine du vault :

```powershell
Get-Location
Get-ChildItem
```

## 5. Connecter un fournisseur et démarrer OpenCode

Depuis le terminal intégré, à la racine du vault :

```powershell
opencode
```

Dans OpenCode :

1. Lance `/connect`, choisis un fournisseur et suis son processus d'authentification.
2. Lance `/models` et sélectionne un modèle disponible.
3. Vérifie qu'OpenCode a ouvert le bon projet. Le fichier `AGENTS.md` à la racine contient ses instructions pour ce vault.

OpenCode n'utilise pas `CLAUDE.md` comme fichier d'instructions V2. Les deux fichiers peuvent coexister : `CLAUDE.md` reste en place pour Claude Code et `AGENTS.md` fournit les consignes à OpenCode.

## 6. Utiliser les skills du vault

Les skills existants sont dans `.claude/skills/`. OpenCode les détecte aussi depuis cet emplacement de compatibilité ; le dossier n'a pas besoin d'être déplacé. Pour en lancer un, demande-le dans ton message avec son identifiant, par exemple :

```text
@initialisation
```

Autres exemples : `@inbox-processor`, `@daily-review`, `@weekly-review`, `@create-skill` et `@done`. Pour un skill qui attend un sujet ou un nom, ajoute l'information dans le même message.

Les consignes communes sont dans `AGENTS.md`. `.claude/settings.json` et `.claude/statusline.js` sont propres à Claude Code et ne configurent pas OpenCode. Les permissions OpenCode sont dans `.opencode/opencode.json` : les commandes shell demandent confirmation sauf quelques commandes Git courantes, les commandes de suppression et `git push` sont bloquées, et la lecture des fichiers `.env` est refusée. En particulier, la barre Claude affiche des quotas Claude Pro/Max qui ne sont pas disponibles dans cette configuration OpenCode.

## 7. (Facultatif) Connecter Firecrawl comme MCP

Firecrawl est facultatif. Pour les pages web, il faut une clé API Firecrawl et Node.js. Ne colle pas la clé dans un fichier versionné.

1. Crée une clé sur [firecrawl.dev](https://firecrawl.dev).
2. Dans PowerShell, enregistre-la pour ton compte Windows (remplace la valeur d'exemple) :

```powershell
[Environment]::SetEnvironmentVariable("FIRECRAWL_API_KEY", "fc-ta-cle", "User")
```

Ferme et rouvre le terminal. Si le service OpenCode était déjà lancé avant l'ajout de la variable, redémarre-le :

```powershell
opencode service restart
```

3. Crée ou complète `.opencode/opencode.json` à la racine du vault avec cette configuration. Si le fichier existe, conserve ses autres réglages et fusionne seulement l'entrée `firecrawl` :

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "firecrawl": {
        "type": "local",
        "command": ["npx", "-y", "firecrawl-mcp"],
        "environment": {
          "FIRECRAWL_API_KEY": "{env:FIRECRAWL_API_KEY}"
        }
      }
    }
  }
}
```

4. Vérifie la connexion avec `opencode mcp list`. Dans la TUI, `/mcps` permet aussi de vérifier les serveurs et leurs outils. La configuration MCP d'OpenCode est distincte de celle de Claude : ne lance pas `claude mcp add`.

## 8. Vérifier le démarrage

Dans OpenCode, essaie `@initialisation`. L'agent devrait lire les instructions de `AGENTS.md` et pouvoir charger le skill. Tu peux ensuite déposer des fichiers dans `0 INBOX` et demander `@inbox-processor`.

Pour terminer une session de travail sur le vault, `@done` propose le flux documenté dans son skill. Vérifie les changements Git avant de les enregistrer :

```powershell
git status
git diff
```

---

## Dépannage

| Symptôme | Vérification |
|---|---|
| `opencode` n'est pas reconnu | Vérifie le dossier contenant `opencode.exe` dans le PATH utilisateur, puis rouvre PowerShell. |
| L'agent ne voit pas le vault | Lance OpenCode depuis la racine du coffre et vérifie avec `Get-Location`. |
| `AGENTS.md` n'est pas pris en compte | Vérifie qu'il est à la racine du projet ouvert dans OpenCode. |
| Un skill n'est pas proposé | Vérifie que le fichier est dans `.claude/skills/<id>/SKILL.md` et contient une description en frontmatter ; mentionne `@<id>` dans ta demande. |
| Firecrawl n'apparaît pas | Vérifie la variable `FIRECRAWL_API_KEY`, la configuration `.opencode/opencode.json` et `opencode mcp list`. |
| Les quotas `5h`/`7j` ne s'affichent pas | Ils sont spécifiques à la barre Claude du dépôt, pas à OpenCode. |

Documentation : [installation et CLI](https://opencode.ai/v2/docs/), [skills](https://opencode.ai/v2/docs/skills/), [instructions](https://opencode.ai/v2/docs/instructions/), [MCP](https://opencode.ai/v2/docs/mcp-servers/) et [permissions](https://opencode.ai/v2/docs/permissions/).
