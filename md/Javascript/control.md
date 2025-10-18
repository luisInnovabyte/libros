# Estructuras de Control en JavaScript

Las **estructuras de control** son fundamentales en JavaScript para dirigir el flujo de ejecución del programa. Permiten tomar decisiones, repetir código y manejar diferentes situaciones según las condiciones que se presenten.

## ¿Qué son las Estructuras de Control?

Las estructuras de control determinan el orden en que se ejecutan las instrucciones de un programa. Sin ellas, el código se ejecutaría línea por línea de forma secuencial, pero con ellas podemos:

- **Tomar decisiones** (condicionales)
- **Repetir código** (bucles)
- **Saltar o interrumpir** la ejecución
- **Manejar múltiples opciones** (selección múltiple)

```javascript
// Ejecución secuencial simple
console.log("Línea 1");
console.log("Línea 2");
console.log("Línea 3");

// Con estructura de control
const edad = 18;
if (edad >= 18) {
    console.log("Es mayor de edad"); // Solo se ejecuta si la condición es verdadera
}
```

## 1. Estructuras Condicionales - if...else

### if básico

```javascript
const edad = 20;

// if simple
if (edad >= 18) {
    console.log("Puedes votar");
}

// if con múltiples declaraciones
if (edad >= 21) {
    console.log("Puedes beber alcohol en EE.UU.");
    console.log("Eres completamente adulto");
}
```

### if...else

```javascript
const temperatura = 25;

if (temperatura > 30) {
    console.log("Hace calor");
} else {
    console.log("Temperatura agradable");
}

// Ejemplo más complejo
const puntuacion = 85;

if (puntuacion >= 90) {
    console.log("Excelente - A");
} else {
    console.log("Buen trabajo - B o menos");
}
```

### if...else if...else

```javascript
const nota = 75;

if (nota >= 90) {
    console.log("Sobresaliente (A)");
} else if (nota >= 80) {
    console.log("Notable (B)");
} else if (nota >= 70) {
    console.log("Bien (C)");
} else if (nota >= 60) {
    console.log("Suficiente (D)");
} else {
    console.log("Insuficiente (F)");
}

// Ejemplo con diferentes tipos de condiciones
const usuario = {
    nombre: "Ana",
    edad: 25,
    premium: true,
    activo: true
};

if (!usuario.activo) {
    console.log("Usuario inactivo");
} else if (!usuario.premium && usuario.edad < 18) {
    console.log("Usuario menor, acceso limitado");
} else if (usuario.premium) {
    console.log("Usuario premium, acceso completo");
} else {
    console.log("Usuario estándar");
}
```

### Operador ternario (condición ? valor1 : valor2)

```javascript
// Sintaxis básica
const edad = 20;
const mensaje = edad >= 18 ? "Adulto" : "Menor";
console.log(mensaje); // "Adulto"

// Comparación con if...else
// Usando if...else
let tipo;
if (edad >= 18) {
    tipo = "Adulto";
} else {
    tipo = "Menor";
}

// Usando operador ternario (más conciso)
const tipo2 = edad >= 18 ? "Adulto" : "Menor";

// Ternarios anidados
const puntos = 1250;
const nivel = puntos >= 1000 ? "Experto" : 
              puntos >= 500 ? "Intermedio" : 
              puntos >= 100 ? "Principiante" : "Novato";

console.log(nivel); // "Experto"

// En funciones
const obtenerDescuento = (esPremium) => esPremium ? 0.2 : 0.05;

// En asignación de propiedades
const producto = {
    nombre: "Laptop",
    precio: 1000,
    descuento: usuario.premium ? 100 : 50
};
```

### Operadores lógicos en condicionales

