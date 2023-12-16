---
lab:
  title: "7 - Ajouter une identité hybride avec Azure\_AD\_Connect"
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Labo 7 - Ajouter une identité hybride avec Azure AD Connect

**Remarque** : ce labo nécessite un pass Azure. Consultez le labo 00 pour obtenir des instructions.

**Remarque 2** : ce labo est facultatif.  Vous aurez besoin d’au moins 1 heure pour terminer ce labo, qui demande précision et sens du détail.  N’hésitez pas à y revenir plus tard si vous n’avez pas suffisamment de temps à y consacrer.  Si votre entreprise a déjà configuré sa configuration hybride ou si vous n’envisagez pas d’utiliser Azure AD Connect, ignorez ce labo.

## Scénario de l’exercice

Votre entreprise utilise un domaine Active Directory Domain Services local.  Elle souhaite continuer à utiliser un domaine Active Directory local comme solution de gestion des identités et des accès, mais elle a également besoin que les utilisateurs puissent accéder aux applications cloud avec le même nom d’utilisateur et le même mot de passe.

#### Durée estimée : 60 minutes

### Exercice 1 - Configurer l’infrastructure locale

#### Tâche 1 : créer une infrastructure Active Directory locale

1. Le modèle de déploiement est accessible via le lien suivant : [Guide du laboratoire de test local](https://github.com/maxskunkworks/TLG/tree/master/tlg-base-config_3-vm).

    **Remarque pour les apprenants et les MCT** : le déploiement de ce modèle peut prendre 30 à 60 minutes. Soyez donc prêt à prendre une pause à cette étape ou à effectuer le déploiement avant la section de cours théorique.

    **Remarque pour les fournisseurs de labo** : si possible, il serait utile aux étudiants de terminer et de déployer dans le cadre de la configuration de l’environnement de labo.

2. Dans la page **TLG (Guide du laboratoire de test) - Configuration de base à 3 machines virtuelles (v1.0),** sélectionnez **Déployer sur Azure**.

   **Remarque** : la configuration de base à 3 machines virtuelles provisionne un contrôleur de domaine Active Directory Windows Server 2016 nommé DC1 à l’aide du nom de domaine que vous spécifiez et un serveur membre de domaine nommé APP1 exécutant Windows Server 2016. Elle permet également d’approvisionner une machine virtuelle cliente sous Windows 10, mais nous ne l’utiliserons pas dans notre labo (principalement en raison des exigences de licence applicables lors de l’exécution de machines virtuelles Windows 10 dans Azure). Le serveur membre de domaine (APP1) a installé automatiquement .NET 4.5 et IIS.  
   
   **Remarque** : la machine virtuelle requise pour ce labo est **DC1**.  Si vous utilisez un pass Azure, il existe une limitation de 2 machines virtuelles, de sorte que la machine virtuelle cliente peut échouer.  Cela n’est pas nécessaire pour ce labo.

3. Dans la page **Déploiement personnalisé**, indiquez les paramètres suivants et sélectionnez **Vérifier + Créer**, puis **Créer**.

   -   Abonnement : nom de l’abonnement Azure cible dans lequel vous souhaitez approvisionner les machines virtuelles Azure de l’environnement labo.
   -   Groupe de ressources : (Créer nouveau) **hybrididentity-RG**
   -   Emplacement : nom de la région Azure qui hébergera les machines virtuelles Azure de l’environnement labo.
   -   Nom de la configuration : **TlgBaseConfig-01**
   -   Nom de domaine : **corp.contoso.com**
   -   Système d’exploitation du serveur : **2016-Datacenter**
   -   Nom d’utilisateur administrateur : **demouser**
   -   Mot de passe administrateur : **saisissez un mot de passe sécurisé et mémorisez-le**
   -   Déployer la machine virtuelle cliente : **Non**
   -   URI du disque dur virtuel du client : **laissez ce champ vide**
   -   Taille de machine virtuelle : **Standard_D2s_v3**
   
   **Remarque** : utilisez une taille de machine virtuelle similaire si votre abonnement ne prend pas en charge la taille répertoriée. Consultez la documentation ici : <https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes>.

   -   Préfixe d’étiquette DNS : **tout nom DNS valide et global unique (chaîne unique composée de lettres, de chiffres et de traits d’union, commençant par une lettre et jusqu’à 47 caractères).**

   -   Emplacement _artifacts : **acceptez la valeur par défaut**
   -   Jeton SAP d’emplacement _artifacts : **laissez ce champ vide**.

4. Sélectionnez **Vérifier + créer**.

5. Une fois la validation réussie, sélectionnez **Créer**.
    
6. Attendez la fin du déploiement. Ceci peut prendre environ 60 minutes.


### Tâche 2 - Configurer les machines virtuelles Azure de l’environnement labo

1. Dans la fenêtre du navigateur affichant le Portail Azure, accédez à la machine virtuelle Azure **DC1** et connectez-vous à elle via le Bureau à distance. Quand vous y êtes invité, connectez-vous à l’aide des informations d’identification suivantes :

   -   Nom d’utilisateur : **demouser**
   -   Mot de passe : **utilisez le mot de passe sécurisé que vous avez créé dans la tâche 1**

2.  Dans la session Bureau à distance sur **DC1**, démarrez **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour désactiver la configuration de sécurité renforcée d’Internet Explorer et le contrôle d’accès utilisateur sur les machines virtuelles Azure **DC1** et **APP1** :

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 00000000}
    ```

    **Remarque :** pour exécuter plusieurs scripts PowerShell dans le même fichier, vous pouvez mettre en surbrillance un script spécifique et sélectionner **Exécuter la sélection** en regard du bouton de lecture vert. 

3.  Dans la fenêtre **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour installer les outils d’administration de serveur distant sur les machines virtuelles Azure **DC1* et **APP1** :

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Install-WindowsFeature RSAT -IncludeAllSubFeature} 
    ```

