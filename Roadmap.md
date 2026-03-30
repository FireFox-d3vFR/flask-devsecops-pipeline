# Roadmap projet DevSecOps

## 1. Objectif du projet

Mettre en place un pipeline DevSecOps avec GitHub Actions afin de sécuriser une petite application conteneurisée.

Le projet doit permettre de :
- comprendre le rôle de la sécurité dans un pipeline CI/CD ;
- intégrer des outils open source de sécurité ;
- mettre en place des règles de sécurité codifiées dans le pipeline ;
- identifier des vulnérabilités dans les dépendances, les conteneurs et les configurations.

---

## 2. Livrables attendus

À la fin du projet, nous devrons fournir :
- le lien vers le dépôt GitHub avec GitHub Actions activé ;
- le pipeline YAML : `.github/workflows/ci.yml` ;
- les résultats des scans Trivy dans les logs du pipeline ;
- un exemple de règle Conftest ;
- un rapport synthétique de 1 à 2 pages avec :
  - les vulnérabilités observées ;
  - les recommandations ;
  - une réflexion sécurité.

---

## 3. Stratégie choisie

Nous allons construire un projet simple et démonstratif basé sur :
- une petite API Flask ;
- un `Dockerfile` ;
- un manifeste Kubernetes YAML ;
- un pipeline GitHub Actions ;
- des scans de sécurité avec Trivy ;
- une politique de sécurité simple avec Conftest.

L'objectif est d'avoir un projet :
- simple à comprendre ;
- rapide à mettre en place ;
- cohérent avec les attendus du sujet ;
- facile à documenter dans le rendu final.

---

## 4. Plan de travail

### Étape 1 — Préparer la structure du projet
Objectif :
- définir l'arborescence du dépôt ;
- choisir les fichiers nécessaires ;
- préparer une base propre pour la suite.

À produire :
- structure du repo ;
- liste des fichiers à créer.

Statut :
- [ ] À faire

---

### Étape 2 — Créer l'application de démonstration
Objectif :
- créer une petite API Flask fonctionnelle ;
- ajouter les dépendances nécessaires ;
- vérifier que le projet peut se lancer localement.

À produire :
- `app.py`
- `requirements.txt`

Statut :
- [ ] À faire

---

### Étape 3 — Ajouter la conteneurisation
Objectif :
- créer un `Dockerfile` pour builder l'application ;
- préparer un `.dockerignore` si nécessaire ;
- vérifier que l'image Docker se construit correctement.

À produire :
- `Dockerfile`
- `.dockerignore`

Statut :
- [ ] À faire

---

### Étape 4 — Ajouter le déploiement Kubernetes
Objectif :
- créer un manifeste Kubernetes simple ;
- préparer un fichier YAML qui pourra être linté et contrôlé ;
- prévoir un cas compatible avec la règle Conftest.

À produire :
- `k8s/deployment.yaml`

Statut :
- [ ] À faire

---

### Étape 5 — Mettre en place les contrôles de qualité
Objectif :
- ajouter le lint YAML ;
- préparer le projet pour les vérifications automatiques dans le pipeline.

À produire :
- configuration éventuelle de lint YAML ;
- validation des fichiers YAML.

Statut :
- [ ] À faire

---

### Étape 6 — Intégrer les scans de sécurité
Objectif :
- scanner les dépendances avec Trivy ;
- scanner le système de fichiers du projet ;
- scanner l'image Docker produite.

À produire :
- intégration Trivy dans le pipeline ;
- résultats exploitables dans les logs GitHub Actions.

Statut :
- [ ] À faire

---

### Étape 7 — Ajouter une politique de sécurité avec Conftest
Objectif :
- écrire une règle simple pour refuser un pod Kubernetes exécuté en root ;
- intégrer cette règle dans le pipeline CI/CD.

À produire :
- `policy/kubernetes.rego`
- étape de vérification Conftest dans le workflow

Statut :
- [ ] À faire

---

### Étape 8 — Créer le pipeline GitHub Actions
Objectif :
- automatiser le build ;
- automatiser le lint YAML ;
- automatiser les scans Trivy ;
- automatiser la vérification Conftest.

À produire :
- `.github/workflows/ci.yml`

Statut :
- [ ] À faire

---

### Étape 9 — Tester et corriger
Objectif :
- exécuter le pipeline ;
- corriger les erreurs bloquantes ;
- vérifier que tous les livrables sont présents.

À produire :
- logs de pipeline ;
- version stabilisée du projet.

Statut :
- [ ] À faire

---

### Étape 10 — Préparer le rendu final
Objectif :
- rédiger un rapport synthétique ;
- résumer les vulnérabilités détectées ;
- proposer des recommandations ;
- expliquer la logique de sécurité mise en place.

À produire :
- rapport 1 à 2 pages ;
- captures ou extraits utiles ;
- checklist de rendu.

Statut :
- [ ] À faire

---

## 5. Arborescence cible envisagée

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
├── README.md
└── Roadmap.md
```

## 6. Checklist de validation finale
- [ ] Le dépôt GitHub est crée
- [ ] Le dépôt GitHub est créé
- [ ] GitHub Actions est activé
- [ ] L'application Flask fonctionne
- [ ] L'image Docker se build
- [ ] Le YAML Kubernetes est présent
- [ ] Le lint YAML fonctionne
- [ ] Trivy scanne les dépendances
- [ ] Trivy scanne l'image Docker
- [ ] La règle Conftest est présente
- [ ] Le workflow GitHub Actions fonctionne
- [ ] Les logs de scan sont exploitables
- [ ] Le rapport final est rédigé

## 7. Méthode de travail

Nous avançons étape par étape.

Chaque fichier sera généré de manière contrôlée.
Les créations de fichiers se feront avec des commandes de type :

```bash
cat > fichier <<'EOF'
...
EOF
```
L'objectif est de garder un suivi clair, de valider chaque étape, puis de construire progressivement le projet complet.