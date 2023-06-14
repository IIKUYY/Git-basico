# Obtener un Repositorio
Generalmente puedes obtener un repositorio de git
1. Puedes tomar un directorio local que actualmente no está bajo control de versiones y convertirlo en un repositorio de Git
2. Puedes clonar un repositorio de Git existente desde otro lugar.
En ambos casos, obtendrás un repositorio de Git en tu máquina local.
## Iniciar un repositorio en un directorio existente
Si tienes un directorio de proyecto que actualmente no está bajo control de versiones y quieres comenzar a controlarlo con Git, primero debes ir al directorio del proyecto.
Linux
```
    $ cd /home/user/my_project
```
MacOs
```
    $ cd /Users/user/my_project
```
Windows
```
    $ cd C:/Users/user/my_project
```
Type
```
    $ git init
```
Esto crea un nuevo subdirectorio llamado `.git` que contiene todos los archivos necesarios para tu repositorio, es decir, la estructura básica de un repositorio Git. En este punto, aún no se está rastreando nada en tu proyecto. Consulta Git Internals para obtener más información sobre los archivos que se encuentran en el directorio `.git` que acabas de crear.

Si deseas comenzar a controlar versiones de archivos existentes (en lugar de un directorio vacío), probablemente debas comenzar a rastrear esos archivos y hacer un commit inicial. Puedes lograrlo con algunos comandos git add que especifiquen los archivos que deseas rastrear, seguidos de un `git commit`:
```
    $ git add *.c
    $ git add LICENSE
    $ git commit -m 'Initial project version'
```
## Clonar un repositorio existente
Si deseas obtener una copia de un repositorio de Git existente, el comando que necesitas es git clone, en lugar de obtener solo una copia de trabajo, Git recibe una copia completa de casi todos los datos que tiene el servidor. Por defecto, se descarga cada versión de cada archivo en la historia del proyecto cuando ejecutas git clone. De hecho, si el disco del servidor se corrompe, a menudo puedes usar casi cualquiera de los clones en cualquier cliente para restaurar el servidor al estado en el que estaba cuando se clonó Clonas un repositorio con git clone <url>. Por ejemplo, si deseas clonar la biblioteca enlazable de Git llamada libgit2, puedes hacerlo de la siguiente manera:
```
    $ git clone https://github.com/libgit2/libgit2
```
Eso crea un directorio llamado libgit2, inicializa un directorio .git en su interior, descarga todos los datos de ese repositorio y crea una copia de trabajo de la última versión, Si deseas clonar el repositorio en un directorio con un nombre distinto a libgit2, puedes especificar el nuevo nombre del directorio como un argumento adicional:
```
    $ git clone https://github.com/libgit2/libgit2 mylibgit
```
Ese comando hace lo mismo que el anterior, pero el directorio de destino se llama mylibgit.

Git tiene varios protocolos de transferencia diferentes que puedes utilizar. El ejemplo anterior utiliza el protocolo https://, pero también puedes ver git:// o user@server:path/to/repo.git, que utiliza el protocolo de transferencia SSH.

Siguiente[Ch2/Ch2.2.md]
Indice[README.md]