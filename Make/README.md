# Make
Make es una utilidad para automatizar la compilación de programas y librerías, así como automatizar la ejecución de comandos en general.

Un archivo de nombre ```Makefile```, ```makefile``` o ```GNUmakefile``` a secas (sin ninguna extensión) es un fichero que se encargará de decirle a la máquina que hay acciones asociadas a dónde se encuentra usted parado. Solo puede haber un make por directorio, y se puede llamar únicamente escribiendo:

```console
PC@PC-234we56t:~directorio_donde_esta_el_make$ make
```

Las tareas de make se crean en base a la siguiente estructura.

```make
Objetivo: Dependencias
    Comando1
    Comando2
    Comando3
```

Vamos a explicar cada uno de los elementos detalladamente.

## Objetivo
Objetivo es lo que queremos que se ejecute.

En sí es como llamar una función cualquiera, digamos que en C tenemos lo siguiente:

```c
int suma(void){
    return 1+3;
}

int main(void) {
    suma()
    return 0;
}
```
Para llamar la función suma, lo único que debemos hacer es escribir ```suma()``` en main, los **objetivos** funcionan como los nombres de la función. Si queremos llamar a una tarea del make, lo único que debemos hacer es escribir el nombre de su respectivo objetivo, como el siguiente makefile

```make
Objetivo: Dependencias
    comandoA
    comandoB
    comandoC
    
Objetivo2: Dependencias
    comandoJ
    comandoI
    comandoK
    
Objetivo3: Dependencias
    comandoO
    comandoP
    comandoQ
```

Por lo tanto, lo que hay que escribir en el directorio donde está el Makefile para ejecutar una tarea especifica es el siguiente comando en la terminal:

```console
$ make objetivo
ejecutando comandoA
ejecutando comandoB
ejecutando comandoC
$ make objetivo3
ejecutando comandoO
ejecutando comandoP
ejecutando comandoQ
```
##### Lo puede llamar como quiera, en el idioma que quiera.
##### Sin embargo **_hay una hay una regla con respecto a estos nombres_**:

