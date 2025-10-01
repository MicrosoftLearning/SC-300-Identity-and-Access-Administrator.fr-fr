---
lab:
  title: '10 : authentification Microsoft Entra ID pour des machines virtuelles Windows et Linux'
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 10 : Authentification Microsoft Entra pour machines virtuelles Windows et Linux

### Type de connexion = Connexion à la ressource Azure

## Scénario de labo

La société a décidé que Microsoft Entra ID doit être utilisé pour l’accès à distance afin de se connecter aux machines virtuelles.  Ce labo explique comment cela peut être configuré pour les machines virtuelles Windows et Linux.

#### Durée estimée : 30 minutes

### Exercice 1 : connexion à des machines virtuelles Windows dans Azure avec Microsoft Entra ID

#### Tâche 1 : créer une machine virtuelle Windows avec une connexion Microsoft Entra ID activée

1. Accéder : [https://portal.azure.com](https://portal.azure.com)

**Conseil de labo** : si vous êtes invité à enregistrer vos informations d’identification, choisissez Jamais.  Et annulez la visite guidée, sauf s’il s’agit de la première fois que vous utilisez le portail Microsoft Azure.

1. Sélectionnez **+ Créer une ressource**.

1. Saisissez **Windows 11** dans la barre de recherche de la Place de marché, puis **Entrée **.

1. Recherchez la zone **Microsoft Windows 11**, puis sélectionnez **Créer** et choisissez **Windows 11 Entreprise, version 22H2** dans le menu qui s’ouvre.

1. Créez la machine virtuelle à l’aide des valeurs suivantes sous l’onglet **Informations de base** :

  | Champ | Valeur à utiliser |
  | :-- | :-- |
  | Abonnement | Accepter la valeur par défaut |
  | Groupe de ressources | Créer une nouvelle - rgEntraLogin |
  | Nom de la machine virtuelle | vmEntraLogin |
  | Région | *default* |
  | Options de disponibilité | Aucune redondance de l’infrastructure requise |
  | Type de sécurité | Standard |
  | Taille | Standard DC1s_v3 -  processeur virtuel, 8 Gio de mémoire |
  | Nom de l’utilisateur administrateur | vmEntraAdmin |
  | Mot de passe administrateur | Utilisez celui fourni par l’environnement de labo ou créez un mot de passe sécurisé que vous pouvez mémoriser |
  | Gestion des licences | Confirmez que vous disposez d’une licence |

1. Vous n’aurez pas besoin de modifier quoi que ce soit sous les onglets **Disques** ou **Mise en réseau**, mais vous pouvez passer en revue les valeurs.

1. Sous l’onglet **Gestion**, cochez la case **Connexion avec Microsoft Entra ID** sous la section de Microsoft Entra ID.

        NOTE: You will notice that the **System assigned managed identity** under the Identity section is automatically checked and turned grey. This action should happen automatically once you enable Login with Microsoft Entra ID.

1. Parcourez le reste de l’expérience de création d’une machine virtuelle. 

1. Sélectionnez **Examiner et créer**, puis sélectionnez **Créer**.

#### Tâche 2 : connexion Microsoft Entra ID pour des machines virtuelles Azure existantes

1. Accédez à **Machines Virtuelles** dans le [https://portal.azure.com](https://portal.azure.com).

1. Sélectionnez la machine virtuelle nouvellement créée lors de la tâche 1.

1. Sélectionnez **Contrôle d’accès (IAM)** .

1. Sélectionnez **+ Ajouter **, puis **Ajouter une attribution de rôle** pour ouvrir la page Ajouter une attribution de rôle.

1. Configurez les paramètres suivants :
  - **Rôle de fonction de tâche**
  - **Rôle** : connexion de l’administrateur aux machines virtuelles
  - **Membres** : choisissez un utilisateur, un groupe ou un principal de service.  Utilisez ensuite **+ Sélectionner des membres** pour ajouter **User2** en tant qu’utilisateur spécifique pour la machine virtuelle.

1. Sélectionnez **Examiner et attribuer** pour terminer le processus.

#### Tâche 3 : mettre à jour la machine virtuelle pour autoriser la connexion Microsoft Entra ID

1. Sélectionnez l’élément de menu **Connect**.

1. Sous l’onglet **RDP**, sélectionnez **Télécharger le fichier RDP**.  Si vous y êtes invité(e), choisissez l’option **Conserver** pour le fichier.  Il sera enregistré dans votre dossier Téléchargements.

1. Ouvrez le dossier **Téléchargements** dans le Gestionnaire de fichiers.

1. Ouvrez le protocole RDP.

1. Choisissez de vous connecter en tant qu’autre utilisateur.

1. Utilisez le nom d’utilisateur d’administrateur (vmEntraAdmin) et le mot de passe que vous avez créés lors de la configuration de la machine virtuelle.
   - Si vous y êtes invité(e), autorisez l’accès à la machine virtuelle ou à la session RDP.

1. Attendez que la machine virtuelle s’ouvre et que tous les logiciels se chargent.

1. Sélectionnez le bouton **Démarrer** dans la machine virtuelle.

1. Tapez **Panneau de contrôle** et lancez l’application associée.

1. Sélectionnez **Système et sécurité** dans la liste des paramètres.

1. Dans le paramètre **Système**, sélectionnez l’option **Autoriser l’accès à distance**.

1. En bas de la boîte de dialogue qui s’ouvre, vous verrez une section **Bureau à distance**.

1. **Décochez** la case **Autoriser des connexions uniquement sur les ordinateurs qui exécutent le Bureau à distance avec authentification au niveau du réseau**.

1. Sélectionnez **Apply** (Appliquer), puis **OK**.

1. **Quittez** la session RDP à la machine virtuelle

#### Tâche 4 – Modifier votre fichier RDP pour prendre en charge la connexion Microsoft Entra ID

1. Ouvrez le dossier **Téléchargements** dans le Gestionnaire de fichiers.

1. **Faites une copie** du fichier RDP et ajoutez **-EntraID** à la fin du nom du fichier.

1. Modifiez la nouvelle version du fichier RDP que vous venez de copier à l’aide de **Bloc-notes**. Ajoutez les deux lignes de texte suivantes au bas du fichier :
     ```
        enablecredsspsupport:i:0
        authentication level:i:2
     ```
 
 1. **Enregistrez** le fichier RDP.  Vous devez maintenant avoir deux versions du fichier :
      - <<virtual machine name>> RDP
      - <<virtual machine name>>-EntraID.RDP

#### Tâche 5 – Se connecter à la machine virtuelle Windows en utilisant une connexion Microsoft Entra ID

1. Ouvrez le fichier **<<virtual machine name>>-EntraID.RDP

1. Sélectionnez l’icône **Se connecter** lorsque la boîte de dialogue s’ouvre.

1. Au lieu de recevoir l’invite sur le compte d’utilisateur à connecter, vous devez recevoir un message indiquant si vous souhaitez vous connecter à l’ordinateur distant.

1. Sélectionnez **Oui** en bas de l’écran.

1. La session Bureau à distance doit s’ouvrir et afficher l’écran de connexion à Windows Server.  **Autre utilisateur** avec un bouton OK doit s’afficher.

1. Cliquez sur **OK**.

1. Dans la boîte de dialogue de connexion, saisissez les informations suivantes :
   - Nom d’utilisateur = **AzureAD\User2@ votre nom de domaine**
   - Mot de passe : entrez le mot de passe d’administrateur communiqué par votre fournisseur d’hébergement de labo.

   NOTE : User2 est l’utilisateur que nous avons autorisé à se connecter en tant qu’administrateur pendant la tâche 1.

1. Windows doit confirmer la connexion et ouvrir le bureau normal.

#### Tâche 6 – Test facultatif pour explorer la connexion Microsoft Entra ID

1. Vérifiez que User2 était le seul utilisateur ajouté au groupe Administrateurs.

1. Cliquez avec le bouton droit sur le bouton DÉMARRER, puis sélectionnez **Gestion de l’ordinateur** dans le menu contextuel.

1. Ouvrez **utilisateurs et groupes locaux**, puis accédez à **Groupes, Administrateurs**.

1. Vous devriez voir **Azure\User2…** dans la liste.

1. Vérifiez si d’autres membres Microsoft Entra ID peuvent se connecter.

1. Quittez la session Bureau à distance.

1. Lancez à nouveau le fichier **<<server name>>-EntraID.RDP**.

1. Essayer de vous connecter sous le nom d’autres membres Microsoft Entra ID.

1. Vous devez remarquer que ces utilisateurs n’ont pas accès.

### Exercice 2 facultatif : Connexion à des machines virtuelles Linux dans Azure avec Microsoft Entra ID

#### Tâche 1 : créer une machine virtuelle Linux avec une identité managée affectée par le système

1. Accéder : [https://portal.azure.com](https://portal.azure.com)

1. Sélectionnez **+ Créer une ressource**.

1. Recherchez **Ubuntu**.

1. Sous **Ubuntu Server 22.04 LTS**, sélectionnez **Créer**. Vous pouvez utiliser d’autres serveurs Linux pour ce labo de test.

1. Sous l’onglet **Gestion**, cochez la case pour activer la **Connexion avec Microsoft Entra ID**.

1. Vérifiez que l’option **Identité managée affectée par le système** est cochée.

1. Parcourez le reste de l’expérience de création d’une machine virtuelle. Dans cette préversion, vous devrez créer un compte administrateur avec un nom d’utilisateur et un mot de passe ou une clé publique SSH.

#### Tâche 2 : connexion Microsoft Entra ID pour des machines virtuelles Azure existantes

1. Accédez à **Machines Virtuelles** dans le [https://portal.azure.com](https://portal.azure.com).

1. Sélectionnez **Contrôle d’accès (IAM)** .

1. Sélectionnez Ajouter  Ajouter une attribution de rôle pour ouvrir la page Ajouter une attribution de rôle.

1. Attribuez le rôle suivant. 
    - **Rôle** : Connexion de l’administrateur aux machines virtuelles ou Connexion de l’utilisateur aux machines virtuelles
    - **Attribuer l’accès à** : Un utilisateur, à un groupe, à un principal de service ou à une identité managée.

##### Partie labo de défi

Essayez de terminer le reste de ce labo par vous-même. Il est très similaire à la version de Windows. Si vous avez besoin des étapes détaillées, consultez l’article « Attribuer des rôles Azure à l’aide du portail Azure » dans la documentation Learn.

