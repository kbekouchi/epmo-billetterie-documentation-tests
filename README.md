# 📚 Documentation de Tests - Migration Billetterie EPMO

**Projet** : Migration billetterie EPMO (VivaTickets → SeeTickets)  
**Version** : 1.0.0  
**Date** : 08/10/2025  
**Statut** : 🚧 EN COURS DE RÉDACTION

---

## 🎯 Vue d'ensemble

Ce repository contient l'ensemble de la documentation de tests pour le projet de migration du système de billetterie de l'Établissement Public des Musées d'Orsay et de l'Orangerie (EPMO).

### Objectif

Assurer une migration sécurisée et sans interruption de service depuis **VivaTickets** vers **SeeTickets** sur les 4 sites institutionnels :
- Musée d'Orsay (musee-orsay.fr)
- Musée de l'Orangerie (musee-orangerie.fr)  
- Site institutionnel EPMO (epmo-musees.fr)
- Les Petits MO (petitsmo.fr)

---

## 📋 Structure du repository

```
test-documentation/
├── README.md                          # Ce fichier
├── CHANGELOG.md                       # Historique des versions
├── .gitignore                         # Fichiers à ignorer
│
├── 01-strategie-recette/
│   ├── README.md                      # Introduction stratégie
│   ├── strategie-recette.md           # Document principal
│   ├── annexes/
│   │   ├── environnements.md
│   │   ├── processus-validation.md
│   │   └── outils-frameworks.md
│   └── schemas/                       # Diagrammes
│
├── 02-plan-tests/
│   ├── README.md                      # Introduction plan
│   ├── plan-tests-general.md          # Vue d'ensemble
│   ├── matrices/
│   │   ├── matrice-couverture.md      # Traçabilité
│   │   ├── matrice-risques.md
│   │   └── matrice-priorites.md
│   └── scenarios/
│       ├── scenarios-critiques.md
│       ├── scenarios-integration.md
│       └── scenarios-regression.md
│
├── 03-cahiers-tests/
│   ├── README.md                      # Introduction cahiers
│   ├── index.md                       # Index tous tests
│   ├── fonctionnel/
│   │   ├── billetterie/               # 🎯 MODULE PRIORITAIRE
│   │   ├── authentification/
│   │   └── multilingue/
│   ├── integration/
│   ├── regression/
│   ├── performance/
│   └── securite/
│
├── 04-donnees-test/
│   ├── README.md
│   ├── jeux-donnees/
│   └── scripts/
│
├── 05-resultats-tests/
│   ├── README.md
│   ├── rapports/
│   └── templates/
│
├── 06-automatisation/
│   ├── README.md
│   ├── references-gitlab.md
│   └── scenarios-automatisables.md
│
└── 99-annexes/
    ├── glossaire.md
    ├── references-redmine.md
    ├── references-gitlab.md
    ├── normes-standards.md
    └── contacts-equipe.md
```

---

## 🚀 Démarrage rapide

### Pour les directeurs de projet

1. **Commencez par** : [📊 Synthèse exécutive](./01-strategie-recette/strategie-recette.md)
2. **Puis consultez** : [🏗️ Plan de tests général](./02-plan-tests/plan-tests-general.md)
3. **Suivez** : [📈 Matrice de couverture](./02-plan-tests/matrices/matrice-couverture.md)

### Pour les testeurs QA

1. **Référez-vous à** : [📋 Index des tests](./03-cahiers-tests/index.md)
2. **Consultez** : [🎫 Tests billetterie](./03-cahiers-tests/fonctionnel/billetterie/)
3. **Utilisez** : [📊 Données de test](./04-donnees-test/)

### Pour les développeurs

1. **Analysez** : [🔄 Scénarios automatisables](./06-automatisation/scenarios-automatisables.md)
2. **Référencez** : [🔗 Code GitLab](./06-automatisation/references-gitlab.md)
3. **Intégrez** : Tests dans pipeline CI/CD

---

## 📊 Métriques de complétude