4.  Dans la **fenêtre Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour activer le protocole TLS 1.2 sur les machines virtuelles Azure **DC1* et **APP1** :

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force}
    Invoke-Command -ComputerName $vmNames {New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value 1 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 0 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value 1 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 0 –PropertyType DWORD}
    Invoke-Command -ComputerName $vmNames {New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value 1 –PropertyType DWORD}
    ```

5.  Dans la fenêtre **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour configurer l’authentification intégrée Windows sur le site web par défaut hébergé sur la machine virtuelle Azure **APP1** :

    ```pwsh

    $vmNames = @('app1')
    Invoke-Command -ComputerName $vmNames {Enable-WindowsOptionalFeature -Online -FeatureName IIS-WindowsAuthentication}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/anonymousAuthentication" -Name Enabled -Value False -PSPath IIS:\ -Location "Default Web Site"}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/windowsAuthentication" -Name Enabled -Value True -PSPath IIS:\ -Location "Default Web Site"}
    ```

### Tâche 3 - Démarrer les machines virtuelles Azure

1. Dans la fenêtre **Windows PowerShell ISE**, à partir du panneau de la console, exécutez la commande suivante pour redémarrer **APP1** :

    ```pwsh

    Restart-Computer -ComputerName 'APP1'
    ```

2. Dans la fenêtre **Windows PowerShell ISE**, à partir du panneau de la console, exécutez la commande suivante pour redémarrer **DC1** :

    ```pwsh
    Restart-Computer -ComputerName 'DC1'
    ```

### Tâche 5 : configurer le domaine Active Directory contoso.local

1. Connectez-vous à nouveau à la machine virtuelle Azure **DC1** via le Bureau à distance. Quand vous y êtes invité, connectez-vous à l’aide des informations d’identification suivantes :

    -   Nom d’utilisateur : **demouser**

    -   Mot de passe : **demo\@pass123**
       - **Il est fortement recommandé d’entrer un mot de passe sécurisé que vous pouvez mémoriser.**

2.  Dans la session Bureau à distance sur **DC1**, démarrez Internet Explorer et accédez au lien ci-dessous.

    ```
    https://github.com/microsoft/MCW-Hybrid-identity/tree/main/Hands-on%20lab/studentfiles
    ```

3. Dans la page **Créer des utilisateurs/groupes pour l’environnement de démonstration/test Active Directory**, sélectionnez le lien **CreateDemoUsers.ps1**, acceptez les termes de licence et enregistrez le script correspondant dans le système de fichiers local.

4. Dans la page **Créer des utilisateurs/groupes pour l’environnement de démonstration/test Active Directory**, sélectionnez le lien **CreateDemoUsers.csv** (directement au-dessus de la section code PowerShell) et enregistrez le fichier CSV correspondant au même emplacement que le fichier **CreateDemoUsers.ps1**.

5. Dans la session Bureau à distance sur **DC1**, démarrez l’Explorateur de fichiers, accédez au dossier dans lequel vous avez téléchargé les deux fichiers, cliquez avec le bouton droit sur le fichier **CreateDemoUsers.ps1**, sélectionnez **Propriétés** dans la boîte de dialogue **CreateDemoUsers.ps1 Properties**, puis cochez la case **Débloquer** et terminez en cliquant sur **OK**.

6. Dans la fenêtre Explorateur de fichiers, sélectionnez à nouveau le fichier **CreateDemoUsers.ps1**, puis **Modifier**. 

7. Dans la fenêtre **Administrateur : Windows PowerShell ISE**, modifiez la ligne **148** de :

    ```pwsh
    $UserCount = 1000 #Up to 2500 can be created
    ```

   à
    ```pwsh
    $UserCount = 2500 #Up to 2500 can be created
    ```

8. Dans la fenêtre **Windows PowerShell ISE**, enregistrez la modification et exécutez le script **CreateDemoUsers.ps1** pour créer une hiérarchie de l’unité d’organisation de l’environnement lab et le remplir avec des comptes d’utilisateur de test. 

9.  Dans la fenêtre **Windows PowerShell ISE** , ajoutez le script suivant au volet de script et exécutez-le pour modifier les paramètres des comptes d’utilisateur AD que vous utiliserez dans ce labo :

    ```pwsh

    $adUser1 = Get-ADUser -Filter {samAccountName -eq "AGAyers"}
    $adUser1groups = $adUser1 | Get-ADPrincipalGroupMembership 
    $adUser1groups | foreach { if ($_.name -ne 'Domain Users') {Remove-ADPrincipalGroupMembership -MemberOf $_.name -Identity $adUser1.DistinguishedName} }
    Add-ADPrincipalGroupMembership -MemberOf 'Engineering' -Identity $adUser1.DistinguishedName
    Move-ADObject -Identity $adUser1.DistinguishedName -TargetPath 'OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Set-ADAccountPassword -Identity 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "demo@pass123" -Force)

    $adUser2 = Get-ADUser -Filter {samAccountName -eq "TFBell"}
    $adUser2groups = $adUser2 | Get-ADPrincipalGroupMembership 
    $adUser2groups | foreach { if ($_.name -ne 'Domain Users') {Remove-ADPrincipalGroupMembership -MemberOf $_.name -Identity $adUser2.DistinguishedName} }
    Add-ADPrincipalGroupMembership -MemberOf 'Engineering' -Identity $adUser2.DistinguishedName
    Move-ADObject -Identity $adUser2.DistinguishedName -TargetPath 'OU=VT,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Set-ADAccountPassword -Identity 'CN=Bell\, Teresa,OU=VT,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "demo@pass123" -Force)
    Get-ADGroup -Identity 'Domain Admins' | Add-ADGroupMember -Members 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    Get-ADGroup -Identity 'Enterprise Admins' | Add-ADGroupMember -Members 'CN=Ayers\, Ann,OU=NJ,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    ```

