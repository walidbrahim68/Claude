# Outils Claude Code installés

Configuration dans `.claude/settings.json` (portée projet). Claude Code installe ces plugins à l'ouverture du repo, après confirmation de confiance du dossier.

Coût = tokens ajoutés à **chaque** session (estimation de `claude plugin details`).

## Plugins actifs

| Outil | Plugin | Coût/session | Note |
|---|---|---:|---|
| superpowers | `superpowers@claude-plugins-official` | ~850 | Planifie et débogue avant d'écrire |
| karpathy-skills | `andrej-karpathy-skills@karpathy-skills` | ~105 | |
| ponytail | `ponytail@ponytail` | ~640 | Options facultatives : `/plugin configure ponytail@ponytail` |
| anthropics/skills | `example-skills@anthropic-agent-skills` | ~1 220 | |
| ui-ux-pro-max | `ui-ux-pro-max@ui-ux-pro-max-skill` | ~1 070 | |
| taste-skill | `taste-skill@taste-skill` | ~330 | |
| claude-plugins | marketplace `claude-plugins-official` | — | Catalogue officiel Anthropic (317 plugins) |
| repomix | `repomix-mcp@repomix`, `repomix-commands@repomix` | ~60 | |
| planning-with-files | `planning-with-files@planning-with-files` | ~1 130 | |
| firecrawl | `firecrawl@claude-plugins-official` | ~230 | Clé API Firecrawl requise |
| playwright-mcp | `playwright@claude-plugins-official` | ~0 | |
| github-mcp | `github@claude-plugins-official` | ~0 | Token GitHub requis en local |
| codex-plugin-cc | `codex@openai-codex` | ~450 | Compte OpenAI/Codex requis |
| claude-hud | `claude-hud@claude-hud` | ~100 | Statusline : lancer `/hud` (Node 22.18+) |
| caveman | skills dans `.claude/skills/` | — | Actif par défaut via `CLAUDE.md` |

Total actif ≈ 6 000 tokens/session. Pour en couper un : `/plugin disable <plugin> --scope project`.

## Installé mais désactivé

| Outil | Pourquoi |
|---|---|
| ECC (`ecc@ecc`) | 412 skills = **~48 000 tokens/session**. Activer : `/plugin enable ecc@ecc --scope project` |
| claude-mem (`claude-mem@thedotmack`) | Appel Claude Haiku en arrière-plan **après chaque outil** + agent planifié toutes les 10 min, sur ton abonnement. Injecte 50 observations au démarrage. Télémétrie PostHog. En cloud la mémoire est perdue à chaque conteneur : coût sans bénéfice. |
| document-skills | Doublon des skills docx/pdf/pptx/xlsx déjà intégrés à Claude |

## Marketplace seulement (rien d'activé)

| Outil | Pourquoi |
|---|---|
| wshobson/agents (`claude-code-workflows`) | 94 plugins. Choisir : `/plugin install <nom>@claude-code-workflows` |

## Pas des plugins : installation sur ta machine

| Outil | Commande / lien |
|---|---|
| gstack | `git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup` |
| graphify | `uv tool install graphifyy` |
| codegraph | `npx @colbymchenry/codegraph` puis `codegraph init` dans le projet |
| cc-switch | App desktop : https://github.com/farion1231/cc-switch/releases |
| claude-code-router | https://github.com/musistudio/claude-code-router |
| vibe-kanban | `npx vibe-kanban` |
| multica | Plateforme auto-hébergée : https://github.com/multica-ai/multica |

## Lecture seulement (listes, docs, code d'apprentissage)

- learn-claude-code : https://github.com/shareAI-lab/learn-claude-code
- awesome-claude-skills : https://github.com/ComposioHQ/awesome-claude-skills
- awesome-mcp-servers : https://github.com/punkpeye/awesome-mcp-servers
- system-prompts-ai : https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools
- best-practice : https://github.com/shanraisshan/claude-code-best-practice

## Non installé

- heretic (https://github.com/p-e-w/heretic) : outil Python qui retire les refus d'un modèle open-weight local (GPU requis). Ce n'est pas un plugin Claude Code et il ne modifie pas Claude.

## Audit consommation (coûts cachés)

Mesuré en exécutant chaque hook :

| Plugin | Injection | Quand |
|---|---:|---|
| superpowers | ~900 tok | Chaque démarrage. Exige de charger une skill avant **toute** réponse : plus de tokens par tour |
| ponytail | ~715 tok | Chaque démarrage. Recoupe caveman (deux règles de style) |
| planning-with-files | ~800 tok | **Chaque message** si `task_plan.md` existe ; +50 tok après chaque Write/Edit |
| codex | 0 | Hook Stop de revue désactivé par défaut (`/codex:setup` l'active) |

Aucune instruction cachée malveillante trouvée dans les skills actives.