| Section | Complétude | Statut |
|---------|------------|--------|
| Stratégie de recette | 100% | ✅ Complétée |
| Plan de tests | 100% | ✅ Complétée |
| Cahiers de tests billetterie | 100% | ✅ Complétée |
| Cahiers de tests autres modules | 40% | 🟡 En cours |
| Données de test | 80% | 🟡 En cours |
| Automatisation | 60% | 🟡 En cours |

**Complétude globale** : 75% 📈

---

## 🔄 Workflow Git

### Branches

- `main` : Version stable et validée
- `develop` : Intégration continue
- `feature/[module]` : Nouveaux tests
- `update/[sprint-XX]` : Mises à jour

### Convention commits

```
[TYPE] Scope: Description courte

Types:
- [STRAT] : Stratégie de recette
- [PLAN] : Plan de tests
- [TEST] : Cahier de test
- [DATA] : Données de test
- [DOC] : Documentation
- [FIX] : Correction
```

### Tags de version

```
v1.0.0 : Version initiale complète
v1.1.0 : Ajout tests performance
v2.0.0 : Migration production validée
```

---

## 📖 Traçabilité des sources

### Sources Redmine

| Ticket | Sujet | Référence |
|--------|-------|-----------|
| [#91157](https://redmine.softeam.agency/issues/91157) | Script mise à jour URLs billetterie | Architecture migration |
| [#92609](https://redmine.softeam.agency/issues/92609) | Documentations techniques | Contexte projet |
| [#92600](https://redmine.softeam.agency/issues/92600) | Audit sécurité | Exigences sécurité |

### Sources GitHub

- **Spécifications** : [kbekouchi/epmo-billetterie-specifications-techniques](https://github.com/kbekouchi/epmo-billetterie-specifications-techniques)
- **Documentation** : Ce repository

### Sources GitLab Softeam

- **Code principal** : softeam-agency/epmo/ep
- **Module billetterie** : softeam-agency/epmo/moo (accès requis)
- **Backend API** : softeam-agency/epmo/moo-backend (accès requis)

---

## 👥 Équipe projet

### Maîtrise d'ouvrage
- **EPMO** : Benjamin, Martin (SI-DENUM)

### Maîtrise d'œuvre
- **Softeam Agency** : David Tripont (Tech Lead), Kim Lev (Dev Lead)

### Qualité et tests
- **Responsable QA** : À définir
- **Testeurs** : Équipe QA Softeam

---

## 🔑 Points clés à retenir

### ✅ Forces

- Architecture robuste avec rollback
- Approche progressive de migration
- Support multilingue (FR/EN/ES)
- Documentation structurée selon standards gouvernementaux

### ⚠️ Risques

- Accès techniques bloqués (GitLab)
- Documentation API SeeTickets incomplète
- Scripts migration non développés
- Audits sécurité partiels

### 🎯 Priorités

1. Tests critiques du parcours billetterie
2. Tests de bascule VivaTickets/SeeTickets
3. Tests multilingues (FR/EN/ES)
4. Tests de rollback
5. Tests de performance

---

## 📞 Support et contacts

- **Issues GitHub** : [Créer une issue](https://github.com/kbekouchi/epmo-billetterie-documentation-tests/issues)
- **Redmine Softeam** : [redmine.softeam.agency](https://redmine.softeam.agency)
- **Documentation projet** : [Spécifications techniques](https://github.com/kbekouchi/epmo-billetterie-specifications-techniques)

---

## 📄 Licence et confidentialité

**Classification** : Document confidentiel - Usage interne EPMO  
**Diffusion** : Restreinte aux équipes projet  
**Propriété** : EPMO & Softeam Agency

---

## 📅 Historique

| Version | Date | Auteur | Modifications |
|---------|------|--------|---------------|
| 1.0.0 | 08/10/2025 | Équipe QA | Initialisation repository |

---

**Dernière mise à jour** : 08 octobre 2025  
**Maintenu par** : Équipe QA Projet EPMO Billetterie
