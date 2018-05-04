# Tabla de Contenido:

* Convenciones
* Git
    * Rasteo
    * Creando el repositorio
    * El Stage
    * Commits
    * Volviendo en el tiempo
        * Resets
        * Revert
        * Checkout
    * Ramas
        * Crear y unir ramas
        * Conflictos
* Github
    * Beneficios de la Universidad
    * Crear una cuenta
    * Iniciar un repositorio
    * Clonar
    * Añadir colaboradores
    * Push
    * Pull
* Comentarios finales.

# Convenciones 

Antes de comenzar, deberán tener en cuenta las siguientes convenciones, no se asusten, ya veremos de que trata cada una.

![legends](https://pbs.twimg.com/media/DbyOPZqXkAELBcC.png)

# Git

## Rastreo 
Comenzaremos tratando Git de su forma local, nos independizaremos completamente del internet, y esa es su mayor ventaja, tratar control de versiones de forma offline.

Git en sí es un controlador de versiones que se encargará de llevar rastro del trabajo de un proyecto, principalmente de momentos los cuales llamaremos **commits**

Si escribimos en un repositorio cualquiera ```git log``` nos retornará los commits hechos durante toda la historia del repositorio y su información, algo así

![repo](https://pbs.twimg.com/media/Dbq_gi7W0AA8AVA.png)

En consola los resultados son algo similar a la representación gráfica, solo que los ids fueron simplificados en el ejemplo gráfico (Mientras que en el gráfico el id del commit superior es **004**, en el commit es **f9e95e454c764bd746c5dec012d13bf2289877bf** y se mantendrá este tipo de simplificación por el resto del documento.

```console
$ git log
commit f9e95e454c764bd746c5dec012d13bf2289877bf
Author: Unknown <alejandradrispe@unal.edu.co>
Date:   Sat Apr 21 20:57:16 2018 -0500

    Iniciando Mapa

commit bf015fef7e9d0e0c214333e020e0e17ea24ec49e
Author: Unknown <alejandradrispe@unal.edu.co>
Date:   Fri Apr 20 20:29:08 2018 -0500

    HashMap creado. Triangulo Borrado

commit 979eda3c12492a3361a7aa78160d13eb712c1092
Author: Unknown <juasmartinezbel@unal.edu.co>
Date:   Thu Apr 19 17:09:14 2018 -0500

    Triangulo generado

commit ce7fc08884cfaa085f07e74f9503ddeda21a7180
Author: Unknown <juasmartinezbel@unal.edu.co>
Date:   Sat Apr 14 10:37:51 2018 -0500

    Archivos creados
```

Si queremos ver algo más detallado, nada más es cuestión de escribir en el repositorio ```$ git log --stat```

![stat](https://pbs.twimg.com/media/DbrC3r6W4AEq8YR.png)

```console
$ git log --stat
commit f9e95e454c764bd746c5dec012d13bf2289877bf
Author: Unknown <alejandradrispe@unal.edu.co>
Date:   Sat Apr 21 20:57:16 2018 -0500

    Iniciando Mapa

 M  |  70
 1 files changed, 70 insertions(+)
 
commit bf015fef7e9d0e0c214333e020e0e17ea24ec49e
Author: Unknown <alejandradrispe@unal.edu.co>
Date:   Fri Apr 20 20:29:08 2018 -0500

    HashMap creado. Triangulo Borrado
    
 T              |  20 ---
 b              |  30 +++
 2 files changed, 60 insertions(+), 20 deletions(-)
 
commit 979eda3c12492a3361a7aa78160d13eb712c1092
Author: Unknown <juasmartinezbel@unal.edu.co>
Date:   Thu Apr 19 17:09:14 2018 -0500

    Triangulo generado
    
 T  |  20
 A  |  30 +++-
 2 files changed, 45 insertions(+), 5 deletions(-)
 
commit ce7fc08884cfaa085f07e74f9503ddeda21a7180
Author: Unknown <juasmartinezbel@unal.edu.co>
Date:   Sat Apr 14 10:37:51 2018 -0500

    Archivos creados
    
 A  |  7 
 B  |  10
 C  |  16
 3 files changed, 33 insertions(+)
```
## Creando el repositorio.

Vale, ya sé como lleva rastro git de esos "momentos", ¿cómo lo inicializo?

Fácil, de manera local, lo único que debes hacer es pararte sobre un directorio y ejecutar el comando ```$ git init``` y ya. Digamos, queremos inicializar un repositorio llamado _Ejemplo_, nada más vamos a iniciarlo dentro de un directorio llamada _Ejemplo_

```console
PC@PC-PR24601:~/Ejemplo$ git init
Initialized empty Git repository in /home/PC/Ejemplo/.git/
```

## El stage

Supongamos que creamos dos archivos llamados ```u``` y ```v```, cada uno con su respectivo contenido dentro del directorio _Ejemplo_.

![uv](https://pbs.twimg.com/media/DbrGsCAX4AE6Z8i.png)

Ya con esto "hacen parte" del repositorio, y lo digo entre comillas porque... pues, no realmente, el repositorio sabe que están ahí, simplemente no sabe que hacer con ellos. En este momento introduciremos un nuevo comando ```git status``` el cual me permitirá ver el estado actual del repositorio, que cambios detecta que hubo. En este caso, dos elementos añadidos, u y v

```console
$ git status
En la rama master

Commit inicial

Archivos sin seguimiento:
  (use «git add <archivo>...» para incluir en lo que se ha de confirmar)

	u
	v

no se ha agregado nada al commit pero existen archivos sin seguimiento (use «git add» para darle seguimiento)
```

Tal y como dice en la parte de abajo, esos archivos no están siendo seguidos, no están "en stage" por así decirlos. Consideremos el Stage como la caja que guarda los archivos que queremos "recordar". Vamos a entonces a empacar **u**.


```console
$ git add u
$ git status
En la rama master

Commit inicial

Cambios para hacer commit:
  (use «git rm --cached <archivo>...» para sacar del stage)

	nuevo archivo: u

Archivos sin seguimiento:
  (use «git add <archivo>...» para incluir en lo que se ha de confirmar)

	v
```
![add](https://pbs.twimg.com/media/DbrIgtLXkAIAsTb.png)

Si queríamos añadir los dos archivos, podíamos escribir lo siguiente

```console
$ git add u v
$ git status
En la rama master

Commit inicial

Cambios para hacer commit:
  (use «git rm --cached <archivo>...» para sacar del stage)

	nuevo archivo: u
	nuevo archivo: v
```

![adduv](https://pbs.twimg.com/media/DbrJKdCXUAE2aSV.png)

En caso de tener más archivos, o ahorrarse el proceso de tener que escribir el nombre de cada uno de los archivos agregados o modificados, podríamos simplemente escribir una de las siguientes opciones:

```console
$ git add --all
$ git add -A
$ git add *
$ git add .
```

Estos comandos, como habrán podido insinuar, añaden todos los archivos cambiados/añadidos/registrados como eliminados, sean 2, 10, 50, 100, todo, absolutamente todo, sin discriminar... bueno, hay una forma de discriminar, y es creando un archivo ```.gitignore```

El archivo ```.gitignore``` será un archivo donde colocaremos todas las cosas que no queremos que el Git recuerde. Quizás porque no me conviene, no es util o es muy pesado.

### _Advertencia, en su práctica **JAMÁS** deben incluir los .dat, son muy pesados_

Supongamos que tengo 4 nuevos archivos:

![files](https://pbs.twimg.com/media/DbrKppiX4AA79jb.png)

Supongamos que **i** es un log que me registra cada cosa que hago y crece megabytes por nada, así que será muy pesado, y los **.tm** son ejecutables dependientes de la maquina, así que no los tomaremos en cuenta (o inventense la excusa que quieran, no los vamos a añadir). aquí entra el ```.gitignore```. Creamos y abrimos un archivo con este nombre y escribimos en él lo que quiero que JAMÁS me añada:

```bash
i
*.tm
```

```console
$ git add *
Las siguientes rutas son ignoradas por alguno de tus archivos .gitignore:
i
j.tm
l.tm
Use -f si en verdad desea agregarlos.
$ git status
En la rama master

Commit inicial

Cambios para hacer commit:
  (use «git rm --cached <archivo>...» para sacar del stage)

	nuevo archivo: k
	nuevo archivo: u
	nuevo archivo: v

Archivos sin seguimiento:
  (use «git add <archivo>...» para incluir en lo que se ha de confirmar)

	.gitignore

$ git add .gitignore
$ git status
En la rama master

Commit inicial

Cambios para hacer commit:
  (use «git rm --cached <archivo>...» para sacar del stage)

	nuevo archivo: .gitignore
	nuevo archivo: k
	nuevo archivo: u
	nuevo archivo: v
```

Al final, la estructura de lo que hicimos quedará de esta forma:

![gitignore](https://pbs.twimg.com/media/DbrN7hFWAAEBUsX.png)

Ahora digamos que no estamos satisfechos con esto que vamos a empacar, deseamos retirar los archivos que ya pusimos, aquí se nos presentan el comando ```$ git reset```

Sacar solo un archivo del stage, ya sea para modificar o algo:
``` console
$ git reset u
```
![u](https://pbs.twimg.com/media/DbrOHbQWsAAE5fc.png)

Sacar varios archivos del stage, ya sea para modificar o algo:
``` console
$ git reset v k
```
![u](https://pbs.twimg.com/media/DbrOGZvW4AAwXgt.png)

Sacar absolutamente todo del stage:
``` console
$ git reset
```
![u](https://pbs.twimg.com/media/DbrN1f2XUAAjO22.png)

## Commits

La parte más interesante de Git, sin duda, son los **commits**, git basa toda su arquitectura en estos pequeños momentos en el tiempo.

Para poder guardar estos commits hay que tener en cuenta dos cosas:

1. Tiene que haber SIEMPRE algo en el stage.
2. Ver que no hayan commits delante de nosotros en la nube.

Trataremos este segundo más adelante. Considerando que tenemos todo listo, lo único que debemos hacer es escribir en la terminal ```$ git commit``` y nos saldrá una pantalla en _Nano_ donde podremos escribir nuestro _Título de commit_ y en comentarios la descripción de dicho ```$git status``` y otra información sobre este.

Existe una versión resumida de hacer esto sin necesidad de que se abra la pantalla del _Nano_ (o del editor configurado), que es escribiendo ```$ git commit -m "Titulo del Commit"```

```console
$ git add --all

$ git status
En la rama master

Commit inicial

Cambios para hacer commit:
  (use «git rm --cached <archivo>...» para sacar del stage)

	nuevo archivo: .gitignore
	nuevo archivo: k
	nuevo archivo: u
	nuevo archivo: v
	
	
$ git commit -m "Todo añadido"
[master (root-commit) 31549bf] Todo añadido
 4 files changed, 183 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 k
 create mode 100644 u
 create mode 100644 v
 
 
$ git status
En la rama master
nothing to commit, working directory clean


$ git log
commit 31549bfd8a795b6b33bc03ac05ffa68062f64efc
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Wed Apr 25 21:41:27 2018 -0500

    Todo añadido
    
    
$ git log --stat
commit 31549bfd8a795b6b33bc03ac05ffa68062f64efc
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Wed Apr 25 21:41:27 2018 -0500

    Todo añadido

 .gitignore | 2 ++
 k          | 43 +++++++
 u          | 104 ++++++++++++
 v          | 34 +++++++
 4 files changed, 183 insertions(+)

```

![commit](https://pbs.twimg.com/media/DbrS7hQXkAAc5Dv.png)

En resumen, hacer commits no es complicado, solo hay que tener mucho cuidado a la hora de trabajar, si no hay problemas, no hay que requerir de ```resets``` o ```.gitignore``` ni nada de eso. Se volverá rutina **añadir trabajo al stage** para luego registrarlo en un **commit**

```console
*Trabajar*
$ git add --all
$ git commit -m "Commit 01"

*Trabajar*
$ git add --all
$ git commit -m "Commit 02"

*Trabajar*
$ git add --all
$ git commit -m "Commit 03"

*Trabajar*
$ git add --all
$ git commit -m "Commit 04"

*Trabajar*
$ git add --all; git commit -m "Commit 05"

*Trabajar*
$ git add -A; git commit -m "Commit 06"

*Trabajar*
$ git add -A && git commit -m "Commit 07"

*Trabajar*
$ git add . && $git commit -m "Commit 08"

```

Y así de simple fue hacer 8 commits de largas y largas horas de trabajo arduo, todo guardado y reservado con solo dos lineas de comandos, o incluso menos, dependiendo de qué tan comodo te sientas. Pudieron ser minutos, horas, días o semanas, pero si deseamos, podemos volver en el tiempo a visitar esos commits que hicimos.

## Volviendo en el tiempo: Resets, Reverts y Checkouts

Existen 3 niveles para volver a un estado en el tiempo, iremos vistando de lo más duro a lo más suave, por referirnos de alguna manera a estos momentos, vamos a visitar cada una.

## Resets:

Reset es regresar físicamente a un commit anterior en el tiempo, borrando los commits que hicimos después del cual deseamos volver. Pero no se asusten, en este tipo de situaciones hay 3 formas de hacer resets para asegurar una mejor forma de reseteo.

Tenemos el siguiente árbol de commits con los logs correspondientes:

```console
$ git log --stat
commit d16ca314f1e9667b27fd849616449ba98b7c056f
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Thu Apr 26 19:07:28 2018 -0500

    Se han implementado funciones

 A         | 60 +++++-
 B         | 20 +-
 C         | 20 ++
 D         | 10 +
 F         | 10 +
 funciones | 10 +
 6 files changed, 110 insertions(+), 20 deletions(-)

commit 5c7759672bb300dc33303040317962ef5dffc97e
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Thu Apr 26 13:16:00 2018 -0500

    Se ha generado el Árbol

 Arbol.py |  50
 C        | 100 ++++++++++
 2 files changed, 150 insertions(+)

commit 7901658d4701e25919bf7dabddb83964c9fb7941
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Thu Apr 26 12:05:19 2018 -0500

    Se ha añadido el README

 A         | 120 ++++++++++++
 B         | 150 +++++++++++++++
 README.md |  200
 3 files changed, 470 insertions(+)

commit 001
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Thu Apr 26 09:02:35 2018 -0500

    Archivos creados

 A | 100
 B | 100
 C | 100
 D | 100
 F | 100
 5 files changed, 100 insertions(+), 0 deletions(-)
```

![commits](https://pbs.twimg.com/media/DbuYCtRWkAE84qG.png)

Vamos a entonces a analizar los diferentes tipos de reset aplicados a este repositorio:

#### SOFT

Un soft reset se hace para volver a un estado anterior, pero que todos los estados queden en stage. Este tipo de reset se utiliza para cuando se nos olvidó agregar un cambio al commit.

```console
$git reset --soft id_commit_a_regresar
```
 Por ejemplo, digamos que se nos olvidó agregar al ```commit 004```, debemos entonces volver al ```commit 003```, lo único que debemos hacer es escribir el siguiente commando:
 
```console
$git reset --soft 5c7759672bb300dc33303040317962ef5dffc97e #id del commit 003
```
Y si escribimos el comando ```$ git status``` podemos ver que los elementos efectivamente se encuentran en stage:

```console
$ git status
En la rama master
Cambios para hacer commit:
  (use «git reset HEAD <archivo>...» para sacar del stage)

	modificado:    A
	modificado:    B
	modificado:    C
	modificado:    D
	modificado:    F
	nuevo archivo: funciones


```

![commit](https://pbs.twimg.com/media/DbuZZoYX0AAThXd.png)

Estos resets pueden ir más allá de un solo commit, si queremos volver no al ```commit 003``` sino al ```commit 002``` lo único que cambia es la id referenciada en el reset

```console
$git reset --soft 7901658d4701e25919bf7dabddb83964c9fb7941
```

![commit](https://pbs.twimg.com/media/DbuZn-1XcAA4BJH.png)

Los cambios que se hicieron después sobrescriben el commit más antiguo, es decir, la versión de ```C``` del ```commit 004``` sobrescribe a la del ```commit 003```

### Mixed 
Mixed funciona similar a soft, en el que el reset no descarta los cambios, sino que los deja guardados en algún lado, en este caso, no los tiene ya puestos en stage.

 
```console
$git reset --mixed 5c7759672bb300dc33303040317962ef5dffc97e
```

 
```console
$git status
En la rama master
Cambios no preparados para el commit:
  (use «git add <archivo>...» para actualizar lo que se confirmará)
  (use «git checkout -- <archivo>...» para descartar cambios en el directorio de trabajo)

	modificado:    A
	modificado:    B
	modificado:    C
	modificado:    D
	modificado:    F

Archivos sin seguimiento:
  (use «git add <archivo>...» para incluir en lo que se ha de confirmar)

	funciones

no hay cambios agregados al commit (use «git add» o «git commit -a»)
```
![mixed](https://pbs.twimg.com/media/DbubPIEWsAYUrzN.png)

### Hard

El hard reset, o el reset de la desesperación, es un reset aun más duro. Si ejecutamos un hard reset al ```commit 003```, todos los commits después de este desaparecerán, incluyendo su contenido.

```console
$git reset --hard 5c7759672bb300dc33303040317962ef5dffc97e
```
```console
$ git status
En la rama master
nothing to commit, working directory clean
```

![hard](https://pbs.twimg.com/media/DbuchPNWkAMxXil.png)

O incluso si estamos en ```commit 004``` y decidimos hacer hard a ```commit 002```, lo único que cambia es el id en el comando

```console
$git reset --hard 7901658d4701e25919bf7dabddb83964c9fb7941
```

![hard](https://pbs.twimg.com/media/Dbucs8UX4AAxbqe.png)

## Revert

Como su nombre lo indica, reversa los cambios que se tienen en un commit. Luego de haber eliminado a ```commit 004``` en el ejemplo pasado, nos queda este arbol

![hard](https://pbs.twimg.com/media/DbuchPNWkAMxXil.png)

Sin embargo, quiero deshacer los cambios en  ```commit 003```, pero quiero mantener a este commit en el tiempo, o quizás está en la nube y no puedo hacerle reset, aquí entra  ```revert```, lo único que hay que hacer es ejecutar el comando ```$ git revert id```, donde id es el commit que queremos revertir.

```console
$ git revert 5c7759672bb300dc33303040317962ef5dffc97e
[master 4e400a0] Revert "Se ha generado el Árbol"
 2 files changed, 150 deletions(-)
 delete mode 100644 Arbol.py
```

Este **revert** es un commit por sí mismo, por lo tanto el nuevo árbol se verá de esta forma:

![revert](https://pbs.twimg.com/media/DbufR3SW0AEkcgR.png)

## Checkout

Hay una forma de volver en el tiempo sin necesidad de pedrer lo que se ha hecho luego, y es con un **checkout**.

Acá se nos introduce el concepto de **Cabeza** o **HEAD**, que es en dónde nos encontramos parados en el árbol, usualmente, siempre es en el último commit hecho

![revert](https://pbs.twimg.com/media/Dbufw9qX4AEXQ-8.png)

Para volver en el tiempo, únicamente debemos escribir el comando ```$ git checkout id``` en donde id es el id al que queremos volver, y una vez parados ahí, los archivos serán exactamente igual a como eran en ese momento del tiempo, pero no se podrán modificar. Solo mirar o crear ramas, que veremos más adelante.

```console
$ git checkout 5c7759672bb300dc33303040317962ef5dffc97e
Note: checking out '5c7759672bb300dc33303040317962ef5dffc97e'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD se encuentra en 5c77596... Se ha generado el Árbol
```

![HEAD](https://pbs.twimg.com/media/DbugtuDXUAECqps.png)

Para volver al tope de todo, solo debemos escribir:
```console
$ git checkout master
```

## Ramas

#### Crear y unir ramas
Ahora hablaremos de un aspecto fundamental en Git, y son las bifurcaciones para realizar tareas. Si bien existe [terminología especifica para realizar un trabajo más formal y prácticas ágiles](http://aprendegit.com/git-flow-la-rama-develop-y-uso-de-feature-branches/), no las trataremos en este documento, acá solo aprenderemos lo fundamental, pasitos de bebé, **no se pedirá el uso obligatorio de ramas** como probablemente les haya tocado en otras materias, pero es recomendable.

Todo el tiempo hemos estado sobre una rama llamada "master", nuestra linea de trabajo principal, sin embargo, si deseamos crear un flujo de trabajo alterno para no interrumpir el trabajo de los demás si vamos a trabajar en otra cosa, o una tarea secundaria que no estamos seguros que tan bien resulte y queremos dejar a master solo para los cambios fijos.

Para crear una rama, lo único que debemos hacer es inicializar una rama, en el punto del tiempo en que nos encontremos (La cabeza debe estar en el punto donde quieras inicializar la rama) y escriremos:
```console
$ git branch nombre_rama
```
Y con eso se habrá creado nuestra nueva rama.

![rama](https://pbs.twimg.com/media/DbyP_L6X0AA4ulq.png)

Veremos como funciona... ahora.

Supongamos que en su espacio de trabajo tenemos este árbol

![arbol](https://pbs.twimg.com/media/DbyRnueWAAAClmj.png)

Donde Main.c tiene el siguiente contenido:

```c
#include <stdio.h>

int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}
```

Ahora supongamos que usted y su pareja necesitan hacer una función ```suma(x,y)``` y una ```resta(x,y)```. Usted confía que su compañero hará un buen trabajo, así que usted creará una nueva rama llamada ```resta```.

```console
$ git branch resta
```

Tendremos esto entonces:

![branch](https://pbs.twimg.com/media/DbyS7nhWAAENapd.png)

Para ver todas las ramas creadas, usted nada más debe escribir ```$ git branch``` a secas.


Su compañero comenzará a trabajar en la función suma.

```c
#include <stdio.h>
int suma(int x, int y){
    int a=x;
    int b=y;
    int c=a+b;
	return c;
}
int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}
```

![suma](https://pbs.twimg.com/media/DbyUVdiW4AE_hBt.png)

Y usted en la función resta, aunque claro, todavía no se ha parado en la rama ```resta```, nada más debe escribir:

```
$ git checkout resta
Switched to branch 'resta'
```

O si desea crear una rama y de una vez pararse ahí

```
$git checkout -b resta
Switched to branch 'resta'
```


![resta](https://pbs.twimg.com/media/DbyU3i7XcAA3fVI.png)

**Nota**: Este cambio de checkout será posible siempre y cuando no hayan cambios conflictivos sin guardar, ya veremos este concepto más adelante.

Y ahora pasamos a modificar.

```c
#include <stdio.h>
int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}

int resta(int x, int y){
    int i=x;
    int j=y;
    int k=i-j;
	return k;
}
```

Ambos hacen commit y ya. Sin embargo, usted no podrá ver los cambios que hay en otras ramas, solo del momento en que se creó su rama de para abajo, y claro, todos los commits de su respectiva rama. Su compañero tampoco podrá ver que cambios ha hecho usted, pero existen.

Si desea verlas todas, el comando ```$git log --all``` es el indicado

```console
$ git log --all
commit 05c1e3bf941e12b7aeb1d3ad37eafd3383313020
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Fri Apr 27 06:28:32 2018 -0500

    resta creada

commit 863e4b5569ae3994e2799863615eef626d4fefa3
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Fri Apr 27 06:26:35 2018 -0500

    suma creada

commit ddbf75f142320e7830ea35739c353982b7c9aaec
Author: Unknown <juasmartinezbel@unal.edu.co>
Date:   Fri Apr 27 06:16:33 2018 -0500

    Main changed
```

![all](https://pbs.twimg.com/media/DbyVr06WsAYgZ9b.png)

Es hora, entonces, de incorporar los cambios, es supremamente sencillo... siempre y cuando hayan hecho las cosas bien. En este caso, todo debería salir bien. 

Lo primero que debemos hacer es pararnos en la rama padre/rama donde queremos incorporar todos los cambios, es decir, a master.

Y luego escribir el siguiente comando:

```console
$ git merge resta -m "Merge resta into master"
Automezclado main.c
Merge made by the 'recursive' strategy.
 Main.c | 4 ++++
 1 file changed, 4 insertions(+)
```

Como ya sabemos, el ```-m``` es para evitar que nos aparezca la pantalla extra de _Nano_, ya que sí, **Los merge cuentan como un commits hechos y derechos**, bien podría ser ```$ git merge resta ``` a secas y daría el mismo resultado.

Y ahora podemos verlo todo
```console
$ git log
commit e1ec067eef2c1c32277d5c78605fdaf5f91d4243
Merge: 863e4b5 05c1e3b
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Fri Apr 27 06:46:19 2018 -0500

    Merge resta into master

commit 05c1e3bf941e12b7aeb1d3ad37eafd3383313020
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Fri Apr 27 06:28:32 2018 -0500

    resta creada

commit 863e4b5569ae3994e2799863615eef626d4fefa3
Author: zebitas <juasmartinezbel@unal.edu.co>
Date:   Fri Apr 27 06:26:35 2018 -0500

    suma creada

```

![merge](https://pbs.twimg.com/media/DbyZeGaWkAEU2fG.png)

Y el producto final será:

```c
#include <stdio.h>

int suma(int x, int y){
	int a=x;
    int b=y;
    int c=a+b;
	return c;
}

int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}

int resta(int x, int y){
    int i=x;
    int j=y;
    int k=i-j;
	return k;
}
```

### Conflictos
Sin embargo la vida no es color de rosa, muchas veces se producirán algo llamado **conflictos** y la razón de porque la coordinación es importante. Cuando se trabaja en ramas, es importante elegir diferentes secciones de código a trabajar, diferentes archivos, diferentes carpetas, nunca trabajar sobre el mismo, sin embargo a veces no tenemos opción.

Supongamos que en el ejemplo anterior, no escribimos resta debajo de main, sino exactamente en el mismo lugar donde nuestro compañero escribió su función suma

```c
#include <stdio.h>

int resta(int x, int y){
	int i=x;
    int j=y;
    int k=i-j;
	return k;

}

int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}
```

El arbol y todo sigue exactamente igual hasta este punto, pero cuando deseamos hacer merge, sucederá lo siguiente:
```console
$ git merge resta
Automezclado Main.c
CONFLICTO(contenido): conflicto de fusión en Main.c
Automatic merge failed; fix conflicts and then commit the result.
```

En este caso, Git nos arrojó un error de conflicto, ¿qué sucedió?, entremos a la función Main.c para veriguarlo

```c
#include <stdio.h>

<<<<<<< HEAD
int suma(int x, int y){
	int a=x;
	int b=y
	int c=a+b;
	return c;
}
=======
int resta(int x, int y){ 
  int i=x; 
  int j=y 
  int k=i-j; 
  return k; 
} 
>>>>>>> resta

int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}
```

Los símbolos extraños nos muestran una división de dónde está el conflicto, 
lo único que debemos hacer para solucionar este conflicto es quitar y dejar las lineas que queramos (y todo eso de HEAD, resta y ======), en este caso no es la gran cosa, es solo **una función** en **un archivo**, nada más es quitar esos símbolos, **asegurarse de que todo se vea en orden** guardar y hacer commit.

```c
#include <stdio.h>

int suma(int x, int y){
	int a=x;
	int b=y
	int c=a+b;
	return c;
}
int resta(int x, int y){ 
  int i=x; 
  int j=y 
  int k=i-j; 
  return k; 
} 

int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}
```

```console
$ git merge restaAutomezclado Main.c
CONFLICTO(contenido): conflicto de fusión en Main.c
Automatic merge failed; fix conflicts and then commit the result.

$ gedit Main.c

$ git add -A; git commit -m "Merge resta into master. Conflict resolved"
[master 4b524ab] Merge resta into master. Conflict resolved
```

Y así se soluciona. Pero estos conflictos pueden ser muy grandes, pueden estar en múltiples archivos, multiples lineas de código, etc. que hacer el proceso general puede terminar siendo un dolor de cabeza. Para esto existen herramientas para ayudar a agilizar este proceso, una de ellas se llama **GitKraken**, donde a la hora de hacer merge, deja seleccionar y ver más detalladamente qué archivos y secciones de código tienen este conflicto y seleccionar más facilmente qué entra y qué sale

![gk](https://pbs.twimg.com/media/DbymYSfWsAAjLLP.jpg)

![gk](https://pbs.twimg.com/media/DbymYSeX0AERnuX.jpg)

Hablaré de esta herramienta más adelante.

El mensaje general queda:

### Mucho cuidado con respecto a las secciones y cómo vayan a trabajar.

Repartanse código con cuidado, o dividan bien las secciones en dónde van a trabajar. 

Si trabajan en el mismo archivo, hay formas de dividir el área de trabajo, ya sea inicializando las funciones en las que trabajarán después antes de hacer la rama.

```c
#include <stdio.h>

int suma(){
	return 0;
}

int resta(){
	return 0;
}

int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}
```
O comentando una linea que divida el documento y cada rama sepa dónde va a trabajar.
```c
#include <stdio.h>

//Suma:

//Resta:

int main(void) {
  int x=6;
  int y=10;
  printf("\n");
  return 0;
}
```
Y otras formas más creativas de saber que hará qué antes de dividir el árbol. A veces es inevitable, quizás alguien borró por accidente una linea que otro usaba o ambos llamaron una variable que cumple la misma tarea con otro nombre, pero siempre tener a la mano qué dejar a la hora de que hayan conflictos.

# Github 
Ahora hablemos de Github, existen otras alternativas como Bitbucket, pero Github es la más popular. Github es un sitio web dedicado al desarrollo colaborativo y colocar repositorios en linea de nabera pública, tal y como el que están leyendo ahora.

## Beneficios de la Universidad

El ser estudiante de La Universidad Nacional de Colombia trae consigo ciertos [beneficios y recursos a los cuales los estudiantes pueden acceder](http://www.pasaralaunacional.com/2017/02/software-beneficios-gratis-convenios-unal.html) entre ellos los que nos interesa hoy es el [Student Developer Pack](https://education.github.com/pack) que nos ofrece Github, de los cuales por ahora nos interesan los siguientes dos:

![Github](https://pbs.twimg.com/media/Dbywu08W4AU8nhl.jpg)

1. Una cuenta premium en Github que nos sirve para crear repositorios privados
2. Una cuenta premium para poder usar GitKraken, una interfaz gráfica para utilizar todas las opciones de Git de forma más directa y representar el árbol de una forma más cómoda.

Pueden usar el software que deseen o seguir haciendo todo el proceso de Git de forma manual, es solo una recomendación personal.

Estos beneficios se pueden obtener siempre y cuando la cuenta de Github esté asociada a la Universidad

## Crear una cuenta

Una sección de sobra, lo único que deben hacer es crear una cuenta para poder usarla, con el correo de la universidad. Lo único que les recomiendo es **crear un usuario con el mismo nombre de su usuario de la universidad**, esto para que sea más facil de encotrarlos en la plataforma y asociarlos más facilmente a la hora de presentar trabajos relacionados con Git.

## Iniciar Un Repositorio

Una vez creada la cuenta, hay varias opciones para iniciar un nuevo proyecto:

![crear](https://pbs.twimg.com/media/Dby1I4SWkAE_enT.jpg)

Una vez le demos click a esta opción, se nos pedirá información básica:

![info](https://pbs.twimg.com/media/Dby1I4VXkAcWkNa.jpg)

1. Nombre
2. Descripción
3. ¿Público o Privado?, es decir, ¿Lo podrán ver todos o solo los colaboradores?
4. README.md

El README.md es un archivo tipo _Markdown_ que describe el proyecto o documentación general, como por ejemplo, esto que estás leyendo. **ÚNICAMENTE** deben darle click a esta opción si el proyecto es **completamente nuevo.**

¿A qué se refiere con completamente nuevo?

Cuando le damos click a _"Create Repository"_ nos saldrá la siquiente ventana

![start](https://pbs.twimg.com/media/Dby1I4TWAAAAc37.jpg)

Analizaremos cada una de estas opciones:
1. **Quick Setup:** Se proporciona un Link HTTPS o un SSH para vincular rápidamente nuestro repositorio. Ignoraremos el SSH, nos intereza el HTTPS, lo único que debemos hacer es ir a donde queremos nuestro repositorio y escribiremos el comando ```$ git clone url```:
    ```console
    $ git clone https://github.com/juasmartinezbel/Mi_Repositorio.git
    Clonar en «Mi_Repositorio»...
    warning: Parece que ha clonado un repositorio vacío.
    Comprobando la conectividad… hecho.
    ```
    Se nos creará un directorio llamado igual que el repositorio, es la manera más fácil [y mi opinión personal, la más práctica] de crear un repositorio
    
    A esto se le llama crear un repositorio **completamente nuevo.** Si hubieran querido, el README.md pudo haber sido creado aquí sin ningún problema.
2. **Crear repositorio localmente:** La opción 2 y 3 son la misma en teoría ya que ambas utilizan el mismo comando: ```$ git remote add origin url```, la diferencia es que la opción 2 si el repositorio es completamente virgen, mientras que la opción 3 es si ya habíamos trabajado anteriormente, por ejemplo, nuestro proyecto de Suma y Resta. 
    ```console
    /Branches$ git remote add origin https://github.com/juasmartinezbel/Mi_Repositorio.git
    
    /Branches$ git push -u origin master
    Username for 'https://github.com': juasmartinezbel
    Password for 'https://juasmartinezbel@github.com': 
    Counting objects: 28, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (26/26), done.
    Writing objects: 100% (28/28), 4.78 KiB | 0 bytes/s, done.
    Total 28 (delta 9), reused 0 (delta 0)
    remote: Resolving deltas: 100% (9/9), done.
    To https://github.com/juasmartinezbel/Mi_Repositorio.git
     * [new branch]      master -> master
    Branch master set up to track remote branch master from origin.
    ```
    Esta opción quedó clara que sirve más que nada para cuando deseamos subir un repositorio en el que ya hemos trabajado, si es un repositorio completamente nuevo, mejor utilizar el ```clone```. Al usar  ```$ git remore add origin url``` tampoco importa el nombre del repositorio que tenemos localmente.
    
    ![new](https://pbs.twimg.com/media/Dby4SMNXcAAKh81.jpg)
    
    ![new](https://pbs.twimg.com/media/Dby4SOfWAAEVNXd.jpg)
3. **Importar código:** Se trata de importar código de otro repositorio repositorio, autoexplicativo, así que no lo trataremos en detalle.

## Clonar

Ya vimos un comando que se llamaba ```$ git clone url```, y ya se pueden hacer una idea para qué sirve, **colocar un repositorio de Github en nuestra computadora**, podemos tomar cualquier código, de quien sea, siempre y cuando sea público, y clonarlo en nuestras máquinas, nada más hay que copiar el enlace propocionado en el repositorio:

![clone](https://pbs.twimg.com/media/Dby5yWPWkAE2m2b.jpg)

Hay que tener en cuenta una cosa, únicamente podremos colaborar en el repositorio si **somos colaboradores**. Podemos hacer los cambios que queramos localmente, claro, pero no subirlo al mismo repositorio.

Existe, sin embargo, algo que se llama **Fork** y **Pull Request**, que sirven para colaboraciones masivas y que el dueño del repositorio vea tus cambios propuestos, pero no los trataremos en este curso.

## Añadir Colaboradores

Si en nuestro proyecto vamos a _Settings>Collaborators_, podremos agregar a las personas que deseamos colaboren en nuestro proyecto. Les llegará una invitación a su correo dónde podrán aceptar la invitación. Si no, podrán enviar el link del repositorio para aceptar la invitación. Por esto mi recomendación de llamarse igual a su usuario de la universidad, es más facil de encontrar.

![sc](https://pbs.twimg.com/media/Dby67GRXkAAmJ0A.jpg)

Las personas añadidas como colaboradores son las que podrán colaborar libremente en el repositorio, aunque el dueño seguirá siendo el mismo.

Existen también las organizaciones, donde un grupo de personas podrá colaborar en proyectos comunes con un fin similar, esto se usa para grupos de 4 o más personas y tengan que trabajar colaborativamente en varios trabajos juntos.

![org](https://pbs.twimg.com/media/Dby8Pv5WsAAJqzg.jpg)

## Push

Una vez nuestro proyecto está creado y hemos trabajado, ```Push``` servirá para colocar nuestros cambios en Github, _"Empujarlos"_ hacia la nube por así decirlo.

![nube](https://pbs.twimg.com/media/Dby9yL5X0AAETKt.png)

Supongamos que nuestro repositorio está subido hasta el momento en que hicimos merge, sin embargo, hicimos dos commits en nuestra computadora, esto quiere decir que **_La cabeza local_** está dos commits más adelante que **_la cabeza del repositorio_**, dependiendo de la rama en la que estemos

```console
$ git status
En la rama master
Su rama está delante de «origin/master» para 2 commits.
  (use "git push" to publish your local commits)
nothing to commit, working directory clean
```
![local](https://pbs.twimg.com/media/Dby_G2PXUAEfGiX.png)

Si deseamos subir nuestros cambios, solamente debemos escribir el comando ```$ git push -u origin branch_name```

En este caso, estamos en la rama ```master```

```console
$ git push -u origin master
Username for 'https://github.com': juasmartinezbel
Password for 'https://juasmartinezbel@github.com': 
Counting objects: 4, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 473 bytes | 0 bytes/s, done.
Total 4 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To https://github.com/juasmartinezbel/Mi_Repositorio.git
   c749b42..17dc062  master -> master
Branch master set up to track remote branch master from origin.
```

Y así, las cabezas local y remota estarán igualadas.

```console
$ git status
En la rama master
Su rama está actualizada con «origin/master».
nothing to commit, working directory clean
```

![update](https://pbs.twimg.com/media/Dby_OKoXcAEfLyE.png)

``


## Pull

Existe algo llamado ```Pull```, que como se imaginarán, hace lo opuesto a ```Push```, que es descargar cambios remotos a la máquina, ya sea porque un compañero lo hizo, o trabajé en otra máquina, o se me dio por editar los archivos en Github

![pull](https://pbs.twimg.com/media/Dby9ydCW0AYUjnt.png)

Existe un comando llamado ```Fetch``` que crea una rama imaginaria llamada ```FETCH_HEAD``` a la cual se le hace ```merge``` a la rama dónde estaban los cambios en Github, ```Pull``` se existe de resumir este proceso y bajar todo directamente a la rama.

Supongamos que tenemos un escenario donde nuestro compañero hizo dos commits en master, y deseamos bajarlos a nuestra máquina, sabemos que esos commits están ahí:

```console
$ git status
En la rama master
Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)
nothing to commit, working directory clean
```
![update](https://pbs.twimg.com/media/DbzCaQTXUAAhxxH.png)

Lo único que debemos hacer es pararnos en la rama y escribir el comando ```$ git pull```
```
$ git pull
Updating 17dc062..e79faef
Fast-forward
 d | 155 +
 e | 213 +
 2 files changed, 368 insertions(+)
```

![update](https://pbs.twimg.com/media/DbzCaqUWkAEaAzh.png)

Y estaremos finalmente actualizados.

# Comentarios Finales: 

Hasta aquí Git básico, si bien, dejé varias cosas por fuera como ya había mencionado antes, por ejemplo lo de Fork y Pull Request, y también me falta aprender varias cosas relacionadas a los muchisimos comandos que tiene Git, esta es la forma más básica y práctica de utilizarlo y que servirán para sus futuras prácticas.
