# Classes

Une classe permet de définir un nouveau type d'objet. Le mot-clé `class`
introduit une déclaration de classe. Par convention, les noms de classe
sont écrits en casse pascal (la première lettre de chaque mot est une
majuscule).

```js
/** Un ou une utilisatrice du site web. */
class User {
    // corps de la classe
}
```

Le code ci-dessus, par exemple, crée un nouveau type d'objet nommé
`User`. Vous remarquerez qu'on utilise un _docstring_ pour indiquer ce
que la classe représente.

> [!NOTE]
> Les exemples suivants omettent parefois la documentation par soucis de
> concision, mais celle-ci est toujours requise lorsque vous soumettez
> du code.

## Propriétés

Entre les accolades se trouve le corps de la classe. On y définit les
propriétés des objets qui seront produits à partir de la classe.

```js
class User {
    /** @type {string} */
    name; // propriété
}
```

La propriété ci-dessus n'est pas initialisée. À moins de lui affecter
une valeur dans une méthode, elle vaudra `undefined`. On peut toutefois
donner une valeur par défaut à une propriété au même moment de définir
son nom. Il suffit d'utiliser l'opérateur d'affectation `=`.

```js
class User {
    name;
    access = 1; // valeur par défaut
}
```

Selon la définition ci-dessus, les objets `User` ont donc deux
propriétés : `name` et `access`.

## Méthodes

Quoique les méthodes sont théoriquement des propriétés, la syntaxe pour
déclarer celles-ci est différente. On utilise la même syntaxe que pour
déclarer un fonction, à la différence qu'on omet le mot-clé `function`.

```js
class User {
    // ...

    greet() {
        return "Hi!";
    }
}
```

## this

Une classe est un gabarit pour produire des objets d'un type
particulier. Au moment de déclarer la classe, ces objets n'existent pas
encore. Pour se référer à ses futurs objets et accéder à leurs
propriétés au sein d'une classe, on utilise l'identifiant `this`.

```js
class User {
    name;
    access = 1;

    greet() {
        return `Hi, my name is ${this.name}!`;
    }
}
```

Ci-dessus, l'identifiant `this` ne fait pas référence à un ou une
utilisatrice en particulier, mais à tous ceux et celles qui seront
éventuellement créé·e à partir de la classe `User`.

## Constructeur

Dans l'exemple précédent, la valeur de `name` est indéfinie. Puisque
chaque utilisateur·rice aura son propre nom, celui-ci sera donné au
moment de créer l'objet.

Pour passer des arguments à une classe, on utilise une fonction spéciale
nommée `constructor`. Celle-ci sera automatiquement appelée pour créer
un nouvel objet à partir de la classe.

```js
class User {
    name;
    access;

    /**
     * @param {string} name
     * @param {number} access
     */
    constructor(name, access = 1) {
        this.name = name;
        this.access = access;
    }
}
```

Puisqu'un constructeur retourne toujours un objet du type défini par la
classe, il n'est pas nécessaire d'annoter le type de sa valeur de
retour. L'instruction `return` est aussi omise.

Par convention, un constructeur est utilisé seulement pour initialiser
les propriétés d'un objet. La plupart du temps, on évitera d'inclure
d'autres instructions dans celui-ci.

## Instancier un nouvel objet

Créer un nouvel objet à partir d'une classe est appelé _instancier_ un
objet. L'objet est une _instance_ de la classe à partir de laquelle il a
été créé.

Pour instancier un objet, on utilise le mot-clé `new` suivi du nom de la
classe. Celle-ci est appliqué sur des arguments comme une fonction. Les
arguments correspondent aux paramètres du constructeur de la classe.

```js
const frieren = new User("Frieren");
console.log(frieren); // User { name: "Frieren", access: 1 }
```

Bien sûr, plus d'un objet peut être instancié à partir de la même
classe. Chacun aura son propre état (c'est-à-dire ses propres valeurs
pour les propriétés définies par la classe).

## Note sur le DOM

On se rappelle que les objets sont indépendants dans le sens qu'ils ne
doivent pas dépendre de l'implémentation d'un autre type d'objet.
Pareillement, l'implémentation d'un objet ne devrait pas dépendre de la
structure du DOM. Si un objet a besoin d'un nœud pour fonctionner, alors
une référence au nœud sera donnée comme argument à la méthode qui en a
besoin. Une classe ne devrait jamais faire référence directement au
`document`, et un changement dans la structure d'une page web ne devrait
en aucun cas compromettre l'implémentation d'un objet.
