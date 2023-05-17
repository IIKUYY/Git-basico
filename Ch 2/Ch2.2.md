# Registrar Cambios en el repositorio
Recuerda que cada archivo en tu directorio de trabajo puede estar en uno de dos estados: rastreado o no rastreado. Los archivos rastreados son aquellos que estaban en la última instantánea, así como cualquier archivo nuevo que hayas preparado para el commit; pueden estar sin modificar, modificados o preparados. En resumen, los archivos rastreados son aquellos que Git conoce. Los archivos no rastreados son todo lo demás: cualquier archivo en tu directorio de trabajo que no esté en la última instantánea y no esté en tu área de preparación. Cuando clonas un repositorio por primera vez, todos tus archivos serán rastreados y sin modificaciones, ya que Git los acaba de descargar y no has editado nada. A medida que editas archivos, Git los considera modificados, ya que los has cambiado desde tu último commit. A medida que trabajas, seleccionas selectivamente los archivos modificados y luego haces commit de todos esos cambios preparados, y el ciclo se repite.
## Revisar el estado de los archivos
La principal herramienta que utilizas para determinar qué archivos se encuentran en qué estado es el comando `git status`. Si ejecutas este comando justo después de clonar, deberías ver algo como esto:
```
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    nothing to commit, working tree clean
```
Esto significa que tienes un directorio de trabajo limpio; en otras palabras, ninguno de tus archivos rastreados está modificado. Git tampoco detecta ningún archivo no rastreado, de lo contrario, se listarían aquí. Por último, el comando te indica en qué rama te encuentras y te informa que no ha divergido de la misma rama en el servidor.
Digamos que agregas un nuevo archivo a tu proyecto, un simple archivo README. Si el archivo no existía antes y ejecutas `git status`, verás tu archivo no rastreado de la siguiente manera:
```
    $ echo 'My Project' > README
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Untracked files:
    (use "git add <file>..." to include in what will be committed)

        README

    nothing added to commit but untracked files present (use "git add" to track)
```
Puedes ver que tu nuevo archivo README está sin rastrear porque se encuentra bajo el encabezado "Untracked files" en la salida del estado. "Untracked" significa básicamente que Git ve un archivo que no estaba presente en la instantánea anterior (commit) y que aún no ha sido preparado (staged); Git no comenzará a incluirlo en tus instantáneas de commit hasta que le indiques explícitamente que lo haga, en este caso, quieres comenzar a incluir el README, así que comencemos a rastrear el archivo.
## Rastreando archivos
Para comenzar a rastrear un nuevo archivo, se utiliza el comando `git add`. Para comenzar a rastrear el archivo README, puedes ejecutar esto:
```
    $ git add README
```
Si ejecutas nuevamente el comando de estado (status), podrás ver que tu archivo README ahora está siendo rastreado y preparado para ser confirmado (staged):
```
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)

        new file:   README
```
Puedes ver que está preparado para ser confirmado porque está bajo el encabezado "Cambios a ser confirmados". Si confirmas en este punto, la versión del archivo en el momento en que ejecutaste `git add` será la que se incluirá en el siguiente histórico. Puede que recuerdes que cuando ejecutaste `git init` anteriormente, luego ejecutaste `git add <archivos>` para comenzar a rastrear archivos en tu directorio. El comando `git add` toma el nombre de ruta de un archivo o directorio; si es un directorio, el comando añade todos los archivos en ese directorio de forma recursiva.
## Almacenar archivos modificados
Cambiemos un archivo que ya estaba rastreado. Si cambias un archivo previamente rastreado llamado `CONTRIBUTING.md` y luego ejecutas el comando `git status` nuevamente, obtienes algo que se ve así:
```
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        new file:   README

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
```
El archivo CONTRIBUTING.md aparece en la sección llamada "Cambios no preparados para confirmar", lo que significa que un archivo que está siendo rastreado ha sido modificado en el directorio de trabajo pero aún no ha sido preparado. Para prepararlo, se ejecuta el comando `git add`. `git add` es un comando versátil que se utiliza para comenzar a rastrear archivos nuevos, preparar archivos y realizar otras acciones, como marcar archivos en conflicto de fusión como resueltos. Puede ser útil pensar en él más como "agregar precisamente este contenido para el próximo commit" en lugar de "agregar este archivo al proyecto". Ejecutemos ahora git add para preparar el archivo `CONTRIBUTING.md` y luego volvamos a ejecutar `git status`:
```
    $ git add CONTRIBUTING.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        new file:   README
        modified:   CONTRIBUTING.md
```
Ambos archivos están preparados y se incluirán en tu próximo commit. En este punto, supongamos que recuerdas un pequeño cambio que deseas hacer en `CONTRIBUTING.md` antes de confirmarlo. Lo abres nuevamente, realizas ese cambio y estás listo para confirmar. Sin embargo, ejecutemos `git status` una vez más:
```
    $ vim CONTRIBUTING.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        new file:   README
        modified:   CONTRIBUTING.md

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
```
Ahora `CONTRIBUTING.md` aparece tanto como preparado (staged) como no preparado (unstaged). ¿Cómo es posible eso? Resulta que Git prepara un archivo exactamente tal como está cuando ejecutas el comando `git add`. Si confirmas ahora, la versión de `CONTRIBUTING.md` tal como estaba cuando ejecutaste por última vez el comando `git add` es la que se incluirá en el commit, no la versión del archivo tal como se ve en tu directorio de trabajo cuando ejecutas `git commit`. Si modificas un archivo después de ejecutar `git add`, debes ejecutar `git add` nuevamente para preparar la última versión del archivo:
```
    $ git add CONTRIBUTING.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        new file:   README
        modified:   CONTRIBUTING.md
```
## Estatus Corto
Si bien la salida del comando git status es bastante completa, también es bastante extensa. Git tiene una bandera de estado corta para que puedas ver tus cambios de una manera más compacta. Si ejecutas `git status -s` o `git status --short`, obtendrás una salida mucho más simplificada del comando:
```
    $ git status -s
    M README
    MM Rakefile
    A  lib/git.rb
    M  lib/simplegit.rb
    ?? LICENSE.txt
```
Los archivos nuevos que no están siendo rastreados tienen `"??"`, los archivos nuevos que han sido agregados a la zona de preparación tienen `"A"`, los archivos modificados tienen `"M"`, y así sucesivamente. Hay dos columnas en la salida: la columna de la izquierda indica el estado de la zona de preparación, y la columna de la derecha indica el estado del árbol de trabajo. Por ejemplo, en esa salida, el archivo `README` está modificado en el directorio de trabajo pero aún no está preparado, mientras que el archivo `lib/simplegit.rb` está modificado y preparado. El archivo `Rakefile` fue modificado, preparado y luego modificado nuevamente, por lo que tiene cambios tanto preparados como no preparados.
## Ignorar archivos
A menudo, tendrás una clase de archivos que no deseas que Git agregue automáticamente o incluso te muestre como no rastreados. Estos suelen ser archivos generados automáticamente, como archivos de registro o archivos producidos por tu sistema de compilación. En estos casos, puedes crear un archivo llamado `.gitignore` que contenga patrones para identificarlos. Aquí tienes un ejemplo de archivo `.gitignore`:
```
    $ cat .gitignore
    *.[oa]
    *~
```
La primera línea indica a Git que ignore cualquier archivo que termine en ".o" o ".a", que son archivos objeto y archivos de biblioteca que pueden ser el resultado de compilar tu código. La segunda línea indica a Git que ignore todos los archivos cuyos nombres terminen con una tilde (`~`), que es utilizada por muchos editores de texto como Emacs para marcar archivos temporales. También puedes incluir un directorio de registro (log), tmp o pid, documentación generada automáticamente, y así sucesivamente. Configurar un archivo `.gitignore` para tu nuevo repositorio antes de comenzar es generalmente una buena idea para evitar comprometer accidentalmente archivos que realmente no deseas en tu repositorio de Git.
Las reglas para los patrones que puedes colocar en el archivo `.gitignore` son las siguientes:
- Las líneas en blanco o las líneas que comienzan con `#` se ignoran.
- Se pueden utilizar patrones glob estándar, que se aplicarán recursivamente en todo el árbol de trabajo.
- Puedes comenzar los patrones con una barra diagonal (`/`) para evitar la recursividad.
- Puedes terminar los patrones con una barra diagonal (`/`) para especificar un directorio.
- Puedes negar un patrón al comenzarlo con un signo de exclamación (`!`).