Imaginese que nos encontramos dentro de la carpeta _1_ del [Ejemplo 1](https://github.com/juasmartinezbel/SISTEMAS_OPERATIVOS_CLASE/tree/master/Compilar%20en%20C) y deseamos crear ```exe``` en make, una función sencilla se encargará de esto, y justo así llamaremos nuestro **objetivo**

```make
exe: dependencias
    comando1
    comando2
    comando3
```

Y para llamarlo hay que ejecutar el comando

```console
$ make exe
ejecutando comando1
ejecutando comando2
ejecutando comando3
```

Bastante intuitivo, ¿no?, literalmente le está diciendo que "haga a exe"

##### Siempre debemos tener en cuenta QUÉ va a generar la función para así nombrar a nuestro objetivo.

## Dependencias:

Su nombre lo dice, literalmente tiene que decir de **qué _depende_ el _objetivo_ para ser ejecutado**, si esta dependencia no existe, no se ejecuta, punto.

En este caso y como se explica, un ```Main.c``` genera un ```Main.o``` que generará un ```exe```

![Generado](https://pbs.twimg.com/media/DbpzMaiX0AELPGn.png "Generado")

Simple lógica, si ```Main.c``` no existe, ```Main.o``` no existe. Y si ```Main.o``` no existe, ```exe``` no existirá.

Sigamos construyando el Makefile de ```Main.c``` para generar el ejecutable ```exe```. 

Como dijimos antes, ¿qué necesita el ```exe``` para existir?, inmediatamente el ```Main.o```

```make
exe: Main.o
    comando1
    comando2
    comando3
```

Listo, ya tenemos que si ```Main.o``` está, y las tareas del objetivo ```exe``` podrán ejecutarse.
Aunque en este escenario no existe ```Main.o``` todavía, si intentaramos ejecutar ```$ make exe``` nos lanzaría error y no correría las tareas. Por lo tanto vamos a generarlo en este mismo makefile.


```make
exe: Main.o
    comandoA1
    comandoA2
    comandoA3
    
Main.o: Main.c
    comandoB1
    comandoB2
    comandoB3
```

¿Qué pasó aquí?, pues, creamos una sección de tareas con un objetivo llamado ```Main.o``` que tiene una dependencia hacia ```Main.c```, es decir, si dicho código fuente no existe, no se podrá ejecutar.

Sin embargo, ¿Qué significa llamarlo ```Main.o```?, si recuerdan la regla establecida en la sección pasada, es porque estas tareas van a generar nuestro querido archivo .o

Tenemos entonces dos elementos llamados ```Main.o``` en nuestro makefile, un objetivo y una dependencia. Si el Makefile corre una tarea que tenga como _**dependencia**_ algo que no exista, pero que es también un _**objetivo**_, ejecutará primero las tareas de dicho objetivo. Make insinúa, gracias al nombre, que estas tareas me generarán ese eslabón perdido que me hacía falta. 

Sabemos que ```Main.o``` no existe, por lo tanto si ejecutamos nuestro famoso ```$ make exe```, correrá primero las tareas para generar ese archivo de código máquina y luego sí las acciones de ```exe```.

```console
$ make exe
ejecutando comandoB1
ejecutando comandoB2
ejecutando comandoB3
ejecutando comandoA1
ejecutando comandoA2
ejecutando comandoA3
```


En caso de existir, de todas formas me realizará estas tareas, ¿por qué?, de seguró querrá recompilar el programa en caso de haya habido algún cambio, así que prefiere hacer todo el backtracking

## Comandos.
Bueno, a todas estas, ¿qué es un comando?, como ya habrán insinuado, son básicamente instrucciones que se pasan a la terminal de Linux. "Ah ya, ¿eso es todo?", pues sí, literalmente, eso es todo. Lo único que deben tener en cuenta es que esos comandos cumplan con dicha tarea, de resto, pueden poner lo que quieran.

![before](https://pbs.twimg.com/media/DbqnskZW0AEQgEe.png "before")

```make
#Makefile:
exe: Main.o
    gcc -o exe Main.o

Main.o: Main.c
    gcc -c Main.c
```

```console
$ make exe
gcc -c Main.c
gcc -o exe Main.o
```

![after](https://pbs.twimg.com/media/DbqnnAYXkAAm2OI.png "after")

Con estas acciones incluso podemos ejecutar nuestro programa, siempre y cuando tengamos nuestras dependencias claras, en este caso, el programa no podrá ejecutarse si ```exe``` no existe. Y como no va a generar nada más de interés, podemos llamarla como queramos, en este caso si algo llamemosla "paquita".

```make
#Make:
paquita: exe
        ./exe
exe: Main.o
    gcc -o exe Main.o

Main.o: Main.c
    gcc -c main.c
```

```console
$ make paquita
gcc -c Main.c
gcc -o exe Main.o
./exe

--------------------

Hola Mundo
--------------------
```

El Makefile siempre intentará ejecutar la primera tarea que se le presenta al archivo, la que está en la parte superior, y claramente sus dependencias.

```console
$ make
gcc -c Main.c
gcc -o exe Main.o
./exe

--------------------

Hola Mundo
--------------------
```
Podemos incluso utilizar el Makefile para otras tareas, por ejemplo, nuestra carpeta quedó así después de compilar

![before](https://pbs.twimg.com/media/DbqnnAYXkAAm2OI.png "before")

Quiero limpiar esta cosa de cuantos *.o se me generen, fácil, Make al rescate.

```make
#Make:
paquita: exe
        ./exe
exe: Main.o
    gcc -o exe Main.o

Main.o: Main.c
    gcc -c main.c
Clean:
    rm -f *.o
```

```console
$ make
gcc -c Main.c
gcc -o exe Main.o
./exe

--------------------

Hola Mundo
--------------------
$ make Clean
rm -f *.o
```
![after](https://pbs.twimg.com/media/DbqoyC3XcAAVMIg.png "after")

Combinando estas dos últimas que dijimos, podemos incluso hacer que compile todo y luego ejecutar, creando una cabecera para darle instrucciones en cadena, a la cual llamaremos "all". El órden importa.

```make
all: exe Clean paquita

paquita: exe
        ./exe
        
exe: Main.o
    gcc -o exe Main.o
    
Main.o: Main.c
    gcc -c main.c
    
Clean:
    rm -f *.o
```

```console
$ make
gcc -c Main.c
gcc -o exe Main.o
rm -f *.o
./exe

--------------------

Hola Mundo
--------------------
```

## Pongamonos serios
Así de simple es usar Make, pero pongamonos serios y volvamos a la vida real, no vamos a ejecutar las cosas así, hagamos el Makefile compilando como siempre lo hemos hecho y con nombres más formales

```make
ejecutar: exe
        ./exe
        
exe: Main.c
    gcc Main.c -o exe
```

```console
$ make
gcc Main.c -o exe
./exe

--------------------

Hola Mundo
--------------------
```

No hay nada más bonito que la familiaridad y la limpieza. Y hablando de limpieza, queremos limpiar la pantalla, y una función que me elimine el o los ejecutables en caso de que así lo desee, quizás para propósitos de prueba o en caso de que no me pueda dar el lujo de siempre tenerlo ahí.

Para limpiar la pantalla utilizaremos el comando ```reset```, ya que, a diferencia de ```clear```, este comando sí limpia de verdad.

```make
ejecutar: exe
        reset
        ./exe
        
exe: Main.c
    gcc Main.c -o exe
    
clear:
    rm -f exe
```

```console
$ make
gcc Main.c -o exe
```
Se ejecuta reset y limpia toda la pantalla.
```console
./exe

--------------------

Hola Mundo
--------------------
```

Clear nunca se ejecutó porque nunca fue llamado, y no queremos llamarlo ahora, es una recomendación personal tener este tipo de tareas de limpieza en su archivo Make.

## Makes para los demás ejemplos

Vamos a seguir a partir de ahora la siguiente estructura para los makefiles

```make
ejecutar: ejecutable
ejecutable: codigo fuente
limpiar:
```

Y ahora crearemos makefiles para los ejemplos [**Ejemplo 2** y **Ejemplo 3**](https://github.com/juasmartinezbel/SISTEMAS_OPERATIVOS_CLASE/blob/master/Compilar%20en%20C/README.md) con las indicaciones dadas en esos archivos

Primero el **Ejemplo 2**
```make
ejecutar: exe2
        reset
        ./exe2
        
exe2: Mathfun.c
    gcc Mathfun.c -o exe2 -lm
    
clear:
    rm -f exe2
```

Nada raro aquí, únicamente añadimos el ```-lm``` al final del comando para compilar.

Ahora vamos con el **Ejemplo 3**
```make
ejecutar: exe3
        reset
        ./exe3
        
exe3: ultimate.c lib.c
    gcc lib.c ultimate.c -o exe3 -lm
    
clear:
    rm -f exe3
```

El cambio más drástico en este escenario es como las dependencias en ```exe3``` incluyen la librería ```lib.c``` y no solo el main **ultimate.c**, esto se debe hacer con cualquier programa que dependa de uno o más archivos, por eso es **dependencias** en prural.

## Un caso más fuerte

Tenemos el siguiente escenario:

![General](https://pbs.twimg.com/media/DbqwBldWsAAEBXz.png "General")

Tenemos una carpeta _General_ donde hay 3 sub carpetas, _1/_, _2/_ & _3/_ respectivamente, y en cada una está uno de los archivos que hemos compilado anteriormente. Deseamos, con el Makefile de la carpeta general, compilar los 3 archivos y dejarlos en nuestra carpeta raíz.

```make
ejecutar1: exe1
	./exe1
	
exe1: 1/Main.c
	gcc 1/Main.c -o exe1 


ejecutar2: exe2
	./exe2

exe2: 2/Mathfun.c 
	gcc 2/Mathfun.c -o exe2 -lm

ejecutar3: exe3
	./exe

exe3: 3/lib.c 3/ultimate.c
	gcc 3/lib.c 3/ultimate.c -o exe3 -lm

clear:
    rm -f exe1
    rm -f exe2
    rm -f exe3
```
(ignoré los reset por conveniencia)
Y podemos ejecutar todas las compilaciones una por una
```console
$ make exe1
gcc 1/Main.c -o exe1 
$ make exe2
gcc 2/Mathfun.c -o exe2 -lm
$ make exe3
gcc 3/lib.c 3/ultimate.c -o exe3 -lm
```
O simplemente tener todo en un llamado de tareas general.


```make
all: exe1 exe2 exe3 

ejecutar1: exe1
	./exe1
	
exe1: 1/Main.c
	gcc 1/Main.c -o exe1 
	
ejecutar2: exe2
	./exe2

exe2: 2/Mathfun.c 
	gcc 2/Mathfun.c -o exe2 -lm

ejecutar3: exe3
	./exe

exe3: 3/lib.c 3/ultimate.c
	gcc 3/lib.c 3/ultimate.c -o exe3 -lm

clear:
    rm -f exe1
    rm -f exe2
    rm -f exe3
```

```console
$ make
gcc 1/Main.c -o exe1
gcc 2/Mathfun.c -o exe2 -lm
gcc 3/lib.c 3/ultimate.c -o exe3 -lm
```

De cualquier forma, tendremos el siguiente resultado:
![resultado](https://pbs.twimg.com/media/DbqyeH9X0AALt60.png "resultado")

## Variables
El último tema que veremos hoy es el de variables, básicamente es que podremos asignar a varios de los nombres y acciones, una variable equivalente, a continuación un ejemplo sencillo.

```make
CC=gcc
EXECUTABLE= exe3
SOURCE= 3/ultimate.c 3/lib.c

all: exe1 exe2 exe3 

ejecutar1: exe1
	reset
	./exe1

exe1: 1/main.c
	gcc 1/main.c -o exe1 

ejecutar2: exe2
	reset
	./exe2

exe2: 2/mathfun.c 
	gcc 2/mathfun.c -o exe2 -lm


ejecutar3: $(EJECUTABLE)
	reset
	./$(EJECUTABLE)

$(EJECUTABLE): $(SOURCE)
	$(CC) $(SOURCE) -o $(EJECUTABLE) -lm
	
clear:
    rm -f exe1
    rm -f exe2
    rm -f $(EJECUTABLE)
```

Como podemos ver, utilizamos las variables para no solo algunos nombres, sino para referirnos a **objetivos**, **dependencias**, **comandos de la consola** y **referencias de archivos**

Para su práctica es necesario que implemente esta metodología de variables.

Hasta aquí el tema de Make, si tienen alguna duda pueden escribirme o [revisar este archivo en la página del curso](https://drive.google.com/file/d/0B421IcV2wvd8Z0xLeHMzR2hjWWc/view)
