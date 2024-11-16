# Déploiement de WordPress et MySQL sur Kubernetes

Ce projet met en œuvre le déploiement de WordPress et d'une base de données MySQL sur Kubernetes en utilisant des manifests YAML. L'objectif est de configurer une architecture simple et modulaire tout en garantissant la persistance des données.

## Structure du Projet

- **ConfigMap** : Stocke les variables d'environnement pour WordPress et MySQL.
- **PersistentVolumeClaims (PVC)** : Assure la persistance des données pour WordPress (`/data`) et MySQL (`/db-data`).
- **Deployments** :
  - MySQL : Déployé avec un seul réplica.
  - WordPress : Connecté à MySQL via des variables d'environnement.
- **Services** :
  - MySQL : Service de type `ClusterIP` pour la communication interne.
  - WordPress : Service de type `NodePort` pour l'accès externe.

## Fichiers YAML

- `configmap.yaml` : Contient les variables d'environnement nécessaires pour WordPress et MySQL.
- `pvc.yaml` : Définit les volumes persistants pour MySQL et WordPress.
- `mysql-deployment.yaml` : Déploiement de MySQL avec le volume monté sur `/db-data`.
- `mysql-service.yaml` : Service `ClusterIP` pour MySQL.
- `wordpress-deployment.yaml` : Déploiement de WordPress avec le volume monté sur `/data`.
- `wordpress-service.yaml` : Service `NodePort` pour exposer WordPress.

## Étapes de Déploiement

1. Clonez ce dépôt :

```bash
git clone <lien-du-dépôt>
cd <nom-du-dépôt>
```

2. Démarrez Minikube (si ce n'est pas encore fait) :

```sh
minikube start
```

3. Appliquez les manifests YAML :

```sh
kubectl apply -f .
```

4. Vérifiez les déploiements et les services :

```sh
kubectl get pods
kubectl get services
```

5. Accédez à Wordpress:

- Obtenez l'IP de Minikube:
  `sh
      minikube ip
      `
- Combinez cette IP avec le NodePort du service WordPress (par exemple, `http://<minikube-ip>:30080`).

## Fonctionnalités

- **Persistance des données** : Les données de MySQL et WordPress sont sauvegardées via des volumes montés.
- **Modularité** : Chaque composant (base de données et application) est séparé, facilitant les modifications et les mises à jour.
- **Accessibilité** : WordPress est accessible via un service `NodePort`.

## Problèmes Rencontrés et Résolutions

- **Changement de port dynamique sur WSL** : Résolu en configurant un reverse proxy avec NGINX pour stabiliser l'accès.
- **Échec du redémarrage de NGINX** : Résolu en corrigeant les erreurs de configuration dans le fichier `nginx.conf`.

## Pré-requis

- Kubernetes (Minikube recommandé)
- kubectl
- Docker
- NGINX (optionnel, utilisé comme reverse proxy)
