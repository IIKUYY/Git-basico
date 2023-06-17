# Manteniendo un proyecto

Ya que se vio como contribuir a un proyecto, veremos como mantener el propio

## Creando un nuevo repositorio
Para crear un nuevo repositorio tenemos que dar click en la opción de crear nuevo repositorio, lo que abrira una pestaña que te pedira el nombre del repositorio, si hacerlo publico o privado, y si crear un `README`, este se llamara `<user>/<project_name>`, como no hay codigo solo se mostraran las instrucciones de como conectar un proyecto Git existente, con este ya creado cualquiera con el URL podra acceder, y si tiene los permisos, modificar el repositorio.

## Agregando colaboradores
Si estas trabajando con otras personas a las que les quieras dar permiso de hacer `commits` necesitaras agregarlos como colaboradores, para hacer esto, en la Ajustes en la parte inferior de la barra de opciones de la izquierda, ya en ajustes, selecciona colaboradores, ahi podra escribir el nombre de usuario de la persona a la que se desea agregar, a su vez, se veran cruces en la parte derecha de los nombres de los colaboradores actuales, esta los elimina.

## Manejando pull request
Ahora que tienes un proyecto con algo de codigo y puede que algunos colaboradores, veamos que hacer cuando recibes un `pull request`
Los `pull request` pueden venir de dos lugares, de un `fork` o de una rama del mismo repositorio, en el primer caso este viene de alguien que no tiene derechos de modificación, mientras que el segundo viene de alguien que puede modificar el repositorio

### Notificaciones por correo
Si alguien crea un `pull request` sobre  un repositorio que tu tengas, te llegara una notificación por correo de este PR, algo para notar de esta notificación es el hecho de que te da información clave como el nombre y descripción del `commit`, los archivos cambiados, por cuanto fueron cambiados, el URL al PR y a al `unified diif`, ademas de lo anterior, tambien da un comando el cual se ve `git pull <url> patch-1`, este lo que hace es hacer `merge` en una rama remota sin tener que agregar un remoto.

### Colaborando en un Pull request
Como se ha comentado antes, se puede tener una conversación con quien hizo el `pull request` asi como comentar en lineas de codigo especificas, en `commits` enteros o en el `pull request` completo, esto pudiendo usar markdown, cada que alguien comente en el `pull request` recibiras una no¿tificación de correo, cada uno tendra un link a la conversación para poder contestar.
Una vez hayas aceptado los cambios, puedes hacer `merge`, para esto hay varias maneras, una de estas es hacer `pull` del codigo y hacer el `merge` local, otra es con la manera que vimos antes, o agregando el `fork` como un remoto, haciendo `fetch` y finalmente el `merge`, si el `merge` es trivial puedes dar click en el boton de `merge` directamente en la pagina de GitHub, lo que hara un `non-fast-forward merge` lo que creara un historial de todos los `commits`, en cambio si no quieres aceptar el `pull request` puedes simplimente cerrarlo y el dueño sera notificado.