10. Dans la fenêtre **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour créer des unités d’organisation supplémentaires nommées **Serveurs** et **Clients**, puis déplacez le compte d’ordinateur **APP1** vers la première d’entre elles :

    ```pwsh

    New-ADOrganizationalUnit -Name 'Servers' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    New-ADOrganizationalUnit -Name 'Clients' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Move-ADObject -Identity 'CN=APP1,CN=Computers,DC=corp,DC=contoso,DC=com' -TargetPath 'OU=Servers,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    ```

11. Déconnectez-vous de la machine virtuelle **DC1**.

## Exercice 3 - Intégrer une forêt Active Directory à un locataire Azure Active Directory

### Tâche 1 - Créer un locataire Azure Active Directory et activez une version d’évaluation EMS E5

Dans cette tâche, vous allez créer un locataire Azure Active Directory avec les paramètres suivants : 

-   Nom d’organisation : **Contoso**

-   Nom de domaine initial :tout nom de domaine valable et unique.

-   Pays ou région : **États-Unis**

1. À partir de l’ordinateur de labo, ouvrez une nouvelle fenêtre de navigateur web et accédez au Portail Azure à <https://portal.azure.com> si vous ne l’avez pas déjà fait.

2. Lorsque vous y êtes invité, connectez-vous à l’abonnement Azure dans lequel vous avez déployé des ressources au cours des exercices de préparation du labo.

3. Dans le portail Azure, cliquez sur la page Accueil, puis sur **+ Créer une ressource**.

4. Sur la page **Nouveau**, dans la zone de texte **Rechercher dans la Place de marché**, saisissez **Azure Active Directory** puis, dans la liste des résultats, sélectionnez **Azure Active Directory**.

5. Dans la page **Azure Active Directory**, sélectionnez **Créer**.

6. Dans la page **Créer un annuaire**, spécifiez les paramètres suivants, puis sélectionnez **Créer** :

Onglet de base :
    -   Sélectionner un type de locataire : choisissez **Azure Active Directory**

Onglet Configuration :
    -   Nom d’organisation : **Contoso**

    -   Nom de domaine initial :tout nom de domaine valable et unique.

    -   Pays ou région : **États-Unis**

7. Une fois le locataire créé, ouvrez **Azure Active Directory**.

8. Dans la page Vue d’ensemble, sélectionnez **Gérer le locataire**.

9. Cochez la case de votre répertoire nouvellement créé.

10. En haut de la page, choisissez **Changer**.

    >**Remarque** : la modification peut prendre quelques minutes avant de prendre effet.

11. Dans la page **Contoso - Vue d’ensemble**, sélectionnez **Utilisateurs**.

12. Notez que vous n’avez qu’un seul utilisateur ExternalAzureAD dans ce nouveau locataire.

### Tâche 2 - Créer et configurer l’utilisateur Azure AD pour administrer ce répertoire

1. À partir de l’ordinateur de labo, dans le Portail Azure, revenez à la page **Contoso - Vue d’ensemble**.

2. Dans la page Contoso - Vue d’ensemble****, sélectionnez **Utilisateurs** sous **Gérer** dans la navigation gauche.

3. Dans la page **Utilisateurs - Tous les utilisateurs**, cliquez sur l’entrée représentant votre compte d’utilisateur.

4. Dans la page **Profil** de votre compte d’utilisateur, sélectionnez **Modifier**.

5. Dans la section **Paramètres**, dans la liste déroulante **Emplacement d’utilisation**, sélectionnez l’entrée **États-Unis**, puis **Enregistrer**.

#### Créer l’administrateur

