A continuación tienes un resumen en markdown sobre las variables en JavaScript, adecuado para documentación técnica o estudio, teniendo en cuenta tu experiencia en programación y uso de este formato:

***
# Variables en JavaScript

Las **variables** en JavaScript se utilizan para almacenar datos que pueden ser usados y modificados a lo largo del código.
Cuidado por que las variables en Javascript distinguen MAYUSCULAS y minusculas.

## Declaración de variables

Puedes declarar variables con tres palabras clave principales:

- **var**:  
  **No la utilizaremos a menos que deseemos una compatibilidad anterior.**
  Declaración clásica. Tiene ámbito de función (*function scope*) y permite redeclaración y reasignación.

- **let**:  
  **Básicamente si queremos asignar el valor a una variable y esta va a cambiar, utilizaremos esta forma**
  Introducida en ES6. Tiene ámbito de bloque (*block scope*) y permite reasignación pero no redeclaración en el mismo ámbito.

- **const**:  
  **Utilizaremos esta forma siempre u cuando el valor no vaya a cambiar.**
  También desde ES6. Ámbito de bloque. No permite reasignación ni redeclaración. El valor debe asignarse al declararse.

## Ejemplos de declaración

```javascript
var nombre = 'Juan';
let edad = 30;
const PI = 3.1416;
```

## Buenas prácticas

- Usa **let** si la variable puede cambiar.
- Usa **const** para valores constantes o referencias a objetos/arrays que no deben reasignarse.
- Evita **var**, salvo para mantener compatibilidad con código antiguo.

## Ejemplo avanzado: Ámbito de bloque

```javascript
if (true) {
    let mensaje = 'Hola!';
    console.log(mensaje); // 'Hola!'
}
console.log(mensaje); // Error: mensaje is not defined
```

## Declaración de Objetos con const

### ¿Qué es un objeto?
Un **objeto** es una estructura de datos que almacena múltiples valores relacionados en pares **clave-valor**:

```javascript
// Declaración básica de objeto
const persona = { nombre: "Luis" };
```

**Desglose:**
- `const` = palabra clave para constante
- `persona` = nombre de la variable
- `{ }` = llaves que definen el objeto
- `nombre: "Luis"` = propiedad (clave: valor)

### Objetos más complejos:

```javascript
const estudiante = {
    nombre: "Ana",
    edad: 22,
    activo: true,
    materias: ["JavaScript", "CSS", "HTML"],
    direccion: {
        calle: "Principal 123",
        ciudad: "Madrid"
    },
    saludar: function() {
        return `Hola, soy ${this.nombre}`;
    }
};
```

### Acceso a propiedades:

```javascript
const persona = { 
    nombre: "Luis", 
    edad: 30,
    "nombre completo": "Luis García" 
};

// Notación de punto
console.log(persona.nombre);        // "Luis"
console.log(persona.edad);          // 30

// Notación de corchetes
console.log(persona["nombre"]);     // "Luis"
console.log(persona["nombre completo"]); // "Luis García"
```

### Modificación de objetos con const:

```javascript
const persona = { nombre: "Luis" };

// ✅ PERMITIDO: Modificar propiedades existentes
persona.nombre = "Carlos";

// ✅ PERMITIDO: Añadir nuevas propiedades
persona.edad = 25;
persona.activo = true;

// ✅ PERMITIDO: Eliminar propiedades
delete persona.activo;

// ❌ ERROR: No se puede reasignar el objeto completo
persona = {}; // TypeError: Assignment to constant variable
```

### ¿Por qué se puede modificar un objeto const?

```javascript
const persona = { nombre: "Luis" };
```

- `const` protege la **referencia** al objeto, no su **contenido**
- Es como una caja etiquetada: no puedes cambiar la etiqueta, pero sí el contenido
- El objeto sigue siendo el mismo en memoria, solo cambian sus propiedades

## Tipos de datos en variables

JavaScript es **dinámicamente tipado**, las variables pueden contener diferentes tipos:

### Tipos primitivos:
```javascript
let texto = "Hola";           // string
let numero = 42;              // number
let decimal = 3.14;           // number
let esVerdadero = true;       // boolean
let indefinido;               // undefined
let nulo = null;              // null (objeto especial)
let simbolo = Symbol('id');   // symbol (ES6)
let enteroGrande = 123n;      // bigint (ES2020)
```

