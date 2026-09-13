[English](2026-09-13-editing-with-agents-by-type-of-work.md) · **Français**

# Monter avec des agents, par type de travail

*Note de terrain, 13 septembre 2026. Tout ce qui suit a été vérifié ce jour-là sur mes propres machines. Quand je n'ai fait que lire une page, je le dis.*

Je couvre presque toutes les situations de montage qui existent. Un long métrage documentaire coupé à la main sur des mois. Un long métrage hybride en développement, où une partie de l'image est générée. Des extraits et des teasers pour les réseaux en trois formats. Des démonstrations de mon propre poste en ligne de commande. Des titres et du motion. Des boucles d'exposition. Et les outils que je fabrique pour tout ça. J'ai longtemps cherché un seul stack qui serve à tout. Il n'existe pas, et la recherche elle-même était l'erreur. La question qui aide vraiment est : **pour ce type de travail, où se juge le montage, qu'est-ce que l'agent lit et écrit, et par quel canal ?**

## Pistes, calques, et la question en dessous

Un fil lu cette semaine-là soutenait que les agents se perdent dans le modèle en pistes d'un éditeur classique (Premiere, la page Edit de Resolve) et s'en sortent mieux avec un arbre d'objets, comme After Effects imbrique ses calques ou comme un navigateur expose le DOM. Diffusion Studio est passé des pistes aux calques exactement pour cette raison. L'argument est juste pour un modèle de langage qui écrit du code. Il est aussi plus vieux qu'il n'en a l'air : Final Cut a abandonné les pistes numérotées en 2011 pour une spine et des clips connectés, et Fusion est un graphe de nœuds depuis toujours.

Le débat pistes contre calques cache la vraie question : **quel objet l'agent lit et écrit, et par quel canal.** Il y a trois canaux, et je les utilise tous les trois :

1. **Une API vivante sur l'application ouverte.** DaVinci Resolve, par son API de scripting et, depuis la 21.1, un serveur MCP livré par Blackmagic. L'agent adresse un plan de timeline comme un objet. À ce niveau, le débat sur les pistes disparaît.
2. **Un fichier d'échange que l'humain importe.** FCPXML pour Final Cut, OpenTimelineIO entre tous. Asynchrone, structure seulement, jamais la couleur ni les effets. Final Cut n'a aucune API de scripting (`sdef` sur l'application ne rend rien), c'est donc sa seule porte.
3. **Du code qui est le document.** HyperFrames (HTML et CSS), Remotion (React), Motion Canvas (TypeScript), le moteur de Diffusion Studio (TypeScript). L'agent y est chez lui, et personne ne coupe un long métrage dans du HTML.

Un film d'auteur vit dans les canaux 1 et 2. La communication vit dans les canaux 1 et 3. Le trou que tout le monde nomme, un outil qui coupe les rushes, compose comme After Effects, livre un DCP et appartient à l'agent de bout en bout, reste ouvert, et je n'ai pas besoin de le fermer avant mon prochain premier assemblage.

## Un stack par type de travail

