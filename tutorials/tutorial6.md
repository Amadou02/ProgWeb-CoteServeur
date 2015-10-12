---
title: TD6 &ndash; Architecture MVC avancée 2/2
subtitle: Vues modulaires, filtrage, formulaires améliorés
layout: tutorial
---

<!-- Rajouter des fonctions de sécurité/contrôle avec isset($_GET) et les
htmlspecialchars quand on écrit dans du HTML et URLencode quand on écrit dans
une URL -->

<!--
Rajouter dispatcher
-->

<!--
Ou est-ce qu'ils codent vraiment utilisateurs et trajets ?
-->

Aujourd'hui nous continuons de développer notre site-école de covoiturage. Au
fur et à mesure que le projet grandit, nous allons bénéficier du modèle MVC qui
va nous faciliter la tâche de conception. En attendant de pouvoir gérer les
sessions d'utilisateur, nous allons développer l'interface "administrateur" du
site.

La semaine dernière, nous avions un site qui propose une gestion minimale des
voitures (*Create / Read / Update / Delete*). Notre objectif est d'avoir une
interface similaire pour les utilisateurs et les trajets. Dans ce TD, nous
allons dans un premier temps rendre notre MVC de voitures plus générique. Cela
nous permettra de l'adapter plus facilement aux utilisateurs et trajets dans un
second temps.

## Dispatcher

Pour l'instant, nous n'avons travaillé que sur le contrôleur voiture. Nous
souhaitons maintenant rajouter les contrôleurs utilisateur et trajet. Pour gérer
tous les contrôleurs à partir de notre page d'accueil unique `index.php`, nous
avons besoin d'un *dispatcher*.

Le *dispatcher* (répartisseur) est une partie du contrôleur dont la fonction est
de charger le bon sous-contrôleur (par ex. voiture, utilisateur ou
trajet). Désormais, nous devons donc spécifier le contrôleur demandé dans le
*query string*. Par exemple, l'ancienne page
`index.php?action=readAll` du contrôleur voiture devra s'obtenir avec
`index.php?controller=utilisateur&action=readAll`.

<div class="exercise">

1. Définissez une variable `$controller` dans `index.php` en récupérant sa
valeur à partir de l'URL ou en mettant le contrôleur voiture par défaut.

   **Aide :** Ce bout de code est similaire à celui concernant `$action` dans
  `controlleurVoiture.php`.

2. Rajoutez un `switch` - `case` dans `index.php` pour charger le bon contrôleur
à l'aide d'un `require`.

3. Testez votre code en appelant vos anciennes pages du contrôleur voiture.

</div>

Maintenant que notre dispatcher est en place, nous pouvons créer de nouveaux
contrôleurs utilisateur et trajet.

**ici**



Déjà besoin de `controller`. Faire coder une première action Utilisateur readAll
en détaillant le copier coller du modèle, du contrôleur et de la vue.

## Vues modulaires

En l'état, certains bouts de code de nos vues se retrouvent dupliqués à de
multiples endroits. Par exemple, l'affichage de la liste des utilisateurs, qui
se trouve dans `viewListUtilisateur.php`, se retrouve en partie dans
`viewCreatedUtilisateur.php`, `viewDeletedUtilisateur.php` et
`viewUpdatedUtilisateur.php`.

Les prochaines questions vont vous aider à réorganiser le code pour éviter les
redondances en vue d'améliorer la maintenance du code et son debuggage.

### Mise en commun de l'en-tête et du pied de page

Actuellement, les scripts de vues sont chargées d'écrire l'ensemble de la page
Web, du `<!DOCTYPE HTML><html>...` jusqu'au `</body></html>`. Ceci empéchait de
mettre facilement deux vues bout à bout.

Par exemple, notre vue de création (action `created`) de voiture affichait
*"Votre voiture a bien été créée"* puis la liste des voitures. Il semblerait
donc naturel que la vue correspondante écrive le message puis appelle la vue
`viewAllVoiture.php`. Mais comme cette dernière vue écrivait la page HTML du
début à la fin, on ne pouvait rien y rajouter au milieu !

