# Exceptions

Même si un programme ne contient pas d'erreur logique, des problèmes
peuvent tout de même survenir lors de son exécution, notamment lorsqu'il
communique avec le monde extérieur. Considérons une entrée mal formée ou
bien une défaillance réseau. Jusqu'à présent, nous avons ignoré ces
possibilités, mais un programme robuste doit les prendre en compte.

Pour gérer ces conditions exceptionnelles qui surviennent pendant
l'exécution d'un programme, plusieurs langages de programmations
utilisent les _exceptions_. Un système de gestion d'exceptions est une
structure de contrôle qui permet d'interrompre l'exécution normale d'un
programme afin de traiter un problème.

Imaginons par exemple une fonction dont la tâche est de retirer d'un
compte en banque un montant d'argent donné. Que faire si le montant à
retirer est supérieur à la balance du compte ? La fonction pourrait
simplement ne rien faire, mais alors il est impossible pour nous de
savoir qu'une opération problématique s'est produite.

```js
class Account {
    balance;

    constructor(balance = 0) {
        this.balance = balance;
    }

    /**
     * Withdraw a given `amount` from the account's balance.
     *
     * @param {number} amount
     * @returns {number}
     */
    withdraw(amount) {
        if (amount > this.balance) return this.balance; // ne fait rien en cas d'erreur
        return this.balance -= amount;
    }
```

## Lever une exception

Plutôt que de ne rien faire, la fonction devrait plutôt aviser celui ou
celle qui l'a appelée qu'une erreur s'est produite. Pour ce faire, on
utilise le mot-clé `throw`, lequel permet de lever une exception.

```js
class Account {
    balance: number;

    constructor(balance: number) {
        this.balance = balance;
    }

    /**
     * Withdraw a given `amount` from the account's balance.
     *
     * @param {number} amount
     * @throws {Error}
     * @returns {number}
     */
    withdraw(amount) {
        if (amount > this.balance) {
            throw new Error("Insufficient funds"); // lève une exception
        }
        return this.balance -= amount;
    }
}
```

Lever une exception est similaire à l'acte de retourner une valeur.
Comme `return`, une instruction `throw` stoppe l'exécution de la
fonction. À la différence de `return`, une instruction `throw`
n'interrompt toutefois pas seulement la fonction courante, mais elle
remonte la pile d'appels jusqu'à la première instruction `catch`. S'il
n'y a pas d'instruction `catch`, alors l'exécution du programme est
interrompue.

Vous remarquerez en outre que l'exception est documentée avec la balise
`@throws`.

```js
function f() {
    const account = new Account(10);
    const balance = account.withdraw(15);
    console.log(balance); // cette ligne ne sera pas exécutée
}

f(); // => Uncaught Error: Insufficient funds
console.log("Cette ligne ne sera pas exécutée");
```

Si vous exécutez le code ci-dessus et que vous lisez le message
d'erreur, vous verrez comment l'interpréteur remonte la pile d'appel, en
commençant par l'instruction `throw` dans la méthode `withdraw`, puis
l'appel de `a.withdraw`, puis l'appel de `f`. L'erreur s'est propagée
dans la pile d'appel un peu comme un événement se propage dans le DOM.

## Intercepter une exception

Pour intercepter une exception, il faut utiliser les instructions `try`
et `catch`. On place la ou les opérations qui risquent d'échouer dans un
bloc `try`, et on indique dans un bloc `catch` la marche à suivre en cas
d'erreur.

```js
try {
    const account = new Account(10);
    const balance = account.withdraw(15);
} catch (err) {
    console.log(err);
}
```

La variable entre parenthèse après le mot-clé `catch` correspond à
l'exception levée par l'instruction `throw`. L'exception est
généralement une instance ou une extension du type natif `Error`.

Notez que l'interception peut se faire à n'importe quel niveau de la
pile d'appel. Pour reprendre l'exemple précédent, le `try..catch` peut
se trouver autant dans la fonction `f` où est appelée `withdraw` que
dans le portée globale où est appelée `f`. Tout dépend de quelle
fonction sait comment gérer l'erreur.

## Types d'erreurs

Généralement, il est conseillé de spécifier quel type d'erreur on
souhaite intercepter. Certains types d'exception (les erreurs de
syntaxe, notamment) ne devraient pas être interceptés puisqu'elles
constituent des erreurs logiques.

Pour cette raison, plutôt que lever une exception générique du type
natif `Error`, on créera une ou plusieurs extensions spécifiques aux
erreurs qui peuvent se produire dans notre programme.

