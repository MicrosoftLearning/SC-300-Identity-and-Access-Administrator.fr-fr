---
lab:
  title: 5 - Ajouter des utilisateurs invités au répertoire
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Labo 5 - Ajouter des utilisateurs invités au répertoire

## Scénario de l’exercice

Votre entreprise travaille avec de nombreux fournisseurs et, à l’occasion, vous devez ajouter des comptes de fournisseurs à votre annuaire en tant qu’invité.

#### Durée estimée : 20 minutes

### Exercice 1 - Ajouter des utilisateurs invités au répertoire

#### Tâche - Ajouter l’utilisateur invité

1. Connectez-vous à l’adresse [https://portal.azure.com](https://portal.azure.com) en tant qu’utilisateur affecté à un rôle d’annuaire administrateur limité ou à un rôle Inviteur d’invités.

2. Sélectionnez **</bpt>"> **Azure Active Directory**.

3. Sous **Gérer**, sélectionnez **Utilisateurs**.

4. Sélectionnez  **+ Nouvel utilisateur**.

5. Dans le menu Nouvel utilisateur, sélectionnez **Inviter un utilisateur externe**, puis ajoutez vos informations d’utilisateur invité.

    **REMARQUE** : les adresses e-mail de groupe ne sont pas prises en charge. Veuillez entrer des adresses e-mail individuelles. Certains fournisseurs de messagerie permettent aux utilisateurs d’ajouter un signe plus (+) et du texte à leurs adresses e-mail pour faciliter notamment le filtrage de la boîte de réception. Toutefois, Azure AD ne prend pas en charge les signes plus dans les adresses e-mail pour l’instant. Pour éviter les problèmes de livraison, omettez le signe plus (+) et les caractères après celui-ci jusqu’au symbole @.

6. Entrez une adresse e-mail, par exemple **sc300externaluser1@sc300email.com**.

7. Lorsque vous avez terminé, sélectionnez **Inviter**.

8. Sur la page Utilisateurs, vérifiez que votre compte est listé et, dans la colonne **Type d’utilisateur**, vérifiez que **Invité** est affiché.

Après avoir envoyé l’invitation, le compte d’utilisateur est automatiquement ajouté au répertoire en tant qu’invité.


### Exercice 2 - Inviter des utilisateurs en bloc

#### Tâche 1 - Invitation d’utilisateurs en bloc

Votre entreprise vient de nouer un partenariat avec une autre société. Pour l’instant, les employés de l’entreprise partenaire seront ajoutés en tant qu’invités. Vous devez vous assurer que vous pouvez importer plusieurs utilisateurs invités à la fois.

1. Connectez-vous à l’adresse [https://portal.azure.com](https://portal.azure.com) en tant qu’administrateur général.

2. Dans le volet de navigation, sélectionnez **Azure Active Directory**.

3. Sous **Gérer**, sélectionnez **Utilisateurs**.

4. Dans le menu de la page Utilisateurs, sélectionnez **Opérations en bloc > Invitation en bloc**.

     ![Capture d’écran montrant la page Tous les utilisateurs, avec les options de menu Opérations en bloc et Invitation en bloc en surbrillance.](./media/lp1-mod3-bulk-invite-option.png)

5. Dans le Panneau utilisateurs d’invitation en bloc, sélectionnez **Télécharger** vers un exemple de modèle CSV avec les propriétés de l’invitation.

6. À l’aide d’un éditeur pour afficher le fichier CSV, passez en revue le modèle.

7. Ouvrez le modèle .csv et ajoutez une ligne pour chaque utilisateur invité. Les valeurs obligatoires sont les suivantes :

    - **Adresse e-mail à inviter** : utilisateur qui recevra une invitation
    - **URL de redirection** : URL vers laquelle l’utilisateur invité est transféré après avoir accepté l’invitation.

    ![Capture d’écran montrant l’exemple de fichier CSV d’invitation en bloc.](./media/lp1-mod3-template-csv.png)

8. Enregistrez le fichier.

9. Dans la page Inviter des utilisateurs en bloc, sous **Chargez votre fichier .csv**, accédez au fichier.

     **Remarque** : quand vous sélectionnez le fichier, la validation du fichier .csv démarre.

10. Quand le contenu du fichier est validé, un message indique **Fichier chargé**. Si des erreurs sont présentes, vous devez les corriger avant de pouvoir envoyer le travail.

    ![Capture d’écran montrant les utilisateurs d’invitation en bloc, avec le fichier téléchargé avec succès en surbrillance.](./media/lp1-mod3-bulk-invite-users-upload-csv.png)

11. Une fois votre fichier validé, sélectionnez **Envoyer** pour démarrer l’opération en bloc Azure qui ajoute les invitations.

12. Pour voir l’état du travail, sélectionnez **Cliquez ici pour afficher l’état de chaque opération**. Vous pouvez également sélectionner **Résultats de l’opération en bloc** dans la section Activité. Pour plus d’informations sur chaque élément de ligne au sein de l’opération en bloc, sélectionnez les valeurs sous les colonnes **Nombre de réussites**, **Nombre d’échecs** ou **Nombre total de requêtes**. Si des échecs se sont produits, les raisons sont affichées.

    ![Capture d’écran montrant les résultats d’une opération en bloc](./media/lp1-mod3-bulk-operations-results.png)

13. Une fois le travail terminé, vous recevez une notification indiquant que l’opération en bloc a réussi.

#### Tâche 2 - Inviter des utilisateurs avec PowerShell

1. Ouvrez PowerShell ISE en tant qu’administrateur.  Pour ce faire, recherchez PowerShell dans Windows et sélectionnez Exécuter en tant qu’administrateur.  

1. Vous devez ajouter le module Azure AD PowerShell si vous ne l’avez pas encore utilisé.  Exécutez la commande suivante : Install-Module AzureAD.  Quand vous y êtes invité, sélectionnez « Y » pour continuer.

    ``` 
    Install-Module AzureAD
    ```

1. Vérifiez que le module est installé correctement en exécutant la commande suivante :  

    ```
    Get-Module AzureAD 
    ```

1. Connectez-vous ensuite à Azure en exécutant la commande suivante :  

    ```
    Connect-AzureAD
    ```
    
1. La fenêtre de connexion Microsoft s’affiche pour vous permettre de vous connecter à Azure AD.  

1. Pour vérifier que vous êtes connecté et que vous voyez les utilisateurs existants, exécutez la commande suivante :  

    ```
    Get-AzureADUser 
    ```

1. Vous êtes fin prêt à convier un utilisateur invité.  Renseignez les informations relatives à l’utilisateur dans la commande suivante et exécutez-la.  Si vous souhaitez ajouter plusieurs utilisateurs, vous pouvez saisir les informations relatives aux utilisateurs dans un fichier txt du Bloc-notes, puis les copier/coller dans PowerShell. 

    ```
    New-AzureADMSInvitation -InvitedUserDisplayName "Display" -InvitedUserEmailAddress name@emaildomain.com -InviteRedirectURL https://myapps.microsoft.com -SendInvitationMessage $true 
    ```

Vous avez appris à inviter des utilisateurs dans le portail Azure AD et dans le Centre d’administration Microsoft 365, ainsi qu’à envoyer des invitations en bloc à l’aide d’un fichier CSV et inviter des utilisateurs en utilisant des commandes PowerShell.
