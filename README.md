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

 toutes les étapes, des constantes aux fonctions, y sont définies
puis exécutées dans l'ordre. L'exécuter de bout en bout régénère `ventes_enrichies.xlsx`.

## Fichiers

| Fichier | Rôle |
| --- | --- |
| `analyse_ecommerce.ipynb` | traitement complet, commenté et contrôlé à chaque étape |
| `NOTE_EXPLICATIVE.md` | démarche, choix techniques et limites |
| `requirements.txt` | dépendances |
| `ventes_enrichies.xlsx` | fichier de sortie |
