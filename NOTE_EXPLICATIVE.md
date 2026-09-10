# Note explicative

## Nature de l'exercice

Ce livrable traite deux tâches de nettoyage et d'enrichissement de données sur un fichier
de ventes e-commerce de 525 034 lignes :

1. Détection et correction des erreurs de catégorisation.
2. Extraction des dimensions et couleurs depuis les libellés produits.

L'approche est celle du data engineering : produire un fichier de sortie propre à partir
d'un traitement reproductible. Un classifieur (TF-IDF + LogisticRegression) est utilisé
pour la première tâche comme outil de nettoyage automatique, pas comme un projet de data
science au sens optimisation de métriques ou comparaison rigoureuse de modèles.

Les choix techniques privilégient la simplicité, l'explicabilité et la robustesse plutôt
que la sophistication.

## Constat de départ

Le fichier contient 596 catégories (`Nature`) pour 56 012 libellés produit distincts.
2 189 de ces libellés, soit 89 200 lignes, portent plusieurs Natures différentes dans le
fichier source : ce sont des erreurs de saisie, pas une ambiguïté réelle du produit.

## Tâche 1 : correction des catégories

Le classifieur est entraîné sur les lignes déjà catégorisées, puis appliqué à l'ensemble du
fichier pour détecter les libellés dont la Nature ne correspond pas au reste du catalogue.
Trois décisions structurent cette étape :

- L'apprentissage porte sur les 56 012 libellés distincts et non sur les 525 034 lignes, un
  produit vendu en volume ne pesant pas davantage qu'un autre dans l'entraînement.
- Seule la Nature majoritaire de chaque libellé est apprise, pour ne pas apprendre puis
  généraliser les incohérences relevées ci-dessus.
- Une Nature n'est remplacée qu'au-delà de 75 % de confiance. En dessous, la valeur
  d'origine est conservée.

**Résultat mesuré** : 18 344 lignes recatégorisées, soit 3,5 % du fichier. Les corrections
les plus fréquentes touchent des matelas rangés en « Meuble à chaussures » et des canapés
rangés en « Friteuse », deux erreurs de saisie visibles dans les données sources.

## Tâche 2 : extraction des dimensions et des couleurs

Aucun modèle ici. Le format des dimensions est régulier et le vocabulaire de couleurs est
fermé, deux règles déterministes suffisent et restent auditables.

- Dimension : un motif de type `120x60` ou `120 x 60 x 75`, normalisé en une seule écriture.
- Couleur : la première entrée d'un vocabulaire fermé de 25 couleurs reconnue dans le
  libellé.

**Résultat mesuré** : dimensions trouvées sur 30,1 % des lignes, couleurs sur 22,4 %.

## Limites

- Le classifieur apprend sur les étiquettes existantes. Le vote majoritaire par libellé
  limite la contagion des erreurs, mais une catégorie fausse à plus de 50 % le reste.
- Aucune vérité terrain n'accompagne le fichier. Le taux de correction est constaté sur les
  cas les plus fréquents, pas mesuré. Une relecture manuelle d'un échantillon serait la
  prochaine étape.
- Les 11 745 lignes sans Nature sont des services et non des produits, par exemple « 1 an
  d'assistance téléphonique PC ». Elles n'ont pas d'Univers non plus, sur 17 687 lignes sans
  Univers au total, et restent vides après traitement.
- Le fichier source a perdu des accents à l'export : « chêne » y est toujours écrit
  « ch ne », sur environ 8 000 lignes. Les cas rencontrés sont corrigés au cas par cas.
- Le vocabulaire de couleurs est fermé. Il rate les nuances commerciales comme « bois
  flotté » au profit d'une colonne exploitable en aval.
- Le motif de dimension retient la première occurrence et ignore l'unité (cm ou mm).

## Positionnement de l'exercice

Cet exercice relève du data engineering : nettoyage et enrichissement d'un fichier de
données pour un usage aval. Le classifieur est un outil de nettoyage, pas un livrable de
data science.

Une vraie démarche de data science aurait exigé :

- Un jeu de test étiqueté manuellement pour mesurer la précision réelle.
- Une comparaison rigoureuse de plusieurs modèles (embeddings SBERT, modèles de type BERT
  fine-tuné).
- Une validation croisée stratifiée par Nature.
- Un tuning fin des hyperparamètres.
- Des métriques par classe (précision, rappel, F1).

Ces étapes n'étaient pas nécessaires ici : l'objectif était de produire un livrable
exploitable, avec un compromis simplicité / efficacité adapté à la taille du fichier et au
temps disponible.
