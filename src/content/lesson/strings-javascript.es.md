---
title: "¿Qué es un string en Javascript?"
subtitle: "Descubre qué es un string en JavaScript y cómo se utiliza para almacenar y manipular texto. Aprende los conceptos básicos y ejemplos prácticos en esta guía."
tags: ["javascript","string-concatenation"]
authors: ["jul1998"]

---

En JavaScript, existe un tipo de dato llamado string y existe para representar texto, además de ser una parte primordial de la programación. Durante este artículo, se explicará su función, cómo se utilizan y algunos ejemplos donde se aplica su uso.

Aquí un ejemplo de cómo luce un string en JavaScript:

```js
let miString = "Esto es un string";
```

SI todavía no tienes un dominio total sobre [cómo programar en JavaScript](https://4geeks.com/es/lesson/que-es-javascript-aprende-a-programar-en-javascript) o [para qué sirve Javascript](https://4geeks.com/es/lesson/para-que-sirve-javascript) te recomendamos leer sobre ello en el [Blog de 4Geeks](https://4geeks.com/).

## ¿Qué es un String en JavaScript?

Un string es un tipo de dato usado en casi todos los lenguajes de programación y cuya funcionalidad principal es representar un texto.

## ¿Para qué sirven los strings en JavaScript?

Este tipo de dato tiene muchos propósitos y funciones que se van a listar aquí abajo para dejar más claro la funcionalidad de ellos:

1.  **Permite la interacción con el usuario**: Programadores pueden causar que los strings interactúen con los usuarios por medio de formularios, entradas de datos como el nombre, edad, apellido, etc. También es posible enviar mensajes por medio de las aplicaciones para avisar al usuario acerca algún tema en específico.
2.	**Almacenamiento y manipulación de texto**: Mediante los strings es posible guardar y manipular información importante como nombres de usuario, mensajes o aplicar funciones como la unión de strings (concatenar), crear una subparte de un string principal(substring), buscar patrones en textos, extraer palabras clave con mayor facilidad y muchas otras más funciones. 
3.	**Comunicación entre puntos**: Los strings son muy usados a la hora de querer comunicar información entre servidores o APIs, por ejemplo. Esto porque es fácil de procesar datos en forma de string para luego crear alguna lógica con programación.

## ¿Cómo declarar un string en Javascript?

Es muy simple declarar un [string en JavaScript](https://4geeks.com/es/lesson/working-with-strings-in-java-es). Lo que se debe hacer es escribir algún texto (Puede contener letras, números, o signos) y encerrarlo ya sea con comillas simples o dobles (no habrá ninguna diferencia).

```js
let mensajeEnString = "Hola Mundo! Esto es un string";

let otroMensaje = 'Otro mensaje, pero esta vez con comillas simples';
```

A continuación, se mostrarán casos de uso con los strings.

## Concatenar strings en JavaScript

La concatenación es el proceso de unir varios strings en uno solo. En Javascript se puede lograr de la siguiente manera:

```js
let nombre = "Albert";
let apellido "Einstein";
let nombreCompleto = nombre + " " + apellido;

console.log(nombreCompleto)

//Resultado: Albert Einstein 
```

> Nota que se concatenó un string con un espacio `" "` para separar `nombre` de `apellido`. Sino el resultado hubiese sido: `AlbertEinstein`

Como se ve en el ejemplo anterior, se utiliza el operado "+" con el fin de concatenar. Otra manera para lograr esto es utilizado comillas invertidas (Backsticks) como se muestra en el ejemplo:

```js
let nombre = “Daniel”;
let edad = 22;
let pais = “Colombia”;

let mensaje = `Mi nombre es ${nombre}. Tengo ${edad} años y vivo en ${pais}`;

console.log(mensaje);

// Resultado: Mi nombre es Daniel. Tengo 22 años y vivo en Colombia
```

En este código, es más sencillo concatenar varios strings o incluso números utilzando los backsticks "\`".

## Comparar strings en JavasScript

Para comparar dos o mas strings, se utilizan los operadores de comparación "===" o de negación "!==". Esto retornar un valor booleano en caso de que sea verdadero o falso, permitiendo aplica alguna lógica en especifico dependiendo del resultado

```js
let fruta1 = "manzana";
let fruta2 = "naranja";

if (fruta1 === fruta2){
    console.log("fruta1 y fruta2 son iguales");
}else{
    console.log("fruta1 y fruta2 son diferentes");
}
```

## Métodos de strings en Javascript

Los strings en este lenguaje de programación poseen distintos métodos que podemos utilizar a nuestro favor para realizar diferentes tareas. Éstos son algunos de los métodos más utilizados en Javascript:

### string.length

Esta propiedad de los strings permite mostrar la cantidad de caracteres de un texto:
```js
let texto = "Este texto contiene 33 caracteres";
console.log(texto.length);

//Resultado 33
```

Es importante destacar que este método esta tomando en cuenta los espacios en blanco.

### string.slice()

Extrae una parte especifica de un string y retorna esta porción como un nuevo string. Este método toma dos parámetros los cuales son la posición inicial y la final:

```js
let texto = "Hola mundo!";
console.log(texto.slice(2, 9));

//Resultado 'la mund'
```

> Nota que si se incluye el caracter ubicado en la posicion del parámetro inicial, pero el del parámetro final no se incluye.

### string.replace()

El método replace() permite remplazar un valor especifico por otro en forma de string:

```js
let texto = "Me gusta las manzanas";
let textoNuevo = texto.replace("manzanas", "peras");

console.log(textoNuevo);

//Resultado "Me gustan las peras"
```

Estos fueron solo algunos ejemplos de los muchos métodos y funciones que se pueden aplicar con los strings en JavaScript. Por medio de práctica, se podrá saber cuando utilizar cada método en el momento idóneo y conseguir el objetivo deseado como programador.

## Conclusión 

Como se pudo estudiar a lo largo de este artículo, los strings en JavaScript tienen un papel fundamental en la programación donde permite desde la interacción con los usuarios hasta procesar información y datos completos utilizando los métodos de string anteriormente mencionados y entro otros muchos más. 

Si estás interesado en aprender más acerca de [JavaScript](https://4geeks.com/es/lesson/que-es-javascript-aprende-a-programar-en-javascript), visita nuestro artículo para profundizar en este tema. Esperamos que te haya quedado claro que son los strings en JavaScript.
