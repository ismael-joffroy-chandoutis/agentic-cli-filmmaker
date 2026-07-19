<picture><source media="(prefers-color-scheme: dark)" srcset="banner.svg"><img src="banner-light.svg" alt="Agentic CLI for Filmmakers" width="100%"></picture>

[English](README.md) · **Français**

Pas un tutoriel pour développeurs. Pas un guide de prompt engineering. C'est un vrai setup, personnel, construit par un artiste qui utilise un CLI de code agentique tous les jours pour le cinéma, les outils créatifs et le vibe coding.

<img src="monde.jpg" alt="agentic-cli-filmmaker" width="100%">

**Config. Patterns. Hooks. Accès distant. Tout au même endroit.**

La méthode est agnostique au CLI. Cette note de terrain documente l'instance actuelle, Claude Code ; quand la pratique bougera, la note de terrain suivante documentera l'instrument suivant.

[![License: PolyForm Noncommercial 1.0.0](https://img.shields.io/badge/License-PolyForm_Noncommercial-yellow.svg)](LICENSE.md)
![CLI agentique](https://img.shields.io/badge/CLI_agentique-outil_quotidien-blueviolet?style=flat-square)
![Skills](https://img.shields.io/badge/Skills-109-green?style=flat-square)
![Personas](https://img.shields.io/badge/Agent_Personas-38-orange?style=flat-square)

---

## Contexte

Je suis [Ismaël Joffroy Chandoutis](https://ismaeljoffroychandoutis.com), cinéaste et artiste basé à Paris. Je travaille entre cinéma, art contemporain et nouveaux médias. Je ne suis pas développeur. J'utilise un CLI de code agentique comme partenaire créatif et technique pour construire des outils, automatiser des workflows et explorer l'IA comme méthode artistique.

Ce dépôt documente la configuration réelle, les décisions et les patterns qui ont émergé d'un usage quotidien depuis janvier 2026. Tout ici est testé en production (ma production, c'est-à-dire faire des films et de l'art).

**César 2022** du meilleur court métrage documentaire. Sélectionné à **Cannes**, **IDFA**, **Hot Docs**, **Annecy**. Mention honorifique **Ars Electronica**.

---

## Contenu

```
claude-code-filmmaker/
├── config/       # Structure des instructions globales, settings.json annoté
├── patterns/     # 14 patterns réutilisables (tmux, mémoire, notifications, agent teams...)
├── hooks/        # Notifications push, nommage auto des onglets, gestion du layout tmux
├── remote/       # Toutes les façons de faire tourner l'agent à distance — comparées
├── setup/        # Inventaire complet : 109 skills, 38 personas, 40+ plugins
├── essays/       # Textes issus de cette pratique
├── journal/      # Journal des décisions
└── sources/      # Articles et fils qui ont façonné le setup
```

---

## Config

Comment l'agent est configuré, et pourquoi.

| Fichier | Ce qu'il couvre |
|------|---------------|
| [Structure du fichier d'instructions](config/claude-md-structure.md) | Le fichier d'instructions globales : identité, workflows, 7 règles de qualité (anti-hallucination, scrap-and-redo, règles auto-actualisées, mode confrontation...) |
| [Settings expliqués](config/settings-explained.md) | Chaque choix de `settings.json` annoté : confidentialité (télémétrie coupée), règles de refus (minimales : ne bloquer que l'irréversible), statusline, agent teams, extended thinking |

**Décisions clés :**
- Les règles de refus sont minimales par design : ne bloquer que les commandes irréversibles. La séparation permissions interactives vs bypass gère le contrôle au quotidien.
- Télémétrie entièrement désactivée (approche Trail of Bits).
- Extended thinking toujours activé.
- Historique de conversation conservé 365 jours au lieu de 30.

---

## Patterns

14 patterns réutilisables, cadres de décision et scripts d'automatisation.

| Pattern | Ce qu'il résout |
|---------|---------------|
| [Max Plan vs API](patterns/max-vs-api.md) | Quand le plan Max (200$/mois) est rentable face à l'API. Données d'usage réelles et analyse du seuil de rentabilité. |
| [Guide de survie tmux](patterns/tmux-survival.md) | Ne jamais perdre une session. Setup multi-machines, accès distant, récupération après reboot. |
| [Pont Telegram](patterns/telegram-bridge.md) | Piloter l'agent depuis son téléphone via Telegram. Bidirectionnel : texte, voix, images, fichiers. |
| [Notifications](patterns/notifications.md) | Être notifié quand une tâche se termine. Son (Zelda !) + push (Pushover/Telegram/macOS). |
| [Statusline](patterns/statusline.md) | Barre de statut sur deux lignes : modèle, usage du contexte, coût, durée, % de cache, lignes modifiées. |
| [Boîte à outils de scripts](patterns/scripts-toolkit.md) | Tous les scripts d'automatisation : bootstrap, dashboard, heartbeat, calculateur de coût, recherche mémoire. |
| [Système de mémoire](patterns/memory-system.md) | Mémoire persistante entre sessions. Logs quotidiens, suivi du backlog, recherche Spotlight. |
| [Reprise des sessions](patterns/resume-sessions.md) | Restaurer toutes les fenêtres tmux après un reboot avec une seule commande de reprise. Un seul script. |
| [Multi-plateforme](patterns/cross-platform.md) | macOS, Linux, Windows (WSL2). Couche d'abstraction et guide d'installation. |
| [Monitoring du layout des agents](patterns/agent-layout-monitoring.md) | Surveiller l'activité des subagents en temps réel. Layout en split, hooks tmux, tail en direct. |
| [Setup multi-machines](patterns/multi-machine.md) | MacBook + Mac Mini + PC sur Tailscale. SSH/mosh, sync git, source de vérité. |
| [Ghostty + cmux](patterns/ghostty-cmux.md) | Terminal GPU + tmux natif. Restauration automatique de toutes les fenêtres au reboot, sans AppleScript. |
| [Agent Teams](patterns/agent-teams.md) | Coordination multi-agents pair à pair. Partage de liste de tâches, messagerie, procédure d'arrêt. |
| [Pipeline d'analyse de voix off](patterns/vo-analysis-pipeline.md) | Analyse de voix off pour production documentaire. ASR local, analyse LLM, mise à jour structurée. |

---

## Hooks

Scripts d'automatisation prêts à l'emploi pour les hooks de l'agent.

| Script | Ce qu'il fait |
|--------|-------------|
| [notify.sh](hooks/notify.sh) | Notifications push (Pushover/Telegram) + son à la fin d'une tâche |
| [tab-title.sh](hooks/tab-title.sh) | Renomme automatiquement l'onglet du terminal selon le projet en cours |
| [layout-hook-start.sh](hooks/layout-hook-start.sh) | Étend le layout tmux quand des subagents apparaissent |
| [layout-hook-stop.sh](hooks/layout-hook-stop.sh) | Réduit le layout + notifie quand tous les agents ont terminé |
| [platform.sh](hooks/platform.sh) | Détection multi-plateforme (macOS/Linux/WSL2) |

**Installation :**

```bash
cp hooks/*.sh ~/.claude/scripts/
chmod +x ~/.claude/scripts/*.sh
```

---

## Accès distant

[Toutes les façons de faire tourner l'agent à distance](remote/access-guide.md) — depuis un iPhone, une tablette ou une autre machine. Comparaison en conditions réelles, pas une fiche technique.

| Méthode | UX mobile | Fiabilité | Installation |
|--------|-----------|-------------|-------|
| SSH + tmux (Blink/Termius) | Faible | Excellente | Moyenne |
| Mosh + tmux | Correcte | Meilleure | Moyenne |
| Pont Telegram Bot | Bonne | Bonne | Élevée |
| OpenClaw (app iOS) | Bonne | Bonne | Faible |
| Remote Control natif | Meilleure | Bêta | Faible |
| Bureau à distance (AnyDesk/Jump) | Correcte | Bonne | Faible |

---

## Vue d'ensemble du setup

L'inventaire complet. [Détail →](setup/overview.md)

| Composant | Nombre |
|---|---|
| Plugins officiels | 40+ |
| Skills personnalisés | 109 |
| Personas d'agents | 38 (cinéastes, philosophes, artistes) |
| Commandes slash personnalisées | 16 |
| Serveurs MCP personnalisés | 3 |
| Scripts d'automatisation | 30+ |

---

## Principes fondamentaux

**L'IA comme altération, pas comme augmentation.** L'agent n'est pas là pour me rendre plus rapide. Il est là pour rendre le travail différent de ce que j'aurais fait seul.

**Les specs avant le code.** Chaque projet commence par une phase d'interview. Pas d'implémentation sans specs. Deux skills personnalisés l'imposent : `/interview` et `/pitch`.

**7 règles de qualité** intégrées au fichier d'instructions :

1. **Anti-hallucination** — jamais de données simulées, inventées ou approximatives
2. **Retour au mode plan** — si un fix échoue, on s'arrête. On ne s'enfonce pas. On replanifie.
3. **Scrap and redo** — quand le résultat est médiocre, on repart de zéro avec le contexte accumulé
4. **Auto-actualisation** — l'agent met à jour ses propres règles après chaque erreur significative
5. **Utiliser des subagents** — paralléliser les tâches complexes, garder le contexte principal propre
6. **Vérifier son travail** — jamais dire "fait" sans preuve
7. **Mode confrontation** — challenger les choix, dire non quand c'est justifié

---

## Stack

| Couche | Outils |
|-------|-------|
| **IA** | Plan Max (200$/mois), modèles frontière, Agent Teams |
| **Terminal** | Ghostty (GPU), tmux, cmux |
| **Matériel** | MacBook Air M3 + Mac Mini M4 (24/7) + PC RTX 5090 |
| **Réseau** | VPN maillé Tailscale, mosh |
| **Mobile** | Bot Telegram, Blink Shell (SSH), Remote Control |
| **Auto-hébergé** | ComfyUI (GPU), planification réseaux sociaux |
| **Mémoire** | BACKLOG.md, logs de session, recherche Spotlight |

---

## Essais

| Essai | Sujet |
|-------|-------|
| [On Agentic Engineering](essays/2026-02-25-on-agentic-engineering.md) | Notes d'un cinéaste qui a traversé plusieurs ruptures technologiques — et reconnaît celle-ci. |
| [L'échelle d'adoption et la scripte](essays/2026-07-17-the-adoption-ladder-and-the-scripte.fr.md) ([EN](essays/2026-07-17-the-adoption-ladder-and-the-scripte.md)) | Ce qu'un modèle de maturité d'ingénieur ne voit pas du travail créatif : les boucles de vérification du cinéma étaient faites de personnes, une pratique solo doit les reconstruire en agents. |

## Journal

| Entrée | Sujet |
|-------|-------|
| [Genesis](journal/2026-02-15-genesis.md) | Jour zéro. Audit complet face aux 25 conseils de gmoney.eth. A abouti à 7 nouvelles règles, la statusline, les règles de refus, les logs de mémoire. |

## Sources

| Source | Ce qu'on en a retenu |
|--------|----------------|
| [gmoney.eth — 25 leçons](sources/2026-02-15-gmoney-25-tips.md) | Chaque conseil noté : implémenté, exploré, ou écarté. |
| [Karpathy — Vibe Coding](sources/2025-02-02-karpathy-vibe-coding.md) | Le tweet original (6,7M vues). Pourquoi ce dépôt rend le vibe coding tenable. |

---

## Influences

- [Trail of Bits](https://github.com/trailofbits/claude-code-config) — réglages de sécurité tranchés
- [@bcherny](https://x.com/bcherny) (créateur du CLI) — conseils d'équipe
- [@gmoneyNFT](https://x.com/gmoneyNFT) — 25 leçons d'un usage quotidien multi-agents
- [@__BOMO](https://x.com/__BOMO) — statusline d'usage du contexte

---

*Ceci n'est pas un template à copier. C'est une référence pour quiconque construit sa propre relation avec un outil d'IA de code — en particulier les non-développeurs qui l'utilisent pour un travail créatif.*
