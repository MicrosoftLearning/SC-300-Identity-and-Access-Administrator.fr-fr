---
lab:
  title: 20 - Implémenter la gestion des accès pour les applications
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# Labo 20 - Implémenter la gestion des accès pour les applications

## Scénario de l’exercice

Votre entreprise exige que seuls des utilisateurs ou des groupes spécifiques aient accès aux applications d’entreprise. Vous devez affecter un utilisateur à une application spécifique.

#### Durée estimée : 5 minutes

### Exercice 1 - Configurer une application d’entreprise

#### Tâche 1 - Ajouter une application à votre locataire Azure AD

1. Connectez-vous à l’adresse  [https://portal.azure.com](https://portal.azure.com) à l’aide d’un compte Administrateur général.

2. Ouvrez le menu du portail et sélectionnez  **Azure Active Directory**.

3. Sur la page Azure Active Directory, sous **Gérer**, sélectionnez **Applications d’entreprise**.

4. Dans le volet Applications d’entreprise, sélectionnez **+ Nouvelle application**.

    ![Capture d’écran affichant la page Applications d’entreprise, avec la nouvelle application mise en surbrillance](./media/lp3-mod1-new-enterprise-application.png)

5. Sur la page Parcourir la galerie Azure AD (préversion), dans la zone **Rechercher une application**, entrez **GitHub**.

    ![Capture d’écran affichant la page Parcourir la galerie Azure AD (préversion), avec la zone de recherche mise en surbrillance](./media/lp3-mod1-azure-ad-gallery-search.png)

6. Dans les résultats, sélectionnez **GitHub Enterprise Cloud – Enterprise Account**.

7. Dans **GitHub Enterprise Cloud – Enterprise Account**, passez en revue les paramètres, puis sélectionnez **Créer**.

8. Une fois le compte créé, vous êtes redirigé vers la page GitHub Enterprise Cloud - Enterprise Account.

#### Tâche 2 - Affecter des utilisateurs à une application

1. Sur la page Vue d’ensemble de la page GitHub Enterprise Cloud - Enterprise Account, sous **Prise en main**, sélectionnez **1. Attribuer des utilisateurs et des groupes**.

2. Dans le volet de navigation gauche, sous **Gérer**, vous pouvez également sélectionner **Utilisateurs et groupes**.

3. Dans la page Utilisateurs et groupes, dans le menu, sélectionnez **+ Ajouter un utilisateur/groupe**.

4. Dans la page Ajouter une affectation, sélectionnez **Utilisateurs et groupes**.

5. Dans le volet Utilisateurs et groupes, sélectionnez votre compte administrateur, puis **Sélectionner**.

    ![Capture d’écran affichant l’ajout d’une attribution de compte d’utilisateur à une application avec le bouton Sélectionner mis en surbrillance ](./media/lp3-mod1-add-app-assignment.png)

6. Sélectionnez **Attribuer**.