6. Sur la page **Nouvel utilisateur**, assurez-vous que l’option **Créer un utilisateur** est sélectionnée, spécifiez les paramètres suivants, puis cliquez sur **Créer** :

    - Nom d’utilisateur : **john.doe *@your Nom de domaine du locataire Azure AD* ** où ***votre nom de domaine de locataire Azure AD*** est le nom de domaine que vous avez spécifié lors de la création du locataire Azure AD Contoso.

    - Nom : **john.doe**

    - Prénom : **John**

    - Nom : **Doe**
    
    - Mot de passe : **Générer automatiquement le mot de passe**
    
    - Afficher le mot de passe : **Activé**, veillez à copier le mot de passe.

    - Groupes : **0 groupe sélectionné**
    
    - Rôle : **Administrateur général**
    
    - Bloquer la connexion : **Non**
    
    - Emplacement d’utilisation : **États-Unis**
    
    - Poste : **Laissez le champ vide**
    
    - Service : **Laissez le champ vide**

    > **Remarque** : copiez les valeurs **Nom d’utilisateur** et **Mot de passe** dans le Bloc-notes. Vous en aurez besoin plus tard dans ce labo.


### Tâche  5 - Configurer le suffixe DNS dans la forêt Active Directory Contoso

Dans cette tâche, vous allez configurer le suffixe DNS de la forêt Active Directory Contoso pour qu’il corresponde au nom de domaine personnalisé Azure AD nouvellement vérifié.

1. Sur l’ordinateur de labo, dans le Portail Azure, vérifiez que vous êtes connecté au locataire Azure AD associé à l’abonnement Azure dans lequel vous avez déployé des ressources dans les exercices de préparation du labo (**Répertoire par défaut**). Le cas échéant, sélectionnez l’icône **Répertoire + Abonnement** dans la barre d’outils du Portail Azure (à droite de l’icône **Cloud Shell**) pour passer à ce locataire Azure AD. 

2. Dans le Portail Azure, accédez à la page de la machine virtuelle **DC1**.

3. Sur la page de la machine virtuelle **DC1**, connectez-vous à **DC1** via le Bureau à distance. Lorsque vous êtes invité à vous connecter, indiquez le nom **demouser** et le mot de passe **démo\@pass123**. 

4. Dans la session Bureau à distance sur **DC1**, dans la fenêtre **Gestionnaire de serveur**, démarrez la console **Domaines et approbations Active Directory** sous **Outils**. 

5. Dans la console **Domaines et approbations Active Directory**, cliquez avec le bouton droit sur **Domaines et approbations Active Directory [DC1.corp.contoso.com]** à gauche, puis sélectionnez **Propriétés**.

6. Sous l’onglet **Suffixes UPN** de la fenêtre **Domaines et approbations Active Directory [DC1.corp.contoso.com],** dans la zone de texte **Suffixes UPN alternatifs**, saisissez le nom du domaine personnalisé que vous avez vérifié dans la tâche précédente, sélectionnez **Ajouter**, puis **OK**.

7. Dans la session Bureau à distance sur **DC1**, dans la fenêtre **Gestionnaire de serveur**, démarrez la console **Utilisateurs et ordinateurs Active Directory** sous **Outils**. 

8. Dans la console **Utilisateurs et ordinateurs Active Directory**, développez **corp.contoso.com** à gauche et examinez la hiérarchie de l’unité d’organisation du domaine et l’appartenance aux groupes du domaine. 

9.  Dans la session Bureau à distance sur **DC1**, démarrez Windows PowerShell ISE et, dans le volet Script, exécutez la commande suivante pour remplacer le suffixe UPN de tous les utilisateurs membres du groupe d’**Ingénierie** par ceux correspondant au nom de domaine vérifié personnalisé du locataire  Azure AD Contoso (remplacez l’espace réservé `<custom_domain_name>` par le nom réel du nom de domaine vérifié personnalisé que vous avez donné au locataire Azure AD Contoso). 

    ```pwsh
    $domainName = '<custom_domain_name>'
    $users = Get-ADGroupMember -Identity 'Engineering' -Recursive | Where-Object {$_.objectClass -eq 'user'}

    foreach ($user in $users) {
        $user = Get-ADUser -Identity $User.SamAccountName
        $userName = $user.UserPrincipalName.Split('@')[0] 
        $upn = $userName + "@" + $domainName 
        $user | Set-ADUser -UserPrincipalName $upn
    }
    ```

### Tâche 6 : installer Azure AD Connect

Dans cette tâche, vous allez installer Azure AD Connect.

1. Dans la session Bureau à distance sur **DC1**, dans le Gestionnaire de serveur, sélectionnez **Serveur local** et vérifiez que la **Configuration de sécurité renforcée d’Internet Explorer** est désactivée. Le cas échéant, sélectionnez le lien **Activé** en regard de la **Configuration de sécurité renforcée d’Internet Explorer**, définissez les paramètres **Administratieur** sur **Désactivé**, puis sélectionnez **OK**.

2. Dans la session Bureau à distance sur **DC1**, ouvrez la fenêtre **Windows PowerShell ISE** et exécutez cette commande pour installer le navigateur Chrome.

    ```pwsh
    $LocalTempDir = $env:TEMP; $ChromeInstaller = "ChromeInstaller.exe"; (new-object System.Net.WebClient).DownloadFile('http://dl.google.com/chrome/install/375.126/chrome_installer.exe', "$LocalTempDir\$ChromeInstaller"); & "$LocalTempDir\$ChromeInstaller" /silent /install; $Process2Monitor = "ChromeInstaller"; Do { $ProcessesFound = Get-Process | ?{$Process2Monitor -contains $_.Name} | Select-Object -ExpandProperty Name; If ($ProcessesFound) { "Still running: $($ProcessesFound -join ', ')" | Write-Host; Start-Sleep -Seconds 2 } else { rm "$LocalTempDir\$ChromeInstaller" -ErrorAction SilentlyContinue -Verbose } } Until (!$ProcessesFound)
    ```

