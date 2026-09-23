# Inventaire des points d'entrée

Le livrable de l'atelier d'analyse. Il dit ce que l'API devra **permettre** — pas comment on
l'écrira.

Une règle : tout ce qui figure ici doit se justifier par la lettre de mission ou par un écran
des maquettes. Si vous ne savez plus d'où vient une ligne, c'est peut-être qu'elle n'en vient
pas. (Seule exception : la section bonus, si vous en ajoutez une.)

---

## Ce que l'utilisateur peut faire

Une action par ligne, en français. Pas encore de chemins.

- Consulter les villes desservies par le réseau
- Rechercher des lancers disponibles (par ville de départ, d'arrivée, date et nombre de voyageurs)
- Consulter le détail d'un lancer (franchise de masse, modèle de catapulte, consignes)
- Créer un dossier voyageur
- Se connecter à son dossier voyageur
- Consulter ses informations personnelles (profil)
- Consulter l'ensemble de ses billets
- Ajouter un voyage au panier
- Retirer un voyage du panier
- Payer / valider son panier avec un moyen de paiement pour obtenir ses billets

## Les points d'entrée

| Ce que ça fait | Chemin proposé | Qui peut l'appeler |
|---|---|---|
| Lister les villes desservies | `GET /cities` | Tout le monde |
| Rechercher des lancers | `GET /launches` | Tout le monde |
| Consulter le détail d'un lancer | `GET /launches/{id}` | Tout le monde |
| Créer un dossier voyageur | `POST /users` | Tout le monde |
| Se connecter (créer une session) | `POST /sessions` | Tout le monde |
| Consulter son profil | `GET /users/me` | Voyageur connecté |
| Voir son panier | `GET /cart` | Voyageur connecté |
| Ajouter un lancer au panier | `POST /cart/items` | Voyageur connecté |
| Retirer un lancer du panier | `DELETE /cart/items/{id}` | Voyageur connecté |
| Payer le panier (créer une commande) | `POST /orders` | Voyageur connecté |
| Lister ses billets | `GET /tickets` | Voyageur connecté |

## Les données qui circulent

Pour chaque point d'entrée : ce qu'il reçoit, ce qu'il renvoie. Nommez les données comme
l'Office les nomme — les traduire en identifiants techniques, c'est le travail de demain.

### Lister les villes desservies
- **Reçoit** : Rien
- **Renvoie** : La liste des villes du réseau.

### Rechercher des lancers
- **Reçoit** : Ville de départ, ville d'arrivée, date, nombre de voyageurs.
- **Renvoie** : La liste des lancers correspondants (horaire, durée du trajet, prix par place).

### Consulter le détail d'un lancer
- **Reçoit** : L'identifiant du lancer.
- **Renvoie** : Les détails du lancer, incluant la franchise de masse, le modèle de catapulte, et les éventuelles consignes d'embarquement.

### Créer un dossier voyageur
- **Reçoit** : Adresse électronique, mot de passe, prénom (facultatif), nom (facultatif).
- **Renvoie** : Le dossier créé ou une confirmation de création.

### Se connecter
- **Reçoit** : Identifiants (adresse électronique, mot de passe).
- **Renvoie** : Un jeton de connexion (token).

### Consulter son profil
- **Reçoit** : Rien (identifié par la session).
- **Renvoie** : Les informations du dossier (adresse électronique, prénom, nom, date d'ouverture du dossier).

### Voir son panier
- **Reçoit** : Rien (identifié par la session).
- **Renvoie** : Le contenu du panier (les lancers ajoutés avec leur identifiant d'article, nombre de places, sous-total) et le montant total à payer.

### Ajouter un lancer au panier
- **Reçoit** : L'identifiant du lancer, le nombre de places.
- **Renvoie** : L'état mis à jour du panier.

### Retirer un lancer du panier
- **Reçoit** : L'identifiant de l'article dans le panier.
- **Renvoie** : L'état mis à jour du panier.

### Payer le panier
- **Reçoit** : Le moyen de paiement choisi (carte bancaire ou bon de transport).
- **Renvoie** : La confirmation et les billets générés.

### Lister ses billets
- **Reçoit** : Rien (identifié par la session).
- **Renvoie** : La liste des billets émis (avec leur numéro de référence unique ONTB, trajet, date/horaire, prix unitaire, et date d'émission).

## Ce dont on n'est pas sûrs

Les questions que la lettre et les maquettes ne tranchent pas. Une question notée vaut mieux
qu'une réponse inventée.

- Une catapulte a-t-elle une capacité maximale (nombre de places disponibles par lancer) qui nous empêcherait d'ajouter au panier ?
- Le jeton de connexion a-t-il une durée de validité (expiration de la session) ?

## Bonus

Actions implicites (non mentionnées dans la lettre ni les maquettes, mais indispensables pour un service en ligne standard) :

- **Réinitialiser son mot de passe** (Mot de passe oublié) : `POST /passwords/reset`
- **Modifier ses informations personnelles** (Mettre à jour son profil) : `PUT /users/me` (ou `PATCH`)
- **Supprimer son dossier voyageur** (Droit à l'oubli / RGPD) : `DELETE /users/me`
- **Consulter les conditions générales de vente (CGV)** de l'Office : `GET /legal/terms`