```javascript
const usuario = {
    edad: 25,
    premium: true,
    activo: true,
    pais: "España"
};

// AND (&&) - todas las condiciones deben ser verdaderas
if (usuario.edad >= 18 && usuario.activo && usuario.premium) {
    console.log("Acceso completo permitido");
}

// OR (||) - al menos una condición debe ser verdadera
if (usuario.premium || usuario.edad >= 65) {
    console.log("Tiene descuento especial");
}

// NOT (!) - invierte el valor boolean
if (!usuario.activo) {
    console.log("Usuario inactivo");
}

// Combinaciones complejas
if ((usuario.premium && usuario.activo) || usuario.pais === "España") {
    console.log("Acceso a contenido especial");
}

// Paréntesis para claridad
if (usuario.edad >= 18 && (usuario.premium || usuario.pais === "VIP")) {
    console.log("Acceso autorizado");
}
```

### Truthy y Falsy en condicionales

```javascript
// Valores falsy: false, 0, "", null, undefined, NaN
// Valores truthy: todo lo demás

let nombre = "";
if (nombre) {
    console.log(`Hola ${nombre}`);
} else {
    console.log("Nombre no proporcionado");
}

// Verificar existencia de arrays
let lista = [];
if (lista.length) {
    console.log("La lista tiene elementos");
} else {
    console.log("La lista está vacía");
}

// Verificar objetos
let config = null;
if (config) {
    console.log("Configuración cargada");
} else {
    console.log("Sin configuración");
}

// Operador de coalescencia nula (??) - ES2020
const configuracion = config ?? { tema: "claro" };

// Encadenamiento opcional (?.) - ES2020
const email = usuario?.contacto?.email ?? "Sin email";
```

## 2. Switch - Selección Múltiple

### Switch básico

```javascript
const diaSemana = 3;
let nombreDia;

switch (diaSemana) {
    case 1:
        nombreDia = "Lunes";
        break;
    case 2:
        nombreDia = "Martes";
        break;
    case 3:
        nombreDia = "Miércoles";
        break;
    case 4:
        nombreDia = "Jueves";
        break;
    case 5:
        nombreDia = "Viernes";
        break;
    case 6:
        nombreDia = "Sábado";
        break;
    case 7:
        nombreDia = "Domingo";
        break;
    default:
        nombreDia = "Día inválido";
}

console.log(nombreDia); // "Miércoles"
```

### Switch sin break (fall-through)

```javascript
const mes = "enero";
let estacion;

switch (mes) {
    case "diciembre":
    case "enero":
    case "febrero":
        estacion = "Invierno";
        break;
    case "marzo":
    case "abril":
    case "mayo":
        estacion = "Primavera";
        break;
    case "junio":
    case "julio":
    case "agosto":
        estacion = "Verano";
        break;
    case "septiembre":
    case "octubre":
    case "noviembre":
        estacion = "Otoño";
        break;
    default:
        estacion = "Mes no válido";
}

console.log(estacion); // "Invierno"
```

### Switch con expresiones complejas

```javascript
const usuario = { tipo: "premium", puntos: 1500 };

switch (true) {
    case usuario.tipo === "premium" && usuario.puntos >= 1000:
        console.log("Usuario premium con muchos puntos");
        break;
    case usuario.tipo === "premium":
        console.log("Usuario premium");
        break;
    case usuario.puntos >= 500:
        console.log("Usuario con muchos puntos");
        break;
    default:
        console.log("Usuario estándar");
}

// Switch con return (en funciones)
function obtenerPrecioEnvio(zona) {
    switch (zona) {
        case "local":
            return 5;
        case "nacional":
            return 10;
        case "internacional":
            return 25;
        default:
            return 0;
    }
}

console.log(obtenerPrecioEnvio("nacional")); // 10
```

### Comparación: switch vs if...else

```javascript
const accion = "guardar";

// Con switch
switch (accion) {
    case "guardar":
        console.log("Guardando datos...");
        break;
    case "cargar":
        console.log("Cargando datos...");
        break;
    case "eliminar":
        console.log("Eliminando datos...");
        break;
    default:
        console.log("Acción no reconocida");
}

// Con if...else (equivalente)
if (accion === "guardar") {
    console.log("Guardando datos...");
} else if (accion === "cargar") {
    console.log("Cargando datos...");
} else if (accion === "eliminar") {
    console.log("Eliminando datos...");
} else {
    console.log("Acción no reconocida");
}

// Uso de objetos como alternativa a switch
const acciones = {
    guardar: () => console.log("Guardando datos..."),
    cargar: () => console.log("Cargando datos..."),
    eliminar: () => console.log("Eliminando datos...")
};

const ejecutarAccion = (accion) => {
    const funcion = acciones[accion];
    if (funcion) {
        funcion();
    } else {
        console.log("Acción no reconocida");
    }
};

ejecutarAccion("guardar"); // "Guardando datos..."
```

