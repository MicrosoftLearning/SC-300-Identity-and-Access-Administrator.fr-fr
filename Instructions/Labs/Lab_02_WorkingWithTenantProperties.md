---
lab:
  title: "02\_- Utilisation des propriétés du locataire"
  learning path: '01'
  module: Module 01 - Implement an Identity Management Solution
---

# Labo 02 - Utilisation des propriétés du locataire

## Scénario de l’exercice

Vous devez identifier et mettre à jour les différentes propriétés associées à votre locataire.

#### Durée estimée : 15 minutes

### Exercice 1 - Créer un sous-domaine personnalisé 

#### Tâche 1 - Créer un nom de sous-domaine personnalisé

1. Accédez à [https://portal.azure.com](https://portal.azure.com) et connectez-vous à l’aide d’un compte Administrateur général pour l’annuaire.

1. Sélectionnez l’icône **Afficher le menu du portail**, puis sélectionnez **Azure Active Directory**.

    ![Menu du portail Azure avec Azure Active Directory sélectionné](./media/azure-portal-menu-aad.png)

1. Dans la section **Gérer** d’**Azure AD**, sélectionnez **Noms de domaine personnalisés**.

1. Sélectionnez **Ajouter un domaine personnalisé**.

1. Dans le champ **Nom de domaine personnalisé**, créez un sous-domaine personnalisé pour le locataire de labo en plaçant **sales** devant le nom de domaine **onmicrosoft.com**.  Le format se présente comme suit :

    ```
    sales.labtenant.onmicrosoft.com
    ```

1. Sélectionnez **Ajouter un domaine** pour ajouter le sous-domaine.


### Exercice 2 - Modifier le nom d’affichage du locataire

#### Tâche 1 - Définir le nom et le contact technique du locataire

1. Dans Azure Active Directory, dans le menu de navigation gauche, dans la section **Gérer**, sélectionnez **Propriétés**.

1. Modifiez les propriétés **Nom** et **Contact technique** du locataire dans la boîte de dialogue.

    | **Paramètre** | **Valeur** |
    | :--- | :--- |
    | Nom | Contoso Marketing |
    | Contact technique | `your Global admin account` |

1. Sélectionnez **Enregistrer** pour mettre à jour les propriétés du locataire.

   **Vous remarquerez que le nom change immédiatement à la fin de l’enregistrement.**

#### Tâche 2 - Passer en revue le pays ou la région et d’autres valeurs associées à votre locataire

1. Sur la page **Azure Active Directory**, dans la section Gérer, sélectionnez **Propriétés**.

2. Sous **Propriétés du locataire**, recherchez **Pays ou région** et passez en revue les informations.

    **IMPORTANT** - Quand le locataire est créé, le pays ou la région sont spécifiés à ce stade. Ce paramètre ne peut pas être modifié ultérieurement.

3. Sur la page **Propriétés**, sous **Propriétés du locataire**, recherchez **Emplacement** et passez en revue les informations.

    ![Capture d’écran affichant la page Propriétés d’Azure Active Directory avec les paramètres Pays ou région et Emplacement mis en évidence](./media/azure-active-directory-properties-country-location.png)

#### Tâche 3 - Rechercher l’ID de locataire

Les abonnements Azure comprennent une relation d’approbation avec Azure Active Directory (Azure AD). Azure AD est approuvé pour authentifier les utilisateurs, les services et les appareils pour l’abonnement. Chaque abonnement est associé à un ID de locataire et il existe plusieurs façons de trouver l’ID de locataire pour votre abonnement.

1. Sur la page **Azure Active Directory**, dans la section Gérer, sélectionnez **Propriétés**.

2. Sous **Propriétés du locataire**, localisez **ID du locataire**. Il s’agit de l’identificateur unique de votre locataire.

    ![Capture d’écran affichant la page Propriétés du locataire avec la zone ID de locataire mise en évidence](./media/portal-tenant-id.png)

### Exercice 3 - Définir vos informations de confidentialité

#### Tâche 1 - Ajouter vos informations de confidentialité sur Azure AD, y compris le contact international chargé de la confidentialité et l’URL de la déclaration de confidentialité

Microsoft vous recommande vivement d’ajouter votre contact international chargé de la confidentialité et la déclaration de confidentialité de votre organisation, pour que vos employés internes et invités externes puissent consulter vos stratégies. Étant donné que les déclarations de confidentialité sont particulièrement créées et adaptées à chaque entreprise, nous vous recommandons vivement de contacter un conseil juridique à des fins d’assistance.

   **REMARQUE** - Pour plus d’informations sur l’affichage ou la suppression de données personnelles, rendez-vous surhttps://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-azure[](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-azure). Pour plus d’informations sur le RGPD, rendez-vous sur [https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Vous ajoutez les informations de confidentialité de votre organisation dans la zone **Propriétés** d’Azure AD. Accéder à la zone Propriétés et ajouter vos informations de confidentialité :

1. Sur la page **Azure Active Directory**, dans la section Gérer, sélectionnez **Propriétés**.

    ![Capture d’écran affichant les propriétés du locataire avec les zones Contact technique, Contact international chargé de la confidentialité et URL de la déclaration de confidentialité mises en évidence](./media/properties-area.png)

2. Ajoutez vos informations de confidentialité pour vos employés :

- **Contact international chargé de la confidentialité** - `AllanD@`**votre domaine de labo Azure**
     - Allan Deyoung est un utilisateur intégré dans votre locataire de labo Azure qui travaille en tant qu’administration informatique. Nous l’utiliserons comme contact chargé de la confidentialité.
     - Cette personne est également la personne que Microsoft contacte en cas de fuite de données. Si aucune personne n’est répertoriée ici, Microsoft contacte vos administrateurs généraux.

- **URL de la déclaration de confidentialité.** -  <https://github.com/MicrosoftLearning/SC-300-Identity-and-Access-Administrator/blob/master/Allfiles/Labs/Lab2/SC-300-Lab_ContosoPrivacySample.pdf>

     - L’exemple de fichier PDF de confidentialité est fourni votre répertoire de labos.
     - Tapez le lien vers le document de votre organisation qui décrit la façon dont elle gère la confidentialité des données des utilisateurs internes et invités externes.

    **IMPORTANT** - Si vous n’incluez pas votre propre déclaration de confidentialité ou votre contact chargé de la confidentialité, vos invités externes voient le texte inclus dans la zone Révision des autorisations, à savoir **<nom de votre organisation\>** n’a fourni aucun lien vers ses conditions pour vous permettre de les examiner. Par exemple, un utilisateur invité voit ce message quand il reçoit une invitation à accéder à une organisation par le biais d’une B2B.

    ![Zone Révision des autorisations B2B Collaboration avec message](./media/active-directory-no-privacy-statement-or-contact.png)

3. Sélectionnez **Enregistrer**.

#### Tâche 2 - Vérifier votre déclaration de confidentialité

1. Revenez à l’écran d’accueil Azure - Tableau de bord.
2. Dans le coin supérieur droit de l’interface utilisateur, sélectionnez votre nom d’utilisateur.
3. Choisissez **Afficher le compte** dans le menu déroulant.

     **Un nouvel onglet de navigateur s’ouvre automatiquement.**

4. Sélectionnez **Paramètres & confidentialité** dans le menu de gauche.
5. Sélectionnez **Confidentialité**.
6. Sous l’**avis de l’organisation**, sélectionnez l’élément **Afficher** en regard de la déclaration de confidentialité de l’organisation Contoso Marketing.

     **Un nouvel onglet de navigateur s’ouvre avec le fichier PDF de confidentialité que vous avez ajouté en lien.**

7. Passez en revue l’exemple de déclaration de confidentialité.
8. Fermez l’onglet du navigateur qui contient le fichier PDF.
9. Fermez l’onglet du navigateur affichant les éléments **Mon compte**.
