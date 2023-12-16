---
lab:
  title: 25 - Créer des révisions d’accès pour utilisateurs internes et externes
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 25 - Créer des révisions d’accès pour utilisateurs internes et externes  

## Scénario de l’exercice

Les accès utilisateur avec élévation de privilèges doivent être régulièrement examinés selon les mêmes modalités.Comme il s’agit d’affectations d’accès avec élévation de privilèges, elles doivent faire l’objet d’un examen régulier, conformément aux procédures de l’entreprise.Les affectations avec élévation de privilèges qui ne sont plus utilisées et sont inutiles doivent être supprimées.La suppression automatique doit également être configurée pour les utilisateurs qui ne font plus partie de l’entreprise ou qui ont changé de service au sein de l’entreprise.

#### Durée estimée : 5 minutes

### Exercice 1 - Créer une révision d’accès interne

#### Tâche - Créer une révision d’accès

1. Connectez-vous à  [https://portal.azure.com](https://portal.azure.com) en tant qu’administrateur global.

2. Les révisions d’accès permettent de gérer le cycle de vie des accès.Dans **Azure Active Directory**, recherchez **Gouvernance des identités** dans le menu **Gérer**.  Dans **Identity Goverance**, sélectionnez **Révisions d’accès**.

3. Sélectionnez **+ Nouvelle révision d’accès**.

4. Dans la zone **Sélectionner le contenu à réviser**, sélectionnez **Équipes + groupes** dans la liste déroulante.

5. Sélectionnez **Sélectionner des équipes + groupes**, sélectionnez le groupe **Sales and Marketing** dans la liste, puis appuyez sur **Sélectionner**.

6. Définissez l’option **Étendue** sur **Tous les utilisateurs**.

7. Sélectionnez l’option **Révisions** pour continuer dans l’Assistant.

8. L’étape suivante consiste à déterminer les réviseurs.Ces réviseurs peuvent être des membres qui effectuent une auto-révision ou être attribués à des superviseurs en cas de révision de l’accès pour un service entier. Vous pouvez également définir l’action à prendre quand un réviseur ne répond pas pour supprimer automatiquement cet accès privilégié du membre.

9. Choisissez un réviseur et une option de récurrence de la révision.  Sélectionnez ensuite **Paramètres**.

10. Les paramètres avancés vous permettent de placer un message dans le cadre de la révision.

11. Sélectionnez **Vérifier et créer** pour finaliser la révision d’accès.

12. Nommez la révision d’accès **SC300 Access Review Test**.

13. Sélectionnez **Créer**. Une fois la révision d’accès créée, la liste de révisions d’accès est renseignée avec les rôles et les propriétaires des révisions.

14. Les membres qui font l’objet d’une révision reçoivent un e-mail au lancement de la révision.

15. La sélection d’une révision d’accès de l’un des rôles fournit l’état de ces révisions d’accès.
