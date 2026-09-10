# Simulateur de devis AMS

Application web mono-page (un seul `index.html`) — générateur de devis pour les
concessionnaires partenaires d'Auto Multi Services.

## Pile

- HTML/CSS/JS statique, hébergé sur **GitHub Pages**.
- **Firebase** (projet `ams-simulateur-devis`, région `eur3`) :
  - **Auth** e-mail/mot de passe — connexion concessionnaire par identifiant + code.
  - **Firestore** — catalogue prix/prestations (`config/services`), fiches
    concessionnaires (`concessionnaires/{uid}`), admins (`admins/{uid}`).

## Espace admin

Double-clic sur le logo → connexion admin. Deux onglets :
- **Prestations & prix** : CRUD du catalogue, écrit dans `config/services`.
- **Concessionnaires** : création / édition / suppression des accès.

## Fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | toute l'application (modifié en place) |
| `firestore.rules` | règles de sécurité Firestore (à publier dans la console) |
| `SETUP-FIREBASE.md` | procédure d'initialisation Firebase (une fois) |

## Déploiement

Push sur `main` → GitHub Pages sert `index.html` automatiquement.
