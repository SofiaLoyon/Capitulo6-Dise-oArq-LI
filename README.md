# Capitulo6-DisenoArq-LI

## Antecedentes

La arquitectura ARM fue desarrollada por Acorn Computer Group en la década de los 80's, empresa a la que actualmente conocemos como ARM;
alrededor de 10 billones de procesadores ARM se venden cada año, tomando en cuenta que la mayoría de celulares y tablets los poseen al igual que muchas de las cosas que usamos hoy como cámaras, robots y servidores.

A pesar de esto ARM no crea procesadores directamente, sino que otorga licencia a otras grandes compañias para que los creen, tales como: Samsung, Apple, Qualcomm, etc. ya sea utilizando microarquitecturas por ARM o microarquitectuas desarrolladas internamente bajo su licencia. 

## Lenguaje Ensamblador
El lenguaje ensamblador es la representación "humanamente legible" del lenguaje nativo del computador. Cada intrucción de este lenguaje representa la operación que se va a realizar y los operandos sobre los cuales se va a llevar a cabo. Introducimos instrucciones aritméticas simples y mostramos como son escritas en lenguaje ensamblador, posteriormente difinimos los operandos de la instrucción ARM: registros, memoria y constantes.

### Instrucciones
La operación más común que se lleva a cabo es la suma, en el ejemplo se muestra la suma de las variables b y c que guardan en el resultado en a, escrito en lenguaje de alto nivel y reescrito en lenguaje ensamblador de ARM. 

#### Ejemplo Adición
| Lenguaje Alto Nivel | Ensamblador ARM |       
|-------------------- | ----------------|       
| a = b + c;          | ADD a , b , c   |  

La primera parte de la instrucción **ADD** es llamada _nemotécnico_ e indica la operación a realizar, la operacion es llevada acabo por **b** y **c** los _operandos fuente_ y el resultado es escrito en **a**, el _operando destino_.

#### Ejemplo Substracción
| Lenguaje Alto Nivel | Ensamblador ARM |       
|-------------------- | ----------------|       
| a = b - c;          | SUB a , b , c   |  

El ejemplo de substacción es similar al de adición excepto por la operación de especificación.

### Principios de Diseño

#### Principio de Diseño 1: La regularidad apoya la simplicidad.

Las instrucciones con un número consistente de operandos en este caso son fuente y uno de destino son mas fáciles de codificar y manejar en hardware. El código más complejo en lenguajes de alto nivel se convierten en múltiples instrucciones ARM.
En los lenguajes de alto nivel los comentarios de una línea comienzan con // y continúan hasta el final de la línea, los multilínea comienzan y terminan con diagonal astérisco. En lenguaje ensamblador ARM comienzan con punto y coma y continuan hasta el final de la línea. 

```
ADD t , b , c : t = b + c

```

#### Principio de Diseño 2: Acelere el caso común

El set de instrucciones ARM acelera el caso común incluyendo solo instrucciones simples comunmente usadas. El numero de instrucciones se mantiene pequeño para que cuando el harware necesite decodificar la instrucción y sus operandos sea mas sencillo y rápido. Las operaciones mas elaboradas son menos comunes y se llevan a cabo usando secuencias múltiples de instrucciones simples. Por lo tanto es una computadora de set de instrucciones reducidas (Arquitectura RISC). Las arquitecturas con muchas instrucciones complejas como Intel x86 son arquiecturas CISC.

La arquitectura RISC minimiza la complejidad del hardware y la codificación de las instrucciones mediante mantener el set de distintas instrucciones pequeño.

#### Operandos: Registros, Memoria y Constantes.

Una instrucción opera en operandos. Las instrucciones necesitan un almacenamiento fisico de donde recuperar los datos binarios. Los operandos pueden ser almacenados en registros o memoria, o pueden ser constantes almacenadas en la misma instrucción.
Las computadoras usan varias locaciones para almacenar operandos en orden para optimizar la velocidad y la capacidad de los datos, los operandos almacenados como constantes o registros son rápidamente accesibles pero solo contienen una pequeña cantidad de información, los datos adicionales son accesados a través de la memoria, lo que es más grande pero más lento. 

#### Registros

Las instrucciones necesitan acceder rapidamente a los operandos para que puedan correr velozmente, pero los operandos almacenados en memoria toman más tiempo en retornar. Las arquitecturas ARM usan 16 registros, entre menos registros más rápido pueden ser accesados.

#### Principio de Diseño 3: Lo pequeño es más rápido.

Leer la información de un registro pequeño es mas rápido que leerlo de una gran memoria. 

#### Set de Registros

Del R0 al R12 son usados para almacenar variables, del R0-R3 también tienen usos especiales durante las llamadas, R13-R15 son también llamadas SP, LR, y PC.

#### Constantes Inmediatas

