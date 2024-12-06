---
lab:
  title: "25\_: Créer des révisions d’accès pour utilisateurs internes et externes"
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 25 : Créer des révisions d’accès pour utilisateurs internes et externes

### Type de connexion = Administrateur Microsoft 365

## Scénario de labo

L’accès privilégié de l’utilisateur doit être régulièrement examiné de manière similaire.° Étant donné qu’il s’agit d’affectations d’accès élevés, l’examen de ces affectations doit être effectué de façon cohérente, comme indiqué par l’entreprise.° Les affectations privilégiées inutilisées et inutiles doivent être supprimées.° La suppression automatisée doit également être configurée pour les utilisateurs qui ne travaillent plus avec l’entreprise ou qui ont changé de service au sein de l’entreprise.

#### Durée estimée : 5 minutes

### Exercice 1 : Créer une révision d’accès interne

#### Tâche : créer une révision d’accès

1. Connectez-vous à la plateforme  [https://entra.microsoft.com](https://entra.microsoft.com) en tant qu’administrateur global.

2. Les révisions d’accès peuvent gérer le cycle de vie des accès.° Dans **Microsoft Entra ID**, recherchez **Gouvernance d’identité**, puis sélectionnez **Révisions d’accès**.

3. Sélectionnez **+ Nouvelle révision d’accès**.

4. Dans la zone **Sélectionner ce qui doit être examiné**, sélectionnez **Équipes + Groupes** à partir de la liste déroulante.

5. Sélectionnez **Équipes + groupes**, puis sélectionnez le groupe **Ventes et Marketing** dans la liste, avant d’appuyer sur **Sélectionner**.

6. Définissez l’**Étendue** sur **Tous les utilisateurs**.

7. Sélectionnez l’option **Suivant : révisions** pour avancer dans l’Assistant.

8. L’étape suivante consiste à déterminer les réviseurs.Ces réviseurs peuvent être eux-mêmes membres pour effectuer une auto-révision ou être attribués aux superviseurs si l’accès à un service entier est examiné. Vous pouvez également définir l’action lorsqu’un réviseur ne répond pas pour supprimer automatiquement cet accès privilégié du membre.

9. Choisissez un réviseur **Alex Wilber** et révisez l’option de périodicité **Annuellement**.  Ensuite, sélectionnez **Suivant : paramètres**.

10. Les paramètres avancés vous permettent d’ajouter un message à la révision.

11. Basculez vers l’onglet **Suivant : examiner et créer** pour finaliser la révision d’accès.

12. Nommez la révision d’accès ** Révision d’accès SC300**.

13. Au bas de la page, sélectionnez **Créer**.

    **Remarque** : lorsque la révision d’accès est créée, la liste de révision d’accès propagera les rôles et les propriétaires des révisions.

14. Les membres qui sont examinés recevront un e-mail lorsque la révision est lancée.

15. La sélection d’une révision d’accès de l’un des rôles fournit l’état de la révision concernée.