### Tipos de referencia:
```javascript
let objeto = { nombre: "Ana" };
let arreglo = [1, 2, 3];
let funcion = function() { return "Hola"; };
let fecha = new Date();
```

## Coerción de tipos

JavaScript convierte automáticamente tipos cuando es necesario:

```javascript
let resultado = "5" + 3;      // "53" (string)
let suma = +"5" + 3;          // 8 (number)
let booleano = !!"texto";     // true
let numero = Number("42");    // 42
let texto = String(123);      // "123"
```

## Ámbito (Scope) avanzado

### ¿Qué es el ámbito (scope)?

El **ámbito** determina dónde y cómo puedes acceder a una variable en tu código. Es como las "reglas de visibilidad" de las variables.

### Ámbito global - Explicación detallada

El **ámbito global** es el contexto principal de tu programa. Las variables declaradas aquí son visibles desde cualquier lugar del código.

```javascript
// Estas variables están en el ámbito global
var globalVar = "Soy global";
let globalLet = "También global";
const globalConst = "Constante global";

function miFuncion() {
    // Desde aquí puedo acceder a todas las variables globales
    console.log(globalVar);   // "Soy global"
    console.log(globalLet);   // "También global"
    console.log(globalConst); // "Constante global"
}

// También puedo acceder desde otros lugares
if (true) {
    console.log(globalVar); // "Soy global"
}

// O desde otra función
function otraFuncion() {
    console.log(globalLet); // "También global"
}
```

### Problemas del ámbito global

#### 1. Contaminación del espacio global:
```javascript
// Malo: muchas variables globales
var nombre = "Juan";
var edad = 30;
var activo = true;
var contador = 0;

// Mejor: agrupar en un objeto
const usuario = {
    nombre: "Juan",
    edad: 30,
    activo: true,
    contador: 0
};
```

#### 2. Conflictos de nombres:
```javascript
// Archivo 1
var datos = "Información importante";

// Archivo 2 (cargado después)
var datos = "Otros datos"; // ¡Sobrescribió la variable anterior!

console.log(datos); // "Otros datos" - Se perdió la información original
```

#### 3. Variables accidentalmente globales:
```javascript
function procesarDatos() {
    // ¡Olvido declarar la variable!
    resultado = "Procesado"; // Se convierte en global automáticamente
}

procesarDatos();
console.log(resultado); // "Procesado" - ¡Variable global accidental!
```

### Buenas prácticas para el ámbito global

#### 1. Minimizar variables globales:
```javascript
// Malo
var nombre = "Ana";
var edad = 25;
var procesarUsuario = function() { /* ... */ };

// Bueno - Usar un namespace
const MiApp = {
    usuario: {
        nombre: "Ana",
        edad: 25
    },
    utilidades: {
        procesarUsuario: function() { /* ... */ }
    }
};
```
### Ámbito de función vs bloque:
```javascript
function ejemploAmbito() {
    if (true) {
        var funcionVar = "Visible en toda la función";
        let bloqueVar = "Solo visible en este bloque";
    }
    
    console.log(funcionVar); // "Visible en toda la función"
    console.log(bloqueVar);  // ReferenceError
}
```

## Buenas prácticas avanzadas

1. **Usa nombres descriptivos:**
```javascript
// Malo
const d = new Date();
const u = users.filter(u => u.a);

// Bueno
const currentDate = new Date();
const activeUsers = users.filter(user => user.isActive);
```

2. **Evita variables globales:**
```javascript
// Malo
var contador = 0;

// Bueno
(function() {
    let contador = 0;
    // Lógica aquí
})();
```

3. **Usa const por defecto:**
```javascript
// Prefiere const
const configuracion = { tema: "oscuro" };
const usuarios = ["Ana", "Luis"];

// Solo usa let si necesitas reasignar
let contador = 0;
for (let i = 0; i < 10; i++) {
    contador += i;
}
```

## Errores comunes con variables

### 1. Redeclaración accidental:
```javascript
let mensaje = "Hola";
let mensaje = "Adiós"; // SyntaxError
```

### 2. Modificar const incorrectamente:
```javascript
const numero = 5;
numero = 10; // TypeError
```

### 3. Usar variables antes de declararlas:
```javascript
console.log(variable); // ReferenceError
let variable = "valor";
```

***