# Mon aide-mémoire curl

Une entrée par quête. Trois lignes chacune, écrites avec mes mots.

- **La situation** — ce que la quête demandait
- **La commande** — celle qui a validé, recopiée telle quelle
- **Le piège** — ce qui m'a coincé, ou rien si tout a coulé

---

## 1 — Day 1: Inventory Check

**La situation** : Faire une requête sur un site et noter un item de la réponse...

**La commande** :

```bash
  $User > curl http://localhost:8080/inventory | grep "B"
```

**Le piège** :
    Aucun pour l'instant : Youpi !
---

## 2 — Day 2: Adding Items

**La situation** :
 Revoir la liste des items et en rajouter deux

**La commande** :

```bash
 $User > curl -s http://localhost:8080/inventory
 #1
 $User > curl -s -X POST -H "Content-Type: application/json" -d '{"name" : "Butter"}' http://localhost:8080/inventory
 #2
  $User > curl -s -X POST -H "Content-Type: application/json" -d '{"name" : "Butter"}' http://localhost:8080/inventory
```

**Le piège** : Simple ici aussi... RAS !

---

## 3 — Day 3: Maintain and Update

**La situation** :
 Apporter de modifications sur l'inventaire pour remettre de l'ordre


**La commande** :

```bash
$User > curl -s -X GET http://localhost:8080/inventory
#1
$User > curl -s -X PATCH http://localhost:8080/inventory/1 -H "Content-Type: application/json" -d '{"name": "Organic Bananas"}' 
#2
$User > curl -s -v -X PUT http://localhost:8080/inventory/2 -H "Content-Type: application/json" -d '{"name": ".....", "price": 5.00}'
#-v c'est pour voir les étapes parcourues par le curl
#3
$User > curl -s -X DELETE http://localhost:8080/inventory/3
```

**Le piège** : J'ai oublié le /inventory dans le lien de l'URL... (un peu con non ? XD)

---

## 4 — The Elemental Search

**La situation** :
Faire des recherches sur une API Pokemon


**La commande** :

```bash
$User > curl -s -X GET "http://localhost:8080/pokemon/search?type=fire"
#1
$User > curl -s -X GET "http://localhost:8080/pokemon/search?type=water,type=grass" 
#2
$User > curl -s -X GET "http://localhost:8080/pokemon/search?type=electric&region=kanto"
#3
$User > curl -s -X GET "http://localhost:8080/pokemon/search?role=special%20attacker"
#4
$User > curl -s -X GET "http://localhost:8080/pokemon/search?sort=base_stat_desc"
#5
$User > curl -s -X GET "http://localhost:8080/pokemon/search?type=fire&type=grass&region=kanto&sort=base_stat_desc"
```

**Le piège** :
C'est juste de la merde, et c'est long, et c'est chiant... Boff le terminal pas ouf unh !
---

## 5 — Payslip Uploader

**La situation** :

**La commande** :

```bash

```

**Le piège** :

---

## 6 — Strict API Contracts

**La situation** :

**La commande** :

```bash

```

**Le piège** :

---

## 7 — The Manager's Secret

**La situation** :

**La commande** :

```bash

```

**Le piège** :

---

## 8 — The Galactic Relay

**La situation** :

**La commande** :

```bash

```

**Le piège** :
