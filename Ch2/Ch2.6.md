# Etiquetando

## Listando las etiquetas
Listar las etiquetas existentes en Git es sencillo. Solo tienes que escribir `git tag` (opcionalmente con `-l` o `--list`):
```
    $ git tag
    v1.0
    v2.0
```
Este comando muestra las etiquetas en orden alfabético; el orden en el que se muestran no tiene una importancia real.
También puedes buscar etiquetas que coincidan con un patrón específico. Por ejemplo, el repositorio fuente de Git contiene más de 500 etiquetas. Si estás interesado en ver solo la serie 1.8.5, puedes ejecutar esto:
```
    $ git tag -l "v1.8.5*"
    v1.8.5
    v1.8.5-rc0
    v1.8.5-rc1
    v1.8.5-rc2
    v1.8.5-rc3
    v1.8.5.1
    v1.8.5.2
    v1.8.5.3
    v1.8.5.4
    v1.8.5.5
```

## Creando etiquetas
Git admite dos tipos de etiquetas: ligeras (*lightweight*) y anotadas (*annotated*).
Una etiqueta ligera es muy similar a una rama que no cambia; simplemente es un puntero a un commit específico.
Por otro lado, las etiquetas anotadas se almacenan como objetos completos en la base de datos de Git. Tienen un checksum, contienen el nombre, correo electrónico y fecha del etiquetador, tienen un mensaje de etiquetado y pueden ser firmadas y verificadas con GNU Privacy Guard (GPG).

## Etiquetas anotadas
Crear una etiqueta anotada en Git es sencillo. La forma más fácil es especificar la opción `-a` al ejecutar el comando `tag`:
```
    $ git tag -a v1.4 -m "my version 1.4"
    $ git tag
    v0.1
    v1.3
    v1.4
```
El parámetro `-m` especifica un mensaje de etiquetado, que se almacena junto con la etiqueta. Si no especificas un mensaje para una etiqueta anotada, Git abrirá tu editor para que puedas escribirlo.
Puedes ver los datos de la etiqueta junto con el commit que se etiquetó utilizando el comando `git show`:
```
    $ git show v1.4
    tag v1.4
    Tagger: Ben Straub <ben@straub.cc>
    Date:   Sat May 3 20:19:12 2014 -0700
    
    my version 1.4
    
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700
    
        Change version number
```
## Etiquetas ligeras
Otra forma de etiquetar commits es con una etiqueta ligera (lightweight tag). Básicamente, consiste en el checksum del commit almacenado en un archivo, sin ningún otra información adicional. Para crear una etiqueta ligera, no utilices las opciones `-a`, `-s` o `-m`, simplemente proporciona un nombre de etiqueta:
```
    $ git tag v1.4-lw
    $ git tag
    v0.1
    v1.3
    v1.4
    v1.4-lw
    v1.5
```
En este caso, si ejecutas el comando `git show` en la etiqueta, no verás la información adicional de la etiqueta. El comando solo muestra el commit:
```
    $ git show v1.4-lw
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700
    
        Change version number
```
## Etiquetando después
También puedes etiquetar commits después de haberlos pasado. Supongamos que tu historial de commits se ve así:
```
    $ git log --pretty=oneline
    15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
    a6b4c97498bd301d84096da251c98a07c7723e65 Create write support
    0d52aaab4479697da7686c15f77a3d64d9165190 One more thing
    6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
    0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc Add commit function
    4682c3261057305bdd616e23b64b0857d832627b Add todo file
    166ae0c4d3f420721acbb115cc33848dfcc2121a Create write support
    9fceb02d0ae598e95dc970b74767f19372d61af8 Update rakefile
    964f16d36dfccde844893cac5b347e7b3d44abbc Commit the todo
    8a5cbc430f1a9c3d00faaeffd07798508422908a Update readme
```
Ahora, supongamos que olvidaste etiquetar el proyecto en v1.2, que corresponde al commit "Update rakefile". Puedes hacerlo después del hecho. Para etiquetar ese commit, debes especificar el checksum del commit (o parte de él) al final del comando:
```
    $ git tag -a v1.2 9fceb02
```
puedes ver el commit etiquetado
```
    $ git tag
    v0.1
    v1.2
    v1.3
    v1.4
    v1.4-lw
    v1.5
    
    $ git show v1.2
    tag v1.2
    Tagger: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Feb 9 15:32:16 2009 -0800
    
    version 1.2
    commit 9fceb02d0ae598e95dc970b74767f19372d61af8
    Author: Magnus Chacon <mchacon@gee-mail.com>
    Date:   Sun Apr 27 20:43:35 2008 -0700
    
        Update rakefile
    ...
```
## Compartiendo etiquetas
De forma predeterminada, el comando `git push` no transfiere las etiquetas a los servidores remotos. Deberás enviar explícitamente las etiquetas a un servidor compartido después de haberlas creado. Este proceso es similar a compartir ramas remotas: puedes ejecutar `git push origin <nombre_etiqueta>`.
```
    $ git push origin v1.5
    Counting objects: 14, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (12/12), done.
    Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
    Total 14 (delta 3), reused 0 (delta 0)
    To git@github.com:schacon/simplegit.git
     * [new tag]         v1.5 -> v1.5
 ```
 Si tienes muchas etiquetas que deseas enviar todas a la vez, también puedes usar la opción `--tags` con el comando `git push`. Esto transferirá todas tus etiquetas al servidor remoto que aún no estén allí.
 ```
     $ git push origin --tags
    Counting objects: 1, done.
    Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
    Total 1 (delta 0), reused 0 (delta 0)
    To git@github.com:schacon/simplegit.git
     * [new tag]         v1.4 -> v1.4
     * [new tag]         v1.4-lw -> v1.4-lw
 ```
 ## Eliminando una etiqueta
 Para eliminar una etiqueta de tu repositorio local, puedes usar `git tag -d <tagname>`. Por ejemplo, podríamos eliminar nuestra etiqueta ligera de la siguiente manera:
 ```
     $ git tag -d v1.4-lw
    Deleted tag 'v1.4-lw' (was e7d5add)
```
Ten en cuenta que esto no elimina la etiqueta de ningún servidor remoto. Hay dos variaciones comunes para eliminar una etiqueta de un servidor remoto.
La primera variación es `git push <remote> :refs/tags/<tagname>:`
```
    $ git push origin :refs/tags/v1.4-lw
    To /git@github.com:schacon/simplegit.git
     - [deleted]         v1.4-lw
 ```
 La forma de interpretar lo anterior es considerar que el valor nulo antes de los dos puntos se está empujando al nombre de la etiqueta remota, eliminándola efectivamente.
