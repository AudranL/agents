# Agents d'une équipe agile

> **Work in progress.** Ces agents sont encore en cours de développement : leurs règles, leurs sections, leurs noms et la structure de leur mémoire peuvent changer d'une version à l'autre, sans compatibilité garantie.

Cinq agents d'IA qui mènent un besoin de l'idée au code livré, fonctionnalité par fonctionnalité, comme une équipe agile : un chef d'orchestre, un product owner, un architecte, un développeur et un testeur. Ils marchent avec Claude Code, GitHub Copilot et opencode.

## Contenu

| Agent | Rôle |
|---|---|
| `orchestrator` | Mène un besoin de bout en bout : lance les quatre autres, pose leurs questions à l'utilisateur et tient les trois étapes de validation (la fiche, les décisions pendant le développement, la fonctionnalité livrée) |
| `product-owner` | Challenge le besoin, le découpe en fonctionnalités, écrit la fiche de spécification et ses scénarios Gherkin, tient le backlog et fixe les besoins non fonctionnels |
| `software-architect` | Relit la fiche sous l'angle de l'architecture, donne les consignes d'une fonctionnalité, instruit les décisions (ADR) et les rend vérifiables par des tests d'architecture |
| `software-engineer` | Implémente en TDD, avec un test d'acceptation par scénario, du code propre et des refactorings à petits pas ; sécurise le code legacy avant de le modifier |
| `quality-analyst` | Relit la fiche sous l'angle des tests, puis fait la recette : tests de bout en bout sans fenêtre, besoins non fonctionnels, captures, exploration |

L'`orchestrator` lance les quatre autres en mode délégué : ils lui renvoient leurs questions, et c'est lui qui parle à l'utilisateur. Chacun peut aussi servir seul.

## Prérequis

- **Skills** : chaque agent déclare dans sa section « Skills » celles dont il a besoin, et sait quand les charger. Elles sont dans le dépôt [skills](https://github.com/AudranL/skills), à installer avec son propre `install.sh` :

  | Agent | Skills |
  |---|---|
  | `orchestrator` | `kanban`, `agent-memory` |
  | `product-owner` | `spec-writing`, `spec-review`, `acceptance-tests`, `kanban`, `agent-memory` |
  | `software-architect` | `adr-writer`, `architecture-tests`, `architecture-styles`, `spec-review`, `architecture-review`, `agent-memory` |
  | `software-engineer` | `test-conventions`, `acceptance-tests`, `clean-code`, `unit-tests`, `legacy-code`, `agent-memory` ; `golden-master` (facultative, pour du legacy Java très combinatoire) |
  | `quality-analyst` | `spec-review`, `e2e-tests`, `agent-memory` |

- **Skills facultatives** : le `software-engineer` se sert aussi, si elles sont installées, de `tdd`, `codebase-design`, `diagnosing-bugs` et `code-review`, de Matt Pocock ([mattpocock/skills](https://github.com/mattpocock/skills)).
- **Serveur MCP** (facultatif) : un serveur MCP Jira (Atlassian), pour pousser les tickets du backlog dans Jira. Sans lui, le backlog reste local, dans `docs/backlog/`.
- **Mémoire** : chaque agent crée, avec ton accord, sa mémoire dans le projet, au premier passage : `docs/ia/orchestrator/`, `docs/ia/product-owner/`, `docs/ia/architecture/`, `docs/ia/software-engineer/` et `docs/ia/quality-analyst/`. Chacune n'est lue et écrite que par son agent.

## Installation

```sh
./install.sh                              # pour toi, dans les trois outils
./install.sh --outils claude              # un seul outil
./install.sh --projet <dossier> --copie   # dans un projet, en copies versionnables
./install.sh --check                      # vérifie sans rien modifier
```

| Outil | Pour toi | Dans un projet |
|---|---|---|
| Claude Code | `~/.claude/agents/<nom>.md` | `.claude/agents/<nom>.md` |
| GitHub Copilot | `~/.copilot/agents/<nom>.agent.md` (CLI) | `.github/agents/<nom>.agent.md` |
| opencode | `~/.config/opencode/agents/<nom>.md` | `.opencode/agents/<nom>.md` |

Installer aussi les skills : `./install.sh` dans le dépôt [skills](https://github.com/AudranL/skills).

## Langue

Les agents sont écrits en français. Ils parlent à l'utilisateur dans sa langue, et le code, les tests et les messages de commit suivent la langue du projet.

## Limites

- `mode` et `permission` ne sont lus que par opencode : les restrictions de lecture et d'écriture par chemin (chaque mémoire réservée à son agent, l'`orchestrator` qui n'écrit que la sienne) ne s'appliquent pas dans Claude Code ni dans Copilot. Les agents les suivent alors par leurs règles.
- Copilot limite le corps d'un agent à 30 000 caractères : le plus long, `product-owner`, en compte environ 28 300.

## Licence

MIT, voir `LICENSE`.
