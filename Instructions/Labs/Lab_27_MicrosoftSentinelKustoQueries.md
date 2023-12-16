---
lab:
  title: "27\_- Requêtes Kusto Microsoft Sentinel pour des sources de données Azure\_AD"
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 27 - Requêtes Kusto Microsoft Sentinel pour des sources de données Azure AD

**Remarque** : ce labo nécessite un pass Azure. Consultez le labo 00 pour obtenir des instructions.

## Scénario de l’exercice

Microsoft Sentinel est la solution SIEM et SOAR native cloud de Microsoft.  Grâce à la connexion de sources de données à partir de solutions de sécurité Microsoft et tierces, vous avez la possibilité d’exécuter des tâches d’opérations de sécurité.  Dans cet exercice de labo, vous allez créer un espace de travail Microsoft Sentinel avec des connecteurs de données à Azure AD pour exécuter des requêtes de chasse à l’aide du langage de requête Kusto (KQL). 

#### Durée estimée : 30 minutes

### Exercice 1 - Configurer Microsoft Sentinel pour les requêtes Kusto

#### Tâche 1 - Créer un espace de travail Microsoft Sentinel

1. Connectez-vous à  [https://portal.azure.com](https://portal.azure.com) en tant qu’administrateur global.

1. Recherchez et sélectionnez **Microsoft Sentinel**. 

1. Sélectionnez **Créer Microsoft Sentinel**.

1. Dans la vignette **Ajouter Microsoft Sentinel à un espace de travail**, sélectionnez **Créer un espace de travail**.

1. Dans **Groupe de ressources**, sélectionnez **Créer**, puis entrez **Sentinel-RG**.

1. Nommez l’espace de travail.  Exemple - SentinelLogAnalytics.

1. Sélectionner une région proche de vous.

1. Sélectionnez **Vérifier + créer**, puis **Créer**.

1. Une fois le déploiement de l’espace de travail Log Analytics terminé, choisissez le bouton **Actualiser**. Sélectionnez ensuite votre espace de travail, puis sélectionnez **Ajouter**.  Cette opération ajoute l’espace de travail à Microsoft Sentinel et ouvre Microsoft Sentinel.

1. Si vous y êtes invité, sélectionnez **OK** pour activer l’essai gratuit de Microsoft Sentinel.

#### Tâche 2 - Ajouter Azure AD en tant que source de données

1. Dans le menu de navigation de **Microsoft Sentinel**, sélectionnez **Configuration**, puis **Connecteurs de données**.

1. Dans la liste des connecteurs de données, recherchez **Azure Active Directory** et sélectionnez-le.

1. À droite, une vignette d’aperçu s’ouvre.  Sélectionnez **Ouvrir la page du connecteur**.

1. Dans la page du connecteur, les instructions et les étapes suivantes sont fournies pour le connecteur de données. Vérifiez qu’une coche est présente en regard de chacune des **conditions préalables** pour poursuivre la **configuration**.

1. Sous **Configuration**, cochez les cases **Journaux de connexion** et **Journaux d’audit**. Des sources de journal supplémentaires sont disponibles, mais sont actuellement en **préversion** et ne sont pas concernées par ce cours.

1. Sélectionnez **Appliquer les modifications**. 

1. Une notification s’affiche pour signaler que les modifications ont bien été appliquées. Accédez à l’espace de travail **Microsoft Sentinel** en sélectionnant la croix (**X**) dans le coin supérieur droit de la page du connecteur.

1. Sélectionnez **Actualiser** sur la vignette **Microsoft Sentinel | Connecteurs de données** et le numéro 1 s’affiche dans le compteur **Connecté**.

   **Remarque** : l’affichage du connecteur de données Azure AD dans le compteur actif peut prendre quelques minutes. 

#### Tâche 3 - Exécuter une requête Kusto sur l’activité utilisateur

1. Dans **Microsoft Sentinel**, accédez à **Journaux** sous le titre de menu **Général**.

1. Fermez la fenêtre **Bienvenue dans Log Analytics**.

1. Une fenêtre s’ouvre avec des exemples de requêtes. Sélectionnez **Audit**, puis faites défiler la page pour rechercher **ID utilisateur**.

1. Sélectionnez **Exécuter**. 

1. Cette opération fournit la liste des ID d’utilisateur sur Azure AD.  Il est possible qu’aucun résultat ne s’affiche, car nous venons de créer l’espace de travail.  Observez le format de la requête.

1. Sous **Gestion des menaces** dans le menu, sélectionnez **Repérage**. 

1. Faites défiler la page vers le bas pour rechercher la requête **Anomalous sign-in location by user account and authenticating application** (Emplacement de connexion anormale par compte d’utilisateur et application d’authentification).  Cette requête sur la connexion Azure Active Directory prend en compte toutes les connexions utilisateur pour chaque application Azure Active Directory et sélectionne la modification anormale la plus importante dans le profil d’emplacement pour un utilisateur au sein d’une application individuelle. L’objectif est de repérer la compromission d’un compte d’utilisateur, éventuellement via un vecteur d’application spécifique. 

1. Sélectionnez **Afficher les résultats de la requête** pour exécuter la requête.

1. Cette opération risque de ne pas fournir de résultats avec le nouvel espace de travail. Vous avez toutefois vu à présent comment exécuter des requêtes pour collecter des informations ou pour rechercher des menaces potentielles.
