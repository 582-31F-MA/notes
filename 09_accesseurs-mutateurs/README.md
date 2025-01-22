## Accesseurs et mutateurs

Il est parfois utile de créer une propriété qui renvoie une valeur
calculée dynamiquement, ou de ne pas avoir recours à l'appel explicite
d'une méthode pour renvoyer la valeur d'une propriété privée. Pour ce
faire, on utilise un _accesseur_, lequel crée une sorte de propriété
synthétique.

Considérons un objet de type `Rectangle` ayant une longueur et une
largeur. Un accesseur pourrait être utilisé pour calculer
automatiquement la valeur de son aire, et s'assurer que celle-ci est
toujours à jour.

```js
class Rectangle {
    width;
    heigth;

    constructor(width, heigth) {
        this.width = width;
        this.heigth = heigth;
    }

    get area() {
        return this.width * this.heigth;
    }
}

const r = new Rectangle(10, 10);
console.log(r); // => Rectangle { width: 10, height: 10 }
console.log(r.area); // => 100
r.width = 1;
console.log(r.area); // => 10
```

Vous remarquerez que l'accesseur `area` n'est pas appelé comme une
méthode. Sa valeur est celle retournée par la fonction qui suit le
mot-clé `get`.

On peut aussi définir un _mutateur_ afin d'exécuter une fonction à
chaque fois qu'on souhaite modifier la valeur d'une propriété. Comme
pour les accesseurs, cela permet de créer une sorte de propriété
synthétique.

Prenons par exemple une classe `Temperature` qui stocke une température
en degrées Celsius, mais dont l'interface permet aussi de lire et de
définir la température en degrées Fahrenheit.

```ts
class Temperature {
    celsius;

    constructor(celsius) {
        this.celsius = celsius;
    }

    get fahrenheit() {
        return this.celsius * 1.8 + 32;
    }

    set fahrenheit(value) {
        this.celsius = (value - 32) / 1.8;
    }
}

const temp = new Temperature(22);
console.log(temp.fahrenheit); // => 71.6
temp.fahrenheit = 86;
console.log(temp.celsius); // => 30
```

Un mutateur prend toujours comme seul argument la nouvelle valeur qu'on
veut affecter à la propriété, et ne retourne rien.
