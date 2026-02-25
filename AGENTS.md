# Instructions pour les Agents IA

Ce fichier contient les instructions pour tous les agents IA (Claude, Gemini, GPT, etc.) travaillant sur ce projet.

---

## 📋 À propos du projet

| Élément | Valeur |
|---------|--------|
| **Projet** | Thème Shopify Horizon pour Valentine Lambert |
| **Boutique** | valentine-lambert.myshopify.com |
| **Thème actif** | `valentine-lambert-shopify/main` (synchronisé avec GitHub) |
| **Thème de base** | Horizon (thème officiel Shopify) |
| **Dépôt GitHub** | https://github.com/dpitch/valentine-lambert-shopify |

> ⚠️ **IMPORTANT** : Le thème est **synchronisé automatiquement avec GitHub**. Ne JAMAIS utiliser `shopify theme push` ou `shopify theme pull`. Utiliser uniquement `git push origin main`.

---

## 🧠 Skills obligatoires

> ⚠️ **IMPORTANT** : Ce projet dispose de plusieurs skills spécialisés. **Tu DOIS les consulter** avant de travailler sur des tâches Shopify !

### Skills disponibles

| Skill | Chemin | Usage |
|-------|--------|-------|
| **shopify-horizon-editor** | `.agents/skills/shopify-horizon-editor/SKILL.md` | ⭐ **PRINCIPAL** — Édition experte du thème Horizon (sections, blocks, snippets, settings) |
| **shopify-apps** | `.agents/skills/shopify-apps/SKILL.md` | Développement d'applications Shopify |
| **shopify-development** | `.agents/skills/shopify-development/SKILL.md` | Développement général Shopify |
| **shopify-expert** | `.agents/skills/shopify-expert/SKILL.md` | Expertise et bonnes pratiques |
| **shopify-theme-dev** | `.agents/skills/shopify-theme-dev/SKILL.md` | Développement de thèmes |
| **shopify-theme-development-guidelines** | `.agents/skills/shopify-theme-development-guidelines/SKILL.md` | Guidelines de développement |
| **frontend-design** | `.agents/skills/frontend-design/SKILL.md` | Design et UI/UX frontend |

### Comment utiliser les skills

1. **Avant de coder** : Lis le fichier `SKILL.md` pertinent avec `view_file`
2. **Pendant le développement** : Suis les instructions et patterns définis dans les skills
3. **Si tu hésites** : Consulte plusieurs skills pour avoir une vue complète

### Quand utiliser quel skill ?

- 🌟 **Horizon — tout ce qui touche au thème** → `shopify-horizon-editor` **(à consulter EN PREMIER)**
- 🎨 **Thème / Liquid / Sections (générique)** → `shopify-theme-dev`, `shopify-theme-development-guidelines`
- 🏗️ **Apps / Extensions / API** → `shopify-apps`, `shopify-development`
- 💡 **Bonnes pratiques / Architecture** → `shopify-expert`
- 🖼️ **Design / CSS / UI** → `frontend-design`

---

