---
title: TD1 &ndash; Introduction à PHP
subtitle: Hello World, objets et formulaires
layout: tutorial
---

<!--

Rajouter un exercice qui fait écrire un lien HTML en PHP avec une partie variable.
Genre:

Un formulaire où on rentre le nom et l'adresse d'un lien et qui crée le lien

OU

$liens = array("index","contact") et crée le menu

OU

Créer un lien de type GET
-->

## Méthodologie

Quelques consignes qui vous feront gagner beaucoup de temps en développement web:

1. PHP est un langage de programmation donc utilisez un environnement de
   développement. Vous ne codez pas du Java avec BlocNotes, c'est pareil pour
   PHP. Nous coderons donc notre PHP sous NetBeans à partir du TD2 (sauf si vous
   avez déjà votre éditeur préféré).

   **Exceptionnellement** pour ce TD, utilisez un éditeur de base comme
    *gedit*. Ainsi, nous n'aurons pas l'impression que la grosse machinerie
    qu'est NetBeans nous cache trop de choses.
     
2. Ne copiez **jamais** vos fichiers à plusieurs endroits.
3. Merci de **ne pas** imprimer ce TP.

## Accédez à vos pages web

### Une page HTML de base

Créez une page **index.html** avec le contenu suivant et enregistrez la dans
le répertoire **public_html** de votre espace personnel.

~~~
<!DOCTYPE html>
<html>
    <head>
        <title> Insérer le titrer ici </title>
    </head>

    <body>
        Un problème avec les accents à é è ?
        <!-- ceci est un commentaire -->
    </body>
</html>
~~~
{:.html}


