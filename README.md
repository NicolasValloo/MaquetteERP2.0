# GAA — Prototype fonctionnel

Prototype interactif d'un logiciel de gestion pour agences d'architecture.  
21 modules HTML autonomes, données persistées en `localStorage`, zéro dépendance serveur.

## Démo

Ouvrir `index.html` dans un navigateur, ou déployer sur **GitHub Pages** (voir ci-dessous).

## Structure

```
/
├── index.html                              # Navigation principale
├── parcours-creation-projet.html           # Wizard création projet
│
├── module-1-1-portefeuille-projets.html    # 1.1 Projets
├── module-1-2-phases-mop.html             # 1.2 Phases MOP (Gantt)
├── module-1-3-gestion-temps.html          # 1.3 Saisie de temps
├── module-1-4-ged-projet.html             # 1.4 GED & Documents
├── module-1-5-finance-facturation.html    # 1.5 Finance & Facturation
├── module-1-6-crm.html                    # 1.6 CRM Commercial
├── module-1-7-planning-charge.html        # 1.7 Planning de charge
├── module-1-8-chantier.html               # 1.8 Suivi de chantier
├── module-1-9-taches.html                 # 1.9 Tâches & Livrables
├── module-1-10-assurances.html            # 1.10 Assurances
│
├── module-2-1-auth.html                   # 2.1 Auth & Onboarding
├── module-2-2-dashboard.html              # 2.2 Dashboard
├── module-2-3-roles.html                  # 2.3 Rôles & Permissions
├── module-2-4-2-6.html                    # 2.4–2.6 Multi-agences · Notifications
├── module-2-5-admin-fonctionnel.html      # 2.5 Administration agence
│
├── module-3-x-integrations.html           # 3.x Intégrations & Connecteurs
│
├── module-4-1-reporting.html              # 4.1 Reporting & BI
├── module-4-2-4-3-alertes-workflows.html  # 4.2–4.3 Alertes & Workflows
└── module-4-4-4-5-4-6.html               # 4.4–4.6 Connaissances · Portail · Messagerie
```

## Déploiement GitHub Pages

```bash
# 1. Créer le repository
git init
git add .
git commit -m "feat: prototype fonctionnel GAA — 21 modules"
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main

# 2. Activer GitHub Pages
# Settings → Pages → Source : Deploy from branch → main → / (root) → Save
```

L'URL sera `https://<user>.github.io/<repo>/`.

## Architecture technique du prototype

### Store partagé

Tous les modules partagent un store commun via `localStorage` (clés préfixées `gaa_*`).  
Les données créées dans un module sont immédiatement disponibles dans les autres.

```
gaa_projets           → liste des projets
gaa_utilisateurs      → équipe agence
gaa_branding          → couleurs, logo agence
gaa_taux              → taux horaires par rôle
gaa_phases            → configuration phases MOP
gaa_contacts          → clients, BETs, partenaires
gaa_audit_log         → journal de toutes les actions
gaa_factures          → factures émises
gaa_temps_saisies     → imputations de temps
gaa_chantier_cr       → comptes-rendus de chantier
gaa_chantier_obs      → observations de chantier
gaa_taches            → tâches par projet/phase
gaa_crm_opportunites  → pipeline commercial
gaa_ged_documents     → documents déposés
gaa_assurances_*      → polices et sinistres
gaa_integrations_*    → état des connecteurs
gaa_workflows_config  → configuration des automatisations
gaa_connaissances     → base de connaissances agence
```

### Réinitialisation des données

```js
// Console navigateur
localStorage.clear()
location.reload()
```

### Données de démonstration

Le store est pré-chargé avec des données fictives au premier chargement :
- Agence : Atelier Lefort & Associés
- 4 utilisateurs actifs
- 3 projets en cours (Résidence Les Oliviers, Groupe scolaire V. Hugo, Résidence Les Cèdres)
- Contacts, taux horaires, phases MOP configurés

## Stack cible

Ce prototype est une maquette fonctionnelle. La production sera développée avec :

| Couche | Technologie |
|---|---|
| Backend | Django 5 + Django REST Framework |
| Templating | Django Templates + django-components |
| Dynamique | htmx (swaps partiels, SSE) |
| Frontend interactif | TypeScript (Gantt, drag-and-drop) |
| CSS | Sass + BEM |
| Base de données | PostgreSQL 16 |
| Cache / Queue | Redis + Celery |
| Déploiement | Docker + Nginx |

## Limites du prototype

- **Pas de multi-utilisateur** : le `localStorage` est local au navigateur
- **Pas de persistance cross-device** : les données ne se synchronisent pas entre appareils
- **Pas d'authentification réelle** : le wizard d'onboarding est simulé
- **Pas d'upload de fichiers** : les dépôts GED sont simulés sans stockage réel
- **Emails** : les diffusions sont simulées (toast de confirmation)

Ces limites sont inhérentes au format prototype statique et seront résolues dans l'implémentation Django.
