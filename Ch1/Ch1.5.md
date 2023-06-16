# Instalando Git
Se tiene que instalar previamente para poder usarlo
## Instalar en Linux 
Si quieres instalar Git tools en linux por instalador binario generalmente puedes hacerlo a través de la herramienta de gestión de paquetes que viene con tu distribución. Si estás en Fedora (o cualquier distribución basada en RPM relacionada, como RHEL o CentOS), puedes usar dnf:
```
    $ sudo dnf install git-all
```
O si estas en una distribucción basada en Debian, como Ubuntu se usa apt:
```
    $ sudo apt install git-all
```
Para más opciones estan las intrucciones en la pagina de Git: https://git-scm.com/download/linux.
## Instalar en MacOs
Existen varias formas de instalar Git en macOS. La más fácil probablemente sea instalar las Herramientas de línea de comandos de Xcode. En Mavericks (10.9) o versiones posteriores, puedes hacer esto simplemente intentando ejecutar git desde la Terminal la primera vez.
```
    $ git --version
```
Si buscas una versión más actual, esta el instalador binario en la pagina web de Git: https://git-scm.com/download/mac.
## Instalar en Windows
También hay varias formas de instalar Git en Windows. La versión más oficial está disponible para su descarga en el sitio web de Git. Solo tienes que ir a https://git-scm.com/download/win
Para obtener una instalación automatizada, puedes utilizar el paquete Git Chocolatey. Ten en cuenta que el paquete Chocolatey es mantenido por la comunidad.
## Instalar desde el Código Fuente
Los instaladores binarios tienden a quedarse un poco atrás, aunque a medida que Git ha madurado en los últimos años, esto ha hecho menos diferencia.

Si deseas instalar Git desde el código fuente, debes tener las siguientes bibliotecas en las que Git depende: autotools, curl, zlib, openssl, expat y libiconv, puedes usar uno de estos comandos para instalar las dependencias mínimas para compilar e instalar los binarios de Git:
```
    $ sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel \
    openssl-devel perl-devel zlib-devel
    $ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev \
    gettext libz-dev libssl-dev
```
Para poder agregar la documentación en varios formatos (doc, html, info), se requieren estas dependencias adicionales:
```
    $ sudo dnf install asciidoc xmlto docbook2X
    $ sudo apt-get install asciidoc xmlto docbook2x
```
Si estás utilizando una distribución basada en Debian, también necesitas el paquete install-info:
```
    $ sudo apt-get install install-info
```
Si estás utilizando una distribución basada en RPM, también necesitas el paquete getopt (que ya está instalado en una distribución basada en Debian):
```
    $ sudo dnf install getopt
```
Adiccionalmente si estas usando Fedora necesitas hacer esto:
```
    $ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
```
Una vez que tienes todas las dependencias necesarias, puedes proceder a obtener el último archivo tarball de la versión etiquetada desde varios lugares. Puedes obtenerlo a través del sitio kernel.org, en https://www.kernel.org/pub/software/scm/git, o en el espejo del sitio web de GitHub, en https://github.com/git/git/tags.t
Se compila y se instala
```
    $ tar -zxf git-2.8.0.tar.gz
    $ cd git-2.8.0
    $ make configure
    $ ./configure --prefix=/usr
    $ make all doc info
    $ sudo make install install-doc install-html install-info
```
Si deseas que se instale automaticamente haz
```
    $ git clone https://git.kernel.org/pub/scm/git/git.git
```

[Anterior](Ch1.4.md)
[Siguiente](Ch1.6.md)
[Indice](Ch1/Indice.md)