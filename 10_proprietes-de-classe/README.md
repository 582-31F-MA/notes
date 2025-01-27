# Propriétés de classe

Parfois, certaines propriétés n'appartiennent pas à une instance en
particulier, mais plutôt à toutes les instances d'une même classe. On
appelle ces propriétés des _propriétés de classe_, ou des _propriétés
statiques_.

Par exemple, la classe native `Node`, qui représente les nœuds du DOM, a
des propriétés statiques pour chaque type de nœud : `Node.ELEMENT_NODE`,
`Node.TEXT_NODE` et `Node.COMMENT_NODE`. Ces propriétés sont
essentiellement des constantes dont la valeur correspond au numéro
associé à ces types (1, 3 et 8).

Pareillement, la classe native `Math` a des méthodes statiques pour
effectuer diverses opérations : `Math.round`, `Math.floor`,
`Math.random`, etc.

La différence entre une propriété d'instance et une propriété de classe
est en partie sémantique. Le type d'un nœud (pour rependre l'exemple
ci-dessus) est le même pour toutes les instance de `Node`. De plus, il
peut être pratique de pouvoir accéder à cette information sans avoir à
créer une instance. Par conséquent, les propriétés de classe sont
souvent des propriétés auxiliaires qui sont liées de près ou de loin au
domaine d'application de la classe.

Considérons une classe qui représente une température dont le
constructeur prend une valeur en degrés Celsius :

```js
class Temperature {
    celsius;

    constructor(celsius) {
        this.celsius = celsius;
    }
}
```

Un bon example de propriété statique est une méthode qui permet de créer
un nouvel instance de `Temperature` à partir d'une valeur en degrés
Fahrenheit.

```js
class Temperature {
    celsius;

    constructor(celsius) {
        this.celsius = celsius;
    }

    // propriété statique
    static fromFahreinheit(value) {
        return new Temperature((value - 32) / 1.8);
    }
}
```

Comme vous pouvez voir, il suffit d'ajouter le mot-clé `static` devant
une propriété pour déclarer celle-ci comme appartenant à la classe, et
non aux instances de celle-ci.

Puisque les propriétés statiques appartiennent à la classe, elles sont
accessibles à partir de l'identifiant de la classe (et oui, même les
classes sont théoriquement des objets).

```js
const t = Temperature.fromFahreinheit(86);
```

Une propriété statique peut être publique ou privée. Par exemple, le
décalage et le facteur de conversion de la formule ci-dessus pourraient
être affectés à des propriétés statiques privée.

```js
class Temperature {
    celsius;

    // propriétés statiques privées
    static #offset = 32;
    static #scalingFactor = 1.8;

    constructor(celsius) {
        this.celsius = celsius;
    }

    static fromFahreinheit(value) {
        return new Temperature(
            (value - Temperature.#offset) / Temperature.#scalingFactor,
        );
    }
}
```

## Type énuméré

Certaines informations du monde réel prennent la forme d'ensembles fini
de valeurs possibles. Considérons par exemple un feu de circulation
routière. Les feux sont généralement déclinés en trois couleurs : le
rouge pour fermer, le vert (ou parfois le bleu) pour ouvrir, et le
jaune-orangé pour signaler le passage du feu vert au feu rouge. L'état
actuel du feu de circulation a donc seulement trois valeurs possibles :
rouge, vert, ou jaune-orangé.

Dans plusieurs langages de programmation, on représente cette
information par une donnée de _type énuméré_, aussi appelée
_énumération_ ou juste _enum_. Le but d'une énumération est d'exprimer
le fait que qu'une information contient des cas prédéterminés (nommés
_énumérateurs_), et de s'assurer que le code respecte cette contrainte.

Malheureusement, JavaScript n'inclut pas de type énuméré. On peut
documenter en JSDoc le fait qu'un argument doit correspondre à telle ou
telle valeur, mais il est difficile d'exprimer cette contrainte à même
le code.

```js
/**
 * @param {"red"|"green"|"yellow"} currentLight
 * @returns {"red"|"green"|"yellow"}
 */
function nextTrafficLight(currentLight) {
    switch (currentLight) {
        case "red":
            return "green";
        case "green":
            return "yellow";
        case "yellow":
            return "red";
    }
}
```

On peut toutefois émuler un type énuméré à l'aide des propriétés de
classe. Pour se faire, il suffit de créer un type d'objet qui représente
l'énumération, et de définir une propriété de classe pour chaque
énumérateur. La valeur des énumérateurs doit également être une instance
de la classe.

```js
class TrafficLight {
    static Red = new TrafficLight();
    static Green = new TrafficLight();
    static Yellow = new TrafficLight();

    get next() {
        switch (this) {
            case TrafficLight.Red:
                return TrafficLight.Green;
            case TrafficLight.Green:
                return TrafficLight.Yellow;
            case TrafficLight.Yellow:
                return TrafficLight.Red;
        }
    }
}
```

Cette version offre plusieurs avantages. D'abord, la relation entre les
énumérateurs est explicite du fait qu'ils sont regroupés sous une même
classe. Implémenter l'énumération à l'aide d'une classe permet aussi
d'encapsuler les énumérateurs avec leur interface. Vous remarquerez que
la fonction `nextTrafficLight` est devenu un accesseur nommé `next`,
auquel on peut accéder directement à partir des énumérateurs.

```js
console.log(TrafficLight.Red.next === TrafficLight.Green); // => true
```

Rien nous empêche d'ailleurs d'ajouter des propriétés d'instance aux
énumérateurs. Chacun pourrait avoir sa propre valeur RGB, par exemple,
ou bien sa propre forme géométrique.
