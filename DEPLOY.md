# Déploiement Somatica Pages Renderer via GitHub + Netlify

Guide étape par étape pour mettre en ligne le renderer public avec auto-deploy.

## Prérequis

- Compte GitHub (déjà créé)
- Compte Netlify (déjà créé)
- Terminal avec `git` installé

---

## Étape 1, créer le repo GitHub

1. Va sur [github.com/new](https://github.com/new)
2. Nom du repo : `somatica-pages-renderer`
3. Visibilité : **Public** (c'est juste un afficheur, pas de secret)
4. NE PAS cocher "Initialize with README"
5. Clic "Create repository"
6. Note l'URL HTTPS du repo

---

## Étape 2, push initial depuis ton Mac

```bash
cd ~/Documents/Claude/Projects/apps/somatica-pages-renderer

git init
git add .
git commit -m "Initial commit Somatica Pages Renderer"
git branch -M main

git remote add origin https://github.com/TONPSEUDO/somatica-pages-renderer.git

git push -u origin main
```

---

## Étape 3, connecter à Netlify

1. Va sur [app.netlify.com](https://app.netlify.com)
2. Clic "Add new site" → "Import an existing project" → "GitHub"
3. Sélectionne le repo `somatica-pages-renderer`
4. Branch : `main`
5. Build command : LAISSER VIDE
6. Publish directory : `.` (un point)
7. Clic "Deploy"

---

## Étape 4, renommer le site

1. Site settings → Change site name → `somatica-pages`
2. URL : `https://somatica-pages.netlify.app`

---

## Étape 5, configurer les sous-domaines personnalisés

Pour servir tes pages sur `decouverte.somatica.fr`, `formation.somatica.fr`, etc.

1. Site settings → Domain management → Add custom domain
2. Ajoute `decouverte.somatica.fr`
3. Chez ton registrar de somatica.fr (Wix, Gandi, OVH...), ajoute un CNAME :
   - Nom : `decouverte`
   - Valeur : `somatica-pages.netlify.app`
4. Attendre propagation DNS (1 à 24h)
5. Activer HTTPS sur Netlify (auto via Let's Encrypt)

Répéter pour chaque sous-domaine que tu veux brancher (formation, studio, etc.).

---

## Étape 6, mettre à jour l'admin

Dans `apps/somatica-pages-admin/index.html`, ajuste la constante `RENDERER_URL` si nécessaire :

```js
const RENDERER_URL = 'https://somatica-pages.netlify.app';
```

Si tu utilises ton domaine personnalisé, remplace par exemple par `https://decouverte.somatica.fr`.

Push le changement, Netlify auto-deploy.

---

## Workflow futur

Pour modifier le code du renderer (nouveaux types de blocs, refonte design) :

```bash
cd ~/Documents/Claude/Projects/apps/somatica-pages-renderer
# Modifs
git add .
git commit -m "Description"
git push
```

Auto-deploy en 1 minute.

---

## Sécurité

- Le renderer utilise la **clé publique Supabase** (sb_publishable_...) qui n'a accès qu'aux pages publiées (`statut = 'publiee'`)
- Aucune écriture possible côté renderer
- Les pages en brouillon ne sont pas servies publiquement

---

## URLs des pages

Une fois déployé, tes pages sont accessibles via :

- `https://somatica-pages.netlify.app/?slug=reposer-systeme-nerveux`
- `https://decouverte.somatica.fr` (si DNS configuré)
- `https://formation.somatica.fr` (si DNS configuré et page créée)

Le renderer lit le slug depuis `?slug=xxx` ou depuis le path URL.

Tu peux aussi configurer Netlify pour mapper directement un sous-domaine à un slug spécifique via les redirections.
