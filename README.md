# TP0: Contador de Palabras #
**Autor: Daniel Alejandro Lovera López**  
**N° padrón: 103442**  
**[https://github.com/DanieLovera/tp0](https://github.com/DanieLovera/tp0)**  

---
### INTRODUCCION ###  

- [x] Paso 0
- [x] Paso 1
- [x] Paso 2
- [ ] Paso 3
- [ ] Paso 4
- [ ] Paso 5

---
### DESARROLLO ###   
En esta sección se detallan los pasos seguidos durante el desarrollo del trabajo.

### Paso 0: Entorno de Trabajo ###  
Se crea un programa simple que muestra el mensaje "Hola Mundo" el cual se ejecuta con y sin valgrind, los resultados obtenidos fueron:

1. **Ejecución sin valgrind**  
![Ejecucion sin valgrind](./screenshots/no_valgrind_exe.png)  
2. **Ejecución con valgrind**  
![Ejecucion con valgrind](./screenshots/valgrind_exe.png)  

**Preguntas Teóricas**

**a. ¿Para que sirve Valgrind? ¿Cuales son sus opciones mas comunes?**   
> **Valgrind** es un conjunto de herramientas que pueden ser utilizadas para *debugging* (corrección de errores) o *profiling* (análisis de rendimiento) de un programa, su ejecución en línea de comando es de la forma: valgrind --tool\<toolname> ./programa_ejecutable.  
 
Las herramientas mas comunes son:  

- **Memcheck:** 
Detecta errores relacionados a la memoria. Posiblemente sea la opción mas común pues es la herramienta que se utiliza por defecto cuando se ejecuta un programa  con valgrind y ademas es muy común en C/C++ que los programadores olviden liberar memoria solicitada dinamicamente (problema grave que imposibilita reutilizar estas porciones de memoria mientras el programa se mantenga en ejecución).  

- **Cachegrind:**
Herramienta de profiling que ayuda al programador a conocer como se comportan las memorias caches del procesador en la ejecución de un programa.  

Otras herramientas menos comunes o al menos desconocidas para el autor de este texto son:
- **Callgrind:**
Recolecta información sobre los llamados a funciones en un programa.  
 
- **Helgrind:**
Detecta errores en la ejecución de threads de un programa. 
 
- **DRD:**
Otra opción para detección de errores en threads.  

- **Massif:**
Analiza el uso del Heap (area de memoria dinámica).
  
- **DHAT:**
Es otra herramienta para analizar el Heap.  

**b. ¿Que representa sizeof()? ¿Cual seria el valor de salida de sizeof(char) y sizeof(int)?**  
> **sizeof()** es un operador de C/C++ que permite calcular el tamaño en bytes de los tipos de datos definidos por el lenguaje o por el usuario. El valor de **sizeof(char)** sera 1 (1 byte) y el de **sizeof(int)** dependerá de la arquitectura o del compilador, 
lo mas común es que sea 4 (4 bytes).

**c. ¿El sizeof() de una struct de C es igual a la suma del sizeof() de cada uno de sus elementos?. Justifique mediante un ejemplo.**  
> No necesariamente, en una estructura pueden haber datos de cualquier tipo de primitivo e incluso otra estructura por lo cual el tamaño dependerá de la arquitectura y además de como el compilador decida colocar las variables en memoria, puede tomar la decisión de alinear los datos a posiciones de memoria multiplos de cada tipo de dato y trabajar con *paddings*(desperdicios) lo cual en particular simplificará el código generado por el compilador pues no necesita 
hacer calculos especiales para leer un dato, o podría no alinearlos y trabajar sin *padding* (si la arquitectura lo permite) para ahorrar espacio en memoria. En conclusión el **sizeof** de una estructura depende de varias variables, no es fijo.  

![Ejecucion sin valgrind](./screenshots/structs_example.png)   

Tomando en cuenta el caso del compilador sin compactar los datos, los resultados serían:  

- **sizeof(struct foo)** = (1 + 3 bytes de padding) + 4 + (1 + 3 bytes de padding) = **12**  
La explicación a esto es que un entero no podría ser colocado en la posición de memoria siguiente al char porque deben estar alineados a posiciones de memoria multiplos de 4. Considerando que el char estuviera en la posicion 0, el int debe ir en la posición 4 y dejar el padding en las posiciones 1,2 y 3, el último char tambien deja un padding de 3 bytes.

- **sizeof(struct bar)** = (1 + 1 + 2 bytes de padding) + 4 = **8**  
Este caso es similar al anterior, pero los char a y b se pueden colocar uno al lado de otro y esto desperdiciaría menos memoria que caso anterior.

**d. Investigar la existencia de los archivos estandar: STDIN, STDOUT, STDERR. Explicar brevemente su uso y como redirigirlos en caso de ser necesario (caracteres > y <) y como conectar la salida estándar de un proceso a la entrada estándar de otro con un pipe (caracter |)**  
> Todos los programas que se ejecuten en UNIX's comienzan con 3 canales abiertos para transferencias de datos, es decir uno para la entrada, otro de salida y el último es de salida tambien pero en caso de errores en la ejecución del programa, al término del programa estos canales se cierran automáticamente. Los archivos estandar STDIN, STDOUT, STDERR son descriptores de archivos (son valores de tipo int en C/C++ que sirven como clave para que internamente se reconozca a que tendrá acceso el proceso que se esta ejecutando), estos son estándar porque los valores que se le asignan a cada uno son: STDIN = 0, STDOUT = 1, STDOUT = 2.
Por defecto la entrada estándar representa a los flujos de datos proveniente del teclado y la salida estandár común/errores representa al flujo de datos que se dirige a la pantalla y se utilizan para ingresar datos que pueda requerir un programa o para extraerlos de el.  

Ejemplos de como redirigir los archivos estandar por CLI:

1. **echo "Hola Mundo" > example.txt** 
2. **grep < example "Hola"** 
3. **ls non_existing_command >> error_example.txt**
4. **echo "Hola Mundo" | grep "Hola"**  

### Paso 1: SERCOM - Errores de generación y normas de programación ###
Se entregaron los módulos correspondientes a la primera entrega en el SERCOM, este reportó multiples errores por fallas en compilación y discrepancias con respecto a las normas de cpplint.  

**Documentación de errores**  
Descripción de errores generados por el SERCOM.

**a. Problemas de estilo (cpplint)**  
```  
1.    /task/student//source_unsafe/paso1_wordscounter.c:27:  Missing space before ( in while(  [whitespace/parens] [5]
2.    /task/student//source_unsafe/paso1_wordscounter.c:41:  Mismatching spaces inside () in if  [whitespace/parens] [5]
3.    /task/student//source_unsafe/paso1_wordscounter.c:41:  Should have zero or one spaces inside ( and ) in if  [whitespace/parens] [5]
4.    /task/student//source_unsafe/paso1_wordscounter.c:47:  An else should appear on the same line as the preceding }  [whitespace/newline] [4]
5.    /task/student//source_unsafe/paso1_wordscounter.c:47:  If an else has a brace on one side, it should have it on both  [readability/braces] [5]
6.    /task/student//source_unsafe/paso1_wordscounter.c:48:  Missing space before ( in if(  [whitespace/parens] [5]
7.    /task/student//source_unsafe/paso1_wordscounter.c:53:  Extra space before last semicolon. If this should be an empty statement, use {}  instead.  [whitespace/semicolon] [5]
8.    /task/student//source_unsafe/paso1_main.c:12:  Almost always, snprintf is better than strcpy  [runtime/printf] [4]
9.    /task/student//source_unsafe/paso1_main.c:15:  An else should appear on the same line as the preceding }  [whitespace/newline] [4]
10.   /task/student//source_unsafe/paso1_main.c:15:  If an else has a brace on one side, it should have it on both  [readability/braces] [5]
11.   /task/student//source_unsafe/paso1_wordscounter.h:5:  Lines should be <= 80 characters long  [whitespace/line_length] [2]
    Done processing /task/student//source_unsafe/paso1_wordscounter.c
    Done processing /task/student//source_unsafe/paso1_main.c
    Done processing /task/student//source_unsafe/paso1_wordscounter.h
    Total errors found: 11
```  
Los **problemas encontrados** son bastante descriptivos ya que informan la ruta del archivo que contiene el problema, la línea de código de donde se encuentra y ademas el detalle del mismo. **Ejemplo** de como se lee una línea de error:  
>  ruta_archivo/nombre_archivo.[extensión_archivo]:[línea_código]: [Descripción]  

A continuación se detallan los errores de la forma [**Descripción**/**Código que lo genera**], estos fueron enumerados para mayor claridad:  
1. **Missing space before ( in while( / while(state != STATE_FINISHED)**: Indica la **falta de un espacio** antes del párentesis en el while, por lo cual el código que lo soluciona sería while (state != STATE_FINISHED).  
2. **Mismatching spaces inside () in if / if (  c == EOF) {**: Indica la falta de **paridad** entre los espacios adentro del paréntesis del  if, por lo cual una de las formas de solucionarlo sería if (  c == EOF  ), de forma tal que sean simétricos los espacios en blanco.  
3. **Should have zero or one spaces inside ( and ) in if / (  c == EOF)**: Indica que debe haber **ningún** o a lo sumo **un** espacio dentro del paréntesis del if, por lo cual una forma de solucionarlo sería if (c == EOF).  
4. **An else should appear on the same line as the preceding } / }\n else if (state == STATE_IN_WORD) {**: Indica que **un else debe aparecer** en la misma línea que la llave que lo precede, por lo cual la forma de solucionarlo es } else if (state == STATE_IN_WORD) {, es decir deben estar alineados la llave y el else.  
5. **If an else has a brace on one side, it should have it on both / }\n else if (state == STATE_IN_WORD) {**: Indica que **si un else** tiene una llave de un lado, **entonces** debe tenerlo tambien en el lado opuesto, por lo cual la forma de solucionarlo es igual al anterior (4), ya que en este caso la llave que no empareja es la que precede al else. *Observación: localmente cpplint junto con el script provisto por la cátedra no reconoce este como un error.*  
6. **Missing space before ( in if( / if(strchr(delim_words, c) != NULL)**: Este error es idéntico al (1), con la salvedad de que es un if y no un while, por lo cual la solución es la misma.  
7. **Extra space before last semicolon. If this should be an empty statement, use {}  instead. / return next_state ;**: Indica que hay un espacio extra antes del último punto y coma, por lo cual el código que lo soluciona sería return next_state;  
8. **Almost always, snprintf is better than strcpy  [runtime/printf] / strcpy(filepath, argv[1])**: Indica que la función snprintf es mejor que strcpy, esto es porque strcpy es una función insegura ya que no tiene control de cuantos datos escribe en un buffer. La solución es usar la funcion que recomienda o usar strncpy que soluciona el problema del tamaño del buffer tambien.  
9. **An else should appear on the same line as the preceding } / }\n else {**: Mismo error que el (4).  
10. **If an else has a brace on one side, it should have it on both / }\n else {**: Mismo error que en el (5).  
11. **Lines should be <= 80 characters long / //Tipo wordscounter_t: almacena la cantidad de palabras procesadas de un archivo**: Indica que las líneas de código tienen que contener menos de 81 caracteres. La solución es separar el comentario en dos líneas.  

**b. Problemas de generación del ejecutable**  

``` 
cc -Wall -Werror -pedantic -pedantic-errors -O3 -ggdb -DDEBUG -fno-inline -D _POSIX_C_SOURCE=200809L -Dwrapsocks=1 -std=c11 -o paso1_main.o -c paso1_main.c
paso1_main.c: In function 'main':
1. paso1_main.c:22:9: error: unknown type name 'wordscounter_t'
   22 |         wordscounter_t counter;
      |         ^~~~~~~~~~~~~~
2. paso1_main.c:23:9: error: implicit declaration of function 'wordscounter_create' [-Wimplicit-function-declaration]
   23 |         wordscounter_create(&counter);
      |         ^~~~~~~~~~~~~~~~~~~
3. paso1_main.c:24:9: error: implicit declaration of function 'wordscounter_process' [-Wimplicit-function-declaration]
   24 |         wordscounter_process(&counter, input);
      |         ^~~~~~~~~~~~~~~~~~~~
4. paso1_main.c:25:24: error: implicit declaration of function 'wordscounter_get_words' [-Wimplicit-function-declaration]
   25 |         size_t words = wordscounter_get_words(&counter);
      |                        ^~~~~~~~~~~~~~~~~~~~~~
5. paso1_main.c:27:9: error: implicit declaration of function 'wordscounter_destroy' [-Wimplicit-function-declaration]
   27 |         wordscounter_destroy(&counter);
      |         ^~~~~~~~~~~~~~~~~~~~
make: *** [/task/student/MakefileTP0:144: paso1_main.o] Error 1
```  
En este caso es el compilador o linker los que informan sobrelos errores de una forma similar al cpplint, estos errores muchas veces son autodescriptivos, e indican el archivo que contiene el problema, la línea en que ocurre, en que caracter de la línea ocurre, y si el problema es un error o un warning junto con la descripción del mismo. **Ejemplo** de como se lee una línea de error:  
>  nombre_archivo.[extensión_archivo]:[línea_código]:[número_caracter]:[warning/error]:[Descripción]  

A continuación se detallan los errores, estos fueron enumerados para mayor claridad y no forman parte de la salida orginal.  
1. **unknown type name 'wordscounter_t'**: Indica que el tipo de dato **wordscounter_t** es desconocido, nunca fue definido por lo tanto el compilador no puede saber cuando espacio necesita reservar en el stack y no le sera posible generar código objeto. Este es un error que detecta el compilador.
2. **implicit declaration of function 'wordscounter_create'**: Indica que hay una declaración implicita de la funcion **wordscounter_create**, es decir que no se puede generar código ejecutable porque no le hemos asegurado al compilador la existencia de dicha función. Este es un error que detecta el compilador.  
3. **implicit declaration of function 'wordscounter_process'**: Es el mismo error que en (2).  
4. **implicit declaration of function 'wordscounter_get_words'**: Es el mismo error que en (2) y (3).  
5. **implicit declaration of function 'wordscounter_destroy'**: Es el mismo error que en (2), (3) y (4).  

**c. ¿El sistema reportó algún WARNING? ¿Por qué?**  Descomprimiendo el codigo 'source_unsafe.zip'...
Archive:  source_unsafe.zip
  inflating: source_unsafe/paso2_main.c
  inflating: source_unsafe/paso2_wordscounter.c
  inflating: source_unsafe/paso2_wordscounter.h
Compilando el codigo...
cc -Wall -Werror -pedantic -pedantic-errors -O3 -ggdb -DDEBUG -fno-inline -D _POSIX_C_SOURCE=200809L -Dwrapsocks=1 -std=c11 -o paso2_wordscounter.o -c paso2_wordscounter.c
In file included from paso2_wordscounter.c:1:
paso2_wordscounter.h:7:5: error: unknown type name 'size_t'
    7 |     size_t words;
      |     ^~~~~~
paso2_wordscounter.h:20:1: error: unknown type name 'size_t'
   20 | size_t wordscounter_get_words(wordscounter_t *self);
      | ^~~~~~
paso2_wordscounter.h:1:1: note: 'size_t' is defined in header '<stddef.h>'; did you forget to '#include <stddef.h>'?
  +++ |+#include <stddef.h>
    1 | #ifndef __WORDSCOUNTER_H__
paso2_wordscounter.h:25:49: error: unknown type name 'FILE'
   25 | void wordscounter_process(wordscounter_t *self, FILE *text_file);
      |                                                 ^~~~
paso2_wordscounter.h:1:1: note: 'FILE' is defined in header '<stdio.h>'; did you forget to '#include <stdio.h>'?
  +++ |+#include <stdio.h>
    1 | #ifndef __WORDSCOUNTER_H__
paso2_wordscounter.c:17:8: error: conflicting types for 'wordscounter_get_words'
   17 | size_t wordscounter_get_words(wordscounter_t *self) {
      |        ^~~~~~~~~~~~~~~~~~~~~~
In file included from paso2_wordscounter.c:1:
paso2_wordscounter.h:20:8: note: previous declaration of 'wordscounter_get_words' was here
   20 | size_t wordscounter_get_words(wordscounter_t *self);
      |        ^~~~~~~~~~~~~~~~~~~~~~
paso2_wordscounter.c: In function 'wordscounter_next_state':
paso2_wordscounter.c:30:25: error: implicit declaration of function 'malloc' [-Wimplicit-function-declaration]
   30 |     char* delim_words = malloc(7 * sizeof(char));
      |                         ^~~~~~
paso2_wordscounter.c:30:25: error: incompatible implicit declaration of built-in function 'malloc' [-Werror]
paso2_wordscounter.c:5:1: note: include '<stdlib.h>' or provide a declaration of 'malloc'
    4 | #include <stdbool.h>
  +++ |+#include <stdlib.h>
    5 |
cc1: all warnings being treated as errors
make: *** [/task/student/MakefileTP0:144: paso2_wordscounter.o] Error 1

real    0m0.020s
user    0m0.015s
sys     0m0.004s
[Error] Fallo la compilacion del codigo en 'source_unsafe.zip'. Codigo de error 2
> El sistema no reportó ningún warning, todos fueron errores, sin embargo los errores del 2 al 5 en realidad son warnings que se estan considerando como errores porque así se le indica al compilador que lo haga (**flag Werror**), es decir realmente no serían un inconveniente para que se generará el código objeto, no obstante los warnings son posibles errores en tiempo de ejecución y no es recomendable dejarlos pasar pues es mas fácil corregir errores en compilación que hacerlo con un debugger en ejecución.  

### Paso 2: SERCOM - Errores de generación 2 ###  
Se entregaron los módulos correspondientes a la segunda entrega en el SERCOM, este no reporto errores de estilo por cpplint, pero incrementaron lo errores por parte del compilador.  

**Documentación de errores**  

**a. Descripción de los cambios realizados con respecto a la versión del paso 1.**  
  
**Ejecución del comando diff**  
  
![Ejecucion del comando diff](./screenshots/diff_command.png)  
  
Se puede observar como en cada archivo .c se incluyeron los .h que contenian las definiciones de funciones requeridas por el compilador, se corrigieron los espacios en blanco extras, adicionalmente se alinearon los else e ifs con sus llaves predecesoras, se redujo la cantidad de carácteres contenidos en el comentario del archivo de cabecera a menos de 81 y se reemplazo la función insegura strcpy por memcpy. Estas modificaciones corrigieron los problemas de estilos reportados por cpplint. **Ejemplo** de como interpretar el comando **diff**:    
> **[num_lineas_archivo1][acción][num_lineas_archivo2]**. Siendo la acción alguno de los siguientes caractéres: **a**(add), **c**(change) o **d**(delete).  
> Entonces la primera indicación del comando diff se leería como:
> Después de la línea 3 del del primer archivo se debe cambiar la función strcpy por memcpy para que ambos archivos sean iguales en la línea 4.  

**b. Correcta ejecución de las normas de programación (cpplint).**  
  
``` 
Done processing /task/student//source_unsafe/paso2_wordscounter.h
Done processing /task/student//source_unsafe/paso2_main.c
Done processing /task/student//source_unsafe/paso2_wordscounter.c
```  
**c. Problemas de generación del ejecutable**  

```
Compilando el codigo...
cc -Wall -Werror -pedantic -pedantic-errors -O3 -ggdb -DDEBUG -fno-inline -D _POSIX_C_SOURCE=200809L -Dwrapsocks=1 -std=c11 -o paso2_wordscounter.o -c paso2_wordscounter.c
In file included from paso2_wordscounter.c:1:
1. paso2_wordscounter.h:7:5: error: unknown type name 'size_t'
    7 |     size_t words;
      |     ^~~~~~
2. paso2_wordscounter.h:20:1: error: unknown type name 'size_t'
   20 | size_t wordscounter_get_words(wordscounter_t *self);
      | ^~~~~~
3. paso2_wordscounter.h:1:1: note: 'size_t' is defined in header '<stddef.h>'; did you forget to '#include <stddef.h>'?
  +++ |+#include <stddef.h>
    1 | #ifndef __WORDSCOUNTER_H__
4. paso2_wordscounter.h:25:49: error: unknown type name 'FILE'
   25 | void wordscounter_process(wordscounter_t *self, FILE *text_file);
      |                                                 ^~~~
5. paso2_wordscounter.h:1:1: note: 'FILE' is defined in header '<stdio.h>'; did you forget to '#include <stdio.h>'?
  +++ |+#include <stdio.h>
    1 | #ifndef __WORDSCOUNTER_H__
6. paso2_wordscounter.c:17:8: error: conflicting types for 'wordscounter_get_words'
   17 | size_t wordscounter_get_words(wordscounter_t *self) {
      |        ^~~~~~~~~~~~~~~~~~~~~~
In file included from paso2_wordscounter.c:1:
7. paso2_wordscounter.h:20:8: note: previous declaration of 'wordscounter_get_words' was here
   20 | size_t wordscounter_get_words(wordscounter_t *self);
      |        ^~~~~~~~~~~~~~~~~~~~~~
8. paso2_wordscounter.c: In function 'wordscounter_next_state':
paso2_wordscounter.c:30:25: error: implicit declaration of function 'malloc' [-Wimplicit-function-declaration]
   30 |     char* delim_words = malloc(7 * sizeof(char));
      |                         ^~~~~~
9. paso2_wordscounter.c:30:25: error: incompatible implicit declaration of built-in function 'malloc' [-Werror]
10. paso2_wordscounter.c:5:1: note: include '<stdlib.h>' or provide a declaration of 'malloc'
    4 | #include <stdbool.h>
  +++ |+#include <stdlib.h>
    5 |
cc1: all warnings being treated as errors
make: *** [/task/student/MakefileTP0:144: paso2_wordscounter.o] Error 1
```
A continuación se detallan los errores, estos fueron enumerados para mayor claridad y no forman parte de la salida orginal.  
1. **unknown type name 'size_t'**: Indica que el tipo de dato size_t es desconocido, esto es un error que detecta el compilador ya que no puede reservar espacio en memoria para la variable.  
2. **unknown type name 'size_t'**: Es el mismo error que (1), mientras no encuentre una definición seguira ocurriendo.  
3. **'size_t' is defined in header '<stddef.h>'; did you forget to '#include stddef.h'**: En este caso el compilador detecto que se esta haciendo uso de un tipo de dato que se encuentra definido en la libreria **stddef.h** y esta haciendo una forma de recordatorio para que sea incluido.  
4. **unknown type name 'FILE'**: Es el mismo error que (1) pero en este caso el compilador no encuentra la definición de FILE.  
5. **'FILE' is defined in header '<stdio.h>'; did you forget to '#include stdio.h'**: Es la misma sugerencia que (3) pero para el dato **FILE** de la libreria **stdio.h**  
6. **conflicting types for 'wordscounter_get_words'**: Indica un problema en los tipos de datos declarados en la funcion **wordscounter_get_words** en este caso el tipo es el **size_t**, el compilador no conoce el tipo de dato de retorno, el error del punto (1) empezo a aparecer en varias partes del código.
7. **previous declaration of 'wordscounter_get_words' was here**: El compilador esta indicando el lugar en donde se declaro la función con el su llamado en el archivo .c.  
8. **implicit declaration of function 'malloc'**: Indica una declaracion implicita de la función **malloc**, esto es porque no se realizo la declaración de la función y el compilador no tiene asegurada su existencia por lo tanto no compilara el código. Esta siendo considerado como error por el compilador pero es un warning, aunque no le aseguremos la existencia de la función el código podría funcionar y en caso de que realmente no exista una función con esa firma fallar en etapa de **linkeo**.  
9. **incompatible implicit declaration of built-in function 'malloc'**: Indica que la misma función del error (8) genera otro problema, el compilador no sabe si la definición implícita empareja con el tipo de dato al que se quiere asignar (char*) en el momento de retornar. Este igualmente es un warning tratado como error.    
10. **include '<stdlib.h>' or provide a declaration of 'malloc'**: El compilador esta nuevamente sugiriendo que incluyamos la libreria que contiene la declaración de **malloc**.  











