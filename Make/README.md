# Make
Make es una utilidad para automatizar la compilación de programas y librerías, así como automatizar la ejecución de comandos.

Un archivo de nombre ```Makefile```, ```makefile``` o ```GNUmakefile``` (sin extensión) se encargará de ejecutar las acciones presentes en el directorio (o fuera de este si así lo especifican los comandos). Las tareas de make se ejecutarán en base a la siguiente estructura.

```make
Objetivo: Dependencias
    Comando1
    Comando2
    Comando3
```

Vamos a explicar cada uno detalladamente.

## Objetivo
Objetivo es lo que queremos que se genere de ejecutar las acciones, así como el llamado. 

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
Para llamar la función suma, lo único que debemos hacer es escribir ```suma``` en otro lado, los **objetivos** funcionan como nombres de funciones. Si queremos llamar a un objetivo, lo único que debemos hacer es escribir el nombre de este objetivo. Lo único que debemos hacer es escribir en el directorio donde está el Makefile :

```bash
$ make objetivo
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

```bash
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

Sigamos construyando el Makefile de ```Main.c```. Como dijimos antes, ¿qué necesita el ```exe``` para existir?, inmediatamente el ```Main.o```

```make
exe: Main.o
    comando1
    comando2
    comando3
```

Listo, ya tenemos que si ```Main.o``` está, y el objetivo ```exe``` podrá servir. 
Pero en este escenario, no existe ```Main.o```, no puede ejecutarse, así que vamos a generarlo en este Make.


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

¿Qué pasó aquí?, pues, creamos un objetivo llamado ```Main.o``` que tiene una dependencia llamada ```Main.c```, es decir, si ```Main.c``` no existe, no se podrá ejecutar.

Sin embargo, ¿Qué significa llamarlo ```Main.o```?, esto significa que, si llamamos a ```$ make exe``` y ```Main.o``` **NO** existe, Make detecta que hay un objetivo que se llama **Exactamente igual que esta dependencia**, esto quiere decir que esta nueva función DEBE generar esta dependencia perdida, así que tendremos lo siguiente si ```Main.o``` no existe.

```bash
$ make exe
ejecutando comandoB1
ejecutando comandoB2
ejecutando comandoB3
ejecutando comandoA1
ejecutando comandoA2
ejecutando comandoA3
```


Si existe, tendremos lo mismo que antes

```bash
$ make exe
ejecutando comandoA1
ejecutando comandoA2
ejecutando comandoA3
```

