---
lab:
  title: "07 (Facultatif)\_: Ajouter une identité hybride avec Microsoft Entra Connect"
  learning path: '01'
  module: Module 01 - Implement an identity management solution
---

# Labo 07 (FACULTATIF) : Ajouter une identité hybride avec Microsoft Entra Connect



# Ce labo n’est pas disponible actuellement.  En raison d’un changement de licence dans Microsoft Entra ID, le labo n’a pas être lancé.  Nous sommes actuellement en train de résoudre les problèmes et de mettre à jour le labo. Nous devrions le remettre en ligne dans un délai d’une semaine.  Passez au labo suivant.




**Remarque** : ce labo nécessite un Pass Azure. Consultez le labo 00 pour obtenir des instructions.

**Remarque 2** : ce labo est à caractère facultatif.  La réalisation de ce labo dure au moins 1 heure et nécessite que vous suiviez avec précision les étapes indiquées.  N’hésitez pas à programmer ces étapes, puisque le temps vous le permet.  Si votre entreprise a déjà effectué sa configuration hybride ou si vous n’envisagez pas d’utiliser Microsoft Entra Connect, passez ce labo.

## Scénario de l’exercice

Votre entreprise dispose de services de domaine Active Directory en local.  Elle souhaite continuer à utiliser Active Directory local comme solution de gestion des identités et des accès, mais elle requiert également la possibilité pour les utilisateurs d’accéder aux applications cloud avec le même nom d’utilisateur et le même mot de passe.

#### Durée estimée : 60 minutes

### Exercice 1 : Configurer l’infrastructure locale

#### Tâche 1 : créer l’infrastructure Active Directory locale