### Pull request refs
Si estas lidiando con muchos `pull rquests` y no quieres añadir un monton de remotos o hacer un `pull` para cada vez, hay un truco que te permite hacer GitHub, es algo avanzado, asi que no veremos los detalles pero como es util lo veremos, GitHub guarda los `pull request` como pseudo-ramas en el servidor, por default no las puedes ver, pero estan ahi ocultas, y puedes acceder de manera sencilla.
Para demostrar esto, se usara un comando de bajo nivel comunmente llamado `plumbing` el cual es `ls-remote`, este comando no se suele usar en el dia a dia pero es util para ver las referencias usadas en el repositorio, si lo corremos podremos obtener una lista de las ramas y las etiquetas junto con otras referencias del repositorio, por ejemplo
```
    $ git ls-remote https://github.com/schacon/blink
    10d539600d86723087810ec636870a504f4fee4d	HEAD
    10d539600d86723087810ec636870a504f4fee4d	refs/heads/master
    6a83107c62950be9453aac297bb0193fd743cd6e	refs/pull/1/head
    afe83c2d1a70674c9505cc1d8b7d380d5e076ed3	refs/pull/1/merge
    3c8d735ee16296c242be7a9742ebfbc2665adec1	refs/pull/2/head
    15c9f4f80973a2758462ab2066b6ad9fe8dcf03d	refs/pull/2/merge
    a5a7751a33b7e86c5e9bb07b26001bb17d775d1a	refs/pull/4/head
    31a45fc257e8433c8d8804e3e848cf61c9d3166c	refs/pull/4/merge
```
En un repositorio con `pull request` estos tendran el prefijo `refs/pull/`, estas son ramas tecnicamente pero como no estan bajo `refs/heads/`, no las obtendras de manera por defecto si haces un `fetch` o clonas el servidor, hay dos referencias por `pull request` la que termina en `/head` apunta al ultimo `commit` en la rama del `pull request` eso significa que si alguien abre un `pull request` desde un `fork` y crea una nueva rama, esa rama no existira en tu servidor, pero si la referencia que apunta al archivo que desea cambiar, asi que puedes hacer `pull` a todos estos archivos de todos los `pull request` del servidor.
Para poder hacer `fetch` de solo una referencia se hace de la siguiente manera
```
    $ git fetch origin refs/pull/958/head
    From https://github.com/libgit2/libgit2
    * branch            refs/pull/958/head -> FETCH_HEAD
```
Esto le dice a Git que conecte al `origin` remoto, y que descarge la referencia llamada `refs/pull/958/head` y coloca esto bajo `.git/FETCH_HEAD` puedes seguir esto con un `git merge FETCH_HEAD` a una rama en la que quieras probar, ese mensaje es un poco raro, ademas de que si tienes muchos `pull rquest` esto se vuelve muy tedisos.
Tambien hay una manera para hacer `fetch` a todos de una sola vez, y mantenerlos al dia cada vez que se conecte, abriendo `.git/config` en el editor de tu elección y buscas por `origin` en el remoto deberia verse algo asi
```
    [remote "origin"]
        url = https://github.com/libgit2/libgit2
        fetch = +refs/heads/*:refs/remotes/origin/*
```
la linea que inicia con `fetch =` es una `refspec` es una manera de mapear nombres en el remoto con nombre de tu `.git` local, esto basicamente se refiere a que esta bajo `refs/heads` y deberia ir a mi local bajo `refs/remotes/origin`, puedes modificar esta sección para agregar otro `refspec` 
```
    [remote "origin"]
        url = https://github.com/libgit2/libgit2.git
        fetch = +refs/heads/*:refs/remotes/origin/*
        fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
```
Eso ultimo le dice a Git que todas las referencias que se vean como `refs/pull/123/head` y se deben guardar en `refs/remotes/origin/pr/123`, ahora si guardas este archivo y realizas un `git fetch`
```
    $ git fetch
    # …
    * [new ref]         refs/pull/1/head -> origin/pr/1
    * [new ref]         refs/pull/2/head -> origin/pr/2
    * [new ref]         refs/pull/4/head -> origin/pr/4
    # …
```

### Pull request en los pull request
No solo puedes hacer `pull request` sobre la rama `master` si no tambien sobro otras o sobre los mismo `pull request`, si ves que un `pull request` va en buen camino y tienes una idea para un cambio pero no sabes si es buena idea o no tienes acceso a hacer `push` puedes abrir un `pull request` sobre este.
Cuando abres un `pull request` un la parte de arriba hay una caja que especifica sobre cual rama lo estas haciendo, si haces click en el boton de editar en la sección inferior no solo podras cambiar ramas si no tambien `forks`.

### Menciones y Notificaciones
GitHub tambien tiene un bonito sistema de notificaciones por si mismo que viene a la mano cuando tienes preguntas o necesitas `feedback` de un individuo o un equipo, en cualquier comentario, si escribes el caracter @ comenzara a autocompletar con los usuarios de personas que son contribuidores o colaboradores del proyecto.
Una vez publiques un comentario citando a un usuario este recibira una notificación, de esta manera puede hacer que alguna persona entre en la conversación de manera rapida y efectiva.
Cuando una persona es mencionada en un proyecto esta queda "Suscrita" a este, lo que significa que recibira notificaciones cada que ocurra algo, pero si deseas dejar de recibir notificaciones hay un boton de `unsuscribe` el cual puedes usar para dejar de recibir notificaciones.

