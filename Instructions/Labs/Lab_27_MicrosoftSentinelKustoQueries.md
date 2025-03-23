---
lab:
  title: "27\_: Requêtes Kusto Microsoft Sentinel pour les sources de données Microsoft Entra"
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 27 (facultatif) : requêtes Kusto Microsoft Sentinel pour les sources de données Microsoft Entra

**Note** : ce labo ne peut pas être réalisé pour l’instant dans l’environnement de labo de formation fourni.  Nous laissons l’étape du labo ici. Vous pouvez donc éventuellement l’essayer sur votre environnement BYOS (Bring You Own Subscription).  Lisez les étapes pour voir ce qui est possible.  Nous travaillons activement à mettre à jour ce labo pour trouver une solution dans l’environnement de labo, et nous le mettrons à jour bientôt.

### Type de connexion = Connexion à la ressource Azure

## Scénario du labo

Microsoft Sentinel est la solution SIEM et SOAR native Cloud de Microsoft.  Grâce à la connexion de sources de données à partir de solutions de sécurité Microsoft et tierces, vous avez la possibilité d’exécuter des tâches d’opérations de sécurité.  Dans cet exercice de labo, vous allez créer un espace de travail Microsoft Sentinel avec des connecteurs de données vers Microsoft Entra ID pour exécuter des requête de repérage en utilisant Langage de requête Kusto (KQL). 

#### Durée estimée : 30 minutes

### Exercice 1 : Configurer Microsoft Sentinel pour les requêtes Kusto

#### Tâche 1 : créer un espace de travail Microsoft Sentinel

1. Connectez-vous à la plateforme  [https://portal.azure.com](https://portal.azure.com) en tant qu’administrateur global.

1. Recherchez et sélectionnez **Microsoft Sentinel**. 

1. Sélectionnez **+ Créer**, en haut à gauche de la page.

1. Dans la vignette **Ajouter Microsoft Sentinel à un espace de travail**, sélectionnez **+ Créer un espace de travail**.

1. Dans **Groupe de ressources**, sélectionnez **Créer**, puis entrez **Sentinel-RG**.

1. Nommez l’espace de travail.  Exemple : SentinelLogAnalytics.

1. Sélectionner une région proche de vous.

1. Sélectionnez **Vérifier + créer**, puis **Créer**.

1. Une fois le déploiement de l’espace de travail Log Analytics terminé, choisissez le bouton **Actualiser**. Sélectionnez ensuite votre espace de travail, puis **Ajouter**.  Cela ajoute l’espace de travail à Microsoft Sentinel et ouvre Microsoft Sentinel.

1. Si l’invite vous le demande, sélectionnez **OK** pour activer l’essai gratuit de Microsoft Sentinel.

#### Tâche 2 : ajouter Microsoft Entra ID en tant que source de données

1. Dans le menu de **Microsoft Sentinel**, faites défiler jusqu’à **Gestion de contenu**, puis sélectionnez **Hub de contenu**.

1. Utilisez la zone de recherche pour rechercher **Entra** dans la liste des connecteurs, recherchez **Microsoft Entra ID** et cochez la case.

1. À droite, une vignette d’aperçu s’ouvre.  Sélectionnez **Installer**.

1. Une fois l’installation terminée, sélectionnez l’élément **Connecteurs de données** dans le menu Configuration.

    **Remarque** : vous devez afficher un connecteur installé et voir l’**ID** Microsoft Entra listé.

1. Sélectionnez **Microsoft Entra ID**, puis **Ouvrir la page du connecteur**.

1. Sur la page Connecteur, les instructions et les étapes suivantes sont fournies pour le connecteur de données. Vérifiez que la case est en regard de chacune des **conditions préalables** est cochée pour poursuivre la **Configuration**.

1. Sous **Configuration**, cochez les cases pour **Journaux de connexion** et **Journaux d’audit**. Des sources de journal supplémentaires sont disponibles, mais sont actuellement en **Préversion** et hors de portée pour ce cours.

1. Sélectionnez **Appliquer les modifications**. 

1. Une notification indiquera que les modifications ont bien été appliquées. Accédez à l’espace de travail **Microsoft Sentinel** en sélectionnant le **X** en haut à droite de la page du connecteur.

1. Sélectionnez **Actualiser** sur la vignette **Microsoft Sentinel | Connecteurs de données** et le chiffre 1 s’affichera dans le compte **Connecté**.

   **Remarque** : la prise en compte du connecteur de données Microsoft Entra ID dans le nombre de connecteurs actifs peut prendre quelques minutes. 

#### Tâche 3 : exécuter une requête Kusto sur l’activité utilisateur

1. Dans **Microsoft Sentinel**, accédez aux **Journaux** sous le titre du menu **Général**.

1. Fermez la fenêtre **Bienvenue dans l’analyse des journaux d’activité**.

1. Une fenêtre s’ouvre avec des exemples de requêtes. Sélectionnez **Audit** et recherchez les **ID d’utilisateur**.

1. Sélectionnez **Exécuter**. 

1. Cela fournit la liste des ID d’utilisateur sur Microsoft Entra ID.  Étant donné que nous venons de créer l’espace de travail, vous ne voyez peut-être pas les résultats.  Notez le format de la requête.
