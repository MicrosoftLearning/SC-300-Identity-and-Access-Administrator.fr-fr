---
lab:
  title: "14\_: Activer des stratégies de connexion et de risque utilisateur"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 14 : Activer des stratégies de connexion et de risque utilisateur

### Type de connexion = Administrateur Microsoft 365

## Scénario de labo

Afin de bénéficier d’une couche de sécurité supplémentaire, activez et configurez les stratégies de connexion et de risque utilisateur de votre organisation Microsoft Entra.

#### Durée estimée : 10 minutes


### Exercice 1 : Activer la stratégie de risque utilisateur

#### Tâche 1 : configurer la stratégie

1. Connectez-vous à [https://entra.microsoft.com]( https://entra.microsoft.com) en utilisant un compte d’administrateur général.

2. Ouvrez le menu du portail, puis sélectionnez  **Microsoft Entra ID**.

3. Dans le menu, sous **Identité**, sélectionnez **Protection**.

4. Sur la page Sécurité, dans le volet de navigation de gauche, sélectionnez **Protection de l’identité**.

5. Dans le panneau Protection de l’identité, dans le volet de navigation de gauche, sélectionnez **Stratégie d’utilisateur à risque**.

    ![Image de l’écran affichant la page Stratégie d’utilisateur à risque et le chemin de navigation en surbrillance](./media/lp2-mod4-browse-to-identity-protection.png)

6. Sous **Affectations**, sélectionnez **Tous les utilisateurs** et passez en revue les options disponibles.

7. Vous pouvez sélectionner **Tous les utilisateurs** ou **Sélectionner des personnes et des groupes** si vous limitez votre déploiement.

8. En outre, vous pouvez choisir d’exclure des utilisateurs de la stratégie.

9. Sous **Risque de l’utilisateur**, sélectionnez **Bas et supérieur**.

10. Dans le volet Risque de l’utilisateur, sélectionnez **Élevé**, puis sélectionnez **Terminé**.

11. Sous **Contrôle** > **Accès**, sélectionnez **Bloquer l’accès**.

12. Dans le volet Accès, passez en revue les options disponibles.

    **Conseil** : Microsoft recommande d’autoriser l’accès et à d’exiger la modification du mot de passe.

13. Cochez la case **Nécessite une modification du mot de passe**, puis sélectionnez **Terminer**.

14. Sous **Application de la stratégie**, sélectionnez **Activé**, puis **Enregistrer**.

#### Tâche 2 : activer la stratégie de connexion à risque

1. Sur la page Protection de l’identité, dans le volet de navigation de gauche, sélectionnez **Stratégie d’utilisateur à risque**.

2. Comme pour la stratégie d’utilisateur à risque, la stratégie de connexion à risque peut être assignée aux utilisateurs et aux groupes et vous permet d’exclure des utilisateurs de la stratégie.

3. Sous **Risque de connexion**, sélectionnez **Moyen et supérieur**.

4. Dans le volet Risque de connexion, sélectionnez **Élevé**, puis sélectionnez **Terminé**.

5. Sous **Contrôle** > **Accès**, sélectionnez **Bloquer l’accès**.

6. Activez la case à cocher **Exiger l’authentification multifacteur**, puis sélectionnez **Terminé**.

7. Sous **Application de la stratégie**, sélectionnez **Activé**, puis **Enregistrer**.
