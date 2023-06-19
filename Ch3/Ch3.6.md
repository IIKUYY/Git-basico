# Rebasing

## Las bases del rebasing

Al usar rebasing en lugar de hacer un `merge` de tres vias, solo aplica los cambios del ultimo paso y los aplica al anterior, para esto se usa `rebasing` en un ejemplo
```
$ git checkout experiment
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command
```
Esto lo que hace es buscar el ancestro común de ambas ramas, (al cual le quieres hacer `rebase`) haciendo todos los cambios de los `commits` a archivos temporales, posteriormente, regresa la rama actual al estado de la rama a la que se la hce el `rebase` y finalmente aplica todos los cambios.
Los producto de usar `rebase` y de usar `merge` son iguales, la diferencia esta en el historial, mientras que `merge` lo crea paralelo, `rebase` lo hace lineal, esto sirve para mantener un historial más limpio y facil de leer, esto sirve cuando quieres hacer una contribución a un proyecto al que no te vas a integrar, asi haces que el que se encarga no tenga que hacer trabajo de integraación y solo tenga que aplicar un `fast-forward` o una aplicación limpia.

## Aplicaciones interesantes
digamos que tu y un cliente trabajan en paralelo en un proyecto, y quieres agregar los cambios del cliente porque ya estan listos, pero todavia no quieres agregar los tuyos porque les falta testearlos un poco, lo que se puede hacer es un `rebase de la parte del cliente a master de la siguiente manera
```
    $ git rebase --onto master server client
```
de esta manera, haciendo un `fast-forward`
```
    $ git checkout master
    $ git merge client
```
la rama `master` estara totalmente actualizada con los cambios del cliente y tu podras trabajar más limpiamente, ahora cuando acabes de trabajar, haces lo mismo con tu parte
```
    $ git rebase master server
```
y aplicas el mismo `fast-forward`
```
    $ git checkout master
    $ git merge server
```
ahora tienes un proyecto con un historial lineal en lugar de tener un proyecto con varias ramas con diferentes historiales, y teniendo todo el historial en `master` puedes eliminar las ramas sin perder el historial.

## Los peligros del rebase
Basicamente los peligros basicos de hacer `rebase` son el borrar una rama en la cual alguiwn haya basado su trsabajo.
eso porque cuando hacer `rebase` se hacen `commits` similares pero diferentes y se hace un lio cuando colegas quieran hacer `merge` con sus avances.
Esto se puede ver con un ejemplo, si tu clonas un repositorio y trabajas en el, el remoto tendre 1 `commit` (lo usaremos como c para abreviar), mientras que en local tendras hasta el c3, digamos que alguien hace `merge` con cambios que el hizo, se crearan desde el c4 al c6 tanto en el repositorio remoto como en tu local, pero solo tu veras los que hayas hecho tu, ahora, si alguien hace un `git push --force` para sobreescribir el historial, se creara un avance en el `master` remoto, pero tus cambios anteriores a hace un `merge` con el nuevo `master`, estaran ligados al `master` anterior.

## Rebase en el rebase
Para solucionar una situacion como la anterior se siguen los siguientes pasos, solo tienes que hacer un `rebase` como este
```
$ git rebase teamone/master
```
esto lo que hara es:
- Determinar que `commits` son unicos en tu local
- Determinar cuales no estan al dia con el `merge`
- Determinar cuales no se han añadido a la rama objetivo
- aplicar esos `commits`

esto es posible debido a que se comparte un `commit` entre los cambios de tu colega y los tuyos, pero de no ser asi creara un error, tambien se puede usar el comando ` git pull --rebase` para hacerlo en un solo paso.

## Rebase vs Merge
Hay argumentos de porque usar cada uno.
Se usa merge para mantener un historial fiel el cual se puede revisar en cualquier momento para mantener la transparencia en el proyecto.
En cambio se usa rebase para tener una idea más simple y clara de como se gestiono el proyecto y mantener una coherencia y crear un historial más facil de leer para futuros usuarios.

[Anterior](Ch2.5.md)
[Indice](https://github.com/IIKUYY/Git-basico/blob/main/Ch3/README.md)