---
lab:
  title: "28\_: Surveiller et gérer la posture de sécurité avec le score d’identité sécurisée"
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 28 : Surveiller et gérer la posture de sécurité avec le score d’identité sécurisée

## Scénario de l’exercice

Microsoft Entra Identity Protection fournit une détection et une correction automatisées des risques basés sur l’identité ainsi que des données dans le portail pour examiner les risques potentiels. Microsoft Entra Identity Protection fournit également un score de sécurité des identités pour surveiller et améliorer votre posture de sécurité des identités.  De la même manière que Microsoft 365 Defender et Microsoft Defender pour le cloud, le score d’identité sécurisée fournit des actions d’amélioration et des recommandations qui peuvent améliorer votre posture de sécurité globale pour l’identité dans Microsoft Entra ID.  Ce labo va vous permettre d’explorer cette fonctionnalité. 

#### Durée estimée : 15 minutes

### Exercice 1 : Utilisation du score de sécurisation des identités pour surveiller et gérer la posture de sécurité des identités

#### Tâche 1 : passer en revue le score sécurisé des identités et les actions d’amélioration

1. Connectez-vous à la plateforme  [https://entra.microsoft.com](https://entra.microsoft.com) en tant qu’administrateur global.

2. Ouvrez le **menu Protection** et sélectionnez **Score d’identité sécurisée**

3. Dans la vignette **Vue d’ensemble**, vous trouverez **Score d’identité sécurisée**.

4. Sélectionnez **Score d’identité sécurisée**.  Vous accédez ainsi au tableau de bord Score d’identité sécurisée.

5. Faites défiler vers le bas pour afficher les **actions d’amélioration**.

6. Contrairement aux actions d’amélioration dans Microsoft Defender pour le cloud et Microsoft 365 Defender, ces actions d’amélioration sont spécifiques à l’identité.  Cet outil fournit une liste plus ciblée d’actions potentielles pour votre gestion de la posture de sécurité.  Toutes les actions d’amélioration lancées à partir de cette liste agissent également sur votre posture globale de sécurité des locataires. 

#### Tâche 2 : exécuter une action d’amélioration

1. Pour améliorer une zone de la posture de sécurité des identités, sélectionnez **Protéger tous les utilisateurs avec une stratégie de connexion à risque**.

2. Dans la vignette qui s’ouvre, faites défiler vers le bas et sélectionnez **Démarrer**.

3. Un nouvel onglet s’ouvre pour **Identity Protection | Stratégie de connexion à risque**.

4. Sélectionnez **Tous les utilisateurs** sous **Affectations**.

5. Sous **Connexion à risque**, sélectionnez **Moyen et supérieur**.

6. Sous **Contrôles** - , sélectionnez **Autoriser****Exiger une authentification multifacteur**.

7. **Activez** **Application de la stratégie** (si ce n’est pas déjà fait), puis sélectionnez **Enregistrer**.

8. Vous avez créé une stratégie de connexion à risque qui doit maintenant augmenter votre score d’identité sécurisée.  Cela prendra jusqu’à 24 heures pour que votre score d’identité sécurisée soit affecté.

9. Passez en revue les autres actions d’amélioration et les étapes à suivre pour les créer et les activer.
