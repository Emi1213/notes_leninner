# Testing Basics

> ¿Cuál es la importancia de Test Driven Development? TDD

Aquí escribe una prueba antes de escribir el código de producción suficiente para cumplir con esa prueba y luego refactoriza el código.

El objetivo principal de TDD es la especificación y no la validación.

En otras palabras, TDD es una forma de pensar en sus requisitos o diseño antes de escribir su código funcional (lo que implica que TDD es tanto un requisito ágil importante como una técnica de diseño ágil). Otro punto de vista es que TDD es una técnica de programación. El objetivo de TDD es escribir código limpio que funcione.

1. El primer paso es agregar rápidamente una prueba, básicamente la prueba suficiente para que su código falle

## ¿Por qué los desarrolladores deberían preocuparse por las pruebas unitarias automatizadas?

¡Te mantiene fuera del depurador (hambriento de tiempo)!
Reduce errores en nuevas funciones y en funciones existentes.
Reduce el costo del cambio
Mejora el diseño
Fomenta la refactorización
Crea una red de seguridad para defenderse de otros programadores
Es divertido
Te obliga a reducir la velocidad y pensar
Acelera el desarrollo al eliminar el desperdicio
Reduce el miedo

## ¿Cómo lleva TDD el desarrollo al siguiente nivel?

Mejora la productividad al:

- Minimizando el tiempo dedicado a la depuración
- reduce la necesidad de verificación manual (mono) por parte de desarrolladores y tester ayudando a los desarrolladores a mantener el enfoque
- reducir el desperdicio: traspasos
- Mejora la comunicación
- Creación de especificaciones vivas y actualizadas
- Comunicar decisiones de diseño
- Aprendizaje: escucha tu código
- Pasos de bebé: disminuya la velocidad y piense
- Mejora la confianza
- Código comprobable por diseño + red de seguridad
- Diseño de acoplamiento flexible
- Refactorización

# Instalación de Jest

Para instalas Jest ejecutamos lo siguiente en la terminal:

- npm install --save-dev jest

Para instalar de manera global, utilizamos:

- sudo npm install jest --global

## Usando Matchers

Jest usa "comparadores" para permitirle probar valores de diferentes maneras.

### Comparadores Comunes

Similitud exacta:

```
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
});
```

En el ejemplo anterior, .toBe(4) es el Matcher
Si desea verificar el valor de un objeto, use toEqual en su lugar:

```
test('object assignment', () => {
  const data = {one: 1};
  data['two'] = 2;
  expect(data).toEqual({one: 1, two: 2});
});
```

toEqual comprueba de forma recursiva todos los campos de un objeto o matriz.
También puede probar lo opuesto a un comparador:

```
test('adding positive numbers is not zero', () => {
  for (let a = 1; a < 10; a++) {
    for (let b = 1; b < 10; b++) {
      expect(a + b).not.toBe(0);
    }
  }
});
```

### Veracidad

A veces necesitamos ver si algo es null, undefined o false. Jest nos ayuda con eso:

- toBeNull matches only null
- toBeUndefined matches only undefined
- toBeDefined is the opposite of toBeUndefined
- toBeTruthy matches anything that an if statement treats as true
- toBeFalsy matches anything that an if statement treats as false

Ejemplo:

```
test('null', () => {
  const n = null;
  expect(n).toBeNull();
  expect(n).toBeDefined();
  expect(n).not.toBeUndefined();
  expect(n).not.toBeTruthy();
  expect(n).toBeFalsy();
});

test('zero', () => {
  const z = 0;
  expect(z).not.toBeNull();
  expect(z).toBeDefined();
  expect(z).not.toBeUndefined();
  expect(z).not.toBeTruthy();
  expect(z).toBeFalsy();
});
```

### Números

Feature muy conveniente:

```
test('two plus two', () => {
  const value = 2 + 2;
  expect(value).toBeGreaterThan(3);
  expect(value).toBeGreaterThanOrEqual(3.5);
  expect(value).toBeLessThan(5);
  expect(value).toBeLessThanOrEqual(4.5);

  // toBe and toEqual are equivalent for numbers
  expect(value).toBe(4);
  expect(value).toEqual(4);
});
```