## 3. Bucles For

### for básico

```javascript
// Sintaxis: for (inicialización; condición; incremento)
for (let i = 0; i < 5; i++) {
    console.log(`Iteración: ${i}`);
}
// Salida: 0, 1, 2, 3, 4

// Recorrer un array
const frutas = ["manzana", "banana", "naranja"];
for (let i = 0; i < frutas.length; i++) {
    console.log(`${i}: ${frutas[i]}`);
}
// 0: manzana
// 1: banana
// 2: naranja

// Bucle hacia atrás
for (let i = frutas.length - 1; i >= 0; i--) {
    console.log(frutas[i]);
}
// naranja, banana, manzana

// Incremento personalizado
for (let i = 0; i <= 10; i += 2) {
    console.log(i); // 0, 2, 4, 6, 8, 10
}
```

### for...in (para propiedades de objetos)

```javascript
const persona = {
    nombre: "Juan",
    edad: 30,
    profesion: "Desarrollador",
    ciudad: "Madrid"
};

// Iterar sobre propiedades de objeto
for (let propiedad in persona) {
    console.log(`${propiedad}: ${persona[propiedad]}`);
}
// nombre: Juan
// edad: 30
// profesion: Desarrollador
// ciudad: Madrid

// Con arrays (no recomendado - usa for...of)
const colores = ["rojo", "verde", "azul"];
for (let indice in colores) {
    console.log(`${indice}: ${colores[indice]}`);
}
// 0: rojo (índice como string)
// 1: verde
// 2: azul

// Verificar propiedades propias (no heredadas)
for (let prop in persona) {
    if (persona.hasOwnProperty(prop)) {
        console.log(`${prop}: ${persona[prop]}`);
    }
}
```

### for...of (para elementos iterables) - ES2015

```javascript
const numeros = [1, 2, 3, 4, 5];

// Iterar sobre valores
for (let numero of numeros) {
    console.log(numero * 2);
}
// 2, 4, 6, 8, 10

// Con strings
const texto = "Hola";
for (let letra of texto) {
    console.log(letra);
}
// H, o, l, a

// Con Set
const conjunto = new Set([1, 2, 3, 3, 4]);
for (let valor of conjunto) {
    console.log(valor);
}
// 1, 2, 3, 4

// Con Map
const mapa = new Map([
    ["a", 1],
    ["b", 2],
    ["c", 3]
]);

for (let [clave, valor] of mapa) {
    console.log(`${clave}: ${valor}`);
}
// a: 1, b: 2, c: 3

// Obtener tanto índice como valor con entries()
const animales = ["gato", "perro", "pájaro"];
for (let [indice, animal] of animales.entries()) {
    console.log(`${indice}: ${animal}`);
}
// 0: gato, 1: perro, 2: pájaro
```

## 4. Bucles While y Do...While

### while

```javascript
// Sintaxis básica
let contador = 0;
while (contador < 5) {
    console.log(`Contador: ${contador}`);
    contador++; // ¡Importante! Evitar bucle infinito
}
// 0, 1, 2, 3, 4

// Ejemplo práctico: leer archivo línea por línea (simulado)
const lineas = ["línea 1", "línea 2", "línea 3"];
let indice = 0;

while (indice < lineas.length) {
    console.log(lineas[indice]);
    indice++;
}

// Bucle con condición compleja
let numero = 1;
let suma = 0;

while (suma < 100) {
    suma += numero;
    console.log(`Sumando ${numero}, total: ${suma}`);
    numero++;
}

// Búsqueda hasta encontrar elemento
const lista = [5, 8, 12, 3, 18, 7];
let posicion = 0;
let valorBuscado = 12;
let encontrado = false;

while (posicion < lista.length && !encontrado) {
    if (lista[posicion] === valorBuscado) {
        encontrado = true;
        console.log(`Encontrado en posición: ${posicion}`);
    } else {
        posicion++;
    }
}
```

