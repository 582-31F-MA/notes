# JSDoc

JSDoc est un langage de balisage qui permet de documenter un programme
JavaScript à même le code source de celui-ci.

## Docstring

Un _docstring_ est un commentaire spécial utilisé pour documenter une
interface de programmation avec JSDoc. Un _docstring_ commence avec les
caractères `/**` et se termine avec `*/`. Par convention, chaque ligne
d'un _docstring_ débute par un `*`.

```js
/**
 * Voici un docstring.
 */
```

Un _docstring_ est placé avant l'élément qu'il documente. La plupart du
temps, cet élément est une fonction, mais on peut aussi utiliser JSDoc
pour documenter un module, une classe, une variable, etc.

```js
/**
 * Calcule le carré de `n`.
 */
function square(n) {
    return n * n;
}
```

Au minimum, un _docstring_ inclut une phrase qui décrit brièvement
l'élément qui suit.

> [!NOTE]
> Un _docstring_ sert à documenter l'_interface_ d'une fonction, et non
> son _implémentation_. Utilisez la syntaxe régulière (`// ...`) pour
> les commentaire qui expliquent le fonctionnement du code.

## Balises

À l'intérieur d'un _docstring_, on utilise des balises pour donner des
informations à propos de l'élément à documenter. Une balise est un
mot-clé préfixé d'un `@`. Vous trouverez dessous une liste des balises
que nous utiliserons dans ce cours.

### @param

La balise `@param` permet d'annoter le type des paramètres d'une
fonction.

```js
/**
 * Calcule le carré de `n`.
 *
 * @param {number} n - Nombre à multiplier.
 */
function square(n) {
    return n * n;
}
```

Après la balise `@param`, on place entre accolades le type du paramètre
(`number`), puis son identifiant (`n`), puis une courte description de
celui-ci. L'identifiant et la description doivent être séparés par un
tiret (`-`).

Le type entre accolades peut être un type primitif (`string`, `number`,
`boolean`, `null`, `undefined`), un tableau (`string[]`, `number[]`,
etc.), ou un type d'objet (`Date`, `HTMLElement`, etc.).

La description du paramètre peut être omise si la signification de
celui-ci est claire de par son nom et son contexte.

### @returns

La balise `@returns` permet d'annoter le type de la valeur de retour
d'une fonction.

```js
/**
 * Calcule le carré de `n`.
 *
 * @param {number} n - Nombre à multiplier.
 * @returns {number} Carré de `n`.
 */
function square(n) {
    return n * n;
}
```

Après la balise `@returns`, on place entre accolades le type de la
valeur de retour (`number`), puis une courte description de celle-ci.

Si une fonction ne retourne pas de valeur, alors le type entre accolade
est `void`.

```js
/**
 * Affiche une salution dans la console.
 *
 * @param {string} name - Nom de la personna à saluer.
 * @returns {void}
 */
function greet(name) {
    console.log(`Hi ${name}!`);
}
```

### @type

La balise `@type` permet d'annoter le type d'une variable. La majorité
du temps, cette balise n'est pas nécessaire puisque JSdoc est capable de
deviner le type d'une variable de par la valeur qui lui est affectée.

```js
/** @type {string} */
let foo = "bar";
```

Le _docstring_ ci-dessus, par exemple, est superflu puisqu'une chaîne de
caractères est affectée à `foo` lors de sa déclaration.

La balise `@type` est toutefois requise lorsqu'une variable est déclarée
sans être initialisée, comme c'est la cas pour les propriétés
d'instance.

```js
class Shape {
    /** @type {number} */
    width;

    /** @type {number} */
    height;
}
```

## Valider la documentation

Puisque les _docstrings_ sont des commentaires, ceux-ci sont ignorés par
les interpréteurs JavaScript. Cela dit, il est important de s'assurer
que la documentation d'un programme correspond au code source (une
documentation fautive est encore pire qu'un manque de documentation).

Pour valider la documentation, on peut utiliser la commande
`deno check *.js`. Celle-ci fonctionne seulement si votre projet
contient un fichier de configuration `deno.json`. Pour afficher les
erreurs directement dans votre éditeur de texte, vous pouvez également
inclure dans votre projet le fichier `jsconfig.json` ci-joint.
