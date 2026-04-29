# Somatica Pages Renderer

Site qui rend les pages de vente Somatica depuis Supabase.

## URL cible

- Production : `decouverte.somatica.fr`, `formation.somatica.fr`, `studio.somatica.fr`
- Sous-domaine Netlify : `somatica-pages.netlify.app`

## Fonctionnement

1. Lit le slug depuis `?slug=xxx` ou le chemin URL
2. Charge la page depuis Supabase (table `sp_pages`)
3. Charge les blocs visibles (table `sp_blocks`)
4. Rend chaque bloc selon son `type_bloc`

## Stack

- HTML/CSS pur, pas de framework
- Supabase JS via CDN (esm.sh)
- Mobile-first
- Design Somatica (palette dorée + vert sombre)

## Types de blocs supportés

- `hero` : Bannière avec titre + CTA
- `beneficies` : Grille de 3 bénéfices numérotés
- `pour_qui` : Liste à cocher (✓)
- `temoignage` : Citation encadrée
- `a_propos` : Bio multi-paragraphes + signature
- `cta_optin` : Formulaire opt-in (à connecter Brevo)
- `garde_fous` : Mention contre-indications
- `footer` : Pied de page avec liens
- `texte` : Bloc texte simple

## Premier déploiement

Drag & drop sur app.netlify.com/drop, nom de site `somatica-pages`.

## Sécurité

- Lit uniquement les pages avec `statut = 'publiee'` (RLS Supabase)
- Aucune écriture possible (clé publique)
- Le formulaire opt-in est à connecter à une Edge Function (à venir)

## TODO

- [ ] Edge Function `optin-leadmagnet` qui pousse vers Brevo
- [ ] Connecter le formulaire à l'Edge Function
- [ ] Tester sur tous les sous-domaines
- [ ] Configurer le DNS multi sous-domaines
