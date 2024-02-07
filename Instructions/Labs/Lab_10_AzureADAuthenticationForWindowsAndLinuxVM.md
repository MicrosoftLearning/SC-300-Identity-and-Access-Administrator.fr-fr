---
lab:
  title: "10\_: Authentification Azure\_AD pour machines virtuelles Windows et Linux"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 10 : Authentification Microsoft Entra pour machines virtuelles Windows et Linux

**Remarque** : ce labo nécessite un Pass Azure. Consultez le labo 00 pour obtenir des instructions.

## Scénario de l’exercice

L’entreprise a décidé qu’Azure Active Directory devait être utilisé pour se connecter aux machines virtuelles pour l’accès à distance.  Ce labo explique comment cela peut être configuré pour les machines virtuelles Windows et Linux.

#### Durée estimée : 30 minutes

### Exercice 1 : Connexion aux machines virtuelles Windows dans Azure avec Azure AD

#### Tâche 1 : créer une machine virtuelle Windows avec la connexion Azure AD activée

1. Accéder : [https://portal.azure.com](https://portal.azure.com)

1. Sélectionnez **+ Créer une ressource**.

1. Saisissez **Windows 11** dans la barre de recherche de la Place de marché.

1. Dans le champ **Windows 11**, sélectionnez **Windows 11 Entreprise 22H2** dans la liste déroulante Sélectionner un plan logiciel.

1. Depuis l’onglet Informations de base, vous devrez créer un nom d’utilisateur et un mot de passe Administrateur pour la machine virtuelle.
   - Utilisez un nom d’utilisateur que vous devrez mémoriser et un mot de passe sécurisé.

1. Sous l’onglet **Gestion**, cochez la case à côté de **Se connecter avec Azure AD** sous la section Azure AD.

    REMARQUE : depuis le 11/01/2023, cette interface utilisateur n’a pas été mise à jour pour afficher l’ID Microsoft Entra. Elle fait toujours référence à Azure AD.

    REMARQUE 2 : vous remarquerez que la case de l’**identité managée affectée par le système** sous la section Identité est automatiquement cochée et grisée. Cette action doit se produire automatiquement une fois que vous avez activé la connexion avec Azure AD.

1. Parcourez le reste de l’expérience de création d’une machine virtuelle. 

1. Sélectionnez Créer.

#### Tâche 2 : connexion Azure AD pour les machines virtuelles Azure existantes

1. Accédez à **Machines Virtuelles** dans le [https://portal.azure.com](https://portal.azure.com).

1. Sélectionnez la machine virtuelle nouvellement créée lors de la tâche 1.

1. Sélectionnez **Contrôle d’accès (IAM)** .

1. Sélectionnez **+ Ajouter **, puis **Ajouter une attribution de rôle** pour ouvrir la page Ajouter une attribution de rôle.

1. Configurez les paramètres suivants :
    - **Type d’affectation** : rôles de fonction de poste
    - **Rôle** : connexion de l’administrateur aux machines virtuelles
    - **Membres** : choisissez un utilisateur, un groupe ou un principal de service.  Utilisez ensuite **+ Sélectionner des membres** pour ajouter **Joni Sherman** en tant qu’utilisateur spécifique pour la machine virtuelle.

1. Sélectionnez **Vérifier + Affecter** pour terminer le processus.

#### Tâche 3 : mettre à jour la machine virtuelle du serveur pour prendre en charge la connexion à Azure AD

1. Sélectionnez l’élément de menu **Connect**.

1. Sous l’onglet **RDP**, sélectionnez **Télécharger le fichier RDP**.  Si vous y êtes invité(e), choisissez l’option **Conserver** pour le fichier.  Il sera enregistré dans votre dossier Téléchargements.

1. Ouvrez le dossier **Téléchargements** dans le Gestionnaire de fichiers.

1. Ouvrez le protocole RDP.

1. Choisissez de vous connecter en tant qu’autre utilisateur.

1. Utilisez le nom d’utilisateur et le mot de passe d’administrateur que vous créez lors de la configuration de la machine virtuelle.
   - Si vous y êtes invité(e), autorisez l’accès à la machine virtuelle ou à la session RDP.

1. Attendez que le serveur soit ouvert et que tous les logiciels se chargent, comme le tableau de bord Gestionnaire de serveur.

1. Sélectionnez le bouton **Démarrer** dans la machine virtuelle.

1. Tapez **Panneau de contrôle** et lancez l’application associée.

1. Sélectionnez **Système et sécurité** dans la liste des paramètres.

1. Dans le paramètre **Système**, sélectionnez l’option **Autoriser l’accès à distance**.

1. En bas de la boîte de dialogue qui s’ouvre, vous verrez une section **Bureau à distance**.

1. **Décochez** la case **Autoriser des connexions uniquement sur les ordinateurs qui exécutent le Bureau à distance avec authentification au niveau du réseau**.

1. Sélectionnez **Apply** (Appliquer), puis **OK**.

1. **Quittez** la session RDP à la machine virtuelle


#### Tâche 4 : modifier le fichier RDP pour prendre en charge la connexion à Azure AD

1. Ouvrez le dossier **Téléchargements** dans le Gestionnaire de fichiers.

1. **Effectuez une copie** du fichier RDP et ajoutez **-AzureAD** à la fin du nom de fichier.

1. Modifiez la nouvelle version du fichier RDP que vous venez de copier à l’aide de Bloc-notes Windows. Ajoutez les deux lignes de texte suivantes au bas du fichier :
     ```
        enablecredsspsupport:i:0
        authentication level:i:2
     ```
 
 1. **Enregistrez** le fichier RDP.  Vous devez maintenant avoir deux versions du fichier :
      - <<virtual machine name>> RDP
      - <<virtual machine name>>-AzureAD.RDP

#### Tâche 5 : se connecter au Centre de données Windows Server 2022 à l’aide de la connexion Azure AD

1. Ouvrez **<<virtual machine name>>-AzureAD.RDP

1. Sélectionnez l’icône **Se connecter** lorsque la boîte de dialogue s’ouvre.

1. Au lieu de recevoir l’invite sur le compte d’utilisateur à connecter, vous devez recevoir un message indiquant si vous souhaitez vous connecter à l’ordinateur distant.

1. Sélectionnez **Oui** en bas de l’écran.

1. La session Bureau à distance doit s’ouvrir et afficher l’écran de connexion à Windows Server.  **Autre utilisateur** avec un bouton OK doit s’afficher.

1. Sélectionnez **OK**.

1. Dans la boîte de dialogue de connexion, saisissez les informations suivantes :
   - Nom d’utilisateur : **AzureAD\JoniS@<<your lab domainname>>
   - Mot de passe : entrez le mot de passe d’administrateur communiqué par votre fournisseur d’hébergement de labo.

   REMARQUE : JoniS est l’utilisateur que nous avons autorisé à se connecter en tant qu’administrateur pendant la tâche 1.

1. Windows Server doit confirmer la connexion et s’ouvrir au tableau de bord Gestionnaire de serveur normal.

#### Tâche 6 : test facultatif pour explorer la connexion Azure AD

1. Vérifiez que JoniS était le seul utilisateur ajouté au groupe Administrateurs.

1. Dans le tableau de bord Gestionnaire de serveur, sélectionnez le menu **Outils** en haut à gauche.

1. Lancez l’outil **Gestion des ordinateurs**.

1. Ouvrez **utilisateurs et groupes locaux**, puis accédez à **Groupes, Administrateurs**.

1. Vous devriez voir **Azure\JoniSherman…** dans la liste.

1. Vérifiez si d’autres membres Azure AD peuvent se connecter.

1. Quittez la session Bureau à distance.

1. Lancez à nouveau le fichier **<<server name>>-AzureAD.RDP**.

1. Essayez de vous connecter en tant qu’autres membres Azure AD comme AdeleV ou AlexW ou DiegoS.

1. Vous devez remarquer que ces utilisateurs n’ont pas accès.

### Exercice 2 : Connexion aux machines virtuelles Linux dans Azure avec Azure AD

#### Tâche 1 : créer une machine virtuelle Linux avec une identité managée affectée par le système

1. Accéder : [https://portal.azure.com](https://portal.azure.com)

1. Sélectionnez **+ Créer une ressource**.

1. Sélectionnez **Créer** sous **Ubuntu Server 18.04 LTS** dans la vue Populaire.

1. Sur l’onglet **Gestion**, cochez la case pour activer **Connexion avec Azure Active Directory (préversion)**.

1. Vérifiez que l’option **Identité managée affectée par le système** est cochée.

1. Parcourez le reste de l’expérience de création d’une machine virtuelle. Dans cette préversion, vous devrez créer un compte administrateur avec un nom d’utilisateur et un mot de passe ou une clé publique SSH.

#### Tâche 2 : connexion Azure AD pour les machines virtuelles Azure existantes

1. Accédez à **Machines Virtuelles** dans le [https://portal.azure.com](https://portal.azure.com).

1. Sélectionnez **Contrôle d’accès (IAM)** .

1. Sélectionnez Ajouter  Ajouter une attribution de rôle pour ouvrir la page Ajouter une attribution de rôle.

1. Attribuez le rôle suivant. 
    - **Rôle** : Connexion de l’administrateur aux machines virtuelles ou Connexion de l’utilisateur aux machines virtuelles
    - **Attribuer l’accès à** : Un utilisateur, à un groupe, à un principal de service ou à une identité managée.

1. Pour connaître les étapes détaillées, consultez Attribuer des rôles Azure à l’aide du portail Azure.
