---
lab:
  title: "22\_: Créer et gérer un catalogue de ressources dans la gestion des droits d’utilisation Microsoft Entra"
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 22 : Créer et gérer un catalogue de ressources dans la gestion des droits d’utilisation Microsoft Entra

### Type de connexion = Administrateur Microsoft 365

## Scénario de labo

Un catalogue est un conteneur de ressources et de packages d’accès. Vous créez un catalogue lorsque vous souhaitez regrouper des ressources et packages d’accès liés. La personne qui crée le catalogue en devient le premier propriétaire. Un propriétaire de catalogue peut ajouter d’autres propriétaires de catalogue. Vous devez créer et configurer un catalogue dans votre organisation.

#### Durée estimée : 15 minutes

### Exercice 1 : Générer des ressources dans Gestion des droits d’utilisation

#### Tâche 1 : créer un catalogue

1. Connectez-vous à [https://entra.microsoft.com](https://entra.microsoft.com) en utilisant un compte d’administrateur général.

    **Important** : pour utiliser et configurer les conditions d’utilisation de Microsoft Entra ID, vous devez disposer des éléments suivants :
    - Abonnement Microsoft Entra ID Premium P1, P2, EMS E3 ou EMS E5.
    - Si vous n’avez pas l’un de ces abonnements, vous pouvez obtenir la version d’évaluation de Microsoft Entra ID Premium.
    - Un des comptes d’administrateur suivants pour le répertoire que vous souhaitez configurer :
        - Administrateur général
        - Security Administrator
        - Administrateur de l’accès conditionnel

2. Ouvrez le menu **Identité**, puis sélectionnez  **Gouvernance d’identité**.

3. Dans le menu de gauche, sous **Gestion des droits d'utilisation**, sélectionnez **Catalogues**.

4. Dans le menu supérieur, sélectionnez **+ Nouveau catalogue**.

    ![Image de l’écran affichant la page du Catalogue de gouvernance des identités avec le menu Nouveau catalogue mis en évidence ](./media/lp4-mod1-identity-governance-new-catalog.png)

5. Dans le volet Nouveau catalogue, dans la zone **Nom** , entrez **Marketing**.

6. Dans la zone **Description**, entrez **Pour les utilisateurs du service marketing**. Les utilisateurs verront ces informations dans les détails d’un package d’accès.

7. Sous **Activé**, sélectionnez **Oui**.

  - Définissez **Activé pour les utilisateurs externes** sur **Non**.  Ce paramètre permet aux utilisateurs des répertoires externes sélectionnés d’être en mesure de demander des packages d’accès dans ce catalogue. Aucune modification ne sera apportée à ce paramètre.

9. Vous pouvez choisir d’activer le catalogue pour une utilisation immédiate ou de désactiver si vous envisagez de le mettre en place ou de le garder indisponible tant que vous n’avez pas l’intention de l’utiliser. Dans le cadre de cet exercice, il n’est pas nécessaire d’activer le catalogue.

  ![Image de l’écran affichant le nouveau panoramique du catalogue avec les options Nom, Description, Activé et Créer mises en surbrillance](./media/lp4-mod1-new-catalog-marketing.png)

10. Sélectionnez **Créer**.

#### Tâche 2 : ajouter des ressources à un catalogue

Pour inclure des ressources dans un package d’accès, les ressources doivent exister dans un catalogue. Les types de ressources que vous pouvez ajouter sont des groupes, des applications et des sites SharePoint Online. Les groupes peuvent être des groupes Microsoft 365 ou des groupes de sécurité Microsoft Entra créés sur le cloud. Pour les applications, il peut s’agir d’applications d’entreprise Microsoft Entra, ce qui comprend les applications SaaS et vos propres applications fédérées à Microsoft Entra ID. Pour les sites, il peut s’agir de sites ou de collection de sites SharePoint Online.

1. Sur la page Gouvernance des identités, si nécessaire, sélectionnez **Catalogues**.

2. Dans la liste **Catalogues**, sélectionnez **Marketing**.

3. Dans le volet de navigation de gauche, sous **Gérer**, sélectionnez **Ressources**.

4. Dans le menu, sélectionnez + **Ajouter des ressources**.

5. Sur la page Ajouter des ressources au catalogue, passez en revue les options disponibles.  Ajoutez les élements suivants :

   | Type de ressource | Valeur |
   | :------------- | :---------- |
   |  **Groupes et équipes** | Retail |
   |  **Applications** | Box |
   |  **Applications** | Salesforce |
   |  **Sites SharePoint** | Marque : sélectionnez ce site SharePoint à partir de la liste de vos sites disponibles. |

6. Vous n’avez peut-être pas de ressources dans des groupes et des équipes, des applications ou des sites SharePoint. Sélectionnez n’importe quelle catégorie de ressources, puis sélectionnez une ressource dans cette catégorie.

7. Pour cet exercice, vous pouvez choisir n’importe quelle ressource disponible.

    ![Ajouter des ressources à un catalogue](./media/catalog-add-resources.png)

8. Lorsque vous avez terminé, sélectionnez **Ajouter**. Ces ressources peuvent désormais être incluses dans des packages d’accès du catalogue.

#### Tâche 3 : ajouter d’autres propriétaires de catalogue

L’utilisateur qui a créé un catalogue devient le premier propriétaire de catalogue. Pour déléguer la gestion d’un catalogue, vous ajoutez des utilisateurs au rôle de propriétaire de catalogue. Ainsi, les responsabilités de la gestion du catalogue sont mieux partagées.

1. Au besoin, dans le centre d’administration Microsoft Entra, accédez à **Identité**, sélectionnez **Gouvernance d’identité**, **Catalogues**, puis **Marketing**.

2. Dans le catalogue marketing, dans le menu de navigation de gauche, sélectionnez **Rôles et administrateurs**.

    ![Image de l’écran affichant la page Rôles et administrateurs du Catalogue marketing](./media/lp4-mod1-catalog-roles-and-admins.png)

3. Dans le menu supérieur, passez en revue les rôles disponibles, puis sélectionnez **+ Ajouter un propriétaire de catalogue**.

4. Dans le volet Sélectionner un membre, sélectionnez **Adele Vance**, puis choisissez **Sélectionner**.

5. Examinez le rôle qui vient d’être ajouté dans la liste rôles et administrateurs.

#### Tâche 4 : modifier un catalogue

Vous pouvez modifier le nom et la description d’un catalogue. Les utilisateurs verront ces informations dans les détails d’un package d’accès.

1. Sur la page Marketing, dans le volet de navigation de gauche, sélectionnez **Vue d’ensemble**.

2. Dans le menu du haut, sélectionnez **Modifier**.

3. Vérifiez le paramètre et, sous **Propriétés** > **Activé**, sélectionnez **Oui**.

    ![Image de l’écran affichant les propriétés en cours d’activation.](./media/lp4-mod1-edit-marketing-catalog.png)

4. Cliquez sur **Enregistrer**.

#### Tâche 5 : créer des révisions d’accès pour les utilisateurs invités

1. Les révisions d’accès peuvent gérer le cycle de vie des accès.° La gouvernance d’identité Microsoft Entra fournit un tableau de bord de vue d’ensemble montrant le statut des révisions d’accès. Sélectionnez **Révisions d’accès** dans le menu **Gouvernance d’identité**.

1. Dans le menu Révision d’accès, vous pouvez sélectionner **révisions d’accès** pour configurer une révision d’accès pour les utilisateurs invités.° Vous sélectionnez **+ Nouvelle révision d’accès** pour créer votre révision d’accès utilisateur invité.° La vignette s’ouvre pour configurer la révision d’accès pour les utilisateurs invités.

1. Dans la zone **Sélectionner ce qui doit être examiné**, sélectionnez **Équipes + Groupes**.

1. Sous **Sélectionner l’étendue de révision**, sélectionnez **Tous les groupes Microsoft 365 avec des utilisateurs invités**

1. Sous **Sélectionner l’étendue utilisateur**, sélectionnez **Utilisateurs invités uniquement**.

1. Sélectionnez **Suivant : Révisions**.

1. La vignette suivante est l’emplacement où vous configurez les personnes qui passent en revue et approuvent l’accès, la fréquence à laquelle l’accès sera examiné et l’expiration de l’accès.

1. Sous **Sélectionner des réviseurs**, sélectionnez **Propriétaires de groupe** comme réviseurs.

  - **Remarque** : les utilisateurs invités ne doivent pas être autorisés à passer en revue leur propre accès en tant que bonne pratique de gouvernance des identités.

1. Entrez une **Durée (en jours)**. La valeur par défaut est 3. Choisissez une **Périodicité de révision** et une **Date de début** pour la révision.

1. Sélectionnez **Suivant : Paramètres** et configurez les paramètres pour la façon dont la révision aura lieu et ce qui se passe lorsque l’utilisateur invité répond ou ne répond pas.  Une bonne pratique consiste à sélectionner **Appliquer automatiquement les résultats à la ressource** et à sélectionner **Supprimer l’accès** si **Les réviseurs ne répondent pas**. 

1. Sélectionnez **Suivant : examiner et créer**, puis **Créer** pour créer la révision d’accès.


#### Tâche 6 : supprimer un catalogue

Vous pouvez supprimer un catalogue, mais seulement s’il ne contient pas de packages d’accès.

1. Dans la page Vue d’ensemble du Catalogue marketing, dans le menu supérieur, sélectionnez Supprimer.

2. Dans la boîte de dialogue Supprimer, passez en revue les informations, puis sélectionnez **Non**.

    **Remarque** : nous conservons le catalogue à utiliser dans le labo suivant.