## 🎯 Source de vérité : GitHub

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│     SHOPIFY     │ ◄─────► │     GITHUB      │ ◄─────► │      LOCAL      │
│  (éditeur web)  │  auto   │ (source vérité) │  git    │   (toi + AI)    │
└─────────────────┘  sync   └─────────────────┘         └─────────────────┘
```

### Comment ça fonctionne

1. **GitHub est la source de vérité** - Tout le code doit passer par GitHub
2. **Shopify ↔ GitHub** - Synchronisation automatique bidirectionnelle
   - Changements dans l'éditeur Shopify → commit auto sur GitHub
   - Push sur GitHub → déploiement auto sur Shopify
3. **Local ↔ GitHub** - Standard Git (`git pull` / `git push`)

---

## 🔄 Workflow obligatoire pour les agents

### AVANT de commencer à travailler

**TOUJOURS** vérifier s'il y a des changements sur GitHub (l'utilisateur peut avoir modifié le thème depuis Shopify) :

```bash
git pull origin main
```

Si des conflits existent, les résoudre avant de continuer.

### APRÈS chaque feature complétée

**OBLIGATOIRE** : Après avoir terminé une fonctionnalité, correction de bug, ou modification significative :

1. Demander à l'utilisateur :
   > "J'ai terminé [description]. Voulez-vous que je commit et push sur GitHub ?"

2. Si l'utilisateur accepte, exécuter :
   ```bash
   git add .
   git commit -m "type: description courte"
   git push origin main
   ```

3. Confirmer le push et rappeler que Shopify se synchronisera automatiquement.

---

## 📝 Format des commits

| Type | Description |
|------|-------------|
| `feat` | Nouvelle fonctionnalité |
| `fix` | Correction de bug |
| `style` | Changements visuels/CSS |
| `refactor` | Refactorisation du code |
| `docs` | Documentation |
| `chore` | Maintenance, config |

**Exemples** :
- `feat: ajouter section hero personnalisée`
- `fix: corriger affichage prix sur mobile`
- `style: améliorer espacement section collection`

**Langue** : Français

---

## 🛠️ Commandes utiles

### Git (workflow principal)
```bash
# Récupérer les derniers changements (FAIRE EN PREMIER)
git pull origin main

# Voir l'état des fichiers
git status

# Committer et pusher
git add .
git commit -m "type: description"
git push origin main
```

### Shopify CLI (développement local uniquement)
```bash
# Lancer le serveur de développement (prévisualisation locale)
shopify theme dev --store valentine-lambert.myshopify.com
```

> ⛔ **INTERDIT** : Ne JAMAIS utiliser `shopify theme push` ou `shopify theme pull`.
> Le thème `valentine-lambert-shopify/main` est synchronisé automatiquement avec GitHub.
> Toute modification doit passer par `git push origin main`.

---

## 📁 Structure du projet

```
ValentineLambert_shopify/
├── assets/          # JS, CSS, images
├── blocks/          # Blocs Horizon
├── config/          # settings_schema.json, settings_data.json
├── layout/          # theme.liquid, password.liquid
├── locales/         # Traductions (fr.json, en.json)
├── sections/        # Sections Shopify
├── snippets/        # Composants réutilisables
├── templates/       # Templates de pages
├── .gitignore       # Fichiers à ignorer
└── AGENTS.md        # Ce fichier
```

> 📌 **Note** : Le thème Horizon utilise un dossier `blocks/` supplémentaire par rapport aux thèmes classiques.

---

## ⚠️ Fichiers sensibles (ne JAMAIS committer)

Ces fichiers sont dans `.gitignore` :
- `.env` et `automation/.env`
- `node_modules/`
- `.DS_Store`
- Tokens d'API ou clés secrètes

---

## 🎓 Spécificités du thème Horizon

### Architecture Horizon

Le thème **Horizon** est un thème moderne Shopify avec :
- **Dossier `blocks/`** : Blocs globaux réutilisables entre plusieurs sections
- **Sections modulaires** : Hautement configurables via l'éditeur de thème
- **Design system** : Variables CSS natives pour la cohérence visuelle
- **Performance** : Chargement optimisé des assets

### Modifier le thème Horizon

- **Ne jamais modifier** les sections/assets Horizon directement si possible → créer de nouvelles sections personnalisées
- **Préférer les overrides** via `settings_data.json` pour les configurations
- **Tester dans l'éditeur** avant de committer

---

## ✅ Checklist pour les agents

- [ ] 📚 **Consulter les skills pertinents** (AVANT de coder !)
- [ ] `git pull` avant de commencer
- [ ] Tester les changements avant de committer
- [ ] Un commit = une fonctionnalité (commits atomiques)
- [ ] Message de commit clair en français
- [ ] Proposer le commit + push à l'utilisateur
- [ ] Confirmer que Shopify se synchronisera automatiquement
