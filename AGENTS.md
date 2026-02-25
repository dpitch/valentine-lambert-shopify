# Instructions pour les Agents IA

Ce fichier contient les instructions pour tous les agents IA (Claude, Gemini, GPT, etc.) travaillant sur ce projet.

---

## A propos du projet

| Element | Valeur |
|---------|--------|
| **Projet** | Theme Shopify Stretch pour Valentine Lambert |
| **Boutique** | valentine-lambert.myshopify.com |
| **Theme actif** | `valentine-lambert-shopify/main` (synchronise avec GitHub) |
| **Theme de base** | Stretch par Maestrooo (v1.13.0) |
| **Depot GitHub** | https://github.com/dpitch/valentine-lambert-shopify |
| **Doc theme** | https://support.maestrooo.com/collection/678-getting-started |

> **IMPORTANT** : Le theme est **synchronise automatiquement avec GitHub**. Ne JAMAIS utiliser `shopify theme push` ou `shopify theme pull`. Utiliser uniquement `git push origin main`.

---

## Skills obligatoires

> **IMPORTANT** : Ce projet dispose de plusieurs skills specialises. **Tu DOIS les consulter** avant de travailler sur des taches Shopify !

### Skills disponibles

| Skill | Chemin | Usage |
|-------|--------|-------|
| **shopify-stretch-editor** | `.agents/skills/shopify-stretch-editor/SKILL.md` | **PRINCIPAL** — Edition experte du theme Stretch (sections, blocks, snippets, settings, dynamic grid, heading effects) |
| **shopify-apps** | `.agents/skills/shopify-apps/SKILL.md` | Developpement d'applications Shopify |
| **shopify-development** | `.agents/skills/shopify-development/SKILL.md` | Developpement general Shopify |
| **shopify-expert** | `.agents/skills/shopify-expert/SKILL.md` | Expertise et bonnes pratiques |
| **shopify-theme-dev** | `.agents/skills/shopify-theme-dev/SKILL.md` | Developpement de themes |
| **shopify-theme-development-guidelines** | `.agents/skills/shopify-theme-development-guidelines/SKILL.md` | Guidelines de developpement |
| **frontend-design** | `.agents/skills/frontend-design/SKILL.md` | Design et UI/UX frontend |

### Comment utiliser les skills

1. **Avant de coder** : Lis le fichier `SKILL.md` pertinent avec `view_file`
2. **Pendant le developpement** : Suis les instructions et patterns definis dans les skills
3. **Si tu hesites** : Consulte plusieurs skills pour avoir une vue complete

### Quand utiliser quel skill ?

- **Stretch — tout ce qui touche au theme** -> `shopify-stretch-editor` **(a consulter EN PREMIER)**
- **Theme / Liquid / Sections (generique)** -> `shopify-theme-dev`, `shopify-theme-development-guidelines`
- **Apps / Extensions / API** -> `shopify-apps`, `shopify-development`
- **Bonnes pratiques / Architecture** -> `shopify-expert`
- **Design / CSS / UI** -> `frontend-design`

---

## Source de verite : GitHub

```
+------------------+         +------------------+         +------------------+
|     SHOPIFY      | <-----> |     GITHUB       | <-----> |      LOCAL       |
|  (editeur web)   |  auto   | (source verite)  |  git    |   (toi + AI)     |
+------------------+  sync   +------------------+         +------------------+
```

### Comment ca fonctionne

1. **GitHub est la source de verite** - Tout le code doit passer par GitHub
2. **Shopify <-> GitHub** - Synchronisation automatique bidirectionnelle
   - Changements dans l'editeur Shopify -> commit auto sur GitHub
   - Push sur GitHub -> deploiement auto sur Shopify
3. **Local <-> GitHub** - Standard Git (`git pull` / `git push`)

---

## Workflow obligatoire pour les agents

### AVANT de commencer a travailler

**TOUJOURS** verifier s'il y a des changements sur GitHub (l'utilisateur peut avoir modifie le theme depuis Shopify) :

```bash
git pull origin main
```

Si des conflits existent, les resoudre avant de continuer.

### APRES chaque feature completee

**OBLIGATOIRE** : Apres avoir termine une fonctionnalite, correction de bug, ou modification significative :

1. Demander a l'utilisateur :
   > "J'ai termine [description]. Voulez-vous que je commit et push sur GitHub ?"

