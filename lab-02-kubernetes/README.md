# TP 2 — Déployer l’image dans Kubernetes

**13 minutes · en binôme**

Objectif : retrouver la page du TP 1 à l’adresse `http://127.0.0.1:8081`, puis voir deux Pods prêts.

Inversez les rôles et ouvrez deux terminaux à la racine du projet. Utilisez l’image `cours-web:v1` construite au TP 1. Si votre binôme n’a pas terminé le TP 1, lancez simplement `docker build -t cours-web:v1 lab-01-docker` et utilisez la page telle quelle.

## 1. Charger l’image et déployer (4 min)

Dans le terminal A :

```sh
kubectl --context kind-cours-k8s get nodes
kind load docker-image cours-web:v1 --name cours-k8s
kubectl --context kind-cours-k8s apply -f lab-02-kubernetes/web.yaml
kubectl --context kind-cours-k8s rollout status deployment/cours-web --timeout=60s
kubectl --context kind-cours-k8s get pods -l app=cours-web
```

Vous devez voir un Pod `1/1` et `Running`. Dans [web.yaml](web.yaml), trouvez `replicas: 1` et le nom de l’image. Repérez le label `app: cours-web` qui permet au Service de trouver les Pods.

## 2. Afficher la page dans le navigateur (4 min)

Toujours dans le terminal A :

```sh
kubectl --context kind-cours-k8s port-forward svc/cours-web 8081:80
```

Laissez la commande active. Dans le navigateur, ouvrez [http://127.0.0.1:8081](http://127.0.0.1:8081). Vérifiez le titre de votre page et gardez ce terminal ouvert pour l’étape suivante.

## 3. Passer à deux réplicas (5 min)

Dans le terminal B :

```sh
kubectl --context kind-cours-k8s scale deployment/cours-web --replicas=2
kubectl --context kind-cours-k8s rollout status deployment/cours-web --timeout=60s
kubectl --context kind-cours-k8s get pods -l app=cours-web
```

Attendez deux lignes en `1/1 Running`. Si le deuxième Pod se lance encore, relisez la liste quelques secondes plus tard. Expliquez à votre binôme quel objet conserve le nombre de copies souhaité et quel objet sélectionne les Pods.

## Après le TP

Arrêtez la redirection dans le terminal A avec **Ctrl+C**. Dans le terminal B, retirez le Deployment et le Service de ce TP :

```sh
kubectl --context kind-cours-k8s delete -f lab-02-kubernetes/web.yaml --ignore-not-found
```

## Si vous êtes bloqués

- **Contexte introuvable ou connexion refusée** — Vérifiez que Docker est démarré. Tapez `kind get clusters` et cherchez `cours-k8s`. Si ce cluster manque, suivez la [préparation](../PREPARATION.md) ou rejoignez un binôme prêt.
- **`kind load` échoue avec `content digest … not found`** — Consultez la procédure d’[import depuis une archive](../PREPARATION.md#import-de-secours-si-kind-load-échoue).
- **ErrImagePull / ImagePullBackOff** — Vérifiez que `docker image ls cours-web` montre `v1`. Chargez l’image dans `cours-k8s`. Si le Pod reste bloqué après quelques secondes, exécutez `kubectl --context kind-cours-k8s rollout restart deployment/cours-web`, puis vérifiez à nouveau.
- **La redirection ne démarre pas** — Vérifiez d’abord qu’un Pod est prêt. Si le port `8081` est pris, remplacez `8081:80` par `8083:80` et ouvrez [http://127.0.0.1:8083](http://127.0.0.1:8083).
- **La redirection s’est arrêtée** — Vérifiez la liste des Pods. Relancez la commande `port-forward` quand un Pod est prêt.

[Retour au sommaire](../README.md)
