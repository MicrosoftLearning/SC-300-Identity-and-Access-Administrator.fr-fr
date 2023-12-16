---
lab:
  title: "10\_- Authentification Azure\_AD pour machines virtuelles Windows et Linux"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 10 - Authentification Azure AD pour machines virtuelles Windows et Linux

**Remarque** : ce labo nécessite un pass Azure. Consultez le labo 00 pour obtenir des instructions.

## Scénario de l’exercice

L’entreprise a décidé qu’Azure Active Directory doit être utilisé pour se connecter aux machines virtuelles pour l’accès à distance.  Ce labo montre comment configurer ce scénario pour les machines virtuelles Windows et Linux.

#### Durée estimée : 30 minutes

### Exercice 1 - Se connecter à des machines virtuelles Windows dans Azure avec Azure AD

#### Tâche 1 - Créer une machine virtuelle Windows avec la connexion Azure AD activée

1. Accédez à l’URL suivante : [https://portal.azure.com](https://portal.azure.com)

1. Sélectionnez **+ Créer une ressource**.

1. Saisissez **Windows Serveur** dans la barre de recherche Rechercher dans la Place de marché.

1. Sélectionnez **Windows Server** et choisissez **Windows Server 2022 Datacenter** dans la liste déroulante Sélectionner un abonnement logiciel.

1. Créez ensuite un nom d’utilisateur et un mot de passe Administrateur pour la machine virtuelle dans l’onglet Informations de base.
   - Utilisez un nom d’utilisateur dont vous vous souviendrez et un mot de passe sécurisé.

1. Sous l’onglet **Gestion**, cochez la case Se connecter avec Azure AD sous la section Azure AD.

1. Vous remarquerez que l’option **Identité managée affectée par le système** sous la section Identité est automatiquement activée et grisée. Cette action doit se produire automatiquement une fois que vous avez activé la connexion avec Azure AD.

1. Parcourez le reste de l’expérience de création d’une machine virtuelle. 

1. Sélectionnez Créer.

#### Tâche 2 - Connexion Azure AD pour des machines virtuelles Azure existantes

1. Accédez à **Machines virtuelles** sur le site [https://portal.azure.com](https://portal.azure.com).

1. Sélectionnez la nouvelle machine virtuelle créée dans la tâche 1.

1. Sélectionnez **Contrôle d’accès (IAM)** .

1. Sélectionnez **+ Ajouter**, puis **Ajouter une attribution de rôle** pour ouvrir la page Ajouter une attribution de rôle.

1. Attribuez les paramètres suivants :
    - **Type d’affectation** : rôles de fonction de travail
    - **Rôle** : connexion de l’administrateur de machines virtuelles
    - **Membres** : choisissez Utilisateur, groupe ou principal de service.  Utilisez ensuite **+ Sélectionner des membres** pour ajouter **Joni Sherman** en tant qu’utilisateur spécifique de la machine virtuelle.

1. Sélectionnez **Vérifier + attribuer** pour terminer le processus.

#### Tâche 3 - Mettre à jour la machine virtuelle serveur pour prendre en charge la connexion Azure AD

1. Sélectionnez l’option de menu **Connecter**.

1. Sous l’onglet **RDP**, sélectionnez l’option **Télécharger le fichier RDP**.  Si vous y êtes invité, choisissez l’option **Conserver** pour le fichier.  Il sera alors enregistré dans votre dossier Téléchargements.

1. Ouvrez le dossier **Téléchargements** dans le Gestionnaire de fichiers.

1. Ouvrez le fichier RDP.

1. Choisissez de vous connecter en tant qu’autre utilisateur.

1. Utilisez le nom d’utilisateur et le mot de passe d’administration que vous avez créés lors de la configuration de la machine virtuelle.
   - Si vous y êtes invité, sélectionnez oui pour autoriser l’accès à la machine virtuelle ou à la session RDP.

1. Attendez que le serveur s’ouvre et que tous les logiciels se chargent, comme le tableau de bord du Gestionnaire de serveur.

1. Sélectionnez le **bouton Démarrer** dans la machine virtuelle.

1. Tapez **Panneau de configuration** et lancez l’application Panneau de configuration.

1. Sélectionnez **Système et sécurité** dans la liste des paramètres.

1. Dans le paramètre **Système**, sélectionnez l’option **Autoriser l’accès à distance**.

1. Dans le bas de la boîte de dialogue qui s’ouvre, vous verrez une section **Bureau à distance**.

1. **Désactivez** la case **Autoriser les connexions uniquement sur les ordinateurs qui exécutent le Bureau à distance avec authentification au niveau du réseau**.

1. Sélectionnez **Apply** (Appliquer), puis **OK**.

1. **Quittez** la session RDP de la machine virtuelle.


#### Tâche 4 - Modifier le fichier RDP pour prendre en charge la connexion Azure AD

1. Ouvrez le dossier **Téléchargements** dans le Gestionnaire de fichiers.

1. **Effectuez une copie** du fichier RDP et ajoutez **-AzureAD** à la fin du nom de fichier.

1. Modifiez la nouvelle version du fichier RDP que vous venez de copier à l’aide du Bloc-notes. Ajoutez les deux lignes de texte suivantes au bas du fichier :
     ```
        enablecredsspsupport:i:0
        authentication level:i:2
     ```
 
 1. **Enregistrez** le fichier RDP.  Vous devez à présent disposer de deux versions du fichier :
      - <<virtual machine name>>.RDP
      - <<virtual machine name>>-AzureAD.RDP

#### Tâche 5 - Se connecter à Windows Server 2022 Datacenter à l’aide de la connexion Azure AD

1. Ouvrez le fichier **<<virtual machine name>>-AzureAD.RDP.

1. Sélectionnez **Connexion** dans la boîte de dialogue affichée.

1. Au lieu d’être invité à indiquer le compte d’utilisateur à utiliser pour la connexion, vous devez recevoir un message indiquant si vous souhaitez vous connecter à l’ordinateur distant.

1. Sélectionnez **Oui** en bas de l’écran.

1. La session Bureau à distance doit s’ouvrir et affiche l’écran de connexion Windows Server.  L’option **Autre utilisateur** avec un bouton OK doit être affichée.

1. Sélectionnez **OK**.

1. Dans la boîte de dialogue de connexion, entrez les informations suivantes :
   - Nom d’utilisateur = **AzureAD\JoniS@<<your lab domainname>>
   - Mot de passe = Entrez le mot de passe communiqué par votre fournisseur d’hébergement de labo

   REMARQUE : JoniS est l’utilisateur que nous avons autorisé à se connecter en tant qu’administrateur pendant la tâche 1.

1. Windows Server doit confirmer la connexion et ouvrir le tableau de bord du Gestionnaire de serveur normal.

#### Tâche 6 - Test facultatif pour explorer la connexion Azure AD

1. Vérifiez que JoniS est le seul utilisateur ajouté au groupe Administrateurs.

1. Dans le tableau de bord du Gestionnaire de serveur, sélectionnez le menu **Outils** dans le coin supérieur gauche.

1. Lancez l’outil **Gestion de l’ordinateur**.

1. Ouvrez **Utilisateurs et groupes locaux**, puis accédez à **Groupes, Administrateurs**.

1. Vous devriez voir **Azure\JoniSherman....** dans la liste.

1. Vérifiez si d’autres membres Azure AD peuvent se connecter.

1. Déconnectez-vous de la session Bureau à distance.

1. Lancez à nouveau le fichier **<<server name>>-AzureAD.RDP**.

1. Essayez de vous connecter en tant qu’autres membres Azure AD comme AdeleV, AlexW ou DiegoS.

1. Vous devez remarquer que l’accès est refusé pour chacun de ces utilisateurs.

### Exercice facultatif 2 - Se connecter à des machines virtuelles Linux dans Azure avec Azure AD

#### Tâche 1 - Créer une machine virtuelle Linux avec une identité managée affectée par le système

1. Accédez à l’URL suivante : [https://portal.azure.com](https://portal.azure.com)

1. Sélectionnez **+ Créer une ressource**.

1. Sélectionnez **Créer** sous **Ubuntu Server 18.04 LTS** dans la vue Populaire.

1. Sous l’onglet **Gestion**, cochez la case pour activer **Connexion avec Azure Active Directory (préversion)**.

1. Vérifiez que l’option **Identité managée affectée par le système** est cochée.

1. Parcourez le reste de l’expérience de création d’une machine virtuelle. Dans cette préversion, vous devrez créer un compte administrateur avec un nom d’utilisateur et un mot de passe ou une clé publique SSH.

#### Tâche 2 - Connexion Azure AD pour des machines virtuelles Azure existantes

1. Accédez à **Machines virtuelles** sur le site [https://portal.azure.com](https://portal.azure.com).

1. Sélectionnez **Contrôle d’accès (IAM)** .

1. Sélectionnez Ajouter > Ajouter une attribution de rôle pour ouvrir la page Ajouter une attribution de rôle.

1. Attribuez le rôle suivant. 
    - **Rôle** : connexion de l’administrateur de machines virtuelles ou Connexion de l’utilisateur de la machine virtuelle
    - **Attribuer l’accès à** : utilisateur, groupe, principal de service ou identité managée

1. Pour connaître les étapes détaillées, consultez Attribuer des rôles Azure à l’aide du portail Azure.
