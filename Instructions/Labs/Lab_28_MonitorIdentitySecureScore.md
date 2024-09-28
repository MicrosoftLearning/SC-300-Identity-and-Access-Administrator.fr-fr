---
lab:
  title: "28\_: Surveiller et gérer la posture de sécurité avec le score d’identité sécurisée"
  learning path: '04'
  module: Module 04 - Plan and Implement and Identity Governance Strategy
---

# Labo 28 : Surveiller et gérer la posture de sécurité avec le score d’identité sécurisée

## Scénario de labo

Microsoft Entra Identity Protection fournit une détection et une correction automatisées des risques basés sur l’identité ainsi que des données dans le portail pour examiner les risques potentiels. Protection des ID Microsoft Entra fournit également un score d’identité sécurisée pour surveiller et améliorer votre posture des identités.  De la même manière que Microsoft Defender XDR et Microsoft Defender pour le cloud, le score d’identité sécurisée offre des suggestions et des actions d’amélioration qui peuvent améliorer votre posture de sécurité globale pour les identités dans Microsoft Entra ID.  Ce labo va vous permettre d’explorer cette fonctionnalité. 

#### Durée estimée : 15 minutes

### Exercice 1 : Utilisation du score de sécurisation des identités pour surveiller et gérer la posture de sécurité des identités

#### Tâche 1 : passer en revue le score sécurisé des identités et les actions d’amélioration

1. Connectez-vous à la plateforme  [https://entra.microsoft.com](https://entra.microsoft.com) en tant qu’administrateur global.

2. Ouvrez le **menu Protection** et sélectionnez **Score d’identité sécurisée**

3. Dans la vignette **Vue d’ensemble**, vous trouverez **Score d’identité sécurisée**.

4. Sélectionnez **Score d’identité sécurisée**.  Vous accédez ainsi au tableau de bord Score d’identité sécurisée.

5. Faites défiler vers le bas pour afficher les **actions d’amélioration**.

6. Contrairement aux actions d’amélioration dans Microsoft Defender pour le cloud et Microsoft Defender XDR, ces actions d’amélioration sont spécifiques aux identités.  Cet outil fournit une liste plus ciblée d’actions potentielles pour votre gestion de la posture de sécurité.  Toutes les actions d’amélioration lancées à partir de cette liste agissent également sur votre posture globale de sécurité des locataires. 

#### Tâche 2 : exécuter une action d’amélioration

1. Pour améliorer un domaine de la posture de sécurité des identités, sélectionnez **Activer les stratégies de risque de connexion à Microsoft Entra ID Identity Protection**.

2. Dans la vignette qui s’ouvre, faites défiler vers le bas et sélectionnez **Démarrer**.

3. Un nouvel onglet s’ouvre pour **l’accès conditionnel**.
 **Remarque :** par défaut, le bouton « Démarrer » s’ouvre dans le Portail Azure. Vous pouvez utiliser le portail ou revenir au Centre d’administration Entra. Chacune de ces deux options fonctionnera.

4. Sélectionnez **+ Nouvelle stratégie**.

5. Donnez un nom à votre stratégie. Nous recommandons aux organisations de créer une norme explicite pour les noms de leurs stratégies.

6. Sous Affectations, sélectionnez Utilisateurs ou identités de charge de travail.

7. Sous Inclure, sélectionnez Tous les utilisateurs.

8. Sous Exclure, sélectionnez Utilisateurs et groupes, puis choisissez les comptes qui doivent conserver la possibilité d’utiliser l’authentification héritée. Microsoft vous recommande d’exclure au moins un compte pour que vous ne soyez pas verrouillé.

9. Sous Ressources cibles > Applications cloud > Inclure, sélectionnez Toutes les applications cloud.

10. Sous Conditions > Applications clientes, définissez Configurer sur Oui.
 - Cochez uniquement les cases Clients Exchange ActiveSync et Autres clients.

11. Cliquez sur Terminé.

12. Sous Contrôles d’accès > Accorder, sélectionnez Bloquer l’accès.

13. Sélectionnez Sélectionner.

14. Confirmez vos paramètres et définissez Activer la stratégie sur Rapport seul.

15. Sélectionnez Créer pour créer votre stratégie.
