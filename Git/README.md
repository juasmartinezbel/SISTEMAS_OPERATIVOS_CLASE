# Convenciones

Antes de comenzar, deberán tener en cuenta las siguientes convenciones, no se asusten, ya veremos de que trata cada una.

![legends](https://pbs.twimg.com/media/Dbq7P2LX0AAke4o.png)

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
*tm
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


```
