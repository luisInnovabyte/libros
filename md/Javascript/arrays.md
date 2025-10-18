# Arrays en JavaScript

Los **arrays** (arreglos o matrices) son estructuras de datos fundamentales en JavaScript que permiten almacenar múltiples valores en una sola variable. Son colecciones ordenadas de elementos que pueden ser de cualquier tipo.

## ¿Qué es un Array?

Un array es una lista ordenada de elementos donde cada elemento tiene un **índice** (posición) que comienza desde 0.

```javascript
// Array simple
const frutas = ["manzana", "banana", "naranja"];

// Los índices van de 0 a length-1
// Índice:     0         1        2
// Valor:   "manzana" "banana" "naranja"
```

## Características principales

- **Ordenados**: Los elementos mantienen su posición
- **Indexados**: Cada elemento tiene un índice numérico (0, 1, 2...)
- **Dinámicos**: Pueden crecer o reducirse durante la ejecución
- **Heterogéneos**: Pueden contener diferentes tipos de datos
- **Mutables**: Se pueden modificar después de su creación

## Creación de Arrays

### 1. Literal de array (más común)

```javascript
// Array vacío
const arrayVacio = [];

// Array con elementos
const numeros = [1, 2, 3, 4, 5];
const colores = ["rojo", "verde", "azul"];
const mixto = [1, "texto", true, null, undefined];

// Array con diferentes tipos de datos
const variado = [
    42,                    // number
    "JavaScript",          // string
    true,                  // boolean
    { nombre: "Juan" },    // object
    [1, 2, 3],            // array anidado
    function() { return "hola"; }  // function
];
```

## Acceso a elementos

### Índices positivos (desde el inicio)

```javascript
const frutas = ["manzana", "banana", "naranja", "uva"];

console.log(frutas[0]);   // "manzana" (primer elemento)
console.log(frutas[1]);   // "banana"
console.log(frutas[2]);   // "naranja"
console.log(frutas[3]);   // "uva" (último elemento)

// Acceso a índice que no existe
console.log(frutas[10]);  // undefined
```

### Acceso dinámico

```javascript
const datos = ["a", "b", "c", "d", "e"];

// Usando variables como índices
const indice = 2;
console.log(datos[indice]);  // "c"

// En bucles
for (let i = 0; i < datos.length; i++) {
    console.log(`Índice ${i}: ${datos[i]}`);
}

// Con expresiones
const mitad = Math.floor(datos.length / 2);
console.log(datos[mitad]);  // "c"
```

## Propiedades importantes

### length

```javascript
const colores = ["rojo", "verde", "azul"];

console.log(colores.length);  // 3

// Modificar length cambia el array
colores.length = 2;
console.log(colores);  // ["rojo", "verde"]

colores.length = 5;
console.log(colores);  // ["rojo", "verde", undefined, undefined, undefined]

// Vaciar array con length
colores.length = 0;
console.log(colores);  // []
```

## Modificación de arrays

### Cambiar elementos existentes

```javascript
const frutas = ["manzana", "banana", "naranja"];

// Cambiar elemento por índice
frutas[1] = "kiwi";
console.log(frutas);  // ["manzana", "kiwi", "naranja"]

// Cambiar múltiples elementos
frutas[0] = "fresa";
frutas[2] = "piña";
console.log(frutas);  // ["fresa", "kiwi", "piña"]
```

### Añadir elementos

```javascript
const numeros = [1, 2, 3];

// Añadir al final con índice
numeros[3] = 4;
numeros[4] = 5;
console.log(numeros);  // [1, 2, 3, 4, 5]

// Crear "huecos" (sparse arrays)
numeros[10] = 11;
console.log(numeros);        // [1, 2, 3, 4, 5, undefined, undefined, undefined, undefined, undefined, 11]
console.log(numeros.length); // 11
```

## Métodos básicos de arrays

### push() - Añadir al final

```javascript
const animales = ["gato", "perro"];

// Añadir uno o más elementos al final
animales.push("pájaro");
console.log(animales);  // ["gato", "perro", "pájaro"]

animales.push("pez", "hamster");
console.log(animales);  // ["gato", "perro", "pájaro", "pez", "hamster"]

// push() devuelve la nueva longitud
const nuevaLongitud = animales.push("conejo");
console.log(nuevaLongitud);  // 6
```

