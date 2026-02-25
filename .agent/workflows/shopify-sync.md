---
description: Synchronisation avec GitHub avant de travailler sur le thème Shopify
---

# Synchronisation GitHub pour le thème Shopify

## ⚠️ IMPORTANT - À faire AVANT chaque session de travail

Avant de commencer à travailler sur le thème Shopify, tu DOIS toujours vérifier et synchroniser avec GitHub. Des changements peuvent avoir été faits directement sur Shopify.com par l'utilisateur, ce qui crée automatiquement un commit sur la branche `main` sur GitHub.

## Étapes de synchronisation

// turbo
1. **Vérifier l'état actuel du dépôt local**
```bash
cd /Users/david/dev/ValentineLambert_shopify && git status
```

// turbo
2. **Récupérer les derniers commits depuis GitHub**
```bash
git fetch origin
```

// turbo
3. **Vérifier s'il y a des commits en avance sur origin/main**
```bash
git log HEAD..origin/main --oneline
```

4. **Si des commits distants existent, les intégrer**
```bash
git pull origin main
```

5. **Si tu as des changements locaux non commités, les sauvegarder d'abord**
```bash
git stash
git pull origin main
git stash pop
```

## Pourquoi c'est important

- Shopify synchronise automatiquement les thèmes avec GitHub
- Les modifications faites via l'éditeur de thème sur Shopify.com créent des commits
- Sans cette synchronisation, on risque des conflits ou de perdre des changements

## Après avoir terminé le travail

// turbo
1. **Commiter et pousser les changements**
```bash
git add .
git commit -m "Description des changements"
git push origin main
```

Cela permet à Shopify de récupérer les modifications et de les appliquer au thème.