Décomposons nos pages Web en trois parties : le *header* (en-tête), le *body*
(corps) et le *footer* (pied de page). Dans le site final de l'an dernier, on
voit bien la distinction entre les 3 parties. On note aussi que le *header* et le 
*footer* sont communs à toutes nos pages.

<p style="text-align:center">
<img src="{{site.baseurl}}/assets/headerbodyfooter.png" width="95%"
style="vertical-align:top">
</p>

Au niveau du HTML, le *header* correspond à la partie :

~~~
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Liste des trajets</title>
    </head>
    <body>
        <nav>
            <ul>
                <li>...</li>
                <li>...</li>
            </ul>
        </nav>
~~~
{:.html}

le *body* à la partie :

~~~
        <div>
            <h1>Liste des trajets:</h1>
            <ol>
                <li>...</li>
                <li>...</li>
            </ol>
			</div>
~~~
{:.html}

et le *footer* à la partie :

~~~
    <p>Copyleft Romain Lebreton</p>
  </body>
</html>
~~~
{:.html}



Nous allons donc changer nos vues pour qu'elles n'écrivent plus que le corps de
la page.

<div class="exercise">

1. Créer une vue générique `TD5/view/view.php` avec le code suivant. La fonction
de `view.php` est de charger une en-tête et un pied de page communs, ainsi que
la bonne vue en fonction de la variable `$view`.

   ~~~
   <!DOCTYPE html>
   <html>
       <head>
           <meta charset="UTF-8">
           <title><?php echo $pagetitle; ?></title>
       </head>
       <body>
   <?php
   // Si $controleur='utilisateur' et $view='find',
   // alors $filepath=".../view/utilisateur/"
   //       $filename="viewFindUtilisateur.php";
   // et on charge '.../view/utilisateur/viewFindUtilisateur.php'
   $filepath = "{$ROOT}{$DS}{$controller}{$DS}view{DS}";
   $filename = ucfirst($view) . ucfirst($controller) . '.php'
   require "{$filepath}{$filename}";
   ?>
       </body>
   </html>
   ~~~
   {:.php}
   
   **Explication:** La fonction `ucfirst` (UpperCase FIRST letter) sert à mettre
   en majuscule la première lettre d'une chaîne de caractère.

2. Dans vos vues existantes, supprimer les parties du code correspondant aux
   *header* et *footer*.