Además del registro de operaciones, las instrucciones ARM pueden usar contantes o operandos _inmediatos_. Estas constantes son llamadas inmediatas, porque sus valores estan disponibles inmediatamente en la instrucción y no requieren un registro o acceso de memoria. 

|    Nombre           |  Uso            |
|-------------------- | ----------------|       
|  R0                 | Argumento, Retonan valor, variable temporal |               
| R1- R3              | Argumento, Variables Temporales   |            
| R4-R11              | Variables Almacenadas   |    
| R12                 |  Variable Temporal  |      
| R13 (SP)            | Puntero de Pila   |      
| R14 (LR)             | Registro de Enlace  |     
| R15(PC)           | Contador de Programa  |

#### Memoria

Si los registros fueran los únicos espacios de almacenamiento para los operandos estaríamos confinados a simples programas de no más de 15 variables. Sin embargo, los datos también pueden ser almacenados en memoria. Mientras que los registros son pequeños y rápidos, la memoria es grande y lenta, por esta razón las variables usadas frecuentemente se mantienen en los registros. En la arquitectura ARM las instrucciones operan únicamente sobre los registros así que la información se guarda en la memoria así se puede mover a un registro antes de ser procesadas. Usando la combinación de memoria y registros, un programa puede accesar a una gran cantidad de datos rápidamente.

ARM utiliza una memoria direccionable por byte. Lo que es que cada byte en memoria tiene una dirección única. Una palabra de 32-bits consiste en 4 bytes de 8 bits., asi que cada palabra es un múltiplo de 4. El bit mas significativo (MSB) se ubica a la derecha y a la izquierda el bit menos significativo. También provee una instrucción de registro de carga (LDR) para leer los datos de memoria a registro. La instrucción LDR especifica la dirección de memoria utilizando el registro base y una compensación.
STR es una instrucción de registro de almacenamiento para escribir los datos del registro a la memoria. 

#### Bytes y Caracteres

Los números en el rango [-128, 127] puede ser almacenado en un solo byte. ARM provee un byte de carga (LDRB), byte de carga con signo (LDRSB), y un byte de almacenamiento (STRB) para acceder a bytes individuales en memoria. Una serie de caracteres de denomina _string_, los cuales tienen una longitufd variable, asi que los lenguajes de programación pueden proveer una forma de determinar la longitud o fianl de la cadena. En C, un caracter nulo (0x00) significa el final de un string. 

#### LLamada de Funciones

Los lenguajes de alto nivel soportan _funciones_ también llamadas Procedimientos o subrutinas para reusar el código común y hacer el programa más modular y legible. Las funciones tienen entradas llamadas _argumentos_ y salidas llamadas _valor de retorno_, las funciones pueden calcular el valor de retorno sin causar otros efectos secundarios no deseados. Cuando una función llama a otra, la función que llama y la llamadora deben coincidir de en donde poner los argumentos y el valor de retorno. En ARM la llamador normalmente pone cuatro argumentos en los registros R0-R3 antes de hacer la llamada de la función y la llamada situa el valor de retorno en el registro R antes de finalizar.

#### Llamada de Funciones y Retornos
ARM utiliza la instruccion _Branch and Link (BL)_ para llamar a una función y mover el registro de enlace a el PC para regresar de la función. _Bl_ y _MOV PC, LR_ son las dos instrucciones escenciales que necesitamos para llamar y retornar una función.
 BL lleva a cabo dos tareas: almacena la dirección de retorno de la siguiente instrucción en el registro de enlace y lo bifurca a la instrucción objetivo. 
 
 #### La Pila
La pila es memoria usada para guardar información sin una función, esta se expande (usa más memoria) cuando el procesador necesita más espacio vacío y se contrae (utiliza menos memoria) cuando el procesador ya no necesita las variables almacenadas ahí. 
Una función guarda los registros de la pila antes de modificarlos, luego los restaura de la pila antes de regresarlos. 

1. Crea un espacio en la pila para almacenar los valores de uno o más registros.
2. Almacena los valores de los registros en la pila.
3. Ejecuta la función usando los registros.
4. Restaura los valores originales de los registros de la pila.
5. Desasigna espacio de la pila. 
 
**Función guardando registros en la pila**
 ```
 ;R4 = result
DIFFOFSUMS
SUB SP, SP, #12 ; make space on stack for 3 registers
STR R9, [SP, #8] ; save R9 on stack
STR R8, [SP, #4] ; save R8 on stack
STR R4, [SP] ; save R4 on stack
ADD R8, R0, R1 ; R8 = f + g
ADD R9, R2, R3 ; R9 = h + i
SUB R4, R8, R9 ; result = (f + g) − (h + i)
MOV R0, R4 ; put return value in R0
LDR R4, [SP] ; restore R4 from stack
LDR R8, [SP, #4] ; restore R8 from stack
LDR R9, [SP, #8] ; restore R9 from stack
ADD SP, SP, #12 ; deallocate stack space
MOV PC, LR ; return to caller
```

