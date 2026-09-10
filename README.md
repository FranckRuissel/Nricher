# Ventes e-commerce : nettoyage et enrichissement

Deux tâches sur un fichier de 525 000 lignes de ventes d'ameublement.

1. Corriger les erreurs de catégorisation de la colonne `Nature`.
2. Extraire la dimension et la couleur depuis le libellé produit.

Le livrable est le fichier `ventes_enrichies.xlsx`. La première tâche s'appuie sur un
classifieur TF-IDF et régression logistique, utilisé comme outil de nettoyage automatique :
les incohérences sont trop nombreuses pour être reprises à la main, et aucun échantillon de
référence n'accompagne le fichier. Le détail de la démarche est dans `NOTE_EXPLICATIVE.md`.

## Installation

```bash
pip install -r requirements.txt
```

## Utilisation

```bash
jupyter notebook analyse_ecommerce.ipynb
```

Le notebook est autonome : toutes les étapes, des constantes aux fonctions, y sont définies
puis exécutées dans l'ordre. L'exécuter de bout en bout régénère `ventes_enrichies.xlsx`.

Temps de calcul : la première lecture du `.xlsb` demande environ 45 minutes, la bibliothèque
pyxlsb étant lente sur ce volume. Une copie `ventes_cache.csv` est écrite à ce moment-là et
relue en une seconde ensuite. L'entraînement dure 40 secondes, l'écriture du `.xlsx` final
environ 3 minutes.

## Fichiers

| Fichier | Rôle |
| --- | --- |
| `analyse_ecommerce.ipynb` | traitement complet, commenté et contrôlé à chaque étape |
| `NOTE_EXPLICATIVE.md` | démarche, choix techniques et limites |
| `requirements.txt` | dépendances |
| `ventes_enrichies.xlsx` | fichier de sortie |
