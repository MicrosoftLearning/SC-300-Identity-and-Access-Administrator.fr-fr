---
lab:
  title: 13 - Implémenter et tester une stratégie d’accès conditionnel
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 13 - Implémenter et tester une stratégie d’accès conditionnel

## Scénario de l’exercice

Votre entreprise doit pouvoir limiter l’accès utilisateur à ses applications internes. Vous devez déployer une stratégie d’accès conditionnel Azure Active Directory.

**Remarque** : pour les stratégies d’accès conditionnel, vous pouvez désactiver les paramètres de sécurité par défaut, les points clés à retenir sont ceux de la formation.  Vous trouverez des informations supplémentaires sur les paramètres de sécurité par défaut en consultant le lien suivant : <https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/concept-fundamentals-security-defaults>

#### Durée estimée : 30 minutes

### Exercice 1 - Définir une stratégie d’accès conditionnel pour empêcher DebraB d’accéder à Yammer

#### Tâche 1 - Confirmer que DebraB a accès à Yammer


1. Lancez une nouvelle fenêtre de navigation privée.
2. Connectez-vous à [https://www.office.com](https://www.office.com) 
3. Lorsque vous y êtes invité, connectez-vous en tant que DebraB :

   | Paramètre | Valeur |
   | :--- | :--- |
   | Nom d’utilisateur | **DebraB@**`<<your lab domain>>.onmicrosoft.com` |
   | Mot de passe | Entrez le mot de passe administrateur du locataire (vous le trouverez dans l’onglet Ressources du labo). |
    
4. Sélectionnez l’icône Yammer pour vous assurer que l’application se lance correctement.

#### Tâche 2 - Créer une stratégie d’accès conditionnel

L’accès conditionnel Azure Active Directory est une fonctionnalité avancée d’Azure AD qui vous permet de spécifier des stratégies détaillées qui contrôlent qui peut accéder à vos ressources. Avec l’accès conditionnel, vous pouvez protéger vos applications en limitant l’accès des utilisateurs en fonction d’éléments tels que les groupes, le type d’appareil, l’emplacement et le rôle.

1. Accédez à l’adresse [https://portal.azure.com](https://portal.azure.com) et connectez-vous à l’aide d’un compte d’administrateur général pour l’annuaire.

2. Ouvrez le menu du portail et sélectionnez  **Azure Active Directory**.

3. Sur la page Azure Active Directory, sous **Gérer**, sélectionnez **Sécurité**.

4. Sur la page Sécurité, dans la navigation gauche, sélectionnez **Accès conditionnel**.

5. Dans l’écran **Vue d’ensemble (préversion),** cliquez sur **+ Créer une stratégie**.

   ![Capture d’écran affichant la page Accès conditionnel avec Nouvelle stratégie mis en surbrillance](./media/lp2-mod1-conditional-access-new-policy.png)

6. Dans la zone **Nom**, saisissez **Bloquer Yammer pour DebraB**.

   **Remarque** : ce nom explicite vous aide à vous souvenir de l’objectif de la stratégie.

7. Sous **Affectations**, sélectionnez **Utilisateurs ou identités de charge de travail**.

8. Dans l’onglet inclure, activez la case à cocher **Utilisateurs et groupes**.

9. Dans le volet Sélectionner, sélectionnez votre compte **DebraB**, puis **Sélectionner**.

10. Sélectionnez **Applications ou actions cloud**.

11. Vérifiez que **Applications cloud** est sélectionné, puis sélectionnez **Sélectionner les applications**.

12. Dans le volet Sélectionner, recherchez **Yammer** et sélectionnez **Office 365 Yammer**, puis **Sélectionner**.

13. Sous **Contrôles d’accès**, sélectionnez **Accorder**.

14. Dans le volet Accorder, sélectionnez **Bloquer l’accès**, puis sélectionnez **Sélectionner**.

   **Remarque** : cette stratégie est configurée pour l’exercice uniquement et est utilisée pour démontrer rapidement une stratégie d’accès conditionnel.

15. Sous **Activer la stratégie**, sélectionnez **Activé**, puis sélectionnez **Créer**.

   ![Image de l’écran affichant une nouvelle stratégie d’accès conditionnel avec les paramètres de stratégie mis en surbrillance](./media/lp2-mod3-create-conditional-access-policy.png)

#### Tâche 3 - Tester la stratégie d’accès conditionnel

Vous devez tester vos stratégies d’accès conditionnel pour vous assurer qu’elles fonctionnent comme prévu.

1. Ouvrez un nouvel onglet de navigation privée et accédez à [https://www.yammer.com/office365](https://www.yammer.com/office365).
    - Lorsque vous y êtes invité, connectez-vous en tant que DebraB :

   | Paramètre | Valeur |
   | :--- | :--- |
   | Nom d’utilisateur | **DebraB@**`<<your lab domain>>.onmicrosoft.com` |
   | Mot de passe | Entrez le mot de passe administrateur du locataire (vous le trouverez dans l’onglet Ressources du labo). |
     
2. Vérifiez que vous ne pouvez pas accéder à Microsoft Yammer.

   ![Image de l’écran affichant un accès aux ressources bloqué en raison d’une stratégie d’accès conditionnel activée](./media/lp2-mod3-test-conditional-access-policy.png)

3. Si vous êtes connecté, fermez l’onglet, attendez 1 minute, puis réessayez.
    
   **Remarque** : si vous êtes connecté automatiquement à Yammer en tant que DebraB, vous devrez vous déconnecter manuellement. Vos informations d’identification/accès ont été mises en cache.  Une fois que vous vous êtes déconnecté et reconnecté, votre session Yammer doit vous refuser l’accès.

4. Fermez l’onglet et revenez à la page Accès conditionnel.

5. Sélectionnez la stratégie **Accès conditionnel à Yammer**.

6. Sous **Activer la stratégie**, sélectionnez **Désactivé**, puis sélectionnez **Enregistrer**.

### Exercice 2 - Tester les stratégies d’accès conditionnel avec « What If »

#### Tâche - Utiliser l’outil What If pour tester les stratégies d’accès conditionnel

1. Ouvrez le menu du portail et sélectionnez  **Azure Active Directory**.

1. Sur la page Azure Active Directory, sous **Gérer**, sélectionnez **Sécurité**.

1. Sur la page Sécurité, dans la navigation gauche, sélectionnez **Accès conditionnel**.

1. Dans le volet de navigation, cliquez sur **Stratégies**.

1. Sélectionnez **What If**.

1. Sous **Identité des utilisateurs ou des charges de travail**, sélectionnez **Aucun utilisateur ou principal de service sélectionné**.

1. Choisissez **DebraB** en tant qu’utilisateur.

1. Sous **Applications cloud, actions ou contexte d’authentification**, sélectionnez **Yammer**. 

1. Sélectionnez **What If**. Vous recevrez un rapport au bas de la vignette détaillant les **Stratégies qui s’appliqueront** et les **Stratégies qui ne s’appliqueront pas**.

Cela vous permet de tester les stratégies et leur efficacité avant de les activer.

### Exercice 3 - Configurer des contrôles de fréquence de connexion à l’aide d’une stratégie d’accès conditionnel

#### Tâche - Configurer l’accès conditionnel dans le portail Azure

Dans le cadre de la configuration de sécurité globale de votre entreprise, vous devez tester une stratégie d’accès conditionnel qui peut être utilisée pour contrôler la fréquence de connexion

1. Accédez à l’adresse [https://portal.azure.com](https://portal.azure.com) et connectez-vous à l’aide d’un compte d’administrateur général pour l’annuaire.

2. Ouvrez le menu du portail et sélectionnez  **Azure Active Directory**.

3. Sur la page Azure Active Directory, sous **Gérer**, sélectionnez **Sécurité**.

4. Sur la page Sécurité, dans la navigation gauche, sélectionnez **Accès conditionnel**.

5. Dans le menu supérieur, dans le menu déroulant **+ Nouvelle stratégie**, sélectionnez **Créer une stratégie**.

   ![Capture d’écran affichant la page Accès conditionnel avec Nouvelle stratégie mis en surbrillance](./media/lp2-mod1-conditional-access-new-policy.png)

6. Dans la zone **Nom**, entrez **Fréquence de connexion**.

7. Sous **Affectations**, sélectionnez **Utilisateurs ou identités de charge de travail**.

8. Dans l’onglet inclure, activez la case à cocher **Utilisateurs et groupes**.

9. Dans le volet Sélectionner, sélectionnez votre compte **Grady Archie**, puis **Sélectionner**.

10. Sélectionnez **Applications ou actions cloud**.

11. Vérifiez que **Applications cloud** est sélectionné, puis sélectionnez **Sélectionner les applications**.

12. Dans le volet Sélectionner, sélectionnez **Office 365**, puis sélectionnez **Sélectionner**.

13. Sous **Contrôles d’accès**, sélectionnez **Session**.

14. Dans le volet Session, sélectionnez **Fréquence de connexion**.

15. Dans la zone de valeur, entrez **30**.

16. Sélectionnez le menu des unités, sélectionnez **Jours**, puis sélectionnez **Sélectionner**.

17. Sous **Activer la stratégie**, sélectionnez **Rapport uniquement**, puis sélectionnez **Créer**.

   ![Image de l’écran affichant une nouvelle stratégie d’accès conditionnel avec les paramètres de stratégie mis en surbrillance](./media/lp2-mod3-create-session-conditional-access-policy.png)

   **REMARQUE** : le mode rapport seul est un nouvel état de la stratégie d’accès conditionnel qui permet aux administrateurs d’évaluer l’impact des stratégies d’accès conditionnel avant de les activer dans leur environnement. Avec la mise en production du mode rapport seul :
    
- Il est possible d’activer les stratégies d’accès conditionnel en mode rapport seul.
- Lors de la connexion, les stratégies en mode rapport seul sont évaluées, mais non appliquées.
- Les résultats sont journalisés dans les onglets Accès conditionnel et Rapport seul des détails du journal de connexion.
- Les clients disposant d’un abonnement Azure Monitor peuvent surveiller l’impact de leurs stratégies d’accès conditionnel dans le classeur Insights sur l’accès conditionnel.
