# Cadre de test

Malheureusement, JavaScript n'inclut pas de cadre de test natif. Il
existe toutefois plusieurs bibliothèques qui permettent de faciliter
l'écriture de tests : Jest, Mocha, Vitest, etc. Pour ce cours, nous
utiliserons le cadre de test inclus avec Deno.

## Importer le cadre

D'abord, créez un fichier dans lequel vous placerez les tests. Il est
coutûme de nommer le fichier qui contient les tests selon le fichier qui
contient le code source à tester. Par exemple, si on veut tester la
fonction `square` qui se trouve dans le fichier `math.js`, le fichier de
tests se nommera `math_test.js`. Il est important de respecter cette
convention, sinon les tests ne seront pas exécutés.

Pour utiliser la cadre de test, il faut d'abord l'importer. Au début du
fichier qui contient les tests, ajoutez la ligne suivante :

```js
// math_test.js

import { expect } from "jsr:@std/expect";
```

## Utilisation

Pour créer un test, on utilise la méthode `test` de l'objet global
`Deno`. La méthode `test` prend deux arguments :

- une chaîne de caractères qui contient le nom du test ;
- une fonction de rappel.

Dans le corps de la fonction de rappel, on utilise la fonction `expect`
pour spécifier la valeur actuelle, et la méthode `toBe` pour spécifier
la valeur attendue.

```js
// math_test.js

import { expect } from "jsr:@std/expect";
import { square } from "./square.js";

Deno.test("Calcule le carré", () => {
    const actual = square(2);
    const expected = 4;
    expect(actual).toBe(expected);
});
```
