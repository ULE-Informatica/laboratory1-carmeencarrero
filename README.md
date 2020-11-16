Assignment 3

# Assignment 3
Para esta tarea se nos da un archivo .c que tenemos que compilar y arreglar puesto que da varios errores y warnings. También tenemos que encontrar diferentes reglas y recomendaciones del SEI CERT C Coding Standard que no cumple el código. Los pasos a seguir serían:

1. Compilación
2. Búsqueda de reglas y recomendaciones
3. Modificación del código

## Compilación
El código se va a compilar de cuatro maneras diferentes, seleccionando diferentes estándares y herramientas de compilación.

**GCC:**
El código se compila mediante el estándar c99:
`gcc -Wall -std=c99 exampleStrings.c`
Salida:
![Screenshot](/home/kali/Escritorio/1.png)
![Screenshot](/home/kali/Escritorio/2.png)
*Figura 1: salida de la compilación con gcc y el estándar C99.*

El código se compila mediante el estándar c11:
`gcc -Wall -std=c11 exampleStrings.c`
Salida:
![Screenshot](/home/kali/Escritorio/3.png)
![Screenshot](/home/kali/Escritorio/4.png)
*Figura 2: salida de la compilación con gcc y el estándar C11.*

**G++:**
El código se compila mediante el estándar c98 en g++:
`g++ -Wall -std=c++98 exampleStrings.c`
Salida:
![Screenshot](/home/kali/Escritorio/5.png)
![Screenshot](/home/kali/Escritorio/6.png)
*Figura 3: salida de la compilación con g++ y el estándar C98.*

El código se compila mediante el estándar c11 en g++:
`g++ -Wall -std=c++11 exampleStrings.c`
![Screenshot](/home/kali/Escritorio/7.png)
*Figura 4: salida de la compilación con g++ y el estándar C11.*

**CMAKE:**
Para compilar con cmake se ha creado el fichero *CMakeLists.txt*, que contiene las siguientes líneas:
```
cmake_minimum_required(VERSION 2.8)
project(exampleStringsCmake)
add_executable(app exampleStrings.c)
```
Después, se han utilizado los comandos
`cmake .` y `make`. Salida:
![Screenshot](/home/kali/Escritorio/8.png)
*Figura 5: salida de la compilación con cmake.*

## Búsqueda de reglas y recomendaciones
Las reglas y recomendaciones encontradas en el código fuente han sido:
- **STR30-C**: Do not attempt to modify string literals (Regla)
En la línea 101
- **STR32-C**: Do not pass a non-null-terminated character sequence to a library function that expects a string (Regla)
En las líneas 83, 97 y 103. 
- **ARR32-C**: Ensure size arguments for variable length arrays are in a valid range (Regla)
En la línea 48.
- **MEM35-C**: Allocate sufficient memory for an object (Regla)
En las líneas 17, 18 y 66.
- **MSC24-C**: Do not use deprecated or obsolescent functions (Recomendación)
En la línea 51.

## Modificación del código
Para solucionar los warnings y errores del código nos fijamos en la salida de compilarlo con g++ y el estándar C11 *(Figura 4)* y vamos modificando el código, quedando un nuevo código que está disponible en un archivo llamado *exampleStringsFixed.c*
Dichos errores junto con su solución han sido:

- En la línea 32 hay un `return 1;` de una función void, por lo que para arreglar el error se pone `exit(1);` en su lugar para que devuelva 1 e interrumpa la ejecución del código. De todos modos, el método no se llama en todo el código por lo que lo podemos comentar sin problema.
- En la línea 39 `slash = strrchr(pathname, '/');` nos dice que no se puede convertir de `const char*` a `char*`, para solucionar esto utilizamos la función `malloc()` para reservar memoria. Como el malloc() devuelve un void* hay que hacer un casting a char*.
- En la línea 48 se ha modificado el tamaño de response de 8 a 2 ya que el caracter que se espera (y o n) es de tamaño 1. El tamaño es 2 porque se le añade el '\0' del final.
- En la línea 51 se encuentra `gets(response);` y el warning nos sale porque esta función está deprecated. Para evitar que nos salga este aviso la solución es cambiar la función por `fgets(response, sizeof(response),stdin);`
- La línea 67 (`char *ptr_char  = "new string literal";`) corresponde a una variable que no tiene sentido usar por lo que se comenta; de todos modos, si se quisiera utilizar daría error por lo que para arreglar esto se pondría `char ptr_char[] = "new string literal";` Además, en la línea 101 se hace una modificación que estaría mal puesto que se trata de un string literal.
- Las líneas 68, 69 y 73 hacen referencia a las variables `size_array1, size_array2 y analitic3` que no son usadas en todo el código, por lo que para evitar que aparezcan estos warnings, las comentamos.
- En la línea 73 se inicializa la variable *analitic3* de la siguiente manera: `char analitic3[100]="аналитик";`
Esta línea ha sido comentada ya que no se utiliza la variable pero en el caso de que se usara el tamaño del char array debería ser 16 (8*2) ya que se trata de wide characters.
- En la línea 98 se pone `sizeof` en vez de `strlen` ya que `strlen` no tiene en cuenta el carácter nulo del final.

**Nota:** Si nos fijamos en la salida de las otras imágenes, cuentan con un error en las líneas 22 y 25 que hace referencia a la declaración de la variable *s1* mediante las líneas *R"foo(Hello World)foo";*
Es una manera de definir los strings literals que se implementó a partir de C++11, por lo que si compilamos con el estándar C++11 en g++ no sale error. 

## Referencias:
- https://wiki.sei.cmu.edu/confluence/display/c/SEI+CERT+C+Coding+Standard
- https://en.cppreference.com/w/cpp/language/string_literal
