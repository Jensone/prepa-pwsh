# prepa-pwsh

## Exercice 1 : Prise en main de PowerShell – Lecture et traitement d’un fichier texte

### Objectif  
Se familiariser avec la syntaxe PowerShell : variables, boucles, conditions et fonctions.

### Instructions

1. **Préparation du fichier d’entrée :**  
   - Créez un fichier texte nommé `noms.txt` contenant plusieurs noms (un par ligne), par exemple :  
     ```
     Alice
     Bob
     Charlie
     David
     Émilie
     ```
   
2. **Écriture du script :**  
   - Créez un script PowerShell (`Exercice1.ps1`) qui :
     - Lit le contenu du fichier `noms.txt`.
     - Parcourt chaque ligne et convertit le nom en majuscules.
     - Affiche le nom converti.
   - Vous pouvez utiliser la commande `Get-Content` pour lire le fichier, puis une boucle `foreach` pour traiter chaque nom.
     
   - **Variante :** Ajoutez une fonction qui prend une chaîne en paramètre et renvoie la chaîne en majuscules, puis utilisez-la dans la boucle.

3. **Test et validation :**  
   - Exécutez le script dans une console PowerShell.
   - Vérifiez que seuls les noms répondant à la condition (exemple : ceux commençant par "A") s’affichent en majuscules.

---

## Exercice 2 : Gestion des services Windows

### Objectif  
Apprendre à interagir avec les services Windows pour lister, filtrer et démarrer ceux qui sont arrêtés.

### Instructions

1. **Liste des services :**  
   - Utilisez la cmdlet `Get-Service` pour récupérer la liste de tous les services.
   - Filtrez cette liste pour obtenir uniquement ceux dont le statut est "Stopped" (arrêté).

2. **Affichage et prise de décision :**  
   - Parcourez la liste filtrée et affichez le nom du service et son statut.
   - Proposez à l’utilisateur de confirmer s’il souhaite démarrer chacun des services arrêtés.

3. **Démarrage des services :**  
   - Si l’utilisateur confirme, utilisez la cmdlet `Start-Service` pour démarrer le service.
   - Gérez les erreurs potentielles à l’aide d’un bloc `try/catch` pour afficher un message d’erreur en cas d’échec du démarrage.

4. **Test et validation :**  
   - Exécutez le script et simulez le démarrage de services (attention en environnement réel, privilégiez un environnement de test).

---

## Exercice 3 : Interrogation et gestion d’Active Directory

### Objectif  
Utiliser les cmdlets Active Directory pour récupérer et manipuler des objets utilisateurs en fonction d’un critère (ex. département ou localisation).

### Instructions

1. **Configuration initiale :**  
   - Assurez-vous d’avoir installé et importé le module Active Directory :
     ```powershell
     Import-Module ActiveDirectory
     ```
   
2. **Requête de récupération des utilisateurs :**  
   - Écrivez un script (`Exercice3.ps1`) qui interroge l’Active Directory pour récupérer les utilisateurs.
   - Utilisez la cmdlet `Get-ADUser` avec des filtres pour sélectionner les utilisateurs d’un département spécifique (par exemple "IT") ou d’une localisation donnée.

3. **Affichage des informations :**  
   - Pour chaque utilisateur trouvé, affichez des informations telles que : Nom, Email et Département.
   
4. **Test et validation :**  
   - Exécutez le script dans un environnement Active Directory de test.
   - Vérifiez que les résultats correspondent aux critères définis.

---

## Exercice 4 : Création et gestion de groupes de distribution

### Objectif  
Automatiser la création et la mise à jour des listes de diffusion à partir d’un fichier CSV.

### Instructions

1. **Création du fichier CSV :**  
   - Créez un fichier nommé `utilisateurs.csv` avec les colonnes suivantes : `Nom`, `Email`, `Localisation`. Exemple :
     ```
     Nom,Email,Localisation
     Alice,alice@example.com,Paris
     Bob,bob@example.com,Lyon
     Charlie,charlie@example.com,Paris
     David,david@example.com,Marseille
     ```

2. **Lecture et traitement du fichier :**  
   - Dans un script (`Exercice4.ps1`), utilisez la cmdlet `Import-Csv` pour lire le fichier.
   - Groupez les utilisateurs par `Localisation`.

3. **Création/Mise à jour des groupes :**  
   - Pour chaque localisation, vérifiez si un groupe de distribution existe (en utilisant `Get-ADGroup` ou la cmdlet spécifique à Exchange comme `Get-DistributionGroup` si disponible).
   - Si le groupe n’existe pas, créez-le à l’aide de `New-ADGroup` (ou `New-DistributionGroup` pour Exchange).
   - Ajoutez les utilisateurs au groupe avec `Add-ADGroupMember`.

4. **Test et validation :**  
   - Testez dans un environnement de développement pour vérifier que les groupes sont correctement créés et que les utilisateurs y sont ajoutés.

---

## Exercice 5 : Automatisation avec paramètres et gestion d’erreurs

### Objectif  
Créer un script modulable qui accepte des paramètres d’entrée et gère les erreurs de manière robuste.

### Instructions

