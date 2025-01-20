# Visibilité

Un objet a une implémentation _privée_ et une interface _publique_.
Cette séparation impose un _couplage faible_ entre les différentes
parties d'un programme, ce qui rend celui-ci plus facile à modifier.
Puisqu'un objet ne connaît pas l'implémentation privée d'un autre objet,
sa propre implémentation ne peut pas en dépendre. En autant que
l'interface publique d'un objet ne change pas, on peut changer son
implémentation sans craindre de briser les autres objets.

Concrètement, la séparation entre l'implémentation et l'interface est
obtenue en jouant avec la _visibilité_ des propriétés. En JavaScript,
les propriétés d'un objet sont publiques par défaut. Autrement dit, la
valeur d'une propriété peut être vue et changée par n'importe quel autre
objet.

```js
class User {
    password;
}

const u = new User();
u.password = "abc123";
```

Ci-dessus, par exemple, puisque la propriété `password` fait partie de
l'interface publique de `User`, n'importe quelle partie du programme
peut modifier sa valeur. Il sera impossible, notamment, de valider le
nouveau mot de passe.

Pour rendre une propriété privée, on ajoute un quadrillon devant
l'identifiant de celle-ci . Une propriété dont la visibilité est privée
est accessible seulement à l'_intérieur_ du corps de la classe.

```js
class User {
    #password;

    constructor(password) {
        this.#password = password;
    }

    changePassword(newPassword) {
        this.#password = newPassword; // OK
    }
}

const u = new User("abc123");
console.log(u.password, u.#pasword); // undefined
```

Changer la visibilité d'une propriété nous assure qu'il faudra passer
par l'interface publique de l'objet (ici, la méthode `changePassword`)
pour changer sa valeur. Cela permet aussi de faire en sorte qu'il soit
possible de _changer_ la valeur d'une propriété sans _voir_ sa valeur
actuel.

Bien que, en JavaScript, les propriétés soient publiques par défaut, on
suggère généralement l'inverse. Lorsque vous concevez une nouvelle
classe d'objets, prenez plutôt l'habitude de déclarer d'abord ses
propriétés (incluant les méthodes) comme étant privées. L'interface d'un
objet devrait être conçue avec soins. Une fois qu'elle est définie et
que d'autres objets en dépendent, une interface est difficile à changer.
À l'inverse, il est facile de changer la visibilité d'une propriété
privée puisqu'aucun autre objet ne dépend encore de celle-ci.
