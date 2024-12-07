---
lab:
  title: "15\_: Configurer une stratégie d’inscription avec l’authentification multifacteur"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 15 : Configurer une stratégie d’inscription avec l’authentification multifacteur

### Type de connexion = Administrateur Microsoft 365

## Scénario de labo

L'authentification multifacteur permet de vérifier l'identité d'une personne en utilisant plus qu'un nom d'utilisateur et un mot de passe. Cette stratégie fournit une deuxième couche de sécurité aux connexions d’utilisateur. Pour que les utilisateurs puissent répondre aux invites d’authentification multifacteur, ils doivent d’abord s’inscrire à l’authentification multifacteur Microsoft Entra. Vous devez configurer la stratégie d’inscription MFA de votre organisation Microsoft Entra pour qu’elle soit affectée à tous les utilisateurs.

#### Durée estimée : 10 minutes

### Exercice 1 : Configurer la stratégie d’inscription MFA

#### Tâche 1 : configurer la stratégie

1. Connectez-vous à [https://entra.microsoft.com]( https://entra.microsoft.com) en utilisant un compte d’administrateur général.

2. Ouvrez le menu du portail, puis sélectionnez  **Microsoft Entra ID**.

3. Dans le menu de gauche **Identité**, sélectionnez **Protection**.

4. Sur la page Sécurité, dans le volet de navigation de gauche, sélectionnez **Protection de l’identité**.

5. Sur la page Protection de l’identité, dans le volet de navigation de gauche, sélectionnez **Protection**, puis **stratégie d’inscription avec l’authentification multifacteur**.

    ![Image de l’écran affichant la page Stratégie d’inscription MFA avec le chemin de navigation mis en surbrillance](./media/lp2-mod4-browse-to-mfa-registration-policy.png)

6. Sous **Affectations**

7. Sous **Affectations**, sélectionnez **Tous les utilisateurs** et passez en revue les options disponibles.

8. Vous pouvez sélectionner **Tous les utilisateurs** ou **Sélectionner des personnes et des groupes** si vous limitez votre déploiement.

9. En outre, vous pouvez choisir d’exclure des utilisateurs de la stratégie.

10. Sous **Contrôles**, notez que l **'inscription d'authentification multifacteur Microsoft Entra ID** est sélectionnée et ne peut pas être modifiée.


#### Tâche 2 : configurer la stratégie d’inscription MFA : Microsoft Entra ID Protection

**Remarque** : Microsoft Entra Identity Protection nécessite l’activation de Microsoft Entra ID Premium P2. 

1. Dans le centre d’administration Microsoft Entra, accédez à **Protection Microsoft Entra ID** dans la barre de recherche.

1. Sous **Protection** dans le menu, sélectionnez **Stratégie d’inscription avec l’authentification**.

1. Sous **Affectations**, sélectionnez **Tous les utilisateurs** sous Utilisateurs, puis sélectionnez un utilisateur pour appliquer l’authentification multifacteur.

1. Définissez **Application de la stratégie** sur **Activé**.

1. Cliquez sur **Enregistrer**.

Cela oblige l’utilisateur à effectuer l’inscription MFA la prochaine fois qu’il tente de se connecter.

1. À partir d’un navigateur privé, accédez à `https://login.microsoftonline.com`. Saisissez un nom d’utilisateur et un mot de passe à partir du locataire.  Notez les exigences supplémentaires en matière d’informations de sécurité que l’utilisateur est invité à saisir.