2. Dans la session Bureau à distance sur **DC1**, lancez le navigateur Chrome et accédez au Portail Azure à l’adresse <https://portal.azure.com>.

3. Lorsque vous êtes invité à vous connecter, saisissez les informations d’identification du compte d’utilisateur Azure AD **John.doe**, que vous avez copié dans le Bloc-notes plus tôt dans cet exercice.

4. Quand vous y êtes invité, modifiez le mot de passe du compte d’utilisateur **john.doe**. 
  
    > **Remarque** : si vous recevez le message **Ce mot de passe est trop commun. Choisissez-en un plus difficile à deviner**, vous devez modifier le mot de passe jusqu’à ce qu’il soit suffisamment unique pour être accepté.

5. Si l’invite **Rester connecté ? »** s’affiche Sélectionnez **Non**. Vous serez redirigé vers l’interface du Portail Azure. 

6. Si la boîte de dialogue **Bienvenue dans Microsoft Azure** s’affiche, sélectionnez **Peut-être plus tard**. 

7. Dans le Portail Azure, sélectionnez **Azure Active Directory** dans la navigation gauche du portail pour accéder à la page **Contoso - Vue d’ensemble**.

8. Sur la page **Contoso - Vue d’ensemble**, sélectionnez **Azure AD Connecter** sous **Gérer** à gauche.

9.  Sur la page **Azure AD Connect**, cliquez sur le lien **Télécharger Azure AD Connect**.  Choisissez ensuite **Connect Sync** dans le menu.

10. Sur la page **Microsoft Azure Active Directory Connect** du site de téléchargement de Microsoft, cliquez sur **Télécharger**.

11. Quand vous êtes invité à exécuter ou enregistrer **AzureADConnect.msi**, sélectionnez **Exécuter**. Le fichier est alors téléchargé et l’assistant **Microsoft Azure Active Directory Connect** démarre automatiquement. 

12. Sur la page **Bienvenue dans Azure AD Connect**, cochez la case **J’accepte les termes du contrat de licence et l’avis de confidentialité**, puis sélectionnez **Continuer**.

13. Sur la page **Configuration rapide**, sélectionnez le bouton **Personnaliser**.

14. Sur la page **Installer les composants requis**, laissez toutes les options de configuration facultatives décochées, puis cliquez sur **Installer**.

15. Sur la page **Connexion de l’utilisateur**, sélectionnez l’option **Authentification directe** et cochez la case **Activer l’authentification unique**, puis cliquez sur **Suivant**.

16. Sur la page **Se connecter à Azure AD**, connectez-vous à l’aide des informations d’identification du compte **john.doe**, puis sélectionnez **Suivant**.

17. Sur la page **Connecter vos répertoires**, vérifiez que l’entrée **corp.contoso.com** s’affiche dans la liste déroulante **FOREST**, puis sélectionnez **Ajouter un répertoire**. Dans le **compte de forêt AD**, vérifiez que l’option **Créer un compte AD** est sélectionnée, puis dans la zone de texte **ENTERPRISE ADMIN USERNAME**, saisissez **CORP.CONTOSO.COM\\demouser**, dans la zone de texte **PASSWORD**, saisissez **demo\@pass123**, puis sélectionnez **OK**.


18. Revenez à la page **Connexion de vos annuaires** et cliquez sur **Suivant**.

19. Sur la page de **configuration de la connexion à Azure AD**, vérifiez que votre nom de domaine personnalisé est répertorié comme **Suffixe UPN Active Directory** vérifié et que l’entrée **userPrincipalName** apparaît dans la liste déroulante **USER PRINCIPAL NAME**. L’avertissement suivant s’affiche : **Les utilisateurs ne pourront pas se connecter à Azure AD avec des informations d’identification locales si le suffixe UPN ne correspond pas à un nom de domaine vérifié**. Cochez la case **Continuer sans faire correspondre tous les suffixes UPN au domaine vérifié**, puis sélectionnez **Suivant**. 

    >**Remarque** : ce comportement est attendu, étant donné que certains utilisateurs sont toujours configurés avec le suffixe UPN **contoso.local**, qui n’est pas routable et ne peut pas être configuré en tant que nom de domaine personnalisé vérifié d’un locataire Azure AD.

20. Sur la page **Filtrage par domaine ou unité d’organisation**, choisissez **Synchroniser les domaines et unités d’organisation sélectionnés**, puis vérifiez que seule l’unité d’organisation **DemoAccounts** et toutes ses unités d’organisation enfants sont sélectionnées et cliquez sur **Suivant**. 


21. Sur la page **Identification de manière unique de vos utilisateurs**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**. 


22. Sur la page **Filtrer les utilisateurs et appareils**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**. 

23. Sur la page **Fonctionnalités facultatives**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**.

24. Sur la page **Activer l’authentification unique**, sélectionnez **Entrer les informations d’identification** pour afficher la boîte de dialogue **Informations d’identification de la forêt** et connectez-vous à l’aide du nom d’utilisateur **CORP\\demouser** et du mot de passe **demo\@pass123**, puis sélectionnez **Suivant**.


