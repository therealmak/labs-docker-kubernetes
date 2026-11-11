# Docker et Kubernetes — travaux pratiques

Module de virtualisation · Niveau débutant · 1 heure

Ayoub MAKHLOUF · Anas BAHLOUL · Reda Es-salhi · Mohamed Labad

Vous allez personnaliser une page HTML, la lancer avec Docker, puis réutiliser la même image dans un cluster Kubernetes sur votre ordinateur. Au premier TP, la page répond sur le port `8080`. Au second, elle répond sur `8081` et tourne dans deux Pods.

| Quand | À faire |
| --- | --- |
| Avant la séance | [Installer et vérifier les outils](PREPARATION.md), puis récupérer les fichiers. |
| 13–26 min | [TP 1 — Construire et lancer l’image Docker](lab-01-docker/README.md). |
| 38–51 min | [TP 2 — Déployer la même image dans Kubernetes](lab-02-kubernetes/README.md). |

## Récupérer les fichiers

Sur la page GitHub du cours, ouvrez **Code → HTTPS** et copiez l’adresse du dépôt. Dans le terminal, placez-vous dans le dossier qui accueillera le projet.

```sh
git clone https://github.com/therealmak/labs-docker-kubernetes.git
cd labs-docker-kubernetes
git status
```

`git clone` crée le dossier au premier téléchargement. Si vous l’avez déjà, ouvrez un terminal dans ce dossier et vérifiez que vous n’avez pas de changements locaux avant d’intégrer les mises à jour.

```sh
git status
git pull --ff-only
```

Si `git status` montre votre travail en cours, gardez cette copie. Au besoin, clonez le dépôt dans un autre dossier en ajoutant `labs-docker-kubernetes-propre` à la fin de la commande `git clone`.

Sans Git, utilisez **Code → Download ZIP**, décompressez l’archive, puis ouvrez le dossier extrait. Les TP se lancent de la même manière. Pour mettre cette copie à jour, téléchargez un nouveau ZIP et extrayez-le dans un autre dossier pour garder vos modifications.

Les chemins utilisés dans les commandes sont relatifs à la racine du projet, c’est-à-dire au dossier contenant ce README et les deux dossiers `lab-01-docker` et `lab-02-kubernetes`. Sous Windows, PowerShell convient. Sous macOS et Linux, utilisez le terminal.

[Guide GitHub sur le clonage et la mise à jour](https://docs.github.com/fr/get-started/using-git/getting-changes-from-a-remote-repository)