3. Reprendre le contrôleur pour que, par exemple, à la place d'un `require
   './view/utilisateur/viewFindUtilisateur.php'`, on initialise la variable
   `$view` avec `'find'` et `$pagetitle` avec le titre de page `'Détails de
   l'utilisateur'`.

   <!-- 3. Redéfinir le `VIEW_PATH` en début de fichier par `define('VIEW_PATH', ROOT -->
   <!--    . DS . 'view' . DS);` -->
   <!-- Enfin, rajouter un `require VIEW_PATH . "view.php";` à la fin du fichier
   pour -->
   <!-- appeler notre vue générique. -->

4. **Testez** tout votre site.

</div> 

Nous allons bénéficier de notre changement d'organisation pour rajouter un
*header* et un *footer* (minimalistes) à toutes nos pages.

<div class="exercise"> 

1. Modifier la vue `view.php` pour ajouter en en-tête de page une barre de menu,
avec trois liens:

   * un lien vers la page d'accueil des voitures  
     `index.php?controller=voiture&action=readall`
   * un lien vers la future page d'accueil des utilisateurs  
     `index.php?controller=utilisateur&action=readall`
   * un lien vers la future page d'accueil des trajets  
   `index.php?controller=trajet&action=readall`

   <!-- Le lien vers utilisateur doit marcher après la partie sur le dispatcher
   ? -->

2. Modifier la vue `view.php` pour rajouter un footer minimaliste comme par exemple

~~~
<p style="border: 1px solid black;text-align:right;padding-right:1em;">
  Site de covoiturage de ...
</p>
~~~
{:.html}

</div> 

Notre réorganisation nous permet aussi de résoudre le problème soulevé plus tôt
à propos de la vue de création d'une voiture.

<div class="exercise">

Le script `viewListUtilisateur.php` sert à écrire la liste des
utilisateurs. Plutôt que d'avoir une copie de ce script dans
`viewCreatedUtilisateur.php`, `viewDeletedUtilisateur.php` et
`viewUpdatedUtilisateur.php`, incluez-le avec un `require`.

**Remarque :** Le fichier `viewDeletedUtilisateur.php` ne doit plus faire que
deux lignes maintenant.

</div>

### Un seul formulaire pour la création et la mise à jour

<div class="exercise"> 
Les vues `viewCreateUtilisateur.php` et `viewUpdateUtilisateur.php` sont quasiment identiques : elles affichent le même formulaire, et le ré-remplissent ou non. Nous allons donc fusionner `viewCreateUtilisateur.php` et `viewUpdateUtilisateur.php` en une unique page. 
En pratique, 

* on supprime la vue `viewCreateUtilisateur.php` et on modifie le contrôleur de sorte
que l'action `create` appelle la vue `viewUpdateUtilisateur.php`.

* **Attention :** quand on arrive sur la vue `viewUpdateUtilisateur.php` depuis l'action `create`, les variables 
`$n`, `$p`, ... ne sont pas renseignées. Penser à les initialiser à chaîne vide dans le contrôleur. 

* le champ `login` du formulaire doit être `required` si l'action est `create` ou `readonly` si l'action est `update` (on ne peut pas modifier la clé primaire d'un tuple).  
Utiliser une variable dans le contrôleur pour permettre l'adaptation de la vue à ces deux actions. 

* enfin, le champ `action` du formulaire doit  être `save` si l'action est `create` ou `updated` si l'action est `update`.
Là aussi, utiliser une variable spécifique.


<!-- Mettre à jour le contrôleur en conséquence.\\ -->
<!-- **Indice :** `<input ... placeholder='Exemple' value='$val'>` affichera
'Exemple' en grisé si `$val` est la chaîne de caractère vide, et pré-remplira
avec la valeur de `$val` autrement. -->

</div> 

## CRUD pour les trajets

L'implémentation du CRUD pour les trajets est un code très similaire à celui pour les utilisateurs. Nous pourrions donc copier/coller le code des utilisateurs et changer les (nombreux) endroits nécessaires. 

Pour éviter de perdre un temps conséquent à développer le CRUD pour chaque nouvel objet, nous allons le créer automatiquement autant que possible.

### Création d'un modèle générique

Nous allons déplacer de `ModelUtilisateur.php` vers le modèle générique `Model.php` toutes les fonctions qui ne sont pas spécifiques aux utilisateurs.

<div class="exercise">
Commençons par la fonction `selectAll()` de `ModelUtilisateur.php`. Dans cette fonction, seul le nom de la table présent dans la requête SQL varie. 

1. Déplacez la fonction `selectAll()` de `ModelUtilisateur.php` vers `Model.php`.
1. Créez dans `ModelUtilisateur.php` une variable `$table` qui est `protected` (accessible uniquement dans la classe courante et ses classes filles) et `static` (qui ne dépend que de la classe, pas des objets).
1. Utilisez cette variable dans la fonction `selectAll()` de `Model.php` pour faire la requête sur la bonne table.
Pour cela, accéder à la variable `$table` avec `static::$table` dans `Model.php`.

**Plus d'explications:** La syntaxe `static::$table` est quelque peu subtile. Dans notre cas, elle permet que lorsque l'on appelle `ModelUtilisateur::selectAll()`, qui est héritée de `Model::selectAll()`, la variable `static::$table` aille chercher `ModelUtilisateur::$table` et non pas `Model::$table`.
1. Testez que votre site marche toujours.

</div>

<div class="exercise">
Passons à la fonction `select()`. Dans cette fonction, le nom de la table et la condition `WHERE` varie. 

1. Déplacez la fonction `select()` de `ModelUtilisateur.php` vers `Model.php`.
1. Utilisez la variable statique `$table` de `ModelUtilisateur.php` pour remplacer le nom de la table.
1. Créez une variable statique `$primary` dans `ModelUtilisateur.php` qui contiendra le nom du champ de la clé primaire.
Utilisez cette variable pour remplacer le nom de la clé primaire dans `select()`.

</div>

<div class="exercise">
Répétez la question précédente avec la fonction `delete()`.
</div>

<div class="exercise">
Passons à la fonction `update()`. Pour reconstituer la requête 

~~~
UPDATE utilisateur SET nom=:nom,prenom=:prenom,email=:email,login=:login WHERE login=:login
~~~
{:.mysql}

 il est nécessaire de pouvoir lister les champs de la table 'utilisateur'. 
Ces champs sont les entrées du tableau `$data` et c'est ainsi que nous allons les récupérer.

1. Déplacez la fonction `update()` de `ModelUtilisateur.php` vers `Model.php`.
1. Remplacer la table et le nom de la clé primaire par les variables adéquates.
1. Nous allons générer la partie `SET` à partir des clés du tableau associatif `$data`. Autrement dit, si `$data['un_champ']` existe, nous voulons rajouter la condition ``un_champ = :un_champ'` à `SET`.

