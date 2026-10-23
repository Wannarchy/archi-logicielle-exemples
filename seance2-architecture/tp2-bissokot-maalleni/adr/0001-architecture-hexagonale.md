# ADR-0001 : Isoler le stockage des commandes derrière le port OrderRepository

- Date : 2026-10-21
- Statut : Accepté
- Décideurs : binôme Cyber (bissokot, maalleni)

## Contexte

Le stockage est en mémoire : les commandes sont perdues au redémarrage, et
l'historique nécessaire aux statistiques disparaît. La mise en production
exigera PostgreSQL sans réécrire le domaine ni les tests. Risque actuel :
qu'un accès SQL ou ORM s'infiltre dans domain ou application.

## Décision

Tout accès au stockage passe par un port sortant OrderRepository
(save, findById) défini par le cœur. Les implémentations
(InMemoryOrderRepository, PostgresOrderRepository) sont des adaptateurs
secondaires câblés uniquement dans container.js. La règle est vérifiée en
CI par npm run lint:archi.

## Conséquences

- (+) Tests du cas d'utilisation en mémoire, sans base, en moins de 2 s.
- (+) Passage mémoire → PostgreSQL : un fichier + une ligne de câblage,
  domai et application inchangés.
- (+) Historique reproductible : unitPrice figé par ligne, id et createdAt
  fournis par des ports.
- (−) Mapping explicite Order  tables (orders, order_lines) à maintenir.
- (−) Port implicite en JS : à documenter (JSDoc) et à couvrir par des tests.
- (−) Indirection supplémentaire pour un nouvel arrivant.

## Alternatives considérées

- Accès direct à la base dans application (pg ou ORM) : non car les
  tests exigeraient une base réelle et le changement de stockage
  contaminerait le cœur.
- Modèle Mongoose dans domain (Active Record) : non car elle viole la
  règle de dépendance et ferait échouer npm run lint:archi.