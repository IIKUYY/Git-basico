# Ramas remotas

Las referencias remotas son apuntadores del repositorio remoto, puedes ver la lista completa con el comando `git ls-remote <remote>` o `git remote show <remote>` estas sirven para revisar el estado de las ramas en el remoto, estas no se pueden mover y sirven para estar concientes de como estaban antes de realizar los cambios.
Estas toman las formas de `<remote>/<branch>` por ejemplo, si quieres ver el estado de `master` en `origin` debes revisar `<origin>/<master>`.
Si mientras trabajas en una rama, alguien hace un cambio en esta en el remoto, la forma de sincronizar tu local con el remoto es correr `git fetch origin` este actualiza toda la información que este en el remoto que no este en tu local

## Push
Para compartir una rama en un repositorio, debes hacer un `push` o empujar, esto porque el repositorio remoto no se sincroniza con el local automaticamente, si quieres empujar una rama debes usar el comando `git push <remote> <branch>` en un ejemplo, si quieres empujar la rama `serverfix`, se veria
```
    $ git push origin serverfix
    Counting objects: 24, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (15/15), done.
    Writing objects: 100% (24/24), 1.91 KiB | 0 bytes/s, done.
    Total 24 (delta 2), reused 0 (delta 0)
    To https://github.com/schacon/simplegit
    * [new branch]      serverfix -> serverfix
```
Este es un atajo, Git lo expande a `refs/heads/serverfix:refs/heads/serverfix` tambien puedes usar `git push origin serverfix:serverfix` usando un comando similar, pero para cambiar el nombre del repositorio en el remoto `git push origin serverfix:awesomebranch`, esto hace que la proxima vez que alguien use `git fetch` esa persona vea algo asi
```
    $ git fetch origin
    remote: Counting objects: 7, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 3 (delta 0), reused 3 (delta 0)
    Unpacking objects: 100% (3/3), done.
    From https://github.com/schacon/simplegit
    * [new branch]      serverfix    -> origin/serverfix
```
Es importante saber que si haces esto no tienes una copia editable de `serverfix`, para tenerla tienes que usar el comando `git merge origin/serverfix` lo cual se veria
```
    $ git checkout -b serverfix origin/serverfix
    Branch serverfix set up to track remote branch serverfix from origin.
    Switched to a new branch 'serverfix'
```

## Rastreando ramas
Revisando una rama local desde una rama de rastreo remoto crea automaticamente algo llamado `tracking branch` (la rama que rastrea se llama `upstream branch`) estas ramas estan ligadas a una en el remoto, si haces `pull` en estas, Git automaticamente sabe a que servidor hacer `fetch` y a cual hacer `merge`.
Cuando clonas un reposiorio, generalmente automaticamente se crea un `master` que rastrea a `origin/master` sin embargo, puedes crear las `tracking branch` que tu desees usando `git checkout -b <branch> <remote>/<branch>`, en un ejemplo
```
    $ git checkout --track origin/serverfix
    Branch serverfix set up to track remote branch serverfix from origin.
    Switched to a new branch 'serverfix'
```
Incluso hay un atajo para este, para usar este deben tener el mismo nombre
```
    $ git checkout serverfix
    Branch serverfix set up to track remote branch serverfix from origin.
    Switched to a new branch 'serverfix'
```
Ahora, si quieres que tengan nombres diferentes debes hacer
```
    $ git checkout -b sf origin/serverfix
    Branch sf set up to track remote branch serverfix from origin.
    Switched to a new branch 'sf'
```
si ya tienes una rama la cual quieres usar, tienes que usar
```
    $ git branch -u origin/serverfix
    Branch serverfix set up to track remote branch serverfix from origin.
```
si quieres ver cuales `tracking branches` tienes, usas
```
    $ git branch -vv
    iss53     7e424c3 [origin/iss53: ahead 2] Add forgotten brackets
    master    1ae2a45 [origin/master] Deploy index fix
    * serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] This should do it
    testing   5ea463a Try something new
```
se puede ver que algunas estan adelantadas pero otras estan atrasadas, para que no esten atrasadas y vayan al dia debes usar
```
    $ git fetch --all; git branch -vv
```

## Pull
Una alternativa a `git fetch` es usar `git pull`, usar este comando lo que hace es una combinación de `fetch` y de `merge`, no siempre se usa porque puede causar confusión y es más sencillo usar estos dos por separado.

## Eliminar las ramas remotas
Cuando todos acaben de usar una rama puedes borrar esta usando el comando `git push --delete <branch>` en un ejemplo
```
    $ git push origin --delete serverfix
    To https://github.com/schacon/simplegit
    - [deleted]         serverfix
```
Esto lo que hace es eliminar el apuntador del servidor, Git mantiene la información hasta que hagan una `garbage collection` esto hace que sea facilmente recuperable

[Anterior](Ch2.4.md)
[Siguiente](Ch2.6.md)
[Indice](Ch3/Indice.md)