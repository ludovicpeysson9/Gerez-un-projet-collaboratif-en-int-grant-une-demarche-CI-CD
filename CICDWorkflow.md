# Documentation CI/CD pour le projet Bobapp

## Étapes du Workflow

### Build du Backend
- **Environnement :** Ubuntu Latest avec JDK 11.
- **Étapes :**
  - Checkout du code source depuis le dépôt GitHub.
  - Configuration de Java (JDK 11) avec distribution AdoptOpenJDK.
  - Build du projet backend avec Maven sans exécuter les tests.
  - Exécution des tests unitaires backend et génération du rapport de couverture Jacoco (back/target/site/jacoco).
  - Upload de l'artefact du backend (contenu du dossier back/target).
  - Build et push de l'image Docker du backend sur Docker Hub avec un tag correspondant au SHA du commit.

### Build du Frontend
- **Dépendances :** Ce job démarre après la réussite du build du backend.
- **Environnement :** Ubuntu Latest avec Node.js 16.
- **Étapes :**
  - Checkout du code source depuis le dépôt GitHub.
  - Configuration de l'environnement Node.js (version 16).
  - Installation des dépendances Node.js définies dans package.json. 
  - Exécution des tests unitaires frontend via Karma avec Chrome Headless et génération du rapport de couverture (front/coverage).
  - Upload de l'artefact du frontend (contenu du dossier front/coverage).
  - Build et push de l'image Docker du frontend sur Docker Hub avec un tag correspondant au SHA du commit.

### Vérification de la Qualité avec SonarCloud
- **Dépendances :** Ce job s'exécute après la réussite des builds backend et frontend.
- **Environnement :** Ubuntu Latest avec JDK 11.
- **Étapes :**
  - Checkout du code source complet (fetch-depth à 0).
  - Téléchargement de l'artefact backend nécessaire à l'analyse (back/target).
  - Configuration de l'environnement Java (JDK 11) pour l'analyse SonarCloud.
  - Analyse SonarCloud exécutée sur le code complet.

### Upload des Rapports de Couverture
- **Dépendances :** Ce job démarre après la réussite des builds backend et frontend.
- **Environnement :** Ubuntu Latest.
- **Étapes :**
  - Téléchargement de l'artefact backend (back/target) et frontend (front/coverage). 
  - Upload des rapports de couverture backend et frontend en tant qu'artefacts sur GitHub :
      - Backend : Rapport Jacoco (back/target/site/jacoco).  
      - Frontend : Rapport Karma Coverage (front/coverage).

## KPIs Proposés

1. **Code Coverage :** La couverture de code doit être au minimum de 80% frontend et backend pour assurer une bonne qualité et maintenabilité du code.
2. **Nombre de bugs critiques détectés par SonarCloud :**
   - Moins de 5 bugs critiques autorisés pour garantir la fiabilité du code en production.
   - Vulnérabilités : Objectif zéro.
   - Dette technique : Inférieure à 5 jours pour une bonne maintenabilité.

## Analyse des Métriques et Retours Utilisateurs

- **Couverture Actuelle :**
  - Backend : XX%
  - Frontend : YY%
- **SonarCloud :**
  - Bugs : Z
  - Vulnérabilités : W
  - Dette Technique : V jours

## Configuration Supplémentaire

- **Environnement Angular :**
  - Les URL de l'API backend ont été ajustées dans les fichiers `environment.ts` et `environment.prod.ts` pour inclure le port du backend.
  
- **CORS Policy :**
  - Une configuration a été ajoutée pour désactiver la politique CORS, permettant des requêtes de différentes origines.