Los patrones glob son similares a expresiones regulares simplificadas que utilizan las shells. Un asterisco (`*`) coincide con cero o más caracteres; `[abc]` coincide con cualquier carácter dentro de los corchetes (en este caso a, b o c); un signo de interrogación (`?`) coincide con un solo carácter; y los corchetes que encierran caracteres separados por un guión (`[0-9]`) coinciden con cualquier carácter entre ellos (en este caso del 0 al 9). También puedes usar dos asteriscos para coincidir con directorios anidados; `a/**/z` coincidiría con `a/z`, `a/b/z`, `a/b/c/z`, y así sucesivamente.

Aquí tienes otro ejemplo de archivo `.gitignore`:
```
    # ignore all .a files
    *.a

    # but do track lib.a, even though you're ignoring .a files above
    !lib.a

    # only ignore the TODO file in the current directory, not subdir/TODO
    /TODO

    # ignore all files in any directory named build
    build/

    # ignore doc/notes.txt, but not doc/server/arch.txt
    doc/*.txt

    # ignore all .pdf files in the doc/ directory and any of its subdirectories
    doc/**/*.pdf
```
## Ver cambios alamcenados y no almacenados
Si el comando `git status` es demasiado vago para ti, es decir, quieres saber exactamente qué cambiaste y no solo qué archivos cambiaron, puedes usar el comando `git diff`, lo utilizarás con mayor frecuencia para responder a estas dos preguntas: ¿Qué has cambiado pero aún no has preparado? ¿Y qué has preparado y estás a punto de confirmar? Aunque `git status` responde a esas preguntas de manera bastante general al listar los nombres de los archivos, `git diff` te muestra las líneas exactas que se agregaron y eliminaron, es decir, el parche. Digamos que editas y preparas nuevamente el archivo `README` y luego editas el archivo `CONTRIBUTING.md` sin prepararlo. Si ejecutas el comando `git status`, verás algo similar a esto nuevamente:
```
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        modified:   README

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
```
Usando `git diff` solo se muestra lo cambiado pero no lo almacenado
```
    $ git diff
    diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
    index 8ebb991..643e24f 100644
    --- a/CONTRIBUTING.md
    +++ b/CONTRIBUTING.md
    @@ -65,7 +65,8 @@ branch directly, things can get messy.
    Please include a nice description of your changes when you submit your PR;
    if we have to read the whole diff to figure out why you're contributing
    in the first place, you're less likely to get feedback and have your change
    -merged in.
    +merged in. Also, split your changes into comprehensive chunks if your patch is
    +longer than a dozen lines.

    If you are starting to work on a particular area, feel free to submit a PR
    that highlights your work in progress (and note in the PR title that it's
```
Si quieres ver lo que has almacenado que entrara en tu proximo commit puedes usar `git diff --staged`. Este comando compara tus cambios preparados con tu último commit:
```
    $ git diff --staged
    diff --git a/README b/README
    new file mode 100644
    index 0000000..03902a1
    --- /dev/null
    +++ b/README
    @@ -0,0 +1 @@
    +My Project
```
Es importante tener en cuenta que `git diff` por sí solo no muestra todos los cambios realizados desde tu último commit, solo muestra los cambios que aún no se han preparado. Si has preparado todos tus cambios, `git diff` no mostrará ninguna salida, ademas de que `--staged` y `--cached` son sinónimos
## "Commiting"" los cambios
Ahora que tienes tu área de almacenamiento (staging area) configurada como deseas, puedes confirmar tus cambios. Recuerda que cualquier cosa que aún esté sin preparar, es decir, cualquier archivo que hayas creado o modificado pero no hayas ejecutado `git add` desde que los editaste, no se incluirá en este commit. Permanecerán como archivos modificados en tu disco. En este caso, supongamos que la última vez que ejecutaste `git status`, viste que todo estaba preparado, por lo que estás listo para confirmar tus cambios. La forma más sencilla de confirmar es escribir `git commit`:
```
    $ git commit
```
El editor muestra el siguiente mensaje:
```
    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    # On branch master
    # Your branch is up-to-date with 'origin/master'.
    #
    # Changes to be committed:
    #	new file:   README
    #	modified:   CONTRIBUTING.md
    #
    ~
    ~
    ~
    ".git/COMMIT_EDITMSG" 9L, 283C
```
Cuando salgas del editor, Git creará tu commit con ese mensaje de commit (sin los comentarios y el diff). Alternativamente, puedes escribir tu mensaje de commit en línea con el comando de commit especificándolo después de la bandera `-m`, de la siguiente manera:
```
    $ git commit -m "Story 182: fix benchmarks for speed"
    [master 463dc4f] Story 182: fix benchmarks for speed
    2 files changed, 2 insertions(+)
    create mode 100644 README
```
Puedes ver que el commit te ha proporcionado información sobre sí mismo: en qué rama has realizado el commit (master), qué checksum SHA-1 tiene el commit (463dc4f), cuántos archivos han sido modificados y estadísticas sobre las líneas agregadas y eliminadas en el commit.
## Saltarse el area de almacenamiento
Aunque puede ser increíblemente útil para crear commits exactamente como los deseas, el área de almacenamieno (staging area) a veces es un poco más compleja de lo necesario en tu flujo de trabajo. Si quieres omitir el área de preparación, Git proporciona un atajo sencillo. Agregar la opción `-a` al comando `git commit` hace que Git automáticamente prepare todos los archivos que ya están rastreados antes de realizar el commit, lo que te permite omitir la parte de `git add`:
```
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

    no changes added to commit (use "git add" and/or "git commit -a")
    $ git commit -a -m 'Add new benchmarks'
    [master 83e38c7] Add new benchmarks
    1 file changed, 5 insertions(+), 0 deletions(-)
```
## Remover archivos
Para eliminar un archivo de Git, debes eliminarlo de tus archivos rastreados (más precisamente, eliminarlo de tu área de almacenamiento) y luego confirmar los cambios. El comando `git rm` hace eso y también elimina el archivo de tu directorio de trabajo para que no lo veas como un archivo no rastreado la próxima vez. Si simplemente eliminas el archivo de tu directorio de trabajo, aparecerá en la sección "Changes not staged for commit" (es decir, sin preparar) en la salida de `git status`:
```
    $ rm PROJECTS.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes not staged for commit:
    (use "git add/rm <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            deleted:    PROJECTS.md

    no changes added to commit (use "git add" and/or "git commit -a")
```
Entonces, si ejecutas `git rm`, se prepara la eliminación del archivo:
```
    $ git rm PROJECTS.md
    rm 'PROJECTS.md'
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        deleted:    PROJECTS.md
```
La próxima vez que realices un commit, el archivo desaparecerá y dejará de ser rastreado. Si modificaste el archivo o ya lo habías agregado al área de preparación, debes forzar la eliminación con la opción `-f`. Esta es una medida de seguridad para evitar la eliminación accidental de datos que aún no se han registrado en una instantánea y que no se pueden recuperar de Git. Otra cosa útil que puedes hacer es mantener el archivo en tu árbol de trabajo (working tree) pero eliminarlo del área de preparación. En otras palabras, puedes mantener el archivo en tu disco duro pero hacer que Git deje de rastrearlo. Esto es especialmente útil si olvidaste agregar algo a tu archivo `.gitignore` y lo preparaste accidentalmente, como un archivo de registro grande o un montón de archivos compilados `.a`. Para hacer esto, utiliza la opción `--cached`:
```
    $ git rm --cached README
```
Puedes pasar archivos, directorios y patrones de archivo-glob al comando `git rm`. Esto significa que puedes hacer cosas como:
```
    $ git rm log/\*.log
```
## Mover Archivos
A diferencia de muchos otros sistemas de control de versiones, Git no realiza un seguimiento explícito del movimiento de archivos. Si renombras un archivo en Git, no se almacenan metadatos en Git que indiquen que renombraste el archivo. Sin embargo, Git es bastante inteligente para detectarlo después del hecho; nos ocuparemos de detectar el movimiento de archivos un poco más adelante. Por lo tanto, es un poco confuso que Git tenga un comando "mv". Si deseas cambiar el nombre de un archivo en Git, puedes ejecutar algo como:
```
    $ git mv file_from file_to
```
y funciona bien. De hecho, si ejecutas algo como esto y observas el estado, verás que Git lo considera como un archivo renombrado:
```
    $ git mv README.md README
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README
```
Sin embargo, esto es equivalente a ejecutar algo como esto:
```
    $ mv README.md README
    $ git rm README.md
    $ git add README
```
Git deduce implícitamente que se trata de un cambio de nombre, por lo que no importa si renombras un archivo de esa manera o con el comando mv. La única diferencia real es que git mv es un solo comando en lugar de tres, es una función de conveniencia.
