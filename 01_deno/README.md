# Deno

Originalement, l'environnement d'exécution de JavaScript était le
navigateur web. C'est d'ailleurs toujours le cas aujourd'hui. À la
différence des autres langages de programmation, un programme JavaScript
n'est pas exécuté à partir de la ligne de commande. Plutôt, c'est la
responsabilité du navigateur web d'interpréter le code source.

Depuis 2010, plusieurs autres environnements d'exécution sont toutefois
disponibles. Au lieu de limiter JavaScript à l'interface du navigateur
web, des plateformes telles que Node, Deno et Bun permettent d'accéder à
l'ensemble du système d'exploitation, permettant ainsi d'exécuter les
programmes JavaScript à partir du terminal.

Ce cours étant un cours de programmation côté client, la plupart de nos
programmes seront exécutés dans un navigateur web. Cela étant dit, à des
fins d'apprentissage, il est parfois utile d'avoir accès à un autre
environnement d'exécution. Lorsque ce sera le cas, nous utiliserons
Deno.

## Installation

La façon la plus simple d'installer Deno est d'utiliser le gestionnaire
de paquets préconisé par votre plateforme. On suggère Scoop sur Windows
et Homebrew sur Mac. Sur Linux, cela dépend de votre distribution.

Windows :

```sh
scoop install deno
```

Mac :

```sh
brew install deno
```

## Utilisation

Pour exécuter un fichier JavaScript avec Deno, il suffit d'exécuter la
commande `deno run` suivie du chemin vers le fichier à exécuter.

Par exemple, la commande suivante exécute le code se trouvant dans le
fichier dont le chemin relatif est `main.js` :

```sh
deno run main.js
```

Pour exécuter un fichier à chaque fois que celui-ci change, vous pouvez
ajouter le drapeau `--watch` (celui-ci est facultatif). Le drapeau
`--allow-all` permet à Deno d'accéder aux fichiers de votre ordinateur
et de faire des requêtes HTTP.

```sh
deno run --allow-all --watch main.js
```

Vous devriez également placer le fichier de configuration `deno.json`
ci-joint à la racine du répertoire où se trouve votre programme.
