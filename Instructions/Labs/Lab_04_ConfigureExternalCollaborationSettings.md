---
lab:
  title: "04\_: Configurer les paramètres de collaboration externe"
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Labo 04 : Configurer les paramètres de collaboration externe

### Type de connexion = Administrateur Microsoft 365

## Scénario de labo

Vous devez activer les paramètres de collaboration externe pour votre organisation afin de permettre l’accès aux invités approuvés.

#### Durée estimée : 5 minutes

### Exercice 1 : Autoriser les utilisateurs invités à accéder à votre organisation

#### Tâche 1 : permettre aux utilisateurs invités d’effectuer une inscription en libre-service

1. Connectez-vous à la plateforme  [https://entra.microsoft.com](https://entra.microsoft.com) en tant qu’administrateur client.
2. Sélectionnez **Identité**, puis **Utilisateurs**.
3. Ouvrez l’élément de menu **Tous les utilisateurs**, puis sélectionnez **Paramètres utilisateur**.
4. Sélectionnez **Gérer les paramètres de collaboration externe**.
5. Vérifiez que **OUI** est sélectionné pour le paramètre **Activer l’inscription en libre-service invité via des flux utilisateur**.
6. Sélectionnez **Enregistrer** en haut de l’écran.

#### Tâche 2 : configurer les paramètres de collaboration externe

1. Connectez-vous à la plateforme  [https://entra.microsoft.com](https://entra.microsoft.com) en tant qu’administrateur client.
2. Sélectionnez  **Identité**.
3. Sélectionnez  **Identités externes**, puis cliquez sur le bouton **Tous les fournisseurs d’identité**.
4. Sélectionnez l’élément **Code secret à usage unique par e-mail** dans la liste des fournisseurs, puis sélectionnez **Configuré**.

    **Remarque** : un code secret unique est un moyen très sécurisé d’inviter un utilisateur à rejoindre votre organisation.
    
5. Vérifiez que l’option **Oui** est sélectionnée.
6. Si nécessaire, sélectionnez **Enregistrer**.
7. Revenez au menu **Identités externes**.
8. Sélectionnez à présent **Paramètres de collaboration externe** dans le menu de gauche.

9. Sous  **Accès utilisateur invité**, passez en revue les niveaux d’accès disponibles, puis sélectionnez **L’accès des utilisateurs invités est limité aux propriétés et aux membres de leurs propres objets de répertoire (le plus restrictif)**.

    **REMARQUE**
    - Les utilisateurs invités ont le même accès que les membres (le plus inclusif) : Cette option donne aux invités le même accès aux ressources Microsoft Entra et aux données d’annuaire que les utilisateurs membres.
    - Les utilisateurs invités ont un accès limité aux propriétés et aux appartenances des objets d’annuaire : Ce paramètre empêche les invités d’effectuer certaines tâches d’annuaire, telles que l’énumération d’utilisateurs, de groupes ou d’autres ressources de répertoire. Les invités peuvent voir l’appartenance de tous les groupes non masqués.
    - L’accès utilisateur invité est limité aux propriétés et aux abonnements de ses propres objets d’annuaire (le plus restrictif) : avec ce paramètre, les invités ne peuvent accéder qu’à leurs propres profils. Les invités ne sont pas autorisés à voir les profils, groupes ou appartenances à des groupes d’autres utilisateurs.

    ![Capture d’écran affichant les options de restriction d’accès des utilisateurs invités](./media/lp1-mod3-guest-user-access-restrictions.png)

10. Sous  **Paramètres d’invitation d’invité**, sélectionnez **Les utilisateurs membres et les utilisateurs affectés à des rôles d’administrateur spécifiques peuvent inviter des utilisateurs invités, y compris ceux disposant d’autorisations de membre**.

    **REMARQUE**
    - Toute personne de l’organisation peut inviter des utilisateurs invités, y compris des invités et des non administrateurs (option la plus inclusive) : Pour permettre aux invités de l’organisation d’inviter d’autres invités, y compris ceux qui ne sont pas membres de l’organisation, sélectionnez cette case d’option.
    - Les utilisateurs membres et les utilisateurs affectés à des rôles Administrateur spécifiques peuvent inviter des utilisateurs invités, y compris des invités disposant d’autorisations de membre : Pour permettre aux utilisateurs membres et aux utilisateurs ayant des rôles Administrateur spécifiques d’inviter des invités, sélectionnez cette case d’option.
    - Seuls les utilisateurs affectés à des rôles Administrateur spécifiques peuvent inviter des utilisateurs invités : Pour autoriser uniquement les utilisateurs ayant des rôles Administrateur à inviter des invités, sélectionnez cette case d’option. Les rôles Administrateur sont les suivants : Administrateur général, Administrateur d’utilisateurs et Inviteur.
    - Aucune personne dans l’organisation ne peut inviter des utilisateurs invités, y compris les administrateurs (option la plus restrictive) : Pour empêcher toute personne dans l’organisation d’inviter des invités, sélectionnez cette case d’option.
    - Si Les membres peuvent inviter est défini sur Non et Les administrateurs et utilisateurs ayant le rôle d’Inviteur d’invités peuvent inviter est défini sur Oui, les utilisateurs du rôle Inviteur d’invité pourront toujours inviter des utilisateurs.

    ![Capture d’écran affichant les paramètres d’invitation « Les invités peuvent inviter » sur « Non » mis en surbrillance](./media/lp1-mod3-guest-user-invite-settings.png)

11. Sous  **Restrictions de collaboration**, passez en revue les options disponibles et appliquez les paramètres par défaut.

    **IMPORTANT**
    - Vous pouvez créer une liste verte ou une liste d’exclusion. Vous ne pouvez pas configurer les deux types de listes. Par défaut, les domaines qui ne sont pas dans la liste verte sont dans la liste d’exclusion et vice versa.
    - Vous ne pouvez créer qu’une seule stratégie par organisation. Vous pouvez mettre à jour la stratégie pour inclure plusieurs domaines ou vous pouvez supprimer la stratégie pour en créer une nouvelle.
    - Le nombre de domaines que vous pouvez ajouter à une liste verte ou à une liste d’exclusion n’est limité que par la taille de la stratégie. La taille maximale de la stratégie entière est de 25 Ko (25 000 caractères). Elle comprend la liste verte ou la liste d’exclusion et tous les autres paramètres configurés pour d’autres fonctionnalités.
    - Cette liste fonctionne indépendamment à partir des listes vertes/rouges OneDrive Entreprise et SharePoint Online. Si vous souhaitez restreindre le partage des fichiers individuels dans SharePoint Online, vous devez configurer une liste verte ou d’exclusion pour OneDrive Entreprise et SharePoint Online.
    - La liste ne s’applique pas aux utilisateurs externes qui ont déjà utilisé l’invitation. La liste est appliquée une fois configurée. Si une invitation de l’utilisateur est en attente et que vous définissez une stratégie qui bloque leur domaine, la tentative de l’utilisateur d’utiliser l’invitation échoue.

12. Quand vous avez terminé, **enregistrez** les modifications.