25. Sur la page **Prêt à configurer**, vérifiez que la case **Démarrez le processus de synchronisation une fois la configuration terminée** n’est **PAS** cochée, puis cliquez sur **Installer**.


   > **Remarque** : vous allez configurer le filtrage au niveau des attributs avant d’activer le processus de synchronisation.

   > **Remarque** : cette installation prend environ 2 minutes.

26. Sur la page **Configuration effectuée**, sélectionnez **Quitter**.


### Tâche 7 : activer la Corbeille Active Directory

Dans cette tâche, vous allez activer la Corbeille dans le domaine Active Directory Contoso. 

1. Dans la session Bureau à distance sur **DC1**, dans le menu Outils de la console Gestionnaire de serveur, lancez le **Centre d’administration Active Directory**.


2. Dans la console **Centre d’administration Active Directory**, cliquez avec le bouton droit sur **corp (local)** à gauche, puis sélectionnez **Activer la corbeille**. Quand vous êtes invité à confirmer, sélectionnez **OK**.


3. Lorsque vous êtes invité à actualiser le centre d’administration AD, sélectionnez **OK**.

   > **Remarque** : pour plus d’informations sur les avantages de la Corbeille dans les scénarios hybrides, consultez l’article <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-recycle-bin>

### Tâche 8 - Configurer le filtrage au niveau de l’attribut azure AD Connect

Dans cette tâche, vous allez configurer le filtrage au niveau de l’attribut Azure AD Connect afin de limiter la synchronisation des comptes d’utilisateur à ceux dont le suffixe UPN correspond au nom de domaine personnalisé du locataire Azure AD cible.

   > **Remarque** : l’option de filtrage positif nécessite au moins deux règles de synchronisation. L’une d’elles détermine la portée des objets à synchroniser. La deuxième règle de synchronisation fourre-tout exclut tous les objets qui n’ont pas encore été identifiés en tant qu’objets à synchroniser.

1. Dans la session Bureau à distance sur **DC1**, ouvrez l’**Éditeur de règles de synchronisation** sous **Azure AD Connect** dans le menu Démarrer.


2. Dans la fenêtre Éditeur de règles de synchronisation, dans la page **Afficher et gérer vos règles de synchronisation**, vérifiez que le trafic **Entrant** apparaît dans la liste déroulante **Direction** et sélectionnez **Ajouter une nouvelle règle**. L’assistant **Créer une règle de synchronisation entrante** s’affiche.


3. Sur la page **Créer une règle de synchronisation entrante - Description**, spécifiez les paramètres suivants, puis sélectionnez **Suivant** :

    - Nom : **Trafic entrant personnalisé d’AD - Filtre UPN**

    - Description : **Règle de trafic entrant personnalisé – inclut les utilisateurs avec un UPN défini pour correspondre au domaine personnalisé Azure AD**

    - Système connecté : **corp.contoso.com**

    - Type d’objet système connecté : **utilisateur**

    - Type d’objet métaverse : **personne**

    - Type de lien : **Jointure**

    - Priorité : **50**

    - Balise : **Laissez ce champ vide**

    - Activer la synchronisation du mot de passe : **Laissez ce champ vide**

    - Désactivé : **Laissez ce champ vide**


4. Sur la page **Créer un filtre de portée entrante**, sélectionnez **Ajouter un groupe**, puis **Ajouter la clause**, indiquez les éléments suivants et sélectionnez **Suivant** :

    - Attribut : **userPrincipalName**

    - Opérateur : **ENDSWITH**

    - Valeur: **\@\<your custom domain name>**

5. Sur la page **Règles de jointure**, sélectionnez **Suivant**.

6. Sur la page **Transformations**, sélectionnez **Ajouter une transformation**, indiquez les éléments suivants et sélectionnez **Ajouter** :

    - FlowType : **Constant**

    - Attribut cible : **cloudFiltered**

    - Source : **False**

7. Lorsque la boîte de dialogue **Avertissement** s’affiche avec le message **Une importation et une synchronisation complètes seront exécutées sur « corp.contoso.com » au cours de votre prochain cycle de synchronisation**, sélectionnez **OK**.

   > **Remarque** : vous devez revenir à l’interface d’affichage et de gestion des règles de synchronisation, avec la nouvelle règle répertoriée en haut de la liste des règles. 

8. De retour dans la fenêtre **Éditeur de règles de synchronisation**, sur la page **Affichage et gestion de vos règles de synchronisation**, vérifiez que le trafic **Entrant** apparaît dans la liste déroulante **Direction** et cliquez à nouveau sur **Ajouter une nouvelle règle**. L’assistant **Créer une règle de synchronisation entrante** s’affiche.

9. Sur la page **Description**, entrez les paramètres suivants et sélectionnez **Suivant** :

    - Nom : **Trafic entrant personnalisé d’AD - Filtre fourre-tout**

    - Description : **Règle de trafic entrant personnalisé - Exclut tous les utilisateurs dont l’UPN n’est pas défini pour correspondre au domaine personnalisé Azure AD**

    - Système connecté : **corp.contoso.com**

    - Type d’objet système connecté : **utilisateur**

    - Type d’objet métaverse : **personne**

    - Type de lien : **Jointure**

    - Priorité : **51**

    - Balise : **Laissez ce champ vide**

    - Activer la synchronisation du mot de passe : **Laissez ce champ vide**

    - Désactivé : **Laissez ce champ vide**


