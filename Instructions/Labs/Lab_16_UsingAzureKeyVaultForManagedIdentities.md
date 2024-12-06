---
lab:
  title: "16\_: Utiliser Azure Key Vault pour les identités managées"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 16 : Utiliser Azure Key Vault pour les identités managées

### Type de connexion = Connexion à la ressource Azure

## Scénario de labo

Lorsque vous utilisez des identités managées pour les ressources Azure, votre code peut obtenir des jetons d’accès pour s’authentifier auprès des ressources qui prennent en charge l’authentification Microsoft Entra.Toutefois, tous les services Azure ne prennent pas en charge l’authentification Microsoft Entra. Pour utiliser des identités managées pour les ressources Azure avec ces services, stockez les informations d’identification des services dans Azure Key Vault, puis utilisez des identités managées afin d’accéder à Key Vault pour récupérer les informations d’identification.

#### Durée estimée : 20 minutes

### Exercice 1 : Utiliser Azure Key Vault pour gérer les identités de la machine virtuelle

#### Tâche 1 : créer un coffre de clés

1. Connectez-vous à [https://portal.azure.com]( https://portal.azure.com) en utilisant un compte d’administrateur général.

1. En haut de la barre de navigation de gauche, sélectionnez **+ Créer une ressource**.

1. Dans la zone Rechercher dans la Place de marché, tapez **Key Vault**.  

1. Sélectionnez **Coffre de clés** dans les résultats.

1. Sélectionnez **Créer**.

1. Fournissez toutes les informations requises. Veillez à choisir l’abonnement que vous utilisez pour ce labo.
    **Remarque** : le nom du coffre de clés doit être unique. Recherchez une case verte à droite du champ.

 - **Groupe de ressources** - rgSC300KeyVault
 - **Nom du coffre de clés** - *anyuniquevalue*
 - Sur la page **Configuration d’accès**, sélectionnez le bouton radio **Stratégie d’accès au coffre**.
1. Sélectionnez **Revoir + créer**.

1. Sélectionnez **Créer**.

#### Tâche 2 : créer une machine virtuelle Windows

1. Sélectionnez **+ Créer une ressource**.

1. Saisissez **Windows 11** dans la barre de recherche de la Place de marché.

1. Sélectionnez **Windows 11** et, dans la liste déroulante du plan, choisissez **Windows 11 Entreprise, version 22H2**. Choisissez ensuite **Créer**.

  | Champ | Valeurs |
  | :--   | :--    |
  | Nom de la machine virtuelle | vmKeyVault |
  | Options de disponibilité | Aucune redondance de l’infrastructure requise |
  | Nom de l’utilisateur administrateur | adminKeyVault |
  | Mot de passe | Définissez un mot de passe sécurisé dont vous pouvez vous souvenir. |
  | Gestion des licences | Confirmez que vous disposez d’une licence éligible. |

1. Utilisez le bouton **Suivant** pour accéder à l’onglet **Gestion**.

1. Sous l’onglet **Gestion**, cochez la case en regard de **Activer l’identité managée affectée par le système**.

1. Parcourez le reste de l’expérience de création d’une machine virtuelle. 

1. Sélectionnez **Examiner et créer**, puis sélectionnez **Créer**.

#### Tâche 3 : créer une clé secrète

1. Accédez au coffre de clés que vous venez de créer.

1. Ouvrez **Objets** dans le menu de gauche, puis sélectionnez **Secrets**.

1. Sélectionnez **+ Générer/importer**.

1. Dans l’écran Créer un secret, dans les Options de chargement laissez **Manuel** sélectionné.

1. Entrez un nom et une valeur pour le secret.  Vous pouvez choisir la valeur de votre choix. 

1. Laissez les champs pour la date d’activation et la date d’expiration vides, puis pour Activé, laissez la valeur Oui. 

1. Sélectionnez **Créer** pour créer le secret.

#### Tâche 4 : autoriser l’accès à Key Vault

1. Accédez au coffre de clés que vous venez de créer.

1. Dans le menu de gauche, sélectionnez **Stratégies d’accès**.

1. Sélectionnez **+ Créer**.

1. Dans la section Ajouter une stratégie d’accès sous Configurer à partir du modèle (facultatif), choisissez **Gestion des secrets** dans le menu déroulant.

1. Utilisez le bouton Suivant pour accéder à l’onglet **Principal**.

1. Dans le champ de recherche, entrez le nom de la machine virtuelle que vous avez créée lors de la tâche 2 **vmKeyVault**.  Sélectionnez la machine virtuelle dans la liste des résultats, puis choisissez Sélectionner.

1. Utilisez le bouton Suivant pour accéder à l’onglet **Examiner et créer**.

1. Sélectionnez **Créer**.

#### Tâche 5 : accéder aux données avec un coffre de clés secret à l’aide de PowerShell

1. Accédez à **vmKeyVault** et utilisez RDP pour vous connecter à votre machine virtuelle en tant qu’**adminKeyVault**.

1. Sur la machine virtuelle du labo, ouvrez PowerShell.  

1. Dans PowerShell, appelez la requête web sur le client pour obtenir le jeton de l’hôte local dans le port spécifique pour la machine virtuelle.  

    ```
    $Response = Invoke-RestMethod -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"}
    ```

1. Ensuite, extrayez le jeton d’accès de la réponse.  

    ```
    $KeyVaultToken = $Response.access_token
    ```

1. Utilisez la commande Invoke-WebRequest de PowerShell pour récupérer la clé secrète que vous avez créée précédemment dans Key Vault et transmettre le jeton d’accès vers l’en-tête d’autorisation.  Vous avez besoin de l’URL de votre Key Vault qui se trouve dans la section Bases de la page Vue d’ensemble de Key Vault.  Rappel : l’URI pour Key Vault se trouve sous l’onglet Vue d’ensemble.

  - URI Key Vault : obtenir à partir de la page Vue d’ensemble des coffres de clés dans le portail Azure
  - Nom secret : obtenir à partir de la page Objets - Secrets dans le coffre de clés

    ```
    Invoke-RestMethod -Uri https://<your-key-vault-URI>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}
    ```
1. Vous devez obtenir une réponse similaire à la sortie suivante. 
    ```
    'My Secret' https://mi-lab-vault.vault.azure.net/secrets/mi-test/50644e90b13249b584c44b9f712f2e51 @{enabled=True; created=16…
    ```
1. Cette clé secrète peut être utilisée pour s’authentifier auprès des services qui nécessitent un nom et un mot de passe.
