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

## Analyse des Métriques 

- **Couverture Actuelle :**
  - Backend : 
      - Couverture globale (Instructions) : 48 %
      - Branches : 50 %
      - Lines manquées : 28 sur 52 (≈ 46 % de couverture de lignes)
      - Couverture par package :
        - com.openclassrooms.bobapp.model : 0 %
        - com.openclassrooms.bobapp.data : 49 %
        - com.openclassrooms.bobapp.service : 25 %
        - com.openclassrooms.bobapp.controller : 54 %
        - com.openclassrooms.bobapp (package racine) : 37 %
        - com.openclassrooms.bobapp.config : 100 %
          
  - Frontend : 
      - Statements (instructions) : 78.57 %
      - Branches : 100 %
      - Functions : 57.14 %
      - Lines : 84.61 %
        
- **Rapport SonarCloud Actuel :**
  - Bugs : 4
  - Vulnérabilités : 0
  - Dette Technique (Maintainability) : 71 code smells, estimées à 5 h 38 min  
  - Debt Ratio : 0 %  
  - Maintainability Rating : A

- ***Analyse du rapport SonarCloud :***
  - Malgré 71 code smells, la dette technique reste faible (5h38 min). La fiabilité est plus problématique avec 4 bugs ouverts. La sécurité n'a pour le moment pas de vulnéraibilité détectée.

- ***Retours utilisateurs :***
  - Bug d'envoi de suggestion : un utilisateur signale que le bouton "poster une suggestion" fait planter le navigateur.
  - Bug sur post de vidéo : signalé il y a deux semaines, toujours pas corrigé selon l'utilisateur.
  - Notifications manquantes : Un utilisateur dit ne plus recevoir les blagues depuis une semaine.
  - Abandon de la plateforme : un autre utilisateur indique avoir supprimé le site de ses favoris en raison de bugs répétés.

- ***Problèmes à résoudre en priorité :***
  - 1 Améliorer la couverure des tests Backend :
    - Cibler en particulier model (0%) et service (25%) qui peinent à être couverts
    - Renforcer les tests d'intégration pour reproduire et corriger les bugs signalés (post vidéo, suggestions).
      
  - 2 Augmenter la couverture des tests Frontend
    - Approcher l'objectif de 80% en statements, améliorer la couverture des fonctions (57.14%).
    - Vérifier particulièrement le composant de suggestion de blagues (puisque bug signalé).

  - 3 Corriger les bugs SonarCloud (Reliability)
    - Prioriser les 4 bugs détectés pour éviter d'impacter l'expérience utilisateur et la fiabilité globale de l'application.

  - 4 Répondre aux retours critiques des utilisateurs
    - Stabiliser la fonctionnalité d'envoi de suggestion (bug critique).
    - Vérifier la logique de notifications.

### Conclusion ###
  - Couverture : Les objectifs de 80 % ne sont pas encore atteints, surtout côté backend (~48 %).
  - Qualité SonarCloud : Note globale de maintenabilité (A) encourageante, mais 4 bugs à adresser rapidement.
  - Retours Utilisateurs : Les bugs critiques (post de vidéo, suggestions) et la fiabilité des notifications doivent être traités en priorité afin de restaurer la confiance et l’engagement utilisateur.

## Configuration Supplémentaire

- **Environnement Angular :**
  - Les URL de l'API backend ont été ajustées dans les fichiers `environment.ts` et `environment.prod.ts` pour inclure le port du backend.
  
- **CORS Policy :**
  - Une configuration a été ajoutée pour désactiver la politique CORS, permettant des requêtes de différentes origines.
