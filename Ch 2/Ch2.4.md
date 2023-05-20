# Deshacer cosas

En cualquier momento, es posible que desees deshacer algo. Ten cuidado, ya que no siempre es posible deshacer algunos de estos cambios. Esta es una de las pocas áreas en Git donde podrías perder trabajo si lo haces mal.

Uno de los deshacer más comunes ocurre cuando haces un commit demasiado pronto y posiblemente olvidas agregar algunos archivos o te equivocas en el mensaje de confirmación. Si deseas rehacer este commit, realiza los cambios adicionales que olvidaste, añádelos al área de preparación y confirma nuevamente usando la opción `--amend`:
```
    $ git commit --amend
```
Al final terminas con una sola confirmación: la segunda confirmación reemplaza los resultados de la primera.

## Deshacer el staging de un archivo preparado
digamos que has cambiado dos archivos y deseas confirmarlos como dos cambios separados, pero accidentalmente escribes `git add` y los agregas al área de preparación. ¿Cómo puedes deshacer el staging de uno de los dos? El comando `git status` te lo recuerda:
```
    $ git add *
    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README
        modified:   CONTRIBUTING.md
```
Justo debajo del texto "Changes to be committed", dice "use git reset HEAD <file>... para deshacer el staging". Entonces, vamos a seguir ese consejo para deshacer el staging del archivo CONTRIBUTING.md:
```
    $ git reset HEAD CONTRIBUTING.md
    Unstaged changes after reset:
    M	CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
```
El archivo es modificado pero no sale del staging

## Deshacer las modificaciones

Haciendo lo que dice `git status` en la sección anterior 
```
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
```
Se ve algo asi 
```
    $ git checkout -- CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README
```
## Deshaciendo cambios con git restore

A partir de la versión 2.23.0 de Git, se introdujo un nuevo comando: `git restore`. Básicamente, es una alternativa a git reset que acabamos de cubrir. A partir de la versión 2.23.0 de Git en adelante, Git utilizará `git restore` en lugar de `git reset` para muchas operaciones de deshacer.

Vamos a retroceder un poco y deshacer cosas utilizando `git restore` en lugar de `git reset`.

### Deshaciendo el staging de un archivo preparado con git restore

digamos que has cambiado dos archivos y deseas confirmarlos como dos cambios separados, pero accidentalmente escribes `git add` y los agregas al área de preparación. ¿Cómo puedes deshacer el staging de uno de los dos? El comando `git status` te lo recuerda:
```
    $ git add *
    $ git status
    On branch master
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
        modified:   CONTRIBUTING.md
        renamed:    README.md -> README
```
Haciendo caso 
```
    $ git restore --staged CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
        renamed:    README.md -> README

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
        modified:   CONTRIBUTING.md
```

### Deshaciendo cambios con git restore

Para deshacer los cambios le hacemos caso a `git status` en la sección:
```
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
        modified:   CONTRIBUTING.md
```
Haciendolo:
```
    $ git restore CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
        renamed:    README.md -> README
```
