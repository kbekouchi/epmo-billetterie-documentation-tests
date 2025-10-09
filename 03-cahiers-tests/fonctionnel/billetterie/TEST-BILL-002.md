---
title: "TEST-BILL-002 : Bascule VivaTickets → SeeTickets"
version: 1.0.0
date: 2025-10-08
statut: VALIDÉ
criticite: CRITIQUE
type: Fonctionnel
module: Billetterie
references:
  redmine: "#91157"
  git: "/docs/01-architecture-systeme-complete.md"
  gitlab: "softeam-agency/epmo/moo - epmo_billetterie/flag"
---

# TEST-BILL-002 : Bascule VivaTickets → SeeTickets

**Version:** 1.0.0  
**Date:** 08/10/2025  
**Statut:** ✅ VALIDÉ  
**Criticité:** 🔴 CRITIQUE  
**Type:** Fonctionnel  
**Module:** Billetterie - Mécanisme de bascule

---

## Références

- **Redmine:** [#91157 - Script mise à jour URLs billetterie](https://redmine.softeam.agency/issues/91157)
- **Git Specs:** [Architecture système - Mécanisme de bascule](https://github.com/kbekouchi/epmo-billetterie-specifications-techniques/blob/main/docs/01-architecture-systeme-complete.md)
- **GitLab Code:** `softeam-agency/epmo/moo` - Module `epmo_billetterie/` - Composant `flag`
- **User Story:** En tant qu'administrateur, je souhaite basculer la billetterie de VivaTickets vers SeeTickets via un flag de configuration

---

## Objectif

Vérifier que le **mécanisme de bascule** (flag) permet de passer de **VivaTickets à SeeTickets** sans interruption de service, et que l'URL affichée change immédiatement sur tous les sites EPMO après activation du flag.

**Comportement attendu :**
- Le flag de bascule est activable depuis le Back-Office Drupal
- Après activation, l'URL SeeTickets remplace l'URL VivaTickets
- La bascule est immédiate (après vidage cache)
- Aucune interruption de service
- Les 4 sites EPMO sont impactés simultanément
- Le système de double field fonctionne correctement

---

## Contexte technique

### Mécanisme de bascule

Le module `epmo_billetterie` utilise un **flag de configuration** pour déterminer quelle plateforme de billetterie est active :

```php
// Configuration flag
$config = \Drupal::config('epmo_billetterie.settings');
$active_platform = $config->get('active_platform'); // 'vivatickets' ou 'seetickets'
```

### Double field URL

Chaque configuration de billetterie stocke **deux URLs** :
- **URL VivaTickets** (backup)
- **URL SeeTickets** (cible)

Le flag détermine quelle URL est utilisée en front-office.

### Architecture

```
Back-Office Drupal
    ↓
Configuration epmo_billetterie
    ↓
Flag: active_platform = 'seetickets'
    ↓
Front-Office (4 sites)
    ↓
Affichage URL SeeTickets
```

---

## Prérequis

### Environnement
- **Environnement de test :** Préproduction
- **URLs préproduction :**
  - BO Drupal : `https://mo-preprod.musee-orsay.fr/fr/bo`
  - Site MO : `https://mo-preprod.musee-orsay.fr/fr`
  - Site LO : `https://lo-preprod.musee-orangerie.fr/fr`
  - Site EPMO : `https://epmo-preprod.epmo-musees.fr/fr`
  - Site Petits MO : `https://petitsmo-preprod.petitsmo.fr/fr`

### Configuration initiale
- ✅ Module `epmo_billetterie` installé et activé
- ✅ **Flag configuré sur VivaTickets** (état initial)
- ✅ Double field URL configuré :
  - URL VivaTickets : `https://www.vivaticket.com/fr/ticket/musee-orsay`
  - URL SeeTickets : `https://billetterie.seetickets.fr/musee-orsay`

### Accès
- **Compte administrateur Drupal** avec permissions :
  - `administer epmo_billetterie configuration`
  - `access administration pages`
  - `administer site configuration`
- Navigateur : Chrome 120+ ou Firefox 120+

### Données de test

| Paramètre | Valeur |
|-----------|--------|
| **État initial flag** | VivaTickets |
| **État cible flag** | SeeTickets |
| **URL VivaTickets** | https://www.vivaticket.com/fr/ticket/musee-orsay |
| **URL SeeTickets** | https://billetterie.seetickets.fr/musee-orsay |

---

## État initial à vérifier (AVANT bascule)

### Étape 0.1 : Vérification état VivaTickets

**Action:**  
1. Ouvrir le navigateur
2. Accéder à : `https://mo-preprod.musee-orsay.fr/fr`
3. Localiser le bouton "BILLETTERIE"
4. Inspecter l'URL (F12 → Éléments)

**Résultat attendu AVANT bascule:**
- ✅ URL affichée = **VivaTickets**
- ✅ Format : `https://www.vivaticket.com/...`
- ✅ **PAS d'URL SeeTickets**

### Étape 0.2 : Vérification configuration BO

**Action:**  
1. Se connecter au BO Drupal : `https://mo-preprod.musee-orsay.fr/fr/bo/user/login`
2. Aller dans : **Configuration → EPMO → Billetterie**
   - URL : `/admin/config/epmo/billetterie`
3. Vérifier le paramètre "Plateforme active"

**Résultat attendu AVANT bascule:**
- ✅ Plateforme active = **VivaTickets** ☑️
- ✅ URL VivaTickets visible et renseignée
- ✅ URL SeeTickets visible et renseignée (double field)
- ✅ Bouton "Enregistrer la configuration" visible

---

## Étapes de test - BASCULE

### Étape 1 : Accès à la configuration billetterie (BO)

**Action:**  
1. Se connecter au Back-Office Drupal
   - URL : `https://mo-preprod.musee-orsay.fr/fr/bo/user/login`
   - Identifiants : Compte administrateur
2. Naviguer vers : **Configuration → EPMO → Billetterie**
   - Chemin : Structure → Configuration → EPMO
   - URL directe : `/admin/config/epmo/billetterie`

**Résultat attendu:**
- ✅ Page de configuration billetterie affichée
- ✅ Section "Plateforme de billetterie active" visible
- ✅ Champ "Plateforme active" avec options :
  - ⚪ VivaTickets
  - ⚪ SeeTickets
- ✅ Section "URLs de billetterie" visible avec deux champs :
  - Champ "URL VivaTickets"
  - Champ "URL SeeTickets"

---

### Étape 2 : Capture état AVANT bascule

**Action:**  
1. Noter l'état actuel :
   - Plateforme active : [VivaTickets ☑️]
   - URL VivaTickets : [Valeur actuelle]
   - URL SeeTickets : [Valeur actuelle]
2. Faire une **capture d'écran** de la configuration AVANT
3. Ouvrir un onglet et vérifier un site front :
   - `https://mo-preprod.musee-orsay.fr/fr`
4. Inspecter l'URL billetterie (F12)
5. **Capturer** l'URL actuelle (VivaTickets)

**Résultat attendu:**
- ✅ Configuration VivaTickets active confirmée
- ✅ URL VivaTickets affichée en front
- ✅ Captures AVANT réalisées

---

### Étape 3 : Activation du flag SeeTickets

**Action:**  
1. Dans la page de configuration BO `/admin/config/epmo/billetterie`
2. Localiser le champ "Plateforme active"
3. **Sélectionner : SeeTickets ☑️**
4. Vérifier que les deux URLs sont bien renseignées :
   - URL VivaTickets : `https://www.vivaticket.com/fr/ticket/musee-orsay`
   - URL SeeTickets : `https://billetterie.seetickets.fr/musee-orsay`
5. Cliquer sur le bouton **"Enregistrer la configuration"**
6. Observer le message de confirmation

**Résultat attendu:**
- ✅ Option "SeeTickets" sélectionnable
- ✅ Les deux URLs restent visibles (double field)
- ✅ Bouton "Enregistrer" cliquable
- ✅ Message de confirmation affiché :
  - "La configuration a été enregistrée." (bandeau vert)
- ✅ **Aucune erreur** affichée
- ✅ Page reste sur la configuration (pas de redirection)

---

### Étape 4 : Vérification configuration enregistrée

**Action:**  
1. Rester sur la page de configuration `/admin/config/epmo/billetterie`
2. Vérifier que le flag est bien passé à SeeTickets
3. Alternativement, rafraîchir la page (F5)
4. Confirmer l'état enregistré

**Résultat attendu:**
- ✅ Plateforme active = **SeeTickets ☑️** (sélection conservée)
- ✅ URL VivaTickets toujours présente (backup)
- ✅ URL SeeTickets toujours présente (active)
- ✅ Configuration persistée en base de données

---

### Étape 5 : Vidage du cache Drupal

**Action:**  
1. Aller dans : **Configuration → Développement → Performances**
   - URL : `/admin/config/development/performance`
2. Cliquer sur le bouton **"Vider tous les caches"**
3. Attendre la confirmation
4. Alternativement, utiliser Drush :
   ```bash
   drush cr
   ```

**Résultat attendu:**
- ✅ Message : "Les caches ont été vidés." (bandeau vert)
- ✅ Page rafraîchie
- ✅ Aucune erreur
- ✅ Site reste accessible

**⚠️ Note importante :**
Le vidage de cache est **OBLIGATOIRE** pour que le changement soit pris en compte en front-office.

---

### Étape 6 : Vérification front Musée d'Orsay

**Action:**  
1. Ouvrir un **nouvel onglet** (ou navigation privée)
2. Accéder à : `https://mo-preprod.musee-orsay.fr/fr`
3. Localiser le bouton "BILLETTERIE"
4. Ouvrir DevTools (F12) → Onglet Éléments
5. Inspecter l'élément `<a>` du bouton billetterie
6. Vérifier l'attribut `href`

**Résultat attendu:**
- ✅ Attribut `href` contient l'URL **SeeTickets**
- ✅ Format : `https://billetterie.seetickets.fr/musee-orsay`
- ✅ **Aucune URL VivaTickets** : `https://www.vivaticket.com/...`
- ✅ Paramètres éventuels transmis : `?lang=fr`

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

### Étape 7 : Vérification front Musée de l'Orangerie

**Action:**  
Répéter l'étape 6 sur le site Orangerie :
1. Accéder à : `https://lo-preprod.musee-orangerie.fr/fr`
2. Inspecter le bouton billetterie
3. Vérifier l'URL

**Résultat attendu:**
- ✅ URL SeeTickets affichée
- ✅ Format : `https://billetterie.seetickets.fr/musee-orangerie?lang=fr`
- ✅ Aucune URL VivaTickets

---

### Étape 8 : Vérification front Site EPMO

**Action:**  
Répéter l'étape 6 sur le site EPMO :
1. Accéder à : `https://epmo-preprod.epmo-musees.fr/fr`
2. Inspecter le bouton billetterie
3. Vérifier l'URL

**Résultat attendu:**
- ✅ URL SeeTickets affichée
- ✅ Aucune URL VivaTickets

---

### Étape 9 : Vérification front Petits MO

**Action:**  
Répéter l'étape 6 sur le site Petits MO :
1. Accéder à : `https://petitsmo-preprod.petitsmo.fr/fr`
2. Inspecter le bouton billetterie
3. Vérifier l'URL

**Résultat attendu:**
- ✅ URL SeeTickets affichée
- ✅ Aucune URL VivaTickets

---

### Étape 10 : Test de clic réel (redirection)

**Action:**  
1. Sur **un des 4 sites** (ex: Musée d'Orsay)
2. Cliquer sur le bouton **"BILLETTERIE"**
3. Observer la redirection

**Résultat attendu:**
- ✅ **Nouvelle fenêtre/onglet** s'ouvre (`target="_blank"`)
- ✅ URL dans la barre d'adresse = **SeeTickets**
- ✅ Page billetterie SeeTickets s'affiche correctement
- ✅ Interface SeeTickets visible (différente de VivaTickets)
- ✅ **Aucune redirection** vers VivaTickets
- ✅ Aucune erreur 404 ou 500
- ✅ Le site d'origine reste ouvert dans l'onglet précédent

---

### Étape 11 : Vérification base de données (optionnel)

**Action:**  
Si accès base de données disponible :
1. Se connecter à la base MySQL/MariaDB de préproduction
2. Requête SQL :
   ```sql
   SELECT name, value 
   FROM config 
   WHERE name = 'epmo_billetterie.settings';
   ```
3. Vérifier la configuration sérialisée

**Résultat attendu:**
- ✅ Configuration contient : `active_platform: 'seetickets'`
- ✅ URL VivaTickets présente (backup)
- ✅ URL SeeTickets présente (active)

---

### Étape 12 : Test multilingue après bascule

**Action:**  
1. Sur le site Musée d'Orsay
2. Basculer en **Anglais (EN)**
3. Vérifier l'URL billetterie
4. Basculer en **Espagnol (ES)**
5. Vérifier l'URL billetterie

**Résultat attendu:**
- ✅ EN : URL SeeTickets avec `?lang=en`
- ✅ ES : URL SeeTickets avec `?lang=es`
- ✅ **Aucune URL VivaTickets** dans aucune langue
- ✅ Paramètre de langue correctement transmis

---

### Étape 13 : Vérification logs (optionnel)

**Action:**  
1. Aller dans : **Rapports → Journaux récents**
   - URL : `/admin/reports/dblog`
2. Filtrer par type : `epmo_billetterie`
3. Rechercher les logs liés à la bascule

**Résultat attendu:**
- ✅ Log d'information : "Configuration billetterie mise à jour"
- ✅ Log : "Plateforme active changée : VivaTickets → SeeTickets"
- ✅ Aucun log d'erreur
- ✅ Horodatage correct

---

## Résultats attendus globaux

### Fonctionnel

✅ **Avant bascule :**
- Plateforme active = VivaTickets
- URL VivaTickets affichée sur les 4 sites

✅ **Après bascule :**
- Plateforme active = SeeTickets
- URL SeeTickets affichée sur les 4 sites
- URL VivaTickets conservée en base (backup)

### Technique

✅ **Configuration :**
- Flag `active_platform` = 'seetickets' en base
- Double field URL intact
- Configuration persistée

✅ **Front-Office :**
- Attribut `href` contient URL SeeTickets
- Paramètres URL transmis (langue)
- Aucune URL VivaTickets visible

✅ **UX/UI :**
- Aucune interruption de service
- Redirection fonctionnelle
- Comportement cohérent sur les 4 sites

### Performance

✅ **Pas d'impact performance :**
- Temps de chargement identique
- Aucune latence ajoutée
- Cache efficace après vidage

---

## Critères de validation

| Critère | Vérification | Statut |
|---------|--------------|--------|
| Flag activable en BO | ✓ Option SeeTickets sélectionnable | ⬜ À tester |
| Configuration enregistrée | ✓ Message confirmation | ⬜ À tester |
| URL SeeTickets affichée MO | ✓ Inspecter href | ⬜ À tester |
| URL SeeTickets affichée LO | ✓ Inspecter href | ⬜ À tester |
| URL SeeTickets affichée EPMO | ✓ Inspecter href | ⬜ À tester |
| URL SeeTickets affichée PMO | ✓ Inspecter href | ⬜ À tester |
| URL VivaTickets absente front | ✓ Aucune occurrence | ⬜ À tester |
| Redirection fonctionnelle | ✓ Clic → SeeTickets | ⬜ À tester |
| Multilingue opérationnel | ✓ FR/EN/ES | ⬜ À tester |
| Aucune interruption service | ✓ Site accessible | ⬜ À tester |
| Double field conservé | ✓ 2 URLs en BO | ⬜ À tester |

**Test réussi si :** Tous les critères sont cochés ✅

---

## Gestion des anomalies

### En cas d'échec du test, collecter :

#### 1. Captures d'écran

- ✅ Configuration BO **AVANT** bascule
- ✅ Configuration BO **APRÈS** bascule
- ✅ Message de confirmation (ou erreur)
- ✅ URL front **AVANT** bascule (DevTools)
- ✅ URL front **APRÈS** bascule (DevTools)
- ✅ Page SeeTickets après clic (ou erreur)

#### 2. Informations techniques

**Configuration :**
- État flag AVANT : [VivaTickets/SeeTickets]
- État flag APRÈS : [VivaTickets/SeeTickets]
- Message BO : [Copie exacte]
- Cache vidé : [Oui/Non]

**Front-Office :**
- Site testé : [MO/LO/EPMO/PMO]
- URL constatée : [URL exacte]
- URL attendue : [URL SeeTickets]
- Navigateur : [Chrome/Firefox + version]

**Base de données (si accessible) :**
```sql
SELECT * FROM config WHERE name = 'epmo_billetterie.settings';
```

#### 3. Logs

**Logs Drupal :**
- `/admin/reports/dblog`
- Filtrer par : `epmo_billetterie`
- Copier les logs d'erreur éventuels

**Logs serveur (si accessible) :**
- `/var/log/apache2/error.log` ou
- `/var/log/nginx/error.log`

**Console navigateur :**
- F12 → Console
- Copier toutes les erreurs JavaScript

---

### Procédure de signalement

**Créer un ticket Redmine avec :**

- **Projet :** EPMO - TMA
- **Tracker :** Bug
- **Sujet :** `[Billetterie] Échec bascule VivaTickets → SeeTickets`
- **Priorité :** Urgent (fonctionnalité critique)
- **Assigné à :** Kim Lev ou David Tripont
- **Environnement :** Préproduction
- **Référence test :** `TEST-BILL-002`

**Template description :**
```
Test échoué : TEST-BILL-002 - Bascule VivaTickets → SeeTickets

Environnement : Préproduction
Étape échouée : [Numéro étape]

Comportement constaté :
- Flag configuré : [État]
- URL affichée : [URL constatée]
- Sites impactés : [MO/LO/EPMO/PMO]

Comportement attendu :
- Flag = SeeTickets
- URL SeeTickets affichée sur tous les sites

Actions réalisées :
1. Activation flag SeeTickets en BO
2. Vidage cache Drupal
3. Vérification front-office

Captures jointes :
- [Liste des captures]

Logs :
- [Copier logs pertinents]

Analyse complémentaire :
- Double field intact : [Oui/Non]
- Configuration enregistrée : [Oui/Non]
- Cache vidé : [Oui/Non]
```

---

### Cas particuliers

#### Cas 1 : URL VivaTickets persiste après bascule

**Cause probable :**
- Cache non vidé
- Configuration non enregistrée
- Problème de permission

**Actions correctives :**
1. Vider le cache : `/admin/config/development/performance`
2. Vider le cache navigateur (Ctrl+Shift+Delete)
3. Vérifier configuration : `/admin/config/epmo/billetterie`
4. Tester en navigation privée

#### Cas 2 : Message d'erreur lors de l'enregistrement

**Erreurs possibles :**
- "Vous n'avez pas la permission..."
- "Une erreur s'est produite..."

**Actions correctives :**
1. Vérifier permissions utilisateur : `/admin/people/permissions`
2. Vérifier logs : `/admin/reports/dblog`
3. Contacter administrateur système

#### Cas 3 : URL vide après bascule

**Cause probable :**
- Champ URL SeeTickets non renseigné
- Erreur de configuration

**Actions correctives :**
1. Vérifier configuration : `/admin/config/epmo/billetterie`
2. S'assurer que le champ "URL SeeTickets" est bien rempli
3. Réenregistrer la configuration

---

## Tags et statut

- ✅ **[VALIDÉ]** : Test validé en recette
- 🔄 **[AUTOMATISABLE]** : Test partiellement automatisable
  - Configuration BO : Manuel (Selenium)
  - Vérification front : Automatisable (Selenium)
  - Script : `tests/e2e/billetterie/test_bascule_seetickets.js`
- 🔴 **[CRITIQUE]** : Test bloquant pour MEP production
- ⚠️ **[RISQUE ÉLEVÉ]** : Impact direct sur continuité de service

---

## Automatisation

### Script Selenium (exemple partiel)

```javascript
// tests/e2e/billetterie/test_bascule_seetickets.js

describe('TEST-BILL-002: Bascule vers SeeTickets', () => {
  
  it('doit afficher URL SeeTickets après activation flag', async () => {
    // Étape 6 : Vérification front MO
    await browser.url('https://mo-preprod.musee-orsay.fr/fr');
    
    const billetterieBtn = await $('.btn-billetterie');
    const href = await billetterieBtn.getAttribute('href');
    
    // Assertions
    expect(href).toContain('seetickets.fr');
    expect(href).not.toContain('vivaticket.com');
  });
  
  it('doit afficher URL SeeTickets sur les 4 sites', async () => {
    const sites = [
      'https://mo-preprod.musee-orsay.fr/fr',
      'https://lo-preprod.musee-orangerie.fr/fr',
      'https://epmo-preprod.epmo-musees.fr/fr',
      'https://petitsmo-preprod.petitsmo.fr/fr'
    ];
    
    for (const site of sites) {
      await browser.url(site);
      const btn = await $('.btn-billetterie');
      const href = await btn.getAttribute('href');
      
      expect(href).toContain('seetickets.fr');
    }
  });
});
```

**Note :** La partie configuration BO nécessite une automatisation manuelle ou via Drush.

---

## Historique

| Version | Date | Auteur | Modifications |
|---------|------|--------|---------------|
| 1.0.0 | 08/10/2025 | QA Team | Création initiale complète |

---

## Liens connexes

- [TEST-BILL-001](./TEST-BILL-001.md) : Affichage URL billetterie SeeTickets
- **[TEST-BILL-003](./TEST-BILL-003.md)** : Rollback SeeTickets → VivaTickets (test complémentaire)
- [TEST-BILL-004](./TEST-BILL-004.md) : Double field URL
- [Plan de tests - Module Billetterie](../../02-plan-tests/plan-tests-general.md)
- [Stratégie de recette](../../01-strategie-recette/strategie-recette.md)

---

*Ce test est maintenu par l'équipe QA du projet EPMO Billetterie - Version 1.0.0 (08/10/2025)*