| Type de travail | Où se juge le montage | Où l'agent agit, par quel canal | Ce que l'agent fait | Ce qu'il ne fait jamais |
|---|---|---|---|---|
| **A. Long métrage d'auteur : documentaire et montage narratif** | Final Cut Pro. Ma coupe, mon œil, et un monteur ou une monteuse qui vit dans Final Cut, en collaboration sur une bibliothèque partagée | Canal fichier : FCPXML écrit par mon propre indexeur, OpenTimelineIO ; transcription locale ; notes et journal | Transcrit, indexe, propose des sélections et un premier assemblage avec marqueurs et rôles, tient la documentation à jour | Le master. Le sens. La coupe finale |
| **B. Hybride : animation, composite, VFX, image et son générés** | DaVinci Resolve Studio, timeline 32 bits float ACES, Fusion pour le nodal. Tourne sur Mac et Windows, donc aussi sur les tours NVIDIA. Une équipe VFX distincte de l'équipe Final Cut. Moins de scènes narratives à couper, plus de détail plan par plan | API vivante (MCP natif, scripting) ; un magasin de plans qui versionne chaque prise ; une couche de verbes qui pilote le NLE ; ComfyUI sur les tours | Génère sur les tours, pose une version au timecode, désactive la précédente, écrit le sidecar et le manifeste C2PA | Choisir la prise qui reste |
| **C. Réseaux : extraits, teasers, multi-format** | La sortie de l'agent, relue par moi ; un passage par Final Cut seulement quand le grain du film compte | Headless : lanceur de pipelines, ffmpeg, transcription locale ; canal 3 pour l'habillage | Coupe, sous-titre, recadre en 16:9 / 9:16 / 1:1, produit des variantes, exporte | Publier sans mon accord |
| **D. Démonstrations techniques de mon propre poste** | Rendu direct, pas de NLE | Capture d'écran plus Remotion ; HyperFrames pour les vidéos de dépôt et de page | Écrit le script, génère l'habillage, le faux terminal, les variantes de durée | Rien d'exclu : c'est là que l'agent peut tout faire |
| **E. Motion, titres, habillage** | Motion (Apple) quand ça entre dans un film ; HyperFrames ou Remotion pour le web et les réseaux | Canal 3 | Produit des séquences autonomes que je réimporte | Décider du graphisme du film |
| **F. Installation, boucle d'exposition** | Resolve pour l'export, la boucle et le calibrage | API vivante pour les exports et les conformations | Encode, décline, vérifie les formats de diffusion | Le dispositif lui-même |
| **G. Recherche et développement d'outils** | Aucun montage : du code | Le moteur de Diffusion Studio comme code à lire, de petits éditeurs locaux comme laboratoires d'agent | Prototype | Devenir une source de vérité de plus |

Quatre règles sortent de la table :

