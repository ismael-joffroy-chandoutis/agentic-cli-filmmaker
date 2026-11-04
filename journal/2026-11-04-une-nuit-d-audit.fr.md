[English](2026-11-04-a-night-of-audit.md) · [Français](2026-11-04-une-nuit-d-audit.fr.md)

# 4 novembre 2026 : une nuit d'audit

Journal de bord. Un système Claude Code qui tourne sur plusieurs machines pour un artiste et cinéaste : ce qui a été mesuré, ce qui a cassé, ce qui a été corrigé, ce qui reste ouvert.

## Ce qu'une session charge avant le premier mot

Mesuré sur le journal d'une session réelle : avant que la personne tape quoi que ce soit, une session charge environ 322 Ko de texte, soit 85 000 à 95 000 tokens. La moitié vient de la liste des agents spécialisés (167 Ko pour 387 descriptions). Viennent ensuite le fichier d'instructions global (49,5 Ko), l'index de mémoire, la liste des 571 skills et les noms de 631 outils chargés à la demande.

La documentation d'Anthropic recommande un fichier d'instructions sous 200 lignes, et de transformer en hook toute règle qui ne souffre aucune exception. La proposition : garder toutes les règles, déplacer les procédures longues dans des fichiers lus à la demande, ramener chaque description d'agent à 200 caractères. Gain estimé de 90 000 à 35 000 tokens par session. C'est une estimation, à mesurer après application.

## Le go qui s'armait tout seul

Rien ne part vers un humain sans un message explicite « go envoi <nom> ». Un hook bloque toute commande d'envoi (mail, messageries) tant qu'un message humain ne l'a pas armé, pour trente minutes et un envoi.

L'audit a trouvé que le motif d'armement acceptait le simple mot « go ». Résultat dans le journal du hook : 23 armements à tort, dont trois sur la phrase « ne lance rien sans go ». La barrière s'ouvrait sur des phrases qui demandaient précisément qu'elle reste fermée.

Correctif : le motif exige désormais « go envoi » suivi d'un nom, exclut les négations (« sans go », « attends mon go ») et passe un banc de douze phrases, qui doivent armer ou non. Douze sur douze.

## Les pannes silencieuses

Le registre des pannes silencieuses compte 105 entrées depuis août : des automatismes qui n'ont rien fait sans que rien ne le signale. Un test unique les trouve : s'il ne fait rien, qu'est-ce que je vois ?

Cette nuit en a ajouté plusieurs. Sur le Mac mini serveur, le gestionnaire de tâches de macOS figé depuis la veille au soir : une surveillance censée tourner toutes les deux minutes affichait 4 821 lancements à 01h20 et toujours 4 821 à 01h33. Une fuite mémoire du noyau macOS : 9,49 Go sur les 11,6 Go de mémoire câblée dans une seule zone (`data.kalloc.1024`), alors que tous les programmes réunis n'en occupent pas 2. Seul un redémarrage la rend. Et pendant la rédaction de ce journal, le disque externe de travail s'est démonté quelques secondes : un fichier écrit à ce moment a été perdu sans aucune erreur, retrouvé à sa taille d'origine. Il a fallu le refaire et vérifier son contenu.

La méthode et dix-huit cas anonymisés sont publiés dans [proof-gate](https://github.com/ismael-joffroy-chandoutis/proof-gate).

## Agnostique

Trois couches se séparent : le modèle, le harness (Claude Code, ou un autre), les fichiers et les outils. Sur 332 skills, 323 suivent déjà le format ouvert Agent Skills, lisible par d'autres outils. Le système principal reste sur Claude Code, qui est le seul endroit où le pilotage à distance, les Artifacts et les connecteurs fonctionnent ensemble. Ce qu'aucun autre harness ne fait tourner tel quel, c'est la barrière d'envoi. C'est la pièce la plus coûteuse à porter.

## Des sessions qui deviennent des bots

6 799 conversations en cinq mois, 2 245 rien qu'en octobre. Compter les sujets qui reviennent dit quelles sessions méritent de devenir des bots permanents, chacun avec sa propre petite interface, sur le modèle d'[Inboxt](https://github.com/ismael-joffroy-chandoutis/inboxt), la boîte de réception publiée aujourd'hui. La règle ne change pas : le bot prépare, la personne décide.

## Ouvert

- Appliquer l'allègement du fichier d'instructions, puis mesurer le gain réel.
- Mettre le banc des douze phrases dans la vérification automatique quotidienne.
- Redémarrer le Mac mini serveur pour vider la fuite du noyau, et surveiller qu'elle revient.
