## Étape 4 · Comparer et analyser


| Critère | MVC | MVP | MVVM |
|---|---|---|---|
| Couplage entre la vue et la logique | la vue lit le modèle pour s'afficher et le contrôleur reçoit les actions de l'utilisateur. Vue et contrôleur restent souvent liés, car le contrôleur choisit la vue à afficher. | la vue est passive et ne connaît que le presenter, via une interface. Le presenter pilote la vue, et la vue ne connaît pas le modèle. |  la vue est reliée au ViewModel par un data binding . Le ViewModel ignore tout de la vue. |
| Ce qu'on peut tester sans navigateur | Le modèle et le contrôleur, avec une requête simulée.  | Presque toute la logique d'affichage  |  on vérifie ses propriétés et ses commandes sans créer de vue. Seul le binding lui-même reste à tester côté interface. |
| Où valider une saisie | Dans le contrôleur pour le format et les champs obligatoires, et dans le modèle pour les règles métier. | Dans le presenter , il reçoit la saisie de la vue, la valide, puis demande à la vue d'afficher l'erreur. | Dans le ViewModel le binding l'affiche automatiquement dans la vue. |
| Cas d'usage typique | Applications web côté serveur  | Interfaces à vue passive où les tests comptent beaucoup  | Interfaces riches avec binding bidirectionnel (WPF, Angular, Vue.js, SwiftUI). |
 

 #### Question Data

 Elle est stocker dans le projet quand la page est recharger tout est perdu , mettre en place un stockage local ou écriture sur un fichier dans le projet ,
add, toggle, remove on le droit de la modifier