La segunda forma (y más intuitiva) de eliminar una etiqueta remota es con:
```
    $ git push origin --delete <tagname>
```
## Cambiando de etiquetas
Si deseas ver las versiones de los archivos a las que apunta una etiqueta, puedes hacer un `git checkout` de esa etiqueta. Sin embargo, esto coloca tu repositorio en un estado de "cabeza desprendida" ("detached HEAD"), lo cual tiene algunos efectos secundarios no deseados:
```
    $ git checkout v2.0.0
    Note: switching to 'v2.0.0'.
    
    You are in 'detached HEAD' state. You can look around, make experimental
    changes and commit them, and you can discard any commits you make in this
    state without impacting any branches by performing another checkout.
    
    If you want to create a new branch to retain commits you create, you may
    do so (now or later) by using -c with the switch command. Example:
    
      git switch -c <new-branch-name>
    
    Or undo this operation with:
    
      git switch -
    
    Turn off this advice by setting config variable advice.detachedHead to false
    
    HEAD is now at 99ada87... Merge pull request #89 from schacon/appendix-final
    
    $ git checkout v2.0-beta-0.1
    Previous HEAD position was 99ada87... Merge pull request #89 from schacon/appendix-final
    HEAD is now at df3f601... Add atlas.json and cover image
```
En el estado "detached HEAD" (cabeza desprendida), si realizas cambios y luego creas un commit, la etiqueta (tag) permanecerá igual, pero tu nuevo commit no pertenecerá a ninguna rama y será inaccesible, excepto a través del hash exacto del commit. Por lo tanto, si necesitas hacer cambios, por ejemplo, si estás corrigiendo un error en una versión anterior, generalmente querrás crear una rama:
```
    $ git checkout -b version2 v2.0.0
    Switched to a new branch 'version2'
```

[Anterior](Ch2.5.md)
[Siguiente](Ch2.7.md)
[Indice](Ch2/Indice.md)