- **Un même rush traverse plusieurs lignes.** Une interview est en A (le film), puis en C (l'extrait), puis en E (l'habillage de l'extrait). Trois montages différents, trois niveaux de délégation. Un seul pipeline ne servirait bien aucun des trois.
- **Le grain commun entre A et C vient des presets.** Les mêmes LUT, les mêmes titres Motion, les mêmes rôles Final Cut, rangés une fois et réutilisés par la ligne réseaux.
- **Plus le montage est court, plus l'agent en fait.** En D il fait tout, en A il prépare. C'est la seule règle qui gouverne le choix de l'outil.
- **Entre A et B, le partage se fait par séquence, jamais par plan.** FCPXML et OpenTimelineIO transportent la structure et rien d'autre. Un plan qui fait des allers-retours entre l'équipe Final Cut et l'équipe Resolve se conforme deux fois. Une séquence hybride part entière en B et revient en A comme un seul média rendu.

## Ce que j'ai mesuré ce jour-là

**Resolve Studio 21.1.0.14, MCP natif, cinq tests scriptés.** Connexion et liste des projets : oui. Bac à sable : `import os` refusé, l'accès aux fichiers n'existe pas. Une séquence de 25 EXR float32 importée comme un seul clip 32 bits et posée au timecode sur une timeline créée par script, à côté d'un ProRes et d'un EXR isolé : oui. `ValidateDCTL` accepte un noyau valide et refuse n'importe quoi avec un message lisible. `AutoAlignClips` et `NormalizeAudioLevel` existent sur la timeline ; `AddTransition`, `SetFades`, `SetSpeed`, `AddVersion`, `AddTake`, `AddFusionComp`, `SetCDL` et `SetClipEnabled` existent sur l'élément. Désactiver puis réactiver un plan par script fonctionne, et c'est exactement le verbe dont la ligne B a besoin.

Un piège à connaître : un dialogue modal dans Resolve (chez moi, un emplacement de cache qui pointait vers un disque disparu) fait rendre `0` à tout import et `None` à la page courante, sans aucune erreur. Le MCP ne voit pas le dialogue. Avant de conclure qu'un import échoue, lister les fenêtres de l'application.

**HyperFrames 0.8.37** (HeyGen, Apache 2.0). Rend une page HTML minutée par GSAP en MP4 par Chrome headless et FFmpeg. Sur un cas réel, une présentation de 30 secondes d'un de mes dépôts publics en 16:9 et 9:16, écrite par l'agent, rendue en local sans aucune clé HeyGen. Deux rendus du même fichier sont identiques octet pour octet. Le paquet est lourd (371 Mo, onnxruntime, sharp, puppeteer-core), le Chrome système doit être pointé explicitement, et ça échoue dans un terminal isolé parce que Chrome refuse le bac à sable de macOS. Il entre dans le stack pour les lignes D et E, et reste hors du montage.

**Remotion.** Une démonstration de 20 secondes, faux terminal sombre, un titre, rendue en 1920x1080 et 1080x1920 en local, déterministe, propre. La seule friction est Chromium, à préinstaller et pointer pour garantir un rendu hors ligne. Gratuit pour un entrepreneur individuel. Oui pour la ligne D, et un agent le pilote mieux qu'HyperFrames : les composants se composent, une page monolithique devient vite dense.

**La recette `screen-demo` d'un lanceur de pipelines** (OpenMontage). La préparation éditoriale, les points de contrôle et les artefacts sont exploitables ; le composeur ne rend pas et sa sortie de repli n'est pas livrable. À réparer avant de compter dessus pour la ligne C.

## Les startups, vérifiées sur le web le même jour, pas testées

- **Palmier** (palmier.io) : un NLE natif Mac, macOS 26, avec un serveur MCP local pour les agents de code, transcription locale, export FCPXML 1.10 à 1.14 et XMEML. Version 0.9.0 le 9 septembre 2026, environ 14 400 étoiles. GPLv3 jusqu'à la 0.7.6, binaires propriétaires depuis la 0.8.1 du 28 août. Le seul nom nouveau qui vaut un test pour la ligne C, un montage piloté par l'agent avec aller-retour Final Cut. Jamais le master.
- **Descript** a un connecteur MCP officiel depuis le 11 juin 2026 (API en bêta ouverte depuis le 16 avril). Utile pour les interviews et les déclinaisons par le texte, ligne C.
- **Mosaic** (YC W25) : API REST, webhooks, un skill publié pour les agents de code, projet Premiere en sortie. Cloud, échelle agence. Un lanceur de pipelines local fait déjà ce que j'en copierais.
- **Cardboard** (YC W26), **Narrative** (YC F25), **Kestroll** (YC S25) : réels, actifs, SaaS. Aucune API, MCP ou skill public chez Cardboard et Narrative ; Kestroll exporte du FCP7 XML et du FCPXML. Des laboratoires.
- **Timeline Studio** : MIT, 1.0.4 du 17 août 2026, skill et CLI, MCP seulement planifié, aucun échange NLE documenté. Un laboratoire, en dessous de Palmier.
- **ChatCut** : un plugin MCP pour agents de code, export Premiere et Resolve, pas Final Cut. Laboratoire.
- **Diffusion Studio** : le moteur `core` est en TypeScript sous MPL-2.0 avec un rendu WebCodecs ; l'éditeur est en alpha depuis mai 2026 avec une CLI, sans MCP ni échange NLE. Un moteur à lire, pas une timeline.

## Avant la timeline

Un second fil du même jour portait sur les canevas infinis (Firefly Boards, Flora, Jaaz, Inline Studio) et les pipelines d'animation. Trois choses tiennent. Le canevas vient *avant* la timeline. Un film est du temps, et un mur en 2D ne porte ni le rythme ni le raccord ; le canevas sert à comparer des mondes (un repérage réel à côté d'un ciel généré, trois lumières, un plan large contre un insert) et à baisser le coût de se tromper avant d'allumer une caméra ou de payer une génération. Le mur et l'usine sont deux outils : un board sert à poser et comparer, un graphe sert à câbler et relancer, et le graphe ne gagne sa place qu'une fois qu'une recette se répète. Et pour l'animation à plans générés, l'animatique est la porte : dessin (le trait est le jeu de données des LoRA, pas une décoration), LoRA, bible, storyboards, animatique avec son temporaire, images clés, mouvement, montage. Générer de la vidéo avant l'animatique et les LoRA verrouillées, c'est payer pour découvrir que le film n'existe pas encore.

## Ce que je garde

Une position de travail, tenue tant qu'elle marche : pas de maison unique. Final Cut est l'endroit où je coupe à la main et où un monteur Final Cut peut me rejoindre. Resolve Studio est l'hôte de tout ce que l'agent doit faire seul sur une timeline, et de la conformation, de la couleur et de l'hybride, sur Mac comme sur les tours Windows. FCPXML et OpenTimelineIO font le pont, et ils ne transportent que la structure. HyperFrames et Remotion font le motion et les démonstrations. Un lanceur de pipelines local rejoue mes recettes pour les réseaux. Le jour où un outil coupe, compose et livre mieux que ce pont, la position change.
