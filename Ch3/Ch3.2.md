# Bases de la ramificación y merging
Seguiremos un ejemplo de una linea de trabajo real
## Ramificación Basica
Primero digamos que estas trabajando en un pagina y ya hay `commits` en la rama `master`, quieres trabajar en el problema #53, para crear una nueva rama y cambiar a ella tienes que usar el comando `git checkout -b`
```
    $ git checkout -b iss53
    Switched to a new branch "iss53"
```
al hacer un `commit` en la nueva rama, esta se mueve hacia adelante
```
    $ vim index.html
    $ git commit -a -m 'Create new footer [issue 53]'
```
Te avisan que hay un nuevo error con la pagina y que tienes arreglarlo lo más pronto posible, en Git no tienes que abandonar todo lo que hiciste en el problema #53, y tampoco tienes que hacer mucho esfuerzo para revertir los cambios que hiciste antes de poder trabajar en el nuevo problema, solo tienes que volver a la rama `master`
Pero antes de que lo hagas, recuerda que si en el area en la que estas trabajando hay algun cambio que no se haya hecho `commit` y cause conflicto, no te dejara hacer el cambio, por eso es mejor tener un area limpia antes de hacer el cambio.
En este punto tu repositorio sera el mismo que tenias antes de empezar a trabajar en el problema #53, asi como tu directorio local tambien abra cambiado, ahora haciendo el hotfix
```
    $ git checkout -b hotfix
    Switched to a new branch 'hotfix'
    $ vim index.html
    $ git commit -a -m 'Fix broken email address'
    [hotfix 1fb7853] Fix broken email address
    1 file changed, 2 insertions(+)
 ```
 Podras notar el texto `Fast Forward`, esto se refiere al hecho de que hay una rama que que esta más adelante, pero al hacer esto se mueve hacia adelante a lo que se le llama `Fast Forward`.
 Acabando este `hotfix` ya puedes continuar con arreglar el problema #53, pero para esto primero tenemos que borrar la rama hotfix, se hace de la siguiente manera
 ```
    $ git branch -d hotfix
    Deleted branch hotfix (3a0874c).
```
Ahora puedes regresar a la rama en la que trabajabas
```
    $ git checkout iss53
    Switched to branch "iss53"
    $ vim index.html
    $ git commit -a -m 'Finish the new footer [issue 53]'
    [iss53 ad82d7a] Finish the new footer [issue 53]
    1 file changed, 1 insertion(+)
```
es importante saber que los cambios que se hagan en hotfix no estaran contenidos en los archivos de la rama #53, para tenerlos deberas hacer un merge de la rama `master`a esta, haciando `git merge master` o puedes esperar a cuando empujes esta rama a `master`

## Merging Basico 
Ahora, teniendo el problema #53 arreglado por completo, esta listo para ser `merge`con la rama `master`, lo cual se hace de esta manera
```
    $ git checkout master
    Switched to branch 'master'
    $ git merge iss53
    Merge made by the 'recursive' strategy.
    index.html |    1 +
    1 file changed, 1 insertion(+)
```
Este se ve un poco diferente al anterior, esto es porque hay otros ancestros y snapshots, por lo que hace un `merge` de tres vias, git crea un nuevo snapshot donde este conectado por los tres lados, y crea un nuevo `commit` al que apunta, a este se le refiere como `merge commit` y tiene más de un padre.
al acabar con el problema eliminamos la rama.

## Problemas Basicos al hacer Merge

Ocacionalmente, este proceso no va bien, si cambias la misma parte del mismo archivo en dos ramas diferentes que quieres hacer `merge` y se ve algo como esto
```
    $ git merge iss53
    Auto-merging index.html
    CONFLICT (content): Merge conflict in index.html
    Automatic merge failed; fix conflicts and then commit the result.
```
para poder ver cual es el archivo con problemas realizamos un `git status`
```
    $ git status
    On branch master
    You have unmerged paths.
    (fix conflicts and run "git commit")

    Unmerged paths:
    (use "git add <file>..." to mark resolution)

        both modified:      index.html

    no changes added to commit (use "git add" and/or "git commit -a")
```
Todo aquello que tenga conflictos se puede ver como `unmerged` git da una solución estandar para que puedas abrilo manualmente y arreglarlo
```
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```
esto se puede ver como que la version en `HEAD` es la parte superior mientras que la versión nueva es la de abajo, para solucionarlo, se elimina la sección que no se desea mantener y se ejecuta `git add`.
Si se quiere ver de manera grafica se puede ver con `gir mergetool`
```
    $ git mergetool

    This message is displayed because 'merge.tool' is not configured.
    See 'git mergetool --tool-help' or 'git help config' for more details.
    'git mergetool' will now attempt to use one of the following tools:
    opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc3 codecompare vimdiff emerge
    Merging:
    index.html

    Normal merge conflict for 'index.html':
    {local}: modified file
    {remote}: modified file
    Hit return to start merge resolution tool (opendiff):
```
Para revisar si ya no hay conflictos se ejecuta `git status`
```
    $ git status
    On branch master
    All conflicts fixed but you are still merging.
    (use "git commit" to conclude merge)

    Changes to be committed:

        modified:   index.html
```
Cuando creas que has finalizado se ejecuta `git commit`
```
    Merge branch 'iss53'

    Conflicts:
        index.html
    #
    # It looks like you may be committing a merge.
    # If this is not correct, please remove the file
    #	.git/MERGE_HEAD
    # and try again.


    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    # On branch master
    # All conflicts fixed but you are still merging.
    #
    # Changes to be committed:
    #	modified:   index.html
    #
```

[Anterior](Ch2.1.md)
[Siguiente](Ch2.3.md)
[Indice](Ch3/Indice.md)