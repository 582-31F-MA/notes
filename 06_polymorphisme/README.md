# Polymorphisme

Le polymorphisme, du grec ancien _polús_ (plusieurs) et _morphê_
(forme), consiste à fournir une interface unique à des valeurs de
différents types, le but étant de généraliser le code. Une même fonction
peut ainsi être appliquée autant sur des nombres que des chaînes, ou
bien sur des instances de classes distinctes. La méthode `console.log`,
par exemple, est polymorphique car on peut l'appeler avec des arguments
de plusieurs types.

```ts
console.log(1); // => 1 (nombre)
console.log("foo"); // => "foo" (chaîne)
```

On distingue plusieurs sortes de polymorphisme :

- polymorphimse _ad hoc_ ;
- polymoprhisme paramétré ; et
- polymorphisme par sous-typage.

En programmation objet, le polymorphismme par sous-typage (aussi appelé
« héritage ») est le type le plus répandu.

## Héritage

L'héritage est un mécanisme par lequel une classe peut hériter des
propriétés d'une classe parente, parfois appelée « super-classe ».

Considérons par exemple un site web ayant des comptes usagers. Un compte
usager possède une adresse courriel ainsi qu'un mot de passe. Il peut
aussi changer ces informations à l'aide de différentes méthodes.

```ts
class User {
    email;
    password;

    constructor(email, password) {
        this.email = email;
        this.password = password;
    }

    changeEmail(email) {
        this.email = email;
    }
}
```

Notre site web a également des comptes administrateurs, lesquels ont
accès à des fonctionnalités _en plus_ de celles des comptes usagers.
Autrement dit, un compte administrateur est un compte usager, mais avec
des propriétés supplémentaires. En JavaScript, cette relation
d'extension est identifiée avec le mot-clé `extends`.

```ts
class Admin extends User {
    getMetrics() {/* ... */}
}
```

Une extension hérite des propriété de sa classe parente. La classe
`Admin` a ainsi les mêmes propriétés que `User`, et ce, sans avoir à les
déclarer de nouveau. Elle déclare également une méthode `getMetrics` qui
sera accessible seulement sur les instances de `Admin`.

```ts
const a = new Admin("bar@email.com", "abc123");
console.log(a); // => Admin { email: "bar@email.com", password: "abc123" }
a.changeEmail("foo@email.com");
```

Puisque la méthode `changeEmail` est dorénavant accessible sur les
objets de types `User` ainsi que sur ceux de types `Admin`, on dira
qu'elle est polymorphique.

### Redéfinition

Il est possible de redéfinir une ou plusieurs propriétés des classes
enfants. Dans l'exemple précédent, `Admin` hérite du constructeur de
`User`, mais cela n'est pas toujours désirable. Imaginons que l'on
veuille initialiser une propriété `domain` propre à `Admin`. Il faudrait
alors redéfinir le constructeur.

```ts
class Admin extends User {
    domain;

    constructor(email, password, domain) {
        super(email, password); // constructeur de User
        this.domain = domain;
    }
}
```

Pour redéfinir une propriété de la classe parente, il suffit de déclarer
la propriété de nouveau dans la classe enfant. Cette nouvelle définition
« écrasera » l'original pour les instances de la classe enfant.

Si une classe enfant redéfinie son constructeur, on doit toutefois
s'assurer d'appeler le constructeur de la classe parente. Sinon, les
propriétés déclarées dans la classe parente ne seront pas initialisées.
Pour se faire, on utilise la fonction native `super` qui représente le
constructeur de la _super_ classe.

### Valeur de profondeur

La valeur de profondeur d'une classe est sa position dans sa _hiérarchie
d'héritage_. Considérons la hiérarchie d'héritage suivante : un compte
administrateur est un compte modérateur, qui est un compte premium, qui
est un compte vérifié, qui est un compte utilisateur.

```
Admin → Mod → Premium → Verified → User
  |      |       |          |       |
  4      3       2          1       0
```

Dans cette hiérarchie d'héritage, la classe `Admin` a une valeur de
profondeur de 4 puisqu'elle hérite indirectement des quatre classes
précédentes.

En règle général, on évitera de créer des classes dont la valeur de
profondeur est supérieure à 1 ou 2. Plus une hiérarchie d'héritage est
profonde, plus il est difficile de comprendre le fonctionnement des
classes qui en font partie. Par ailleurs, une classe enfant _dépend_ de
l'implémentation de sa classe parente, laquelle _dépend_ de sa propre
classe parente, et ainsi de suite. Le but premier de la programmation
objet étant de limiter l'interdépendance des diverses parties d'un
programme, on peut voir comment l'héritage est une pratique à utiliser
avec prudence et modération.

## Composition

La composition est une façon alternative (et souvent préférable) de
formuler la relation entre deux types d'objets. Alors qu'une relation
d'héritage s'exprime sous la forme « est un » (_is a_, en anglais), une
relation de composition s'exprime sous la forme « a un » (_has as_).
Pour transformer l'exemple précédent en une relation de composition, on
pourrait dire qu'un compte _a un_ rôle ; celui d'administrateur, ou
d'utilisateur, ou de modérateur, etc.

Au contraire de l'héritage, la composition encourage une organisation
horizontale. Il n'y a pas de relation hiérarchique entre les types
d'objets. L'implémentation interne d'une classe ne dépendra donc jamais
de la classe avec laquelle elle est en relation. Plutôt, l'objet
accèdera aux propriétés de son objet « frère » à travers son interface,
comme n'importe quel autre objet.

Considérons par exemple les employé·es et les employeur·es d'une
organisation. Avec l'héritage, on définirait ces deux classes comme
étant des extensions d'une classe parente — disons « salarié·e ».

```ts
class Salaried {
    raise(newSalary: number) {/* ... */}
}

class Employer extends Salaried {/* ... */}
class Employee extends Salaried {/* ... */}
```

Les méthodes partagées par `Employee` et `Employer` sont déclarées dans
`Salaried`, et accessibles directement aux extensions de cette dernière.

```ts
const e = new Employee();
e.raise(100000);
```

Avec la composition, on dirait plutôt que les employé·es et les
employeur·es _ont un_ salaire. Celui-ci serait représenté par une classe
`Salary` dans laquelle seraient déclarées les méthodes qui permettent de
manipuler le salaire.

```ts
class Salary {
    raise(newSalary: number) {/* ... */}
}

class Employee {
    salary: Salary;

    constructor(salaray: Salary) {
        this.salary = salary;
    }
}
```

Puisque la valeur de la propriété `salary` est une instance de la classe
`Salary`, les méthodes de cette dernière sont accessibles à partir de la
propriété.

```ts
const s = new Salary();
const e = new Employee(s);
e.salary.raise(100000);
```
