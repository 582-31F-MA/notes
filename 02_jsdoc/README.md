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

### @returns

La balise `@returns` permet d'annoter le type de la valeur de retour
d'une fonction.

```js
/**
 * Calcule le carré de `n`.
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
 * @param {string} name - Nom de la personna à saluer.
 * @returns {void}
 */
function greet(name) {
    console.log(`Hi ${name}!`);
}
```

## Valider la documentation

Puisque les _docstrings_ sont des commentaires, ceux-ci sont ignorés par
les interpréteurs JavaScript. Cela dit, il est important de s'assurer
que la documentation d'un programme correspond au code source (une
documentation fautive est encore pire qu'un manque de documentation).
Pour ce faire, on peut utiliser la commande `deno check *.js` (n'oubliez
pas votre fichier de configuration `deno.json`). On peut aussi inclure
le fichier `jsconfig.json` ci-joint pour que les erreurs s'affichent
directement dans votre éditeur de texte.
