# Exo 3

## Objectif
Déployer dans un cluster Kubernetes l'application Flask (python) dont le code source est situé dans le dossier src.  
Il s'agit d'une application web liée une base de données relationnelle.  
Nous utiliserons un serveur de base de données mariadb, version 10.  

Vous devrez:
- Ecrire le **Dockerfile** permettant de dockeriser l'application flask.
- Ecrire le **docker-compose.yaml** permettant de démarrer l'application avec deux services: un service web et un service mariadb.
- Publier l'image vers votre compte docker.
- Ecrire un **manifeste k8s exo3.yaml** permettant de déployer l'application en cluster.

## Ecrire le Dockerfile permettant de dockeriser l'application flask.
```bash
docker build . -t silvereberdal/silverek8s-exo3:1.0.0
docker run --rm --name exo3 -d -p 8000:5000 silvereberdal/silverek8s-exo3:1.0.0
# docker stop exo3
```

## Ecrire le docker-compose.yaml permettant de démarrer l'application avec deux services: un service web et un service mariadb.
```bash
docker-compose up -d
docker cp students.sql exo3-flask_mariadb_1:/tmp
docker exec -it exo3-flask_mariadb_1 bash
mysql -p < /tmp/students.sql
# docker-compose down
```

## Publier l'image vers votre compte docker.
```bash
# docker login
docker push silvereberdal/silverek8s-exo3:1.0.0
# docker logout
```

## Ecrire un manifeste k8s exo3.yaml permettant de déployer l'application en cluster.
```bash
kubectl apply -f .\exo3\
kubectl -n exo3 cp students.sql mariadb-pod:/tmp
kubectl -n exo3 exec -it po/mariadb-pod -- sh -c 'mysql < /tmp/students.sql -p'
```