### pop() - Eliminar del final

```javascript
const frutas = ["manzana", "banana", "naranja"];

// Eliminar y obtener último elemento
const ultimaFruta = frutas.pop();
console.log(ultimaFruta);  // "naranja"
console.log(frutas);       // ["manzana", "banana"]

// pop() en array vacío devuelve undefined
const vacio = [];
console.log(vacio.pop());  // undefined
```

### unshift() - Añadir al inicio

```javascript
const numeros = [2, 3, 4];

// Añadir al inicio
numeros.unshift(1);
console.log(numeros);  // [1, 2, 3, 4]

numeros.unshift(-1, 0);
console.log(numeros);  // [-1, 0, 1, 2, 3, 4]

// unshift() devuelve la nueva longitud
const longitud = numeros.unshift(-2);
console.log(longitud);  // 7
```

### shift() - Eliminar del inicio

```javascript
const colores = ["rojo", "verde", "azul"];

// Eliminar y obtener primer elemento
const primerColor = colores.shift();
console.log(primerColor);  // "rojo"
console.log(colores);      // ["verde", "azul"]

// shift() en array vacío devuelve undefined
const vacio = [];
console.log(vacio.shift());  // undefined
```

## Verificar si es un array

```javascript
const array = [1, 2, 3];
const objeto = { 0: 1, 1: 2, length: 2 };
const string = "hola";

// Método recomendado
console.log(Array.isArray(array));   // true
console.log(Array.isArray(objeto));  // false
console.log(Array.isArray(string));  // false
```

## Arrays multidimensionales

### Arrays de arrays (matrices)

```javascript
// Matriz 2D
const matriz = [
    [1, 2, 3],
    [4, 5, 6], 
    [7, 8, 9]
];

// Acceso a elementos
console.log(matriz[0][0]);  // 1 (fila 0, columna 0)
console.log(matriz[1][2]);  // 6 (fila 1, columna 2)
console.log(matriz[2][1]);  // 8 (fila 2, columna 1)

// Modificar elementos
matriz[0][1] = 10;
console.log(matriz[0]);  // [1, 10, 3]
```

### Crear matrices dinámicamente

```javascript
// Crear matriz 3x3 con ceros
const filas = 3;
const columnas = 3;

const matriz = [];
for (let i = 0; i < filas; i++) {
    matriz[i] = [];
    for (let j = 0; j < columnas; j++) {
        matriz[i][j] = 0;
    }
}

// Usando Array.from()
const matriz2 = Array.from({ length: 3 }, () => 
    Array.from({ length: 3 }, () => 0)
);

console.log(matriz2);
// [
//   [0, 0, 0],
//   [0, 0, 0],
//   [0, 0, 0]
// ]
```

### Arrays complejos

```javascript
// Array de objetos
const usuarios = [
    { id: 1, nombre: "Ana", edad: 25 },
    { id: 2, nombre: "Luis", edad: 30 },
    { id: 3, nombre: "María", edad: 28 }
];

// Acceso a propiedades de objetos en array
console.log(usuarios[0].nombre);  // "Ana"
console.log(usuarios[1].edad);    // 30

// Array de funciones
const operaciones = [
    (a, b) => a + b,
    (a, b) => a - b,
    (a, b) => a * b,
    (a, b) => a / b
];

console.log(operaciones[0](5, 3));  // 8 (suma)
console.log(operaciones[2](4, 6));  // 24 (multiplicación)
```

## Iteración de arrays

### for tradicional

```javascript
const frutas = ["manzana", "banana", "naranja"];

for (let i = 0; i < frutas.length; i++) {
    console.log(`${i}: ${frutas[i]}`);
}
// 0: manzana
// 1: banana
// 2: naranja
```

### for...of (ES2015) - Recomendado

```javascript
const numeros = [10, 20, 30];

// Obtener valores
for (const numero of numeros) {
    console.log(numero);  // 10, 20, 30
}

// Con índice usando entries()
for (const [indice, valor] of numeros.entries()) {
    console.log(`${indice}: ${valor}`);
}
```

