# Lab 7.2 — Pod Security

## 1. Objectif
Renforcer la sécurité des Pods Kubernetes en appliquant des restrictions au niveau des conteneurs.

## 2. Concepts étudiés
- `runAsNonRoot` : exécuter le conteneur avec un utilisateur non privilégié.
- `allowPrivilegeEscalation: false` : empêcher l'élévation des privilèges.
- `readOnlyRootFilesystem: true` : rendre le système de fichiers racine en lecture seule.
- `capabilities.drop: ["ALL"]` : supprimer les capacités Linux du conteneur.
- `seccompProfile: RuntimeDefault` : appliquer le profil Seccomp par défaut du runtime.
- `emptyDir` : fournir un répertoire temporaire inscriptible lorsque nécessaire.

## 3. Fichiers YAML
Les manifestes se trouvent dans le dossier `yaml/` :
- `web-v1.yaml` : test de l'exécution non-root avec une image Nginx standard.
- `web-v2.yaml` : deuxième tentative de durcissement du conteneur.
- `web-durci.yaml` : configuration sécurisée fonctionnelle.
- `privilegie.yaml` : test d'un Pod privilégié.

## 4. Tests et observations
### Pod standard
Le conteneur Nginx standard fonctionne avec les paramètres par défaut, mais ne respecte pas toutes les restrictions de sécurité souhaitées.

### Exécution non-root
La configuration `web-v1.yaml` a échoué car l'image Nginx utilisée s'exécute avec l'utilisateur root par défaut.

### Système de fichiers en lecture seule
La configuration `web-v2.yaml` a rencontré un problème d'écriture dans `/tmp/proxy_temp`.

### Configuration durcie
Le Pod `web-durci` a démarré correctement avec l'image `nginx-unprivileged`. Le répertoire `/tmp` utilise un volume `emptyDir` inscriptible.

Les vérifications ont confirmé :
- utilisateur non-root (`uid=101`) ;
- capacités Linux supprimées (`CapEff=0`) ;
- élévation de privilèges désactivée (`NoNewPrivs=1`) ;
- Seccomp actif (`Seccomp=2`) ;
- système de fichiers racine en lecture seule ;
- réponse HTTP 200 sur le port 8080.

### Pod privilégié
Le manifeste `privilegie.yaml` a été refusé par la politique Pod Security appliquée au namespace lors du test.

## 5. Conclusion
Le durcissement d'un Pod nécessite de combiner plusieurs contrôles de sécurité. Il faut également adapter l'image et prévoir les répertoires temporaires nécessaires à l'application.
