# Kubernetes Labs — TP 3.1 et TP 3.2

**Étudiant :** Firas Khdhir  
**Formation :** Master DevOps & Cloud — ISET Tozeur

## 1. Environnement de travail

- Système : Ubuntu Server
- Orchestration : Kubernetes
- Installation : kubeadm
- Master : 192.168.10.181
- Ingress Controller : Traefik
- Applications : Nginx et Podinfo

# TP 3.1 — Services Kubernetes

## Étape 1 : Vérifier le cluster

```bash
kubectl get nodes
kubectl get pods -A
```

## Étape 2 : Déployer un Pod Nginx

```bash
kubectl apply -f web.yaml
kubectl get pods
kubectl describe pod web
```

## Étape 3 : Déployer l’application Podinfo

```bash
kubectl apply -f podinfo.yaml
kubectl get deployments
kubectl get pods
```

## Étape 4 : Créer un service ClusterIP

```bash
kubectl apply -f podinfo-svc.yaml
kubectl get services
```

ClusterIP permet l’accès à l’application depuis le cluster Kubernetes.

## Étape 5 : Créer un service NodePort

```bash
kubectl apply -f podinfo-nodeport.yaml
kubectl get services
```

NodePort permet l’accès à l’application via un port des nœuds Kubernetes.

## Étape 6 : Créer un service LoadBalancer

```bash
kubectl apply -f podinfo-lb.yaml
kubectl get services
```

LoadBalancer permet d’exposer une application à travers un équilibreur de charge lorsqu’une implémentation adaptée est disponible.

## Étape 7 : Déployer plusieurs applications

```bash
kubectl apply -f apps.yaml
kubectl get deployments
kubectl get services
kubectl get pods
```

# TP 3.2 — Ingress avec Traefik

## Étape 1 : Vérifier le cluster

```bash
kubectl get nodes
kubectl get pods -A
```

## Étape 2 : Installer Traefik avec Helm

Traefik a été installé avec Helm dans le namespace kube-system.

```bash
helm list -n kube-system
kubectl get pods -n kube-system
kubectl get services -n kube-system
```

## Étape 3 : Vérifier les ports de Traefik

Le service Traefik expose HTTP sur le NodePort 30081 et HTTPS sur le NodePort 30444.

```bash
kubectl get svc -n kube-system
kubectl get ingressclass
```

## Étape 4 : Vérifier les applications et l’Ingress

```bash
kubectl get pods -n tp-k8s
kubectl get services -n tp-k8s
kubectl get ingress -n tp-k8s
kubectl describe ingress web-ingress -n tp-k8s
```

L’Ingress `web-ingress` dirige les requêtes vers les services selon le nom d’hôte.

- `web.k3s.local` : service web-svc
- `api.k3s.local` : service api-svc

## Étape 5 : Configurer le fichier hosts sous Windows

Ajouter la ligne suivante au fichier hosts de Windows :

```text
192.168.10.181 web.k3s.local api.k3s.local
```

Le fichier se trouve dans :

`C:\Windows\System32\drivers\etc\hosts`

## Étape 6 : Tester dans le navigateur

Site Nginx :

http://web.k3s.local:30081

Application Podinfo :

http://api.k3s.local:30081

Les deux applications ont été testées avec succès depuis Windows.

## 7. Captures d’écran

Les captures d’écran des travaux pratiques sont disponibles dans le dossier `screenshots/`.

- `cap1.png`
- `cap2.png`
- `cap3.png`
- `cap4.png`
- `cap5.png`
- `cap6.png`

## Conclusion

Ces travaux pratiques ont permis de découvrir les Services Kubernetes (ClusterIP, NodePort et LoadBalancer), de déployer des applications et de configurer Traefik comme contrôleur Ingress pour accéder aux applications par nom d’hôte.

