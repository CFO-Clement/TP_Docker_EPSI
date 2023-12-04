---
title: TP Docker,docker-compose,k8s
author: Tom Avenel
abstract: Le but de ce TP est d'isoler et de déployer une application dans une stack de conteneurs Docker.
---

# Présentation de l'application

L'application est composée de 4 composants : `vote`, `result`, `worker` et `proxy`.

- `vote` est un composant hébergeant une UI permettant de voter
- `result` est un composant hébergeant une UI affichant les résultats de vote
- `worker` est un composant gérant le backend des votes
- `proxy` est un serveur Nginx servant de reverse-proxy vers le front des resultats.

Une fois déployée, l'application expose :

- une UI pour les votes (conteneur `vote`) accessible depuis le reverse-proxy : <http://vote.votes.fr/>
- une UI pour afficher les résultats (conteneur `result`) accessible depuis le reverse-proxy : <http://results.votes.fr/>


**Attention : le reverse-proxy a besoin des entrées DNS suivantes pour fonctionner à ajouter dans le fichier hosts : /etc/hosts sous Linux, /private/etc/hosts sous MacOS, C:\Windows\System32\drivers\etc\hosts sous Windows.**

```
127.0.0.1 vote.votes.fr
127.0.0.1 results.votes.fr
```

# Spécifications de déploiement

L'application totale sera déployée dans une stack composée de 5 services :

- un service `proxy` (reverse-proxy Nginx) buildé depuis `./proxy` :
  + Lié au port `80` de l'hôte
  + Utilise le réseau du frontend
- un service `vote` buildé depuis `./vote` :
  + Le web serveur tourne sur le port `80` dans le conteneur.
  + Le conteneur doit s'appeler `vote`.
- un service `result` buildé depuis `./result` (tourne sur le port `80`).
  + Le web serveur tourne sur le port `80` dans le conteneur.
  + Le port `5858` doit être exposé sur le même port de l'hôte.
  + Le conteneur doit s'appeler `results`.
- un service `worker` buildé depuis `./worker`
- un message broker nommé `redis` utilisant l’image `redis:alpine` pour gérer les interactions entre les composants : ce conteneur organise les échanges de messages entre tous les autres composants de l'application.
  + Le port `6379` doit être exposé sur le même port de l'hôte.
- une base de données nommée `db` utilisant l’image `postgres:9.4` pour persister les votes
  + La base de données stocke les données dans le répertoire `/var/lib/postgresql/data`.
  + La base de données requiert deux variables d’environment au démarrage: `POSTGRES_USER="postgres"` et `POSTGRES_PASSWORD="postgres"`.
- Les sources des applications `vote` et `result` doivent être accessibles dans le répertoire `/app` dans le conteneur.
- Si les sources sont modifiées dans le dépôt de code, on ne créera pas de nouvelle image du conteneur (accès depuis l'extérieur).
- Seuls les services `vote` et `result` sont des services _métier_ :
  + Les autres conteneurs n'ont donc pas à être lancés séparéments mais doivent démarrer automatiquement si l'un des deux services métier démarre.
  + Seuls les services métier et le reverse-proxy doivent être accessibles depuis l'hôte.
- Les conteneurs `redis` et `db` doivent être accessibles à tous les autres conteneurs.

# Legal

- © 2023 Tom Avenel under CC BY-SA 4.0
