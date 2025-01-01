
# Dépôt Master Pipeline pour l'Orchestration des Microservices

Ce dépôt contient le `Jenkinsfile` du pipeline d'orchestration principal qui gère le processus de build, de test et de déploiement des microservices de l'application. L'objectif de ce pipeline est de centraliser la gestion des versions, de déclencher les pipelines des microservices en fonction des changements détectés et de garantir une cohérence globale du système.

## Architecture et Flux d'Orchestration

Ce `master-pipeline` est déclenché par un webhook GitHub lors de modifications sur ce dépôt. Ce dépôt doit contenir les configurations partagées entre les différents services ou dépendances communs.

### Déclenchement

1.  **Webhook GitHub:** Lorsqu'un commit est poussé (ou qu'une merge request est faite) sur la branche principale de ce dépôt, un webhook est envoyé à Jenkins.
2.  **Déclenchement du `master-pipeline` :** Ce webhook déclenche l'exécution de ce `Jenkinsfile`.

### Étapes d'Orchestration

1.  **`Checkout Triggering Repo`:** Le pipeline clone le dépôt source qui a déclenché le webhook. Ceci permet de récupérer les changements effectués.

2.  **`Analyse Changes & Orchestrate Deployments`:** Cette étape effectue les tâches suivantes :

    *   **Analyse des modifications:** On utilise `git diff` pour analyser les modifications depuis le dernier commit.
    *   **Logique conditionnelle:** En fonction des changements (modifications dans `config/` ou `dependencies/`)  le pipeline va exécuter certaines actions.
    *  Si la modification concerne les fichiers dans `config/`, le pipeline va déclencher une reconstruction de tous les services de l'application.
    *  Si la modification concerne les fichiers dans `dependencies/`, le pipeline va lancer une reconstruction des services qui sont impactés par ces modifications.
    *  Si aucune des conditions ci-dessus n'est remplie, alors le pipeline s'arrête.
    *   **Déclenchement des pipelines de microservices:**  Le pipeline déclenche les builds des pipelines des microservices via `build job`, en passant un paramètre `BUILD_LIB_SUCCESS` à `true` pour informer les microservices.
    *  **Ou : Redémarrage des déploiements:** Le pipeline déclenche un redémarrage des deployments dans EKS via `kubectl rollout restart` pour assurer que la nouvelle configuration est appliquée sur le systeme.

## Configuration de Jenkins

### Prérequis

*   **Jenkins:** Une instance Jenkins fonctionnelle avec les plugins nécessaires :
    *   Pipeline
    *   Git
    *   Pipeline: Build Step
    *   Kubernetes (Si vous utilisez l'option B de déploiement)
*   **Credentials:**  Un credential AWS de type `AWS-CREDENTIALS` nommé `aws-credentials` si vous utilisez l'option B de déploiement.
*   **kubectl** : Si vous utilisez l'option B de déploiement, kubectl doit être disponible sur les workers jenkins.
*   **Dépôts microservices** : Chaque microservice doit avoir son propre dépôt git et son propre pipeline jenkins.

### Configuration du Pipeline

1.  **Webhook GitHub:** Configurez un webhook GitHub dans votre dépôt de référence, pointant vers l'URL de votre instance Jenkins, en utilisant `/github-webhook/`.
2.  **Déclencheur du `master-pipeline` :** Dans la configuration de ce pipeline Jenkins, configurez un déclencheur "GitHub hook trigger for GITScm polling".

### Configuration des Pipelines Microservices

1.  **Désactiver Webhooks:** Les pipelines des microservices ne doivent plus être déclenchés par les Webhooks Github.


## Licence

Ce projet est sous licence [Bank Of ASMAS].

    