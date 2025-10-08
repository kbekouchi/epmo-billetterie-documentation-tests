# 📋 Cahiers de Tests - Migration Billetterie EPMO

## Introduction

Ce dossier contient tous les cahiers de tests détaillés pour la migration du système de billetterie EPMO.

## 📁 Organisation

### Structure

```
03-cahiers-tests/
├── index.md                       # Index complet de tous les tests
├── fonctionnel/
│   ├── billetterie/               # 🎯 MODULE PRIORITAIRE
│   │   ├── README.md
│   │   ├── TEST-BILL-001.md      # Affichage URL
│   │   ├── TEST-BILL-002.md      # Bascule vers SeeTickets
│   │   ├── TEST-BILL-003.md      # Rollback vers VivaTickets
│   │   └── ...
│   ├── authentification/
│   └── multilingue/
├── integration/
│   ├── api/
│   └── modules/
├── regression/
├── performance/
└── securite/
```

## 🎯 Tests prioritaires

### Module Billetterie (15 tests)

**Tests critiques :**
- TEST-BILL-001 : Affichage URL billetterie
- TEST-BILL-002 : Bascule VivaTickets → SeeTickets
- TEST-BILL-003 : Rollback SeeTickets → VivaTickets
- TEST-BILL-004 : Double field URL
- TEST-BILL-005 à 007 : Support multilingue (FR/EN/ES)

### Sécurité (5 tests)

- TEST-SEC-001 : CVE-2025-31675 (Drupal XSS)
- TEST-SEC-002 : CVE-2025-8361 (Config Pages)
- TEST-SEC-003 : Injection SQL
- TEST-SEC-004 : Protection CSRF
- TEST-SEC-005 : Validation entrées

### Performance (3 tests)

- TEST-PERF-001 : Temps réponse < 2s
- TEST-PERF-002 : Charge 100 utilisateurs
- TEST-PERF-003 : Pic 500 users/heure

## 📊 Métriques

| Catégorie | Nombre de tests | Statut |
|-----------|----------------|--------|
| Fonctionnel | 25 | ✅ Complété |
| Intégration | 8 | ✅ Complété |
| Régression | 4 | ✅ Complété |
| Performance | 3 | ✅ Complété |
| Sécurité | 5 | ✅ Complété |
| Accessibilité | 3 | ✅ Complété |
| **TOTAL** | **48** | **✅ Complété** |

## 🔖 Convention de nommage

### Format des identifiants

```
TEST-[MODULE]-[NUMÉRO]

Modules:
- BILL : Billetterie
- AUTH : Authentification
- MULT : Multilingue
- INT : Intégration
- REG : Régression
- PERF : Performance
- SEC : Sécurité
- ACC : Accessibilité
```

### Exemple

`TEST-BILL-001` : Premier test du module billetterie

## 📝 Structure type d'une fiche de test

Chaque fiche de test contient :

1. **En-tête** : Métadonnées (ID, titre, criticité, etc.)
2. **Références** : Liens Redmine, GitHub, GitLab
3. **Objectif** : Description du comportement à vérifier
4. **Prérequis** : État initial requis
5. **Données de test** : Jeux de données
6. **Étapes** : Actions détaillées numérotées
7. **Résultats attendus** : Comportement final
8. **Critères de validation** : Conditions de succès
9. **Gestion anomalies** : Procédure signalement
10. **Historique** : Versions et modifications

## 🏷️ Tags et statuts

### Tags d'identification

- ✅ **[VALIDÉ]** : Test validé et opérationnel
- 🔄 **[AUTOMATISABLE]** : Candidat à l'automatisation
- 📋 **[À VALIDER - MÉTIER]** : Nécessite validation fonctionnelle
- ⚙️ **[À VALIDER - TECHNIQUE]** : Nécessite validation technique
- ⚠️ **[RISQUE IDENTIFIÉ]** : Zone à risque
- 🔒 **[OBSOLÈTE]** : Test désactivé

### Niveaux de criticité

- 🔴 **CRITIQUE** : Parcours principal, bloquant
- 🟠 **ÉLEVÉE** : Impact métier significatif
- 🟡 **STANDARD** : Fonctionnalité secondaire

## 🔗 Liens utiles

- **[Index complet des tests](./index.md)**
- **[Plan de tests général](../02-plan-tests/plan-tests-general.md)**
- **[Matrice de couverture](../02-plan-tests/matrices/matrice-couverture.md)**
- **[Données de test](../04-donnees-test/)**

---

*Dernière mise à jour : 08/10/2025*