---
lab:
  title: "16 - Utiliser Azure\_Key\_Vault pour les identités managées"
  learning path: '02'
  module: Module 02 - Implement an Authentication and Access Management Solution
---

# Labo 16 - Utiliser Azure Key Vault pour les identités managées

**Remarque** : ce labo nécessite un pass Azure. Consultez le labo 00 pour obtenir des instructions.

## Scénario de l’exercice

Lorsque vous utilisez les identités managées pour vos ressources Azure, votre code peut obtenir des jetons d’accès pour vous authentifier sur des ressources prenant en charge l’authentification Azure AD.Toutefois, tous les services Azure ne prennent pas en charge l’authentification Azure AD. Pour utiliser des identités managées pour les ressources Azure avec ces services, stockez les informations d’identification des services dans Azure Key Vault, puis utilisez les identités managées afin d’accéder à Key Vault pour récupérer les informations d’identification.

#### Durée estimée : 20 minutes

### Exercice 1 - Utiliser Azure Key Vault pour gérer les identités de machine virtuelle

#### Tâche 1 - Créer une machine virtuelle Windows

1. Accédez à l’URL suivante : [https://portal.azure.com](https://portal.azure.com)

1. Sélectionnez **+ Créer une ressource**.

1. Saisissez **Windows Client** dans la barre de recherche Rechercher de la Place de marché.

1. Sélectionnez **Windows Client** puis, dans la liste déroulante du plan, choisissez **Windows 10 Entreprise, version 22H2 - x64 Gen 1**. Choisissez ensuite **Créer**.

1. Créez ensuite un nom d’utilisateur et un mot de passe Administrateur pour la machine virtuelle dans l’onglet Informations de base.

1. Sous l’onglet **Gestion**, cochez la case pour **Activer l’identité managée affectée par le système**.

1. Parcourez le reste de l’expérience de création d’une machine virtuelle. 

1. Sélectionnez Créer.

#### Tâche 2 - Créer un coffre de clés

1. Connectez-vous au [https://portal.azure.com]( https://portal.azure.com) à l’aide d’un compte d’administrateur général.

1. En haut de la barre de navigation de gauche, sélectionnez Créer une ressource.

1. Dans la zone Rechercher dans la Place de marché, saisissez **Coffre de clés**.  

1. Sélectionnez **Coffre de clés** dans les résultats.

1. Sélectionnez **Créer**.

1. Remplissez toutes les informations requises, comme indiqué ci-dessous. Veillez à choisir l’abonnement que vous utilisez pour ce labo.
    **Remarque** : le nom du coffre de clés doit être unique. Recherchez une coche verte à droite du champ.

 - **Groupe de ressources** : sc300KeyVaultrg
 - **Nom du coffre de clés** - *toutevaleurunique*
 - Sur la page **Configuration de l’accès**, sélectionnez la case d’option **Stratégie d’accès au coffre**.
1. Sélectionnez **Revoir + créer**.

1. Sélectionnez **Create** (Créer).


#### Tâche 3 - Créer un secret

1. Accédez au coffre de clés que vous venez de créer.

1. Sélectionnez **Secrets**.

1. Sélectionnez **+ Générer/importer**.

1. Dans l’écran Créer un secret, dans les Options de chargement, laissez **Manuel** sélectionné.

1. Entrez un nom et une valeur pour le secret.  Vous pouvez choisir la valeur de votre choix. 

1. Laissez les champs pour la date d’activation et la date d’expiration vides, puis pour Activé, laissez la valeur Oui. 

1. Sélectionnez **Créer** pour créer le secret.

#### Tâche 4 - Autoriser l’accès à Key Vault

1. Accédez au coffre de clés que vous venez de créer.

1. Dans le menu de gauche, sélectionnez **Stratégies d’accès**.

1. Sélectionnez **+ Créer**.

1. Dans la section Ajouter une stratégie d’accès sous Configurer à partir du modèle (facultatif), choisissez Gestion des secrets dans le menu déroulant.

1. Pour **Sélectionner le principal**, choisissez **Aucun sélectionné** pour ouvrir la liste de sélection des principaux. Dans le champ de recherche, entrez le nom de la machine virtuelle que vous avez créée dans la tâche 2.  Sélectionnez la machine virtuelle dans la liste des résultats, puis choisissez Sélectionner.

1. Sélectionnez **Ajouter**.

1. Sélectionnez **Enregistrer**.

#### Tâche 5 - Accéder aux données contenant le secret Key Vault avec PowerShell

1. Sur la machine virtuelle du labo, ouvrez PowerShell.  

1. Dans PowerShell, appelez la requête web sur le client pour obtenir le jeton de l’hôte local dans le port spécifique pour la machine virtuelle.  

    ```
    $Response = Invoke-RestMethod -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"}
    ```

1. Ensuite, extrayez le jeton d’accès de la réponse.  

    ```
    $KeyVaultToken = $Response.access_token
    ```

1. Utilisez la commande Invoke-WebRequest de PowerShell pour récupérer le secret que vous avez créé précédemment dans Key Vault et transmettre le jeton d’accès vers l’en-tête d’autorisation.  Vous avez besoin de l’URL de votre Key Vault qui se trouve dans la section Bases de la page Vue d’ensemble de Key Vault.  Rappel : l’URI du Key Vault se trouve sous l’onglet Vue d’ensemble.

    ```
    Invoke-RestMethod -Uri https://<your-key-vault-URI>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}
    ```
1. Vous devez recevoir une réponse semblable à la suivante : 
    ```
    'My Secret' https://mi-lab-vault.vault.azure.net/secrets/mi-test/50644e90b13249b584c44b9f712f2e51 @{enabled=True; created=16…
    ```
1. Ce secret permet de s’authentifier auprès des services qui nécessitent un nom et un mot de passe.