#### Pagina de notificaciones
GitHub tiene varias configuraciones posibles para mandarte las notiicaciones, para acceder a estas entramos en ajustes y en centro de notificaciones, dos opciones aparecen en este, correo y web.

##### Notificaciones Web
Las notificaciones web solo existen en GitHub y solo puedes revisarlas ahi, en esta sección puedes configurar cuando quieres que se mande una notificación y cuando no, en la parte superior derecha aparecera un icono de color azul en donde podras encontrar todas las notificaciones que tienes, puedes filtrarlas por proyecto, tambien puedes silenciarlas con esta misma herramienta.

##### Notificaciones por correo
Las notificaciones por correo son otra manera por la cual GitHub te puede informar de los cambios en los proyectos, Es importante saber que hay mucha información en los meta datos de los encabezados de los correos que puede servir para filtrar y personalizar las reglas.
Para empezar veremos un encabezado de un email y la información que contiene
```
    To: tonychacon/fade <fade@noreply.github.com>
    Message-ID: <tonychacon/fade/pull/1@github.com>
    Subject: [fade] Wait longer to see the dimming effect better (#1)
    X-GitHub-Recipient: tonychacon
    List-ID: tonychacon/fade <fade.tonychacon.github.com>
    List-Archive: https://github.com/tonychacon/fade
    List-Post: <mailto:reply+i-4XXX@reply.github.com>
    List-Unsubscribe: <mailto:unsub+i-XXX@reply.github.com>,...
    X-GitHub-Recipient-Address: tchacon@example.com
```
Para empezar, `Message-ID` muestra la información en el formato `<user>/<project>/<type>/<id>`, `List-Post` y `List-Unsubscribe` estos muestran a los que les llegan las notificaciones, y tambien puedes dessuscribirse del hilo lo que seria como hacer click en el boton de `unsubscribe` tambien es importante denotar que si tienes ambas opciones, correo y web, y abres el correo la versión web tambien se marcara como vista

## Archivos especiales
Hay unos archivos especiales que son importantes de revisar

### README
Lo primero es que el archivo `README` puede estar en cualquier formato que GitHub reconozca como texto, es decir `README.md`, `README.asciidoc`, etc.
Si GitHub ve un `README` en tu proyecto, este sera el archivo donde se abrira tu proyecto.
Muchas veces este se utiliza para guardar información importante del proyecto como

- Para que es el proyecto
- Como configurarlo e instalarlo
- Un ejemplo de como usarlo o como hacer que funcione
- Las licensas sobre las cuales el proyecto esta
- Como contribuir


### Contributing
El otro archivo especial es el `Contributing` con cualquier extensión, GitHub mostrara un acceso directo a este archivo cuando alguien quiera hacer un `pull request` en tu repositorio, esto es para que puedas especifiar cualquier cosa que quieras o no en un `pull request`

## Administración de proyecto
Generalmente no hay mucho que hacer en terminos administrativos, pero hay unos cuantos que ver

### Cambiando la rama default
Si estas usando una rama diferente a `master` como tu rama principal, y quieres que los demas tambien lo hagan, en la sección de opciones en la parte de abajo, simplemente puede cambiar la rama default a cualquiera que exista en el repositorio.

### Transfiriendo un proyeto
Si quieres transferir un proyecto a otro usuario u organización en GitHub, en la `Danger Zone` hay una opción de transferir propiedad, esto es util si vas a abandonar el proyecto y alguien más lo va a llevar, o si se esta volviendo grande y lo quieres mover a una organización.
Esto movera el dominio y movera todo con este, hasta los clones de git.

[Anterior](Ch6.2.md)
[Siguiente](Ch6.4.md)
[Indice](Ch6/Indice.md)