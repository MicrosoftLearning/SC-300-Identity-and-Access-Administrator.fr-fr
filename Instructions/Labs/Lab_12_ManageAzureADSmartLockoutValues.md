---
lab:
  title: "12 - Gérer les valeurs du verrouillage intelligent Azure\_AD"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 12 - Gérer les valeurs de verrouillage intelligent Azure AD

## Scénario de l’exercice

Vous devez configurer les paramètres de protection de mot de passe supplémentaires pour votre organisation.

#### Durée estimée : 5 minutes

### Exercice 1 - Gérer les valeurs de verrouillage intelligent Azure AD

#### Tâche - Ajouter des verrouillages intelligents

En fonction des exigences de votre organisation, vous pouvez personnaliser les valeurs du verrouillage intelligent Azure AD. Pour personnaliser les paramètres de verrouillage intelligent en vue de répondre aux besoins de votre organisation, vos utilisateurs doivent disposer d’une licence Azure AD Premium P1 ou plus élevée.

1. Accédez à l’adresse [https://portal.azure.com](https://portal.azure.com) et connectez-vous à l’aide d’un compte d’administrateur général pour l’annuaire.

2. Ouvrez le menu du portail et sélectionnez  **Azure Active Directory**.

3. Sur la page Azure Active Directory, sous **Gérer**, sélectionnez **Sécurité**.

4. Sur la page Sécurité, dans la navigation gauche, sélectionnez **Méthodes d’authentification**.

5. Dans le volet de navigation de gauche, sélectionnez **Protection par mot de passe**.

    ![Capture de l’écran affichant le panneau Méthodes d’authentification et les sélections pour accéder à l’authentification par mot de passe mises en surbrillance ](./media/lp2-mod3-browse-to-password-protection.png)

6. Dans les paramètres de protection par mot de passe, dans la zone **Durée du verrouillage en secondes**, définissez la valeur sur **120**.

7. En regard de **Mode**, sélectionnez **Appliqué**.

8. Enregistrez vos modifications.

    **REMARQUE** : lorsque le seuil de verrouillage intelligent est déclenché, le message suivant s'affiche lorsque le compte est verrouillé :
    - Votre compte est temporairement verrouillé pour éviter toute utilisation non autorisée. Réessayez plus tard. Si le problème persiste, contactez votre administrateur.

9. Pour vérifier que l’opération a réussi, choisissez un utilisateur dans votre locataire Azure AD, accédez à la page <login.microsoftonline.com> dans une fenêtre de navigation privée et entrez un mot de passe incorrect jusqu’à recevoir un message indiquant que le compte est verrouillé.