1. **Déclaration des paramètres :**  
   - Dans un script (`Exercice5.ps1`), commencez par déclarer un bloc `param` qui accepte :
     - Le chemin du fichier CSV.
     - Le nom du groupe.
     - La localisation (optionnelle).
   - Exemple :
     ```powershell
     param (
         [Parameter(Mandatory=$true)]
         [string]$CsvPath,
         
         [Parameter(Mandatory=$true)]
         [string]$NomGroupe,
         
         [string]$Localisation
     )
     ```

2. **Traitement du fichier CSV et filtrage :**  
   - Importez le fichier CSV avec `Import-Csv`.
   - Si le paramètre `Localisation` est fourni, filtrez les utilisateurs en conséquence.

3. **Création du groupe et ajout des membres :**  
   - Implémentez la logique de création du groupe (comme dans l’Exercice 4) en utilisant les paramètres fournis.
   - Utilisez des blocs `try/catch` pour gérer les exceptions lors de la création du groupe ou de l’ajout des membres.
   
4. **Journalisation des erreurs :**  
   - En cas d’erreur, écrivez un message dans un fichier log (par exemple `log.txt`), en ajoutant la date et l’heure pour le suivi.

5. **Test et validation :**  
   - Exécutez le script en passant différents paramètres pour vérifier que la gestion des erreurs et la journalisation fonctionnent comme prévu.

---

## Exercice 6 : Test et déploiement en environnement simulé

### Objectif  
Simuler un déploiement complet enchaînant les scripts et en effectuant des tests unitaires simples.

### Instructions

1. **Création d’un script de déploiement :**  
   - Créez un script principal (`Deploy.ps1`) qui appelle les scripts précédemment réalisés dans un ordre logique :
     - Vérification de l’environnement (par exemple, s’assurer que les modules nécessaires sont chargés).
     - Création/mise à jour des groupes.
     - Ajout des membres aux groupes.

2. **Intégration de tests unitaires :**  
   - Familiarisez-vous avec **Pester**, le framework de tests pour PowerShell.
   - Écrivez quelques tests unitaires pour vérifier, par exemple, que :
     - Un groupe de distribution a bien été créé.
     - Un utilisateur a été ajouté au groupe.
   - Exemple d’un test simple avec Pester :
     ```powershell
     Describe "Test de création de groupe" {
         It "Le groupe doit exister après exécution du script" {
             # Supposons que le script a créé un groupe nommé 'TestGroupe'
             $groupe = Get-ADGroup -Filter { Name -eq "TestGroupe" }
             $groupe | Should -Not -BeNullOrEmpty
         }
     }
     ```
   - Enregistrez ces tests dans un fichier `Tests.Tests.ps1`.

3. **Exécution du déploiement :**  
   - Dans `Deploy.ps1`, enchaînez l’exécution des scripts avec des appels comme `.\Exercice5.ps1 -CsvPath "utilisateurs.csv" -NomGroupe "TestGroupe"`.
   - À la fin, exécutez les tests avec la commande `Invoke-Pester`.

4. **Test et validation :**  
   - Exécutez le script de déploiement dans un environnement de test pour vous assurer que toutes les étapes se déroulent correctement.
   - Analysez les résultats des tests unitaires pour identifier et corriger d’éventuels problèmes.

---

## Exercice 7 : Documentation automatique des scripts

### Objectif  
Apprendre à documenter vos scripts PowerShell et générer une documentation automatique à partir de commentaires structurés.

### Instructions

1. **Ajout de commentaires structurés :**  
   - Pour chaque fonction ou script, ajoutez des commentaires en utilisant le format d’aide intégré (Comment-Based Help).  
   - Exemple pour une fonction :
     ```powershell
     function Convertir-EnMajuscules {
         <#
         .SYNOPSIS
         Convertit une chaîne de caractères en majuscules.
         
         .DESCRIPTION
         Cette fonction prend une chaîne en entrée et retourne la version en majuscules de cette chaîne.
         
         .PARAMETER Texte
         La chaîne à convertir.
         
         .EXAMPLE
         Convertir-EnMajuscules -Texte "bonjour" retourne "BONJOUR"
         #>
         param (
             [string]$Texte
         )
         return $Texte.ToUpper()
     }
     ```

2. **Extraction de la documentation :**  
   - Écrivez un script (`GenererDocumentation.ps1`) qui parcourt un dossier contenant vos scripts et extrait la documentation avec la cmdlet `Get-Help`.
   - Par exemple, pour générer un fichier HTML :
     ```powershell
     $scripts = Get-ChildItem -Path "C:\Chemin\Vers\Scripts" -Filter *.ps1
     $documentation = foreach ($script in $scripts) {
         Get-Help $script.FullName -Full
     }
     $documentation | Out-File -FilePath "Documentation.txt"
     
     # Ou convertir en HTML
     $documentation | ConvertTo-Html -Title "Documentation des scripts" | Out-File "Documentation.html"
     ```

3. **Test et validation :**  
   - Exécutez le script pour vérifier que la documentation est bien générée et correctement formatée.
   - Ouvrez le fichier généré (texte ou HTML) pour vérifier la lisibilité et la complétude des informations.

---

Bonne préparation !
