---
lab:
  title: "06\_: Ajouter un fournisseur d’identité fédéré"
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Labo 06 : Ajouter un fournisseur d’identité fédéré

### Type de connexion = Administrateur Microsoft 365

## Scénario de labo

Votre entreprise travaille avec de nombreux fournisseurs et, à l’occasion, vous devez ajouter des comptes de fournisseurs à votre répertoire en tant qu’invités et leur permettre d’utiliser leur compte Google pour se connecter.

#### Durée estimée : 25 minutes

### Exercice 1 : Configurer des fournisseurs d’identité

#### Tâche 1 : configurer Google en tant que fournisseur d’identité

**Remarque importante** : pour cet exercice, vous aurez besoin d’un compte Gmail sur Google. Créez un **compte Google**, puis suivez les étapes de l’exercice.  Veillez à noter l’adresse e-mail et le mot de passe, ces informations sont nécessaires pour effectuer le labo.

1. Accédez aux API Google à l’adresse https://console.developers.google.com et connectez-vous avec votre compte Google. Nous vous recommandons d’utiliser un compte Google d’équipe partagé.

2. Acceptez les conditions d’utilisation du service si vous y êtes invité.

**Créer un projet :**
3. En haut de la page, sélectionnez le menu de projet pour ouvrir la page Sélectionner un projet. Choisissez **Nouveau projet**.  Laissez les valeurs par défaut des champs restants.

4. Sur la page Nouveau projet, attribuez un nom au projet (par exemple **MyB2BApp**), puis sélectionnez **Créer**.

5. Ouvrez le nouveau projet en sélectionnant le lien dans la zone de message Notifications ou en utilisant le menu de projet en haut de la page.

6. Dans le menu de gauche, sélectionnez **API et services**, puis **Écran de consentement OAuth**.

7. Sous Type d’utilisateur, sélectionnez **Externe**, puis **Créer**.

8. Sur l’**écran de consentement OAuth**, sous Informations sur l’application, entrez un nom d’application, comme **Microsoft Entra ID**.

9. Sous E-mail de support de l’utilisateur, sélectionnez une adresse e-mail. Cela doit inclure l’adresse e-mail que vous avez utilisée pour vous connecter à Google.

10. Sous Domaines autorisés, sélectionnez **Ajouter un domaine**, puis ajoutez le domaine microsoftonline.com.

   ```
   microsoftonline.com
   ```

11. Sous Informations de contact du développeur, entrez l’adresse e-mail du compte de labo que vous avez utilisé pour vous connecter au portail.

12. Sélectionnez **Enregistrer et continuer**.

13. Dans le menu de gauche, sélectionnez **Informations d’identification**.

14. Sélectionnez **+ Créer des informations d’identification**, puis **ID client OAuth**.

15. Dans le menu Type d’application, sélectionnez Application web. donnez à l'application un nom qui convient, comme Microsoft Entra B2B. Sous **URI de redirection autorisés**, ajoutez les URI suivants :

   ```
      https://login.microsoftonline.com
   ```
      https://login.microsoftonline.com/te/**ID de locataire**/oauth2/authresp (où <tenant ID> se trouve votre ID de locataire)
   ```
      https://login.microsoftonline.com/te/**tenant name**.onmicrosoft.com/oauth2/authresp
       (where <tenant name> is your tenant name)
   ```

**Conseil de labo** : les résultats doivent ressembler à ceci, avec votre ID de locataire et votre nom de locataire.
| URI # | Lien |
| :--- | :--- |
| URI 1 | https://login.microsoftonline.com |
| URI 2 | https://login.microsoftonline.com/te/aaaa1111bbbb2222cccc |
| URI 3 | https://login.microsoftonline.com/te/MyTenantName.onmicrosoft.com/oauth |

16. Sélectionnez **Créer**. Copiez vos **ID client** et **clé secrète client**. Vous les utiliserez lorsque vous ajouterez le fournisseur d’identité dans le portail Azure.

17. Vous pouvez laisser votre projet sur le statut de publication Test.

#### Tâche 2 : ajouter un utilisateur de test
18. Sous API et services, sélectionnez **Écran de consentement OAuth**.

19. Dans la section **Tester les utilisateurs* de la page, choisissez **+ Ajouter des utilisateurs**.

20. Entrez le compte Gmail que vous avez créé (ou utilisé) pour ce labo.

21. Cliquez sur **Enregistrer**