**Indice:** Utilisez la boucle `foreach ($tableau as $cle => $valeur)` pour
  récupérer les clés du tableau. Googler aussi la fonction `rtrim` de PHP qui
  pourra vous être utile pour enlever la virgule de trop à la fin de votre
  requête.

</div>

<div class="exercise">
Répétez la question précédente avec la fonction `insert()`.
</div>

### Adaptation du contrôleur

Pour mémoire, dans la variante du MVC que nous avons choisi d'implémenter, il y a un contrôleur par classe. 
Dans cette partie, nous allons nous donc créer un contrôleur pour la classe 'Trajets'. 

**Attention :** NE PAS copier/coller bêtement `ControleurUtilisateur.php` en `ControleurTrajet.php`. 
Adaptez chacune des actions de `ControleurTrajet.php` et les tester une à une. 
<!-- Couper l'adaptation du contrôleur en petit bouts testables. -->

<div class="exercise"> 
Créer une vue `viewListTrajets`, et gérer l'action `readAll` de `ControleurTrajet.php`.
</div>

<div class="exercise">
Faire de même pour les autres actions. Nous vous conseillons de faire dans l'ordre les actions `read`, `delete`, `create`, `update`, `save` et `updated`.
</div>

<!-- Il faut aussi adapter les vues au fur et à mesure. Finalement, il faut
faire quelques remplacements dans VIEW_PATH, ModelUtilisateur et les vues (comme
viewErrorUtilisateur) pour simplifier la tâche.  -->
<!-- Modifier le header pour afficher un menu vers les pages d'accueils pour les
utilisateurs et les trajets. -->

<!--

<div class="exercise">
Factoriser le modèle dans `Model.php`. Reste le nom de la table que l'on peut avoir dans un premier temps comme variable. Dans un deuxième temps, le récupérer automatiquement à base d'introspection.
</div>

<div class="exercise">
Factoriser le contrôleur en utilisant l'introspection ** ?? get_name
</div>

<div class="exercise">
Adapter les vues
</div>

## Association d'utilisateur et de trajets

<div class="exercise">
Créer table 'passager' ...
</div>

<div class="exercise">
Affiche liste des passagers pour un trajet
</div>

<div class="exercise">
Affiche liste des trajets pour un passager
</div>

-->
