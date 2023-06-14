# Control de Versiones
## ¿Qué es y para que sirve?
El control de versiones es un sistema que registra los cambios en un archivo o conjunto de archivos a lo largo del tiempo para que puedas recuperar versiones específicas más adelante.Te permite revertir archivos seleccionados a un estado anterior, revertir todo el proyecto a un estado anterior, comparar cambios a lo largo del tiempo, ver quién modificó por última vez algo que podría estar causando un problema, quién introdujo un problema y cuándo, y más.
## Versión Local (VCS)
Una de las forma más basica para hacer un control de versiones es copiar los archivos a otro directorio, esto sin embargo es ineficiente y propenso a causar errores, por eso los desarrolladores, diseñaron herramientas para poder manter un control de manera más sencilla y ordenanda, un ejemplo es RCS que funciona manteniendo conjuntos de parches (es decir, las diferencias entre los archivos) en un formato especial en el disco; luego puede recrear cómo se veía cualquier archivo en cualquier momento sumando todos los parches.
## Versión Centralizada (CVCS)
Para poder colaborar con desarrolladores en otros sistemas se desarrollaron los Sistemas de Control de Versiones Centralizados, estos tienen un único servidor que contiene todos los archivos versionados y varios clientes que obtienen los archivos de ese lugar central. Esta configuración ofrece muchas ventajas, especialmente en comparación con los VCS locales. Por ejemplo, todos tienen cierto grado de conocimiento sobre lo que los demás en el proyecto están haciendo. Los administradores tienen un control detallado sobre quién puede hacer qué, sin embargo, esta configuración también tiene algunas desventajas serias. La más obvia es el único punto de falla que representa el servidor centralizado. Si ese servidor se cae durante una hora, durante ese tiempo nadie puede colaborar en absoluto ni guardar cambios versionados en lo que están trabajando.
## Versión Distribuida (DVCS)
En los Sistemas de Control de Versiones Distribuidos, los clientes no solo obtienen la última instantánea de los archivos, sino que reflejan completamente el repositorio, incluyendo su historial completo. Por lo tanto, si un servidor falla y estos sistemas estaban colaborando a través de ese servidor, cualquier repositorio de cliente puede ser copiado de vuelta al servidor para restaurarlo. Cada clon es realmente una copia de seguridad completa de todos los datos. muchos de estos sistemas manejan bastante bien la posibilidad de tener varios repositorios remotos con los que pueden trabajar, lo que te permite colaborar con diferentes grupos de personas de diferentes maneras simultáneamente dentro del mismo proyecto.