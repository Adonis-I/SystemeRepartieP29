# Guide — Publier ce projet sur GitHub

Ce fichier explique comment mettre le projet en ligne. Tu peux le supprimer une fois le dépôt créé.

## Option A — Tout à la main sur le site GitHub (le plus simple)

1. Va sur https://github.com et connecte-toi.
2. Clique sur **New** (nouveau dépôt), nomme-le par exemple `SystemeRepartieP10`.
3. Laisse-le **public** (ou privé selon ta préférence), ne coche rien d'autre, clique **Create**.
4. Sur la page du dépôt vide, clique **uploading an existing file**.
5. Glisse-dépose tous les fichiers et dossiers de ce projet.
6. Clique **Commit changes**. C'est en ligne.

## Option B — En ligne de commande (Git)

Depuis le dossier du projet :

```bash
git init
git add .
git commit -m "TP benchmark CPU vs GPU - binome En-Najdy / Kraichy"
git branch -M main
git remote add origin https://github.com/<ton-compte>/SystemeRepartieP10.git
git push -u origin main
```

Remplace `<ton-compte>` par ton nom d'utilisateur GitHub.

## Ordre de remplissage conseillé

1. **Maintenant** : pousse le README, le notebook, requirements.txt (déjà prêts).
2. **Après avoir exécuté le notebook sur Colab** : ajoute dans `results/` ton fichier CSV de
   résultats et les images des graphiques (clic droit → enregistrer l'image depuis Colab).
3. **Ensuite** : ajoute le rapport PDF dans `rapport/` et la présentation dans `presentation/`.
