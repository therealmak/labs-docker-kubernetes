# Préparation avant la séance

Faites cette préparation au moins la veille, sur l’ordinateur que vous utiliserez en classe. Prévoyez une connexion pour installer les outils et récupérer les premières images. Un seul poste prêt par binôme suffit.

## 1. Installer les outils

- **Windows** — Installez Docker Desktop avec le moteur WSL 2 et utilisez les conteneurs Linux. Installez Git pour Windows, puis `kubectl` et `kind` pour Windows. Lancez Docker Desktop et ouvrez un nouveau PowerShell.
- **macOS** — Installez Docker Desktop selon votre processeur (Apple Silicon ou Intel). Installez Git, `kubectl` et `kind` pour la même architecture. Lancez Docker Desktop et ouvrez Terminal.
- **Linux** — Installez Docker Engine pour votre distribution ou Docker Desktop. Démarrez le moteur Docker, puis installez Git, `kubectl` et `kind`. Utilisez un compte qui a déjà accès au moteur Docker ou demandez l’aide du responsable des postes.

Pour un binaire installé à la main, suivez les étapes de la documentation pour le rendre accessible dans le terminal (variable `PATH`). Avec Docker Desktop, le cluster sera créé par kind. L’option Kubernetes intégrée de Docker Desktop peut rester désactivée.

- [Docker Desktop](https://docs.docker.com/desktop/) · [Docker Engine](https://docs.docker.com/engine/install/)
- [Git](https://git-scm.com/downloads) · [kubectl](https://kubernetes.io/docs/tasks/tools/) · [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)

## 2. Vérifier les outils

```sh
git --version
docker version
kubectl --context kind-cours-k8s version --client
kind version
```

`docker version` doit afficher les sections `Client` et `Server`. Si vous voyez seulement `Client`, vérifiez que Docker est démarré et que votre compte a accès au moteur. Pour `kubectl`, l’option `--client` fonctionne avant la création du cluster.

## 3. Créer le cluster et mettre en cache l’image de base

```sh
docker pull nginx:alpine
kind create cluster --name cours-k8s --wait 5m
kubectl --context kind-cours-k8s get nodes
kubectl --context kind-cours-k8s version
```

L’état attendu est `Ready` sur le nœud `cours-k8s-control-plane`. La version mineure de `kubectl` doit être à une version mineure au plus de celle du serveur, par exemple un client `1.36` pour un serveur `1.35` ou `1.37`. Si vous avez déjà créé ce cluster, vérifiez les nœuds et reprenez à partir de cette étape. [Compatibilité de kubectl](https://kubernetes.io/docs/concepts/overview/kubectl/#version-compatibility)

Après le téléchargement du nœud et de l’image Nginx, le cluster local peut être utilisé sans Internet. Récupérez également les fichiers depuis le [README](README.md) avant la séance si le Wi-Fi de la salle est incertain.

## 4. Vérifier le poste avant le cours

- Gardez le cluster et l’image Nginx en place. Vérifiez à nouveau l’état `Ready` en arrivant.
- Gardez les ports locaux `8080` et `8081` libres. Si un port est déjà pris, les fiches proposent un autre numéro.
- Ouvrez un éditeur de texte et deux terminaux à la racine du projet. Un terminal garde la redirection Kubernetes active pendant que l’autre sert aux commandes.
- Si le moteur ou le cluster ne démarre pas, rejoignez un binôme équipé et prenez le clavier pendant l’un des TP.

Pour la démonstration, gardez une copie ZIP des fichiers et répétez les deux TP sur un poste de secours. Testez le chargement de l’image dans kind avant la séance. Pour repartir avec une page vierge le jour du cours, utilisez une copie propre des fichiers. Nettoyez les objets de l’essai en suivant les fiches et gardez le cluster actif.

## Import de secours si kind load échoue

Si `kind load docker-image` échoue en mentionnant un `content digest` introuvable, exportez une archive qui contient seulement l’image pour l’architecture du nœud. Relevez d’abord l’architecture avec cette commande.

```sh
kubectl --context kind-cours-k8s get nodes -o custom-columns=ARCH:.status.nodeInfo.architecture
```

L’exemple ci-dessous correspond à un nœud `amd64`. Pour un nœud `arm64`, remplacez `linux/amd64` par `linux/arm64`.

```sh
docker image save --platform linux/amd64 --output cours-web.tar cours-web:v1
kind load image-archive cours-web.tar --name cours-k8s
```

Reprenez le TP à la commande `kubectl apply`. Vous pouvez retirer le fichier local `cours-web.tar` une fois le chargement terminé. [Problèmes connus de kind](https://kind.sigs.k8s.io/docs/user/known-issues/#unable-to-kind-load-docker-images)