1. Ouvrez cette page dans le navigateur directement en double-cliquant dessus
   directement depuis votre gestionnaire de fichiers.
   Notez l'URL du fichier :
   [file://chemin_de_mon_compte/public_html/index.html](file://chemin_de_mon_compte/public_html/index.html).
   
   **Un problème avec les accents ?**
   Dans l'en-tête du fichier HTML vous devez rajouter la ligne qui spécifie
   l'encodage
   
   ~~~
   <meta charset="utf-8" />
   ~~~
   {:.html}
   Il faut que vos fichiers soient enregistrés avec le même encodage. UTF-8 est
   souvent l'encodage par défaut, mais les éditeurs de texte offrent généralement
   le choix de l'encodage lors du premier enregistrement du fichier.

2. **Vous souvenez-vous comment fait-on pour qu'une page Web soit servie par le serveur HTTP de l'IUT (sur [infolimon.iutmontp.univ-montp2.fr](infolimon.iutmontp.univ-montp2.fr)) ?**

   Les pages Web écrites dans le dossier **public_html** de son répertoire
   personnel seront servies sur [infolimon](infolimon.iutmontp.univ-montp2.fr).
   Ouvrez donc `index.html` depuis le navigateur en tapant l'URL suivante dans
   la barre d'adresse
   [http://infolimon.iutmontp.univ-montp2.fr/~mon_login/index.html](http://infolimon.iutmontp.univ-montp2.fr/~mon_login/index.html)

   **Un problème de droit ?**
   Pour pouvoir servir vos pages, le serveur HTTP (Apache) de l'IUT doit avoir
   le droit de lecture des pages Web (permission `r--`) et le droit de
   traverser les dossiers menant à la page Web (permission `--x`). À
   l'IUT, la gestion des droits se fait par les ACL.  
   Pour donner les droits à l'utilisateur www-data (Apache), utilisez la commande
   `setfacl` dans un terminal sous Linux :

   * `setfacl -m u:www-data:--x nom_du_répertoire`  pour chaque répertoire
     menant à votre page Web.
     
   * Puis `setfacl -m u:www-data:r-- nom_de_la_page_Web`

   **Note :** Les ACL permettent d'avoir des droits spécifiques à plusieurs
   utilisateurs et à plusieurs groupes quand les droits classiques sont limités
   à un utilisateur et un groupe. Pour lire les droits ACL d'un fichier ou
   dossier, on tape `getfacl nom_du_fichier`.

3. Quelle(s) différence(s) observez-vous entre les deux `index.html` ouvert de
deux manières différentes ?

### Notre première page PHP

4. Créez une page `echo.php` avec le contenu suivant.  
   Pour ne pas que votre **public_html** devienne un dépôt de pages Web à ciel
   ouvert, créez des répertoires pour les cours et les TDs. Nous vous
   conseillons donc d'enregistrer `echo.php` dans
   `.../public_html/PHP/TD1.echo.php`.

   ~~~
   <!DOCTYPE html>
   <html>
       <head>
           <meta charset="utf-8" />
           <title> Mon premier php </title>
       </head>
   
       <body>
           Voici le résultat du script PHP : 
           <?php
             // Ceci est un commentaire PHP sur une ligne
             /* Ceci est le 2ème type de commentaire PHP
             sur plusieurs lignes */
           
             // On met la chaine de caractères "hello" dans la variable 'texte'
             // Les noms de variable commencent par $ en PHP
             $texte="hello";

             // Concatenation de 2 chaînes de caractères avec '.'
             $texte=$texte . " world";

             // On écrit le contenu de la variable 'texte' dans la page Web
             echo $texte;
           ?>
       </body>
   </html> 
   ~~~
   {:.html}


5. Ouvrez cette page dans le navigateur directement depuis votre gestionnaire de fichiers :  
   [file://chemin_de_mon_compte/public_html/PHP/TD1/echo.php](file://chemin_de_mon_compte/public_html/PHP/TD1/echo.php).

6. Ouvrez cette page dans le navigateur dans un second onglet en passant par le serveur web :   
   [http://infolimon.iutmontp.univ-montp2.fr/~mon_login/PHP/TD1/echo.php](http://infolimon.iutmontp.univ-montp2.fr/~mon_login/PHP/TD1/echo.php)

7. Quelle(s) différence(s) observez-vous dans l'affichage des deux pages Web ?

8. Quelle(s) différence(s) observez-vous dans le code source des deux pages Web ?  
   (Clic droit, code source ou `Ctrl-U`)

**Remarque :** Comme vous l'avez sans doute intuité en regardant le code source de [http://infolimon...echo.php](http://infolimon.iutmontp.univ-montp2.fr/~mon_login/PHP/TD1/echo.php), la page Web produite est composée des lignes en dehors du PHP, et de ce que le PHP écrit avec `echo`.

## La programmation objet en PHP

PHP était initialement conçu comme un langage de script, mais est passé Objet à partir de la
version 5.

### Un exemple de classe PHP

Créer un fichier **Voiture.php** :

~~~
<?php
class Voiture {

  private $marque;
  private $couleur;
  private $immatriculation;
   
  //un getter      
  public function getMarque() {
       return $this->marque;  
  }
  
  //un setter 
  public function setMarque($marque2) {
       $this->marque = $marque2;
  }
   
   //Un constructeur
  public function __construct($m = NULL, $c = NULL, $i = NULL)  { //On spécifie les valeurs par défaut
   if (!is_null($m) ) { $this->marque = $m; }
   if (!is_null($c) ) { $this->couleur = $c; }
   if (!is_null($i) ) { $this->immatriculation = $i; }   
  } 
        
  // une methode d'affichage.
  public function afficher() {
    echo "<p> Voiture {$this->immatriculation} de marque {$this->marque} </p>" ;
  }
}
?>
~~~
{:.php}


### Différences avec Java

1. Pas de typage des variables
2. Le code PHP doit être compris entre la balise ouvrante `<?php` et la balise fermante `?>`
2. Les variables sont précédées d'un `$`
3. Pour accéder à un attribut ou une fonction d'un objet, on utilise le `->` au lieu du `.`
4. Le constructeur ne porte pas le nom de la classe, mais s'appelle `__construct()`. Et on a le droit à au plus un constructeur, qu'on pourra eventuellement appeler avec un sous-ensemble de paramètres. 


### Utilisation de cette classe

Dans un fichier **testVoiture.php**, copiez le code suivant

~~~
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title> Mon premier php </title>
    </head>
   
    <body>
    <?php
      // On importe 'Voiture.php', comme import en Java
      require 'Voiture.php';   

      $voiture1 = new Voiture('Renault','Bleu','256AB34'); 
      $voiture2 = new Voiture('Peugeot','Vert','128AC30');

      $voiture1->afficher();
      $voiture2->afficher();
    ?>
    </body>
</html>
~~~
{:.php}

Testez cette page: 
[http://infolimon.iutmontp.univ-montp2.fr/~mon_login/PHP/TD1/testVoiture.php](http://infolimon.iutmontp.univ-montp2.fr/~mon_login/PHP/TD1/testVoiture.php)

À la différence de Java, il n'y a pas de besoin d'une méthode `main()`. N'importe quelle
fichier **PHP** est considéré comme un `main()`.


### Gestion des listes

1. Ajouter un attribut **options** à la classe voiture. 
2. Initialiser cette liste dans le constructeur, à l'aide de `$this->options = array();`
3. Ajouter une méthode à la classe `Voiture` qui permet d'ajouter une option à la liste,
   à l'aide de `$this->options[] = $uneOption;` qui ajoute l'option `uneOption`
   à la fin du tableau d'options `options`.
4. Modifier la méthode `afficher()` pour qu'elle permette de lister les options

   ~~~
   foreach ($this->options as $option) {
       echo $this->option; 
   }
   ~~~
   {:.php}

   **Remarque :** La boucle `foreach` est pratique pour parcourir les indices et
   valeurs d'un tableau. Il existe aussi bien sûr une boucle `for` classique

   ~~~
   for ($i = 0; $i < count($this->options); $i++) {
       echo $this->options[$i];
   }
   ~~~
   {:.php}


## Interaction avec un formulaire

1. Créez un fichier **formulaireVoiture.html**, réutilisiez l'entête du fichier
   **index.html** et dans le body, insérez le formulaire suivant:

   ~~~
   <form method="get" action="creerVoiture.php">
     <fieldset>
       <legend>Mon formulaire :</legend>
       <p>
         <label for="immat_id">Immatriculation</label> :
         <input type="text" placeholder="Ex : 256AB34" name="immatriculation" id="immat_id" required/>
       </p>
       <p>
         <label for="marque_id">Marque</label> :
         <input type="text" placeholder="Ex : Renault" name="marque" id="marque_id"  required/>
       </p>
       <p>
         <label for="couleur_id">Couleur</label> :
         <input type="text" placeholder="Ex : Bleu" name="couleur" id="couleur_id"  required/>
       </p>
       <p>
         <input type="submit" value="Envoyer" />
       </p>
     </fieldset> 
   </form>
   ~~~
   {:.html}

   **Rappel :** L'attribut important des `<input type="text">` est `name="marque"`
   qui indique que ce que vous taperez dans ce champ texte sera associé au nom de variable `marque`.  
	Quand l'attribut `for` de `<label>` contient l'identifiant d'un champ, un
    clic sur le text du label vous amène directement dans ce champ.
   

2. Cliquez sur le bouton **"Envoyer"**. Vous voyez apparaître dans votre navigateur l'URL:
   [http://infolimon.iutmontp.univ-montp2.fr/~mon_login/PHP/TD1/creerVoiture.php?immatriculation=256AB34&marque=Renault&couleur=Bleu](http://infolimon.iutmontp.univ-montp2.fr/~mon_login/PHP/TD1/creerVoiture.php?immatriculation=256AB34&marque=Renault&couleur=Bleu)

   La page **creerVoiture.php** n'existe pas, vous devez donc avoir une erreur 404.

3. Créer **creerVoiture.php** et dans le corps de cette page, vous pouvez
   récupérer la valeur du champ "marque" du formulaire à l'aide de :

   ~~~
   <?php
     $marque = $_GET["marque"];
   ?>
   ~~~
   {:.php}

4. Complétez cette page de sorte qu'elle récupère tous les champs de voiture,
   instancie la classe Voiture et appelle la méthode affiche().

5. Afin d'éviter que les paramètres du formulaire n'apparaissent dans l'URL, modifiez 
   le formulaire pour qu'il appelle la méthode post:

   ~~~
   <form method="post" action="creerVoiture.php">
   ~~~
   {:.html}

   et côté PHP, récupérez les paramètres avec

   ~~~
   <?php
     $marque = $_POST["marque"];
   ?>
   ~~~
   {:.php}

<!--
 Expliquer les requêtes HTTP POST, et qu'elles permettent d'avoir un corps de requête.
 En simuler une avec telnet ?
-->
   
## Les bases d'un site de covoiturage

Vous allez programmer les classes d'un site de covoiturage, dont voici la description d'une version
minimaliste:

* **Trajet :** Une annonce de trajet comprend un identifiant unique `id`, les
  détails d'un trajet (un point de départ `depart` et un point d’arrivée
  `arrivee`) et des détails spécifiques à l’annonce comme une date de départ
  `date`, un nombre de places disponibles `nbplaces` et un prix `prix`.
* **Utilisateur :** Un utilisateur possède des champs propres `(login, nom,
prénom)` et peut proposer une liste d'annonces de trajets.


## Chez vous

Vous pouvez installer Apache + PhP + MySql sur votre machine perso (WAMP sous
windows, LAMP sous Linux, MAMP sous MacOs)

Attention, pensez à modifier le php.ini pour mettre `display_errors = On`, pour
avoir les messages d'erreurs. Car par défaut, le serveur est configuré en mode
production (`display_errors = Off`). Il faut redémarrer Apache pour que les
modifications soient prises en compte.

<!--
Référence utile : php.net
-->
