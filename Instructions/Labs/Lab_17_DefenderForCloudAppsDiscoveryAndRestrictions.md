---
lab:
  title: "17\_: Détection de l’application Defender pour le cloud et gestion des restrictions"
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# Labo 17 : Détection de l’application Defender pour le cloud et gestion des restrictions

## Scénario de l’exercice

Microsoft Defender for Cloud Apps exploite les journaux du trafic réseau pour identifier les applications auxquelles les utilisateurs accèdent.° Les journaux de trafic provenant de pare-feu locaux fournissent un rapport d’instantané sur les applications les plus courantes et les utilisateurs qui y accèdent.° Le trafic provenant d’appareils gérés sera transmis dans le tableau de bord vue d’ensemble de la détection des applications Microsoft Defender for Cloud Apps

#### Durée estimée : 10 minutes

### Exercice 1 : Détection de Defender for Cloud Apps

#### Tâche 1 : Applications de détection dans Defender for Cloud Apps

1. Connectez-vous [https://security.microsoft.com](https://security.microsoft.com) en utilisant un compte d’administrateur général.

1. Dans le menu de gauche, faites défiler jusqu’au titre **Cloud Apps**, puis cliquez sur **Catalogue d’applications cloud**.

1. Dans le volet **Parcourir par catégorie**, sélectionnez **Stockage cloud**.

1. Dans la liste des applications, notez le **score de risque** en regard du nom de l’application.  

1. Ouvrez un autre onglet de navigateur et accédez à **www.dropbox.com**.

1. Vous pourrez accéder à ce site web.


#### Tâche 2 : restreindre des applications dans Defender for Cloud Apps

1. Revenez aux **Applications détectées** et basculez la vignette sur **non approuvée** pour Dropbox.  **Remarque** : cette option se trouve en regard du cercle coché.

1. Cliquez sur **Enregistrer**.

1. Ce processus vous permet de bloquer les applications qui ne sont pas approuvées dans votre stratégie d’entreprise, limitant l’informatique fantôme au sein de votre organisation.

**Remarque** : il existe un délai entre l’annulation de l’approbation d’une application et le blocage de l’application.

Une fois l’application bloquée comme non approuvée, l’application ne sera plus accessible via le navigateur, le navigateur privé ou le téléchargement du magasin sur un client intégré à MDE (Microsoft Defender pour point de terminaison) intégré à Microsoft Defender for Cloud Apps.



