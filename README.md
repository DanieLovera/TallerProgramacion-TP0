# Tp0: Contador de Palabras #
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

### Paso 0: Entorno de Trabajo ###  
Se crea un programa simple que muestra el mensaje "Hola Mundo" el cual se ejecuta con y sin valgrind, los resultados obtenidos fueron:

1. **Ejecución sin valgrind**  
![Ejecucion sin valgrind](./screenshots/no_valgrind_exe.png)  
2. **Ejecución con valgrind**  
![Ejecucion con valgrind](./screenshots/valgrind_exe.png)  

**Preguntas Teóricas**

**a. ¿Para que sirve Valgrind? ¿Cuales son sus opciones mas comunes?** (REVISAR ORTOGRAFIA)  
> **Valgrind** es un conjunto de herramientas que pueden ser utilizadas para *debugging* (corrección de errores) o *profiling* (análisis de rendimiento) de un programa.  
> Las *tools* (herramientas) mas comunes son:  

- **Memcheck:** 
>> Detecta errores relacionados a la memoria. Posiblemente sea la opción mas común pues es la herramienta que se utiliza por defecto cuando se ejecuta un programa  con valgrind y ademas es muy comun en C/C++ que los programadores olviden liberar memoria solicitada dinamicamenta (problema grave que imposibilita reutilizar estas porciones de memoria mientras el programa se mantenga 
en ejecución).  

- **Cachegrind:**
>> Herramienta de profiling que ayuda al programador a conocer como comportan las memorias caches del procesador en la 
ejecucion de un programa.  

Otras herramientas menos comunes o al menos desconocidas para el autor de este texto son:
- **Callgrind:**
>> Recolecta información sobre los llamados a funciones en un programa.  
- **Helgrind:**
>> Detecta errores en la ejecucion de threads de un programa.  
- **DRD:**
>> Otra opcion para deteccion de errores en threads.  
- **Massif:**
>> Analiza el uso del Heap (area de memoria dinamica).  
- **DHAT:**
>> Es otra herramienta para analizar el Heap.  

La ejecucion de valgrind en linea de comando es de la forma: valgrind --tool<toolname> ./programa_ejecutable

b. ¿Que representa sizeof()? ¿Cual seria el valor de salida de sizeof(char) y sizeof(int)?
sizeof() es un operador de C/C++ que permite calcular el tamaño en bytes de los tipos de datos definidos por el lenguaje 
o por el usuario. El valor de sizeof(char) sera 1 (1 byte) y el sizeof(int) dependera de la arquitectura o del compilador, 
lo mas comun es que sea 4 (4 bytes).

c. ¿El sizeof() de una struct de C es igual a la suma del sizeof() de cada uno de sus elementos?. Justifique mediante un ejemplo
No necesariamente, en una estructura pueden haber datos de cualquier tipo de primitivo e incluso otra estructura 
por lo cual el tamaño dependera de la arquitectura y ademas de como el compilador decida colocar las variables 
en memoria, puede tomar la decisión de alinear los datos a posiciones de memoria multiples de cada tipo de dato y trabajar con 
paddings(desperdicios) lo cual en particular simplificara el codigo generado por el compilador pues no necesita 
hacer calculos especiales para saber como leer un dato o podria no alinearlos y trabajar sin padding(si la arquitectura lo permite)
para ahorrar espacio en memoria. En conclusion el sizeof de una estructura depende de varias variables, no es fijo.
Ejemplos:
struct foo {
	char a;
	int num;
	char b; 
};

struct bar {
	char a;
	char b;
	int num;
};

Tomando en cuenta el caso del compilador sin compactar los datos los resultados serian: 

- sizeof(foo) = (1 + 3 bytes de padding) + 4 + (1 + 3 bytes de padding) = 12
La explicación a esto es que un entero no podria ser colocado en la posicion de memoria siguiente al char porque 
deben estar alineados a posiciones de memoria multiplos de 4. Considerando que el char estuviera en la 
posicion 0, el int debe ir en la posicion 4 y dejar el padding en las posiciones 1,2 y 3, el ultimo char tambien
deja un padding de 3 bytes.

- sizeof(bar) = (1 + 1 + 2 bytes de padding) + 4 = 8
Este caso es similar al anterior, pero los char a y b se pueden colocar uno al lado de otro y esto desperdiciaria
menos memoria que caso anterior.

d. Investigar la existencia de los archivos estandar: STDIN, STDOUT, STDERR. Explicar brevemente su uso y como 
redirigirlos en caso de ser necesario (caracteres > y <) y como conectar la salida estándar de un proceso a la
entrada estadar de otro con un pipe (caracter |)

Todos los programas que se ejecuten en UNIX's comienzan con 3 canales abiertos para transferencias de datos, es decir
unos para la entrada otro de salida y el ultimo es de salida tambien pero en caso de errores en la ejecución 
del programa, al termino del programa estos canales se cierran automaticamente. Los archivos estandar STDIN, STDOUT, STDERR 
son descriptores de archivos (son valores de tipo int en C/C++ que sirven como clave para que internamente se reconozca a que tendra 
acceso el proceso que se esta ejecutando), estos son estandar porque los valores que se le asignan a cada uno 
son: STDIN = 0, STDOUT = 1, STDOUT = 2).
Por defecto la entrada estandar representa a los flujos de datos proveniente del teclado y la salida estandar 
comun/errores representa al flujo de datos que se dirige a la pantalla y se utilizan para ingresar datos que pueda 
requerir un programa o para extraerlos de el.

Ejemplos de como redirigir los archivos estandar por CLI:

1. echo "Hola Mundo" > example.txt 
2. grep < example "Hola"
3. ls non_existing_command >> error_example.txt
4. echo "Hola Mundo" | grep "Hola"
