# TP0: Contador de Palabras #
**Autor: Daniel Alejandro Lovera López**  
**N° padrón: 103442**  
**[https://github.com/DanieLovera/tp0](https://github.com/DanieLovera/tp0)**  

---
### INTRODUCCION ###  

- [x] Paso 0
- [ ] Paso 1
- [ ] Paso 2

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

- **Problemas de estilo (cpplint)**  
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
Los **problemas encontrados** fueron numerados para mayor claridad, estos son bastante descriptivos ya que informan la ruta del archivo que tiene el problema, la línea de código de donde se encuentra y ademas el detalle del mismo. **Ejemplo** de como se lee una línea de error:  
>  ruta_archivo/nombre_archivo.[extensión_archivo]:[línea_código]: [Descripción]  

A continuación se detallan los errores de la forma [Descripción/Código que lo genera]:  
1. **Missing space before ( in while( / while(state != STATE_FINISHED)**: Indica la **falta de un espacio** antes del párentesis en el while, por lo cual el código que lo soluciona sería while (state != STATE_FINISHED).  
2. **Mismatching spaces inside () in if / if (  c == EOF) {**: Indica la falta de **paridad** entre los espacios adentro del paréntesis del  if, por lo cual una de las formas de solucionarlo sería if (  c == EOF  ), de forma tal que sean simétricos los espacios en blanco.  
3. **Should have zero or one spaces inside ( and ) in if / (  c == EOF)**: Indica que debe haber **ningún** o a lo sumo **un** espacio dentro del paréntesis del if, por lo cual una forma de solucionarlo sería if (c == EOF).  
4. **An else should appear on the same line as the preceding } / }\n else if (state == STATE_IN_WORD) {**: Indica que **un else debe aparecer** en la misma línea que la llave que lo precede, por lo cual la forma de solucionarlo es } else if (state == STATE_IN_WORD) {, es decir deben estar alineados la llave y el else.  
5. **If an else has a brace on one side, it should have it on both / }\n else if (state == STATE_IN_WORD) {**: Indica que **si un else** tiene una llave de un lado, **entonces** debe tenerlo tambien en el lado opuesto, por lo cual la forma de solucionarlo es igual al anterior (4), ya que en este caso la llave que no empareja es la que precede al else. *Observación: localmente cpplint junto con el script provisto por la cátedra no reconoce este como un error.*  
6. **Missing space before ( in if( / if(strchr(delim_words, c) != NULL)**: Este error es idéntico al (1), con la salvedad de que es un if y no un while, por lo cual la solución es la misma.  
7. **Extra space before last semicolon. If this should be an empty statement, use {}  instead. / return next_state ;**: Indica que hay un espacio extra antes del último punto y coma, por lo cual el código que lo soluciona sería return next_state;  
8. **Almost always, snprintf is better than strcpy  [runtime/printf] / strcpy(filepath, argv[1])**: Indica que la función snprintf es mejor que strcpy, esto es porque strcpy es una función insegura ya que no tiene control de cuantos datos escribe en un buffer. La solución es usar la funcion que recomienda o usar strncpy que soluciona el problema del tamaño del buffer tambien.  
9. **An else should appear on the same line as the preceding } / }\n else { **: Mismo error que el (4).  
10. **If an else has a brace on one side, it should have it on both / }\n else {**: Mismo error que en el (5).  
11. **Lines should be <= 80 characters long / //Tipo wordscounter_t: almacena la cantidad de palabras procesadas de un archivo.**: Indica que las líneas de código tienen que contener menos de 81 caracteres. La solución es separar el comentario en dos líneas.




