# Configuración inicial 
Git viene con una herramienta llamada `git config` que te permite obtener y establecer variables de configuración que controlan todos los aspectos de cómo se ve y funciona Git. Estas variables se pueden almacenar en tres lugares diferentes:

Archivo `[ruta]/etc/gitconfig`: Contiene valores aplicados a todos los usuarios del sistema y todos sus repositorios. Si pasas la opción `--system a git config`, leerá y escribirá específicamente en este archivo. Dado que este es un archivo de configuración del sistema, necesitarías privilegios de administrador o superusuario para realizar cambios en él.

Archivo `~/.gitconfig` o `~/.config/git/config`: Valores específicos personalmente para ti, el usuario. Puedes hacer que Git lea y escriba en este archivo específicamente pasando la opción `--global`, y esto afecta a todos los repositorios con los que trabajas en tu sistema.

`config` en el directorio Git (es decir, `.git/config`) del repositorio que estás utilizando actualmente: Específico para ese único repositorio. Puedes obligar a Git a leer y escribir en este archivo con la opción `--local`, pero eso, de hecho, es el valor predeterminado. Como era de esperar, debes estar ubicado en algún lugar de un repositorio de Git para que esta opción funcione correctamente.

Cada nivel anula los valores en el nivel anterior, por lo que los valores en `.git/config` tienen más prioridad que los de `[ruta]/etc/gitconfig`.

En sistemas Windows, Git busca el archivo `.gitconfig` en el directorio `$HOME` (`C:\Users$USER` para la mayoría de las personas). También busca `[ruta]/etc/gitconfig`, aunque es relativo a la raíz de MSys, que es donde decidas instalar Git en tu sistema Windows cuando ejecutas el instalador. Si estás utilizando la versión 2.x o posterior de Git para Windows, también hay un archivo de configuración a nivel del sistema en `C:\Documents and Settings\All Users\Application Data\Git\config` en Windows XP, y en `C:\ProgramData\Git\config` en Windows Vista y versiones posteriores. Este archivo de configuración solo se puede cambiar mediante `git config -f <archivo>` como administrador.

Puedes ver todas tus configuraciones y de dónde provienen usando:
```
    $ git config --list --show-origin
```
## Identidad
Lo primero que debes hacer cuando instalas Git es establecer tu nombre de usuario y dirección de correo electrónico, esto porque se usa esa información para identifiacar commits.
```
    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com
```
Nuevamente, solo necesitas hacer esto una vez si pasas la opción --global, porque Git siempre usará esa información para todo lo que hagas en ese sistema. Si deseas anular esto con un nombre o dirección de correo electrónico diferente para proyectos específicos, puedes ejecutar el comando sin la opción --global cuando estés en ese proyecto.
Muchas de las herramientas gráficas te ayudarán a hacer esto cuando las ejecutes por primera vez.
## Tu editor
Ahora que tu identidad está configurada, puedes configurar el editor de texto predeterminado que se utilizará cuando Git necesite que escribas un mensaje. Si no está configurado, Git utiliza el editor de texto predeterminado de tu sistema.
si quieres usar un editor diferente debes usar:
```
    $ git config --global core.editor emacs
```
En un sistema Windows, si deseas utilizar un editor de texto diferente, debes especificar la ruta completa del archivo ejecutable.
En el caso de Notepad++, un popular editor de programación, es probable que desees utilizar la versión de 32 bits, ya que en el momento de escribir esto, la versión de 64 bits no admite todos los complementos.
Se agrega asi:
```
    $ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```
## Nombre predeterminado de las Branch
Por defecto, Git creará una rama llamada master cuando creas un nuevo repositorio con git init. A partir de la versión 2.28 de Git, puedes establecer un nombre diferente para la rama inicial.

Para establecer main como el nombre predeterminado de la rama, ejecuta:
```
$ git config --global init.defaultBranch main
```
## Verificando las configuraciones
Si deseas verificar tus configuraciones, puedes utilizar el comando git config --list para listar todas las configuraciones que Git puede encontrar en ese momento:
```
    $ git config --list
    user.name=John Doe
    user.email=johndoe@example.com
    color.status=auto
    color.branch=auto
    color.interactive=auto
    color.diff=auto
    ...
```
También puedes verificar cuál es el valor que Git asigna a una clave específica escribiendo `git config <key>`:
```
    $ git config user.name
    John Doe
```

[Anterior](Ch1.5.md)
[Siguiente](Ch1.7.md)
[Indice](Ch1/Indice.md)