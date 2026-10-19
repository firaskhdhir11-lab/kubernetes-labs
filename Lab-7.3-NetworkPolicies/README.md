# Lab 7.3 — Network Policies Kubernetes

## 1. Objectif
Contrôler les communications réseau entre les Pods Kubernetes à l'aide des NetworkPolicies.

## 2. Architecture
L'application de test contient trois composants dans le namespace `tp-k8s` :
- Frontend : interface Web Nginx.
- API : service intermédiaire.
- DB : composant représentant la base de données.

Un Ingress Traefik permet d'accéder au frontend depuis l'extérieur du cluster.

## 3. Fichiers YAML
Les manifestes se trouvent dans `yaml/` :
- `app.yaml` : Deployments, Services et Ingress de l'application.
- `default-deny.yaml` : bloque par défaut le trafic entrant vers les Pods sélectionnés.
- `allow-traffic.yaml` : autorise Traefik vers le frontend, le frontend vers l'API et l'API vers la DB.
- `frontend-deny-egress.yaml` : bloque le trafic sortant du frontend.
- `frontend-allow-egress.yaml` : autorise le DNS et les connexions du frontend vers l'API.

## 4. Tests réalisés

### Communication initiale
Avant l'application des NetworkPolicies, les composants pouvaient communiquer entre eux.

### Blocage du trafic entrant
La politique `default-deny-ingress` a bloqué les connexions entrantes non explicitement autorisées.

### Autorisations ciblées
Les tests ont confirmé que :
- Traefik peut accéder au frontend.
- Le frontend peut accéder à l'API.
- L'API peut accéder à la DB.
- Le frontend ne peut pas accéder directement à la DB.

### Sélection des Pods et des namespaces
Une règle utilisant `namespaceSelector` et `podSelector` dans le même élément `from` exige que les deux sélecteurs correspondent. Cela permet de limiter l'accès aux Pods Traefik du namespace `kube-system`.

### Contrôle du trafic sortant
La politique de blocage sortant a interrompu la résolution DNS du frontend. Après l'ajout des autorisations DNS sur UDP/TCP 53 et de l'accès à l'API sur TCP 80 :
- le frontend a pu résoudre le nom du Service API ;
- la connexion frontend vers API a fonctionné ;
- l'accès direct frontend vers DB est resté bloqué.

## 5. Accès au frontend
L'accès externe a été testé avec l'Ingress Traefik via le NodePort HTTP 30081 et l'en-tête `Host: app.k3s.local`.

Exemple de test depuis Windows :

```bash
curl.exe -m 5 -H "Host: app.k3s.local" http://192.168.10.181:30081
