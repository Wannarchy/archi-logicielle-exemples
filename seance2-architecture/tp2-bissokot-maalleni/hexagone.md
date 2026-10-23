## Étape 1 · Comparer et analyser

| Porte | Résultat obtenu (code HTTP, message, total…) |
|---|---|
| Ligne de commande | Commande : `node seance2-architecture/hexagonal/cli.js C-42 CLAVIER:2 SOURIS:1`<br><br>AFFICHE : [e-mail simulé] Commande CMD-0001 confirmée pour C-42 : 117 €<br><br>Commande créée :<br><pre>{<br>  "id": "CMD-0001",<br>  "customerId": "C-42",<br>  "lines": [<br>    {"sku": "CLAVIER", "qty": 2, "unitPrice": 49},<br>    {"sku": "SOURIS", "qty": 1, "unitPrice": 19}<br>  ],<br>  "total": 117,<br>  "createdAt": "2026-10-22T14:13:54.630Z"<br>}</pre> |
| HTTP : commande valide | Requête : `POST http://localhost:3000/orders`<br><br>Envoie une commande avec 2 claviers et une souris.<br><br>AFFICHE :<br><pre>{<br>  "id": "CMD-0002",<br>  "customerId": "C-42",<br>  "lines": [<br>    {"sku": "CLAVIER", "qty": 2, "unitPrice": 49},<br>    {"sku": "SOURIS", "qty": 1, "unitPrice": 19}<br>  ],<br>  "total": 117,<br>  "createdAt": "2026-10-22T14:17:10.882Z"<br>}</pre> |
| HTTP : relecture de la commande | Requête : `GET http://localhost:3000/orders/CMD-0001`<br><br>Requête pour lire la première commande, nommée CMD-0001.<br><br>AFFICHE :<br><pre>{<br>  "id": "CMD-0002",<br>  "customerId": "C-42",<br>  "lines": [<br>    {"sku": "CLAVIER", "qty": 2, "unitPrice": 49},<br>    {"sku": "SOURIS", "qty": 1, "unitPrice": 19}<br>  ],<br>  "total": 117,<br>  "createdAt": "2026-10-22T14:17:10.882Z"<br>}</pre> |
| HTTP : produit inconnu | Requête : `POST http://localhost:3000/orders`<br><br>Requête retournant le statut 400, avec une réponse au format JSON.<br><br>AFFICHE :<br><pre>{<br>  "error": "Produit inconnu : TELE"<br>}</pre> |
| HTTP : commande vide | Requête : `POST http://localhost:3000/orders`<br><br>Requête retournant le statut 400 au format JSON lorsque l'information est absente.<br><br>AFFICHE :<br><pre>{<br>  "error": "Une commande doit contenir au moins une ligne"<br>}</pre> |
| Tests : combien ? serveur nécessaire ? | 3 tests. Pas de serveur nécessaire. |

### question 
ils s'agit d'appelle api, donné retourné en format json , ce qui change c'est le status et le type de requete

## Etape 2

flowchart LR

  %% ===== Adaptateurs primaires (pilotent) =====
  subgraph Primaires["Adaptateurs primaires (pilotent)"]
    HTTP[httpServer.js]
    CLI[cli.js]
    TESTS[PlaceOrder.test.js]
  end

  %% ===== Cœur =====
  subgraph Coeur["Cœur"]
    UC[PlaceOrder]
    D[Order]
  end

  %% ===== Adaptateurs secondaires (pilotés) =====
  subgraph Secondaires["Adaptateurs secondaires (pilotés)"]
    REPO[InMemoryOrderRepository]
    CAT[StaticCatalog]
    NOT[ConsoleNotifier]
  end

  %% ===== Liens =====
  HTTP --> UC
  CLI  --> UC
  TESTS --> UC

  UC --> D

  UC -- port OrderRepository --> REPO
  UC -- port Catalog         --> CAT
  UC -- port Notifier        --> NOT