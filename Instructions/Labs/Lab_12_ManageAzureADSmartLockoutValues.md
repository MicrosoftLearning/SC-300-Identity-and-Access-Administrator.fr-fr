---
lab:
  title: "12\_: Gérer les valeurs de verrouillage intelligent de Microsoft Entra"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 12 : Gérer les valeurs de verrouillage intelligent Microsoft Entra

### Type de connexion = Administrateur Microsoft 365

## Scénario de labo

Vous devez configurer les paramètres supplémentaires de protection de mot de passe pour votre organisation.

#### Durée estimée : 5 minutes

### Exercice 1 : Gérer les valeurs de verrouillage intelligent de Microsoft Entra

#### Tâche : ajouter des verrous intelligents

En fonction des exigences de votre organisation, vous pouvez personnaliser les valeurs du verrouillage intelligent Microsoft Entra. Pour personnaliser les paramètres de verrouillage intelligent en vue de répondre aux besoins de votre organisation, vos utilisateurs doivent disposer d'une licence Microsoft Entra ID Premium P1 ou plus élevée.

1. Accédez au [https://entra.microsoft.com](https://entra.microsoft.com) et connectez-vous à l’aide d’un compte d’administrateur général pour le répertoire.

2. Ouvrez le menu du portail, puis sélectionnez  **Identité**.

3. Dans le menu Identité, ouvrez le menu **Protection**.

4. Dans le panneau de navigation de gauche, sélectionnez **Méthodes d’authentification**.

5. Sélectionnez ensuite **Protection par mot de passe**.

    ![Image de l’écran affichant la page Méthodes d’authentification et les sélections pour accéder à l’authentification par mot de passe mises en surbrillance](./media/lp2-mod3-browse-to-password-protection.png)

6. Dans les paramètres de protection par mot de passe, dans le champ **Durée du verrouillage en secondes**, définissez la valeur sur **120**.

7. En regard de **Mode**, sélectionnez **Appliqué**.

8. Enregistrez les changements apportés.

    **REMARQUE** : lorsque le seuil de verrouillage intelligent est déclenché, le message suivant s’affiche en cas de verrouillage du compte :
    - Votre compte est temporairement verrouillé pour éviter toute utilisation non autorisée. Réessayez plus tard. Si le problème persiste, contactez votre administrateur.

9. Cela peut être testé en choisissant un utilisateur dans votre locataire Microsoft Entra. Accédez à <login.microsoftonline.com> via un navigateur privé et entrez un mot de passe incorrect jusqu’à ce que le compte reçoive une notification indiquant qu’il est verrouillé.