### do...while

```javascript
// Se ejecuta al menos una vez, luego verifica la condición
let respuesta;
do {
    respuesta = prompt("¿Quieres continuar? (s/n)");
    console.log(`Respuesta: ${respuesta}`);
} while (respuesta !== "n" && respuesta !== "s");

// Ejemplo: validación de entrada
let numero;
do {
    numero = parseInt(prompt("Ingresa un número entre 1 y 10:"));
    if (numero < 1 || numero > 10 || isNaN(numero)) {
        console.log("Número inválido, intenta de nuevo");
    }
} while (numero < 1 || numero > 10 || isNaN(numero));

console.log(`Número válido: ${numero}`);

// Menú de opciones
let opcion;
do {
    console.log("\n--- MENÚ ---");
    console.log("1. Ver perfil");
    console.log("2. Editar perfil");
    console.log("3. Configuración");
    console.log("0. Salir");
    
    opcion = parseInt(prompt("Selecciona una opción:"));
    
    switch (opcion) {
        case 1:
            console.log("Mostrando perfil...");
            break;
        case 2:
            console.log("Editando perfil...");
            break;
        case 3:
            console.log("Configuración...");
            break;
        case 0:
            console.log("Saliendo...");
            break;
        default:
            console.log("Opción inválida");
    }
} while (opcion !== 0);
```

## 5. forEach y map (Métodos de Array)

### forEach - Iterar sin retornar

```javascript
const numeros = [1, 2, 3, 4, 5];

// forEach básico
numeros.forEach(function(numero) {
    console.log(numero * 2);
});
// 2, 4, 6, 8, 10

// forEach con arrow function
numeros.forEach(numero => console.log(numero * 2));

// forEach con índice y array completo
numeros.forEach((numero, indice, array) => {
    console.log(`Posición ${indice}: ${numero} (Array de ${array.length} elementos)`);
});

// Ejemplo práctico: procesar lista de usuarios
const usuarios = [
    { nombre: "Ana", edad: 25 },
    { nombre: "Luis", edad: 30 },
    { nombre: "María", edad: 28 }
];

usuarios.forEach(usuario => {
    console.log(`${usuario.nombre} tiene ${usuario.edad} años`);
    // Realizar operaciones como enviar emails, actualizar BD, etc.
});

// forEach no modifica el array original
let total = 0;
numeros.forEach(numero => {
    total += numero; // Efecto secundario válido
});
console.log(`Total: ${total}`); // 15
```

### map - Transformar y retornar nuevo array

```javascript
const numeros = [1, 2, 3, 4, 5];

// map básico - crear nuevo array transformado
const duplicados = numeros.map(function(numero) {
    return numero * 2;
});
console.log(duplicados); // [2, 4, 6, 8, 10]

// map con arrow function
const cuadrados = numeros.map(numero => numero ** 2);
console.log(cuadrados); // [1, 4, 9, 16, 25]

// map con objetos
const usuarios = [
    { nombre: "Ana", edad: 25 },
    { nombre: "Luis", edad: 30 },
    { nombre: "María", edad: 28 }
];

// Extraer solo los nombres
const nombres = usuarios.map(usuario => usuario.nombre);
console.log(nombres); // ["Ana", "Luis", "María"]

// Transformar objetos
const usuariosConCategoria = usuarios.map(usuario => ({
    ...usuario,
    categoria: usuario.edad >= 30 ? "Senior" : "Junior"
}));
console.log(usuariosConCategoria);
// [
//   { nombre: "Ana", edad: 25, categoria: "Junior" },
//   { nombre: "Luis", edad: 30, categoria: "Senior" },
//   { nombre: "María", edad: 28, categoria: "Junior" }
// ]

// map con índice
const numerosConIndice = numeros.map((numero, indice) => ({
    valor: numero,
    posicion: indice,
    esPar: numero % 2 === 0
}));

// Casos prácticos complejos
const productos = [
    { id: 1, nombre: "Laptop", precio: 1000 },
    { id: 2, nombre: "Mouse", precio: 25 },
    { id: 3, nombre: "Teclado", precio: 75 }
];

// Aplicar descuento y formato
const productosConDescuento = productos.map(producto => ({
    ...producto,
    precioOriginal: producto.precio,
    precioConDescuento: producto.precio * 0.9,
    precioFormateado: `$${(producto.precio * 0.9).toFixed(2)}`
}));
```