```js
class InsufficientFundsError extends Error {}

class Account {
    // ...

    /**
     * Withdraw a given `amount` from the account's balance.
     *
     * @param {number} amount
     * @throws {InsufficientFundsError}
     * @returns {number}
     */
    withdraw(amount) {
        if (amount > this.balance) {
            throw new InsufficientFundsError("Insufficient funds");
        }
        return this.balance -= amount;
    }
}
```

Ces extensions permettent ensuite de spécifier quel type d'erreur nous
voulons intercepter. À l'aide de l'opérateur `instanceof`, on ajoutera
ainsi une clause de garde dans le bloc `catch` pour filtrer les
exceptions dont le type ne correspond pas à celui recherché.

```js
try {
    const account = new Account(10);
    const balance = account.withdraw(15);
} catch (err) {
    if (!(err instanceof InsufficientFundsError)) throw err; // clause de garde
    console.log(err);
}
```

Le code ci-dessus, par exemple, intercepte seulement les exceptions de
type `InsufficientFundsError`. Les autres exceptions ne seront pas
interceptées, et elles continuerons à remonter la pile d'appels.

## Approche alternative

On soulève souvent deux problèmes avec les exceptions :

1. Premièrement, il est trop facile de les ignorer. Rien ne force le ou
   la programmeuse à gérer les exceptions, et il peut être difficile de
   déterminer le bon endroit pour les intercepter. Or, pour être
   robuste, un programme doit tenir compte des erreurs potentielles, et
   non en faire fi.

2. Deuxièmement, l'instruction `throw` rend plus difficile la
   compréhension du code. Non seulement est-ce un mot-clé supplémentaire
   à mémoriser, mais la propagation des exceptions dans la pile d'appel
   peut porter à confusion.

Plusieurs langages de la famille dite « fonctionnelle » offrent une
alternative plus simple au système des exceptions. Plutôt que de lancer
une exception, une fonction dont l'exécution peut causer erreur inclus
l'erreur dans sa valeur de retour. La fonction retourne soit la valeur
attendue, soit une erreur. Pour accéder à la valeur attendue, le ou la
programmeuse n'a donc pas d'autre choix que de gérer l'erreur.
Aujourd'hui, cette approche est généralement préconisée dans les
langages qui la supporte.

Dans un sens, vous êtes déjà familiers et familières avec cette
pratique. La méthode `querySelector`, par exemple, retourne soit un
objet qui représente un élément dans le DOM, soit `null` si aucun
élément correspond au sélecteur CSS donné. Pour utiliser la valeur de
retour de `querySelector`, il faut donc s'assurer que celle-ci n'est pas
`null`.

```js
const node = document.querySelector(".foo");
console.log(node); // => soit `Element`, soit `null`
if (!node) console.log("Il n'y a pas d'élément avec la classe « foo »");
else console.log(node); // => `Element`
```

La même idée peut être appliquée à la méthode `withdraw` discutée
précédemment. Plutôt que de lancer une exception si le montant à retirer
du compte est supérieur à la balance, on pourrait inclure l'erreur dans
la valeur de retour.

```js
//..

class Account {
    // ...

    /**
     * Withdraw a given `amount` from the account's balance.
     *
     * @param {number} amount
     * @returns {number|InsufficientFundsError}
     */
    withdraw(amount) {
        if (amount > this.balance) {
            return InsufficientFundsError("Insufficient funds");
        }
        return this.balance -= amount;
    }
}
```

Comme vous pouvez voir, la signature de `withdraw` a changé. La méthode
retourne désormais une valeur de type `number` _ou_ de type
`InsufficientFundsError`. Pour utiliser la valeur de retour de
`withdraw`, il faudra ainsi vérifier que ce n'est pas une erreur. Pour
ce faire, on utilise normalement une clause de garde.

```js
const balance = account.withdraw();
console.log(typeof balance); // number ou object
if (balance instanceof InsufficientFundsError) {
    // ... gérer l'erreur en affichant un message, par exemple
}
console.log(typeof balance); // => number
```

## Tester les exceptions

Pour valider qu'une fonction lève ou retourne l'exception attendue, on
peut utiliser respectivement les méthodes `toThrow` ou `toBeInstanceOf`.

Par exemple :

```js
const account = new Account(0);
expect(account.withdraw(5)).toThrow(InsufficientFundsError); // pour tester un lancement
expect(account.withdraw(5)).toBeInstanceOf(InsufficientFundsError); // pour tester une valeur de retour
```