2. Si l'utilisateur accepte, executer :
   ```bash
   git add .
   git commit -m "type: description courte"
   git push origin main
   ```

3. Confirmer le push et rappeler que Shopify se synchronisera automatiquement.

---

## Format des commits

| Type | Description |
|------|-------------|
| `feat` | Nouvelle fonctionnalite |
| `fix` | Correction de bug |
| `style` | Changements visuels/CSS |
| `refactor` | Refactorisation du code |
| `docs` | Documentation |
| `chore` | Maintenance, config |

**Exemples** :
- `feat: ajouter section hero personnalisee`
- `fix: corriger affichage prix sur mobile`
- `style: ameliorer espacement section collection`

**Langue** : Francais

---

## Commandes utiles

### Git (workflow principal)
```bash
# Recuperer les derniers changements (FAIRE EN PREMIER)
git pull origin main

# Voir l'etat des fichiers
git status

# Committer et pusher
git add .
git commit -m "type: description"
git push origin main
```

### Shopify CLI (developpement local uniquement)
```bash
# Lancer le serveur de developpement (previsualisation locale)
shopify theme dev --store valentine-lambert.myshopify.com
```

> **INTERDIT** : Ne JAMAIS utiliser `shopify theme push` ou `shopify theme pull`.
> Le theme `valentine-lambert-shopify/main` est synchronise automatiquement avec GitHub.
> Toute modification doit passer par `git push origin main`.

---

## Structure du projet

```
ValentineLambert_shopify/
├── assets/          # theme.css, theme.js, vendor.min.js, custom.css/js, images
├── config/          # settings_schema.json, settings_data.json
├── layout/          # theme.liquid, password.liquid
├── locales/         # Traductions (fr, en, it, de, es)
├── sections/        # Sections Liquid + section groups JSON
├── snippets/        # css-variables.liquid, image.liquid, meta-tags.liquid
├── templates/       # JSON page templates (index, product, collection, etc.)
├── .gitignore       # Fichiers a ignorer
└── AGENTS.md        # Ce fichier
```

> **Note** : Le theme Stretch n'utilise PAS de dossier `blocks/` separé (contrairement à Horizon). Les blocks sont definis dans les schemas des sections.

---

## Fichiers sensibles (ne JAMAIS committer)

Ces fichiers sont dans `.gitignore` :
- `.env` et `automation/.env`
- `node_modules/`
- `.DS_Store`
- Tokens d'API ou cles secretes

---

## Specificites du theme Stretch

### Architecture Stretch

Le theme **Stretch** (par Maestrooo) est un theme premium Shopify avec :
- **Dynamic Grid** : Systeme de grille 16 colonnes (desktop) / 8 colonnes (mobile) exclusif a Stretch
- **30+ sections** : Sections hautement configurables via l'editeur
- **Heading Effects** : 9 effets visuels pour mettre en valeur des mots dans les titres
- **Product Highlighting** : Mise en avant de produits dans les collections via metafield
- **CSS/JS consolides** : Un seul `theme.css` + `theme.js` (ne JAMAIS les modifier)
- **Web components** : Architecture basee sur des custom elements natifs
- **Motion v12** : Animations via la librairie Motion

### Modifier le theme Stretch

- **Ne JAMAIS modifier** `theme.css`, `theme.js`, ou `vendor.min.js` — utiliser `custom.css` / `custom.js`
- **CSS mineur** : Utiliser la boite "Custom CSS" dans l'editeur de theme
- **CSS majeur** : Creer `assets/custom.css` et le lier dans `layout/theme.liquid`
- **JS custom** : Creer `assets/custom.js` et le lier dans `layout/theme.liquid`
- **Etendre les composants** : Importer et sous-classer (`import {Drawer} from "theme"`)
- **Tester sur desktop ET mobile** (la Dynamic Grid se comporte differemment)

---

## Checklist pour les agents

- [ ] **Consulter les skills pertinents** (AVANT de coder !)
- [ ] `git pull` avant de commencer
- [ ] Tester les changements avant de committer
- [ ] Un commit = une fonctionnalite (commits atomiques)
- [ ] Message de commit clair en francais
- [ ] Proposer le commit + push a l'utilisateur
- [ ] Confirmer que Shopify se synchronisera automatiquement
