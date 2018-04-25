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

Y esto nos realiza la siguiente tarea

```bash
gcc Main.c -o exe
```

![alt text](https://pbs.twimg.com/media/DbptVF2XkAA_rIT.png "Main a exe")

![alt text](https://pbs.twimg.com/media/DbpxISvW4AABAeO.png "Main a exe Carpeta")



Sin embargo, todo esto es un proceso resumido, en general lo que sucede en pantalla es el siguiente proceso:

![alt text](https://pbs.twimg.com/media/DbpuwfGX4AMak9w.jpg "Proceso General")

Vamos entonces a detallar un poco más el proceso de compilar ```Main.c```

```bash
gcc -c Main.c 
gcc -o exe Main.o
```
Lo que hace la primera linea es compilar un archivo ```Main.o```, del cual el ejecutable ```exe``` **dependerá** (palabra clave), para ser generado y poder compilar todo de forma común y corriente. Es decir, tendremos lo siguiente

![Generado](https://pbs.twimg.com/media/DbpzMaiX0AELPGn.png "Generado")
![Carpeta](https://pbs.twimg.com/media/Dbpzl3wWAAEvBH1.png "Generado")

Sin embargo, este proceso es trivial hoy día, sin embargo nos servirá para entender más cosas próximamente.

Si estamos en un directorio llamado _general_ que contiene a _1_, y queremos escribir el ```exe``` para aquí, solo se debe compilar, solo es cuestión de escribir

```bash
gcc 1/Main.c -o exe
```

![General](https://pbs.twimg.com/media/Dbp0PDMWkAAL7ra.png "General")
### Ejemplo 2

Ahora imaginese que tenemos el siguiente programa en C que llamaremos ```Mathfun.c```

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

Esto debido a que se debe compilar añadiendo la libería explicitamente en el compilador, en este caso la _"library math"_ se llamará con el nombre ```lm```

```bash
gcc Mathfun.c -o exe -lm
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
Toda cabecera (*.h) necesita su fuente (*c) la cual tendrá el siguiente contenido, un ```lib.c```

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

Ya que este programa utiliza la librería ```math.c```, es necesario incluir el ```-lm``` como vimos anteriormente, y como tiene una librería personal, lo único que hay que hacer es compilar el ```lib.c```

```bash
gcc lib.c ultimate.c -o exe -lm
```
