# Projet DevSecOps

Ce projet a pour objectif de mettre en place un pipeline DevSecOps avec GitHub Actions autour d'une petite application conteneurisée.

## Objectifs

- construire un pipeline CI/CD simple ;
- intégrer des contrôles de sécurité open source ;
- analyser les dépendances, l'image Docker et les fichiers de configuration ;
- ajouter une politique de sécurité simple avec Conftest.

## Structure prévue

- `app.py` : application Flask de démonstration
- `requirements.txt` : dépendances Python
- `Dockerfile` : conteneurisation de l'application
- `k8s/deployment.yaml` : manifeste Kubernetes
- `policy/kubernetes.rego` : règle Conftest
- `.github/workflows/ci.yml` : pipeline GitHub Actions
- `Roadmap.md` : feuille de route du projet

## Outils envisagés

- GitHub Actions
- Trivy
- yamllint
- Conftest
- Docker
- Kubernetes YAML

## Avancement

Le projet sera construit étape par étape avec validation à chaque phase.
