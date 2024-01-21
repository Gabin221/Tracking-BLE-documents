# Fiches de tests unitaires et fiches de tests d'intégration

## Fiches de tests unitaires

Partie camion :

| Titre | Elément | Comment | Observation |
| :-------------: | :-------------: | :-------------: | :-------------: |
| 1 | Affichage de la liste des beacons présents autour de l'utilisateur | En allumant l'application et en cliquant sur le bouton "voir la liste des beacons" | Une fenêtre modale s'ouvre avec une liste de couples Major-Minors |
| 2 | Affichage du RSSI | En allumant l'application et en cliquant sur le bouton "voir la liste des beacons" | Une fenêtre modale s'ouvre avec une liste de RSSI |
| 3 | Affichage de la liste des immatriculations | En allant dans les paramètres et en cliquant sur "voir les immatriculations" | Une liste déroulante s'affiche et propose plusieurs immatriculations |
| 4 | Envoie des données au serveur | En lançant l'application | On observe dans la BDD de nouvelles lignes |
| 5 | Saisie de l'immatriculation | En allant dans les paramètres et en cliquant sur "voir les immatriculations" | On peut cliquer sur une des immatriculations | On voit que cette immatriculation est sélectionnée |
| 6 | Affichage de la température | En allant sur le dashboard | On constate qu'il y a une température qui s'affiche |
| 7 | Capter la position GPS | En lançant l'application | On observe dans la BDD que la position GPS est indiquée |

## Fiches de tests d'intégration

Partie camion :

| Titre | Elément | Comment | Observation |
| :-------------: | :-------------: | :-------------: | :-------------: |
| 1 | Affichage de la fenêtre modale de la liste des beacons présents | En allumant l'application et en cliquant sur le bouton "voir la liste des beacons" | Une fenêtre modale s'ouvre avec une liste de couples Major-Minors ainsi que le RSSI associé |