### Exercice 2 : Configurer Azure pour qu’il fonctionne avec un fournisseur d’identité externe

#### Tâche 1 : configurer la fédération Google dans Microsoft Entra ID
1. Connectez-vous au  [https://entra.microsoft.com](https://entra.microsoft.com)  en tant qu’administrateur.

2. Sélectionnez  **Microsoft Entra ID**.

3. Sous  **Identité**, sélectionnez  **Identités externes**.

4. Sélectionnez **Tous les fournisseurs d’identité** dans le menu de gauche.

5. Microsoft fournit une fédération directe pour **Google** en tant que fournisseur d’identité.° Cette fédération peut être lancée en sélectionnant **+ Google** à partir de la page **Identités externes | Tous les fournisseurs d’identité**.
 
6. Après avoir sélectionné + Google, une autre page s’ouvre avec des informations supplémentaires requises pour configurer Google en tant que fournisseur d’identité.  

7. Entrez l’**ID client** et la **clé secrète client** obtenus précédemment.

8. Cliquez sur **Enregistrer**.

Cette opération termine la configuration de Google en tant que fournisseur d’identité.

#### Tâche 2 : inviter le compte d’utilisateur test
9. Si vous avez utilisé un compte Gmail existant, n’oubliez pas de supprimer le compte via **Identités externes | Tous les fournisseurs d’identité**. Vous pouvez également revenir à la console du développeur Google et supprimer le projet que vous avez créé.

10. Ouvrez Microsoft Entra ID.

11. Accédez à Utilisateurs, puis sélectionnez **Tous les utilisateurs**.

12. Sélectionnez **+ Nouvel utilisateur**.

13. Sélectionnez **Inviter un utilisateur externe** dans le menu déroulant.

14. Entrez les informations du compte Gmail que vous avez configuré en tant qu’utilisateur test pour l’application Google, dans la tâche 2 de l’exercice 1.

15. Entrez un message personnel de votre choix.

16. Sélectionnez **Inviter**.

#### Tâche 3 : accepter l’invitation et se connecter
17. Utilisez un navigateur InPrivate pour vous connecter à votre compte Gmail.

18. Ouvrez l’**invitation Microsoft depuis la boîte de réception**.

19. Sélectionnez le lien **Accepter l’invitation** dans le message.

20. Saisissez votre nom d’utilisateur et votre mot de passe comme demandé dans la boîte de dialogue de connexion (le cas échéant).
   **REMARQUE** : si la fédération fonctionne correctement, c’est là que vous verrez les premiers résultats de votre nouveau fournisseur d’identité externe Google.  Vous allez accéder à l’écran de connexion et pourrez vous connecter avec vos informations d’identification Gmail.  Si la fédération ne fonctionne pas ou n’a pas été configurée, l’utilisateur recevra un e-mail ACCOUNT VERIFICATION après la connexion, afin de confirmer le compte.  Avec la fédération, aucune vérification supplémentaire n’est nécessaire.

   **REMARQUE** : si vous obtenez une erreur d’accès 500, attendez environ 30 secondes et actualisez la page.  Sélectionnez RENVOYER.  Cette erreur est un problème de minutage uniquement dans l’environnement de labo.

21. Lisez le nouveau message  **Autorisations demandées par :** que vous recevez.  Ce message provient de votre domaine Azure Lab.

22. Choisissez **Accepter**.

23. Une fois la connexion terminée, vous recevrez MyApplications.

#### Tâche 4 : se connecter à Microsoft 365 à l’aide de votre compte Google
24. Une fois que vous avez terminé le processus d’invitation de l’utilisateur externe de la tâche 3, vous pouvez vous connecter directement à Microsoft Online.

25. Ouvrez un nouvel onglet dans le navigateur que vous avez ouvert.
   **REMARQUE** : si vous n’avez pas ouvert un nouveau navigateur InPrivate durant la tâche 3, vous devez le faire pour cette étape.

26. Entrez l’adresse web suivante :

   ```
   login.microsoftonline.com
   ```

27. Sélectionnez **Options de connexion** dans la boîte de dialogue.
 
28. Sélectionnez **Se connecter à une organisation**.

29. Saisissez le **Nom de domaine de votre locataire de labo** dans la zone, puis sélectionnez **Suivant**.

30. Saisissez l’adresse e-mail et le mot de passe **Google** que vous avez créés.
À ce stade, vous devriez voir votre compte transmis à Google pour confirmation. Accédez ensuite au Portail Microsoft Office.