### for...in (no recomendado para arrays)

```javascript
const colores = ["rojo", "verde", "azul"];

// for...in itera sobre índices (como strings)
for (const indice in colores) {
    console.log(`${indice}: ${colores[indice]}`);
}
// ⚠️ Puede incluir propiedades heredadas
```

### forEach() - Método funcional

```javascript
const nombres = ["Ana", "Luis", "María"];

// Función de callback recibe (elemento, índice, array)
nombres.forEach((nombre, indice) => {
    console.log(`${indice}: ${nombre}`);
});

// Con función separada
function mostrarElemento(elemento, indice, array) {
    console.log(`Posición ${indice}: ${elemento} (Array de ${array.length} elementos)`);
}

nombres.forEach(mostrarElemento);
```

## Búsqueda en arrays

### indexOf() y lastIndexOf()

```javascript
const frutas = ["manzana", "banana", "manzana", "naranja"];

// Buscar primera aparición
console.log(frutas.indexOf("manzana"));     // 0
console.log(frutas.indexOf("banana"));      // 1
console.log(frutas.indexOf("kiwi"));        // -1 (no encontrado)

// Buscar desde posición específica
console.log(frutas.indexOf("manzana", 1));  // 2

// Buscar última aparición
console.log(frutas.lastIndexOf("manzana")); // 2
```

### includes() - ES2016

```javascript
const numeros = [1, 2, 3, 4, 5];

console.log(numeros.includes(3));     // true
console.log(numeros.includes(10));    // false
console.log(numeros.includes(2, 2));  // false (buscar desde índice 2)

// Con NaN
const arrayConNaN = [1, NaN, 3];
console.log(arrayConNaN.includes(NaN));      // true
console.log(arrayConNaN.indexOf(NaN));       // -1 (indexOf no encuentra NaN)
```

### find() y findIndex() - ES2015

```javascript
const usuarios = [
    { id: 1, nombre: "Ana", activo: true },
    { id: 2, nombre: "Luis", activo: false },
    { id: 3, nombre: "María", activo: true }
];

// Encontrar primer elemento que cumple condición
const usuarioActivo = usuarios.find(user => user.activo);
console.log(usuarioActivo);  // { id: 1, nombre: "Ana", activo: true }

// Encontrar índice del primer elemento que cumple condición
const indiceInactivo = usuarios.findIndex(user => !user.activo);
console.log(indiceInactivo);  // 1

// Si no encuentra, devuelve undefined y -1 respectivamente
const noEncontrado = usuarios.find(user => user.edad > 50);
console.log(noEncontrado);  // undefined
```

#### Explicación detallada de find()

El método `find()` es muy potente y merece una explicación más profunda:

**¿Qué hace exactamente?**
```javascript
const usuarioActivo = usuarios.find(user => user.activo);
//                    ^^^^^^^     ^^^^    ^^^^^^^^^^^^^
//                       |         |           |
//                    método    parámetro   condición
```

**1. La función callback**: `user => user.activo`
```javascript
// Esta arrow function equivale a:
function(user) {
    return user.activo;  // Devuelve true o false
}

// 'user' representa cada elemento durante la iteración
// 'user.activo' accede a la propiedad 'activo' del objeto actual
```

**2. Cómo funciona internamente**:
```javascript
// find() hace algo similar a esto:
for (let i = 0; i < usuarios.length; i++) {
    const user = usuarios[i];           // Elemento actual
    const cumpleCondicion = user.activo; // Ejecutar condición
    
    if (cumpleCondicion) {              // Si es truthy
        return user;                    // Devolver elemento completo
    }
}
return undefined;  // Si no encuentra nada
```

**3. Paso a paso con nuestro ejemplo**:
```javascript
// Iteración 1: user = { id: 1, nombre: "Ana", activo: true }
// user.activo = true ✓
// ¡Encontrado! Devuelve: { id: 1, nombre: "Ana", activo: true }
// Se detiene aquí, no continúa iterando
```

