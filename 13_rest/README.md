# REST

REST (_Representational State Transfer_) est une architecture logicielle
adoptée par la majorité des serveurs web. Le terme est utilisé pour la
première fois par Roy T. Fielding (coauteur du HTTP et membre fondateur
de la fondation Apache) dans sa thèse
_[Architectural Styles and the Design of Network-based Software
Architectures](Fielding)_, laquelle décrit comment devrait se comporter
une bonne application web. Fielding y esquisse une série de contraintes
architecturales qui permettent de créer des systèmes réseaux robustes.
Examinons ces contraintes une par une. Elles nous aideront à comprendre
le fonctionnement du web.

[Fielding]: https://ics.uci.edu/~fielding/pubs/dissertation/top.htm

## Client-serveur

Un système REST doit être composé de _clients_ et de _serveurs_ :

- Un serveur est un ordinateur ayant des ressources d'intérêt.
- Un client est un ordinateur qui désire interagir avec les ressources
  stockées sur un serveur.

Lorsque vous naviguez sur le Web, votre ordinateur agit en tant que
client qui envoie des requêtes HTTP à un serveur afin d'accéder à ces
ressources.

## Sans état

Les requêtes effectuées par un client doivent être traitées par le
serveur de manière indépendante. Le serveur ne doit pas conserver pas en
mémoire les requêtes passées. L'état de la communication doit être
transmis avec chaque nouvelle requête.

Imaginez par analogie une conversation avec une personne amnésique. À
chaque intervention, ont doit lui rappeler ce qui a été dit
précédemment. C'est ce qu'on entend par « sans état ».

## Interface uniforme

Les serveurs et les clients doivent communiquer grace à un langage
commun. Ce langage doit permettre aux deux parties de modifier leur
fonctionnement interne sans briser le système. À cette fin, les quatre
sous contraintes suivantes doivent être respectées.

### Identification des ressources

Les ressources doivent être identifiées de façon unique par un
_identifiant permanent_. Une ressource peut être un document HTML, une
image, des données brutes, une feuille de style, etc. Si une ressource
change d'identifiant (ce qui devrait être évité à tout prix), alors le
serveur doit en aviser le client, et lui fournir le nouvel identifiant
de la ressource.

Le Web utilise les URI (_Uniform Ressource Identifier_) pour identifier
les ressource, et HTTP (_Hypertext Transfer Protocol_) comme langage de
communication. Pour accéder à une ressource se trouvant sur un serveur,
un client envoie une requête HTTP contenant l'URI qui identifie la
ressource désirée.

### Manipulation des ressources via leurs représentations

Les clients ne peuvent pas interagir directement avec les ressources du
serveur. La manipulation des ressources doit se faire via leurs
_représentations textuelles_. Le serveur a un contrôle total sur les
ressources en tout temps, et seulement lui peut les modifiers.

Lorsque l'utilisateur·rice d'un site Web soumet un formulaire, par
exemple, le navigateur Web ne dit pas serveur comment traiter les
données. Le navigateur envoie seulement une requête, laquelle est une
_suggestion_ adressée au serveur. Libre au serveur d'interpréter cette
requête comme bon lui semble, ou de la refuser.

### Messages autonomes

Les messages échangés entre le client et le serveur doivent contenir
toutes les informations nécessaires à la compréhension desdits messages.
Un message ne doit pas dépendre d'information se trouvant dans un autre
message ou un autre document.

Par exemple, lorsqu'un ou une utilisatrice entre l'adresse «
http://www.example.com » dans la barre de recherche de son navigateur
Web, ce dernier envoie la requête HTTP suivante :

```
GET / HTTP/1.1
Host: www.example.com
```

Ce message est autonome car qu'il contient toutes les informations
nécessaires, incluant l'action désirée (`GET`) et le langage du message
(`HTTP 1.1`), pour que le serveur puisse le comprendre. Voici comment le
serveur pourrait répondre à cette requête :

```
HTTP/1.1 200 OK
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  </body>
    <h1>Hello World!</h1>
  </body>
</html>
```

Pareillement, ce message est autonome car il indique au client comment
interpréter le contenu (`text/html`).

### Hypermédia

Un serveur doit envoyer seulement des _hypermédias_. Un hypermédia est
un type de donnée qui contient des informations quant aux autres données
qu'il est possible de consulter. Un hypermédia indique au client quelles
sont les requêtes qu'il est possible d'effectuer à partir de la
ressource actuelle.

HTML est un type d'hypermédia. Les éléments d'ancre, par exemple,
indiquent aux clients qu'ils peuvent faire une requête GET à l'adresse
spécifiée par l'attribut `href`. Pareillement, un formulaire indique aux
clients qu'il est possible de soumettre des données au serveur.

## Bibliographie

- Lauren Long.
  [What RESTful actually means](https://codewords.recurse.com/issues/five/what-restful-actually-means).
- Codecademy.
  [What is REST?](https://www.codecademy.com/article/what-is-rest).
- HTMX. [HATEOAS](https://htmx.org/essays/hateoas/).
