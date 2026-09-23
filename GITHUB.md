Paso 1
git config --global core.editor "code --wait"

Paso 2
git config --global -e

Paso 3
[init]
    defaultBranch = main
[core]
    editor = code --wait
    autocrlf = input
[user]
    name = nombre_GitHub
    email = correo_GitHub