**4. Diferentes formas de escribir la misma condición**:
```javascript
// Forma 1: Arrow function implícita (recomendada)
const activo1 = usuarios.find(user => user.activo);

// Forma 2: Arrow function explícita
const activo2 = usuarios.find(user => {
    return user.activo;
});

// Forma 3: Función tradicional
const activo3 = usuarios.find(function(user) {
    return user.activo;
});

// Forma 4: Condición más explícita
const activo4 = usuarios.find(user => user.activo === true);
```

**5. Más ejemplos prácticos**:
```javascript
const usuarios = [
    { id: 1, nombre: "Ana", activo: true, edad: 25 },
    { id: 2, nombre: "Luis", activo: false, edad: 30 },
    { id: 3, nombre: "María", activo: true, edad: 28 }
];

// Buscar por nombre específico
const ana = usuarios.find(user => user.nombre === "Ana");
console.log(ana);  // { id: 1, nombre: "Ana", activo: true, edad: 25 }

// Buscar por ID
const usuario2 = usuarios.find(user => user.id === 2);
console.log(usuario2);  // { id: 2, nombre: "Luis", activo: false, edad: 30 }

// Condición numérica
const mayorDe27 = usuarios.find(user => user.edad > 27);
console.log(mayorDe27);  // { id: 2, nombre: "Luis", activo: false, edad: 30 }

// Condiciones múltiples con AND
const activoJoven = usuarios.find(user => 
    user.activo === true && user.edad < 27
);
console.log(activoJoven);  // { id: 1, nombre: "Ana", activo: true, edad: 25 }
```

**6. Manejo cuando no encuentra resultados**:
```javascript
// Buscar algo inexistente
const noExiste = usuarios.find(user => user.nombre === "Pedro");
console.log(noExiste);  // undefined

// Siempre verificar antes de usar el resultado
const usuarioBuscado = usuarios.find(user => user.edad > 50);

if (usuarioBuscado) {
    console.log(`Encontrado: ${usuarioBuscado.nombre}`);
} else {
    console.log("No se encontró ningún usuario con esa edad");
}

// Usar operador nullish coalescing (??)
const resultado = usuarioBuscado ?? { nombre: "Usuario por defecto" };
```

**7. find() vs otros métodos similares**:
```javascript
const numeros = [1, 2, 3, 4, 5];

// find() → Devuelve el ELEMENTO completo
const elemento = usuarios.find(user => user.activo);
// Resultado: { id: 1, nombre: "Ana", activo: true }

// findIndex() → Devuelve el ÍNDICE (posición)
const indice = usuarios.findIndex(user => user.activo);
// Resultado: 0

// filter() → Devuelve ARRAY con TODOS los que cumplen
const todosActivos = usuarios.filter(user => user.activo);
// Resultado: [{ id: 1, ... }, { id: 3, ... }]

// some() → Devuelve true/false si ALGUNO cumple
const hayActivos = usuarios.some(user => user.activo);
// Resultado: true

// every() → Devuelve true/false si TODOS cumplen
const todosActivos2 = usuarios.every(user => user.activo);
// Resultado: false
```

**8. Caso práctico: Sistema de autenticación**
```javascript
function autenticarUsuario(email, password) {
    const usuarios = [
        { id: 1, email: "ana@gmail.com", password: "123", activo: true, rol: "admin" },
        { id: 2, email: "luis@gmail.com", password: "456", activo: false, rol: "user" },
        { id: 3, email: "maria@gmail.com", password: "789", activo: true, rol: "user" }
    ];
    
    // Buscar usuario que coincida credenciales y esté activo
    const usuario = usuarios.find(user => 
        user.email === email && 
        user.password === password && 
        user.activo === true
    );
    
    if (usuario) {
        return {
            success: true,
            message: `Bienvenido ${usuario.email}`,
            usuario: usuario
        };
    } else {
        return {
            success: false,
            message: "Credenciales inválidas o usuario inactivo",
            usuario: null
        };
    }
}

// Uso del sistema
const login1 = autenticarUsuario("ana@gmail.com", "123");
console.log(login1);
// { success: true, message: "Bienvenido ana@gmail.com", usuario: {...} }

const login2 = autenticarUsuario("luis@gmail.com", "456");
console.log(login2);
// { success: false, message: "Credenciales inválidas o usuario inactivo", usuario: null }
```

