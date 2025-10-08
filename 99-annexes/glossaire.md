# 📖 Glossaire - Migration Billetterie EPMO

**Version:** 1.0.0  
**Date:** 08/10/2025

---

## A

**API (Application Programming Interface)**  
Interface de programmation permettant la communication entre le module Drupal et les plateformes de billetterie (VivaTickets/SeeTickets).

**Automatisation**  
Exécution automatique de tests via scripts (Selenium, PHPUnit) dans un pipeline CI/CD.

---

## B

**Backend**  
Partie serveur de l'application Drupal, incluant la base de données et la logique métier.

**Bascule (Flag de)**  
Mécanisme permettant de basculer entre VivaTickets et SeeTickets sans modification de code.

**BO (Back-Office)**  
Interface d'administration Drupal accessible aux contributeurs et administrateurs.

---

## C

**Cache**  
Système de stockage temporaire des données pour améliorer les performances.

**CI/CD (Continuous Integration / Continuous Deployment)**  
Pipeline automatisé d'intégration et de déploiement continu (GitLab CI/CD).

**Criticité**  
Niveau d'importance d'un test ou d'un bug :
- 🔴 **CRITIQUE** : Bloquant, impact majeur
- 🟠 **ÉLEVÉE** : Impact significatif
- 🟡 **STANDARD** : Impact mineur

**CVE (Common Vulnerabilities and Exposures)**  
Identifiant unique pour une vulnérabilité de sécurité connue.

---

## D

**Double field**  
Mécanisme technique stockant deux URLs (VivaTickets + SeeTickets) pour permettre le rollback.

**Drupal**  
CMS open-source utilisé pour les sites EPMO (version 9.4+).

---

## E

**EPMO**  
Établissement Public des Musées d'Orsay et de l'Orangerie.

**epmo_billetterie**  
Module Drupal custom gérant l'intégration avec les plateformes de billetterie.

---

## R

**Recette**  
Phase de tests de validation avant mise en production.

**Redmine**  
Outil de gestion de projet et de tickets utilisé par Softeam Agency.

**RGAA**  
Référentiel Général d'Amélioration de l'Accessibilité (niveau AA visé).

**Rollback**  
Retour à la configuration précédente en cas de problème.

---

## S

**SeeTickets**  
Nouvelle plateforme de billetterie cible de la migration.

**Selenium**  
Framework de test automatisé pour applications web.

---

## T

**TMGMT**  
Translation Management Tool - Module Drupal pour les traductions.

**Traçabilité**  
Lien entre exigences, spécifications, code et tests.

---

## V

**VivaTickets**  
Ancienne plateforme de billetterie, conservée en backup.

---

**Dernière mise à jour:** 08/10/2025
