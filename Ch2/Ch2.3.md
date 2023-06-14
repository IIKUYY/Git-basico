# Viendo el historial de commits

## Historial de commits
Después de haber creado varios commits, o si has clonado un repositorio con un historial de commits existente, probablemente querrás echar un vistazo atrás para ver qué ha sucedido. La herramienta más básica y poderosa para hacer esto es el comando git log. Estos ejemplos utilizan un proyecto muy simple llamado "simplegit". Para obtener el proyecto, ejecuta:
```
    $ git clone https://github.com/schacon/simplegit-progit
```
Se deberia ver algo asi:
```
    $ git log
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700

        Change version number

    commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Sat Mar 15 16:40:33 2008 -0700

        Remove unnecessary test

    commit a11bef06a3f659402fe7563abf99ad00de2209e6
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Sat Mar 15 10:31:28 2008 -0700

        Initial commit
```
Por defecto, sin argumentos, `git log` muestra los commits realizados en ese repositorio en orden cronológico inverso; es decir, los commits más recientes se muestran primero. Como puedes ver, este comando lista cada commit con su checksum SHA-1, el nombre y correo electrónico del autor, la fecha de escritura y el mensaje del commit. Una de las opciones más útiles es `-p` o `--patch`, que muestra la diferencia (la salida del parche) introducida en cada commit. También puedes limitar el número de entradas de registro mostradas, por ejemplo, usando `-2` para mostrar solo las últimas dos entradas.
```
    $ git log -p -2
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700

        Change version number

    diff --git a/Rakefile b/Rakefile
    index a874b73..8f94139 100644
    --- a/Rakefile
    +++ b/Rakefile
    @@ -5,7 +5,7 @@ require 'rake/gempackagetask'
    spec = Gem::Specification.new do |s|
        s.platform  =   Gem::Platform::RUBY
        s.name      =   "simplegit"
    -    s.version   =   "0.1.0"
    +    s.version   =   "0.1.1"
        s.author    =   "Scott Chacon"
        s.email     =   "schacon@gee-mail.com"
        s.summary   =   "A simple gem for using Git in Ruby code."

    commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Sat Mar 15 16:40:33 2008 -0700

        Remove unnecessary test

    diff --git a/lib/simplegit.rb b/lib/simplegit.rb
    index a0a60ae..47c6340 100644
    --- a/lib/simplegit.rb
    +++ b/lib/simplegit.rb
    @@ -18,8 +18,3 @@ class SimpleGit
        end

    end
    -
    -if $0 == __FILE__
    -  git = SimpleGit.new
    -  puts git.show
    -end
```
También puedes utilizar una serie de opciones de resumen con `git log`. Por ejemplo, si quieres ver estadísticas abreviadas para cada commit, puedes usar la opción `--stat`:
```
    $ git log --stat
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700

        Change version number

    Rakefile | 2 +-
    1 file changed, 1 insertion(+), 1 deletion(-)

    commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Sat Mar 15 16:40:33 2008 -0700

        Remove unnecessary test

    lib/simplegit.rb | 5 -----
    1 file changed, 5 deletions(-)

    commit a11bef06a3f659402fe7563abf99ad00de2209e6
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Sat Mar 15 10:31:28 2008 -0700

        Initial commit

    README           |  6 ++++++
    Rakefile         | 23 +++++++++++++++++++++++
    lib/simplegit.rb | 25 +++++++++++++++++++++++++
    3 files changed, 54 insertions(+)
```
Otra opción realmente útil es `--pretty`. Esta opción cambia el formato de salida del registro a algo distinto al valor predeterminado. Hay algunos valores predefinidos disponibles para que los utilices. El valor oneline para esta opción muestra cada commit en una sola línea, lo cual es útil si estás revisando muchos commits. Además, los valores `short`, `full` y `fuller` muestran la salida en un formato similar, pero con menos o más información, respectivamente:
```
    $ git log --pretty=oneline
    ca82a6dff817ec66f44342007202690a93763949 Change version number
    085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 Remove unnecessary test
    a11bef06a3f659402fe7563abf99ad00de2209e6 Initial commit
```
El valor de opción más interesante es `format`, que te permite especificar tu propio formato de salida del registro. Esto es especialmente útil cuando estás generando salida para su procesamiento automatizado, ya que al especificar el formato de manera explícita, sabes que no cambiará con las actualizaciones de Git:
```
    $ git log --pretty=format:"%h - %an, %ar : %s"
    ca82a6d - Scott Chacon, 6 years ago : Change version number
    085bb3b - Scott Chacon, 6 years ago : Remove unnecessary test
    a11bef0 - Scott Chacon, 6 years ago : Initial commit
```
Aquí tienes algunos de los especificadores más útiles que puedes utilizar con el comando git log --pretty=format:

%H: El hash completo del commit.
%h: El hash abreviado del commit.
%an: El nombre del autor del commit.
%ae: El correo electrónico del autor del commit.
%ad: La fecha de autoría del commit.
%s: El asunto del commit.
Estos son solo algunos ejemplos, y hay muchos más especificadores disponibles. Puedes combinarlos y personalizarlos según tus necesidades para obtener el formato de salida deseado.

### Autor vs Commiter
El autor es la persona que originalmente escribió el trabajo, mientras que el committer es la persona que aplicó por última vez el trabajo. Entonces, si envías un parche a un proyecto y uno de los miembros principales aplica el parche, ambos reciben crédito: tú como autor y el miembro principal como committer.
Los valores de opción `oneline` y `format` son particularmente útiles con otra opción de `log` llamada `--graph`. Esta opción agrega un pequeño gráfico ASCII que muestra tu historial de ramas y fusiones:
```
    $ git log --pretty=format:"%h %s" --graph
    * 2d3acf9 Ignore errors from SIGCHLD on trap
    *  5e3ee11 Merge branch 'master' of https://github.com/dustin/grit.git
    |\
    | * 420eac9 Add method for getting the current branch
    * | 30e367c Timeout code and tests
    * | 5a09431 Add timeout protection to grit
    * | e1193f8 Support for heads with slashes in them
    |/
    * d6016bc Require time for xmlschema
    *  11d191e Merge branch 'defunkt' into local
```
Esas son solo algunas opciones simples de formato de salida para el comando `git log` - hay muchas más. Las opciones comunes para `git log` enumeran las opciones que hemos cubierto hasta ahora, así como algunas otras opciones de formato comunes que pueden ser útiles, junto con cómo cambian la salida del comando `log`.

## Limitar la salida del registro

Además de las opciones de formato de salida, "git log" tiene varias opciones útiles para limitar la cantidad de commits que se muestran. Ya has visto una de esas opciones: la opción "-2", que muestra solo los dos últimos commits. De hecho, puedes usar "-<n>", donde "n" es cualquier número entero, para mostrar los últimos "n" commits. En realidad, es poco probable que uses esto con frecuencia, porque por defecto Git muestra toda la salida a través de un paginador para que solo veas una página de salida del registro a la vez.

Sin embargo, las opciones de limitación de tiempo, como "--since" y "--until", son muy útiles. Por ejemplo, este comando obtiene la lista de commits realizados en las últimas dos semanas:
```
    $ git log --since=2.weeks
```
Este comando funciona con muchos formatos: puedes especificar una fecha específica como `2008-01-15` o una fecha relativa como `hace 2 años, 1 día, 3 minutos`.
También puedes filtrar la lista de commits que coinciden con ciertos criterios de búsqueda. La opción `--author` te permite filtrar por un autor específico, y la opción `--grep` te permite buscar palabras clave en los mensajes de commit.

Otra opción de filtro realmente útil es la opción "-S" (colloquialmente conocida como la opción "pickaxe" de Git), que toma una cadena y muestra solo aquellos commits que cambiaron el número de ocurrencias de esa cadena. Por ejemplo, si quisieras encontrar el último commit que agregó o eliminó una referencia a una función específica, podrías ejecutar el siguiente comando:
```
    $ git log -S function_name
```
La última opción realmente útil que se puede pasar a `git log` como filtro es una ruta. Si especificas un directorio o nombre de archivo, puedes limitar la salida del registro a los commits que introdujeron cambios en esos archivos. Esta opción se coloca siempre al final y generalmente se precede de dos guiones (`--`), para separar las rutas de las opciones. Por ejemplo:
```
    $ git log -- path/to/file
```
Por ejemplo, si quieres ver qué commits modificaron archivos de prueba en el historial del código fuente de Git, fueron realizados por Junio Hamano en el mes de octubre de 2008 y no son commits fusionados, puedes ejecutar algo como esto:
```
    $ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \
    --before="2008-11-01" --no-merges -- t/
    5610e3b - Fix testcase failure when extended attributes are in use
    acd3b9e - Enhance hold_lock_file_for_{update,append}() API
    f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
    d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
    51a94af - Fix "checkout --track -b newbranch" on detached HEAD
    b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn branch
```

Anterior[Ch2/Ch2.2.md]
Siguiente[Ch2/Ch2.4.md]
Indice[README.md]