#### Cargando y Almacenando Múltiples Registros.

Guardar y restaurar registros en una pila es una operación muy común que ARM provee a través de instrucciones _LDM y STM_ que son optimizadas para tal propósito. La pila contiene exactamente la misma información pero el código es más corto. 

 ```
 ; R4 = result
DIFFOFSUMS
STMFD SP!, {R4, R8, R9} ; push R4/8/9 on full descending stack
ADD R8, R0, R1 ; R8 = f + g
ADD R9, R2, R3 ; R9 = h + i
SUB R4, R8, R9 ; result = (f + g) − (h + i)
MOV R0, R4 ; put return value in R0
LDMFD SP!, {R4, R8, R9} ; pop R4/8/9 off full descending stack
MOV PC, LR ; return to caller
 ```
 
#### Registros Preservados

Los registros preservados incluyen R4-R11 y los no preservados son R0-R3 y R12, R13 y R14 tambien pueden ser preservados. Una función puede guardar y restaurar cualquiera de los registros preservados que desee usar pero puede ser cambiado a los registros no preservados libremente. 

#### Llamada de Funciones Sin Hojas

Una función que no llamaa otra es llamada funcion hoja, una función que llama a otra es llamada función sin hoja.
Las anteriormente mencionadas son algo más complicadas porque necesitan guardar registros no preservados en la pila antes de llamar a otra función y después restaurar esos registros.
**Una función sin hoja sobreescribe LR cuando se llama a otra funcion usando BL, por lo tanto siempre deberá guardar a LR en la pila para restaurarlo antes de retornar**.

 ```
 Lenguaje de Alto Nivel
 
int f1(int a, int b) {
int i, x;
x = (a + b)*(a − b);
for (i=0; i<a; i++)
x = x + f2(b+i);
return x;
}
int f2(int p) {
int r;
r = p + 5;
return r + p;
}

Lenguaje Ensamblador ARM

; R0 = a, R1 = b, R4 = i, R5 = x
F1
PUSH {R4, R5, LR} ; save preserved registers used by f1
ADD R5, R0, R1 ; x = (a + b)
SUB R12, R0, R1 ; temp = (a − b)
MUL R5, R5, R12 ; x = x * temp = (a + b) * (a − b)
MOV R4, #0 ; i = 0
FOR
CMP R4, R0 ; i < a?
BGE RETURN ; no: exit loop
PUSH {R0, R1} ; save nonpreserved registers
ADD R0, R1, R4 ; argument is b + i
BL F2 ; call f2(b+i)
ADD R5, R5, R0 ; x = x + f2(b+i)
POP {R0, R1} ; restore nonpreserved registers
ADD R4, R4, #1 ; i++
B FOR ; continue for loop
RETURN
MOV R0, R5 ; return value is x
POP {R4, R5, LR} ; restore preserved registers
MOV PC, LR ; return from f1
; R0 = p, R4 = r
F2
PUSH {R4} ; save preserved registers used by f2
ADD R4, R0, 5 ; r = p + 5
ADD R0, R4, R0 ; return value is r + p
POP {R4} ; restore preserved registers
MOV PC, LR ; return from f2
 ```
#### Llamada de Funciones Recursivas
Una función recursiva en una función sin hoja que se llama a si misma. Estas se comportan como llamadora y llamada; también deben guardar nos registros preservados y no preservados, por ejemplo una función factorial.

#### Argumentos Adicionales y Variables Locales
Las funciones deben tener por lo menos cuatro argumentos de entrada y muchas variables locales para mantener preservados los registros, la pila es utilizada para almacenar esta información. Si una función tiene más de cuatro argumentos, los primeros cuatro son pasados en los registros de argumentos como usualmente, los argumentos adicionales son pasados a la pila, justo arriba de SP. 

Una función también puede declarar variables locales y arreglos. Las variables locales se declaran dentro de una función y solo se puede acceder a ellas dentro de esa función. Las variables locales se almacenan en R4 – R11; si hay demasiadas variables locales, también se pueden almacenar en el marco de pila de la función. En particular los arreglos son almacenados en la pila. 

### Lenguaje Máquina

El lenguaje ensamblador es conveniente de leer para los humanos, sin embargo los circuitos digitales solo entienden 0's y 1's. Por lo tanto un programa escrito en lenguaje ensamblador es trasladado de nemotécnicos a una representación de 0 y 1 llamado lenguaje máquina.

#### Principio de Diseño 4: El buen diseño demanda buen compromiso. 

ARM hace el compromiso de definir 3 formatos de instrucciones principales: Procesamiento de Datos, Memoria y Bifuración. Este pequeño número de procesos permite regularidad entre instrucciones de esta manera el decodificador de hardware se adapta mejor a las necesidades de cada instrucción.


