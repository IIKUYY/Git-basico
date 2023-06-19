# Contribuir a un proyecto

## Forking
Si quieres contribuir a un proyecto al que no tienes derecho de `push` lo que puedes hacer es hacer un `Fork` lo que haces es crear una copia identica del proyecto tuya, donde si puedes hacer `push`
De esta manera no se preocupan por agregar colaboradores, teniendo los cambios puedes hacer un `pull request` donde los contribuidores pueden revisarlos cambios y decidir si aceptarlos, rechazarlos o solicitar correcciones.

## El flujo de GitHub
GitHub esta diseñado para un flujo de trabajo basado en `pull request` donde un equipo de trabajo grande trabaja con un solo repositorio, asi que generalmente se siguen unos pasos, los cuales son:

1. Hacer `Fork` al proyecto
2. Crear un rama topica desde `master`
3. Hacer alguno `commits` que mejore el proyecto
4. Hacer `push` a esta rama al proyecto
5. Abrir un `pull request`
6. Hacer correciones necesarias
7. Hacer `merge` y cerrar el `pull request`
8. Sincronisar el nuevo `master` a tu `Fork`

## Crear un Pull request
Tomemos el caso de Tony, el quiere el codigo para su microcontrolador programable arduino, y lo encontro en `https://github.com/schacon/blink.` El unico problema es que el ratio de `blinking` es demasiado rapido, el piensa que 3 segundos es mucho más adecuado que 1, asi que quiere proponerlo.
Primero hacemos click en el boton de `Fork`, su nombre de usuario es tonychacon, asi que la copia del proyecto se hara en `https://github.com/tonychacon/blink`, le hacermos un `clon` local, crearemos una rama topica, haremos los cambios y haremos `push`
```
    $ git clone https://github.com/tonychacon/blink (1)
    Cloning into 'blink'...

    $ cd blink
    $ git checkout -b slow-blink (2)
    Switched to a new branch 'slow-blink'

    $ sed -i '' 's/1000/3000/' blink.ino (macOS) (3)
    # If you're on a Linux system, do this instead:
    # $ sed -i 's/1000/3000/' blink.ino (3)

    $ git diff --word-diff (4)
    diff --git a/blink.ino b/blink.ino
    index 15b9911..a6cc5a5 100644
    --- a/blink.ino
    +++ b/blink.ino
    @@ -18,7 +18,7 @@ void setup() {
    // the loop routine runs over and over again forever:
    void loop() {
    digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
    [-delay(1000);-]{+delay(3000);+}               // wait for a second
    digitalWrite(led, LOW);    // turn the LED off by making the voltage LOW
    [-delay(1000);-]{+delay(3000);+}               // wait for a second
    }

    $ git commit -a -m 'Change delay to 3 seconds' (5)
    [slow-blink 5ca509d] Change delay to 3 seconds
    1 file changed, 2 insertions(+), 2 deletions(-)

    $ git push origin slow-blink (6)
    Username for 'https://github.com': tonychacon
    Password for 'https://tonychacon@github.com':
    Counting objects: 5, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 340 bytes | 0 bytes/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    To https://github.com/tonychacon/blink
    * [new branch]      slow-blink -> slow-blink
```
en resumen

1. Clonar nuestro `fork` en nuestro local
2. Crear una rama topica descriptiva
3. Hacer los cambios en el codigo
4. Comprabar si el cambio es bueno
5. Hacer `commit` con los cambios de la rama topica
6. Hacer `push` de nuestra rama topica al `fork`

Ahora si regresamos a GitHub, podremos ver una notificación de que hemos hecho `push` de la rama topica y nos presenta un gran boton verde que nos dice que comparemos nuestros resultados y abramos un `pull request` en el proyecto original, de otra manera puedes ir a la pagina de las ramas, buscar tu rama y hacer el `pull request` desde ahi.
Si le damos click al gran boton verde, veremos una pantalla que nos pide que le demos un nombre y descripción al `pull request`, es generalmente importante poner esfuerzo a estos para cominicar bien al dueño los cambios que propones y como mejorarian el proyecto.
Tambien vemos una lista de los `commits` que hicimos que estan `ahead` de `master`.
Cuando crees este `pull request` el dueño del proyecto al que le hiciste `fork` recibira un correo notificandolo de que alguien creo un `pull request`.

