# Setup Firebase — Simulateur de devis AMS

Projet : `ams-simulateur-devis` · Région Firestore : `eur3`

Le code client est déjà branché dans `simulateurdevisAMSV9.html`.
Il reste 4 opérations **à faire une fois dans la console Firebase**.

---

## 1. Publier les règles Firestore

Console → **Firestore Database → Règles** → colle le contenu de `firestore.rules`
(dans ce dossier) → **Publier**.

Tant que ces règles ne sont pas publiées, la connexion échouera (« Compte non reconnu »).

---

## 2. Activer la connexion e-mail / mot de passe

Console → **Authentication → Sign-in method** →
active **« Adresse e-mail/Mot de passe »**.
Laisse **« Lien de connexion par e-mail » désactivé**.

---

## 3. Créer le premier compte admin

> Le double-clic sur le logo ouvre une invite « Identifiant admin / Code admin ».

### 3a. Créer l'utilisateur Auth
Console → **Authentication → Users → Ajouter un utilisateur**
- E-mail : `admin@ams-simulateur-devis.web.app`
- Mot de passe : un code d'**au moins 6 caractères** (ce sera le « Code admin »)
- **Copie l'UID** de l'utilisateur créé (colonne « Identifiant utilisateur »).

### 3b. Créer le flag admin dans Firestore
Console → **Firestore Database → Données → Démarrer une collection**
- ID de la collection : `admins`
- ID du document : **colle l'UID copié à l'étape 3a**
- Ajoute un champ quelconque, ex. `label` (type *string*) = `Admin AMS`
- **Enregistrer**

➡️ Connexion admin = identifiant `ADMIN` + le code choisi.
(`ADMIN` → `admin@ams-simulateur-devis.web.app`, en minuscules, automatiquement.)

---

## 4. Créer le compte concessionnaire de test

### 4a. Utilisateur Auth
Console → **Authentication → Users → Ajouter un utilisateur**
- E-mail : `ams@ams-simulateur-devis.web.app`
- Mot de passe : un code d'au moins 6 chiffres, ex. `001234`
- **Copie l'UID**.

### 4b. Fiche concessionnaire dans Firestore
Console → **Firestore Database → Données** →
collection `concessionnaires` → **Ajouter un document**
- ID du document : **l'UID copié à l'étape 4a**
- Champs :

| Champ             | Type   | Valeur              |
|------------------|--------|---------------------|
| `loginId`         | string | `AMS`               |
| `label`           | string | `Compte test AMS`   |
| `agencyGroup`     | string | `Test`              |
| `priceMultiplier` | number | `1`                 |

➡️ Connexion concessionnaire = identifiant `AMS` + code `001234`.

---

## 5. Catalogue prix / prestations

Pour l'instant le catalogue reste **codé en dur** dans le HTML (tableau `services`),
utilisé comme valeur par défaut.

La migration vers Firestore (`config/services`) + le CRUD se font à l'**étape 2**
(espace admin). Aucune action requise maintenant.

---

## Correspondance identifiant → e-mail (interne)

L'identifiant saisi est mis en minuscules, nettoyé, puis suffixé :

```
AMS    → ams@ams-simulateur-devis.web.app
ADMIN  → admin@ams-simulateur-devis.web.app
```

Ce domaine ne sert jamais à recevoir du courrier. Un « mot de passe oublié »
n'est donc pas possible : l'admin réattribue un code depuis l'espace admin
(étape 2) ou depuis Authentication → Users → menu ⋮ → « Réinitialiser le mot de passe »
n'enverra rien d'utile — préférer la suppression / recréation.
