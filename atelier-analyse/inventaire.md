# Inventaire des points d'entrée

Le livrable de l'atelier d'analyse. Il dit ce que l'API devra **permettre** — pas comment on
l'écrira.

Une règle : tout ce qui figure ici doit se justifier par la lettre de mission ou par un écran
des maquettes. Si vous ne savez plus d'où vient une ligne, c'est peut-être qu'elle n'en vient
pas. (Seule exception : la section bonus, si vous en ajoutez une.)

---

## Ce que l'utilisateur peut faire

Une action par ligne, en français. Pas encore de chemins.

- Consulter les villes desservies par le réseau ;
- Rechercher un lancer selon les critères de voyage (départ, arrivée, date, nombre de personnes) ;
- Consulter le détail et les contraintes balistiques d'un lancer ;
- S'inscrire en créant un compte voyageur ;
- Se connecter à son compte ;
- Consulter ses informations personnelles ;
- Gérer son panier (consulter, ajouter un lancer, supprimer un trajet) ;
- Valider et régler son panier ;
- Consulter et afficher ses billets émis.

## Les points d'entrée


| Ce que ça fait | Chemin proposé | Verbe | Qui peut l'appeler | Statut / Remarque |
|---|---|---|---|---|
| Obtenir les villes desservies | `/cities` | `GET` | Tout le monde | Pour l'autocomplétion |
| Rechercher des lancers | `/launches` | `GET` | Tout le monde | Filtres: `from`, `to`, `date`, `passengers` |
| Consulter le détail d'un lancer | `/launches/{id}` | `GET` | Tout le monde | Inclut la franchise de masse |
| Créer un compte | `/users` | `POST` | Visiteur | Inscription |
| Connexion / Obtenir un token | `/auth/login` | `POST` | Visiteur | Authentification |
| Voir son profil | `/users/me` | `GET` | Voyageur connecté | Infos personnelles |
| Consulter son panier | `/cart` | `GET` | Voyageur connecté | 1 panier actif par utilisateur |
| Ajouter un lancer au panier | `/cart/items` | `POST` | Voyageur connecté |  |
| Supprimer un trajet du panier | `/cart/items/{itemId}` | `DELETE` | Voyageur connecté |  |
| Valider et payer le panier | `/cart/checkout` | `POST` | Voyageur connecté | Génère les billets et vide le panier |
| Voir la liste de ses billets | `/tickets` | `GET` | Voyageur connecté | Liste des billets acquis |
| Télécharger / Voir un billet | `/tickets/{id}` | `GET` | Voyageur connecté | Billet d'embarquement nominatif |


## Les données qui circulent

Pour chaque point d'entrée : ce qu'il reçoit, ce qu'il renvoie. Nommez les données comme
l'Office les nomme — les traduire en identifiants techniques, c'est le travail de demain.



## Les données qui circulent

### GET /cities
* **Reçoit :**
  * Paramètre optionnel (Query Parameter) : `q` (chaîne de caractères pour filtrer à la saisie, ex: `GET /cities?q=Pa`).
* **Renvoie :**
  * Liste des villes desservies :
    ```json
    [
      {
        "id": "PAR",
        "name": "Paris"
      },
      {
        "id": "DIJ",
        "name": "Dijon"
      }
    ]
    ```

---

### GET /launches
* **Reçoit (Query Parameters) :**
  * `from` (string) : Code ou nom de la ville de départ.
  * `to` (string) : Code ou nom de la ville d'arrivée.
  * `date` (string/date) : Date souhaitée (ex: `2026-10-15`).
  * `passengers` (integer) : Nombre de personnes à transporter.
* **Renvoie :**
  * Liste des lancers disponibles correspondant aux critères :
    ```json
    [
      {
        "id": "LANCER-802",
        "departureCity": "Paris",
        "arrivalCity": "Dijon",
        "departureTime": "2026-10-15T08:30:00Z",
        "durationMinutes": 12,
        "price": 45.00
      }
    ]
    ```

---

### GET /launches/{id}
* **Reçoit :**
  * Paramètre de chemin : `id` du lancer (ex: `LANCER-802`).
* **Renvoie :**
  * Détail complet d'un lancer avec les contraintes balistiques :
    ```json
    {
      "id": "LANCER-802",
      "departureCity": "Paris",
      "arrivalCity": "Dijon",
      "departureTime": "2026-10-15T08:30:00Z",
      "durationMinutes": 12,
      "price": 45.00,
      "maxLuggageWeightKg": 15.0,
      "catapultName": "Le Trébuchet II"
    }
    ```

---

### POST /users
* **Reçoit (Body JSON) :**
  * `email` (string)
  * `password` (string)
  * `firstName` (string)
  * `lastName` (string)
* **Renvoie :**
  * Profil de l'utilisateur créé (sans le mot de passe, statut `201 Created`) :
    ```json
    {
      "id": "USR-1042",
      "email": "voyageur@example.com",
      "firstName": "Jean",
      "lastName": "Dupont"
    }
    ```

---

### POST /auth/login
* **Reçoit (Body JSON) :**
  * `email` (string)
  * `password` (string)
