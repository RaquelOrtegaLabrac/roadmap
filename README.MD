RUTA WEEK 2 DAY 1

He creado la ruta para todo lo que tenemos que hacer al iniciar un proyecto. Lo hice para mí y pensé que os puede interesar, así que os lo paso.
No está perfecto, pero está bien para tomarlo como referencia.
La idea es que este archivo esté vivo. Estaría bien ir mejorándolo, así que no dudéis en pedirme que cambie cosas.

1. git init
2. git remote add origin link del repo
3. crear editorconfig, gitignore, styles.scss
4. git commit -m "initial commit"
5. git push
6. npm init -y
7. npm install --save-dev sass
8. guardar script en package.json

   Hay que poner la ruta bien, porque habrá quien tenga carpeta de sass y quien no.
   Sin carpeta sería:

   "sass": "sass styles.scss:styles.css -w",

   Con carpeta sería:

   "sass": "sass ./sass/styles.scss:styles.css -w",

9. npm run sass
10. Crear los hooks:

    A) commit-msg

    #!/bin/sh
    . "$(dirname "$0")/\_/husky.sh"

    npx --no -- commitlint --edit ${1}

    B) post-merge

    #!/usr/bin/env sh
    . "$(dirname -- "$0")/\_/husky.sh"

    if git diff HEAD~ HEAD --name-only | grep -E '^(package-lock\.json|yarn\.lock)$';
    then
    echo -e "Detected changes in dependencies, executing npm install..."
    npm i
    fi

    exit 0

    C) pre-commit

    #!/usr/bin/env sh
    . "$(dirname -- "$0")/\_/husky.sh"

    branch="$(git symbolic-ref --short -q HEAD)"

    if [["$branch" = "main" || "$branch" = "master"]]; then
    RED='\033[0;31m'
    echo "${RED}Can't commit because you are the master/main branch"
        NC='\033[0m'
        echo "${NC}You can't commit directly to main branch, please commit from other branch"
    exit 1
    fi

11. npx husky-init && npm install
12. npm install -g @commitlint/cli @commitlint/config-conventional
13. crear el archivo commitlint.config.js
    Este archivo debe incluir el siguiente texto:

    module.exports = {
    extends: ["@commitlint/config-conventional"],
    formatter: "@commitlint/format",
    rules: {
    "header-min-length": [2, "always", 10],
    "header-max-length": [2, "always", 72],
    "header-case": [2, "always", ["sentence-case"]],
    "type-empty": [2, "always"],
    "scope-empty": [2, "always"],
    "subject-empty": [2, "always"],
    },
    };

14. git checkout -b feature/add-husky-hooks-files
    guardar
15. git commit -m "install husky and add hooks"