## Iterando en un pull request
En este punto, el dueño del proyecto puede revisar tus cambios y aceptarlos, en este caso, le gustaron, pero prefiere que la luz este apagada durante más tiempo que encendida, esta conversación puede pasar por correos o por la pagina de GitHub.
Cualquier persona puede hacer un comentario general en la discución, pero el dueño o los colaboradores pueden señalar lineas de codigo especificas.

Ahora puedes saber que tienes que hacer para que el cambio sea aceptado, con GitHub, solo tienes que realizar los cambios en la rama topica y cuando este lista realizar el `push`, lo que automaticamente actualizara el `pull request`, en este podras ver el codigo que cambiaste anteriormente, agregar `commits` a un `pull request` no crea nuevas notificaciones, asi que puedes dejar un comentario para que el dueño lo vea.

Una cosa interesante para denotar es que si das click en la documentos cambiados de este `pull request` podras ver `unified diff` que es el total de los cambios agregados que se introduciran a `master`, esto es ver `git diff master_<branch>` automaticamente.

Otra cosa que pódras notar es que si tienes los derechos de escritura GitHub mostrara una sección que muestra si se puede hacer un `merge` de manera limpia, si decides hacer el `merge`, Git hara un `non-fast-forward merge` lo que significa que aunque pueda hacer un `fast-forward merge` este no sucedera creando un `merge commit`.

Si deseas puedes hacerlo localmente, siguiendo estas instrucciones, hacer `pull` a la rama y hacer el `merge` con `master` de manera local, para despues hacer un `push` al remoto.

## Pull request avanzados
Ahora que hemos cubierto los basicos de contribuir en un proyecto de GitHub, cubriremos unos trucos importantes para que seas más efectivo

### Pull request como Patches
Es importante entender de que muchos proyectos no ven a los `pull request` como una cola de parches perfectos que deberian aplicarse de manera limpia y en orden, muchos proyectos basados en listas de correos piensan en los parches como series de contribuciones, la mayoria de los proyectos de GitHub ven a los `pull request` como conversaciones iterativas sobre cambios propuestos culminando en un `unified diff` que es `merged`.

Es una importante distinción porque generalmente los cambios son propuestos antes de que el codigo sea perfecto, que es mucho más raro con (contribuciones basadas en series de parches en listas de correo) _no estoy seguro de esta traducción y no me quedo del todo claro a que se refiere, tanto aqui como en el anterior parrafo_. esto permite una conversación con mantenimiento para llegar a una solución conjunta, cuando un `pull request` es creado y se sugieren cambios, la serie de parches no es cambiada, si no `pushed` en un nuevo `commit` sobre el `pull request` moviendo la conversación hacia adelante manteniendo el contexto intacto.

### Mantiendose en el Upstream
Si tu `pull request` se vuelve obsoleto, o no puede ser `merged` de manera limpia vas a querer hacer unos cambios para que mantenimiento pueda hacer `merge` de manera facil, GitHub comprobara esto por ti y y te hara saber en la parte de abajo de cada `pull request` si el `merge` es trivial o no.

si ves algo como `Pull Request does not merge cleanly` necesitas arreglar tu rama para que mantenimiento no tenga que hacer más trabajo.
tienes dos opciones principales para hacer esto, puedes hacer `rebase` de tu rama en la rama a la que quieras hacer el merge (comunmente `master`) o puedes hacer `merge` de la rama objetivo a la tuya.
Muchos desarrolladores prefieren la segunda, lo que importa son el historial y el `merge` final, `rebase` solo te da un historial un poco más limpio, pero a cambio te da un proceso más dificil y es más suceptible a errores.
Si quieres hacer `merge` en la rama objetivo para hacer tu `pull request` `mergeable` tienes que agregar el repositorio original como un nuevo remoto, hacer `fetch` desde este, hacer `merge` de la rama `main` del remoto a tu rama topica, arreglar cualquier problema y finalmente hacer `push` a la misma rama del `pull request`.
Una vez hecho el cambio, GitHub automaticamente volvera a revisar si hace `merge` de manera limpia.
Si encerio quieres hacer un `rebase` debes recordar todos lo inconvenientes que presenta, como el eliminar los `pull request` sobre esa rama y podria afectar a los otros desarrolladores

