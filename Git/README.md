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

 M              |  70 +++
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
    
 T              |  20 +++
 A              |  30 +++-
 2 files changed, 45 insertions(+), 5 deletions(-)
 
commit ce7fc08884cfaa085f07e74f9503ddeda21a7180
Author: Unknown <juasmartinezbel@unal.edu.co>
Date:   Sat Apr 14 10:37:51 2018 -0500

    Archivos creados
    
 A             |  7 +++
 B             |  10 +++
 C             |  16 +++
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

Trataremos este segundo más adelante. Considerando que tenemos todo listo, lo único que debemos hacer es escribir en la terminal ```$ git commit``` y nos saldrá una pantalla en _Nano_ donde personalizaremos nuestro mensaje de commit y en comentarios la descripción de dicho ```$git status``` y otra información commit

Existe una versión resumida de hacer esto, que es escribiendo ```$ git commit -m "Titulo del Commit"```
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

# CONTINUARÁ
```


