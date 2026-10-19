# Lab 6.2 — Patterns multi-conteneurs Kubernetes

## Objectifs
- Utiliser les Init Containers.
- Diagnostiquer un Init Container en échec.
- Partager un volume entre conteneurs avec emptyDir.
- Vérifier le partage du réseau dans un Pod.
- Comprendre le pattern Ambassador avec Nginx.

## Fichiers YAML
- appli.yaml : application avec Init Container.
- backend.yaml : service backend.
- init-erreur.yaml : exemple d'échec d'initialisation.
- app-logs.yaml : application avec Sidecar de logs.
- reseau-partage.yaml : test du réseau partagé.
- ambassador.yaml : proxy Ambassador.

## 1. Init Container
```bash
kubectl apply -f appli.yaml -n tp-k8s
kubectl get pods -n tp-k8s
