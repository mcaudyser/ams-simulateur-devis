# Setup Firebase — Simulateur de devis AMS

Projet : `ams-simulateur-devis` · Région Firestore : `eur3`

La config client est déjà dans `index.html`. Opérations à faire **une fois** dans
la console Firebase pour repartir d'un projet vierge.

---

## 1. Publier les règles Firestore

Console → **Firestore Database → Règles** → coller le contenu de `firestore.rules`
→ **Publier**. Sans ça, toute connexion échoue (« Compte non reconnu »).

## 2. Activer la connexion e-mail / mot de passe

Console → **Authentication → Sign-in method** → activer
**« Adresse e-mail/Mot de passe »** (laisser « Lien par e-mail » désactivé).

## 3. Créer le premier compte admin

L'accès admin se fait par **double-clic sur le logo** (invite identifiant / code).

1. **Authentication → Users → Ajouter un utilisateur**
   - E-mail : `admin@ams-simulateur-devis.web.app`
   - Mot de passe : un code fort (≥ 8 caractères) — ce sera le « Code admin »
   - Copier l'**UID** de la ligne créée.
2. **Firestore → Données → Démarrer une collection**
   - Collection : `admins`
   - Document : **coller l'UID** ci-dessus
   - Champ `label` (string) = `Admin AMS` → Enregistrer

➡️ Connexion admin = identifiant `ADMIN` + le code choisi.

## 4. Concessionnaires

À créer **depuis l'espace admin** (onglet « Concessionnaires » → « + Nouveau
concessionnaire »). Le panel gère la création du compte, le code, l'activation
et la suppression. Aucune manip console nécessaire.

## 5. Catalogue prix / prestations

Le catalogue par défaut est codé en dur dans `index.html` (tableau `services`).
Dans l'espace admin, onglet « Prestations & prix » → **« Enregistrer dans
Firestore »** publie le catalogue dans `config/services`, qui devient alors la
source (le tableau en dur reste un filet de sécurité).

## 6. Agences et grilles de prix locales

Chaque concessionnaire peut être rattaché à une agence (onglet « Agences » de
l'espace admin) pour pratiquer ses propres prix :

- La grille « Chartres » (onglet « Prestations & prix », sélecteur sur « grille
  par défaut ») reste le catalogue de base (`config/services`), utilisé par
  tout concessionnaire non rattaché à une agence.
- Créer une agence dans l'onglet « Agences » (collection `agencies`).
- Dans « Prestations & prix », choisir l'agence dans le sélecteur « Grille de
  prix » pour ne modifier QUE les prix propres à cette agence (le reste
  continue de suivre la grille par défaut). Stocké dans `agencyPrices/{id}`.
- Dans « Concessionnaires », relier chaque compte à son agence via la colonne
  « Agence » (champ `agencyId` sur la fiche `concessionnaires/{uid}`).

---

## Correspondance identifiant → e-mail (interne)

L'identifiant saisi est mis en minuscules, nettoyé, puis suffixé
`@ams-simulateur-devis.web.app` (domaine synthétique, ne reçoit aucun courrier).

```
AMS    → ams@ams-simulateur-devis.web.app
ADMIN  → admin@ams-simulateur-devis.web.app
```

Pas de « mot de passe oublié » : un code se réattribue depuis l'espace admin
(colonne « Code »), ou en supprimant / recréant le compte dans la console.

## Domaine autorisé (déploiement)

Après mise en ligne : **Authentication → Settings → Domaines autorisés** →
ajouter le domaine du site (ex. `mcaudyser.github.io`).