**Puntos clave para recordar:**
- `find()` devuelve el **primer elemento** que cumple la condición
- Se **detiene** en cuanto encuentra uno (no sigue buscando)
- Devuelve `undefined` si **no encuentra nada**
- La función callback debe devolver un valor **truthy/falsy**
- Es **perfecto** cuando necesitas un objeto específico del array

## Conversión de arrays

### toString() y join()

```javascript
const frutas = ["manzana", "banana", "naranja"];

// Convertir a string con comas
console.log(frutas.toString());  // "manzana,banana,naranja"

// Convertir con separador personalizado
console.log(frutas.join());      // "manzana,banana,naranja"
console.log(frutas.join(" - "));  // "manzana - banana - naranja"
console.log(frutas.join(""));     // "manzanabanananaranja"
console.log(frutas.join(" | "));  // "manzana | banana | naranja"
```

### Array desde string

```javascript
// split() convierte string a array
const texto = "rojo,verde,azul";
const colores = texto.split(",");
console.log(colores);  // ["rojo", "verde", "azul"]

// Con diferentes separadores
const frase = "JavaScript es genial";
const palabras = frase.split(" ");
console.log(palabras);  // ["JavaScript", "es", "genial"]

// Limitar número de elementos
const limitado = frase.split(" ", 2);
console.log(limitado);  // ["JavaScript", "es"]
```

## Concatenación de arrays

### concat()

```javascript
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const array3 = [7, 8];

// Concatenar arrays (no modifica originales)
const resultado = array1.concat(array2);
console.log(resultado);  // [1, 2, 3, 4, 5, 6]

// Concatenar múltiples arrays
const todoJunto = array1.concat(array2, array3);
console.log(todoJunto);  // [1, 2, 3, 4, 5, 6, 7, 8]

// Concatenar elementos individuales
const conElementos = array1.concat(99, 100);
console.log(conElementos);  // [1, 2, 3, 99, 100]
```

### Spread operator (...) - ES2015

```javascript
const frutas = ["manzana", "banana"];
const verduras = ["lechuga", "tomate"];

// Concatenar con spread
const alimentos = [...frutas, ...verduras];
console.log(alimentos);  // ["manzana", "banana", "lechuga", "tomate"]

// Añadir elementos en cualquier posición
const mezcla = ["inicio", ...frutas, "medio", ...verduras, "final"];
console.log(mezcla);
// ["inicio", "manzana", "banana", "medio", "lechuga", "tomate", "final"]
```

## Extracción de porciones

### slice()

```javascript
const numeros = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

// Desde índice hasta el final
console.log(numeros.slice(3));      // [3, 4, 5, 6, 7, 8, 9]

// Entre dos índices (fin no incluido)
console.log(numeros.slice(2, 6));   // [2, 3, 4, 5]

// Con índices negativos
console.log(numeros.slice(-3));     // [7, 8, 9] (últimos 3)
console.log(numeros.slice(-5, -2)); // [5, 6, 7]

// Copiar array completo
const copia = numeros.slice();
console.log(copia);  // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## Arrays como copia vs referencia

```javascript
// Los arrays se asignan por referencia
const original = [1, 2, 3];
const referencia = original;

referencia.push(4);
console.log(original);    // [1, 2, 3, 4] (¡se modificó!)
console.log(referencia);  // [1, 2, 3, 4]

// Hacer copias superficiales
const copia1 = [...original];           // Spread operator
const copia2 = Array.from(original);    // Array.from()
const copia3 = original.slice();        // slice()

copia1.push(5);
console.log(original);  // [1, 2, 3, 4] (no se modificó)
console.log(copia1);    // [1, 2, 3, 4, 5]

// Copia profunda (para arrays anidados)
const anidado = [[1, 2], [3, 4]];
const copiaSuperficial = [...anidado];
copiaSuperficial[0].push(3);
console.log(anidado);  // [[1, 2, 3], [3, 4]] (¡se modificó!)

