---
title: "TEST-BILL-001 : Affichage URL billetterie SeeTickets"
version: 1.0.0
date: 2025-10-08
statut: VALIDÉ
criticite: CRITIQUE
type: Fonctionnel
module: Billetterie
references:
  redmine: "#91157"
  git: "/docs/01-architecture-systeme-complete.md"
  gitlab: "softeam-agency/epmo/ep - epmo_billetterie/"
---

# TEST-BILL-001 : Affichage URL billetterie SeeTickets

**Version:** 1.0.0  
**Date:** 08/10/2025  
**Statut:** ✅ VALIDÉ  
**Criticité:** 🔴 CRITIQUE  
**Type:** Fonctionnel  
**Module:** Billetterie

---

## Références

- **Redmine:** [#91157 - Script mise à jour URLs billetterie](https://redmine.softeam.agency/issues/91157)
- **Git Specs:** [Architecture système](https://github.com/kbekouchi/epmo-billetterie-specifications-techniques/blob/main/docs/01-architecture-systeme-complete.md)
- **GitLab Code:** `softeam-agency/epmo/ep` - Module `epmo_billetterie/`
- **User Story:** En tant qu'utilisateur, je souhaite accéder à la billetterie SeeTickets depuis le site du musée

---

## Objectif

Vérifier que l'URL de la billetterie **SeeTickets** s'affiche correctement sur tous les sites EPMO après migration depuis VivaTickets, conformément aux spécifications du ticket Redmine #91157.

**Comportement attendu :**
- L'URL SeeTickets est affichée au lieu de l'URL VivaTickets
- Le lien est cliquable et fonctionnel
- L'affichage est correct sur les 4 sites (MO, LO, EPMO, Petits MO)
- Le support multilingue est opérationnel (FR/EN/ES)

---

## Prérequis

### Environnement
- **Environnement de test :** Préproduction
- **URLs préproduction :**
  - Musée d'Orsay : `https://mo-preprod.musee-orsay.fr`
  - Musée Orangerie : `https://lo-preprod.musee-orangerie.fr`
  - EPMO : `https://epmo-preprod.epmo-musees.fr`
  - Petits MO : `https://petitsmo-preprod.petitsmo.fr`

### Configuration
- Flag de bascule activé vers **SeeTickets** (configuration BO)
- Module `epmo_billetterie` installé et activé
- Cache Drupal vidé

### Accès
- Compte administrateur Drupal (accès BO si besoin)
- Navigateur : Chrome 120+ ou Firefox 120+ (version récente)

### Données de test
- URL SeeTickets configurée : `https://billetterie.seetickets.fr/musee-orsay` (exemple)
- URL VivaTickets (backup) : `https://www.vivaticket.com/fr/ticket/musee-orsay` (exemple)

---

## Données de test

| Paramètre | Valeur |
|-----------|--------|
| **Site testé** | Musée d'Orsay (MO) |
| **URL SeeTickets** | https://billetterie.seetickets.fr/musee-orsay |
| **URL VivaTickets (backup)** | https://www.vivaticket.com/fr/ticket/musee-orsay |
| **Langue test** | Français (FR) |
| **Device** | Desktop (résolution 1920x1080) |
| **Navigateur** | Chrome 120+ |

---

## Étapes de test

### Étape 1 : Accès à la page d'accueil

**Action:**  
1. Ouvrir le navigateur Chrome
2. Accéder à l'URL : `https://mo-preprod.musee-orsay.fr/fr`
3. Vérifier que la page s'affiche correctement

**Résultat attendu:**
- ✅ Page d'accueil du musée affichée correctement
- ✅ Langue française active (FR)
- ✅ Navigation principale visible
- ✅ Aucune erreur affichée

---

### Étape 2 : Localisation du bouton/lien billetterie

**Action:**  
1. Repérer le bouton ou lien "BILLETTERIE" dans le header
2. Observer son emplacement et son apparence

**Résultat attendu:**
- ✅ Bouton "BILLETTERIE" visible dans le header (coin supérieur droit)
- ✅ Icône billetterie présente (selon design system)
- ✅ Texte "BILLETTERIE" lisible
- ✅ Style cohérent avec la charte graphique

---

### Étape 3 : Inspection de l'URL (DevTools)

**Action:**  
1. Ouvrir les outils de développement (F12)
2. Aller dans l'onglet "Éléments" ou "Inspecteur"
3. Localiser l'élément `<a>` du bouton billetterie
4. Vérifier l'attribut `href`

**Résultat attendu:**
- ✅ Attribut `href` contient l'URL **SeeTickets**
- ✅ Format URL correct : `https://billetterie.seetickets.fr/...`
- ✅ **PAS d'URL VivaTickets** : `https://www.vivaticket.com/...`
- ✅ Paramètres URL présents si nécessaire (langue, événement)

**Exemple attendu :**
```html
<a href="https://billetterie.seetickets.fr/musee-orsay?lang=fr" 
   class="btn-billetterie" 
   target="_blank" 
   rel="noopener noreferrer">
  BILLETTERIE
</a>
```

---

### Étape 4 : Test du clic (sans quitter la page)

**Action:**  
1. Faire un **clic droit** sur le bouton "BILLETTERIE"
2. Sélectionner "Copier l'adresse du lien"
3. Coller l'URL dans un éditeur de texte
4. Vérifier l'URL copiée

**Résultat attendu:**
- ✅ URL copiée = URL SeeTickets
- ✅ Format : `https://billetterie.seetickets.fr/...`
- ✅ Aucune URL VivaTickets détectée

---

### Étape 5 : Test du clic réel (redirection)

**Action:**  
1. Cliquer sur le bouton "BILLETTERIE"
2. Observer la redirection

**Résultat attendu:**
- ✅ Nouvelle fenêtre/onglet s'ouvre (`target="_blank"`)
- ✅ Redirection vers le site **SeeTickets**
- ✅ URL dans la barre d'adresse commence par `https://billetterie.seetickets.fr/` ou domaine SeeTickets
- ✅ Page billetterie SeeTickets s'affiche (interface SeeTickets visible)
- ✅ Pas de redirection vers VivaTickets
- ✅ Pas d'erreur 404 ou 500

---

### Étape 6 : Vérification sur les autres sites

**Action:**  
Répéter les étapes 1 à 5 sur les 3 autres sites :
1. Musée Orangerie : `https://lo-preprod.musee-orangerie.fr/fr`
2. EPMO : `https://epmo-preprod.epmo-musees.fr/fr`
3. Petits MO : `https://petitsmo-preprod.petitsmo.fr/fr`

**Résultat attendu:**
- ✅ URL SeeTickets affichée sur **tous les sites**
- ✅ Comportement identique sur les 4 sites
- ✅ Aucune URL VivaTickets détectée

---

### Étape 7 : Test multilingue (EN et ES)

**Action:**  
1. Basculer la langue du site en **Anglais (EN)**
2. Vérifier le bouton billetterie
3. Basculer en **Espagnol (ES)**
4. Vérifier le bouton billetterie

**Résultat attendu:**
- ✅ URL SeeTickets présente en **EN** : `?lang=en`
- ✅ URL SeeTickets présente en **ES** : `?lang=es`
- ✅ Paramètre de langue correctement transmis
- ✅ Texte du bouton traduit ("TICKETS" en EN, "ENTRADAS" en ES selon specs)

---

## Résultats attendus globaux

### Fonctionnel
- ✅ URL **SeeTickets** affichée et fonctionnelle
- ✅ URL **VivaTickets** non utilisée (remplacée)
- ✅ Redirection correcte vers plateforme SeeTickets
- ✅ Support multilingue opérationnel (FR/EN/ES)
- ✅ Comportement cohérent sur les 4 sites

### Technique
- ✅ Attribut `href` contient URL SeeTickets
- ✅ Attribut `target="_blank"` présent (nouvelle fenêtre)
- ✅ Attribut `rel="noopener noreferrer"` présent (sécurité)
- ✅ Paramètres URL transmis correctement

### UX/UI
- ✅ Bouton visible et accessible
- ✅ Design cohérent avec charte graphique
- ✅ Aucune erreur affichée

---

## Critères de validation

| Critère | Vérification | Statut |
|---------|--------------|--------|
| URL SeeTickets affichée | ✓ Inspecter href | ⬜ À tester |
| URL VivaTickets absente | ✓ Aucune occurrence | ⬜ À tester |
| Redirection fonctionnelle | ✓ Clic → SeeTickets | ⬜ À tester |
| Multilingue FR/EN/ES | ✓ Paramètre lang | ⬜ À tester |
| 4 sites opérationnels | ✓ MO/LO/EPMO/Petits MO | ⬜ À tester |

**Test réussi si :** Tous les critères sont cochés ✅

---

## Gestion des anomalies

### En cas d'échec du test, collecter :

1. **Captures d'écran :**
   - Bouton billetterie dans le header
   - Élément HTML inspecté (DevTools)
   - URL dans la barre d'adresse après clic
   - Page SeeTickets ou erreur affichée

2. **Informations techniques :**
   - URL exacte testée
   - Navigateur et version
   - Langue du site au moment du test
   - Message d'erreur éventuel (copie texte)
   - Console navigateur (onglet Console DevTools)

3. **Vérifications complémentaires :**
   - Vérifier cache Drupal vidé : `/admin/config/development/performance`
   - Vérifier configuration module : `/admin/config/epmo/billetterie`
   - Vérifier flag de bascule : État SeeTickets/VivaTickets

### Procédure de signalement

**Créer un ticket Redmine avec :**
- **Projet :** EPMO - TMA
- **Tracker :** Bug
- **Sujet :** `[Billetterie] URL SeeTickets non affichée - [Site concerné]`
- **Description :** Détails complets avec captures
- **Référence test :** `TEST-BILL-001`
- **Priorité :** Urgent (fonctionnalité critique)
- **Environnement :** Préproduction
- **Assigné à :** Kim Lev ou David Tripont

**Template description ticket :**
```
Test échoué : TEST-BILL-001

Environnement : Préproduction
Site testé : [Musée d'Orsay / Orangerie / EPMO / Petits MO]
Langue : [FR / EN / ES]

Comportement constaté :
- [Décrire le problème exact]

Comportement attendu :
- URL SeeTickets affichée et fonctionnelle

Captures et logs :
- [Joindre captures d'écran]
- [Copier logs console si erreur]

URL testée : [URL exacte]
Navigateur : [Chrome/Firefox + version]
```

---

## Tags et statut

- ✅ **[VALIDÉ]** : Test validé en recette le 08/10/2025
- 🔄 **[AUTOMATISABLE]** : Test automatisable Selenium
  - Script : `tests/e2e/billetterie/test_url_seetickets.js`
  - Pipeline : CI/CD automatique sur chaque MR
- 🔴 **[CRITIQUE]** : Test bloquant pour MEP production

---

## Automatisation

### Script Selenium (exemple)

```javascript
// tests/e2e/billetterie/test_url_seetickets.js

describe('TEST-BILL-001: URL SeeTickets', () => {
  it('doit afficher URL SeeTickets sur MO', async () => {
    await browser.url('https://mo-preprod.musee-orsay.fr/fr');
    
    const billetterieBtn = await $('.btn-billetterie');
    const href = await billetterieBtn.getAttribute('href');
    
    expect(href).toContain('seetickets.fr');
    expect(href).not.toContain('vivaticket.com');
  });
});
```

---

## Historique

| Version | Date | Auteur | Modifications |
|---------|------|--------|---------------|
| 1.0.0 | 08/10/2025 | QA Team | Création initiale |

---

## Liens connexes

- [TEST-BILL-002](./TEST-BILL-002.md) : Bascule VivaTickets → SeeTickets
- [TEST-BILL-003](./TEST-BILL-003.md) : Rollback SeeTickets → VivaTickets
- [TEST-BILL-004](./TEST-BILL-004.md) : Double field URL
- [Plan de tests - Module Billetterie](../../02-plan-tests/plan-tests-general.md)
- [Stratégie de recette](../../01-strategie-recette/strategie-recette.md)

---

*Ce test est maintenu par l'équipe QA du projet EPMO Billetterie - Version 1.0.0 (08/10/2025)*
