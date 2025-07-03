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

4. Sur la page Nouveau projet, attribuez un nom au projet : +++MyB2BApp+++ puis sélectionnez **Créer**.

5. Ouvrez le nouveau projet en sélectionnant le lien dans la zone de message Notifications ou en utilisant le menu de projet en haut de la page.

6. Dans le menu de gauche, sélectionnez **API et services**, puis **Écran de consentement OAuth**.

7. Cliquez sur le bouton **Commencer**.

8. Sur l’écran Informations sur l’application, renseignez les informations suivantes :

| Section | Nom du champ | Valeur |
| :---    | :---    | :---  |
| 1 Informations sur l’application | | |
|            | Nom de l’application | +++Microsoft Entra ID+++ |
|            | E-mail du support utilisateur | Sélectionnez le nom de l’e-mail dans le menu déroulant |
| 2 Audience | | |
|            | Interne/Externe | **Externe** |
| 3 Informations de contact | | |
|            | Adresses e-mail | Utilisez le même nom d’e-mail que ci-dessus |
| 4 Terminer | | |
|            | Contrat | Cochez la case |

9. Sélectionnez le bouton **Créer** pour continuer.

10. Sélectionnez le bouton **Créer un client OAuth**.

11. Choisissez **Type d’application = Application Web**.

12. Acceptez le nom par défaut de l’application.

13. Dans la section **Origines JavaScript autorisées**, sélectionnez le bouton **+ Ajouter un URI**.

14. Entrez l’URI +++https://microsoftonline.com+++ comme valeur.

15. Dans la section **URI de redirection autorisés**, sélectionnez le bouton **+ Ajouter un URI**.  Vous devrez ajouter trois URI différents dans cette section :

 - **Premier URI** = +++https://login.microsoftonline.com+++
 - **Deuxième URI** = +++https://login.microsoftonline.com/te/**ID du client**/oauth2/authresp+++ (où <tenant ID>correspond à votre ID client)
 - **Troisième URI** = +++https://login.microsoftonline.com/te/**nom du client**.onmicrosoft.com/oauth2/authresp+++ (où <tenant name>correspond à votre nom de client)

**Astuce labo** : cette étape peut être plus simple si vous utilisez Notepad dans la machine virtuelle du labo pour créer ces URI, puis les copier-coller à partir de là.

**Conseil de labo** 2 : le résultat final doit ressembler à cela, avec votre ID client et le nom de client.

| URI # | Lien |
| :--- | :--- |
| URI 1 | https://login.microsoftonline.com |
| URI 2 | https://login.microsoftonline.com/te/aaaa1111bbbb2222cccc |
| URI 3 | https://login.microsoftonline.com/te/MyTenantName.onmicrosoft.com/oauth |

16. Cliquez sur le bouton **Créer**.

17. Une fois l’élément créé, copiez l’**ID client** et le **secret client** dans Notepad pour les réutiliser plus tard.

18. Vous pouvez laisser le projet tel quel, il n’est pas nécessaire de le publier.

#### Tâche 2 : ajouter un utilisateur de test

1. Dans le menu de gauche, sélectionnez l’élément **Audience**.

2. Dans la section **Utilisateurs de test** de la page, choisissez **+ Ajouter des utilisateurs**.

3. Saisissez le compte Gmail que vous utilisez pour ce labo.

4. Cliquez sur **Enregistrer**

#### Tâche 3 - Ajouter un domaine autorisé à la personnalisation

1. Dans le menu de gauche, sélectionnez l’élément **Personnalisation**.

2. Faites défiler vers le bas de la page.

3. Dans la section **Domaines autorisés**, ajoutez le domaine **microsoftonline.com**.

4. Dans la section **Informations de contact du développeur**, ajoutez l’adresse e-mail que vous utilisez pour ce labo.

5. Cliquez sur **Enregistrer**.


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
1. Si vous avez utilisé un compte Gmail existant, n’oubliez pas de supprimer le compte via **Identités externes | Tous les fournisseurs d’identité**. Vous pouvez également revenir à la console du développeur Google et supprimer le projet que vous avez créé.

2. Ouvrez Microsoft Entra ID.

3. Accédez à Utilisateurs, puis sélectionnez **Tous les utilisateurs**.

4. Sélectionnez **+ Nouvel utilisateur**.

5. Sélectionnez **Inviter un utilisateur externe** dans le menu déroulant.

6. Entrez les informations du compte Gmail que vous avez configuré en tant qu’utilisateur test pour l’application Google, dans la tâche 2 de l’exercice 1.

7. Entrez un message personnel de votre choix.

8. Sélectionnez **Vérifier et inviter**, puis sélectionnez **Inviter**.


| **Remarque relative à la sécurité** |
| ----: |
| Si vous utilisez un compte Gmail existant avec les Passkeys activées, vous ne pourrez pas finaliser le processus de connexion dans l’environnement du labo.  Les Passkeys nécessitent le Bluetooth, qui ne peut pas être activé dans la machine virtuelle.  Vous pouvez malgré tout terminer le labo, il vous suffit de réaliser ces dernières étapes dans un navigateur InPrivate en dehors de l’environnement du labo. |


#### Tâche 3 : accepter l’invitation et se connecter

1. Utilisez un navigateur InPrivate pour vous connecter à votre compte Gmail.

2. Ouvrez l’**invitation Microsoft depuis la boîte de réception**.

3. Sélectionnez le lien **Accepter l’invitation** dans le message.

3. Saisissez votre nom d’utilisateur et votre mot de passe comme demandé dans la boîte de dialogue de connexion (le cas échéant).

   **REMARQUE** : si la fédération fonctionne correctement, c’est là que vous verrez les premiers résultats de votre nouveau fournisseur d’identité externe Google.  Vous allez accéder à l’écran de connexion et pourrez vous connecter avec vos informations d’identification Gmail.  Si la fédération ne fonctionne pas ou n’a pas été configurée, l’utilisateur recevra un e-mail ACCOUNT VERIFICATION après la connexion, afin de confirmer le compte.  Avec la fédération, aucune vérification supplémentaire n’est nécessaire.

   **REMARQUE** : si vous obtenez une erreur d’accès 500, attendez environ 30 secondes et actualisez la page.  Sélectionnez RENVOYER.  Cette erreur est un problème de minutage uniquement dans l’environnement de labo.

4. Lisez le nouveau message  **Autorisations demandées par :** que vous recevez.  Ce message provient de votre domaine Azure Lab.

5. Choisissez **Accepter**.

6. Une fois la connexion terminée, vous recevrez MyApplications.

#### Tâche 4 : se connecter à Microsoft 365 à l’aide de votre compte Google

1. Une fois que vous avez terminé le processus d’invitation de l’utilisateur externe de la tâche 3, vous pouvez vous connecter directement à Microsoft Online.

2. Ouvrez un nouvel onglet dans le navigateur que vous avez ouvert.

   **REMARQUE** : si vous n’avez pas ouvert un nouveau navigateur InPrivate durant la tâche 3, vous devez le faire pour cette étape.

3. Entrez l’adresse web suivante :

   ```
   login.microsoftonline.com
   ```

4. Sélectionnez **Options de connexion** dans la boîte de dialogue.
 
5. Sélectionnez **Se connecter à une organisation**.

6. Saisissez le **Nom de domaine de votre locataire de labo** dans la zone, puis sélectionnez **Suivant**.

7. Saisissez l’adresse e-mail et le mot de passe **Google** que vous avez créés.

À ce stade, vous devriez voir votre compte transmis à Google pour confirmation. Accédez ensuite au Portail Microsoft Office.
