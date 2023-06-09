# ¿Qué es git?
Se marcaran las caracteristicas de git y las diferencias con otros sistemas
## Fotos, no diferencias
La principal diferencia entre Git y cualquier otro sistema de control de versiones es la forma en que Git piensa en sus datos. Conceptualmente, la mayoría de los otros sistemas almacenan información como una lista de cambios basados en archivos. Estos otros sistemas consideran la información que almacenan como un conjunto de archivos y los cambios realizados en cada archivo a lo largo del tiempo (esto se describe comúnmente como control de versiones basado en diferencias). En cambio, Git piensa en sus datos más como una serie de fotos de un sistema de archivos en miniatura. Con Git, cada vez que haces un commit o guardas el estado de tu proyecto, Git básicamente toma una foto de cómo lucen todos tus archivos en ese momento y almacena una referencia a esa foto. Para ser eficiente, si los archivos no han cambiado, Git no almacena el archivo nuevamente, solo un enlace al archivo idéntico anterior que ya ha almacenado. Git piensa en sus datos más como un flujo de fotos. 
## Casi todas las operaciones son locales
La mayoría de las operaciones en Git solo necesitan archivos y recursos locales para funcionar, generalmente no se necesita información de otra computadora en tu red. Esto también significa que hay muy pocas cosas que no puedas hacer si estás desconectado o fuera de la red VPN. Si te subes a un avión o un tren y quieres hacer un poco de trabajo, puedes hacer commits felizmente hasta que encuentres una conexión de red para subir los cambios.
## Git tiene integridad
Todo en Git se somete a una suma de verificación (checksum) antes de ser almacenado y se referencia a través de esa suma de verificación. Esto significa que es imposible cambiar el contenido de cualquier archivo o directorio sin que Git lo note, No puedes perder información en tránsito o sufrir corrupción de archivos sin que Git pueda detectarlo. El mecanismo que Git utiliza para esta suma de verificación se llama hash SHA-1. Es una cadena de 40 caracteres compuesta por caracteres hexadecimales (0-9 y a-f) y se calcula en función del contenido de un archivo o estructura de directorio en Git.
## Git generalmente solo agrega datos
Cuando realizas acciones en Git, casi todas ellas solo agregan datos a la base de datos de Git. Es difícil hacer que el sistema haga algo que no se pueda deshacer o que borre datos de alguna manera. Como con cualquier sistema de control de versiones, puedes perder o arruinar cambios que aún no has confirmado, pero una vez que haces un commit de una instantánea en Git, es muy difícil perderla, especialmente si regularmente sincronizas tu base de datos con otro repositorio. Esto hace que usar Git sea un placer porque sabemos que podemos experimentar sin el peligro de arruinar las cosas gravemente.
## Los tres estados
Git tiene tres estados principales en los que tus archivos pueden encontrarse: modificados, preparados (staged) y confirmados (committed):
-Modificado: significa que has cambiado el archivo pero aún no lo has confirmado en tu base de datos.
-Preparado: (staged) significa que has marcado un archivo modificado en su versión actual para que se incluya en tu próxima instantánea de confirmación (commit).
-Confirmado: (committed) significa que los datos están almacenados de forma segura en tu base de datos local.
Esto nos lleva a las tres secciones principales de un proyecto Git: el árbol de trabajo (working tree), el área de preparación (staging area) y el directorio de Git.
-El árbol de trabajo es una sola copia de una versión del proyecto. Estos archivos se extraen de la base de datos comprimida en el directorio de Git y se colocan en el disco para que los utilices o modifiques.
-El área de preparación es un archivo, generalmente contenido en tu directorio de Git, que almacena información sobre qué se incluirá en tu próxima confirmación. Su nombre técnico en la jerga de Git es "índice" (index), pero la expresión "área de preparación" funciona igual de bien.
-El directorio de Git es donde Git almacena los metadatos y la base de datos de objetos de tu proyecto. Esta es la parte más importante de Git y es lo que se copia cuando clonas un repositorio desde otra computadora.
El flujo de trabajo básico de Git es algo así:
-Modificas archivos en tu árbol de trabajo.
-Seleccionas de forma selectiva solo esos cambios que deseas incluir en tu próxima confirmación, lo cual agrega solo esos cambios al área de preparación.
-Realizas una confirmación (commit), que toma los archivos tal como están en el área de preparación y almacena esa instantánea de forma permanente en tu directorio de Git.

[Anterior](Ch1.2.md)
[Siguiente](Ch1.4.md)
[Indice](https://github.com/IIKUYY/Git-basico/blob/main/Ch1/README.md)