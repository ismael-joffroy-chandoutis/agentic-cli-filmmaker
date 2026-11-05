[English](2026-11-05-local-versus-api-embeddings.md) · [Français](2026-11-05-embeddings-locaux-contre-api.fr.md)

# 5 novembre 2026 : embeddings locaux contre API

Journal de bord. Un banc mené avec Claude Code pour savoir si une recherche locale dans des archives de travail tient face au modèle d'embeddings de Google par API : le cas, le protocole, les résultats question par question, le coût, ce qui reste ouvert.

## Le cas

Des archives de travail (rapports, notes, enregistrements, images) où il faut retrouver un passage précis. Une partie de ces archives ne doit pas quitter la machine. La question : jusqu'où va une recherche qui tourne entièrement en local, et ce qu'on gagne à envoyer le corpus à une API.

## Les deux modèles

mediarag est un outil local : il découpe les fichiers, calcule les vecteurs, les range dans une base SQLite et répond aux recherches par MCP. Il embarque EmbeddingGemma 2, modèle ouvert de Google (Apache 2.0) sorti le 06/10/2026 : 740 millions de paramètres, 768 dimensions, 8 192 jetons, texte, image et audio. Il tourne sur le processeur, hors ligne après téléchargement. Score publié sur MTEB multilingue v2 : 61,36.

Gemini Embedding 2 (`gemini-embedding-2`) est le modèle d'embeddings multimodal de Google par API, disponible pour la production depuis le 22/04/2026. Il accepte texte, image, vidéo, audio et PDF, jusqu'à 8 192 jetons, avec des vecteurs de 128 à 3 072 dimensions. Score publié sur MTEB multilingue : 69,9. Aucun des deux n'a de score français isolé publié.

La comparaison porte donc sur deux modèles, l'un dans un outil complet, l'autre derrière une API qu'il faut entourer d'un script. mediarag ne sait pas appeler Gemini : son code ne contient aucune option de modèle.

## Le protocole

Le corpus : un dossier de rapports de travail d'une seule nuit, jugé non sensible, 9,9 Mo. 71 fichiers : 64 textes (Markdown, HTML, texte brut), 6 enregistrements d'un agent vocal (120 secondes au total), 1 image. mediarag en a tiré 1 531 morceaux : 1 509 de texte, 20 extraits audio, 2 entrées image.

Côté Gemini, les mêmes 1 531 morceaux, lus dans la base de mediarag, extraits audio découpés aux mêmes minutes. Vecteurs de 768 dimensions, comme EmbeddingGemma 2. Consignes de tâche recommandées par Google (`title: … | text: …` pour les documents, `task: search result | query: …` pour les questions). Recherche par similarité seule.

Une différence de méthode reste : mediarag combine similarité et mots-clés, Gemini n'a ici que la similarité.

Cinq questions, dont la bonne réponse a été vérifiée dans les fichiers. Pour chacune, les trois premiers résultats sont lus.

## Question par question

1. La latence mesurée d'un agent vocal local. La réponse (0,90 seconde en médiane, de la fin de la phrase au premier son) se trouve dans un rapport de mesure. Les deux ratent. mediarag rend la consigne qui demandait de mesurer la latence. Gemini rend une note de recherche qui cite des latences relevées ailleurs (400 à 700 ms). Le rapport qui donne la mesure n'est dans les trois premiers ni d'un côté ni de l'autre.

2. L'outil le plus rapide dans un banc de modèle local. Les deux réussissent, chacun sur un passage différent du même rapport : la conclusion pour mediarag, le tableau des temps pour Gemini.

3. Retrouver un échange précis parmi les enregistrements de l'agent vocal (recherche audio). Gemini réussit : le bon échange arrive en premier. mediarag place un autre échange en premier et le bon en deuxième et troisième. Dans les deux cas, le résultat est un extrait audio avec ses minutes, sans les mots : pour savoir ce qui est dit, il faut écouter ou transcrire.

4. La taille d'un cache système dans un rapport de ménage disque. mediarag réussit : le passage qui porte le chiffre arrive en premier. Gemini rend d'abord un passage du même rapport sans le chiffre, puis le bon en deuxième.

5. Une photo de bureau de monteur (recherche d'image). Les deux réussissent.

Bilan identique : 3 réussis, 1 à moitié, 1 raté de chaque côté, sur des questions différentes.

## Ce que chaque approche rate

La question 1 est ratée des deux côtés pour la même raison. Un document qui parle de latence dans des termes proches de la question passe devant le rapport qui donne le chiffre. Ici, la consigne ressemble plus à la question que le rapport chiffré.

mediarag range moins bien l'audio. Il trouve mieux le passage qui contient le chiffre exact, probablement grâce à sa part de mots-clés.

Gemini range mieux l'audio et rate le passage chiffré quand un passage voisin du même document est plus proche du sens de la question.

L'écart de 8,5 points sur MTEB multilingue ne se voit pas sur ce banc. Cinq questions ne suffisent pas à départager deux modèles.

## Temps et coût

| | mediarag (EmbeddingGemma 2) | Gemini Embedding 2 |
|---|---|---|
| Indexation des 1 531 morceaux | 496 s sur le processeur d'un ordinateur portable | 33 s, par lots de 50 requêtes |
| Recherche | 0,97 s pour la première (chargement du modèle), 0,03 s ensuite | |
| Coût | 0 $ | environ 0,12 $ ; moins de 0,0001 $ pour les cinq questions |
| Ce qui sort de la machine | rien | le texte des 64 fichiers, les deux minutes d'audio et l'image |
| Place prise | environ 1,8 Go (environnement Python, modèle, index de 11 Mo) | |

Le coût a été annoncé avant lancement, avec un plafond de 0,50 $ et une coupure automatique réglée à 0,45 $, jamais atteinte. L'estimation préalable, au tarif publié par Google le 30/10/2026, donnait environ 0,12 $ (0,20 $ par million de jetons de texte, 0,00016 $ par seconde d'audio). Pendant le passage, le compteur comptait large (un jeton pour trois caractères) et affichait 0,135 $. Le premier lot a été facturé 17 555 jetons pour 19 581 estimés, soit 10 % de moins.

Sur l'offre payante, la page de prix de Google indique que les données ne servent pas à entraîner les modèles. Sur le niveau gratuit, Google se réserve le droit de les utiliser pour améliorer ses produits.

Gemini indexe environ 15 fois plus vite que le modèle local, pour quelques centimes.

## Ouvert

- Indexer les transcriptions des enregistrements comme du texte, à côté de l'audio, pour que la recherche rende aussi les mots.
- Ajouter un reranker, ou une part de mots-clés côté Gemini, pour les questions qui attendent un chiffre, puis rejouer la question 1.
- Refaire le banc avec plus de questions pour mesurer l'écart réel entre les deux modèles, en particulier en français.
- Brancher Gemini Embedding 2 dans mediarag demanderait de modifier l'outil.