### Diferencias entre forEach y map

```javascript
const numeros = [1, 2, 3, 4, 5];

// ❌ Uso incorrecto de forEach (intentar retornar)
const resultadoIncorrecto = numeros.forEach(numero => numero * 2);
console.log(resultadoIncorrecto); // undefined

// ✅ Uso correcto de map (transformación)
const resultadoCorrecto = numeros.map(numero => numero * 2);
console.log(resultadoCorrecto); // [2, 4, 6, 8, 10]

// ❌ Uso incorrecto de map (efectos secundarios sin retorno)
numeros.map(numero => {
    console.log(numero); // Efecto secundario
    // No retorna nada - map devuelve [undefined, undefined, ...]
});

// ✅ Uso correcto de forEach (efectos secundarios)
numeros.forEach(numero => {
    console.log(numero); // Efecto secundario apropiado
    // No necesita retornar nada
});

// Cuándo usar cada uno:
// forEach: cuando quieres hacer algo CON cada elemento
// map: cuando quieres crear un nuevo array BASADO en cada elemento
```

## 6. Control de Flujo - break y continue

### break - Salir del bucle

```javascript
// break en for
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        break; // Sale del bucle cuando i es 5
    }
    console.log(i);
}
// Salida: 0, 1, 2, 3, 4

// break en while
let numero = 0;
while (true) { // Bucle infinito controlado
    console.log(numero);
    numero++;
    
    if (numero >= 5) {
        break; // Sale cuando numero es 5
    }
}

// break en búsqueda
const lista = [1, 3, 5, 7, 9, 2, 4, 6];
let encontrado = false;

for (let i = 0; i < lista.length; i++) {
    if (lista[i] % 2 === 0) { // Buscar primer número par
        console.log(`Primer par encontrado: ${lista[i]} en posición ${i}`);
        encontrado = true;
        break; // Sale en cuanto encuentra uno
    }
}
```

### continue - Saltar iteración actual

```javascript
// continue en for
for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) {
        continue; // Salta números pares
    }
    console.log(i);
}
// Salida: 1, 3, 5, 7, 9

// Procesar solo elementos válidos
const datos = [1, null, 3, undefined, 5, "", 7, 0, 9];

for (let i = 0; i < datos.length; i++) {
    if (!datos[i]) { // Si es falsy, saltar
        continue;
    }
    console.log(`Procesando: ${datos[i]}`);
}
// Salida: 1, 3, 5, 7, 9

// continue en bucle anidado
for (let i = 1; i <= 3; i++) {
    console.log(`--- Grupo ${i} ---`);
    
    for (let j = 1; j <= 5; j++) {
        if (j === 3) {
            continue; // Solo salta en el bucle interno
        }
        console.log(`  Elemento ${j}`);
    }
}
```

### Etiquetas (labels) para bucles anidados

```javascript
// Etiquetas para control preciso en bucles anidados
exterior: for (let i = 0; i < 3; i++) {
    interior: for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) {
            console.log("Saliendo de ambos bucles");
            break exterior; // Sale del bucle exterior
        }
        console.log(`i: ${i}, j: ${j}`);
    }
}

// Ejemplo práctico: búsqueda en matriz
const matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

let valorBuscado = 5;
let encontradoEnMatriz = false;

busqueda: for (let fila = 0; fila < matriz.length; fila++) {
    for (let col = 0; col < matriz[fila].length; col++) {
        if (matriz[fila][col] === valorBuscado) {
            console.log(`Encontrado ${valorBuscado} en fila ${fila}, columna ${col}`);
            encontradoEnMatriz = true;
            break busqueda; // Sale de ambos bucles
        }
    }
}
```

