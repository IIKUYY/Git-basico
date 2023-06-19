# Trabajando con  servidores remotos

Los servidores remotos son versiones en linea del repositorio que tienes localmente, es importante trabajar con estos para asi poder trabajar con varias personas a la vez

## Mostrando tus servidores remotos
Para ver qué servidores remotos tienes configurados, puedes ejecutar el comando `git remote`. Este comando lista los nombres cortos de cada servidor remoto que has especificado. Si has clonado tu repositorio, al menos deberías ver `origin`, que es el nombre predeterminado que Git le da al servidor desde el que hiciste la clonación:
```
    $ git clone https://github.com/schacon/ticgit
    Cloning into 'ticgit'...
    remote: Reusing existing pack: 1857, done.
    remote: Total 1857 (delta 0), reused 0 (delta 0)
    Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
    Resolving deltas: 100% (772/772), done.
    Checking connectivity... done.
    $ cd ticgit
    $ git remote
    origin
```
También puedes especificar la opción `-v`, que te muestra las URL que Git ha almacenado para el nombre corto que se utiliza al leer y escribir en ese servidor remoto:
```
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
```
Si tienes más de un servidor remoto, el comando los lista todos. Por ejemplo, un repositorio con varios servidores remotos para colaborar con varios colaboradores podría verse así:
```
    $ cd grit
    $ git remote -v
    bakkdoor  https://github.com/bakkdoor/grit (fetch)
    bakkdoor  https://github.com/bakkdoor/grit (push)
    cho45     https://github.com/cho45/grit (fetch)
    cho45     https://github.com/cho45/grit (push)
    defunkt   https://github.com/defunkt/grit (fetch)
    defunkt   https://github.com/defunkt/grit (push)
    koke      git://github.com/koke/grit.git (fetch)
    koke      git://github.com/koke/grit.git (push)
    origin    git@github.com:mojombo/grit.git (fetch)
    origin    git@github.com:mojombo/grit.git (push)
```
## Agregando repositorios de servidores en linea
Para agregar un nuevo repositorio Git remoto con un nombre corto al que puedas hacer referencia fácilmente, ejecuta `git remote add <nombre_corto> <url>`:
```
    $ git remote
    origin
    $ git remote add pb https://github.com/paulboone/ticgit
    $ git remote -v
    origin	https://github.com/schacon/ticgit (fetch)
    origin	https://github.com/schacon/ticgit (push)
    pb	https://github.com/paulboone/ticgit (fetch)
    pb	https://github.com/paulboone/ticgit (push)
```
Ahora puedes utilizar la cadena `pb` en lugar de la URL completa en la línea de comandos. Por ejemplo, si deseas obtener toda la información que Paul tiene pero que aún no tienes en tu repositorio, puedes ejecutar `git fetch pb`:
```
    $ git fetch pb
    remote: Counting objects: 43, done.
    remote: Compressing objects: 100% (36/36), done.
    remote: Total 43 (delta 10), reused 31 (delta 5)
    Unpacking objects: 100% (43/43), done.
    From https://github.com/paulboone/ticgit
    * [new branch]      master     -> pb/master
    * [new branch]      ticgit     -> pb/ticgit
```
## "Fetch" y "Pull" en un servidor remoto
para poder hacer un fetch se utiliza el comando:
```
    $ git fetch <remote>
```
El comando se conecta al proyecto remoto y descarga todos los datos que aún no tienes de ese proyecto remoto. Después de hacer esto, deberías tener referencias a todas las ramas de ese remoto.
Si tu rama actual está configurada para hacer seguimiento de una rama remota, puedes utilizar el comando `git pull` para hacer un fetch automáticamente y luego hacer un merge esa rama remota en tu rama actual. Esto puede ser un flujo de trabajo más fácil o cómodo para ti. Por defecto, el comando `git clone` configura automáticamente tu rama local `master` para hacer seguimiento de la rama `master` remota en el servidor desde el que clonaste.
## "Pushing" a tus servidores remotos
El comando para hacer esto es sencillo: `git push <remoto> <rama>`. Si quieres enviar tu rama `master` a tu servidor `origin` puedes ejecutar esto para enviar cualquier confirmación que hayas realizado de vuelta al servidor:
```
    $ git push origin master
```
Este comando solo funciona si clonaste desde un servidor al cual tienes acceso de escritura y si nadie ha enviado cambios desde que lo clonaste.
## Inspeccionar un servidor remoto
Si deseas ver más información sobre un servidor remoto en particular, puedes utilizar el comando `git remote show <remoto>`. Si ejecutas este comando con un nombre corto específico, como `origin`, obtendrás algo como esto:
```
    $ git remote show origin
    * remote origin
    Fetch URL: https://github.com/schacon/ticgit
    Push  URL: https://github.com/schacon/ticgit
    HEAD branch: master
    Remote branches:
        master                               tracked
        dev-branch                           tracked
    Local branch configured for 'git pull':
        master merges with remote master
    Local ref configured for 'git push':
        master pushes to master (up to date)
```
El comando muestra la URL del repositorio remoto, así como la información de seguimiento de la rama. El comando te indica útilmente que si estás en la rama master y ejecutas `git pull`, automáticamente fusionará la rama master del remoto en la rama local después de haberla obtenido. También muestra todas las referencias remotas que se han obtenido, ejemplo de `git remote show`:
```
    $ git remote show origin
    * remote origin
    URL: https://github.com/my-org/complex-project
    Fetch URL: https://github.com/my-org/complex-project
    Push  URL: https://github.com/my-org/complex-project
    HEAD branch: master
    Remote branches:
        master                           tracked
        dev-branch                       tracked
        markdown-strip                   tracked
        issue-43                         new (next fetch will store in remotes/origin)
        issue-45                         new (next fetch will store in remotes/origin)
        refs/remotes/origin/issue-11     stale (use 'git remote prune' to remove)
    Local branches configured for 'git pull':
        dev-branch merges with remote dev-branch
        master     merges with remote master
    Local refs configured for 'git push':
        dev-branch                     pushes to dev-branch                     (up to date)
        markdown-strip                 pushes to markdown-strip                 (up to date)
        master                         pushes to master                         (up to date)
```
## Renombrando y Eliminando Servidores remotos
Puedes utilizar el comando `git remote rename` para cambiar el nombre corto de un servidor remoto. Por ejemplo, si quieres cambiar el nombre de "pb" a "paul", puedes hacerlo con `git remote rename`:
```
$ git remote rename pb paul
$ git remote
origin
paul
```
Si deseas eliminar un servidor remoto por algún motivo, como haber cambiado de servidor o dejar de utilizar un mirror en particular, o tal vez un colaborador ya no está contribuyendo, puedes utilizar `git remote remove` o `git remote rm` para hacerlo:
```
$ git remote remove paul
$ git remote
origin
```

[Anterior](Ch2.4.md)
[Siguiente](Ch2.6.md)
[Indice](https://github.com/IIKUYY/Git-basico/blob/main/Ch2/README.md)