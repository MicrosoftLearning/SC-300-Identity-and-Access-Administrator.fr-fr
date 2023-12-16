---
lab:
  title: 6 - Ajouter un fournisseur d’identités fédérées
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Labo 6 - Ajouter un fournisseur d’identités fédérées

## Scénario de l’exercice

Votre entreprise travaille avec de nombreux fournisseurs et, à l’occasion, vous devez ajouter des comptes de fournisseurs à votre annuaire en tant qu’invité et leur permettre d’utiliser leur compte Google pour se connecter.

#### Durée estimée : 25 minutes

### Exercice 1 - Configurer des fournisseurs d’identité

#### Tâche 1 - Configurer Google en tant que fournisseur d’identité

**Remarque importante** : pour cet exercice, vous aurez besoin d’un compte Gmail de Google. Créez un **compte Google**, puis suivez les étapes de l’exercice.  Veillez à noter l’adresse e-mail et le mot de passe, vous en aurez besoin plus loin dans le labo.

1. Accédez aux API Google à l’adresse https://console.developers.google.com et connectez-vous avec votre compte Google. Nous vous recommandons d’utiliser un compte Google d’équipe partagé.

2. Acceptez les conditions d’utilisation du service si vous y êtes invité.

**Créez un projet :**
3. En haut de la page, sélectionnez le menu de projet pour ouvrir la page Sélectionner un projet. Choisissez **Nouveau projet**.  Laissez les valeurs par défaut des champs restants.

4. Sur la page Nouveau projet, attribuez un nom au projet (par exemple **MyB2BApp**), puis cliquez sur **Créer**.

5. Ouvrez le nouveau projet en sélectionnant le lien dans la zone de message Notifications ou en utilisant le menu de projet en haut de la page.

6. Dans le menu de gauche, sélectionnez **API et services**, puis **Écran de consentement OAuth**.

7. Sous Type d’utilisateur, sélectionnez **Externe**, puis **Créer**.

8. Sur l’**Écran de consentement OAuth**, sous Informations sur l’application, entrez un nom d’application, par exemple **Azure AD**.

9. Sous E-mail de support de l’utilisateur, sélectionnez une adresse e-mail. Il doit s’agir de l’adresse e-mail utilisée pour vous connecter à Google.

10. Sous Domaines autorisés, sélectionnez **+ Ajouter un domaine**, puis ajoutez le domaine microsoftonline.com.

   ```
   microsoftonline.com
   ```

11. Sous Informations de contact du développeur, entrez l’adresse e-mail du compte de labo que vous avez utilisé pour vous connecter au portail.

12. Sélectionnez **Enregistrer et continuer**.

13. Dans le menu de gauche, sélectionnez **Informations d’identification**.

14. Sélectionnez **+ Créer des informations d’identification**, puis **ID client OAuth**.

15. Dans le menu Type d’application, sélectionnez Application web. Donnez à l’application un nom approprié, par exemple Azure AD B2B. Sous **URI de redirection autorisés**, ajoutez les URI suivants :

   ```
      https://login.microsoftonline.com
   ```
      https://login.microsoftonline.com/te/**ID de locataire**/oauth2/authresp    (où <tenant ID> est votre ID de locataire)
   ```
      https://login.microsoftonline.com/te/**tenant name**.onmicrosoft.com/oauth2/authresp
       (where <tenant name> is your tenant name)
   ```

16. Sélectionnez **Créer**. Copiez vos **ID client** et **Clé secrète client**. Vous les utiliserez lorsque vous ajouterez le fournisseur d’identité dans le portail Azure.

17. Vous pouvez laisser votre projet à un état de publication de test.

#### Tâche 2 - Ajouter un utilisateur de test
18. Dans le menu API et services, sélectionnez **Écran de consentement OAuth**.

19. Dans la section **Utilisateurs de test* de la page, choisissez **+ Ajouter des utilisateurs**.

20. Entrez le compte Gmail que vous avez créé (ou celui que vous utilisez) pour ce labo.

21. Sélectionnez **Enregistrer**.


### Exercice 2 - Configurer Azure pour utiliser un fournisseur d’identité externe

