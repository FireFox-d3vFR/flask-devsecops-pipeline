# Flask DevSecOps Pipeline

Projet de démonstration d'un pipeline **DevSecOps** autour d'une petite API **Flask**, avec intégration de contrôles de sécurité dans un workflow **GitHub Actions**.

## Objectif

L'objectif de ce projet est de montrer comment sécuriser une chaîne CI/CD en intégrant automatiquement des outils de vérification et de scan dans le pipeline.

Le pipeline met en œuvre :
- le **lint** des fichiers YAML ;
- le contrôle de politique de sécurité avec **Conftest** ;
- le scan de dépendances, de configuration et d'image avec **Trivy** ;
- le **build** d'une image Docker ;
- l'analyse de vulnérabilités et de mauvaises configurations.

## Stack et outils

- **Flask** pour l'application de démonstration
- **Docker** pour la conteneurisation
- **Kubernetes YAML** pour le manifeste de déploiement
- **GitHub Actions** pour le pipeline CI
- **yamllint** pour le lint YAML
- **Conftest** pour la policy as code
- **Trivy** pour le scan des dépendances, des fichiers et de l'image Docker

## Structure du projet

```text
.
├── .github/
│   └── workflows/
│       └── ci.yml
├── k8s/
│   └── deployment.yaml
├── policy/
│   └── kubernetes.rego
├── app.py
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── .gitignore
├── .yamllint.yml
├── README.md
└── Roadmap.md
```

## Fonctionnement du pipeline

Le workflow GitHub Actions effectue les étapes suivantes :

1. récupération du dépôt ;
2. installation de Python ;
3. installation et exécution de `yamllint` ;
4. installation et exécution de `Conftest` ;
5. installation et exécution de `Trivy fs` ;
6. build de l'image Docker ;
7. exécution de `Trivy image`.

Le fichier principal du pipeline est :

```
.github/workflows/ci.yml
```

## Contrôles de sécurité mis en place

### 1. Lint YAML

Le manifeste Kubernetes est validé avec `yamllint`.

### 2. Policy as Code avec Conftest

Une règle Rego a été ajoutée pour refuser un déploiement Kubernetes si le conteneur n'est pas configuré pour tourner en non-root.

Fichier concerné :

```
policy/kubernetes.rego
```

### 3. Scan de sécurité avec Trivy

Trivy est utilisé pour :

- scanner les dépendances Python ;
- détecter les mauvaises configurations dans le `Dockerfile` ;
- détecter les mauvaises configurations dans le manifeste Kubernetes ;
- scanner l'image Docker construite dans le pipeline.

## Améliorations réalisées pendant le projet

Au cours du projet, plusieurs corrections de sécurité ont été apportées.

### Dockerfile

- ajout d'un utilisateur non-root ;
- ajout d'un `HEALTHCHECK` ;
- amélioration du comportement de l'image pour répondre aux recommandations de sécurité.

### Kubernetes

- ajout d'un `securityContext` renforcé ;
- activation de `runAsNonRoot` ;
- ajout de `runAsUser` et `runAsGroup` ;
- ajout de `allowPrivilegeEscalation: false` ;
- ajout de `readOnlyRootFilesystem: true` ;
- ajout du drop des capacités Linux ;
- ajout de `seccompProfile: RuntimeDefault` ;
- ajout de `resources.requests` et `resources.limits` ;
- utilisation d'un namespace dédié.

### Dépendances applicatives

- mise à jour de **Flask** pour corriger une vulnérabilité détectée lors des scans.

## Résultats obtenus

Après amélioration :

- le scan filesystem Trivy ne remonte plus de vulnérabilité sur `requirements.txt` ;
- le `Dockerfile` ne remonte plus de mauvaise configuration ;
- le manifeste Kubernetes ne remonte plus de mauvaise configuration ;
- la règle Conftest passe correctement dans le pipeline.

Les vulnérabilités restantes concernent principalement :

- l'image de base système ;
- certains paquets présents dans l'environnement Python de l'image.

Cela montre que la sécurité DevSecOps repose à la fois sur :

- la qualité de la configuration ;
- la mise à jour des dépendances applicatives ;
- le choix et la maintenance de l'image de base.

## Exécution locale

### Lancer l'application localement

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```

### Build Docker

```bash
docker build -t devsecops-demo:local .
```

### Lancer le conteneur

```bash
docker run --rm -p 5000:5000 devsecops-demo:local
```

### Tester l'API

```bash
curl http://localhost:5000/
curl http://localhost:5000/health
```

## À propos

Ce dépôt a été réalisé comme projet de mise en pratique DevSecOps avec une logique de progression étape par étape :

- construction d'une application simple ;
- intégration de contrôles CI ;
- détection des problèmes ;
- durcissement progressif ;
- validation finale dans GitHub Actions.