10. Sur la page **Filtre de sélection**, sélectionnez **Suivant**.

11. Sur la page **Règles de jointure**, sélectionnez **Suivant**.

12. Sur la page **Transformations**, sélectionnez **Ajouter une transformation**, indiquez les éléments suivants et sélectionnez **Ajouter** :

    - FlowType : **Constant**

    - Attribut cible : **cloudFiltered**

    - Source : **True**

13. Lorsqu’une boîte de dialogue d’**Avertissement** s’affiche avec un message indiquant qu’une **Importation et une synchronisation complètes seront exécutées sur « corp.contoso.com » au cours de votre prochain cycle de synchronisation**, sélectionnez **OK**.

    >**Remarque** : vous êtes redirigé vers l’interface **Afficher et gérer votre interface de règles de synchronisation**, qui contient les nouvelles règles en haut de la liste des règles. 

### Tâche 9 - Démarrer et vérifier la synchronisation d’annuaires

1. Dans la session Bureau à distance sur **DC1**, double-sélectionnez le raccourci bureau **Azure AD Connect**.

2. Sur la page **Bienvenue dans Azure AD Connect**, cliquez sur **Configurer**. 

3. Sur la page **Tâches supplémentaires**, sélectionnez **Personnalisation des options de synchronisation**, puis cliquez sur **Suivant**.

4. Sur la page **Se connecter à Azure AD**, connectez-vous à l’aide des informations d’identification du compte **john.doe**, puis sélectionnez **Suivant**.

5. Dans la page **Connexion de vos annuaires**, cliquez sur **Suivant**.

6. Dans la page **Filtrage par domaine ou unité d’organisation**, cliquez sur **Suivant**. 

7. Sur la page **Fonctionnalités facultatives**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**.

8. Sur la page **Activer l’authentification unique**, sélectionnez **Suivant**.

9. Sur la page **Prêt à configurer**, sélectionnez la case **Démarrez le processus de synchronisation une fois la configuration terminée**, puis cliquez sur **Configurer**.

10. Sur la page **Configuration effectuée**, sélectionnez **Quitter**.

11. Dans la session Bureau à distance sur **DC1**, dans la fenêtre de navigateur Edge affichant le portail Azure, accédez à la page **Utilisateurs – Tous les utilisateurs** du locataire Azure AD Contoso.

12. Sur la **page Utilisateurs - Tous les utilisateurs** , notez que la liste des objets utilisateur inclut tous les comptes d’utilisateur dont le suffixe UPN correspond au nom de domaine personnalisé du locataire Azure AD. Vous devrez peut-être actualiser la page ou attendre quelques minutes pour que la modification prenne effet.

13. Dans le Portail Azure, accédez à la page **Groupes – Tous les groupes** du locataire Azure AD Contoso. Vous remarquerez que tous les groupes du domaine corp.contoso.com ont également été synchronisés. 

14. Dans le Portail Azure, accédez à la page **Contoso – Azure AD Connect** et sélectionnez **Azure AD Connect** sur la gauche. Vérifiez que les paramètres suivants sont définis : 

    - État de synchronisation d’Azure AD Connect : **Activé** 
  
    - Dernière synchronisation : **un horodatage doit s’afficher**. 
  
    - Synchronisation de hachage du mot de passe : **Désactivé** 
  
    - Fédération : **Désactivé**
   
    - Authentification unique transparente : **Activée pour 1 domaine** 
  
    - Authentification directe : **Activée avec 1 agent**

   > **Remarque** : dans un environnement de production, vous devez installer des agents supplémentaires pour la redondance. Pour plus d'informations, consultez <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-quick-start>.

### Tâche 10 : configurer la jonction Azure AD Hybride

Dans cette tâche, vous allez configurer les options de synchronisation d’appareils Azure AD Connect.

1. Dans la session Bureau à distance sur **DC1**, double-sélectionnez le raccourci bureau **Azure AD Connect**.

2. Sur la page **Bienvenue dans Azure AD Connect**, cliquez sur **Configurer**. 

3. Sur la page **Tâches supplémentaires**, sélectionnez **Configurer les options de l’appareil**, puis **Suivant**.

4. Dans la page **Vue d’ensemble**, passez en revue les informations relatives à la **jonction Azure AD Hybride** et à la **Réécriture d’appareil**, puis sélectionnez **Suivant**.

5. Sur la page **Se connecter à Azure AD**, connectez-vous à l’aide des informations d’identification du compte **john.doe**, puis sélectionnez **Suivant**.

6. Sur la page **Options de l’appareil**, vérifiez que l’option **Configurer la jonction Azure AD Hybride** est sélectionnée, puis cliquez sur **Suivant**. 

7. Sur la page **Système d’exploitation de l’appareil**, cochez les cases **Appareils joints à un domaine Windows 10 ou ultérieur** et **Appareils joints à un domaine Windows de niveau inférieur pris en charge**, puis sélectionnez **Suivant**. 

   > **Remarque** : les appareils Windows de niveau inférieur sont pris en charge uniquement si vous utilisez l’authentification unique transparente pour les domaines managés ou un service de fédération tel qu’AD FS pour les domaines fédérés.

