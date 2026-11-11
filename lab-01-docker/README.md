# TP 1 — Construire et lancer une image Docker

**13 minutes · en binôme**

Objectif : afficher votre propre titre à l’adresse `http://127.0.0.1:8080`.

Exécutez les commandes depuis la racine du projet. L’un de vous saisit les commandes, l’autre lit la fiche et vérifie les résultats.

## 1. Personnaliser la page (3 min)

Ouvrez [index.html](index.html) dans votre éditeur. Repérez la ligne qui commence par `<h1>`. Remplacez le texte « Bienvenue dans le lab » par votre titre, par exemple « Bonjour du binôme 4 ». Enregistrez le fichier.

Ouvrez ensuite le [Dockerfile](Dockerfile) et repérez quelle ligne récupère la base Nginx et quelle ligne ajoute votre fichier.

## 2. Construire l’image (3 min)

```sh
docker build -t cours-web:v1 lab-01-docker
docker image ls cours-web
```

Cherchez une ligne contenant `cours-web` et le tag `v1`. Le chemin `lab-01-docker` désigne le dossier des fichiers utilisés par la construction.

## 3. Démarrer et vérifier le conteneur (5 min)

```sh
docker run -d --name cours-web -p 127.0.0.1:8080:80 cours-web:v1
docker ps --filter name=cours-web
```

1. Dans la colonne `PORTS`, repérez la liaison entre le port `8080` de votre ordinateur et le port `80` du conteneur.
2. Ouvrez [http://127.0.0.1:8080](http://127.0.0.1:8080). Montrez à votre binôme le titre que vous avez choisi.
3. Expliquez en une phrase la différence entre `cours-web:v1` et le conteneur `cours-web`.

## 4. Arrêter et retirer le conteneur (2 min)

```sh
docker stop cours-web
docker rm cours-web
docker image ls cours-web
```

L’image `cours-web:v1` doit encore apparaître. Gardez-la pour le TP 2.

## Si vous êtes bloqués

- **Dockerfile introuvable** — Revenez dans le dossier qui contient `lab-01-docker` puis relancez la construction.
- **Conteneur déjà nommé `cours-web`** — Affichez `docker ps -a --filter name=cours-web`. Si c’est le conteneur d’un essai précédent du TP, arrêtez-le et retirez-le avant de relancer.
- **Port 8080 déjà utilisé** — Vérifiez si l’essai a laissé un conteneur arrêté avec le même nom. Retirez ce conteneur du TP avec `docker rm cours-web`, puis reprenez `docker run` en remplaçant `8080:80` par `8082:80`. Ouvrez alors [http://127.0.0.1:8082](http://127.0.0.1:8082).
- **Le navigateur affiche l’ancien titre** — Vérifiez que le fichier a été enregistré. Arrêtez et retirez le conteneur, reconstruisez l’image, puis relancez le conteneur. Actualisez la page.

[Retour au sommaire](../README.md)
