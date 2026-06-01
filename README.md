# holbertonschool-system_engineering-devops

## Web Infrastructure Design

### Diagramme de la pile web (Web Stack)

```
          INTERNET
              |
         [DNS Server]
         Resout le nom de domaine (ex: www.foobar.com -> IP)
              |
       [Load Balancer]
       HAProxy - distribue le trafic entre les serveurs
       Algorithme: Round Robin
              |
    __________|__________
    |                   |
[Serveur Web 1]   [Serveur Web 2]
   Nginx               Nginx
Gere les requetes HTTP/HTTPS
Sert les fichiers statiques
    |                   |
[App Server 1]    [App Server 2]
 Gunicorn/PHP-FPM  Gunicorn/PHP-FPM
Traite la logique metier
Execute le code applicatif
    |                   |
    |___________________| 
              |
    [Base de donnees]
    MySQL (Primary + Replica)
    Stocke les donnees persistantes
```

---

### Explication de chaque composant

**DNS (Domain Name System)**
Traduit un nom de domaine lisible (www.foobar.com) en adresse IP. C'est l'annuaire d'Internet. Un enregistrement de type A pointe vers l'IP du serveur.

**Load Balancer (HAProxy)**
Distribue les requetes entrantes entre plusieurs serveurs pour eviter la surcharge. L'algorithme Round Robin envoie chaque requete au serveur suivant dans la liste, de maniere cyclique.

**Serveur Web (Nginx)**
Recoit les requetes HTTP/HTTPS, sert les fichiers statiques (HTML, CSS, images) et redirige les requetes dynamiques vers le serveur applicatif.

**Serveur Applicatif (Gunicorn / PHP-FPM)**
Execute le code de l'application (Python, PHP, etc.), traite la logique metier, interagit avec la base de donnees, et retourne une reponse dynamique.

**Base de donnees (MySQL)**
Stocke et gere les donnees de l'application. En mode Primary/Replica : le Primary accepte les ecritures, le Replica synchronise et accepte les lectures.

---

### Redondance systeme (System Redundancy)

La redondance consiste a dupliquer des composants critiques pour eliminer les points de defaillance uniques.

- Plusieurs serveurs web et applicatifs : si l'un tombe, les autres prennent le relais
- Replica de base de donnees : si le Primary echoue, le Replica peut prendre le role de Primary
- Load Balancer en Active-Active ou Active-Passive : evite que le load balancer lui-meme soit un SPOF
- Monitorer et alerter : Nagios, Datadog, ou autre outil de supervision

---

### Acronymes importants

**LAMP**
Linux - Apache - MySQL - PHP/Python/Perl
Pile logicielle classique pour les applications web dynamiques.
- Linux : systeme d'exploitation
- Apache : serveur web
- MySQL : base de donnees relationnelle
- PHP/Python/Perl : langage de programmation cote serveur

**SPOF (Single Point of Failure)**
Composant unique dont la defaillance entraine l'arret total du systeme.
Exemple : un seul serveur sans redondance est un SPOF. Solution : dupliquer chaque composant critique.

**QPS (Queries Per Second)**
Nombre de requetes traitees par seconde. Mesure la charge et la capacite d'un systeme.
Exemple : un site populaire peut recevoir des milliers de QPS, ce qui necessite un load balancer et plusieurs serveurs pour repartir la charge.

---

### Concepts supplementaires

**Firewall**
Filtre le trafic reseau entrant et sortant. Protege les serveurs des connexions non autorisees.

**HTTPS**
Protocole HTTP chiffre avec TLS/SSL. Garantit la confidentialite et l'integrite des donnees echangees entre le client et le serveur.

**Monitoring**
Surveille l'etat des serveurs (CPU, RAM, disque, QPS). Envoie des alertes en cas d'anomalie. Outils : Datadog, Nagios, Prometheus, Grafana.
