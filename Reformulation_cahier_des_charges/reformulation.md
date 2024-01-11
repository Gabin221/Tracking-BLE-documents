# Reformulation du cahier des charges

## Dans l'entrepôt

### Détection des beacons

Il y a deux sortes de matériels que nous ne voulons pas perdre dans l'entrepôt : les supports de charge et les outils. Afin d'éviter les risques de perte, une des solutions trouvées est la pose de balise <b>iBeacon</b> standard sur eux. Ainsi, les supports de charge ainsi que les outils émettent un signal Bluetooth. Ce signal est détecté par des <b>gateways BLE/WiFi</b>, il y en a une par zone de l'entrepôt.  
Les gateways lisent en continu les signaux présents autour d'elles, elles envoient au <b>tampon de données</b> la liste des beacons en format JSON à l'aide de requêtes HTTP POST.  
Les gateways sont configurées à l'aide de l'application <b>CheckBlue</b> déjà disponible sur le marché Androïd. La configuration nécessite juste l'adresse et le port du tampon de données.

### Tampon des données

Le tampon de données est réalisé à partir d'un ordinateur, ici ce sera un Raspberry Pi 4. Son objectif est de faire l'intermédiaire entre les gateways et le serveur. Il reçoit donc les données par les gateways via des requêtes HTTP POST puis les envoies au serveur également sous forme de requêtes HTTP POST. Etant donné que nous voulons identifier quelle gateway a identifié quels beacon, la trame de données reçue par le tampon contient au format JSON l'adresse MAC de la gateway qui a émise la trame suivi de la liste des beacons détectés.  
Le responsable de l'entrepôt a le choix dans la fréquence à laquelle le tampon envoie les données au serveur, en sachant que si le ampon envoie ses données au serveur, alors il vide sa mémoire automatiquement. Il peut donc choisir parmis trois modes différents : le <b>mode transparent</b>, le <b>mode programmé</b> et le <b>mode quotidien</b>.

+ Le mode transparent consiste à envoyer instantanément les données au serveur une fois réceptionnées par le tampon  
+ Le mode programmé consiste à configurer l'intervalle de temps entre chaque vidage des données par le tampon
+ Le mode quotidien consiste à programmer une heure et tous les jours à la même heure programmée le tampon envoie les données au serveur

Il est également possible de filtrer les données émises au serveur par les gateways. En effet, il est possible de paramétrer le tampon pour filtrer selon soit le <b>Major</b> du beacon soit selon le <b>RSSI (Received Signal Strength Indicator)</b>.

+ Le Major consiste à ne garder que les beacons utilisés sur les <b>actifs</b>
+ Le RSSI consiste à ne garder que les beacons proches de la gateway.

De plus, le tri des beacons envoyés au serveur se fait au niveau de la zone détectée pour un beacon donné. Il n'est pas utile de transmettre au serveur plusieurs fois la zone du dit beacon s'il n'a pas changé de zone. Par ailleurs, un beacon peut être détecté par plusieurs gateway, il faut donc dans ces cas là prendre conserver la gateway dont le RSSI est le plus fort, indiquant une plus grande proximité avec celle-ci.  

Les paramètres de configuration sont enregistrés dans un fichier texte. De plus, le stockage au niveau du tampon est libre, aucun exigence n'est imposée.

### Stockage sur le serveur

Le serveur est un serveur web et un serveur de base de données <b>MariaDB</b>. Les données stockées dans la base de données est consultable via une page web.

### Application mobile

L'application mobile permet de rechercher dans l'entrepôt un support de charge ou un outil, elle sert donc de balise pour détecter les signaux émis par les beacons environnants. L'employé peut donc rechercher un beacon en fonction de son couple <b>Major-Minor</b>. Il peut pour cela le saisir dans un champ de saisie dédié ou le sélectionner dans une liste déroulante. La liste est mise à jour grâce au serveur. La validation du choix du beacon permet donc d'afficher une distance relative au beacon à partir du téléphone.

## Transport par camion

Le camion étant rempli de supports de charges, il y a donc plusieurs beacons dans le chargement. Ceux-ci sont configurés en <b>iBeacon</b> et détectés par l'application du chauffeur. Il y a également un beacon configuré en <b>Eddystone</b> qui permet de mesurer la température de la soute. Cette température est également affichée par l'application.  
Les données du chargement sont transmises au serveur grâce à son adresse IP à saisir dans l'application. De plus, l'application doit transmettre au serveur l'immatriculation du véhicule ainsi que sa position GPS mesurée grâce au téléphone.

## Administration et supervision par le responsable de l'entrepôt

Le responsable de l'entrepôt peut faire plusieurs opérations grâce à des pages web :

+ Il gère la liste des gateways ;
+ Il associe les supports de charge et les outils à un beacon (couple Major-Minor) ;
+ pour chacune des zones de stockage, il visualise la liste des supports de charge et des outils ;
+ Il visualise les dates-heures d’entrée et de sortie des supports de charge ;
+ Il utilise une page de configuration pour filtrer les beacons selon le Major ou une plage de RSSI ;
+ Il identifie la zone de stockage d’un support de charge ou d’un outil précis ;
+ Il configure la fonction « tampon de données » :
    + mode de transmission
    + filtrage des beacons selon le Major et/ou une plage de RSSI

## Administration et supervision par le responsable logistique

Egalement grâce aux pages web, le responsable logistique peut réaliser les opérations suivantes :

+ Il gère la liste des camions ;
+ Il sélectionne un camion et visualise les données envoyée (supports de charge et température) ;
+ Il visualise sur une carte la situation géographique d’un camion ;
+ Il visualise sur une carte la situation géographique d’un support de charge ;
+ Il visualise la liste des supports de charge présents dans les camions ;
+ Il sélectionne un support de charge et identifie le camion qui le transporte.
