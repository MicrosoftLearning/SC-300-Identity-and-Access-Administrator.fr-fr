---
lab:
  title: "15\_- Configurer une stratégie d’inscription d’authentification multifacteur Azure\_AD"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 15 - Configurer une stratégie d’inscription d’authentification multifacteur Azure AD

## Scénario de l’exercice

L’authentification multifacteur Azure AD permet de vérifier votre identité, à l’aide d’une méthode plus sécurisée que la simple fourniture d’un nom d’utilisateur et d’un mot de passe. Ce service fournit une deuxième couche de sécurité pour les connexions utilisateur. Pour que les utilisateurs puissent répondre aux invites d’authentification multifacteur, ils doivent d’abord s’inscrire à l’authentification multifacteur Azure AD. Vous devez configurer la stratégie d’inscription MFA de votre organisation Azure AD pour qu’elle soit attribuée à tous les utilisateurs.

#### Durée estimée : 10 minutes

### Exercice 1 - Configurer la stratégie d’inscription MFA

#### Tâche 1 - Configurer la stratégie

1. Connectez-vous au [https://portal.azure.com]( https://portal.azure.com) à l’aide d’un compte d’administrateur général.

2. Ouvrez le menu du portail et sélectionnez  **Azure Active Directory**.

3. Sur la page Azure Active Directory, sous **Gérer**, sélectionnez **Sécurité**.

4. Sur la page Sécurité, dans le volet de navigation de gauche, sélectionnez **Identity Protection**.

5. Sur la page Identity Protection, dans le menu de navigation de gauche, sous **Protéger**, sélectionnez **Stratégie d’inscription d’authentification multifacteur**.

    ![Image de l’écran affichant la page Stratégie d’inscription MFA avec le chemin de navigation mis en surbrillance](./media/lp2-mod4-browse-to-mfa-registration-policy.png)

6. Sous **Affectations**

7. Sous **Affectations**, sélectionnez **Tous les utilisateurs** et passez en revue les options disponibles.

8. Vous pouvez sélectionner **Tous les utilisateurs** ou **Sélectionner des personnes et des groupes** si vous limitez votre déploiement.

9. En outre, vous pouvez choisir d’exclure des utilisateurs de la stratégie.

10. Sous **Contrôles**, notez que **Exiger l’inscription MFA Azure AD** est sélectionné et ne peut pas être changé.


#### Tâche 2 - Configurer la stratégie Azure AD Identity Protection pour l’inscription MFA

**Remarque** : Azure AD Identity Protection nécessite une licence Azure AD Premium P2 activée. 

1. Dans le portail Azure, accédez à **Azure AD Identity Protection** dans la barre de recherche.

1. Sous **Protéger** dans le menu, sélectionnez **Stratégie d’inscription MFA**.

1. Sous **Attributions**, sélectionnez **Tous les utilisateurs** sous Utilisateurs, puis sélectionnez un utilisateur pour appliquer l’authentification multifacteur.

1. Remplacez la valeur **Désactivé** de l’option **Appliquer la stratégie** par **Activé**.

1. Sélectionnez **Enregistrer**.

L’utilisateur devra alors effectuer l’inscription MFA lors de sa prochaine connexion.

1. Dans une session de navigation privée, accédez à `https://login.microsoftonline.com`. Entrez un nom d’utilisateur et un mot de passe à partir du locataire.  Observez les exigences supplémentaires en matière d’informations de sécurité que l’utilisateur doit entrer.