## 7. try...catch...finally - Manejo de Errores

### try...catch básico

```javascript
try {
    // Código que puede fallar
    let resultado = 10 / 0;
    console.log(resultado); // Infinity (no es error en JS)
    
    // Esto sí genera error
    let objeto = null;
    console.log(objeto.propiedad); // TypeError
    
} catch (error) {
    // Se ejecuta si hay error en try
    console.log("Error capturado:", error.message);
    console.log("Tipo de error:", error.name);
}

console.log("El programa continúa..."); // Se ejecuta normalmente
```

### try...catch...finally

```javascript
function procesarDatos(datos) {
    console.log("Iniciando procesamiento...");
    
    try {
        if (!datos) {
            throw new Error("Datos no proporcionados");
        }
        
        if (!Array.isArray(datos)) {
            throw new Error("Los datos deben ser un array");
        }
        
        // Procesar datos
        let resultado = datos.map(item => item * 2);
        console.log("Datos procesados:", resultado);
        return resultado;
        
    } catch (error) {
        console.log("Error en el procesamiento:", error.message);
        return null;
        
    } finally {
        // SIEMPRE se ejecuta, haya error o no
        console.log("Limpiando recursos...");
        console.log("Procesamiento terminado");
    }
}

// Pruebas
procesarDatos([1, 2, 3]); // Éxito
procesarDatos(null);       // Error controlado
procesarDatos("texto");    // Error controlado
```

### Manejo específico de errores

```javascript
function dividir(a, b) {
    try {
        if (typeof a !== "number" || typeof b !== "number") {
            throw new TypeError("Los argumentos deben ser números");
        }
        
        if (b === 0) {
            throw new Error("División por cero no permitida");
        }
        
        return a / b;
        
    } catch (error) {
        if (error instanceof TypeError) {
            console.log("Error de tipo:", error.message);
        } else if (error instanceof Error) {
            console.log("Error general:", error.message);
        }
        
        return null;
    }
}

// Async/await con try...catch
async function obtenerDatos() {
    try {
        const response = await fetch('https://api.ejemplo.com/datos');
        
        if (!response.ok) {
            throw new Error(`Error HTTP: ${response.status}`);
        }
        
        const datos = await response.json();
        return datos;
        
    } catch (error) {
        if (error.name === 'TypeError') {
            console.log("Error de red:", error.message);
        } else {
            console.log("Error al obtener datos:", error.message);
        }
        
        return [];
    }
}
```

## 8. Estructuras Avanzadas

### Operadores de Cortocircuito

```javascript
// AND (&&) - Evaluación perezosa
const usuario = { nombre: "Ana", premium: true };

// Solo ejecuta la función si el usuario es premium
usuario.premium && console.log("Usuario premium detectado");

// Asignación condicional
const descuento = usuario.premium && 0.2; // false o 0.2

// OR (||) - Valor por defecto
const nombre = usuario.nombre || "Usuario Anónimo";
const configuracion = usuario.config || { tema: "claro" };

// Nullish coalescing (??) - ES2020
const puerto = process.env.PORT ?? 3000; // Solo null/undefined
const valor = 0 ?? 5; // 0 (no se considera null/undefined)
```

### Optional Chaining - ES2020

```javascript
const usuario = {
    nombre: "Ana",
    direccion: {
        calle: "Main St",
        ciudad: "Madrid"
    }
};

// Sin optional chaining (riesgo de error)
// console.log(usuario.contacto.telefono); // Error!

// Con optional chaining
console.log(usuario.contacto?.telefono);        // undefined
console.log(usuario.direccion?.ciudad);         // "Madrid"
console.log(usuario.direccion?.pais?.codigo);   // undefined

// Con arrays
const lista = usuario.hobbies?.[0] ?? "Sin hobbies";

// Con funciones
usuario.saludar?.() ?? console.log("No hay método saludar");
```

