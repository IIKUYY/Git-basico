# Ramas para principiantes

Para comprender realmente cómo Git realiza las ramificaciones, debemos dar un paso atrás y examinar cómo Git almacena sus datos.
Como recordarás de ¿Qué es Git?, Git no almacena los datos como una serie de conjuntos de cambios o diferencias, sino como una serie de instantáneas.
Cuando haces un commit, Git almacena un objeto de commit que contiene un puntero a la instantánea del contenido que has preparado. Este objeto también contiene el nombre y la dirección de correo electrónico del autor, el mensaje que has escrito y los punteros al commit o commits que precedieron directamente a este commit (su padre o padres): cero padres para el commit inicial, un padre para un commit normal y múltiples padres para un commit que resulta de una fusión de dos o más ramas.
Para visualizar esto, supongamos que tienes un directorio que contiene tres archivos y los preparas todos y haces commit. Preparar los archivos calcula un valor hash (SHA-1) para cada uno, almacena esa versión del archivo en el repositorio de Git (Git los llama blobs) y añade ese valor hash al área de preparación.
```
    $ git add README test.rb LICENSE
    $ git commit -m 'Initial commit'
```
Cuando creas el commit ejecutando `git commit`, Git calcula el valor hash de cada subdirectorio (en este caso, solo el directorio raíz del proyecto) y los almacena como un objeto de árbol en el repositorio de Git. Luego, Git crea un objeto de commit que contiene los metadatos y un puntero al árbol raíz del proyecto para poder recrear esa instantánea cuando sea necesario.
Ahora tu repositorio de Git contiene cinco objetos: tres blobs (cada uno representando el contenido de uno de los tres archivos), un árbol que enumera el contenido del directorio y especifica qué nombres de archivo se almacenan como qué blobs, y un commit con el puntero a ese árbol raíz y todos los metadatos del commit.
Si realizas cambios y haces otro commit, el siguiente commit almacenará un puntero al commit que le precedió inmediatamente.
Una rama en Git es simplemente un puntero ligero y móvil hacia uno de estos commits. El nombre de la rama predeterminada en Git es `master`. A medida que comienzas a hacer commits, se crea una rama llamada master que apunta al último commit que hiciste. Cada vez que haces un commit, el puntero de la rama master se mueve automáticamente hacia adelante.

## Creando una nueva rama

¿Qué sucede cuando creas una nueva rama? Bueno, al hacerlo, se crea un nuevo puntero que puedes mover a tu gusto. Digamos que quieres crear una nueva rama llamada `testing`. Puedes hacerlo con el comando `git branch`:
```
    $ git branch testing
```
Esto crea un nuevo puntero hacia el mismo commit en el que te encuentras actualmente.
¿Cómo sabe Git en qué rama te encuentras actualmente? Git mantiene un puntero especial llamado `HEAD`. Cabe destacar que esto es bastante diferente al concepto de `HEAD` en otros sistemas de control de versiones que puedas haber utilizado, como Subversion o CVS. En Git, `HEAD` es un puntero a la rama local en la que te encuentras actualmente. En este caso, todavía estás en la rama `master`. El comando `git branch` solo creó una nueva rama, pero no cambiaste a esa rama.
Puedes ver fácilmente esto ejecutando un simple comando `git log` que te muestra a dónde apuntan los punteros de las ramas. Esta opción se llama `--decorate`.
```
    $ git log --oneline --decorate
    f30ab (HEAD -> master, testing) Add feature #32 - ability to add new formats to the central interface
    34ac2 Fix bug #1328 - stack overflow under certain conditions
    98ca9 Initial commit
```
Puedes ver las ramas `master` y `testing` que están justo al lado del commit `f30ab`.

## Cambiando de Ramas

Para cambiar a una rama existente, ejecutas el comando `git checkout`. Veamos cómo cambiar a la nueva rama `testing`:
```
    $ git checkout testing
```
Esto mueve `HEAD` para que apunte a la rama `testing`.
Si haces un commit la rama testing avanza para la rama `master` aun apunta al commit anterior a hacer el cambio de rama, pero si vuelves a `master` esto regresa `HEAD` a `master` y regresa los archivos al estado en el que estaban en esta rama, lo que haces que `testing` y `master` vayan en direcciones diferentes
También puedes ver esto fácilmente con el comando `git log`. Si ejecutas `git log --oneline --decorate --graph --all`, se imprimirá el historial de tus commits, mostrando dónde se encuentran los punteros de las ramas y cómo se ha divergido tu historial.
```
    $ git log --oneline --decorate --graph --all
    * c2b9e (HEAD, master) Make other changes
    | * 87ab2 (testing) Make a change
    |/
    * f30ab Add feature #32 - ability to add new formats to the central interface
    * 34ac2 Fix bug #1328 - stack overflow under certain conditions
    * 98ca9 Initial commit of my project
```
Dado que una rama en Git es en realidad un archivo simple que contiene el checksum SHA-1 de 40 caracteres del commit al que apunta, las ramas son fáciles de crear y eliminar. Crear una nueva rama es tan rápido y sencillo como escribir 41 bytes en un archivo (40 caracteres y un salto de línea).

Esto contrasta enormemente con la forma en que la mayoría de las herramientas de control de versiones más antiguas crean ramas, que implica copiar todos los archivos del proyecto en un segundo directorio. Esto puede llevar varios segundos o incluso minutos, dependiendo del tamaño del proyecto, mientras que en Git el proceso siempre es instantáneo. Además, debido a que registramos los padres al hacer commit, encontrar una base adecuada para fusionar (merge base) se hace automáticamente y generalmente es muy fácil de hacer. Estas características ayudan a fomentar que los desarrolladores creen y utilicen ramas con frecuencia.