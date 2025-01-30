# Programmation asynchrone

Un programme _synchrone_ est un programme où chaque ligne est exécutée
une à la fois, de la première à la dernière. Le terme désigne un style
de programmation séquentielle où l'exécution d'une instruction dépend de
l'exécution des instructions précédentes.

```ts
const n = 1; // étape 1
n = n + 1; // étape 2
console.log(n); // étape 3
```

Comme son nom l'indique, la programmation _asynchrone_ est le contraire
de la programmation synchrone. Dans un programme asynchrone, une
instruction peut ne pas attendre l'exécution des instructions
précédentes, surtout si celles-ci prennent un certain temps. Imaginons,
par exemple, une requête HTTP faite à un serveur particulièrement lent,
ou la désérialisation de nombreuses données. Autrement dit, la
programmation asynchrone permet à un programme d'effectuer plusieurs
tâches en même temps.

## Fonctions de rappel

Une façon d'implémenter un programme asynchrone est d'utiliser les
fonctions de rappel. Une fonction de rappel est une fonction donnée
comme argument à une autre fonction, laquelle est responsable d'appeler
la fonction de rappel au moment opportun. Un fonction qui prend comme
argument une fonction de rappel ou qui retourne une fonction comme
valeur de retour se nomme une _fonction d'ordre supérieur_.

La fonction native `setTimeout`, par exemple, prend deux arguments : une
fonction de rappel, et un délai en millisecondes après lequel la
fonction de rappel est appelée.

```js
const callback = () => console.log("Tick");
setTimeout(callback, 1000);
```

La fonction `setTimeout` est considérée comme asynchrone car les
instructions qui suivent n'ont pas besoin d'attendre la fin du délais
pour être exécutées.

```js
setTimeout(callback, 1000);
console.log("Tack");
```

Si le code ci-dessus était synchrone (c'est-à-dire s'il était exécuté de
façon séquentielle), on s'attendrait à voir s'afficher dans la console
`"Tick"`, puis `"Tack"`. Or, ce n'est pas le cas. Puisque la fonction
`callback` est exécutée après un délais de 1 seconde, `"Tack"` s'affiche
avant `"Tick"`. Ce code est donc asynchrone.

### Enchaîner les fonctions de rappel

Vous remarquerez que la fonction de rappel dicte ce qu'il faut faire une
fois terminée l'opération qui prend un certain temps. Considérons par
exemple une fonction `sendRequest` qui fait semblant d'envoyer un
requête HTTP, et retourne la réponse en format `string` après un court
délais.

```ts
function sendRequest(handler) {
    setTimeout(() => {
        const response = "Hello, World!";
        handler(response);
    }, 1000);
}
```

Ci-dessus, le paramètre `handler` représente la fonction à exécuter une
fois la réponse reçue. La fonction `sendRequest` est asynchrone dans le
sens que les instructions suivantes peuvent être exécutées sans attendre
l'exécution de `handler`.

```ts
console.log("Envoi de la requête"); // synchrone
sendRequest((res) => console.log(`Requête : ${res}`)); // asynchrone
console.log("Exécutée avant la résolution de la requête"); // synchrone
```

## Contagion

On dit parfois que les fonctions asynchrones sont contagieuses. Puisque
qu'il est impossible pour le code synchrone d'accéder à la valeur
produite par une fonction asynchrone, les parties du programme qui
dépendent de cette valeur doivent se trouver dans la fonction de rappel.

Dans le code précédent, la réponse de la requête est seulement
disponible à l'intérieur de la fonction `handler`. Toutes les fonctions
qui dépendent de la réponse doivent donc être aussi asynchrones.
