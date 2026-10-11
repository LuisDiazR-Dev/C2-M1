# C2-M1: Conceptos Básicos JavaScript ECMAscript 6*

## Variable

Es una expresión que puede tomar cualquier valor e ir variando, o no, de acuerdo a las necesidades del código.
Las variables tienen 2 estados: inicializada y definida.

```js
X      // -> Inicializada
X = 1  // -> Definida
```

```js
var x       // Inicializada -> se crea y queda a la espera de tomar un valor de acuerdo a las necesidades del código.
var x = 1   // Definida -> se le asigna un valor desde el comienzo
```

## Nombrar variables: Convenciones de nomenclatura (naming conventions)

| Estilo | Formato | Ejemplo | ¿Dónde se usa principalmente? |
| :--- | :--- | :--- | :--- |
| **camelCase** | Primera palabra en minúscula; las siguientes inician con mayúscula. | `nombreUsuario` | **JavaScript**, TypeScript, Java. |
| **snake_case** | Todo en minúsculas, palabras separadas por guiones bajos. | `nombre_usuario` | **Python**, **PostgreSQL** (¡clave para tu PERN* stack*!). |
| **PascalCase** | Todas las palabras inician con mayúscula (también llamado *UpperCamelCase*). | `NombreUsuario` | Clases en JS/Python, componentes de **React**, C#. |
| **kebab-case** | Todo en minúsculas, palabras separadas por guiones medios. | `nombre-usuario` | Clases de **CSS**, rutas de URLs, archivos HTML. |

### ⚠️ El Choque de Mundos en el PERN* Stack* (Tip)

Al trabajar con este Stack* te enfrentarás a una inconsistencia natural entre tecnologías:

* **PostgreSQL** exige por estándar usar `snake_case` para las columnas de las tablas (ej: `user_id`, `created_at`).
* **JavaScript / React** exige por convención usar `camelCase` para los objetos y variables (ej: `userId`, `createdAt`).

**Solución práctica:** Cuando hagas consultas desde Express* usando consultas SQL* puras o librerías, asegúrate de mapear los datos o usar alias (`SELECT user_id AS userId`) para que tu código en el Backend y Frontend* se mantenga 100% fiel a las reglas de JavaScript.

## Declaración de variables

Existen 3 palabras clave para declarar variables en JavaScript. Su comportamiento varía según su **Scope*** (alcance), si permiten **re-declaración** y su **mutabilidad***:

### 1. `var` (El modo antiguo - Evitar usar)

* **Scope*:** Global o de Función (No respeta bloques como `if` o `for`).
* **Re-declaración:** **Sí** lo permite. Puedes volver a declarar una variable con el mismo nombre sin que el código falle, lo que suele causar errores graves al pisar datos sin querer.
* **Mutabilidad*:** El valor puede cambiar en cualquier momento.

```js
var nombre = "Luis";
var nombre = "Carlos"; // ✅ Permitido (¡Peligroso! Pisa el valor anterior)
nombre = "Pedro";      // ✅ Permitido cambiar el valor
```

### 2. `let` (El estándar moderno para valores variables)

* **Scope*:** De Bloque (Solo existe dentro de las llaves `{}` donde fue creada).
* **Re-declaración:** **No**. Si intentas volver a declararla en el mismo scope*, el código arroja un error antes de correr.
* **Mutabilidad*:** El valor puede cambiar (puedes reasignar su valor).

```js
let edad = 25;
// let edad = 30;    // ❌ Error: Identifier 'edad' has already been declared
edad = 26;           // ✅ Permitido cambiar el valor
```

### 3. `const` (Para valores fijos o estructuras de datos)

* **Scope*:** De Bloque (Igual que `let`).
* **Re-declaración:** **No**.
* **Mutabilidad (Inmutabilidad*):** El valor primitivo no se puede reasignar. **Importante:** Si guardas un Objeto o un Array en un `const`, no puedes cambiar el objeto entero, pero *sí* puedes modificar sus propiedades o elementos internos.

```js
const pi = 3.1416;
// pi = 3.14;        // ❌ Error: Assignment to constant variable

// Caso especial con Objetos y Arrays:
const usuario = { nombre: "Luis" };
usuario.nombre = "Carlos"; // ✅ ¡Permitido! Modificas el interior del objeto
// usuario = {};           // ❌ Error: No puedes reasignar la variable completa
```

## Scope*

Se le llama Scope* (alcance) a la región del código donde se definen las variables y, por tanto, desde donde pueden ser llamadas. Puede ser global o local.

```js
if (true) {
  var seEscapa = "Hola, me salí del bloque";
  let bloqueado = "No me puedes ver afuera";

  console.log(bloqueado); // Me imprimo porque estoy dentro del bloque "if"
}

console.log(seEscapa);  // Imprime: "Hola, me salí del bloque"
console.log(bloqueado); // ❌ Error: bloqueado is not defined
```

### Scope* Global

Es cuando las variables se declaran fuera de cualquier función o bloque de código y están disponibles a lo largo de todo el archivo.

