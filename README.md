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