8. Sur la page **configuration SCP**, cochez la zone de forêt Active Directory **corp.contoso.com**, sélectionnez l’entrée **Azure Active Directory** dans la liste déroulante **Service d’authentification**, puis cliquez sur **Ajouter**.

9. Lorsque vous êtes invité à saisir les informations d’identification d’administrateur d’entreprise pour corp.contoso.com, dans la boîte de dialogue **Sécurité Windows**, connectez-vous à l’aide du nom d’utilisateur **CORP\\demouser** et du mot de passe **demo\@pass123**.

10. Revenez à la page **SCP configuration** et cliquez sur **Suivant**.

11. Sur la page **Prêt à configurer**, sélectionnez **Configurer**.

12. Sur la page **Configuration terminée**, vérifiez que la tâche s’est déroulée correctement et sélectionnez **Quitter**.


### Tâche 11 : effectuer une jonction Azure AD Hybride

1. Sur l’ordinateur de labo, dans le Portail Azure, vérifiez que vous êtes connecté au locataire Azure AD associé à l’abonnement Azure dans lequel vous avez déployé des ressources lors des exercices de préparation du labo (**répertoire par défaut**). Le cas échéant, sélectionnez l’icône **Répertoire + Abonnement** dans la barre d’outils du Portail Azure (à droite de l’icône **Cloud Shell**) pour passer à ce locataire Azure AD. 

2. Dans le Portail Azure, accédez à la page de la machine virtuelle **APP1**.

3. Sur la page de la machine virtuelle **APP1**, connectez-vous à **APP1** via le Bureau à distance. Lorsque vous êtes invité à vous connecter, utilisez le nom d’utilisateur **AGAyers\@<custom_domain_name>** avec le mot de passe **demo@pass123** (où l’espace réservé  **<custom_domain_name>** représente le nom de domaine DNS personnalisé que vous avez affecté au locataire Azure AD Contoso plus tôt dans cet exercice.

4. Dans la session Bureau à distance sur **APP1**, dans la fenêtre **Gestionnaire de serveur**, lancez le **Planificateur de tâches** sous **Outils**. 


5. Dans la console **Planificateur de tâches**, accédez à la **Bibliothèque du planificateur de tâches** > **Microsoft** > **Windows** > **Jonction d’espace de travail**. Dans l’écran qui s’affiche, activez puis exécutez la tâche **Jonction automatique d’appareil** . 


6. Passez à la session Bureau à distance sur **DC1** et, à partir du panneau de la console de la fenêtre Windows PowerShell ISE, démarrez la synchronisation delta Azure AD Connect en exécutant la commande suivante :

   ```pwsh
   Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'
   
   Start-ADSyncSyncCycle -PolicyType Delta
   ```

7. Revenez à la session Bureau à distance sur **APP1** et ouvrez une **invite de commandes**.

8. Dans la fenêtre d’invite de commandes, vérifiez l’état de l’inscription d’APP1 dans Azure AD en exécutant la commande suivante : 

   ```
   dsregcmd /status
   ```

9. Vérifiez que la sortie de la commande se présente comme suit :

   ```
   +----------------------------------------------------------------------+
   | Device State                                                         |
   +----------------------------------------------------------------------+

        AzureAdJoined : YES
     EnterpriseJoined : NO
             DeviceId : 61eea2b8-efbe-43d9-b267-126433c8ee34
           Thumbprint : BBAAA0FB4A55E880388851BED955A2669A961A96
       KeyContainerId : 2eb75eb8-0a1d-437b-99d9-9dd161ca0d90
          KeyProvider : Microsoft Software Key Storage Provider
         TpmProtected : NO
         KeySignTest: : PASSED
                  Idp : login.windows.net
             TenantId : xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx
           TenantName : xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx
          AuthCodeUrl : https://login.microsoftonline.com/xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx/oauth2/authorize
       AccessTokenUrl : https://login.microsoftonline.com/xxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxx/oauth2/token
               MdmUrl :
            MdmTouUrl :
     MdmComplianceUrl :
          SettingsUrl :
       JoinSrvVersion : 1.0
           JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/
            JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net
        KeySrvVersion : 1.0
            KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/
             KeySrvId : urn:ms-drs:enterpriseregistration.windows.net
         DomainJoined : YES
           DomainName : CORP

   +----------------------------------------------------------------------+
   | User State                                                           |
   +----------------------------------------------------------------------+

               NgcSet : NO
      WorkplaceJoined : NO
        WamDefaultSet : NO
           AzureAdPrt : NO

   +----------------------------------------------------------------------+
   | Ngc Prerequisite Check                                               |
   +----------------------------------------------------------------------+

        IsUserAzureAD : NO
        PolicyEnabled : NO
       DeviceEligible : YES
   SessionIsNotRemote : NO
     X509CertRequired : NO
         PreReqResult : WillNotProvision

   ```
11. Revenez à la session Bureau à distance sur **DC1**, dans la fenêtre du navigateur Edge affichant le Portail Azure, accédez à la page **Appareils - Tous les appareils** du locataire Azure AD Contoso et vérifiez qu’il existe une entrée représentant le serveur APP1, avec le **type de jointure** défini sur **Azure AD Hybride joint**.

   > **Remarque** : vous devrez peut-être attendre quelques instants pour que l’état de l’inscription dans Azure AD et que son objet Azure AD s’affichent dans le Portail Azure.


