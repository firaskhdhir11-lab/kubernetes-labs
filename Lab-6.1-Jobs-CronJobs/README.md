# Lab 6.1 — Jobs et CronJobs Kubernetes

## Objectifs
- Créer et exécuter des Jobs Kubernetes.
- Diagnostiquer les échecs et tester OnFailure.
- Exécuter des tâches en parallèle.
- Planifier des tâches avec CronJob.
- Vérifier la persistance des sauvegardes.

## Fichiers YAML
- calcul.yaml : exécution d'un Job.
- echec.yaml : Job en échec.
- echec-onfailure.yaml : test OnFailure.
- succes-onfailure.yaml : test d'un Job réussi.
- parallele.yaml : exécution parallèle.
- lent.yaml : tâche de longue durée.
- sauvegarde.yaml : CronJob de sauvegarde.
- sauvegarde-vol.yaml : sauvegarde avec volume persistant.
- lecteur.yaml : lecture des sauvegardes.

## Vérification
```bash
kubectl get jobs -n tp-k8s
kubectl get cronjobs -n tp-k8s
kubectl get pods -n tp-k8s
kubectl logs job/calcul -n tp-k8s
kubectl describe job echec -n tp-k8s