### Destructuring con control de flujo

```javascript
const procesarPedido = ({ id, productos = [], cliente = {} }) => {
    // Validaciones con destructuring
    if (!id || productos.length === 0) {
        return { error: "Pedido inválido" };
    }
    
    const { nombre, email = "Sin email" } = cliente;
    
    if (!nombre) {
        return { error: "Cliente debe tener nombre" };
    }
    
    // Procesar pedido...
    return {
        pedidoId: id,
        clienteNombre: nombre,
        clienteEmail: email,
        totalProductos: productos.length
    };
};

// Array destructuring con valores por defecto
const [primero, segundo = "Por defecto", ...resto] = [1];
console.log(primero, segundo, resto); // 1, "Por defecto", []
```

## Casos Prácticos Complejos

### 1. Validador de formulario completo

```javascript
function validarFormulario(datos) {
    const errores = [];
    
    // Validar nombre
    if (!datos.nombre || datos.nombre.trim().length < 2) {
        errores.push("Nombre debe tener al menos 2 caracteres");
    }
    
    // Validar email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!datos.email || !emailRegex.test(datos.email)) {
        errores.push("Email no válido");
    }
    
    // Validar edad
    if (!datos.edad || datos.edad < 18 || datos.edad > 120) {
        errores.push("Edad debe estar entre 18 y 120 años");
    }
    
    // Validar contraseña
    if (!datos.password || datos.password.length < 8) {
        errores.push("Contraseña debe tener al menos 8 caracteres");
    }
    
    // Verificar caracteres especiales en contraseña
    const tieneEspecial = /[!@#$%^&*(),.?":{}|<>]/.test(datos.password);
    if (datos.password && !tieneEspecial) {
        errores.push("Contraseña debe contener al menos un caracter especial");
    }
    
    return {
        valido: errores.length === 0,
        errores: errores
    };
}

// Sistema de procesamiento con múltiples intentos
function procesarFormulario(datos, maxIntentos = 3) {
    let intentos = 0;
    let resultado;
    
    do {
        intentos++;
        console.log(`Intento ${intentos} de ${maxIntentos}`);
        
        resultado = validarFormulario(datos);
        
        if (resultado.valido) {
            console.log("Formulario válido, procesando...");
            return { exito: true, mensaje: "Formulario procesado correctamente" };
        } else {
            console.log("Errores encontrados:", resultado.errores);
            
            if (intentos < maxIntentos) {
                // Simular corrección de errores automática o manual
                datos = corregirErroresComunes(datos);
            }
        }
        
    } while (intentos < maxIntentos && !resultado.valido);
    
    return { 
        exito: false, 
        mensaje: "No se pudo procesar después de múltiples intentos",
        errores: resultado.errores 
    };
}
```

### 2. Sistema de paginación y filtros