1. Le modèle de déploiement est accessible via ce lien : [Guide du labo de test local](https://github.com/maxskunkworks/TLG/tree/master/tlg-base-config_3-vm).

    **À l’attention des apprenants et des MCT** : le déploiement de ce modèle peut prendre 30 à 60 minutes. N’hésitez pas à prendre une pause lors de cette étape ou à exécuter le déploiement avant une section de cours.

    **À l’attention des fournisseurs de labo** : si possible, il serait utile aux étudiants de réaliser l’exercice et le déploiement dans le cadre de la configuration de l’environnement de labo.

2. Sur la page **TLG (Guide du labo de test) (Configuration de base à 3 machines virtuelles [v1.0]),**, sélectionnez **Déployer sur Azure**.

   **Remarque** : la configuration de base à 3 machines virtuelles approvisionne un contrôleur de domaine Active Directory Windows Server 2016 nommé DC1 à l’aide du nom de domaine que vous spécifiez et d’un serveur membre de domaine nommé APP1 exécutant Windows Server 2016. Elle offre également une option permettant d’approvisionner une machine virtuelle cliente exécutant Windows 10, mais nous ne l’utiliserons pas dans notre labo (principalement en raison des exigences de licence applicables lors de l’exécution de machines virtuelles Windows 10 dans Azure). Le serveur membre de domaine (APP1) a installé automatiquement .NET 4.5 et IIS.  
   
   **Remarque** : la machine virtuelle requise pour ce labo est **DC1**.  Si vous utilisez un Pass Azure, il existe une limitation de 2 machines virtuelles, ce qui explique que la machine virtuelle cliente peut échouer.  Cela n’est pas nécessaire pour ce labo.

3. Sur la page **Déploiement personnalisé**, spécifiez les paramètres suivants, sélectionnez **Vérifier + Créer**, puis **Créer**.

   -   Abonnement : nom de l’abonnement Azure cible dans lequel vous souhaitez approvisionner les machines virtuelles Azure de l’environnement de labo
   -   Groupe de ressources : (Créer) **hybrididentity-RG**
   -   Emplacement : nom de la région Azure qui héberge les machines virtuelles Azure de l’environnement de labo
   -   Nom de la configuration : **TlgBaseConfig-01**
   -   Nom de domaine racine : **corp.contoso.com**
   -   Système d’exploitation du serveur : **2016-Datacenter**
   -   Nom d’utilisateur de l’administrateur : **demouser**
   -   Mot de passe de l’administrateur : **saisissez un mot de passe sécurisé que vous mémoriserez**
   -   Déployer une machine virtuelle cliente : **non**
   -   URI du disque dur virtuel du client : **laisser ce champ vide**
   -   Taille de machine virtuelle : **Standard_D2s_v3**
   
   **Remarque** : utilisez une taille de machine virtuelle similaire si votre abonnement ne prend pas en charge la taille spécifiée. Voici un lien menant à la documentation : <https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes>

   -   Préfixe d’étiquette DNS : **tout nom DNS valide et global unique (chaîne unique composée de lettres, de chiffres et de traits d’union, commençant par une lettre et comprenant jusqu’à 47 caractères)**

   -   Emplacement _artifacts : **accepter la valeur par défaut**
   -   Jeton SAS d’emplacement _artifacts : **laisser ce champ vide**

4. Sélectionnez **Vérifier + créer**.

5. Une fois la validation réussie, sélectionnez **Créer**.
    
6. Attendez la fin du déploiement. Ceci peut prendre environ 60 minutes.


### Tâche 2 : configurer les machines virtuelles Azure de l’environnement de labo

1. Dans la fenêtre du navigateur affichant le Portail Azure, accédez à la machine virtuelle Azure **DC1** et connectez-vous à celle-ci via le Bureau à distance. Lorsque l’invite de connexion s’ouvre, utilisez les informations d’identification suivantes :

   -   Nom d’utilisateur : **demouser**
   -   Mot de passe : **utilisez le mot de passe sécurisé que vous avez créé dans la tâche 1**

2.  Dans la session Bureau à distance de **DC1**, démarrez **Windows PowerShell ISE**, puis ouvrez le volet de script.  Ensuite, ajoutez le script suivant au volet de script et exécutez-le pour désactiver la configuration de sécurité renforcée d’Internet Explorer et le contrôle d’accès utilisateur sur les machines virtuelles Azure **DC1** et **APP1** :

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0}
    Invoke-Command -ComputerName $vmNames {Set-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 00000000}
    ```

    **Remarque :** pour exécuter plusieurs scripts PowerShell dans le même fichier, vous pouvez surligner un script spécifique et sélectionner **Exécuter la sélection** en regard du bouton de lecture vert. 

3.  Dans la fenêtre **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour installer les outils d’administration de serveur distant sur les machines virtuelles Azure **DC1* et **APP1** :

    ```pwsh

    $vmNames = @('dc1','app1')
    Invoke-Command -ComputerName $vmNames {Install-WindowsFeature RSAT -IncludeAllSubFeature} 
    ```

4.  Dans la fenêtre **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour activer TLS 1.2 sur les machines virtuelles Azure *DC1* et **APP1** :

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

5.  Dans la fenêtre **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour configurer l’authentification Windows intégrée sur le site web par défaut hébergé sur la machine virtuelle Azure **APP1** :

    ```pwsh

    $vmNames = @('app1')
    Invoke-Command -ComputerName $vmNames {Enable-WindowsOptionalFeature -Online -FeatureName IIS-WindowsAuthentication}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/anonymousAuthentication" -Name Enabled -Value False -PSPath IIS:\ -Location "Default Web Site"}
    Invoke-Command -ComputerName $vmNames {Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/windowsAuthentication" -Name Enabled -Value True -PSPath IIS:\ -Location "Default Web Site"}
    ```

### Tâche 3 : redémarrer les machines virtuelles Azure

1. Dans la fenêtre **Windows PowerShell ISE**, à partir du volet de la console, exécutez ce qui suit pour redémarrer **APP1** :

    ```pwsh

    Restart-Computer -ComputerName 'APP1'
    ```

2. Dans la fenêtre **Windows PowerShell ISE**, à partir du volet de la console, exécuter ce qui suit pour redémarrer **DC1** :

    ```pwsh
    Restart-Computer -ComputerName 'DC1'
    ```

### Tâche 5 : configurer l’Active Directory contoso.local

1. Connectez-vous à nouveau à la machine virtuelle Azure **DC1** via le Bureau à distance. Lorsque l’invite de connexion s’ouvre, utilisez les informations d’identification suivantes :

    -   Nom d’utilisateur : **demouser**

    -   Mot de passe : pass123 de **démonstration\@**
       - **Il est fortement recommandé d’entrer un mot de passe sécurisé que vous pouvez mémoriser**.

2.  Dans la session Bureau à distance de **DC1**, démarrez Internet Explorer, puis ouvrez le lien ci-dessous.

    ```
    https://github.com/microsoft/MCW-Hybrid-identity/tree/main/Archive/Hands-on%20lab/studentfiles
    ```

3. Sur la page **Créer des utilisateurs/groupes pour l’environnement de démonstration/test Active Directory**, sélectionnez le lien **CreateDemoUsers.ps1**, acceptez les termes du contrat de licence et enregistrez le script correspondant dans le système de fichiers local.

4. Sur la page **Créer des utilisateurs/groupes pour l’environnement de démonstration/test Active Directory**, sélectionnez le lien **CreateDemoUsers.csv** (directement au-dessus de la section de code PowerShell) et enregistrez le fichier CSV correspondant au même emplacement que le fichier **CreateDemoUsers.ps1**.

5. Dans la session Bureau à distance de **DC1**, démarrez l’Explorateur de fichiers, accédez au dossier dans lequel vous avez téléchargé les deux fichiers, sélectionnez-le avec le bouton droit sur le fichier **CreateDemoUsers.ps1**, puis **Propriétés**, dans la boîte de dialogue **Propriétés de CreateDemoUsers.ps1**, cochez la case **Débloquer** et sélectionnez **OK**.

6. Dans la fenêtre Explorateur de fichiers, sélectionnez à nouveau le fichier **CreateDemoUsers.ps1**, puis sélectionnez **Modifier**. 

7. Dans la fenêtre **Administrateur : Windows PowerShell ISE**, remplacez la ligne **148** de :

    ```pwsh
    $UserCount = 1000 #Up to 2500 can be created
    ```

   à
    ```pwsh
    $UserCount = 2500 #Up to 2500 can be created
    ```

8. Dans la fenêtre **Windows PowerShell ISE**, enregistrez la modification et exécutez le script **CreateDemoUsers.ps1** pour créer une hiérarchie d’unités d’organisation de l’environnement de labo et remplissez-la avec des comptes d’utilisateur test. 

9.  Dans la fenêtre **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour modifier les paramètres des comptes d’utilisateur AD que vous allez utiliser durant ce labo :

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

10. Dans la fenêtre **Windows PowerShell ISE**, ajoutez le script suivant au volet de script et exécutez-le pour créer des unités d’organisation supplémentaires nommées **Serveurs** et **Clients**, puis déplacez le compte d’ordinateur **APP1** vers le premier d’entre eux :

    ```pwsh

    New-ADOrganizationalUnit -Name 'Servers' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    New-ADOrganizationalUnit -Name 'Clients' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

    Move-ADObject -Identity 'CN=APP1,CN=Computers,DC=corp,DC=contoso,DC=com' -TargetPath 'OU=Servers,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
    ```

11. Déconnectez-vous de **DC1**.

## Exercice 2 : Intégrer une forêt Active Directory avec un locataire Azure Active Directory

### Tâche 1 : créer un locataire Azure Active Directory et activer une évaluation EMS E5

Dans cette tâche, vous allez créer un locataire Azure Active Directory avec les paramètres suivants : 

-   Nom de l’organisation : **Contoso**

-   Nom de domaine initial : n’importe quel nom de domaine valide et unique

-   Pays ou région : **États-Unis**

1. À partir de l’ordinateur labo, démarrez une nouvelle fenêtre de navigateur web et accédez au Portail Azure à l’adresse <https://portal.azure.com>, si vous n’en avez pas déjà fait.

2. Lorsque l’invite s’ouvre, connectez-vous à l’abonnement Azure dans lequel vous avez déployé des ressources durant les exercices qui précèdent le labo pratique.

3. Sur l’ordinateur labo, dans le Portail Azure, sélectionnez **+ Créer une ressource**.

4. Sur la page **Nouveau**, dans la zone de texte **Rechercher dans la Place de marché**, saisissez **Azure Active Directory** et, dans la liste des résultats, sélectionnez **Azure Active Directory**.

5. Sur la page **Azure Active Directory**, sélectionnez **Créer**.

6. Sur la page **Créer un répertoire**, entrez les paramètres suivants, puis sélectionnez **Créer** :

Onglet Informations de base :
    -   Sélectionner le type de locataire : choisissez **Azure Active Directory**

Onglet Configuration :
    -   Nom de l’organisation : **Contoso**

    -   Nom de domaine initial : n’importe quel nom de domaine valide et unique

    -   Pays ou région : **États-Unis**

7. Une fois qu’il a été créé, ouvrez **Azure Active Directory**.

8. Sur la page Vue d’ensemble, sélectionnez **Gérer le locataire**.

9. Créez un check-in dans votre répertoire nouvellement créé.

10. Sélectionnez **Basculer** en haut de l’écran.

    >**Remarque** : il peut s’écouler quelques minutes avant que tout s’affiche.

11. Sur la page **Contose : vue d’ensemble**, sélectionnez **Utilisateurs**.

12. Sachez que vous n’avez qu’un seul utilisateur ExternalAzureAD dans ce nouveau locataire.

### Tâche 2 : créer et configurer l’utilisateur Azure AD pour administrer ce répertoire

1. À partir de l’ordinateur labo, dans le Portail Azure, revenez sur la page **Contoso : vue d’ensemble**.

2. Sur la page **Contoso : vue d’ensemble**, sélectionnez **Utilisateurs** sous **Gérer** dans le volet de navigation gauche.

3. Dans le volet **Utilisateurs : tous les utilisateurs**, cliquez sur l’entrée représentant votre compte d’utilisateur.

4. Sur la page **Profil**, de votre compte d’utilisateur, sélectionnez **Modifier**.

5. Dans la section **Paramètres**, dans la liste déroulante **Emplacement d’utilisation**, sélectionnez l’entrée **États-Unis**, puis sélectionnez **Enregistrer**.

#### Créer le nouvel administrateur

6. Sur la page **Nouvel utilisateur**, assurez-vous que l’option **Créer un utilisateur** est sélectionnée, spécifiez les paramètres suivants et sélectionnez **Créer** :

    - Nom d’utilisateur : **john.doe *@your Nom de domaine de locataire Azure AD* ** où ***votre nom de domaine de locataire Azure AD*** est le nom de domaine que vous avez spécifié lors de la création du locataire Contoso Azure AD.

    - Nom : **john.doe**

    - Prénom : **John**

    - Nom : **Doe**
    
    - Mot de passe : **générer automatiquement le mot de passe**
    
    - Afficher le mot de passe : **activé**. Copiez le mot de passe.

    - Groupes : **aucun groupe sélectionné**
    
    - Rôles : **administrateur général**
    
    - Bloquer la connexion : **non**
    
    - Emplacement d’utilisation : **États-Unis**
    
    - Poste : **laisser ce champ vide**
    
    - Service : **laisser ce champ vide**

    > **Remarque** : copiez les valeurs **Nom d’utilisateur** et **Mot de passe** dans Bloc-notes Windows. Vous en aurez besoin plus tard durant ce labo.


### Tâche 5 : configurer le suffixe DNS dans la forêt Contoso Active Directory

Dans cette tâche, vous allez configurer le suffixe DNS de la forêt Contoso Active Directory pour qu’il corresponde au nom de domaine personnalisé Azure AD nouvellement vérifié.

1. Sur l’ordinateur labo, dans le Portail Azure, vérifiez que vous êtes connecté(e) au locataire Azure AD associé à l’abonnement Azure dans lequel vous avez déployé des ressources lors des exercices pratiques préalables (**répertoire par défaut**). Si ce n’est pas le cas, sélectionnez l’icône **Répertoire + Abonnement** dans la barre d’outils du Portail Azure (à droite de l’icône **Cloud Shell**) pour basculer vers ce locataire Azure AD. 

2. Dans le Portail Azure, accédez à la page de la machine virtuelle **DC1**.

3. Sur la page de la machine virtuelle **DC1**, connectez-vous à **DC1** via le Bureau à distance. Lorsque l’invite de connexion s’ouvre, utilisez le nom **demouser** et le mot de passe de **démonstration\@pass123**. 

4. Dans la session Bureau à distance de **DC1**, dans la fenêtre **Gestionnaire de serveur**, démarrez la console **Domaines et approbations Active Directory**, sous **Outils**. 

5. Dans la console **Domaines et approbations Active Directory**, sélectionnez avec bouton droit **Domaines et approbations Active Directory [DC1.corp.contoso.com]** à gauche, puis sélectionnez **Propriétés**.

6. Dans l’onglet **Suffixes UPN** de la fenêtre **Domaines et approbations Active Directory [DC1.corp.contoso.com]**, dans la zone de texte **Suffixes UPN de remplacement**, saisissez le nom du domaine personnalisé que vous avez vérifié dans la tâche précédente, sélectionnez **Ajouter**, puis **OK**.

7. Dans la session Bureau à distance de **DC1**, dans la fenêtre **Gestionnaire de serveur**, démarrez la console **Utilisateurs et ordinateurs Active Directory**, sous **Outils**. 

8. Dans la console **Utilisateurs et ordinateurs Active Directory**, développez **corp.contoso.com** à gauche et examinez la hiérarchie d’unités d’organisation du domaine et l’appartenance au groupe des groupes de domaines. 

9.  Dans la session Bureau à distance de **DC1**, démarrez Windows PowerShell ISE et, dans le volet de script, exécutez ce qui suit pour remplacer le suffixe UPN de tous les utilisateurs membres du groupe **Ingénierie** par celui correspondant au nom de domaine vérifié personnalisé du locataire Contoso Azure AD (remplacez l’espace réservé `<custom_domain_name>` par le nom réel du nom de domaine vérifié personnalisé que vous avez affecté au locataire Contoso Azure AD). 

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

### Tâche 6 : installer Microsoft Entra Connect

Dans cette tâche, vous allez installer Microsoft Entra Connect.

1. Dans la session Bureau à distance de **DC1**, dans Gestionnaire de serveur, sélectionnez **Serveur local** et vérifiez que **la configuration de sécurité renforcée d’Internet Explorer** est désactivée. Si ce n’est pas le cas, sélectionnez le lien **Activé** en regard de **la configuration de sécurité renforcée d’Internet Explorer**, basculez les paramètres **Administrateurs** sur **Désactivé**, puis sélectionnez **OK**.

2. Dans la session Bureau à distance de **DC1**, ouvrez la fenêtre **Windows PowerShell ISE** et exécutez cette commande pour installer le navigateur Chrome.

    ```pwsh
    $LocalTempDir = $env:TEMP; $ChromeInstaller = "ChromeInstaller.exe"; (new-object System.Net.WebClient).DownloadFile('http://dl.google.com/chrome/install/375.126/chrome_installer.exe', "$LocalTempDir\$ChromeInstaller"); & "$LocalTempDir\$ChromeInstaller" /silent /install; $Process2Monitor = "ChromeInstaller"; Do { $ProcessesFound = Get-Process | ?{$Process2Monitor -contains $_.Name} | Select-Object -ExpandProperty Name; If ($ProcessesFound) { "Still running: $($ProcessesFound -join ', ')" | Write-Host; Start-Sleep -Seconds 2 } else { rm "$LocalTempDir\$ChromeInstaller" -ErrorAction SilentlyContinue -Verbose } } Until (!$ProcessesFound)
    ```

2. Dans la session Bureau à distance de **DC1**, démarrez Internet Explorer, puis ouvrez le Portail Azure en cliquant sur le lien <https://portal.azure.com>.

3. Lorsque l’invite de connexion s’ouvre, entrez les informations d’identification du compte d’utilisateur Microsoft Entra **john.doe**, que vous avez copié dans Bloc-notes Windows plus tôt dans cet exercice.

4. Quand l’invite s’ouvre, entrez le mot de passe du compte d’utilisateur **john.doe**. 
  
    > **Remarque** : si vous recevez le message **Nous avons vu ce mot de passe trop souvent avant**, choisissez quelque chose de plus difficile à deviner. Vous devrez modifier le mot de passe jusqu’à ce qu’il soit suffisamment unique pour être accepté.

5. Si l’invite vous demandant **Souhaitez-vous rester connecté(e) ?** s’ouvre, sélectionnez **Non**. Vous allez être redirigé(e) vers l’interface du Portail Azure. 

6. S’il est présenté avec la boîte de dialogue **Bienvenue dans Microsoft Azure**, sélectionnez **Peut-être plus tard**. 

7. Dans le portail Microsoft Azure, recherchez **Microsoft Entra Connect**.

8. Dans les résultats de la recherche, sélectionnez **Microsoft Entra Connect**.

9.  Sur la page **Microsoft Entra Connect**, sélectionnez le lien **Télécharger Microsoft Entra Connect**.  Choisissez ensuite **Synchroniser Connect** dans le menu.

10. Sur la page web **Microsoft Azure Active Directory Connecter v2** du site de téléchargements Microsoft, sélectionnez **Télécharger**.

11. Lorsque vous êtes invité(e) à exécuter ou enregistrer **AzureAD Connecter.msi**, sélectionnez **Exécuter**. Cela télécharge le fichier et démarre automatiquement l’Assistant **Microsoft Azure Active Directory Connect**. 

12. Sur la page **Bienvenue dans Azure AD Connect**, cochez la case **J’accepte les termes du contrat de licence et l’avis de confidentialité**, puis sélectionnez **Continuer**.

13. Sur la page **Paramètres express**, sélectionnez le bouton **Personnaliser**.

14. Sur la page **Installer les composants requis**, laissez toutes les options de configuration facultatives décochées, puis sélectionnez **Installer**.

15. Dans la page **Connexion utilisateur**, sélectionnez l’option **Authentification directe**, cochez les cases **Activer l’authentification unique**, puis sélectionnez **Suivant**.

16. Sur la page **Connecter à Azure AD**, connectez-vous à l’aide des informations d’identification du compte **john.doe**, puis sélectionnez **Suivant**.

17. Sur la page **Connecter vos répertoires**, vérifiez que l’entrée **corp.contoso.com** s’affiche dans la **liste déroulante FORÊT**, puis sélectionnez **Ajouter un répertoire**. Dans le **compte de forêt AD**, vérifiez que l’option **Créer un compte AD** est sélectionnée, dans le champ **NOM D’UTILISATEUR ADMINISTRATEUR D’ENTREPRISE**, tapez **CORP.CONTOSO.COM\\demouser**, dans le champ **MOT DE PASSE**, tapez **le mot de passe de démonstration\@pass123**, puis sélectionnez **OK**.


18. Sur la page **Connexion de vos annuaires**, cliquez sur **Suivant**.

19. Sur la page **Configuration de connexion Azure AD**, vérifiez que votre nom de domaine personnalisé est répertorié comme **suffixe UPN Active Directory** vérifié et que l’entrée **userPrincipalName** apparaît dans la **liste déroulante NOM D’UTILISATEUR PRINCIPAL**. L’avertissement indiquant **Les utilisateurs ne pourront pas se connecter à Azure AD avec des informations d’identification locales si le suffixe UPN ne correspond pas à un nom de domaine vérifié** peut apparaître. Cochez la case **Continuer sans que tous les suffixes UPN pour vérifier le domaine ne correspondent**, puis sélectionnez **Suivant**. 

    >**Remarque** : Cela est prévu, étant donné que certains utilisateurs sont toujours configurés avec le **suffixe UPN contoso.local**, qui n’est pas routable et ne peut pas être configuré en tant que nom de domaine personnalisé vérifié d’un locataire Azure AD.

20. Sur la page **Filtrage du domaine et de l’unité d’organisation**, choisissez **Synchroniser les domaines sélectionnés et les unités d’organisation sélectionnées**, puis vérifiez que seules l’unité d’organisation ** DemoAccounts** et toutes ses unités d’organisation enfants sont sélectionnées, avant de sélectionner **Suivant**. 


21. Sur la page **Identification de manière unique de vos utilisateurs**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**. 


22. Sur la page **Filtrer les utilisateurs et appareils**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**. 

23. Sur la page **Fonctionnalités facultatives**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**.

24. Sur la page **Activer l’authentification unique**, sélectionnez **Entrer les informations d’identification**, dans la boîte de dialogue **Informations d’identification de la forêt**, connectez-vous avec le nom d’utilisateur de **démonstration** CORP\\ et **le mot de passe de démonstration\@pass123**, puis sélectionnez **Suivant**.


25. Sur la page **Prêt à configurer**, vérifiez que la case **Démarrez le processus de synchronisation** une fois la case est **DÉCOCHÉE**, puis cliquez sur **Installer**.


   > **Remarque** : vous allez configurer le filtrage au niveau des attributs avant d’activer le processus de synchronisation.

   > **Remarque** : cette installation prend environ 2 minutes.

26. Sur la page **Configuration effectuée**, sélectionnez **Quitter**.


### Tâche 7 : Activer la corbeille Active Directory

Dans cette tâche, vous allez activer la corbeille dans le domaine Contoso Active Directory. 

1. Dans la session Bureau à distance de **DC1**, dans le menu Outils de la console Gestionnaire de serveur, démarrez **Centre d’administration Active Directory**.


2. Dans la **console du Centre d’administration Active Directory**, sélectionnez **corp (local)** à gauche, puis sélectionnez **Activer la corbeille**. Quand vous êtes invité à confirmer, sélectionnez **OK**.


3. Lorsque vous êtes invité(e) à actualiser le centre d’administration AD, sélectionnez **OK**.

   > **Remarque** : pour plus d’informations sur les avantages de la corbeille dans les scénarios hybrides, reportez-vous à <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-recycle-bin>

### Tâche 8 : configurer le filtrage au niveau de l’attribut azure AD Connect

Dans cette tâche, vous allez configurer le filtrage au niveau de l’attribut Azure AD Connect, qui limite la synchronisation des comptes d’utilisateur avec le suffixe UPN correspondant au nom de domaine personnalisé du locataire Azure AD cible.

   > **Remarque** : l’option de filtrage positif nécessite deux règles de synchronisation. L’une d’elles détermine l’étendue correcte des objets à synchroniser. L’autre règle de synchronisation fourre-tout exclut tous les objets qui n’ont pas encore été identifiés en tant qu’objets à synchroniser.

1. Dans la session Bureau à distance de **DC1**, démarrez l’**Éditeur de règles de synchronisation** sous **Azure AD Connect** dans le menu Démarrer.


2. Dans la fenêtre Éditeur de règles de synchronisation, sur la page **Afficher et gérer vos règles de synchronisation**, vérifiez que **Entrant** apparaît dans la liste déroulante **Direction** et sélectionnez **Ajouter une nouvelle règle**. Cela lance l’Assistant **Créer une règle de synchronisation entrante**.


3. Sur la page **Créer une règle de synchronisation entrante : description**, entrez les paramètres suivants, puis sélectionnez **Suivant** :

    - Nom : **personnalisez à partir d’AD : filtre UPN**

    - Description : **règle entrante personnalisée : inclut les utilisateurs avec un UPN défini pour correspondre au domaine personnalisé Azure AD**

    - Système connecté : **corp.contoso.com**

    - Type d’objet système connecté : **utilisateur**

    - Type d’objet métaverse : **personne**

    - Type de lien : **invitation**

    - Antécédent : **50**

    - Étiquette : **laisser ce champ vide**

    - Activer la synchronisation du mot de passe : **laisse ce champ vide**

    - Désactivé : **laisser ce champ vide**


4. Sur la page **Créer un filtre de contrôle d’entrée**, sélectionnez **Ajouter un groupe**, **Ajouter une clause**, indiquez ce qui suit, puis sélectionnez **Suivant** :

    - Attribut : **userPrincipalName**

    - Opérateur : **ENDSWITH**

    - Valeur: **\@\<your custom domain name>**

5. Sur la page **Règles d’invitation**, sélectionnez **Suivant**.

6. Sur la page **Transformations**, sélectionnez **Ajouter une transformation**, spécifiez les éléments suivants, puis sélectionnez **Ajouter** :

    - FlowType : **Constant**

    - Attribut cible : **cloudFiltered**

    - Source : **Faux**

7. Une fois présenté avec une boîte de dialogue **Avertissement** affichant un message qui indique **Une importation et une synchronisation complètes seront exécutées sur « corp.contoso.com » au cours de votre prochain cycle de synchronisation**, sélectionnez **OK**.

   > **Remarque** : vous devez revenir à l’affichage et gérer votre interface de règles de synchronisation, avec la nouvelle règle répertoriée en haut de la liste des règles. 

8. Dans la fenêtre **Éditeur de règles de synchronisation**, sur la page **Afficher et gérer vos règles de synchronisation**, vérifiez que **Entrant** apparaît dans la liste déroulante **Direction** et sélectionnez **Ajouter une nouvelle règle**. Cela lance l’Assistant **Créer une règle de synchronisation entrante**.

9. Dans la page **Description**, indiquez les paramètres suivants et sélectionnez **Suivant** :

    - Nom : **personnalisez à partir d’AD : filtre UPN fourre-tout**

    - Description : **règle entrante personnalisée : exclut tous les utilisateurs avec un UPN défini pour correspondre au domaine personnalisé Azure AD**

    - Système connecté : **corp.contoso.com**

    - Type d’objet système connecté : **utilisateur**

    - Type d’objet métaverse : **personne**

    - Type de lien : **invitation**

    - Antécédent : **51**

    - Étiquette : **laisser ce champ vide**

    - Activer la synchronisation du mot de passe : **laisse ce champ vide**

    - Désactivé : **laisser ce champ vide**


10. Sur la page **Filtre d’étendue**, sélectionnez **Suivant**.

11. Sur la page **Règles d’invitation**, sélectionnez **Suivant**.

12. Sur la page **Transformations**, sélectionnez **Ajouter une transformation**, spécifiez les éléments suivants, puis sélectionnez **Ajouter** :

    - FlowType : **Constant**

    - Attribut cible : **cloudFiltered**

    - Source : **Vrai**

13. Une fois présenté avec une boîte de dialogue **Avertissement** affichant un message qui indique **Une importation et une synchronisation complètes seront exécutées sur « corp.contoso.com » au cours de votre prochain cycle de synchronisation**, sélectionnez **OK**.

    >**Remarque** : vous devez revenir à l’interface **Afficher et gérer votre interface de règles de synchronisation**, avec la nouvelle règle répertoriée en haut de la liste des règles. 

### Tâche 9 : vérifier la synchronisation du répertoire

1. Dans la session Bureau à distance de **DC1**, double-sélectionnez le raccourci bureau **Azure AD Connect**.

2. Sur la page **Bienvenue dans Azure AD Connect**, cliquez sur **Continuer**. 

3. Sur la page **Tâches supplémentaires**, sélectionnez **Personnalisation des options de synchronisation**, puis cliquez sur **Suivant**.

4. Sur la page **Connecter à Azure AD**, connectez-vous à l’aide des informations d’identification du compte **john.doe**, puis sélectionnez **Suivant**.

5. Dans la page **Connexion de vos annuaires**, cliquez sur **Suivant**.

6. Dans la page **Filtrage par domaine ou unité d’organisation**, cliquez sur **Suivant**. 

7. Sur la page **Fonctionnalités facultatives**, acceptez les paramètres par défaut, puis cliquez sur **Suivant**.

8. Sur la page **Activer l’authentification unique**, sélectionnez **Suivant**.

9. Sur la page **Prêt à configurer**, cochez la case **Démarrez le processus de synchronisation une fois la configuration terminée**, puis cliquez sur **Installer**.

10. Sur la page **Configuration effectuée**, sélectionnez **Quitter**.

11. Dans la session Bureau à distance de **DC1**, dans la fenêtre Microsoft Edge affichant le Portail Azure, accédez à la page **Utilisateurs : tous les utilisateurs** du locataire Azure AD Contoso.

12. Sur la page **Utilisateurs : tous les utilisateurs**, notez que la liste des objets utilisateur inclut tous les comptes d’utilisateur avec le suffixe UPN correspondant au nom de domaine personnalisé du locataire Azure AD. Vous devrez peut-être actualiser la page ou attendre quelques minutes pour voir la modification.

13. Dans le Portail Azure, accédez à la page **Groupes : tous les groupes** du locataire Contoso Azure AD et notez que tous les groupes de domaines corp.contoso.com ont également été synchronisés. 

14. Dans le Portail Azure, accédez à la page **Contoso : Azure AD Connect** et sélectionnez **Azure AD Connect** sur la gauche. Vérifiez que les paramètres suivants présents : 

    - État de synchronisation d’Azure AD Connect : **Activé** 
  
    - Dernière synchronisation : **il doit s’agir d’un horodatage d’une certaine sorte**. 
  
    - Synchronisation de hachage de mot de passe : **désactivé** 
  
    - Fédération : **désactivée**
   
    - Authentification unique transparente : **activée pour 1 domaine** 
  
    - Authentification directe : **Activée avec 1 agent**

   > **Remarque** : dans un environnement de production, vous devez installer des agents supplémentaires pour la redondance. Pour plus d’informations, consultez <https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-quick-start>.

### Tâche 10 : configurer des appareils joints à Azure AD hybrides

Dans cette tâche, vous allez configurer les options de synchronisation d’appareils Azure AD Connect.

1. Dans la session Bureau à distance de **DC1**, double-sélectionnez le raccourci bureau **Azure AD Connect**.

2. Sur la page **Bienvenue dans Azure AD Connect**, cliquez sur **Continuer**. 

3. Sur la page **Tâches supplémentaires**, sélectionnez **Configurer les options de l’appareil**, puis **Suivant**.

4. Sur la page **Vue d’ensemble**, passez en revue les informations relatives à la **jonction Azure AD hybride** et à la **réécriture d’appareil**, puis sélectionnez **Suivant**.

5. Sur la page **Connecter à Azure AD**, connectez-vous à l’aide des informations d’identification du compte **john.doe**, puis sélectionnez **Suivant**.

6. Sur la page **Options de l’appareil**, sélectionnez **Configurer la jonction Azure AD Hybride**, puis **Suivant**. 

7. Sur la page **système d’exploitation de l’appareil**, cochez les cases des **appareils joints à un domaine Windows 10 ou ultérieur** et des **appareils joints à un domaine Windows de niveau inférieur pris en charge**, puis sélectionnez **Suivant**. 

   > **Remarque** : les appareils Windows de niveau inférieur sont pris en charge uniquement si vous utilisez l’authentification unique transparente pour les domaines managés ou un service de fédération tel qu’AD FS pour les domaines fédérés.

8. Sur la page **Configuration SCP**, cochez la case **corp.contoso.com** de la forêt Active Directory, sélectionnez l’entrée **Azure Active Directory** dans la liste déroulante **Service d’authentification**, puis sélectionnez **Ajouter**.

9. Lorsque l’invite vous demande de saisir les informations d’identification d’administrateur d’entreprise pour corp.contoso.com, dans la boîte de dialogue **Sécurité Windows**, connectez-vous avec le nom d’utilisateur **CORP\\demouser** et **le mot de passe de démonstration\@pass123**.

10. Sur la page **Configuration SCP**, sélectionnez **Suivant**.

11. Sur la page **Prêt à configurer**, sélectionnez **Configurer**.

12. Sur la page **Configuration terminée**, vérifiez que la tâche s’est terminée correctement et sélectionnez **Quitter**.


### Tâche 11 : effectuer la jonction Azure AD Hybride

1. Sur l’ordinateur labo, dans le Portail Azure, vérifiez que vous êtes connecté(e) au locataire Azure AD associé à l’abonnement Azure dans lequel vous avez déployé des ressources lors des exercices pratiques préalables (**répertoire par défaut**). Si ce n’est pas le cas, sélectionnez l’icône **Répertoire + Abonnement** dans la barre d’outils du Portail Azure (à droite de l’icône **Cloud Shell**) pour basculer vers ce locataire Azure AD. 

2. Dans le Portail Azure, accédez à la page de la machine virtuelle **APP1**.

3. Sur la page de la machine virtuelle **APP1**, connectez-vous à **APP1** via le Bureau à distance. Lorsque l’invite de connexion s’ouvre, utilisez le nom d’utilisateur **AGAyers\@<custom_domain_name>** avec le ****demo@pass123mot de passe **<custom_domain_name>** (l’espace réservé représente le nom de domaine DNS personnalisé que vous avez affecté au locataire Contoso Azure AD précédemment dans cet exercice).

4. Dans la session Bureau à distance de **APP1**, dans la fenêtre **Gestionnaire de serveur**, démarrez le **Planificateur de tâches**, sous **Outils**. 


5. Dans la console du **Planificateur de tâches**, accédez à la **Bibliothèque du Planificateur de tâches** > **Microsoft** > **Windows** > **Workplace Join**. À partir de là, activez puis exécutez la **tâche Automatic-Device-Join** . 


6. Basculez vers la session Bureau à distance de **DC1** et, à partir du volet console de la fenêtre Windows PowerShell ISE, démarrez la synchronisation DELTA d’Azure AD Connect en exécutant ce qui suit :

   ```pwsh
   Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'
   
   Start-ADSyncSyncCycle -PolicyType Delta
   ```

7. Revenez à la session Bureau à distance d’**APP1** et démarrez une **invite de commandes**.

8. Dans la fenêtre d’invite de commandes, cochez la case de l’état d’inscription Azure AD d’APP1 en exécutant ce qui suit : 

   ```
   dsregcmd /status
   ```

9. Vérifiez que la sortie de la commande ressemble à l’exemple suivant :

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
11. Revenez à la session Bureau à distance de **DC1**. Dans la fenêtre du navigateur Edge affichant le Portail Azure, accédez à la page **Appareils : tous les appareils** du locataire Contoso Azure AD et vérifiez qu’il existe une entrée représentant le serveur APP1, avec le **Type de jonction** défini sur **Jointure hybride Azure AD**.

   > **Remarque** : vous devrez peut-être attendre que l’état de l’inscription Azure AD soit correctement signalé et que son objet Azure AD s’affiche dans le Portail Azure.


