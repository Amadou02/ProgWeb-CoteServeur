---
title : Cours 1 <br> Introduction au Web dynamique
subtitle : Le rôle du PHP
layout : slideshow
---

## Le fonctionnement du World Wide Web

### Client / Serveur

* Le *client* : C'est le visiteur d'un site Web. Il demande la page Web au
  serveur. En pratique, vous êtes des clients quand vous surfez sur le Web.  
  Plus précisement c'est votre *navigateur Web* (Firefox, Chrome, Safari, IE,
  Edge, ...) qui est le client car c'est lui qui demande la page Web.

* Le *serveur* : Ce sont les ordinateurs qui délivrent les site Web aux
  internautes, c'est-à-dire aux clients.

<p style="text-align:center">
![Mécanisme client/serveur]({{site.baseurl}}/assets/ClientServeur.png)
<br>
Le client fait une *requête* que serveur, qui répond en donnant la page Web
</p>

### Comment communiquent le client et le serveur ?

**HTTP** (*HyperText Transfer Protocol*) est un protocole de communication
entre un client et un serveur développé pour le Web. L'une de ses fonctions
principales est ainsi de récupérer des pages Web.

**À quoi ressemble une requête HTTP ?**

La requête HTTP la plus courante est la requête GET. Par exemple pour demander
la page Web
[http://infolimon.iutmontp.univ-montp2.fr/~rletud/index.html](http://infolimon.iutmontp.univ-montp2.fr/~rletud/index.html)
:

```http
GET /~rletud/index.html HTTP/1.1
host: infolimon.iutmontp.univ-montp2.fr

```

 La réponse est alors:

```http
HTTP/1.1 200 OK
Date: Tue, 08 Sep 2015 13:32:19 GMT
Server: Apache/2.2.14 (Ubuntu)
Last-Modified: Tue, 08 Sep 2015 13:06:07 GMT
Accept-Ranges: bytes
Content-Length: 5781
Content-Type: text/html

<html><head>... (contenu de index.html)
```

<!-- Parler de réponse et découpage en en-tête et corps de la réponse -->

### Le navigateur comme client HTTP

Quand on ouvre une URL en `http://`, le navigateur va agir comme un client
HTTP. Il va donc envoyer une *requête HTTP*.
<!-- à l'hôte indiqué dans l'URL. -->
Le serveur HTTP renvoie une réponse HTTP qui contient la page Web
demandée. Le navigateur interprête alors la page Web et l'affiche.

<p style="text-align:center">
![Requête HTTP]({{site.baseurl}}/assets/RequeteHTTP.png)
</p>


### Écoutons le réseau

<!-- Ouvrir http://infolimon.iutmontp.univ-montp2.fr/~rletud/index.html dans le
navigateur en expliquant la requête et réponse -->

Ouvrons
[http://infolimon.iutmontp.univ-montp2.fr/~rletud/index.html](http://infolimon.iutmontp.univ-montp2.fr/~rletud/index.html)
en écoutant le réseau à l'aide des outils de développement (`F12` ou Menu Outils/Outils de développement puis onglet Réseau).

<!--
Ouvrir Réseau, recharger la page,
view source des Request Headers puis
view source des Response Headers puis
Response
-->

Nous verrons l'autre type courant (POST) de requêtes HTTP lors de l'envoi de
formulaires en méthode POST. 

### Qu'est-ce qu'un serveur HTTP ? 

Un *serveur* **HTTP** est un logiciel qui répond à des requêtes HTTP. Il est souvent
associé au port 80 de la machine hôte.

**Quelques exemples de serveurs HTTP ?**

* Apache HTTP Server : classique, celui que l'on utilisera
* Apache TomCat : évolution pour Java (J2EE)
* IIS : Microsoft
* Node.js : codé en JavaScript.

**En pratique** lors des TDs, nous utiliserons le serveur **HTTP** de l'IUT
  (`infolimon`) et nous vous ferons installer des serveurs HTTP sur vos
  ordinateurs portables.

<!-- La pratique du serveur avec public_html, PB file://, installation chez eux -->

## Pages Web statiques ou dynamiques

* Les sites *statiques* :  
  sites réalisés uniquement à l'aide de HTML/CSS. Ils
  fonctionnent très bien mais leur contenu ne change pas.  
  Les sites statiques sont donc bien adaptés pour réaliser des sites « vitrine»
  (e.g. projet HTML/CSS 1ère année).

<p style="text-align:center">
![Requête HTTP]({{site.baseurl}}/assets/RequeteHTTP.png)
</p>

  <!-- il faut que le propriétaire du site (le webmaster) modifie le code source pour y ajouter des nouveautés. -->

* Les sites *dynamiques* :  
  ils utilisent d'autres langages tels que PHP pour générer du HTML et CSS. La
  plupart des sites web que vous visitez sont dynamiques.


<!-- Bonjour *untel*, vos informations, votre mot de passe ... -->

**Fonctionnalités typiques de sites dynamiques :**  
un espace membres, un forum, un compteur de visiteurs, des actualités, une
newsletter

### Mécanisme de génération des pages dynamiques

<!-- Voir l'un puis l'autre -->

* Site statique :  
  1. le client demande au serveur à voir une page web (requête HTTP) ;
  2. le serveur lui répond en lui envoyant la page réclamée (réponse HTTP).
  
  <!-- déjà vu -->
  
<p style="text-align:center">
![Requête HTTP]({{site.baseurl}}/assets/RequeteHTTP.png)
</p>

* Site dynamique :  
  1. le client demande au serveur à voir une page web (requête HTTP) ;
  2. le serveur crée la page spécialement pour le client (en suivant les instructions du PHP) ;
  3. le serveur lui répond en lui envoyant la page qu'il vient de générer (réponse HTTP).

<p style="text-align:center">
![Génération PHP]({{site.baseurl}}/assets/RolePHP.png)
</p>

### Où intervient le PHP ?

Un module PHP (mod_php5) est intégré au serveur HTTP Apache.  Quand le serveur
Web reçoit une requête d'un fichier .php, il génère dynamiquement la page Web en
exécutant le code PHP de la page. La page généré est ensuite renvoyée dans la
réponse HTTP.  
C'est ce que l'on appelle une page dynamique.

<!--
 La création du document advient au moment de la requête
 Le serveur peut
 o Compiler le document à la volée (comme dans la génération statique),
 o Interagir avec d’autres serveurs (authentification, API, …),
 o Interroger des bases de données.
-->

 <p style="text-align:center">
 ![Rôle du PHP]({{site.baseurl}}/assets/RolePHP.png)
 </p>



### Le langage de création de page Web : PHP

Le rôle de PHP est justement de générer du code HTML.  
précédents. C'est un langage que seuls les serveurs comprennent et qui permet de
rendre votre site dynamique.

<p style="text-align:center">
<img src="{{site.baseurl}}/assets/ElePHPant.svg" style="width:200px" alt="Mascotte PHP">  
L'éléPHPant, la mascotte de PHP
</p>

**Attention :** Les clients sont incapables de comprendre le code PHP : ils ne
connaissent que le HTML et le CSS.

<!--
Attention notamment si vous ouvrez demandez une page PHP avec le protocole
file:// (e.g. si vous double-cliquez sur un fichier PHP sur les ordinateurs de
l'IUT), le fichier PHP est envoyé directement au navigateur qui ne sait pas le
lire
-->

### Les concurrents de PHP

* ASP .NET : conçu par Microsoft, il exploite le framework .NET bien connu des développeurs C#.
* Ruby on Rails : ce framework s'utilise avec le langage Ruby.
* Django : il est similaire à Ruby on Rails, mais il s'utilise en langage
  Python.
* Java et les JSP (Java Server Pages) : plus couramment appelé « JEE », il est
  particulièrement utilisé dans le monde professionnel.

**Lequel est le meilleur ?**
Tout dépend de vos connaissances en programmation.

PHP se démarque de ses concurrents par une importante communauté qui peut vous
aider. C'est un langage facile à utiliser, idéal pour les débutants comme pour
les professionnels (Wikipédia, Yahoo et Facebook).

<p style="text-align:center">
<img src="{{site.baseurl}}/assets/ServerSideProgLang.png" alt="Popularité des langages côté serveur">  
Popularité des langages côté serveur
</p>


## Un premier aperçu de PHP

### PHP comme langage de génération de pages Web

**PHP sert à créer des documents HTML :**

* Il prend donc en entrée un fichier .php qui contient de l'HTML et du PHP
* Il ressort un document HTML pur.
* Pour cela, il exécute les instructions PHP qui lui indique comment générer le
document en sortie.

**Remarque :** PHP peut en générer tout type de document, pas nécessairement du
  HTML.


### Votre premier fichier PHP

Document PHP en entrée :

```php
<?php
  echo "Hello World!";
?>
```

PHP s'exécute sur ce document et produit

```text
Hello World
```

**Explications :**

* Les balises ouvrantes `<?php` et fermantes `?>` doivent le code PHP
* L'instruction `echo` a pour effet d'insérer du texte dans le document en sortie

<!-- Démo avec php_cli ou LAMP -->

### Imbrication de PHP dans le HTML 1/2

```php
<!DOCTYPE html>
<html>
    <head>
        <title> Mon premier php </title>
    </head>
    <body>
      <?php echo "Bonjour" ?>
    </body>
</html>
```

produira

```html
<!DOCTYPE html>
<html>
    <head>
        <title> Mon premier php </title>
    </head>
    <body>
      Bonjour
    </body>
</html>
```

### Imbrication de PHP dans le HTML 2/2

En fait, les deux fichiers suivants sont équivalents.  
En effet, ce qui est en dehors des balises PHP est écrit tel quel dans la page
Web générée.


```php
<!DOCTYPE html>
<html>
    <head>
        <title> Mon premier php </title>
    </head>
    <body>
      <?php echo "Bonjour" ?>
    </body>
</html>
```

```php
<?php
  echo "<!DOCTYPE html>";
  echo "<html>
      <head>
          <title> Mon premier php </title>
      </head>
      <body>";
  echo "Bonjour";
  echo "</body></html>";
?>
```

### Test de la page sur un serveur HTTP

Enregistrons ce fichier PHP sur le serveur HTTP `infolimon` de l'IUT. Les
fichiers PHP se mettent dans le dossier `public_html` de votre répertoire
personnel.

Vous pouvez alors y accéder à partir de l'URL
[http://infolimon.iutmontp.univ-montp2.fr/~loginIUT/](http://infolimon.iutmontp.univ-montp2.fr/~loginIUT/)
en remplaçant `loginIUT` par votre login.

```php
<!DOCTYPE html>
<html>
    <head>
        <title> Mon premier php </title>
    </head>
    <body>
      <?php echo "Bonjour" ?>
    </body>
</html>
```

## Transmettre des données entre pages Web

### Comment faire ?

Les pages Web se transmettent des données entre elles. Par exemple, votre
nom/prénom, le fait que vous soyez connectés, vos réponses au formulaire
d'inscription.

Sinon on aurait pas de pages personnalisés, on serait ramenés aux sites
statiques.

<p style="text-align:center">
**Mais comment çà marche ?**
</p>

<!-- ICI HERE : expliquer d'abord les query string ? -->

### Les *query strings* dans l'URL

### Récupérer des données GET en PHP

### Les formulaires GET

<div style="display:flex;">
Comprenons comment marche le formulaire suivant :&nbsp;

<form method="get" action="traitement.php" style="display:inline">
<input type="text" name="nom_var">
<input type="submit">
</form>
</div>

```html
<form method="get" action="traitement.php">
    <input type="text" name="nom_var" />
	<input type="submit" />
</form>
```


Un clic sur le bouton de soumission `<input type="submit" />` du formulaire a
pour effet de charger la page Web `traitement.php` avec des arguments.  Plus
précisement, si l'utilisateur a tapé `valeur` dans le champ texte
`<input type="text" name="nom_var" />`,
le navigateur va demander la page Web `traitement.php?nom_var=valeur`.

La partie `nom_var=valeur` de l'URL s'appelle la *query string*. PHP comprend la
*query string* et s'en sert pour remplir le tableau `$_GET`. Dans notre
exemple, PHP se charge de faire l'affectation
`$_GET["nom_var"] = "valeur"`
juste avant d'exécuter la page PHP `traitement.php`.

**Pourquoi la méthode s'appelle "GET" ?**  
Parce qu'elle correspond à une requête HTTP de type GET (la plus courante pour
demander une page Web.

Lors du clic sur le bouton de soumission du formulaire, le navigateur (qui est un client HTTP) va envoyer la requête HTTP suivante

```http
GET /~rletud/traitement.php?nom_var=valeur HTTP/1.1
host: infolimon.iutmontp.univ-montp2.fr

```



Vous pouvez utiliser la commande `telnet infolimon.iutmontp.univ-montp2.fr 80` dans le terminal pour répéter vous-même l'expérience.

### Exemple avec un fichier PHP


### Les formulaires POST

Considérons le même formulaire mais en `method="post"` :

```html
<form method="post" action="traitementPost.php">
    <input type="text" name="nom_var" />
	<input type="submit" />
</form>
```

La fonctionnement va être similaire à trois différences près :

1. la page chargée va être `traitementPost.php` sans query string ;
2. les données du formulaire sont envoyées avec la requête HTTP ;
3. On récupère les données dans le tableau PHP `$_POST`. Dans notre exemple, PHP
   fait l'affectation `$_POST["nom_var"] = "valeur"` juste avant d'exécuter la
   page PHP `traitementPost.php`.


Plus précisement, le navigateur va faire la requête HTTP suivante

```http
POST /~rletud/traitementPost.php HTTP/1.1
host: localhost
Content-Length:14
Content-Type:application/x-www-form-urlencoded

nom_var=valeur

```

Nous voyons ici le deuxième type de requête HTTP le plus courant : les requêtes
POST. Elles servent aussi à demander des pages Web. La principale différence est
que l'on peut envoyer, en plus de l'en-tête de la requête HTTP, un corps de
requête HTTP contenant des informations. L'en-tête et le corps de la requête
sont séparés par une ligne vide.

Vous pouvez le tester comme précédement avec la commande `telnet`.

### Récupérer des données POST avec PHP

### Avantages et inconvénients des 2 méthodes

* La méthode GET se prête bien à un site en développement car on peut facilement
contrôler les valeurs et noms de variables du formulaire.  
  Il est facile de créer un lien `<a>` vers une page traitant un formulaire en
  méthode GET et d'y envoyer des données via le *query string.*

* La méthode POST est plus propre car les valeurs ne sont plus affichées dans la barre d'adresse du navigateur. Attention, ces informations ne sont pas vraiment cachées pour autant.

<!--
Note sur ù met-on le dollar 

$objet->attribut
$tableau[$index]
Classe::$attribut_static

-->


<!--
%%%


PHP 7
Versions de PHP et apports

1er exemple echo et mélange
Explication processus création page


Dans une note séparées sur la page des TDs ?

Installation Sous Windows WAMP : https://openclassrooms.com/courses/concevez-votre-site-web-avec-php-et-mysql/preparer-son-ordinateur-2#r-911305

Installation sous Mac OS X : MAMP : https://openclassrooms.com/courses/concevez-votre-site-web-avec-php-et-mysql/preparer-son-ordinateur-2#r-911335

Installation sous Linux :
XAMP - https://openclassrooms.com/courses/concevez-votre-site-web-avec-php-et-mysql/preparer-son-ordinateur-2#r-911385
MariaDb (Open Source fork of MySQL)
ou
LAMP https://doc.ubuntu-fr.org/lamp
+ phpmyadmin
+ mod_user


Serveur Web, diag client / serveur / php, file:/// http://

DevTools Network pour voir les requetes dans le navigateur

dire aussi que cliquer sur un lien, c'est juste demander une page Web donc faire une requete HTTP

echo ...

Isoler que PHP écrit la page Web
Différentes façons d'écrire : zone PHP, echo

Reprendre le fil rouge de site de covoiturage

Parler des spécificités du PHP (vs Java) : typage, lambda, portée variables ???

require

tableaux

echo

if - for imbriqués html

-->

## Émuler un client HTTP textuel

**Expérience amusante :**  
Même si le client HTTP le plus connu est votre navigateur, il est facile de
simuler un client HTTP autrement. La commande `telnet` permet d'envoyer du texte à
une machine distance. En envoyant le texte d'une requête HTTP à un serveur HTTP,
celui nous envoie sa réponse HTTP normalement.

**Exemple :**  

```http
> telnet infolimon.iutmontp.univ-montp2.fr 80
GET /~rletud/ HTTP/1.1
host: infolimon.iutmontp.univ-montp2.fr

```

nous répond

```http
HTTP/1.1 200 OK
Date: Tue, 08 Sep 2015 20:24:04 GMT
Server: Apache/2.2.14 (Ubuntu)
Last-Modified: Tue, 08 Sep 2015 20:05:38 GMT
Accept-Ranges: bytes
Content-Length: 225
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title> Insérer le titrer ici </title>
  </head>

  <body>
    Un problème avec les accents à é è ?
    <!-- ceci est un commentaire -->
  </body>
</html>
```

Faites de même avec la requête POST précédente.

## Sources

**Source :**

* https://openclassrooms.com/courses/concevez-votre-site-web-avec-php-et-mysql/introduction-a-php