* **Renvoie :**
  * Jeton de connexion pour sécuriser les requêtes suivantes :
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
    ```

---

### GET /users/me
* **Reçoit :**
  * En-tête HTTP : `Authorization: Bearer <token>`
* **Renvoie :**
  * Informations personnelles de l'utilisateur actuellement connecté :
    ```json
    {
      "id": "USR-1042",
      "email": "voyageur@example.com",
      "firstName": "Jean",
      "lastName": "Dupont"
    }
    ```

---

### GET /cart
* **Reçoit :**
  * En-tête HTTP : `Authorization: Bearer <token>`
* **Renvoie :**
  * Contenu du panier actif du voyageur :
    ```json
    {
      "items": [
        {
          "itemId": "ITEM-01",
          "launchId": "LANCER-802",
          "departureCity": "Paris",
          "arrivalCity": "Dijon",
          "departureTime": "2026-10-15T08:30:00Z",
          "passengersCount": 2,
          "unitPrice": 45.00,
          "totalPrice": 90.00
        }
      ],
      "totalAmount": 90.00
    }
    ```

---

### POST /cart/items
* **Reçoit (Body JSON) :**
  * `launchId` (string) : Identifiant du lancer sélectionné.
  * `passengersCount` (integer) : Nombre de passagers retenus.
* **Renvoie :**
  * État mis à jour du panier (statut `201 Created` ou `200 OK`).

---

### DELETE /cart/items/{itemId}
* **Reçoit :**
  * Paramètre de chemin : `itemId` à retirer du panier.
* **Renvoie :**
  * Réponse vide (statut `204 No Content`).

---

### POST /cart/checkout
* **Reçoit :**
  * En-tête HTTP : `Authorization: Bearer <token>` (Règlement fictif/simulé, pas de corps bancaire nécessaire).
* **Renvoie :**
  * Résultat de la validation avec les billets émis ($N$ billets pour $N$ personnes) :
    ```json
    {
      "orderId": "CMD-9921",
      "status": "PAID",
      "totalPaid": 90.00,
      "tickets": [
        {
          "ticketId": "TKT-802-1",
          "launchId": "LANCER-802",
          "passengerName": "Jean Dupont",
          "seatNumber": "A1"
        },
        {
          "ticketId": "TKT-802-2",
          "launchId": "LANCER-802",
          "passengerName": "Jean Dupont",
          "seatNumber": "A2"
        }
      ]
    }
    ```

---

### GET /tickets
* **Reçoit :**
  * En-tête HTTP : `Authorization: Bearer <token>`
* **Renvoie :**
  * Liste de tous les billets acquis par le voyageur :
    ```json
    [
      {
        "ticketId": "TKT-802-1",
        "launchId": "LANCER-802",
        "departureCity": "Paris",
        "arrivalCity": "Dijon",
        "departureTime": "2026-10-15T08:30:00Z",
        "passengerName": "Jean Dupont"
      }
    ]
    ```

---

### GET /tickets/{id}
* **Reçoit :**
  * Paramètre de chemin : `id` du billet à consulter/imprimer.
* **Renvoie :**
  * Détail complet du billet d'embarquement nominatif :
    ```json
    {
      "ticketId": "TKT-802-1",
      "launchId": "LANCER-802",
      "departureCity": "Paris",
      "arrival

## Ce dont on n'est pas sûrs

## Ce dont on n'est pas sûrs

Questions et points à clarifier avec l'Office National des Trajectoires Balistiques (ONTB) :

1. **Durée de rétention et expiration du panier** :
   Lorsqu'un voyageur ajoute un lancer à son panier, le siège est-il réservé/bloqué temporairement (avec un compte à rebours avant expiration), ou les places ne sont-elles vérifiées et déduites qu'au moment du règlement effectif ?

2. **Nomination des billets émis (Réservation multi-passagers)** :
   La lettre de mission précise que la V1 exclut le « choix nominatif des passagers ». Pour une réservation de $N$ personnes, les $N$ billets émis doivent-ils tous comporter le nom du titulaire du compte connecté, ou faut-il prévoir un libellé générique (ex: *Passager 1*, *Passager 2*) ?

3. **Format et restitution du billet** :
   L'API doit-elle uniquement retourner un objet JSON pour l'affichage dans les applications (mobile/web), ou le point d'entrée `GET /tickets/{id}` doit-il prévoir un format binaire direct (ex: génération d'un fichier PDF ou d'une image QR Code d'embarquement) ?

4. **Gestion de la masse réelle vs franchise balistique** :
   La franchise de masse est une contrainte balistique essentielle pour l'Office. L'API se contente-t-elle de transmettre la limite autorisée (`maxLuggageWeightKg`) à titre informatif, ou le client doit-il envoyer la masse réelle estimée du passager/bagage lors de l'ajout au panier ?

5. **Gestion du stock et disponibilité des lancers** :
   Que doit retourner la recherche `GET /launches` lorsqu'un lancer est complet ? Faut-il le masquer entièrement des résultats ou le retourner avec un statut `fullyBooked: true` pour information au voyageur ?
