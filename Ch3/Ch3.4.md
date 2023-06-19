# Linea de trabajo de las ramas

Veremos las posibilidades que tienes al usar las ramas y los posibles caminos que puedes recorrer para que seas mejor usandolas

## Ramas de largo plazo
Como git utiliza un un sistema de tres vias para hacer `merges` es facil usar una solo rama para hacer los `merge` sobre `master`, esto significa que puedes tener siempre una cantidad determinada de ramas sobre las cuales hacer distintos pasos o procesos del proyecto e ir `merging` a la rama `master`.
Muchos desarrolladores utilizan una rama paralela a `master` para probar si el codigo es estable o funciona, a la cual le hacen `merge` a `master` una vez estan seguros si funciona, y sobre esa rama crean ramas topicas las cuales son de vida corta, esto se hace para comprobar que no se introducen bugs al programa principal.
Puedes tener varios niveles de estabilidad para asegurarte que tu codigo esta seguro y de que los contribuidores pueden hacer o proponer cambios que concideren necesarios.

## Ramas Topicas
Las ramas topicas son utiles en un proyecto de cualquier tamaño, funcionan de la siguiente manera, son ramas creadas especificamente para un solo tema del trabajo.
Una ventaja de estas, es que puedes trabajar en multiples al mismo tiempo, entonces pueden existir por dias o meses antes de que esten listas y les hagas `merge`.

[Anterior](Ch2.3.md)
[Siguiente](Ch2.5.md)
[Indice](https://github.com/IIKUYY/Git-basico/blob/main/Ch3/README.md)