// Para copia profunda simple
const copiaProfunda = JSON.parse(JSON.stringify(anidado));
copiaProfunda[0].push(99);
console.log(anidado);       // [[1, 2, 3], [3, 4]] (no se modificó)
console.log(copiaProfunda); // [[1, 2, 3, 99], [3, 4]]
```

## Casos prácticos básicos

### 1. Lista de tareas

```javascript
let tareas = [];

// Añadir tareas
function agregarTarea(descripcion) {
    const tarea = {
        id: Date.now(),
        descripcion: descripcion,
        completada: false,
        fecha: new Date()
    };
    tareas.push(tarea);
    return tarea;
}

// Marcar como completada
function completarTarea(id) {
    const tarea = tareas.find(t => t.id === id);
    if (tarea) {
        tarea.completada = true;
    }
}

// Eliminar tarea
function eliminarTarea(id) {
    const indice = tareas.findIndex(t => t.id === id);
    if (indice !== -1) {
        tareas.splice(indice, 1);
    }
}

// Uso
agregarTarea("Comprar leche");
agregarTarea("Estudiar JavaScript");
agregarTarea("Hacer ejercicio");

console.log(tareas);
completarTarea(tareas[0].id);
console.log(tareas);
```

### 2. Carrito de compras

```javascript
let carrito = [];

function agregarProducto(producto, cantidad = 1) {
    const existente = carrito.find(item => item.producto.id === producto.id);
    
    if (existente) {
        existente.cantidad += cantidad;
    } else {
        carrito.push({
            producto: producto,
            cantidad: cantidad
        });
    }
}

function removerProducto(idProducto) {
    const indice = carrito.findIndex(item => item.producto.id === idProducto);
    if (indice !== -1) {
        carrito.splice(indice, 1);
    }
}

function calcularTotal() {
    return carrito.reduce((total, item) => {
        return total + (item.producto.precio * item.cantidad);
    }, 0);
}

// Productos de ejemplo
const productos = [
    { id: 1, nombre: "Laptop", precio: 999.99 },
    { id: 2, nombre: "Mouse", precio: 25.50 },
    { id: 3, nombre: "Teclado", precio: 79.99 }
];

// Uso
agregarProducto(productos[0], 1);
agregarProducto(productos[1], 2);
console.log(carrito);
console.log("Total:", calcularTotal());
```

### 3. Historial de navegación

```javascript
class HistorialNavegacion {
    constructor(limite = 10) {
        this.paginas = [];
        this.indiceActual = -1;
        this.limite = limite;
    }
    
    // Navegar a nueva página
    navegar(url) {
        // Eliminar páginas hacia adelante si existían
        this.paginas = this.paginas.slice(0, this.indiceActual + 1);
        
        // Añadir nueva página
        this.paginas.push({
            url: url,
            timestamp: new Date(),
            titulo: `Página ${this.paginas.length + 1}`
        });
        
        this.indiceActual++;
        
        // Mantener límite de historial
        if (this.paginas.length > this.limite) {
            this.paginas.shift();
            this.indiceActual--;
        }
    }
    
    // Ir hacia atrás
    retroceder() {
        if (this.puedeRetroceder()) {
            this.indiceActual--;
            return this.paginas[this.indiceActual];
        }
        return null;
    }
    
    // Ir hacia adelante
    avanzar() {
        if (this.puedeAvanzar()) {
            this.indiceActual++;
            return this.paginas[this.indiceActual];
        }
        return null;
    }
    
    puedeRetroceder() {
        return this.indiceActual > 0;
    }
    
    puedeAvanzar() {
        return this.indiceActual < this.paginas.length - 1;
    }
    
    paginaActual() {
        return this.paginas[this.indiceActual] || null;
    }
}

// Uso
const historial = new HistorialNavegacion(5);
historial.navegar("https://google.com");
historial.navegar("https://github.com");
historial.navegar("https://stackoverflow.com");

console.log(historial.paginaActual());
console.log(historial.retroceder());
console.log(historial.avanzar());
```

***

Los arrays son una de las estructuras de datos más importantes en JavaScript. Dominar estos conceptos básicos te permitirá manejar colecciones de datos de manera efectiva y prepararte para métodos más avanzados como `map()`, `filter()`, `reduce()` y otros que veremos en guías posteriores.