#### Tâche 1 - Configurer la fédération de Google dans Azure AD
1. Connectez-vous à l’adresse  [https://portal.azure.com](https://portal.azure.com) en tant qu’administrateur.

2. Sélectionnez **</bpt>"> **Azure Active Directory**.

3. Sous **Gérer**, sélectionnez **Identités externes**.

4. Choisissez **Tous les fournisseurs d’identité** dans le menu de gauche.

5. Microsoft fournit une fédération directe pour **Google** en tant que fournisseur d’identité.Pour commencer la configuration, sélectionnez **+ Google** sur la page **Identités externes | Tous les fournisseurs d’identité**
 
6. Une fois que vous avez sélectionné + Google, une autre page s’ouvre avec les informations supplémentaires requises pour configurer Google en tant que fournisseur d’identité.  

7. Entrez l’**ID client** et la **Clé secrète client** obtenus précédemment.

8. Sélectionnez **Enregistrer**.

Google est maintenant configuré en tant que fournisseur d’identité.

#### Tâche 2 - Inviter votre compte d’utilisateur de test
9. Si vous avez utilisé un compte Gmail existant, n’oubliez pas de supprimer le compte avec **Identités externes | Tous les fournisseurs d’identité**. Vous pouvez également revenir à la Google Developer Console et supprimer le projet que vous avez créé.

10. Ouvrez Azure Active Directory (Azure AD).

11. Accédez à Users.

12. Sélectionnez **+ Nouvel utilisateur**.

13. Choisissez **Inviter un utilisateur externe** dans le menu déroulant.

14. Entrez les informations du compte Gmail que vous avez configuré en tant qu’utilisateur de test pour l’application Google dans l’exercice 1 de la tâche 2.

15. Rédigez un message personnel si vous le souhaitez.

16. Sélectionnez **Inviter**.

#### Tâche 3 - Accepter l’invitation et se connecter
17. Dans une fenêtre de navigation privée, connectez-vous à votre compte Gmail.

18. Ouvrez l’**Invitation Microsoft au nom de** dans la boîte de réception.

19. Sélectionnez le lien **Accepter l’invitation** dans le message.

20. Saisissez votre nom d’utilisateur et votre mot de passe comme demandé dans la boîte de dialogue de connexion (le cas échéant).
   **REMARQUE** : si la fédération fonctionne correctement, cet écran vous permet de vous connecter à l’aide de votre nouveau fournisseur d’identité externe Google.  Accédez à l’écran de connexion pour vous vous connecter avec vos informations d’identification Gmail.  Si la fédération ne fonctionne pas ou n’a pas été configurée, l’utilisateur reçoit un e-mail de VÉRIFICATION DE COMPTE après la connexion, pour confirmer le compte.  Avec la fédération, aucune vérification supplémentaire n’est nécessaire.

   **REMARQUE** : si vous obtenez une erreur d’accès 500, attendez environ 30 secondes et actualisez la page.  Choisissez RENVOYER.  Cette erreur est un problème de délai uniquement dans l’environnement du labo.

21. Consultez le message **Autorisations demandées par :** qui s’affiche.  Ce message provient de votre Azure Lab Domain.

22. Choisissez **Accepter**.

23. Une fois la connexion effectuée, vous êtes redirigé vers l’écran Mes applications.

#### Tâche 4 - Se connecter à Microsoft 365 à l’aide de votre compte Google
24. Une fois que vous avez terminé la tâche 3 et le processus d’invitation de l’utilisateur externe, vous pouvez vous connecter directement à Microsoft Online.

25. Ouvrez un nouvel onglet dans le navigateur que vous avez ouvert.
   **REMARQUE** : si vous n’avez pas ouvert de fenêtre de navigation privée au cours de la tâche 3, vous devez le faire pour cette étape.

26. Entrez l’adresse web suivante :

   ```
   login.microsoftonline.com
   ```

27. Sélectionnez **Options de connexion** dans la boîte de dialogue.
 
28. Sélectionnez **Se connecter à une organisation**.

29. Entrez le **nom de domaine de votre locataire labo** dans la zone, puis sélectionnez **Suivant**.

30. Entrez l’adresse e-mail **Google** et le mot de passe que vous avez créés.
À ce stade, vous devriez voir votre compte transmis à Google pour confirmation ; accédez ensuite au portail Microsoft Office.