```javascript
function procesarDatos(datos, opciones = {}) {
    const {
        pagina = 1,
        elementosPorPagina = 10,
        filtros = {},
        ordenamiento = {}
    } = opciones;
    
    let datosProcesados = [...datos];
    
    // Aplicar filtros
    for (const [campo, valor] of Object.entries(filtros)) {
        if (valor !== undefined && valor !== null && valor !== "") {
            datosProcesados = datosProcesados.filter(item => {
                if (typeof valor === "string") {
                    return item[campo]?.toString().toLowerCase().includes(valor.toLowerCase());
                } else {
                    return item[campo] === valor;
                }
            });
        }
    }
    
    // Aplicar ordenamiento
    if (ordenamiento.campo) {
        datosProcesados.sort((a, b) => {
            const valorA = a[ordenamiento.campo];
            const valorB = b[ordenamiento.campo];
            
            let comparacion = 0;
            if (valorA > valorB) comparacion = 1;
            if (valorA < valorB) comparacion = -1;
            
            return ordenamiento.direccion === "desc" ? -comparacion : comparacion;
        });
    }
    
    // Aplicar paginación
    const inicio = (pagina - 1) * elementosPorPagina;
    const fin = inicio + elementosPorPagina;
    const datosPaginados = datosProcesados.slice(inicio, fin);
    
    return {
        datos: datosPaginados,
        paginacion: {
            paginaActual: pagina,
            elementosPorPagina: elementosPorPagina,
            totalElementos: datosProcesados.length,
            totalPaginas: Math.ceil(datosProcesados.length / elementosPorPagina)
        },
        filtrosAplicados: filtros,
        ordenamientoAplicado: ordenamiento
    };
}

// Uso del sistema
const usuarios = [
    { id: 1, nombre: "Ana", edad: 25, ciudad: "Madrid", activo: true },
    { id: 2, nombre: "Luis", edad: 30, ciudad: "Barcelona", activo: false },
    { id: 3, nombre: "María", edad: 28, ciudad: "Madrid", activo: true },
    // ... más usuarios
];

const resultado = procesarDatos(usuarios, {
    pagina: 1,
    elementosPorPagina: 2,
    filtros: {
        ciudad: "Madrid",
        activo: true
    },
    ordenamiento: {
        campo: "edad",
        direccion: "asc"
    }
});

console.log(resultado);
```

### 3. Sistema de cache con expiración

```javascript
class CacheConExpiracion {
    constructor() {
        this.cache = new Map();
        this.timers = new Map();
    }
    
    set(clave, valor, tiempoExpiracion = 300000) { // 5 minutos por defecto
        // Limpiar timer existente si existe
        if (this.timers.has(clave)) {
            clearTimeout(this.timers.get(clave));
        }
        
        // Guardar valor
        this.cache.set(clave, {
            valor: valor,
            timestamp: Date.now(),
            expiracion: Date.now() + tiempoExpiracion
        });
        
        // Configurar auto-limpieza
        const timer = setTimeout(() => {
            this.delete(clave);
        }, tiempoExpiracion);
        
        this.timers.set(clave, timer);
    }
    
    get(clave) {
        if (!this.cache.has(clave)) {
            return null;
        }
        
        const entrada = this.cache.get(clave);
        
        // Verificar si expiró
        if (Date.now() > entrada.expiracion) {
            this.delete(clave);
            return null;
        }
        
        return entrada.valor;
    }
    
    delete(clave) {
        if (this.timers.has(clave)) {
            clearTimeout(this.timers.get(clave));
            this.timers.delete(clave);
        }
        
        this.cache.delete(clave);
    }
    
    limpiarExpirados() {
        const ahora = Date.now();
        
        for (const [clave, entrada] of this.cache.entries()) {
            if (ahora > entrada.expiracion) {
                this.delete(clave);
            }
        }
    }
    
    obtenerEstadisticas() {
        const ahora = Date.now();
        let activos = 0;
        let expirados = 0;
        
        for (const entrada of this.cache.values()) {
            if (ahora > entrada.expiracion) {
                expirados++;
            } else {
                activos++;
            }
        }
        
        return {
            totalEntradas: this.cache.size,
            entradasActivas: activos,
            entradasExpiradas: expirados
        };
    }
}

// Uso del sistema de cache
const cache = new CacheConExpiracion();

// Guardar datos con diferentes tiempos de expiración
cache.set("usuario_123", { nombre: "Ana", email: "ana@email.com" }, 60000); // 1 minuto
cache.set("configuracion", { tema: "oscuro", idioma: "es" }); // 5 minutos por defecto
cache.set("datos_temporales", [1, 2, 3, 4, 5], 10000); // 10 segundos

// Recuperar datos
console.log(cache.get("usuario_123"));
console.log(cache.obtenerEstadisticas());
```

***

Las estructuras de control son fundamentales para crear programas dinámicos y funcionales. Dominar estas estructuras te permitirá manejar cualquier lógica de programación de manera eficiente y elegante. Practica combinando diferentes estructuras para resolver problemas complejos.
