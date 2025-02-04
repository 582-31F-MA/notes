# Fetch

La fonction native `fetch` permet d'envoyer manuellement des requêtes
HTTP à partir d'un programme JavaScript. Dans sa forme la plus simple,
`fetch` prend comme argument l'adresse URL de la ressource désirée.

Le code ci-dessous, par exemple, permet d'envoyer une requête à
l'adresse `https://www.examplecat.com`.

```js
fetch("https://www.examplecat.com");
```

## Réponse

Puisqu'une requête HTTP prend un certain temps à être acheminée, la
fonction `fetch` est asynchrone. Elle retourne une promesse dont la
valeur éventuelle est un objet de type `Response`.

```js
const res = await fetch("examplecat.com");
console.log(res); // => Response { ... }
```

Comme son nom l'indique, un objet `Response` représente une réponse
HTTP. Vous trouverez le [détail de ses propriétés][Response] sur MDN. On
mentionnera ici la propriété `headers` qui contient les entêtes de la
réponse, la propriété `ok` qui indique si la requête fût un succès ou un
échec, et la méthode `text` qui retourne une promesse dont la valeur
éventuelle correspond au corps de la réponse.

```ts
console.log(res.headers); // => Headers { ... }
console.log(res.ok); // => true

const body = await res.text();
console.log(body); // => "<!doctype html>\n<html>\n<head>\n..."
```

[Response]: https://developer.mozilla.org/en-US/docs/Web/API/Response

## Requête

Outre une adresse URL, la fonction `fetch` prend alternativement comme
argument un objet de type `Request` qui représente la requête à envoyer.
Un objet `Request` peut être créé grâce à son constructeur, lequel prend
comme arguments une adresse URL et un objet d'options. Ce dernier permet
de personnaliser la requête en spécifiant sa méthode, ses entêtes, son
corps, et ainsi de suite. Vous trouverez également le
[détail de ses propriétés][Request] sur MDN.

[Request]: https://developer.mozilla.org/fr/docs/Web/API/Request/Request

```js
const options = {
    method: "POST",
    body: "lorem ipsum",
    headers: {
        "Content-type": "text/plain",
    },
};
const req = new Request("https://www.examplecat.com", options);

fetch(req);
```

On peut aussi appliquer la fonction `fetch` directement sur l'objet
d'options sans créer un objet `Request`. Le résultat sera le même.

```js
fetch("https://www.examplecat.com", options);
```
