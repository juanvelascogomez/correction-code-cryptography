# Códigos correctores de errores y criptografía
---

## Información general
---

Doble Grado en Ingeniería Informática y Matemáticas (Universidad de Granada)

**Alumno:** Juan Antonio Velasco Gómez

**Tutores:** Pedro A. García-Sánchez y José Ignacio Farrán Martín

**Fecha:** 26 de Junio de 2017

## Resumen

En este Trabajo de Fin de Grado se aborda el estudio de la Teoría de Códigos Correctores basada en los códigos de Goppa. Para comenzar, plantearemos uno de los principales problemas de la criptografía actual frente a la posibilidad de sufrir un ataque cuántico. Incluyendo sus posibles consecuencias. De hecho, trataremos de responder a la pregunta de si la criptografía actual se encuentra en riesgo. Para responderla, buscaremos criptosistemas conocidos que pudiesen ser capaces de aguantar un posible ataque cuántico. Hasta el momento se piensa que algunos como RSA y los cifrados de curvas elípticas son vulnerables. Parece que el algoritmo de Shor es capaz de vulnerarlos. Debemos buscar otras posibilidades. Daniel J. Bernstein propuso el criptosistema de McEliece como respuesta a esta pregunta, puesto que supuestamente era capaz de resistir el algoritmo de Shor. A lo largo del trabajo se irá evolucionando desde los conceptos más básicos y que conforman la base de esta teoría de códigos correctores hasta los algoritmos de recuperación de información en los cuales residen las esperanzas de resistir un posible ataque cuántico. Para llegar hasta este punto introduciremos conceptos muy importante en los códigos correctores como el concepto de palabra, el concepto de distancia de Hamming que nos servirá para medir el número de elementos que difieren entre dos palabras cualesquiera de un determinado código. Esta distancia nos será muy importante cuando busquemos corregir los errores que tiene una palabra en concreto.

¿Pero cuándo diremos que un código está bien definido?. En general hablaremos de códigos lineales, es decir, códigos que poseen una estructura algebraica. Esto nos llevará a introducir el concepto de códigos binarios, que serán el tipo de códigos que usaremos en los algoritmos de Patterson y de McEliece.

Estos algoritmos usan procesos de codificación y decodificación, procesos que cuando estemos usando códigos lineales serán mucho más simples que en el caso general. Esto nos llevará a introducir una serie de conceptos muy importantes para entender los pasos de estos procesos. Se introducirá la matriz generatriz de un código, que será una aplicación lineal biyectiva que forma una base del código. Veremos también las matrices de control de un código y estableceremos una relación entre estas dos matrices.

¿Existe relación entre las matrices, en especial la matriz de control y la distancia mínima de un código?. En efecto. Esta relación la veremos cuando introduzcamos los conceptos de soporte de un elemento y el peso de Hamming de un elemento del código.

Después de establecer la base de la teoría de códigos correctores estudiaremos el caso de detección de errores, introduciendo los patrones de errores o el síndrome de una palabra de un código, que nos ayudará a detectar esa existencia de errores en una palabra.

Además se presentarán las herramientas necesarias para poder realizar estos algoritmos, entre ellos los códigos de Goppa, más en concreto los códigos binarios de Goppa. Estos códigos presentan una serie de características que los dotan de una gran importancia ante el problema de encontrar algoritmos de cifrado capaces de aguantar ataques que otros algoritmos como RSA, ElGamal o la criptografía de curvas elípticas no han sido capaces (de forma teórica) de aguantar. El aprendizaje de los códigos de Goppa nos permitirá entender el algoritmo de Patterson, capaz de detectar y corregir los errores de una palabra recibida.

También veremos el cifrado de McEliece, un tipo de criptosistema de clave pública que usa códigos correctores para crear la clave pública y privada. Este criptosistema no es totalmente seguro, de hecho, son conocidos algunos de los ataques que podrían comprometer dicho algoritmo como por ejemplo el ataque Stern, que veremos más en profundidad en la última parte del trabajo y que consiste en la búsqueda de palabras codificadas con un peso de Hamming bajo. Sin embargo, su estudio es importante para el caso que nosotros estamos considerando, el posible ataque cuántico puesto que aún no se han encontrado algoritmos de corrección de errores eficientes sin el conocimiento de un polinomio generador y por tanto es complicado recomponer la estructura del código a partir de la información conocida por un posible atacante.

En relación con el ataque de Stern, analizaremos la seguridad del criptosistema de McEliece y trataremos de buscar soluciones ante estos ataques capaces de romperlo. El estudio de este tipo de criptosistemas capaces de resistir los posibles ataques de los sistemas cuánticos es de gran importancia. De hecho, el estudio cobra aún más importancia cuando la búsqueda de estos criptosistemas se realiza en la actualidad, es decir, buscamos aquellos algoritmos actuales que pudiesen resistir con éxito ataques tan potentes como los que se podrán realizar dentro de poco tiempo.

Finalmente, estableceremos una serie de conclusiones con respecto al trabajo realizado, analizaremos el cumplimiento de los objetivos establecidos y volveremos a reflexionar, al igual que en la introducción sobre el estado de la criptografía en la actualidad. Sus ventajas e inconvenientes frente a la posible llegada (inminente o no) de la computación y de la criptografía cuántica. Estas conclusiones vendrán acompañadas de una serie de comentarios sobre posible trabajo futuro en este campo, también relacionada con esa posible llegada de la criptografía cuántica, la búsqueda de nuevos algoritmos de cifrado post-cuánticos, las ventajas e inconvenientes frente a la criptografía actual y las posibilidades "reales" de que estos cifrados lleguen a nuestras manos razonando por qué debería de centrarse la comunidad en este nuevo tipo de criptografía en vez de seguir de lleno en la criptografía actual.

Todo el trabajo matemático viene acompañado de un desarrollo informático utilizando varios software matemáticos. El primero de ellos, basado en el lenguaje de programación Python es SageMath, un software de código libre que cubre muchos aspectos de las matemáticas como el álgebra, la combinatoria, el cálculo numérico y la teoría de números. Para ello dispone de múltiples paquetes matemáticos que dotan al sistema de una gran potencia. En él se han ido realizando los ejemplos visto en la teoría así como el algoritmo de Patterson y el cifrado de McEliece. Además de este software, también se han visto los mismos ejemplos implementados en GAP, un sistema para álgebra computacional y muy útil en la teoría de códigos que implementa su propio lenguaje de programación.


## Repositorio
---
  - Trabajo desarrollado (PDF)
  - Códigos desarrollados en [SageMath](http://www.sagemath.org/)
  - Códigos desarrollados en [GAP](http://www.gap-system.org/)
