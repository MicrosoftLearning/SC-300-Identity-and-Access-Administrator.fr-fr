---
lab:
  title: "17\_- Découverte d’applications et application de restrictions dans Defender for Cloud Apps"
  learning path: '03'
  module: Module 03 - Implement Access Management for Apps
---

# Labo 17 - Découverte d’applications et application de restrictions dans Defender for Cloud Apps

## Scénario de l’exercice

Microsoft Defender for Cloud Apps utilise les journaux du trafic réseau pour identifier les applications auxquelles les utilisateurs accèdent.Les journaux de trafic provenant de pare-feu locaux fournissent un rapport instantané sur les applications les plus courantes et les utilisateurs qui accèdent à ces applications.Le trafic provenant d’appareils gérés est transmis dans le tableau de bord de vue d’ensemble de la découverte Microsoft Defender for Cloud Apps.

#### Durée estimée : 10 minutes

### Exercice 1 - Découverte dans Defender for Cloud Apps

#### Tâche 1 - Découverte d’applications dans Defender for Cloud Apps

1. Connectez-vous sur [https://security.microsoft.com](https://security.microsoft.com) à l’aide d’un compte Administrateur général.

1. Dans le menu de gauche, faites défiler la page jusqu’au titre **Cloud Apps**, puis cliquez sur **Catalogue d’applications cloud**.

1. Dans le volet **Parcourir par catégorie**, sélectionnez **Stockage cloud**.

1. Dans la liste des applications, notez le **score de risque** en regard du nom de l’application.  

1. Ouvrez un autre onglet de navigateur et accédez à **Dropbox.com**.

1. Vous pouvez accéder à ce site web.


#### Tâche 2 - Restriction d’applications dans Defender for Cloud Apps

1. Revenez à la vignette **Applications découvertes** et sélectionnez l’option **Marquer comme Non approuvé** pour Dropbox.  **Remarque** : cette option se trouve en regard de la case dans un cercle.

1. Cliquez sur **Enregistrer**.

1. Ce processus vous permet de bloquer les applications qui ne sont pas approuvées dans votre stratégie d’entreprise afin de limiter l’informatique fantôme au sein de votre organisation.

**Remarque** : il y a un délai entre l’annulation de l’approbation d’une application et le blocage de l’application.

Une fois bloquée comme non approuvée, l’application n’est pas accessible en téléchargement via le navigateur, le navigateur en mode InPrivate ou le store sur un client intégré à MDE (Microsoft Defender pour point de terminaison) intégré à Microsoft Defender for Cloud Apps.



