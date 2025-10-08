# 🔗 Références GitLab - Projet EPMO Billetterie

**Version:** 1.0.0  
**Date:** 08/10/2025

---

## 📦 Repositories GitLab Softeam Agency

### Repository principal (Accessible ✅)

**softeam-agency/epmo/ep**
- **URL:** https://gitlab.softeam.agency/softeam-agency/epmo/ep
- **Description:** Repository principal sites EPMO
- **Branche défaut:** integration
- **Dernière activité:** 21/08/2025
- **Accès:** ✅ Disponible

**Contenu principal:**
- `composer.json` : Stack technique Drupal 9.4+
- `README.md` : Documentation environnements
- `.gitlab-ci.yml` : Pipeline CI/CD
- `/web/modules/custom/` : Modules custom EPMO

---

### Repositories modules billetterie (Accès requis ⚠️)

**softeam-agency/epmo/moo**
- **Description:** Module billetterie custom
- **Contenu attendu:** 
  - `epmo_billetterie/` : Module principal
  - Configuration flag de bascule
  - Gestion double field URL
- **Accès:** ❌ **BLOQUÉ - Déblocage urgent requis**
- **Ticket:** À créer - Demande accès admin système

**softeam-agency/epmo/moo-backend**
- **Description:** Backend API billetterie
- **Contenu attendu:**
  - API endpoints SeeTickets
  - Gestion cache
  - Logs et monitoring
- **Accès:** ❌ **BLOQUÉ - Déblocage urgent requis**

---

## 🏗️ Stack technique (depuis composer.json)

### Core

```json
{
  "require": {
    "drupal/core": "^9.4",
    "php": ">=8.1"
  }
}
```

### Modules contrib principaux

- `drupal/admin_toolbar` : Interface admin améliorée
- `drupal/tmgmt` : Gestion traductions (FR/EN/ES)
- `drupal/search_api_solr` : Recherche Apache Solr
- `drupal/config_split` : Configuration par environnement
- `drupal/redis` : Cache Redis
- `drupal/facets` : Facettes recherche

### Modules custom EPMO

- `epmo_billetterie` : **Module prioritaire - Billetterie**
- `epmo_analytics` : Piano, Commander Act
- `epmo_ephoto` : Intégration DAM images

---

## 🔧 Pipeline CI/CD

### Fichier `.gitlab-ci.yml`

**Stages:**
1. **validate** : Validation code (PHP CodeSniffer, ESLint)
2. **test** : Tests unitaires (PHPUnit)
3. **build** : Build assets (npm, composer)
4. **deploy** : Déploiement automatisé

### Environnements

| Environnement | Branche | Déploiement |
|---------------|---------|-------------|
| **Integration** | `integration` | Automatique |
| **Préproduction** | `preprod` | Manuel (MR) |
| **Production** | `main` | Manuel (tag) |

---

## 📂 Structure repository ep

```
softeam-agency/epmo/ep/
├── .gitlab-ci.yml                 # Pipeline CI/CD
├── composer.json                  # Dépendances PHP/Drupal
├── README.md                      # Documentation
├── web/
│   ├── modules/
│   │   ├── contrib/              # Modules communautaires
│   │   └── custom/               # Modules custom EPMO
│   │       ├── epmo_billetterie/ # 🎯 MODULE BILLETTERIE
│   │       ├── epmo_analytics/
│   │       └── epmo_ephoto/
│   ├── themes/
│   │   └── custom/               # Thèmes EPMO
│   └── sites/
│       └── default/
│           └── settings.php
└── config/                        # Configuration Drupal
    ├── sync/                      # Config partagée
    └── splits/                    # Config par environnement
```

---

## 🔍 Commandes GitLab utiles

### Cloner le repository

```bash
git clone git@gitlab.softeam.agency:softeam-agency/epmo/ep.git
cd ep
```

### Checkout branche integration

```bash
git checkout integration
git pull origin integration
```

### Voir les derniers commits

```bash
git log --oneline -10
```

### Rechercher dans le code

```bash
# Rechercher "billetterie"
git grep "billetterie"

# Rechercher dans les commits
git log --all --grep="billetterie"
```

---

## 🔗 Merge Requests liées billetterie

**À identifier après déblocage accès**

Format recherche MR :
- Label: `billetterie`
- Auteur: Kim Lev, David Tripont
- Période: 2024-2025

---

## ⚠️ Actions requises

### Déblocage accès urgent

1. **Créer ticket demande d'accès**
   - Repository: softeam-agency/epmo/moo
   - Repository: softeam-agency/epmo/moo-backend
   - Utilisateur: [À définir]
   - Rôle: Developer (Read + Write)

2. **Contact:**
   - Administrateur GitLab Softeam Agency
   - Escalade: Chef de projet
   - Délai: **24h maximum**

### Impact blocage

- ❌ Impossible d'analyser code billetterie
- ❌ Tests automatisés incomplets
- ❌ Traçabilité code ↔ tests partielle
- ❌ Revue de code impossible

---

## 📊 Commits récents ep (aperçu)

**Derniers commits sur integration:**
- Dernière activité: 21/08/2025
- Commits à analyser après déblocage

---

**Dernière synchronisation:** 08/10/2025  
**Statut accès:** ⚠️ Partiel (1/3 repositories accessibles)  
**Action prioritaire:** Déblocage accès moo + moo-backend
