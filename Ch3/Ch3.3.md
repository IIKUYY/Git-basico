 Manejo de ramas

Veremos como manejar de manera como manejar las ramas de una manera más efectiva.

## Manejo de ramas
El comando `git branch` si se ejecuta sin argumentos, mostrara una lista de las ramas
```
    $ git branch
    iss53
    * master
    testing
```
el `*` denota la rama en la que se encuentra, si quiere ver el ultimo `commit` realizado en cada rama se ejecuta `git branch -v`
```
    $ git branch -v
    iss53   93b412c Fix javascript issue
    * master  7a98805 Merge branch 'iss53'
    testing 782fd34 Add scott to the author list in the readme
```
el comando `--merged` `--no-merged` sirve para ver las ramas que esten o no `merged` a la rama en la que te encuentras
```
$ git branch --merged
  iss53
* master
```

```
    $ git branch --no-merged
    testing
```
Esto puede servir para revisar las ramas que ya pueden ser eliminadas con el comando `git branch -d [branch]`

## Cambiando el nombre de una rama
Para cambiar el nombre de una rama y mantener el historial se siguen los siguientes pasos:
```
$ git branch --move bad-branch-name corrected-branch-name
```
Esto cambia el nombre pero solo en el local, tienes que empujarlo al remoto
```
    $ git push --set-upstream origin corrected-branch-name
```
revisandolo
```
$ git branch --all
* corrected-branch-name
  main
  remotes/origin/bad-branch-name
  remotes/origin/corrected-branch-name
  remotes/origin/main
```
la rama nueva ya esta disponible en el remoto, pero la rama con el nombre equivocado sigue existiendo por lo que es necesesario borrarla con el siguiente comando
```
    $ git push origin --delete bad-branch-name
```

## Cambiando el nombre de la rama master
usando el comando
```
$ git branch --move master main
```
para empujarlo al remoto
```
$ git push --set-upstream origin main
```
revisando
```
$ git branch --all
* main
  remotes/origin/HEAD -> origin/master
  remotes/origin/main
  remotes/origin/master
```
es necesario tomar ciertas acciones antes de borrar `master`

- Cualquier proyecto que dependa de este deberá actualizar su código y/ o configuración.
- Actualizar cualquier archivo de configuración del ejecutor de pruebas.
- Ajustar los scripts de compilación y lanzamiento.
- Modificar la configuración en tu plataforma de repositorios, como la rama predeterminada del repositorio, las reglas de fusión y otros elementos que coincidan con los nombres de las ramas.
- Actualizar las referencias a la antigua rama en la documentación.
- Cerrar o fusionar cualquier solicitud de extracción dirigida a la antigua rama.

Ahora si puedes eliminar la rama con
```
$ git push origin --delete master
```

[Anterior](Ch2.2.md)
[Siguiente](Ch2.4.md)
[Indice](Ch3/Indice.md)