```js
var variableGlobal = "Soy Global"

function llamarGlobal(){
    console.log(variableGlobal)
}

console.log(variableGlobal + "2") // Soy Global 2

llamarGlobal() // Soy Global
```

## Hoisting

Término complejo que requiere un esfuerzo extra. Conozcamos el concepto y más adelante con la práctica lo veremos en acción.

El **Hoisting** es un comportamiento de JavaScript que "eleva" de forma imaginaria las declaraciones de variables y funciones al principio del código antes de ejecutarlo. Sin embargo, el comportamiento cambia drásticamente dependiendo de la palabra clave que uses (`var`, `let` o `const`).

### 1. Hoisting con `var` (El origen del caos)

Cuando declaras una variable con `var`, JavaScript eleva la **declaración**, pero **no la definición** (el valor). Por lo tanto, si intentas usar la variable *antes* de la línea donde la creaste, no arrojará un error, sino que devolverá `undefined*`.

```js
console.log(miNombre); // Imprime: undefined (¡No da error!)

var miNombre = "Luis";

console.log(miNombre); // Imprime: "Luis"
```

**Lo que JavaScript hace por detrás:**

```js
var miNombre;          // Eleva la declaración al inicio y le asigna undefined
console.log(miNombre); // Imprime undefined
miNombre = "Luis";     // Asigna el valor en su posición original
```

### 2. Hoisting con `let` y `const` (La Zona Muerta Temporal)

Con `let` y `const` también ocurre Hoisting, pero el lenguaje te protege. JavaScript sabe que la variable existe, pero **prohíbe estrictamente acceder a ella** antes de su línea de declaración. 

A este espacio de tiempo donde la variable existe pero no se puede tocar se le llama **Temporal Dead Zone (TDZ)** o *Zona Muerta Temporal*.

```js
console.log(miEdad); // ❌ Error: Cannot access 'miEdad' before initialization

let miEdad = 25;
```

---

### 💡 Resumen para recordar:

* `var` se eleva y se inicializa automáticamente como `undefined*` (deja tu código vulnerable a bugs).
* `let` y `const` se elevan pero entran en la **TDZ**, obligándote a escribir un código más limpio y ordenado donde siempre declaras las cosas antes de usarlas.

## Tipos de Datos (Data Types)

En JavaScript, los datos se dividen principalmente en **Primitivos** (valores únicos e inmutables) y **Complejos**.

### 1. Datos Primitivos

Son los valores más básicos del lenguaje. No son objetos y no tienen métodos* propios:

* **String:** Cadenas de texto. Se escriben entre comillas. `let nombre = "Luis"`
* **Number:** Números de cualquier tipo (enteros o decimales). `let edad = 25` o `let pi = 3.14`
* **Boolean:** Solo puede tener dos valores: `true` (verdadero) o `false` (falso). Se usa para evaluar condiciones.
* **Null:** Representa la ausencia intencional de cualquier valor. Está vacío porque tú decidiste dejarlo vacío. `let datos = null`
* **Undefined*:** Significa que una variable ha sido declarada (inicializada), pero aún no se le ha asignado un valor. `let x; // x es undefined`
* **Symbol / BigInt:** (Avanzados) Se usan para identificadores únicos y números extremadamente grandes.

### 2. Datos Complejos (Estructuras de Datos Objeto)

A diferencia de los primitivos, estos pueden almacenar colecciones de valores, estructuras complejas o funciones. No se copian por valor sino por referencia en memoria y son mutables*:

* **Objects (Objetos):** Colecciones de propiedades en pares de clave-valor. `let usuario = { nombre: "Luis", edad: 25 }`
* **Arrays (Arreglos/Matrices):** Listas ordenadas de datos guardados en una sola variable. `let colores = ["azul", "rojo", "verde"]`
* **Functions (Funciones):** Bloques de código ejecutables y reutilizables.

---

## 📖 Glosario de Términos

* **Backend / Frontend**
  El *Frontend* es la parte visual de una aplicación con la que interactúa el usuario (la pantalla). El *Backend* es la lógica detrás de escena, la base de datos y el servidor que procesa la información.

* **ECMAscript 6 (ES6)**
  Es el estándar oficial que regula el lenguaje JavaScript. La versión 6 (lanzada en 2015) fue una de las actualizaciones más importantes de la historia del lenguaje, introduciendo `let`, `const`, funciones flecha y más.

* **Express**
  Una librería (framework) popular de JavaScript que corre en el Backend para crear servidores web y APIs de forma rápida y minimalista.

* **Inmutabilidad**
  Propiedad de un dato que impide que su valor original sea modificado una vez creado. Si requieres cambiarlo, debes generar un dato nuevo en su lugar.

* **Métodos**
  Son funciones especiales que "viven" dentro de un objeto o tipo de dato y sirven para realizar acciones con ellos (por ejemplo, `.toUpperCase()` para pasar un texto a mayúsculas).

* **Mutabilidad (Mutable)**
  La capacidad que tiene un dato u objeto de cambiar, reconfigurarse o modificar su contenido interno a lo largo del tiempo sin necesidad de ser destruido.

* **PERN Stack**