### Referencias
Podrias preguntarte "¿como puedo referenciar otro `pull request`?" hay varias maneras
La primera que veremos es como hacer una referencia cruzada con otro `pull request`, todos los `pull request` tienen un número unico dentro del proyecto, por ejemplo no puedes tener dos PR#3 o un PR#3 y un Issue#3, si quieres referenciarlos, solo tienes que poner `#<num>` en un comentario, si esta alojado en un `fork` tienes que poner `username#<num>`, si quieres referenciar un `PR` en  otro repositorio tienes que usar `username/repo<num>`.

### Markdown
Es una gran herramienta para hacer una descripción detalla del `PR` o para agregar información adicional y de tener funciones agregadas.

#### Listas de tareas
es una lista con casillas las cuales puedes marcar como completadas, estas se suelen usar para marcar los objetivos planeados en cada `PR`
```
    - [X] Write the code
    - [ ] Write all the tests
    - [ ] Document the code
```
Estas tambien hacen que el la vista de los `PR` se muestre una barra de progreso y un porcentaje

#### Snipets de codigo
Tambien puedes poner pequeños pedazos de codigo, esto sirve para presentar una idea de cambio antes de aplicar directamente el cambio en la rama para agregalo tienes que hacer algo del estilo
```
    /```java
    for(int i=0 ; i < 5 ; i++)
    {
    System.out.println("i is : " + i);
    }
    /```
```

#### Citar
Si estas respondiendo una pequeña parte de un gran comentario puedes citar unas lineas con el caracter />, es tan comun que si seleccionas una porción del texto y tecleas la "r" automaticamente citara ese texto.

#### Emoji
Tambien puedes usar emojis, aunque suene raro es bastante comun verlos en comentarios de GitHub, si estas en la sección de comentarios y colocas : automaticamente se mostrara una autompletar y se mostrara el emoji.

#### Imagenes
Aunque tecnicamente esta no es una función de markdown, ademas de poder escribir el `embed URL` tambien se puede arrastrar una imagen y automaticamente se generara este.

## Mantener tu repositorio publico al dia
Cuando hayas hecho un `fork` de algun repositorio, este existe independientemente del original, pero cuando al anterior se le realicen `commits`, se mostrara este mensaje en tu `fork`
```
    This branch is 5 commits behind progit:master.
```
Pero este no se actualizara automaticamente, pero e facil de hacerlo manualmente.
Hay varias formas, una de ella que no requiere configuración es hacer lo siguiente
```
    $ git checkout master (1)
    $ git pull https://github.com/progit/progit2.git (2)
    $ git push origin master (3)
```
1. Si estas en otra rama regresas a `master`
2. Hacer `fetch` con los cambios de https://github.com/progit/progit2.git y hacer `merge` a `master`
3. Hacer `push` de tu `master` a `origin`

Esto funciona, pero es tedioso usar el URL todas las veces, por lo que puedes hacer esto
```
    $ git remote add progit https://github.com/progit/progit2.git (1)
    $ git fetch progit (2)
    $ git branch --set-upstream-to=progit/master master (3)
    $ git config --local remote.pushDefault origin (4)
```
1. Agregar el repositorio fuente y darle un nombre, en este caso "progit"
2. Obtener una referencia en una de las ramas de progit, en este caso `master`
3. Configurar tu rama `master` para hacer `fetch` desde progit en el remoto
4. Definir el repositorio `push` por defecto a `origin`

una vez hecho esto solo tienes que hacer esto las siguientes veces
```
    $ git checkout master (1)
    $ git pull (2)
    $ git push (3)
```
1. regresar a `master`
2. Hacer `fetch` a progit y hacer `merge` de los cambios a `master`
3. Hacer `push` de tu `master` a `origin`

Este acercamiento es util y más eficiente, pero tiene sus defectos, porque git hara todos estos cambios facilmente pero tendra que tener cuidado de no no hacer `commits` directo a `master` porque efectivamente esta rama es del repositorio original.

[Anterior](Ch6.1.md)
[Siguiente](Ch6.3.md)
[Indice](https://github.com/IIKUYY/Git-basico/blob/main/Ch6/README.md)