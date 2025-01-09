# Programmation orientée objet

Un ou une programmeuse travaille avec deux types d'éléments : d'une
part, des _données_, et de l'autre, les _procédures_ qui décrivent les
règles pour manipuler celles-ci. Jusqu'à présent, vous avez défini ces
deux éléments de manière relativement indépendante. La programmation
orientée objet est un paradigme (ou une philosophie) qui propose de les
jumeler dans une entité appelée « objet ». Organiser un programme en
terme d'objets distincts permet de penser plus clairement sa structure
et d'imposer une certaine discipline.

Les sections ci-dessous décrivent l'aspect théorique de l'approche
orientée objet, laquelle repose sur les concepts d'objet, de message et
d'interface. On abordera plus tard comment mettre en pratique cette
approche à l'aide des classes.

## Objet

Théoriquement, un _objet_ est un conteneur qui permet de grouper les
variables décrivant l'état d'une chose et les fonctions qui actent sur
cet état. Cette pratique de regrouper les données et les opérations qui
permettent manipuler ces données est appelée « encapsulation ». Un objet
peut représenter quelque chose d'abstrait ou de concret. L'important est
que cette chose ait un état, et une ou plusieurs façons de modifier cet
état.

Par exemple, un objet qui représente un nombre contient la valeur
actuelle du nombre (son état) ainsi que les opérations arithmétiques
(les fonctions) qu'il est possible d'effectuer avec celui-ci. Un objet
représentant une personne contient son nom, son âge, son statut légal
(état), ainsi que les moyens de récupérer et de mettre à jour ces
informations (fonctions). Un objet représentant un terrain contient ses
dimensions (état) ainsi que les opérations qui permettent d'obtenir son
aire et sa valeur marchande (fonctions).

Concrètement, un objet (le concept) est représenté en JavaScript par une
valeur dont le type est un objet. Dans certains langages, le concept
d'objet est représenté par d'autres types de valeur : enregistrement,
structure, tableau associatif, etc. Techniquement, n'importe quel type
de valeur composé qui permet de regrouper des données et des procédures
peut être utilisé pour représenter un objet.

## Message

Un _message_ est une requête faite à un objet pour qu'il exécute une de
ses opérations. Autrement dit, pour envoyer un message à un objet, on
appele une des ses fonctions. Un message spécifie _quelle_ opération
doit être exécutée, mais pas _comment_ cette opération doit être
exécutée. Cette distinction est importante pour maintenir l'indépendance
des objets. Seul le _récepteur_ (c'est-à-dire l'objet qui reçoit le
message) sait comment l'opération fonctionne réellement. Le bon
fonctionnement d'un objet ne doit pas dépendre de l'implémentation d'un
autre objet.

Considérons un objet `Order` représentant une commande pour laquelle un
objet `Client` veut connaître les frais d'expéditions. L'objet `Client`
envoie à l'objet `Order` un message qui spécifie l'opération désirée
(connaître les frais d'expédition) ainsi que les informations
nécessaires (adresse de livraison) pour effectuer celle-ci. L'objet
`Order` retourne le résultat de l'opération à l'objet `Client`. À aucun
moment ce dernier sait comment les frais d'expédition ont été calculés.

## Interface

L'ensemble des messages auxquels un objet peut répondre est appelé son
_interface_. La seule manière d'interagir avec un objet est à travers
son interface. Cette caractéristique assure que l'implémentation d'un
objet donné ne dépend pas de l'implémentation d'un autre objet, et nous
permet d'utiliser un objet sans comprendre son fonctionnement interne.
Puisque l'implémentation d'un objet est privée, et que seule son
interface est publique, on peut changer l'implémentation sans affecter
le reste du programme.

Un ordinateur portable, par exemple, peut-être utilisé sans comprendre
le fonctionnement du processeur ou de la mémoire vive. Il suffit de
savoir utiliser son interface, c'est-à-dire comment taper sur le
clavier, cliquer sur la souris, etc. De plus, on peut remplacer les
composantes internes de l'ordinateur sans changer la façon dont un
utilisateur ou une utilisatrice interagit avec celui-ci. Dans ce
contexte, l'ordinateur est un objet et l'utilisateur·rice est un autre
objet.
