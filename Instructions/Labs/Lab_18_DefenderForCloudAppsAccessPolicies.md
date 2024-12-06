---
lab:
  title: "18\_: Stratégies d’accès Defender for Cloud Apps"
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# 18 : Microsoft Defender for Cloud Apps : stratégies d’accès et de session

### Type de connexion = Administrateur Microsoft 365

## Scénario de labo

Microsoft Defender for Cloud Apps nous permet de créer des stratégies d’accès conditionnel supplémentaires spécifiques aux applications cloud que nous surveillons.  La création de ces stratégies peut être effectuée à partir du menu Contrôle dans le portail Microsoft Defender for Cloud Apps.

#### Durée estimée : 20 minutes

### Exercice 1 : Créer et tester la stratégie de contrôle d’accès conditionnel aux applications

#### Tâche 1 : confirmer que PradeepG a un accès inconditionnel à FORMS

1. Ouvrez une nouvelle fenêtre de **navigateur InPrivate**.
2. Se connecter à [https://forms.microsoft.com](https://forms.microsoft.com).
3. Cliquez sur le bouton de connexion situé dans le coin supérieur droit de la page.
4. Connectez-vous en tant que Pradeep Gupta.
   - Nom d’utilisateur : PradeepG@<<<your lab hoster provided domain>>>
   - Mot de passe : le mot de passe de l’onglet Ressources
5. Vérifiez que Microsoft Forms s’ouvre et que vous ne recevez aucun message d’avertissement.
6. Fermez la fenêtre de navigation InPrivate.

#### Tâche 2 : configurer votre Microsoft Entra ID pour qu’il fonctionne avec Microsoft Defender pour les applications cloud.

1. Accédez au [https://entra.microsoft.com](https://entra.microsoft.com), puis à Microsoft Entra ID.

2. Sous **Identité**, sélectionnez **Protection**.

3. Sélectionnez **Accès conditionnel**.

4. Sélectionnez **+ Créer une nouvelle stratégie**.

5. Entrez un nom de stratégie, tel que **Surveiller Pradeep à l’aide de Forms**.

6. Sous l’onglet **Affectations**, sélectionnez **Aucun utilisateur ou groupe**, **Utilisateurs spécifiques inclus**, **Sélectionner des utilisateurs et des groupes**, cochez **0 Utilisateurs et groupes**.

7. Choisissez le compte **Pradeep Gupta** pour le locataire de labo, puis choisissez **Sélectionner**.

8. Sous **Ressources cibles**, sélectionnez **Aucune ressource cible sélectionnée**.

9. Sélectionnez **Sélectionner des applications**, choisissez **Microsoft Forms**, puis **Sélectionner**. 

10. Sous **Contrôles d’accès** sélectionnez **Session**, puis **Aucun contrôle sélectionné**.

11. Sélectionnez la zone **Utiliser le contrôle** d’application d’accès conditionnel, laissez la valeur par défaut **Moniteur uniquement**, puis sélectionnez **Sélectionner**.

12. Sous **Activer la stratégie**, sélectionnez **Activé**, puis sélectionnez **Créer**.

#### Tâche 3 : se connecter à Forms et vérifier que l’accès conditionnel est en cours de surveillance

1. Ouvrez une nouvelle fenêtre de **navigateur InPrivate**.
2. Se connecter à [https://forms.microsoft.com](https://forms.microsoft.com).
3. Cliquez sur le bouton de connexion situé dans le coin supérieur droit de la page.
4. Connectez-vous en tant que Pradeep Gupta.
   - Nom d’utilisateur : PradeepG@<<<your lab hoster provided domain>>>
   - Mot de passe : le mot de passe de l’onglet Ressources
5. Vérifiez que Pradeep a accès et que vous recevez un nouveau message :
   - Votre entreprise surveille l’utilisation de cette application.
6. Fermez la fenêtre de navigation InPrivate.

### Exercice 2 : Configuration des alertes générées dans Microsoft Defender for Cloud Apps

#### Tâche 1 : se protéger avec le contrôle d’application par accès conditionnel Microsoft Defender for Cloud Apps

L’inscription de votre application établit une relation d’approbation entre votre application et la plateforme d’identités Microsoft. L’approbation est unidirectionnelle : votre application approuve la plateforme d’identités Microsoft, et non le contraire.

1. Connectez-vous à [https://security.microsoft.com](https://security.microsoft.com) en utilisant un compte d’administrateur général.

1. Dans le menu de gauche, faites défiler et sélectionnez **Stratégies** dans la section **Applications cloud** du menu de gauche.

1. Dans le menu **Stratégies**, recherchez et sélectionnez **Gestion des stratégies**.

1. Sélectionnez **+ Créer une stratégie**. Sélectionnez **Stratégie d’accès**.

1. Entrez un nom pour la stratégie, par exemple **Surveiller l’accès à Microsoft Forms**.

1. Vous pouvez le paramètre **Catégorie** défini sur **Contrôle d’accès**.

1. Sous **Activités correspondant à tous les éléments suivants**, sélectionnez la liste déroulante pour **Conformité à Intune, Jonction Microsoft Entra Hybride** et désélectionnez **Jonction Microsoft Entra Hybride**.

1. Sélectionnez la liste déroulante pour **Sélectionner des applications**.  Sélectionnez **Microsoft Forms**.

1. Laissez **Actions** défini sur **Test**.

1. Sous **Alertes**, laissez la case **Créer une alerte...** activée et sélectionnez **Envoyer l’alerte par e-mail**.

1. Entrez l’adresse e-mail de l’administrateur du labo, puis sélectionnez **Entrée** sur votre clavier.

1. Sélectionnez **Créer** pour créer la stratégie d’accès.

#### Tâche 2 : se connecter en tant que Pradeep à Forms pour déclencher l’activité

1. Ouvrez une nouvelle fenêtre de **navigateur InPrivate**.
2. Se connecter à [https://forms.microsoft.com](https://forms.microsoft.com).
3. Cliquez sur le bouton de connexion situé dans le coin supérieur droit de la page.
4. Connectez-vous en tant que Pradeep Gupta.
   - Nom d’utilisateur : PradeepG@<<<your lab hoster provided domain>>>
   - Mot de passe : le mot de passe de l’onglet Ressources
5. Vérifiez que Pradeep a accès et que vous recevez un nouveau message :
   - Votre entreprise surveille l’utilisation de cette application.
6. Fermez la fenêtre de navigation InPrivate.

#### Tâche 3 : passer en revue l’activité dans Defender for Cloud Apps

1. Revenez au navigateur exécutant Defender for Cloud Apps.
2. Actualisez le navigateur pour vous assurer que les données les plus récentes sont téléchargées.
3. Sélectionnez **Journal d’activité** dans le menu **Examen**.
4. À l’aide de **Application : filtrer** sélectionnez **Microsoft Forms** dans la liste.
5. Notez les enregistrements d’authentification pour Pradeep.
