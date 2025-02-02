# Promesses

Une autre façon de faire de la programmation asynchrone est de concevoir
une fonction qui retourne un objet représentant son _éventuelle_ valeur
de retour. Le principe est similaire à un chèque tiré sur un
établissement financier ; l'objet retourné par la fonction asynchrone
est une _promesse_ de paiement, laquelle sera éventuellement résolue en
la valeur attendue.

## Instancier une promesse

En JavaScript, cette promesse de valeur est représentée par un objet de
type `Promise`. Le constructeur `Promise` prend comme argument une
fonction de rappel qui sera exécutée immédiatement. C'est dans cette
fonction que doit se trouver la ou les opérations asynchrones.

Reprenons l'exemple d'une fonction `sendRequest` qui fait semblant
d'envoyer un requête HTTP. Cette fois-ci, plutôt que de prendre comme
argument une fonction de rappel, `sendRequest` retournera un promesse.

```js
/**
 * @returns {Promise<string>}
 */
function sendRequest() {
    function executor(resolve) {
        setTimeout(() => resolve("Hello, World"), 1000);
    }

    return new Promise(executor);
}
```

Première chose à remarquer : l'annotation pour un objet de type promesse
doit inclure entre crochets le type de la valeur en laquelle la promesse
sera éventuellement résolue. Ci-dessus, `sendRequest` promet de produire
une chaîne de caractères. C'est pourquoi le type de sa valeur de retour
est `Promise<string>`.

Seconde chose à remarquer : la fonction de rappel `executor` donnée
comme argument au constructeur `Promise` prend comme argument une autre
fonction de rappel, nommée ci-dessus `resolve`. On appel cette deuxième
fonction de rappel pour résoudre la promesse lorsque la valeur attendue
est prête.

Appelons la fonction `sendRequest` pour voir le résultat :

```ts
const p = sendRequest();
console.log(p); // => Promise { <pending> }
```

## États d'une promesse

Une promesse peut être dans un des trois états suivant :

- En cours (_pending_, en anglais) : une promesse est en cours pendant
  que la ou les opérations asynchrones associées ne sont pas encore
  terminée. Dans l'exemple précédent, la promesse sera en cours pendant
  1000 millisecondes.

- Tenue (_fulfilled_) : une promesse est tenue lorsqu'elle est résolue
  en la valeur attendue. Dans l'exemple précédent, une fois la réponse
  reçue (lorsque la fonction `resolve` est appelée), la promesse est
  tenue.

- Rompue (_rejected_) : une promesse rompue est incapable de produire la
  valeur attendue. Imaginons une requête HTTP envoyée à un serveur
  inexistant. Nous verrons plus tard comment gérer les erreurs liées aux
  promesses.

## Gérer une promesse

Vous aurez rarement à créer vos propres promesses. La plupart du temps,
vous aurez plutôt à les gérer. Une fonction native quelconque dont vous
avez besoin retournera un objet de type `Promise`, et vous devrez
manipuler cet objet pour accéder à la valeur désirée.

Il y a deux façons de gérer un objet de type `Promise`. La première est
d'utiliser la méthode `then`. La seconde est d'utiliser les mots-clés
`async` et `await`. Quoiqu'une promesse soit un objet, on ne peut pas
accéder directement à sa valeur. On doit absolument utiliser une des
deux techniques ci-dessous.

### Then

La méthode `then` est une propriété des objets de type `Promise`. Elle
prend comme argument une fonction de rappel qui sera appelée
automatiquement lorsque la promesse sera résolue. La fonction de rappel
doit avoir un paramètre, lequel représente la valeur enveloppée dans la
promesse.

```js
const p = sendRequest();
p.then((res) => {
    console.log(res); // => "Hello, World"
});
```

Attention, la méthode `then` retourne toujours une promesse. Il est donc
impossible d'affecter directement la valeur de la promesse à une
variable. Tout code qui dépend de la requête devra se trouver dans la
fonction `handleResponse`.

### Async / await

Alternativement, on peut utiliser l'opérateur `await`. La combinaison de
l'opérateur `await` et d'une fonction asynchrone forme une expression
dont on obtiendra la valeur une fois la promesse résolue.

```ts
const res = await sendRequest();
console.log(res); // => "Hello, World"
```

Notez qu'une expression `await` interrompt l'exécution de toutes les
instructions suivantes, même si celles-ci ne dépendent pas de la valeur
de la promesse.

```ts
const res = await sendRequest();
console.log(res); // => "Hello, World"
console.log("foo"); // => "foo"
```

Dans l'exemple ci-dessus, même si l'instruction `console.log("foo");` ne
dépend pas de la valeur de `res`, elle sera tout de même exécutée après
la résolution de la promesse.

Quoique l'opérateur `await` simplifie grandement la gestion de
promesses, il peut seulement être utilisé à l'intérieur d'une fonction
précédée par le mot-clé `async`.

```js
/**
 * @returns {Promise<string>}
 */
async function getBody() {
    const res = await sendRequest();
    return res;
}
```

Une fonction déclarée avec le mot-clé `async` est automatiquement
considérée comme étant asynchrone. Pour cette raison, une telle fonction
retournera toujours un objet de type `Promise`, même si l'instruction de
retour semble indiquer autrement.

```js
/**
 * @returns {Promise<number>}
 */
async function square(n) {
    return n * n;
}

const s = square(2);
console.log(s); // => Promise { 4 }
```

Comme vous pouvez voir, même si le code à l'intérieur de la fonction
`square` ne semble pas asynchrone, puisque celle-ci est déclarée avec le
mot-clé `async`, l'interpréteur JavaScript enveloppe automatiquement sa
valeur de retour dans une promesse.
