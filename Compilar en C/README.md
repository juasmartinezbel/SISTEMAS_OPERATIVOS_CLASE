# Compilaciones en C
### Ejemplo 1 y Ensamblaje en C

Tenemos el siguiente archivo ```Main.c```

```c
#include <stdio.h>
int main(void) {
  printf("\n--------------------\n");
  printf("\nHola Mundo");
  printf("\n--------------------\n");
  return 0;
}
```
El cual se encuentra en una carpeta llamada _1/_, es decir, tendremos lo siguiente:
![alt_Text](https://pbs.twimg.com/media/DbpxRydXUAIpGHX.png "Main.c en Carpeta")

Si queremos compilar este archivo, únicamente es necesario escribir el siguiente comando en la consola de Linux


```bash
$ gcc Main.c -o exe
```


Y esto nos realiza la siguiente tarea

![alt text](https://pbs.twimg.com/media/DbptVF2XkAA_rIT.png "Main a exe")

![alt text](https://pbs.twimg.com/media/DbpxISvW4AABAeO.png "Main a exe Carpeta")



Sin embargo, todo esto es un proceso resumido, en general lo que sucede dentro de la máquina es el siguiente proceso:

![alt text](https://pbs.twimg.com/media/DbpuwfGX4AMak9w.jpg "Proceso General")

Vamos entonces a detallar un poco más el proceso de compilar ```Main.c```

```bash
$ gcc -c Main.c 
$ gcc -o exe Main.o
```

Lo que hace la primera linea es compilar el archivo ```Main.c``` generando el archivo ```Main.o```.

Una vez con este código de máquina presente, se podrá por fin crear el ejecutable ```exe``` de forma común y corriente. Es decir, se realiza;

![Generado](https://pbs.twimg.com/media/DbpzMaiX0AELPGn.png "Generado")
![Carpeta](https://pbs.twimg.com/media/Dbpzl3wWAAEvBH1.png "Generado")

Sin embargo, este proceso es trivial hoy día, pero nos servirá para entender más cosas próximamente.

Si estamos en un directorio llamado _general_ que contiene a la carpeta _1/_, y queremos hacer que el ```exe``` quede en esta posición, solo se debe compilar todo de la siguiente forma:

```bash
gcc 1/Main.c -o exe
```

![General](https://pbs.twimg.com/media/Dbp0PDMWkAAL7ra.png "General")

Esto aplica para cualquier directorio en el que esté, nada más hay que especificar la ruta de dónde se encuentra nuestro código fuente.

### Ejemplo 2

Ahora imaginese que tenemos el siguiente programa en C que llamaremos ```Mathfun.c``` cuyo único proposito es calcular un simple x^y.

```c
#include <stdio.h>
#include <math.h>
int main(void) {
  float x;
  float y;
  printf("Inserte x: ");
  scanf("%f", &x);
  printf("Inserte y: ");
  scanf("%f", &y);

  float p = pow(x,y);
  printf("\nResultado: %.2f\n",p);
  return 0;
}
```

Debido a que usa una librería externa, **algunos** compiladores intenterán ejecutarlo y fallarán debido a que usan una librería externa llamada ```math.h```  y saldrá el siguiente error

```bash
/tmp/ccF0gb0o.o: En la función `main':
mathfun.c:(.text+0x74): referencia a `pow' sin definir
collect2: error: ld returned 1 exit status
```

Esto debido a que se debe compilar _mencionando_ la libería explicitamente en el compilador, en este caso la _"library: math"_ se llamará con el nombre ```lm```

```bash
$ gcc Mathfun.c -o exe -lm
```

### Ejemplo 3

Tenemos un archivo en C que llamaremos ```Ultimate.c```

```c
#include "lib.h"
int main(void) {
  float p = spec_pow();
  printf("\nResultado: %.2f\n", p);
  return 0;
}
```

Como podemos ver, requiere una librería personal llamada ```lib.h```, la cual crearemos en el mismo directorio y tendrá el siguiente contenido

```c
#include <stdio.h>
#include <math.h>

float spec_pow(void);
```

Toda cabecera (*.h) necesita su fuente (*.c), en este caso ```lib.c```, el cual tendrá el siguiente contenido,

```c
#include <stdio.h>
#include <math.h>

float spec_pow(void){
  float x;
  float y;
  printf("Inserte x: ");
  scanf("%f", &x);
  printf("Inserte y: ");
  scanf("%f", &y); 

  float p = pow(x,y)*(x-y);
  return p;
}
```

Ya que este programa utiliza la librería ```math.c``` para calcular potencias otra vez, es necesario incluir el ```-lm``` como vimos anteriormente, y como tiene una librería personal, lo único que hay que hacer es compilar el ```lib.c``` junto al código fuente del programa principal.

```bash
$ gcc lib.c ultimate.c -o exe -lm
```
