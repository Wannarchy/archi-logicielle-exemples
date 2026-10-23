1.

Une commande contient id, customerId, lines[] chaque ligne : sku, qty, unitPrice, createdAt, et depuis le TP2 le statut (EN_ATTENTE / PAYEE / REFUSEE) ainsi qu'un éventuel motif. Le total n'est jamais stocké : il est calculé par le getter get total() dans Order.js

2.

unitPrice est figé au moment de la commande C'est essentiel  si le catalogue change demain comme une promo , l'historique reste fidèle à ce qui a été facturé.

3.
introuvable ou indefini , probleme lors du test je peux que spéculer, non pas dans la doc

4.
tout est perdu une fois redémarrage, les id sont fit localement

### Etape 4

npm run lint:archi              

> archi-logicielle-exemples@2.0.0 lint:archi
> node tools/check-architecture.js

✗ Règle violée : le domaine dépend de l'extérieur :
   seance2-architecture\hexagonal\domain\Order.js  ->  ../container.js