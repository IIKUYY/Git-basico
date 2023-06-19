# Git alias
Git no infiere automáticamente tu comando si lo escribes parcialmente. Si no deseas escribir todo el texto de cada uno de los comandos de Git, puedes configurar fácilmente un alias para cada comando utilizando `git config`. Aquí tienes un par de ejemplos que puedes configurar.
```
    $ git config --global alias.co checkout
    $ git config --global alias.br branch
    $ git config --global alias.ci commit
    $ git config --global alias.st status
```
Esto significa que, por ejemplo, en lugar de escribir `git commit`, solo necesitas escribir `git ci`. A medida que uses Git, es probable que también utilices otros comandos con frecuencia; no dudes en crear nuevos alias.
Esta técnica también puede ser muy útil para crear comandos que creas que deberían existir. Por ejemplo, para solucionar el problema de usabilidad que encontraste al deshacer los cambios de un archivo, puedes agregar tu propio alias "unstage" a Git:
```
    $ git config --global alias.unstage 'reset HEAD --'
```
Esto hace estos dos comandos equivalentes
```
    $ git unstage fileA
    $ git reset HEAD -- fileA
```
Esto parece más claro. También es común agregar un comando `last`, de esta manera:
```
    $ git config --global alias.last 'log -1 HEAD'
```
de esta manera puedes ver el ultimo commit
```
$ git last
    commit 66938dae3329c7aebe598c2246a8e6af90d04646
    Author: Josh Goebel <dreamer3@example.com>
    Date:   Tue Aug 26 19:48:51 2008 +0800
    
        Test for current head
    
        Signed-off-by: Scott Chacon <schacon@example.com>
```

[Anterior](Ch2.6.md)
[Indice](Chttps://github.com/IIKUYY/Git-basico/blob/main/Ch2/README.md)