---
lab:
  title: 3 - Attribuer des licences à l’aide de l’appartenance au groupe
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Labo 3 - Attribuer des licences à l’aide de l’appartenance au groupe

## Scénario de l’exercice

Votre organisation a décidé d’utiliser des groupes de sécurité dans Azure AD pour gérer les licences. Vous devez configurer un nouveau groupe de sécurité et attribuer une licence à ce groupe, puis vérifier que les licences des membres du groupe ont été mises à jour.

#### Durée estimée : 25 minutes

### Exercice 1 - Créer un groupe de sécurité et ajouter un utilisateur

#### Tâche 1 - Vérifier si Delia Dennis a accès à Office 365

1. Lancez une nouvelle fenêtre de navigation privée.
2. Se connecter à [https://www.office.com](https://www.office.com).
3. Sélectionnez Se connecter et se connectez-vous en tant que Delia Dennis.

   | **Paramètre**| **Valeur**|
   | :--- | :--- |
   | Nom d’utilisateur | DeliaD@`your domain name.com`|
   | Mot de passe| Entrez le mot de passe de l’administrateur général (disponible dans la section Ressources)|

4. Vous devez vous connecter au site web Office.com et recevoir un message indiquant que vous n’avez pas de licence.

   ![Capture d’écran du site web Office.com montrant Delia Dennis connectée, mais sans application Office disponible, car aucune licence ne lui est attribuée.](./media/delia-no-office-license.png)
    
5. Fermez la fenêtre du navigateur.

#### Tâche 2 - Créer un groupe de sécurité dans Azure Active Directory

1. Accédez à [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).

2. Dans le volet de navigation de gauche, sous **Gérer**, sélectionnez **Groupes**.
3. Sur la page Groupes, dans le menu, sélectionnez **Nouveau groupe**.
4. Créez un nouveau groupe à l’aide des informations suivantes :

   | **Paramètre**| **Valeur**|
   | :--- | :--- |
   | Type de groupe| Sécurité|
   | Nom du groupe| sg-SC300-O365|
   | Type d’appartenance| Attribué|
   | Propriétaires| *Affecter votre propre compte d’administrateur en tant que propriétaire du groupe*|

5. Sous Membres, sélectionnez **Aucun membre sélectionné**.
6. Sélectionnez **Delia Dennis** dans la liste des utilisateurs.
7. Cliquez sur le bouton **Sélectionner**.

   ![Capture d’écran de la page Nouveau groupe, avec le type de groupe, le nom de groupe, les propriétaires et les membres mis en surbrillance.](./media/lp1-mod2-create-group.png)

8. Cliquez sur le bouton **Créer**.
9. Lorsque vous avez terminé, vérifiez que le groupe nommé **sg-SC300-O365** est affiché dans la liste **Tous les groupes**.

#### Tâche 3 - Attribuer une licence à un groupe

1. Dans la liste **Tous les groupes** , sélectionnez **sg-SC300-O365**.
2. Sur la page Marketing, sous **Gérer**, sélectionnez **Licences**.
3. Dans le menu, sélectionnez **+ Affectations**.
4. Sur la page des affectations de licence de mise à jour, sous **Sélectionner des licences**, passez en revue la liste des licences disponibles, puis cochez la case correspondant à la licence **Office 365 E3**.

   **Conseil** : lorsque plusieurs licences sont sélectionnées, vous pouvez utiliser le menu Consulter les options de licence pour sélectionner une licence spécifique et afficher l’option de licence de cette licence.

   ![Image de l’écran affichant les licences sélectionnées et affectées à un groupe. Le menu Consulter la licence est également sélectionné et affiche plusieurs options de sélection.](./media/lp1-mod2-assign-license-group.png)

6. Sélectionnez **Enregistrer**.

#### Tâche 4 - Confirmer la licence Office 365

1. Lancez une nouvelle fenêtre de navigation privée.
2. Se connecter à [https://www.office.com](https://www.office.com).
3. Sélectionnez Se connecter et se connectez-vous en tant que Delia Dennis.

   | **Paramètre**| **Valeur**|
   | :--- | :--- |
   | Nom d’utilisateur | DeliaD@`your domain name.com`|
   | Mot de passe| Entrez le mot de passe de l’administrateur général (disponible dans la section Ressources)|

4. Lors de votre connexion au site web Office.com, aucun message concernant la licence ne devrait s’afficher. Toutes les application Office sont disponibles à gauche.

   ![Capture d’écran du site web Office.com montrant Delia Dennis connectée et les applications Office disponibles, car une licence lui est attribuée.](./media/delia-office-license.png)
    
5. Fermez la fenêtre du navigateur. 

### Exercice 2 - Créer un groupe Microsoft 365 dans Azure Active Directory

#### Tâche 1 - Créer le groupe

L’un des aspects de votre rôle d’administrateur Azure AD consiste à créer différents types de groupes. Vous devez créer un groupe Microsoft 365 pour le service commercial de votre entreprise.

1. Accédez à [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview]( https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).

2. Dans le volet de navigation de gauche, sous **Gérer**, sélectionnez **Groupes**.

3. Sur la page Groupes, dans le menu, sélectionnez **Nouveau groupe**.

4. Créez un nouveau groupe à l’aide des informations suivantes :

   | **Paramètre**| **Valeur**|
   | :--- | :--- |
   | Type de groupe| Microsoft 365|
   | Nom du groupe| Northwest Sales|
   | Type d’appartenance| Attribué|
   | Propriétaires| *Affecter votre propre compte d’administrateur en tant que propriétaire du groupe*|
   | Membres| **Alex Wilber** et **Bianca Pisani**|

   ![Capture d’écran de la page Nouveau groupe, avec le type de groupe, le nom de groupe, les propriétaires et les membres mis en surbrillance.](./media/lp1-mod2-create-o365-group.png)

5. Lorsque vous avez terminé, vérifiez que le groupe **Northwest Sales** est affiché dans la liste **Tous les groupes**.

### Exercice 3 - Créer un groupe dynamique avec tous les utilisateurs en tant que membres

#### Tâche 1 - Créer le groupe dynamique

À mesure que votre entreprise se développe, la gestion manuelle des groupes devient chronophage. Depuis la normalisation du répertoire, vous pouvez désormais tirer parti des groupes dynamiques. Vous devez créer un groupe dynamique pour vous assurer que vous êtes prêt à créer un groupe dynamique en production.

1. Connectez-vous à l’adresse [https://portal.azure.com](https://portal.azure.com) en utilisant un compte attribué au rôle d'administrateur général ou d’administrateur d’utilisateurs dans le locataire.

2. Sélectionnez **Azure Active Directory**.

3. Sous **Gérer**, sélectionnez **Groupes**, puis **Nouveau groupe**.

4. Sur la page Nouveau groupe, sous **Type de groupe**, sélectionnez **Sécurité**.

5. Dans la zone **Nom du groupe**, entrez **SC300-myDynamicGroup**.

6. Sélectionnez le menu **Type d’appartenance**, puis sélectionnez **Utilisateur dynamique**.

7. Sélectionnez un **propriétaire** pour le groupe.

7. Sous **Membres dynamiques**, sélectionnez **Ajouter une requête dynamique**.

8. À droite au-dessus de la zone **Syntaxe de la règle**, sélectionnez **Modifier**.

9. Dans le volet « Modifier la syntaxe de la règle », entrez l’expression suivante dans la zone **Syntaxe de la règle** :

   ```powershell
   user.objectid -ne null
   ```

   **Avertissement** : l’expression `user.objectid` est sensible à la casse.

10. Sélectionnez **OK**. La règle s’affiche dans la zone « Syntaxe de la règle ».

   ![Capture d’écran affichant la page des règles d’appartenance au groupe dynamique, avec la syntaxe de règle mise en surbrillance.](./media/lp1-mod3-dynamic-group-membership-rule.png)

11. Sélectionnez **Enregistrer**. Le nouveau groupe dynamique inclut désormais les utilisateurs invités B2B, ainsi que les utilisateurs membres.

12. Sur la page « Nouveau groupe », sélectionnez **Créer** pour créer le groupe.

#### Tâche 2 - Vérifier que les membres ont été ajoutés

**Remarque** : l’ajout des membres au groupe dynamique peut prendre jusqu’à 15 minutes.

1. Sélectionnez la **Page d’accueil**`Azure Active Directory`.
2. Lancez **Azure Active Directory**.
3. Dans le menu **Gérer**, sélectionnez **Groupes**.
4. Dans la zone de filtre, saisissez **SC300** pour afficher votre groupe nouvellement créé.
5. Sélectionnez **SC300-myDynamicGroup** pour ouvrir le groupe.
6. Notez qu’il indique qu’il contient plus de 30 **Membres directs*.
7. Dans le menu **Gérer**, sélectionnez **Membres**.
8. Passez en revue les membres.

#### Tâche 3 - Faire des tests avec d’autres règles

1. Essayez de créer un groupe contenant uniquement des utilisateurs **Invités** :

   - (user.objectid -ne null) et (user.userType -eq "Guest")

2. Essayez de créer un groupe avec uniquement des **Membres** des utilisateurs Azure AD.

   - (user.objectid -ne null) et (user.userType -eq "Member")
