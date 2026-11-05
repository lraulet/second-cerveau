---
name: connect-mcp
description: Connecter un serveur MCP à OpenCode V2 sans exposer de clé secrète dans le dépôt.
---

# Connecter un serveur MCP à OpenCode

Utilise ce skill lorsqu'on demande d'ajouter, configurer ou dépanner un serveur MCP dans OpenCode. Cette définition OpenCode remplace le skill Claude de même identifiant dans OpenCode ; le skill `.claude/skills/connect-mcp/` reste inchangé pour Claude Code.

## Procédure

1. Identifie le serveur, son package officiel, son transport pris en charge par OpenCode V2, ses arguments et ses variables requises. Consulte la documentation officielle du serveur plutôt que de deviner.
2. Demande uniquement les secrets nécessaires. Ne les écris pas dans un fichier versionné, ne les affiche pas dans la réponse et ne les mets pas dans l'historique Git.
3. Lis toute configuration OpenCode existante avant de la modifier. Préserve ses autres clés et serveurs.
4. Configure le serveur **au niveau utilisateur** (comme `--scope user` chez Claude Code), dans la config globale `~/.config/opencode/opencode.json` (sous Windows `%USERPROFILE%\.config\opencode\opencode.json`), sous `mcp.servers`. Ne l'ajoute au `.opencode/opencode.json` du vault que si l'utilisateur le demande. Pour un secret, utilise une substitution d'environnement `{env:NOM_VARIABLE}` plutôt que sa valeur littérale.
5. Pour un serveur local stdio, la forme V2 est :

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "nom-serveur": {
        "type": "local",
        "command": ["npx", "-y", "package-officiel"],
        "environment": {
          "NOM_VARIABLE": "{env:NOM_VARIABLE}"
        }
      }
    }
  }
}
```

Retire `environment` si aucune variable n'est nécessaire. Pour un serveur distant, vérifie que son endpoint utilise un transport accepté par OpenCode V2 avant d'utiliser `type: "remote"` et `url`.

## Cas Firecrawl

Pour le package `firecrawl-mcp`, l'exécutable local est `npx -y firecrawl-mcp` et la clé est `FIRECRAWL_API_KEY`. Sous Windows, enregistre la variable pour l'utilisateur avec `[Environment]::SetEnvironmentVariable("FIRECRAWL_API_KEY", "<clé>", "User")`, puis `opencode service restart` pour que le service hérite de la variable. Le serveur peut alors référencer la clé avec :

```json
"environment": {
  "FIRECRAWL_API_KEY": "{env:FIRECRAWL_API_KEY}"
}
```

Ne demande pas à l'utilisateur de lancer `claude mcp add` et ne modifie pas `~/.claude.json`.

## Vérification

- Exécute `opencode mcp list` et vérifie l'état du serveur.
- Dans la TUI, `/mcps` permet d'inspecter les connexions et l'authentification.
- Teste un appel réel et signale clairement toute erreur de connexion ou d'authentification.
- Les changements de configuration sont rechargés à chaud par OpenCode : pas besoin de relancer. Seul l'ajout d'une variable d'environnement demande `opencode service restart`.

Consulte la documentation officielle OpenCode V2 sur https://opencode.ai/v2/docs/mcp-servers/ si un détail de configuration est incertain.
