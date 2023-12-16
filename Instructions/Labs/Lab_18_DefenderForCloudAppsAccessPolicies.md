---
lab:
  title: 18 - Stratégies d’accès Defender for Cloud Apps
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# 18 - Stratégies d’accès et de session Defender for Cloud Apps

## Scénario de l’exercice

Microsoft Defender for Cloud Apps permet de créer des stratégies d’accès conditionnel supplémentaires spécifiques aux applications cloud surveillées.  Pour créer ces stratégies, accédez au menu Contrôle du portail Microsoft Defender for Cloud Apps.

#### Durée estimée : 20 minutes

### Exercice 1 - Créer et tester la stratégie Contrôle d’application par accès conditionnel

#### Tâche 1 - Confirmer que PradeepG a un accès inconditionnel à FORMS

1. Lancez une nouvelle fenêtre de **navigation privée**.
2. Se connecter à [https://forms.microsoft.com](https://forms.microsoft.com).
3. Sélectionnez l’icône de connexion dans le coin supérieur droit de la page.
4. Connectez-vous en tant que Pradeep Gupta.
   - Nom d’utilisateur = PradeepG@<<<your lab hoster provided domain>>>
   - Mot de passe = le mot de passe de l’onglet Ressources
5. Vérifiez que Microsoft Forms s’ouvre et que vous ne recevez aucun message d’avertissement.
6. Fermez la fenêtre de navigation privée.

#### Tâche 2 - Configurer Azure AD pour utiliser Defender for Cloud Apps

1. À l’adresse [portal.azure.com](portal.azure.com), accédez à Azure Active Directory.

2. Sous **Gérer**, sélectionnez **Sécurité**.

3. Sous **Sécurité**, sélectionnez **Accès conditionnel**.

4. Sélectionnez la liste déroulante **+ Nouvelle stratégie**, puis **Créer une stratégie**.

5. Nommez la stratégie, par exemple **Surveiller Pradeep à l’aide de Forms**.

6. Sous **Utilisateurs ou identités de charge de travail**, sélectionnez **Utilisateurs spécifiques inclus**, puis **Sélectionner Utilisateurs et groupes** et marquez les **Utilisateurs et groupes**.

7. Choisissez le compte **Pradeep Gupta** en tant que locataire labo, puis cliquez sur **Sélectionner**.

8. Sous **Applications ou actions cloud**, sélectionnez **Aucune application cloud, action ni contexte d’authentification sélectionnés**.

9. Cliquez sur **Sélectionner des applications**, choisissez **Microsoft Forms**, puis cliquez sur **Sélectionner**. 

10. Sous **Contrôles d’accès**, sélectionnez **Session** et ** 0 contrôle sélectionné**.

11. Sélectionnez la zone **Utiliser le contrôle d’application par accès conditionnel**, laissez la valeur par défaut **Moniteur uniquement**, puis sélectionnez **Sélectionner**.

12. Sous **Activer la stratégie**, sélectionnez **Activé**, puis **Créer**.

#### Tâche 3 - Se connecter à Forms et vérifier que l’accès conditionnel est en cours de surveillance

1. Lancez une nouvelle fenêtre de **navigation privée**.
2. Se connecter à [https://forms.microsoft.com](https://forms.microsoft.com).
3. Sélectionnez l’icône de connexion dans le coin supérieur droit de la page.
4. Connectez-vous en tant que Pradeep Gupta.
   - Nom d’utilisateur = PradeepG@<<<your lab hoster provided domain>>>
   - Mot de passe = le mot de passe de l’onglet Ressources
5. Vérifiez que Pradeep a accès et que vous recevez lenouveau message suivant :
   - Votre entreprise surveille l’utilisation de cette application.
6. Fermez la fenêtre de navigation privée.

### Exercice 2 - Configurer des alertes dans Microsoft Defender for Cloud Apps

#### Tâche 1 - Accéder à Microsoft Defender for Cloud Apps et créer un contrôle d’application par accès conditionnel

L’inscription de votre application établit une relation d’approbation entre votre application et la plateforme d’identités Microsoft. L’approbation est unidirectionnelle : votre application approuve la plateforme d’identités Microsoft, et non le contraire.

1. Connectez-vous à l’adresse [https://security.microsoft.com](https://security.microsoft.com) à l’aide d’un compte Administrateur général.

1. Dans le menu de gauche, faites défiler vers le bas et sélectionnez **Autres ressources**.

1. Dans la fenêtre **Autres ressources**, recherchez et sélectionnez **Ouvrir** sous **Microsoft Defender for Cloud Apps**.  Vous accédez ainsi au portail **Microsoft Defender for Cloud Apps** dans le compte Microsoft 365.

1. Dans le menu du portail **Microsoft Defender for Cloud Apps**, sélectionnez la flèche déroulante vers **Contrôle** et cliquez sur **Stratégies**.

1. Sélectionnez **+ Créer une stratégie**. Sélectionnez **Stratégie d'accès**.

1. Saisissez un nom pour la stratégie, par exemple **Surveiller l’accès à Microsoft Forms**.

1. Laissez le paramètre **Catégorie** défini sur **Contrôle d’accès**.

1. Sous **Activités correspondant à l’ensemble des éléments suivants**, sélectionnez la liste déroulante pour **Conforme à Intune, jonction Azure AD Hybride** et désélectionnez **Jonction Azure AD Hybride**.

1. Sélectionnez la liste déroulante pour **Sélectionner des applications**.  Sélectionnez **Microsoft Forms**.

1. Laissez **les actions** en tant que **Test**.

1. Sous **Alertes**, laissez la case **Créer une alerte...** activée et sélectionnez **Envoyer l’alerte par e-mail**.

1. Entrez l’adresse e-mail de l’administrateur du labo, puis sélectionnez **Entrée** sur votre clavier.

1. Sélectionnez **Créer** pour créer la stratégie d’accès.

#### Tâche 2 - Se connecter en tant que Pradeep à Forms pour déclencher l’activité

1. Lancez une nouvelle fenêtre de **navigation privée**.
2. Se connecter à [https://forms.microsoft.com](https://forms.microsoft.com).
3. Sélectionnez l’icône de connexion dans le coin supérieur droit de la page.
4. Connectez-vous en tant que Pradeep Gupta.
   - Nom d’utilisateur = PradeepG@<<<your lab hoster provided domain>>>
   - Mot de passe = le mot de passe de l’onglet Ressources
5. Vérifiez que Pradeep a accès et que vous recevez lenouveau message suivant :
   - Votre entreprise surveille l’utilisation de cette application.
6. Fermez la fenêtre de navigation privée.

#### Tâche 3 - Examiner l’activité dans Defender for Cloud Apps

1. Revenez dans le navigateur exécutant Defender for Cloud Apps.
2. Actualisez-le pour vous assurer qu’il affiche les données les plus récentes.
3. Sélectionnez **Journal d’activité** dans le menu **Enquêter**.
4. À l’aide de l’option **App : filtre** choisissez **Microsoft Forms** dans la liste.
5. Consultez les enregistrements de connexion de Pradeep.
