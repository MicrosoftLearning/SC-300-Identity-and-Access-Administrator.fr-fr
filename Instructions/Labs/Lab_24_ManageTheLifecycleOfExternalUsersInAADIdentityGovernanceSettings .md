---
lab:
  title: "24 - Gérer le cycle de vie des utilisateurs externes dans les paramètres d’Azure\_AD\_Identity\_Governance."
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 24 - Gérer le cycle de vie des utilisateurs externes dans les paramètres d’Azure AD Identity Governance  

## Scénario de l’exercice

Vous pouvez sélectionner ce qui se passe lorsqu’un utilisateur externe, qui a été invité à accéder à votre annuaire par le biais d’une demande de package d’accès en cours d’approbation, ne dispose plus d’attributions de package d’accès. Cela peut se produire si l’utilisateur abandonne toutes les attributions de package d’accès, ou si l’attribution de son dernier package d’accès arrive à expiration. Par défaut, lorsqu’un utilisateur externe n’a plus d’attributions de package d’accès, il ne peut pas se connecter à votre annuaire. Au bout de 30 jours, son compte d’utilisateur invité est supprimé de votre annuaire.

#### Durée estimée : 5 minutes

### Exercice 1 - Paramètres d’Azure AD Identity Governance

#### Tâche 1 - Gérer le cycle de vie des utilisateurs externes dans les paramètres d’Azure AD Identity Governance

1. Connectez-vous à  [https://portal.azure.com](https://portal.azure.com) en tant qu’administrateur global.

2. Pour effectuer ces tâches, vous devez disposer d’un compte administrateur général ou administrateur d’utilisateurs.

3. Ouvrez Azure Active Directory et sélectionnez  **Gouvernance des identités**.

4. Dans le menu de navigation de gauche, sous **Gestion des droits d’utilisation**, sélectionnez **Paramètres**.

5. Dans le menu du haut, sélectionnez **Modifier**.

    ![Image de l’écran affichant la page des paramètres de gouvernance des identités avec la mise en surbrillance du cycle de vie des utilisateurs externes.](./media/lp4-mod1-manage-lifcycle-of-ext-users.png)

6. Dans la section **Gérer le cycle de vie des utilisateurs externes**, sélectionnez les différents paramètres pour les utilisateurs externes.

7. Si, lorsqu’un utilisateur externe perd sa dernière attribution aux packages d’accès, vous souhaitez l’empêcher de se connecter à cet annuaire, définissez **Empêcher l'utilisateur externe de se connecter à cet annuaire** sur **Oui**.

8. Si un utilisateur n’est pas autorisé à se connecter à ce répertoire, il ne peut pas redemander le package d’accès ou demander un accès supplémentaire dans ce répertoire. Ne configurez pas le blocage de leur connexion s’ils doivent par la suite demander l’accès à d’autres packages d’accès.

9. Si, lorsqu’un utilisateur externe perd sa dernière attribution aux packages d’accès, vous souhaitez supprimer son compte d’utilisateur invité dans ce répertoire, définissez **Supprimer l’utilisateur externe**  sur **Oui**.

    **Remarque** : la gestion des droits d'utilisation supprime uniquement les comptes qui ont été invités par l’intermédiaire de la gestion des droits d'utilisation. Notez également qu’un utilisateur ne pourra pas se connecter et sera supprimé de ce répertoire, même s’il a été ajouté aux ressources du répertoire qui n’étaient pas des attributions de packages d’accès. Si l’invité était présent dans ce répertoire avant de recevoir des attributions de package d’accès, il sera conservé. Toutefois, s’il a été invité par le biais d’une attribution de package d’accès, et qu’après avoir été invité il a également été affecté à un site OneDrive Entreprise ou SharePoint Online, il sera toujours supprimé.

10. Si vous souhaitez supprimer le compte d’utilisateur invité du répertoire, vous pouvez définir le nombre de jours avant sa suppression. Si vous souhaitez supprimer le compte d’utilisateur invité dès qu’il perd la dernière attribution à un package d’accès, définissez **Nombre de jours avant la suppression de l’utilisateur externe de cet annuaire**  sur **0**.

11. Si vous avez apporté des modifications, sélectionnez **Enregistrer**.
