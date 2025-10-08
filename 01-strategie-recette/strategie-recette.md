---
title: Stratégie de Recette - Migration Billetterie EPMO
version: 1.0.0
date: 2025-10-08
auteur: Équipe QA Softeam
statut: VALIDATED
references:
  - redmine: ["#91157", "#92609", "#92600"]
  - gitlab: ["softeam-agency/epmo/ep", "softeam-agency/epmo/moo"]
  - github: ["kbekouchi/epmo-billetterie-specifications-techniques"]
---

# Stratégie de Recette - Migration Billetterie EPMO

**Version:** 1.0.0  
**Date:** 08/10/2025  
**Statut:** ✅ VALIDÉE

## Historique des versions

| Version | Date | Auteur | Modifications |
|---------|------|--------|---------------|
| 1.0.0 | 08/10/2025 | Équipe QA | Version initiale complète |

---

## 📋 Table des matières

1. [Contexte et objectifs](#1-contexte-et-objectifs)
2. [Périmètre et objectifs de test](#2-périmètre-et-objectifs-de-test)
3. [Typologie des tests](#3-typologie-des-tests)
4. [Environnements de test](#4-environnements-de-test)
5. [Organisation et responsabilités](#5-organisation-et-responsabilités)
6. [Calendrier et jalons](#6-calendrier-et-jalons)
7. [Critères de validation](#7-critères-de-validation)
8. [Gestion des risques](#8-gestion-des-risques)

---

## 1. Contexte et objectifs

### 1.1 Contexte du projet

L'Établissement Public des Musées d'Orsay et de l'Orangerie (EPMO) migre son système de billetterie depuis **VivaTickets** vers **SeeTickets** pour moderniser l'infrastructure de billetterie en ligne tout en assurant la continuité de service.

**Sites concernés :**
- Musée d'Orsay (musee-orsay.fr)
- Musée de l'Orangerie (musee-orangerie.fr)
- Site institutionnel EPMO (epmo-musees.fr)
- Les Petits MO (petitsmo.fr)

**Références sources :**
- [Redmine #91157](https://redmine.softeam.agency/issues/91157) : Script mise à jour URLs billetterie
- [Redmine #92609](https://redmine.softeam.agency/issues/92609) : Documentations techniques & fonctionnelles
- [GitHub Specs](https://github.com/kbekouchi/epmo-billetterie-specifications-techniques)
- [GitLab: softeam-agency/epmo/ep](https://gitlab.softeam.agency/softeam-agency/epmo/ep)

### 1.2 Objectifs stratégiques

1. **Assurer la continuité de service** pendant la phase de transition
2. **Moderniser l'infrastructure** de billetterie en ligne
3. **Améliorer l'expérience utilisateur** avec les nouveaux endpoints API
4. **Garantir la compatibilité multilingue** (FR, EN, ES)
5. **Sécuriser le mécanisme de rollback** en cas de problème

### 1.3 Enjeux qualité

- **Enjeu métier** : Préservation du chiffre d'affaires (billetterie = source de revenus critique)
- **Enjeu technique** : Migration sans interruption de service
- **Enjeu réglementaire** : Conformité RGPD et accessibilité
- **Enjeu utilisateur** : Expérience fluide sur tous les parcours

---

## 2. Périmètre et objectifs de test

### 2.1 Périmètre inclus

#### Fonctionnalités testées

**Module billetterie (epmo_billetterie) :**
- Intégration API SeeTickets
- Affichage des URLs billetterie
- Mécanisme de bascule VivaTickets/SeeTickets (flag)
- Système de double field URL
- Support multilingue (FR, EN, ES)
- Mécanisme de rollback

**Parcours utilisateur :**
- Accès à la billetterie depuis les 4 sites
- Redirection vers la plateforme de billetterie
- Passage de paramètres (langue, événement)
- Tracking et analytics

**Environnements :**
- Intégration
- Préproduction
- Production

#### Référence code source

- [GitLab: softeam-agency/epmo/ep](https://gitlab.softeam.agency/softeam-agency/epmo/ep) - Repository principal
- Module custom : `epmo_billetterie` (softeam-agency/epmo/moo - accès requis)
- Backend API : softeam-agency/epmo/moo-backend (accès requis)

### 2.2 Périmètre exclu

**Hors périmètre de cette recette :**
- Plateforme SeeTickets elle-même (responsabilité éditeur)
- Processus de paiement interne SeeTickets
- Module boutique RMN (projet séparé - [Redmine #92604](https://redmine.softeam.agency/issues/92604))
- Migration historique des données de commandes

### 2.3 Objectifs qualité

| Critère | Objectif | Mesure |
|---------|----------|--------|
| **Couverture fonctionnelle** | 100% des fonctionnalités critiques | Tests exécutés / Fonctionnalités |
| **Taux de succès tests** | ≥ 95% | Tests OK / Tests exécutés |
| **Régression** | 0 régression bloquante | Bugs critiques détectés |
| **Performance** | Temps réponse < 2s | Mesure temps chargement |
| **Disponibilité** | 99.9% uptime | Monitoring production |
| **Accessibilité** | RGAA 100% conforme | Audit accessibilité |

### 2.4 Critères de succès

✅ **Migration réussie si :**
1. Tous les tests critiques sont au vert
2. Aucune régression bloquante détectée
3. Mécanisme de rollback validé fonctionnel
4. Performance équivalente ou supérieure à l'existant
5. Validation métier EPMO obtenue

---

## 3. Typologie des tests

### 3.1 Tests fonctionnels

**Objectif :** Valider que les fonctionnalités répondent aux exigences métier

**Couverture :**
- Parcours d'accès à la billetterie (user stories Redmine #91157)
- Affichage correct des URLs
- Bascule entre VivaTickets et SeeTickets
- Support multilingue (FR/EN/ES)
- Gestion des paramètres d'URL

**Méthode :** Tests manuels + automatisation Selenium

**Référence cahiers de tests :**
- [Tests billetterie](../03-cahiers-tests/fonctionnel/billetterie/)

### 3.2 Tests d'intégration

**Objectif :** Valider les interactions entre composants

**Couverture :**
- Module Drupal ↔ API SeeTickets
- Module billetterie ↔ Base de données
- Intégration avec module de traduction (TMGMT)
- Intégration analytics (Piano, Commander Act)

**Méthode :** Tests API + tests de bout en bout

**Architecture testée :**
```
Utilisateurs (FR/EN/ES)
         ↓
    Sites EPMO (x4)
         ↓
   Drupal 9 Platform
         ↓
 Module epmo_billetterie
    ↓              ↓
VivaTickets    SeeTickets
 (Backup)       (Target)
```

### 3.3 Tests de régression

**Objectif :** S'assurer qu'aucune fonctionnalité existante n'est dégradée

**Couverture :**
- Tous les parcours billetterie existants
- Fonctionnalités non modifiées (navigation, recherche, etc.)
- Bugs historiques corrigés (historique Redmine)

**Bugs de référence Redmine :**
- Consulter l'historique des anomalies billetterie
- Vérifier non-régression suite aux correctifs

### 3.4 Tests de performance

**Objectif :** Valider que les performances sont acceptables

**Contraintes techniques (depuis specs Git) :**
- Temps de réponse API < 2 secondes
- Charge simultanée : 100 utilisateurs concurrent
- Pic de charge : 500 utilisateurs/heure

**Scénarios :**
- Montée en charge progressive
- Test de pic (vente flash exposition)
- Test d'endurance (24h)

**Outils :** JMeter, GTmetrix

### 3.5 Tests de sécurité

**Objectif :** Identifier et corriger les vulnérabilités

**Références :**
- [Redmine #92600](https://redmine.softeam.agency/issues/92600) : Audit sécurité
- [Redmine #92009](https://redmine.softeam.agency/issues/92009) : Audit sécurité complémentaire

**Vulnérabilités à valider (CVE identifiées) :**
- CVE-2025-31675 : Drupal XSS
- CVE-2025-8361 : Config Pages Access bypass
- Injection SQL (CWE-89)
- Attaques CSRF (CWE-352)
- Traversée de chemin (CWE-22)

**Actions :**
1. Mise à jour Drupal core et modules contrib
2. Validation des correctifs de sécurité
3. Tests de pénétration ciblés
4. Scan automatisé (OWASP ZAP)

### 3.6 Tests d'accessibilité

**Objectif :** Conformité RGAA niveau AA minimum

**Couverture :**
- Navigation clavier
- Lecteurs d'écran (NVDA, JAWS)
- Contrastes et lisibilité
- Alternatives textuelles

**Référentiel :** RGAA 4.1

### 3.7 Tests multilingues

**Objectif :** Valider le fonctionnement dans les 3 langues supportées

**Couverture :**
- Interface FR / EN / ES
- Redirection billetterie avec paramètre langue
- Cohérence des traductions
- Module TMGMT (Translation Management Tool)

**Méthode :** Tests manuels par langue

---

## 4. Environnements de test

### 4.1 Architecture des environnements

#### Configuration (depuis GitLab CI/CD)

| Environnement | URL | Usage | Données |
|---------------|-----|-------|----------|
| **Intégration** | inte-*.musee-orsay.fr | Tests développement | Données de test |
| **Préproduction** | preprod-*.musee-orsay.fr | Validation fonctionnelle | Copie production |
| **Production** | *.musee-orsay.fr | Service réel | Données réelles |

#### Stack technique (depuis composer.json GitLab)

- **CMS :** Drupal 9.4+
- **PHP :** 8.1+
- **Base de données :** MySQL/MariaDB
- **Search :** Apache Solr
- **Cache :** Redis
- **Serveur web :** Nginx
- **Hébergement :** Claranet

### 4.2 Données de test

**Jeux de données :**
- Utilisateurs test (profils variés)
- Événements billetterie (expositions, visites)
- Configuration module billetterie
- URLs VivaTickets et SeeTickets

**Référence :** [04-donnees-test/](../04-donnees-test/)

### 4.3 Outils et frameworks

**Tests manuels :**
- Navigateurs : Chrome, Firefox, Safari, Edge
- Devices : Desktop, Tablette, Mobile

**Tests automatisés :**
- Selenium WebDriver (tests E2E)
- PHPUnit (tests unitaires backend)
- Behat (tests BDD)
- GitLab CI/CD (automatisation)

**Référence détaillée :** [annexes/outils-frameworks.md](./annexes/outils-frameworks.md)

---

## 5. Organisation et responsabilités

### 5.1 Équipe projet

#### Maîtrise d'ouvrage (EPMO)
- Benjamin (SI-DENUM) : Référent métier
- Martin (SI-DENUM) : Référent métier

#### Maîtrise d'œuvre (Softeam Agency)
- David Tripont : Tech Lead / Référent technique
- Kim Lev : Dev Lead / Développement module
- Hacene TEMAM : Développeur

#### Qualité et tests
- Responsable QA : À définir
- Testeurs : Équipe QA Softeam

### 5.2 Rôles et responsabilités tests

| Rôle | Responsabilités | Personne |
|------|----------------|----------|
| **Responsable QA** | Stratégie tests, validation finale | À définir |
| **Testeur fonctionnel** | Exécution tests manuels | Équipe QA |
| **Testeur technique** | Tests automatisés, performance | Dev team |
| **Validateur métier** | Validation parcours utilisateur | EPMO |

### 5.3 Processus de validation

**Workflow de validation :**

1. **Développement** → Tests unitaires (dev)
2. **Intégration** → Tests d'intégration automatisés (CI/CD)
3. **Préproduction** → Recette fonctionnelle (QA + Métier)
4. **Production** → Smoke tests + Monitoring

**Critères de passage :**
- Intégration → Préproduction : 100% tests automatisés OK
- Préproduction → Production : Validation métier + 0 bug bloquant

**Référence détaillée :** [annexes/processus-validation.md](./annexes/processus-validation.md)

### 5.4 Gestion des anomalies

**Workflow Redmine :**

1. **Détection** → Création ticket Redmine (label `Bug-Billetterie`)
2. **Qualification** → Analyse et priorisation (QA)
3. **Correction** → Développement fix (Dev team)
4. **Validation** → Tests de non-régression (QA)
5. **Clôture** → Validation métier (EPMO)

**Niveaux de criticité :**
- 🔴 **CRITIQUE** : Blocage parcours principal, perte de revenus
- 🟠 **ÉLEVÉE** : Impact métier significatif, workaround possible
- 🟡 **STANDARD** : Confort utilisateur, impact mineur
- 🟢 **BAS** : Cosmétique, documentation

**SLA de traitement :**
- Critique : Correction < 4h, MEP urgente
- Élevée : Correction < 24h, MEP planifiée
- Standard : Correction < 5 jours, prochain sprint
- Bas : Backlog, priorisation ultérieure

---

## 6. Calendrier et jalons

### 6.1 Planning global

| Phase | Période | Activités | Livrables |
|-------|---------|-----------|----------|
| **Préparation** | S38 (16-20 sept) | Analyse sources, rédaction stratégie | Ce document |
| **Levée blocages** | S38-39 | Accès GitLab, API SeeTickets | Accès complets |
| **Développement** | S39-40 (23 sept - 4 oct) | Scripts migration, tests | Module fonctionnel |
| **Tests intégration** | S41 (7-11 oct) | Tests automatisés, CI/CD | Tests verts |
| **Recette préproduction** | S42 (14-18 oct) | Tests fonctionnels, validation | PV de recette |
| **Déploiement production** | S43 (21-25 oct) | MEP, smoke tests | Service en ligne |
| **Stabilisation** | S44 (28 oct - 1 nov) | Monitoring, correctifs | Production stable |

### 6.2 Jalons clés

**📍 Jalons techniques :**
- ✅ 08/10/2025 : Documentation tests complétée
- ⏳ 15/10/2025 : Scripts migration développés
- ⏳ 18/10/2025 : Recette préproduction validée
- ⏳ 25/10/2025 : MEP production

**📍 Jalons métier :**
- Validation EPMO parcours billetterie
- Validation EPMO multilangue
- Validation EPMO rollback
- Go/No-Go production

### 6.3 Critères de passage entre phases

**Intégration → Préproduction :**
- ✅ 100% tests unitaires passent
- ✅ 100% tests d'intégration passent
- ✅ Pipeline CI/CD vert
- ✅ Audit sécurité validé

**Préproduction → Production :**
- ✅ PV de recette signé EPMO
- ✅ 0 bug critique ou bloquant
- ✅ Tests de performance validés
- ✅ Procédure de rollback testée
- ✅ Documentation à jour

---

## 7. Critères de validation

### 7.1 Critères fonctionnels

✅ **Parcours billetterie :**
- [ ] Accès billetterie depuis les 4 sites fonctionne
- [ ] URLs correctement affichées (SeeTickets)
- [ ] Paramètres transmis correctement (langue, événement)
- [ ] Redirection fluide vers plateforme billetterie

✅ **Mécanisme de bascule :**
- [ ] Flag de bascule VivaTickets/SeeTickets opérationnel
- [ ] Double field URL fonctionnel
- [ ] Rollback vers VivaTickets possible
- [ ] Aucune interruption de service lors bascule

✅ **Multilingue :**
- [ ] Interface FR opérationnelle
- [ ] Interface EN opérationnelle
- [ ] Interface ES opérationnelle
- [ ] Cohérence traductions

### 7.2 Critères techniques

✅ **Performance :**
- [ ] Temps réponse < 2s (95e percentile)
- [ ] Charge 100 utilisateurs simultanés supportée
- [ ] Pic 500 utilisateurs/heure supporté
- [ ] Pas de dégradation performance vs existant

✅ **Sécurité :**
- [ ] CVE critiques corrigées (CVE-2025-31675, etc.)
- [ ] Scan sécurité sans vulnérabilité critique
- [ ] Conformité RGPD validée
- [ ] Pas de fuite de données sensibles

✅ **Accessibilité :**
- [ ] RGAA niveau AA atteint
- [ ] Navigation clavier fonctionnelle
- [ ] Lecteurs d'écran compatibles
- [ ] Contrastes conformes

### 7.3 Critères de non-régression

✅ **Fonctionnalités existantes :**
- [ ] Navigation site intacte
- [ ] Recherche fonctionnelle
- [ ] Module Ephoto opérationnel (DAM images)
- [ ] Analytics (Piano, Commander Act) actifs
- [ ] Autres modules non impactés

---

## 8. Gestion des risques

### 8.1 Risques identifiés

| ID | Risque | Impact | Probabilité | Mitigation |
|----|--------|--------|-------------|------------|
| **R1** | Accès GitLab modules custom bloqué | 🔴 Critique | Élevée | Escalade direction + déblocage urgent |
| **R2** | Documentation API SeeTickets incomplète | 🔴 Critique | Moyenne | Réunion urgente SeeTickets |
| **R3** | Régression fonctionnelle non détectée | 🟠 Élevée | Moyenne | Tests régression exhaustifs |
| **R4** | Performance dégradée | 🟠 Élevée | Faible | Tests charge + optimisation |
| **R5** | Problème lors MEP production | 🔴 Critique | Faible | Procédure rollback validée |
| **R6** | Incompatibilité navigateur | 🟡 Standard | Faible | Tests cross-browser |
| **R7** | Charge équipe surchargée | 🟠 Élevée | Élevée | Priorisation stricte + renfort |

### 8.2 Plan de continuité

**En cas de problème majeur en production :**

1. **Détection** : Monitoring alerte
2. **Évaluation** : Qualification impact (< 15 min)
3. **Décision** : Go/No-Go rollback (< 30 min)
4. **Rollback** : Bascule vers VivaTickets (< 1h)
5. **Communication** : Information utilisateurs
6. **Post-mortem** : Analyse et plan d'action

**Mécanisme de rollback :**
- Flag de bascule inversé (SeeTickets → VivaTickets)
- URLs VivaTickets conservées en base
- Aucune migration de données irréversible
- Procédure testée en préproduction

### 8.3 Indicateurs de suivi

**KPIs de recette :**

| Indicateur | Objectif | Seuil alerte |
|------------|----------|-------------|
| Taux de couverture tests | 100% | < 95% |
| Taux de succès tests | ≥ 95% | < 90% |
| Bugs critiques détectés | 0 | ≥ 1 |
| Temps moyen résolution bug | < 24h | > 48h |
| Disponibilité préproduction | 99% | < 95% |

**Tableau de bord :** Suivi quotidien en phase de recette

---

## 📊 Annexes

- [Annexe A : Environnements de test détaillés](./annexes/environnements.md)
- [Annexe B : Processus de validation](./annexes/processus-validation.md)
- [Annexe C : Outils et frameworks](./annexes/outils-frameworks.md)
- [Annexe D : Schémas d'architecture](./schemas/)

---

## 📞 Contacts

**Support technique :**
- Redmine Softeam : [redmine.softeam.agency](https://redmine.softeam.agency)
- Email équipe : epmo-projet@softeam.agency

**Escalade :**
- Niveau 1 : Équipe projet (2h)
- Niveau 2 : Chef de projet (4h)
- Niveau 3 : Direction (24h)

---

**Document validé par :**
- [ ] Responsable QA Softeam
- [ ] Tech Lead (David Tripont)
- [ ] Référent métier EPMO (Benjamin/Martin)
- [ ] Direction projet

---

*Ce document est maintenu par l'équipe QA du projet EPMO Billetterie - Version 1.0.0 (08/10/2025)*