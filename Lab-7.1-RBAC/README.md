# Lab 7.1 — RBAC et ServiceAccounts

## 1. Objectif
Comprendre la gestion des identités et des autorisations dans Kubernetes avec les ServiceAccounts, les Roles et les RoleBindings.

## 2. Concepts étudiés
- ServiceAccount : identité utilisée par un processus dans un Pod.
- Role : définit les permissions dans un namespace.
- RoleBinding : associe un Role à un utilisateur ou à un ServiceAccount.
- Principe du moindre privilège : accorder uniquement les permissions nécessaires.

## 3. Fichiers YAML
Les manifestes se trouvent dans le dossier `yaml/` :
- `pod-reader.yaml` : permissions de lecture.
- `ops-role.yaml` : permissions opérationnelles.
- `viewer.yaml` : configuration de l'auditeur.
- `api-sa.yaml` : ServiceAccount et Pod sans montage automatique du token.

## 4. Vérifications
Commandes utilisées :

```bash
kubectl get serviceaccounts
kubectl get roles,rolebindings
kubectl auth can-i list pods --as=system:serviceaccount:default:dev-viewer
kubectl auth can-i delete pods --as=system:serviceaccount:default:dev-viewer
kubectl auth can-i get secrets --as=system:serviceaccount:default:auditeur
kubectl get pods
