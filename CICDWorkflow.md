# Documentation CI/CD pour le projet Bobapp

## Étapes du Workflow

### Build du Backend
- **Environnement :** Ubuntu Latest avec JDK 11.
- **Étapes :**
  - Checkout du code.
  - Configuration de l'environnement Java.
  - Construction du projet Maven sans exécuter les tests.
  - Exécution des tests et génération du rapport Jacoco.
  - Construction de l'image Docker pour le backend et push sur Docker Hub.

### Build du Frontend
- **Dépendances :** Dépend du build du backend.
- **Environnement :** Ubuntu Latest avec Node.js 16.
- **Étapes :**
  - Checkout du code.
  - Configuration de l'environnement Node.js.
  - Installation des dépendances Node.js.
  - Exécution des tests frontend et génération du rapport de couverture.
  - Construction de l'image Docker pour le frontend et push sur Docker Hub.

### Vérification de la Qualité avec SonarCloud
- **Dépendances :** Dépend des builds du backend et du frontend.
- **Étapes :**
  - Configuration de l'environnement Java.
  - Rebuild du backend pour l'analyse SonarCloud.
  - Exécution de l'analyse SonarCloud.

### Upload des Rapports de Couverture
- **Dépendances :** Dépend des builds du backend et du frontend.
- **Étapes :**
  - Upload du rapport de couverture du backend sur GitHub.
  - Upload du rapport de couverture du frontend sur GitHub.

## KPIs Proposés

1. **Code Coverage :** La couverture de code doit être au minimum de 80% pour assurer une bonne qualité et maintenabilité du code.
2. **Nombre de bugs critiques détectés par SonarCloud :** Moins de 5 bugs critiques autorisés pour garantir la fiabilité du code en production.

## Analyse des Métriques et Retours Utilisateurs

- **Couverture Actuelle :**
  - Backend : XX%
  - Frontend : YY%
- **SonarCloud :**
  - Bugs : Z
  - Vulnérabilités : W
  - Dette Technique : V jours

Les retours des utilisateurs indiquent une amélioration notable de la performance et de la stabilité depuis l'implémentation de CI/CD, mettant en lumière quelques fonctionnalités à améliorer pour la prochaine itération.

## Configuration Supplémentaire

- **Environnement Angular :**
  - Les URL de l'API backend ont été ajustées dans les fichiers `environment.ts` et `environment.prod.ts` pour inclure le port du backend.
  
- **CORS Policy :**
  - Une configuration a été ajoutée pour désactiver la politique CORS, permettant des requêtes de différentes origines.
