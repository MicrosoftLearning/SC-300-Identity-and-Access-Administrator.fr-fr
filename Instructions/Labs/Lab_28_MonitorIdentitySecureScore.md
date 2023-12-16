---
lab:
  title: 28 - Surveiller et gérer votre posture de sécurité avec le score d’identité sécurisée
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 28 - Surveiller et gérer votre posture de sécurité avec le score d’identité sécurisée

## Scénario de l’exercice

Azure AD Identity Protection assure la détection et la correction automatisées des risques liés à l’identité et fournit des données dans le portail pour examiner les risques potentiels. Azure AD Identity Protection fournit également un score de sécurisation des identités pour surveiller et améliorer votre posture de sécurité des identités.  A l’instar de Microsoft 365 Defender et Microsoft Defender pour le cloud, le score d’identité sécurisée fournit des actions d’amélioration et des recommandations qui peuvent améliorer votre posture de sécurité globale pour l’identité dans Azure Active Directory.  Ce labo est consacré à cette fonctionnalité. 

#### Durée estimée : 15 minutes

### Exercice 1 - Utilisation du score d’identité sécurisée pour surveiller et gérer la posture de sécurité des identités

#### Tâche 1 - Passer en revue le score d’identité sécurisée et les actions d’amélioration

1. Connectez-vous à  [https://portal.azure.com](https://portal.azure.com) en tant qu’administrateur global.

2. Recherchez et sélectionnez **Azure AD Identity Protection**.

3. Dans la vignette **Vue d’ensemble**, vous trouverez **Score d’identité sécurisée**.

4. Sélectionnez **Score d’identité sécurisée**.  Vous accédez ensuite au tableau de bord Score d’identité sécurisée.

5. Faites défiler vers le bas pour afficher les **actions d’amélioration**.

6. Contrairement aux actions d’amélioration dans Microsoft Defender pour le cloud et Microsoft 365 Defender, ces actions d’amélioration sont spécifiques à l’identité.  Vous y trouverez une liste plus ciblée d’actions potentielles pour votre gestion de la posture de sécurité.  Toutes les actions d’amélioration lancées à partir de cette liste auront également un impact sur votre posture globale de sécurité des locataires. 

#### Tâche 2 - Exécuter une action d’amélioration

1. Pour améliorer un domaine de la posture de sécurité des identités, sélectionnez **Protéger tous les utilisateurs avec une stratégie de connexion à risque**.

2. Dans la vignette qui s’ouvre, faites défiler vers le bas et sélectionnez **Commencer**.

3. Un nouvel onglet s’affiche, **Identity Protection | Stratégie de connexion à risque**.

4. Sélectionnez **Tous les utilisateurs** sous **Affectations**.

5. Sous **Risque de connexion**, sélectionnez **Moyen et supérieur**.

6. Sous **Contrôles**, sélectionnez **Accorder** - **Exiger une authentification multifacteur**.

7. **Activez** l’**Application de la stratégie** (si ce n’est pas déjà fait), puis sélectionnez **Enregistrer**.

8. Vous avez créé une stratégie de connexion à risque qui doit maintenant augmenter votre score d’identité sécurisée.  Cette opération peut prendre jusqu’à 24 heures avant d’être prise en compte dans votre score d’identité sécurisée.

9. Passez en revue les autres actions d’amélioration et les étapes à suivre pour les créer et les activer.
