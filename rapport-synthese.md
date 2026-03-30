# Rapport synthétique — Projet DevSecOps

## 1. Contexte et objectif

L’objectif de ce projet était de mettre en place un pipeline DevSecOps autour d’une petite application Flask conteneurisée.  
Le but était d’intégrer des contrôles de sécurité directement dans une chaîne CI/CD avec GitHub Actions, afin de détecter des vulnérabilités dans les dépendances, les images Docker et les fichiers de configuration.  
Le projet devait aussi inclure une règle de sécurité codifiée avec Conftest, ainsi qu’un rapport de synthèse sur les vulnérabilités observées et les recommandations.

## 2. Architecture du projet

Le projet repose sur une petite API Flask avec :
- un fichier `app.py` ;
- un fichier `requirements.txt` ;
- un `Dockerfile` ;
- un manifeste Kubernetes `k8s/deployment.yaml` ;
- une règle Rego `policy/kubernetes.rego` ;
- un pipeline GitHub Actions dans `.github/workflows/ci.yml`.

Le pipeline exécute plusieurs étapes automatisées :
- lint YAML avec `yamllint` ;
- contrôle de politique de sécurité avec `Conftest` ;
- scan filesystem avec `Trivy fs` ;
- build de l’image Docker ;
- scan de l’image avec `Trivy image`.

## 3. Outils utilisés

Les principaux outils utilisés dans ce projet sont :
- **GitHub Actions** pour le pipeline CI ;
- **Flask** pour l’application de démonstration ;
- **Docker** pour la conteneurisation ;
- **Kubernetes YAML** pour le déploiement ;
- **yamllint** pour la validation syntaxique des fichiers YAML ;
- **Conftest** pour la vérification de règles de sécurité ;
- **Trivy** pour le scan de vulnérabilités et de mauvaises configurations.

## 4. Résultats du premier scan

Lors des premiers scans, plusieurs problèmes ont été détectés.

### 4.1 Dépendances
Le scan Trivy a détecté une vulnérabilité sur la dépendance Flask utilisée dans `requirements.txt`.  
La version `Flask 3.0.3` présentait une vulnérabilité corrigée dans une version plus récente.

### 4.2 Dockerfile
Le `Dockerfile` initial présentait des mauvaises configurations :
- absence d’utilisateur non-root ;
- absence de `HEALTHCHECK`.

### 4.3 Kubernetes
Le manifeste Kubernetes présentait plusieurs faiblesses de configuration, notamment :
- manque de paramètres complets dans le `securityContext` ;
- absence de `allowPrivilegeEscalation: false` ;
- absence de `readOnlyRootFilesystem: true` ;
- absence de `capabilities.drop` ;
- absence de `seccompProfile` ;
- absence de limites et requêtes de ressources ;
- utilisation implicite du namespace par défaut.

### 4.4 Image Docker
Le scan de l’image Docker a également remonté un nombre important de vulnérabilités système dans l’image de base Debian.  
Cela montre qu’une partie des vulnérabilités ne vient pas directement du code applicatif, mais aussi de la base système utilisée pour construire l’image.

## 5. Mesures correctives mises en place

Plusieurs corrections ont été apportées au projet afin d’améliorer la posture de sécurité.

### 5.1 Correction du Dockerfile
Le `Dockerfile` a été durci en :
- ajoutant un utilisateur non-root ;
- ajoutant un `HEALTHCHECK` ;
- ajustant les permissions sur le répertoire applicatif.

### 5.2 Correction du manifeste Kubernetes
Le fichier `k8s/deployment.yaml` a été renforcé avec :
- `runAsNonRoot: true` ;
- `runAsUser` et `runAsGroup` ;
- `allowPrivilegeEscalation: false` ;
- `readOnlyRootFilesystem: true` ;
- `capabilities.drop: [ALL]` ;
- `seccompProfile: RuntimeDefault` ;
- des `resources.requests` et `resources.limits` ;
- un namespace dédié `devsecops`.

### 5.3 Mise à jour des dépendances
La dépendance Flask a été mise à jour de `3.0.3` vers `3.1.3` afin de corriger la vulnérabilité détectée dans le scan des dépendances.

## 6. Résultats après correction

Après les corrections :
- le scan `Trivy fs` ne remonte plus de vulnérabilité sur `requirements.txt` ;
- le `Dockerfile` ne présente plus de mauvaise configuration ;
- le manifeste Kubernetes ne présente plus de mauvaise configuration ;
- la règle Conftest passe correctement dans le pipeline.

Le pipeline donne donc un résultat beaucoup plus propre qu’au début du projet.  
Les vulnérabilités restantes concernent surtout l’image de base système et certains paquets présents dans l’environnement Python de l’image.

## 7. Analyse sécurité

Ce projet montre qu’une démarche DevSecOps ne consiste pas seulement à lancer des outils, mais à mettre en place une logique d’amélioration continue.  
Les premiers scans ont permis de détecter des défauts de configuration et une dépendance vulnérable.  
Les corrections apportées ont ensuite permis de réduire fortement les problèmes remontés par le pipeline.  
Cela illustre bien l’intérêt d’intégrer la sécurité le plus tôt possible dans le cycle de développement, avec des contrôles automatisés directement dans la CI.

Le projet montre aussi qu’il existe plusieurs niveaux de sécurité :
- la sécurité du code et des dépendances ;
- la sécurité du conteneur ;
- la sécurité du déploiement Kubernetes ;
- la sécurité de l’image de base.

Même si certaines vulnérabilités système restent présentes, le pipeline permet au moins de les rendre visibles et de guider les futures actions de remédiation.

## 8. Recommandations

Pour aller plus loin, plusieurs améliorations pourraient être envisagées :
- utiliser une image de base encore plus minimale ou mieux maintenue ;
- mettre en place un seuil bloquant sur certaines vulnérabilités critiques ;
- générer un SBOM dans le pipeline ;
- ajouter d’autres contrôles de sécurité comme le scan de secrets dédié ou du SAST ;
- stocker les résultats de scan sous forme d’artefacts ;
- compléter le déploiement Kubernetes avec d’autres règles de sécurité plus avancées.

## 9. Synthèse des résultats

Le projet montre une amélioration nette entre le premier état du pipeline et la version finale.

Avant correction :
- `requirements.txt` contenait 1 vulnérabilité détectée par Trivy ;
- le `Dockerfile` présentait 2 mauvaises configurations ;
- le manifeste `k8s/deployment.yaml` présentait 15 mauvaises configurations.

Après correction :
- `requirements.txt` ne remonte plus de vulnérabilité ;
- le `Dockerfile` ne remonte plus de mauvaise configuration ;
- le manifeste Kubernetes ne remonte plus de mauvaise configuration ;
- la policy Conftest passe correctement dans le pipeline.

Cette évolution montre que les outils intégrés dans le pipeline n’ont pas seulement servi à détecter des problèmes, mais aussi à guider leur correction de manière progressive.

## 10. Conclusion

Ce projet a permis de mettre en place un pipeline DevSecOps simple mais complet autour d’une application Flask.  
Les outils intégrés dans GitHub Actions ont permis d’automatiser le lint, le contrôle de politique, le scan des dépendances, le scan de configuration et le scan d’image.  
Les premiers résultats ont mis en évidence plusieurs faiblesses de sécurité, qui ont ensuite été corrigées progressivement.  
Le résultat final montre une nette amélioration de la posture de sécurité du projet, avec un pipeline capable de détecter, contrôler et accompagner la remédiation de plusieurs types de risques.