Para la igualdad de punto flotante, use toBeCloseTo en lugar de toEqual, porque no desea que una prueba dependa de un pequeño error de redondeo.

```
test('adding floating point numbers', () => {
  const value = 0.1 + 0.2;
  //expect(value).toBe(0.3);           This won't work because of rounding error
  expect(value).toBeCloseTo(0.3); // This works.
});
```

### Strings

Puede comparar cadenas con expresiones regulares con toMatch:

```js
test("there is no I in team", () => {
  expect("team").not.toMatch(/I/);
});

test('but there is a "stop" in Christoph', () => {
  expect("Christoph").toMatch(/stop/);
});
```

### Arrays and iterables

Puede verificar si una matriz o iterable contiene un elemento en particular usando toContain:

```js
const shoppingList = ["diapers", "kleenex", "trash bags", "paper towels", "milk"];

test("the shopping list has milk on it", () => {
  expect(shoppingList).toContain("milk");
  expect(new Set(shoppingList)).toContain("milk");
});
```

### Excepciones

Si desea probar si una función en particular arroja un error cuando se llama, use toThrow.

```js
function compileAndroidCode() {
  throw new Error("you are using the wrong JDK");
}

test("compiling android goes as expected", () => {
  expect(() => compileAndroidCode()).toThrow();
  expect(() => compileAndroidCode()).toThrow(Error);

  // You can also use the exact error message or a regexp
  expect(() => compileAndroidCode()).toThrow("you are using the wrong JDK");
  expect(() => compileAndroidCode()).toThrow(/JDK/);
});
```

## Instalación de Babel para Jest

Jest no reconoce la manera de importar de ES6, por esa razón debemos instalar Babel:

- npm i -D @babel/preset-env

Archivo .babelrc para configuración:

- { "presets": ["@babel/preset-env"] }

## Aislamiento

Solo debe probar un método a la vez, y sus pruebas para una función no deben depender de que una función externa se comporte correctamente, especialmente si esa función se está probando en otro lugar.

La razón principal de esto es que cuando sus pruebas fallan, desea poder delimitar la causa de esta falla lo más rápido posible. Si tiene una prueba que depende de varias funciones, puede ser difícil saber exactamente qué está fallando.

### ¿Qué son funciones puras?

1. La función siempre devuelve el mismo resultado si se pasan los mismos argumentos. No depende de ningún cambio de estado o de datos durante la ejecución de un programa. Solo debe depender de sus argumentos de entrada.

2. La función no produce ningún efecto secundario observable, como solicitudes de red, dispositivos de entrada y salida o mutación de datos.

#### ¿Qué son los efectos secundarios observables?

- Es cualquier interacción con el mundo exterior desde dentro de una función. Eso podría ser cualquier cosa, desde cambiar una variable que existe fuera de la función, hasta llamar a otro método desde dentro de una función.

NOTE: Si una función pura llama a una función pura, esto no es un efecto secundario y la función que llama sigue siendo pura.

Los efectos secundarios incluyen, entre otros:

- Hacer una solicitud HTTP
- Mutando datos
- Imprimir en una pantalla o consola
- Consulta / manipulación DOM
- Math.random ()
- Obtener la hora actual

> En sí mismos no son malos y a menudo son necesarios.

No todas las funciones deben ser puras

Ejemplo de función pura:

```
function priceAfterTax(productPrice) {
 return (productPrice * 0.20) + productPrice;
}
```

Ejemplo de función impura:

```
let tax = 20;
function calculateTax(productPrice) {
 return (productPrice * (tax/100)) + productPrice;
}
```

NOTE: Uno de los beneficios más grandes de usar funciones puras es que son testeables.

## Mocking

Hay dos soluciones al problema del "código estrechamente acoplado":

1. The first, and best option is to simply remove those dependencies from your code as we did above, but that is simply not always possible.
2. La segunda opción es Mocking: escribir versiones "falsas" de una función que siempre se comporta exactamente como usted quiere. Por ejemplo en el uso de inputs por parte del usuario.

Un Unit es un módulo, componente o función. Unit test hace tests de ese módulo, componente o función.

Las afirmaciones de pruebas simples proporcionan:

- Mejor legibilidad.
- Menos código.
- Menos mantenimiento.
