# Funciones en JavaScript

Las **funciones** son uno de los pilares fundamentales de JavaScript. Son bloques de código reutilizable que realizan tareas específicas y pueden ser ejecutados cuando se necesiten. Las funciones permiten organizar el código, evitar repetición y crear programas más modulares y mantenibles.

## ¿Qué es una Función?

Una función es un conjunto de instrucciones agrupadas que realizan una tarea específica. Pueden recibir datos de entrada (parámetros), procesarlos y opcionalmente devolver un resultado.

```javascript
// Función básica
function saludar() {
    console.log("¡Hola mundo!");
}

// Ejecutar la función
saludar(); // ¡Hola mundo!
```

## Características principales

- **Reutilizable**: Se puede llamar múltiples veces
- **Modular**: Organiza el código en bloques lógicos
- **Parámetros**: Puede recibir datos de entrada
- **Valor de retorno**: Puede devolver un resultado
- **Scope**: Tiene su propio ámbito de variables
- **First-class citizens**: En JavaScript las funciones son valores

## Declaración de funciones

### 1. Function Declaration (Declaración de función)

```javascript
// Sintaxis básica
function nombreFuncion() {
    // código de la función
}

// Ejemplo con parámetros
function saludar(nombre) {
    console.log(`Hola ${nombre}!`);
}

// Ejemplo con return
function sumar(a, b) {
    return a + b;
}

// Uso
saludar("Ana");           // Hola Ana!
const resultado = sumar(5, 3); // 8
```

### 2. Function Expression (Expresión de función)

```javascript
// Función anónima asignada a variable
const saludar = function(nombre) {
    console.log(`Hola ${nombre}!`);
};

// Función con nombre (named function expression)
const calcular = function multiplicar(x, y) {
    return x * y;
};

// Uso
saludar("Luis");              // Hola Luis!
const producto = calcular(4, 5); // 20
```

### 3. Arrow Functions (Funciones flecha) - ES2015

```javascript
// Sintaxis básica
sintaxis básica
const saludar = (nombre) => {
    console.log(`Hola ${nombre}!`);
};

Sintaxis tradicional
// Ejemplo con parámetros
function saludar(nombre) {
    console.log(`Hola ${nombre}!`);
}

=====

// Arrow function con return implícito
const sumar = (a, b) => a + b;

// Sin parámetros
const obtenerFecha = () => new Date();

// Un solo parámetro (paréntesis opcionales)
const duplicar = x => x * 2;

// Múltiples líneas
const procesar = (datos) => {
    const procesados = datos.map(x => x * 2);
    return procesados.filter(x => x > 10);
};

// Uso
saludar("María");              // Hola María!
console.log(sumar(3, 7));      // 10
console.log(duplicar(5));      // 10
```

## Diferencias entre tipos de funciones

### Arrow Functions vs Funciones tradicionales

#### Comparación de nomenclatura y sintaxis

**1. Función sin parámetros**
```javascript
// Función tradicional
function decirHola() {
    return "¡Hola!";
}

// Arrow function
const decirHola = () => {
    return "¡Hola!";
};

// Arrow function con return implícito
const decirHola = () => "¡Hola!";
```

**2. Función con un parámetro**
```javascript
// Función tradicional
function duplicar(numero) {
    return numero * 2;
}

// Arrow function (paréntesis opcionales con un parámetro)
const duplicar = numero => {
    return numero * 2;
};

// Arrow function con return implícito
const duplicar = numero => numero * 2;

// Arrow function con paréntesis (estilo preferido por claridad)
const duplicar = (numero) => numero * 2;
```

**3. Función con múltiples parámetros**
```javascript
// Función tradicional
function sumar(a, b) {
    return a + b;
}

// Arrow function
const sumar = (a, b) => {
    return a + b;
};

// Arrow function con return implícito
const sumar = (a, b) => a + b;
```

**4. Función con múltiples líneas**
```javascript
// Función tradicional
function procesarUsuario(usuario) {
    const nombre = usuario.nombre.toUpperCase();
    const email = usuario.email.toLowerCase();
    return {
        nombre: nombre,
        email: email,
        fechaProceso: new Date()
    };
}

// Arrow function (requiere llaves y return explícito)
const procesarUsuario = (usuario) => {
    const nombre = usuario.nombre.toUpperCase();
    const email = usuario.email.toLowerCase();
    return {
        nombre: nombre,
        email: email,
        fechaProceso: new Date()
    };
};
```

**5. Función que devuelve un objeto literal**
```javascript
// Función tradicional
function crearPersona(nombre, edad) {
    return {
        nombre: nombre,
        edad: edad
    };
}

// Arrow function (necesita paréntesis para devolver objeto)
const crearPersona = (nombre, edad) => ({
    nombre: nombre,
    edad: edad
});

// ❌ Incorrecto (JavaScript interpreta {} como bloque de código)
const crearPersona = (nombre, edad) => {
    nombre: nombre,
    edad: edad
};
```

**6. Funciones con parámetros por defecto**
```javascript
// Función tradicional
function saludar(nombre, saludo) {
    if (saludo === undefined) {
        saludo = "Hola";
    }
    return saludo + ", " + nombre;
}

// Función tradicional con ES6+
function saludar(nombre, saludo = "Hola") {
    return `${saludo}, ${nombre}`;
}

// Arrow function
const saludar = (nombre, saludo = "Hola") => `${saludo}, ${nombre}`;
```

**7. Funciones con rest parameters**
```javascript
// Función tradicional
function sumarTodos() {
    var numeros = Array.prototype.slice.call(arguments);
    var suma = 0;
    for (var i = 0; i < numeros.length; i++) {
        suma += numeros[i];
    }
    return suma;
}

// Función tradicional con ES6+
function sumarTodos(...numeros) {
    return numeros.reduce((total, num) => total + num, 0);
}

// Arrow function
const sumarTodos = (...numeros) => numeros.reduce((total, num) => total + num, 0);
```

#### Diferencias importantes de comportamiento

**1. Contexto `this`**
```javascript
const persona = {
    nombre: "Ana",
    
    // Función tradicional - `this` se refiere al objeto persona
    presentarseTradicional: function() {
        console.log(`Soy ${this.nombre}`);
    },
    
    // Arrow function - `this` hereda del contexto exterior
    presentarseArrow: () => {
        console.log(`Soy ${this.nombre}`); // undefined!
    },
    
    // Uso correcto de arrow function en callback
    iniciarTemporizador: function() {
        setTimeout(() => {
            // Arrow function mantiene el `this` del método padre
            console.log(`Temporizador de ${this.nombre} terminado`);
        }, 1000);
    },
    
    // Uso incorrecto con función tradicional en callback
    iniciarTemporizadorIncorrecto: function() {
        setTimeout(function() {
            // `this` se refiere a window/global, no al objeto persona
            console.log(`Temporizador de ${this.nombre} terminado`); // undefined!
        }, 1000);
    }
};

persona.presentarseTradicional(); // "Soy Ana"
persona.presentarseArrow();       // "Soy undefined"
persona.iniciarTemporizador();    // "Temporizador de Ana terminado" (después de 1s)
```


#### Cuándo usar cada una

**Usar funciones tradicionales cuando:**
- Necesites usar `this` que se refiera al objeto que llama la función
- Quieras usar la función como constructor
- Necesites acceso al objeto `arguments`
- Definas métodos de objetos o clases
- Necesites hoisting (poder usar la función antes de declararla)

```javascript
// ✅ Casos apropiados para funciones tradicionales
const miObjeto = {
    valor: 42,
    obtenerValor: function() {
        return this.valor; // `this` se refiere a miObjeto
    }
};

function MiConstructor(nombre) {
    this.nombre = nombre; // Constructor
}

function funcionConHoisting() {
    return "Disponible antes de la declaración";
}
```

**Usar arrow functions cuando:**
- Quieras mantener el contexto `this` del scope padre
- Escribas callbacks cortos y concisos
- Hagas programación funcional (map, filter, reduce, etc.)
- No necesites `this`, `arguments`, ni constructor
- Quieras sintaxis más concisa

```javascript
// ✅ Casos apropiados para arrow functions
const numeros = [1, 2, 3, 4, 5];

// Callbacks en métodos de array
const duplicados = numeros.map(num => num * 2);
const pares = numeros.filter(num => num % 2 === 0);
const suma = numeros.reduce((total, num) => total + num, 0);

// Callbacks que mantienen contexto
class MiClase {
    constructor() {
        this.valor = 100;
    }
    
    iniciarTemporizador() {
        setTimeout(() => {
            console.log(this.valor); // Mantiene el `this` de la clase
        }, 1000);
    }
}

// Funciones utilitarias simples
const esPar = num => num % 2 === 0;
const obtenerNombre = usuario => usuario.nombre;
const calcularArea = radio => Math.PI * radio * radio;
```

#### Resumen de la comparación

| Aspecto | Función Tradicional | Arrow Function |
|---------|-------------------|----------------|
| **Sintaxis** | `function nombre() {}` | `const nombre = () => {}` |
| **`this`** | Propio contexto | Hereda del padre |
| **`arguments`** | Disponible | No disponible |
| **Hoisting** | Sí | No |
| **Constructor** | Sí | No |
| **Return implícito** | No | Sí (una línea) |
| **Mejor para** | Métodos, constructores | Callbacks, utilidades |




```javascript
const objeto = {
    nombre: "Juan",
    
    // Función tradicional - tiene su propio 'this'
    saludarTradicional: function() {
        console.log(`Hola, soy ${this.nombre}`);
    },
    
    // Arrow function - hereda 'this' del contexto superior
    saludarArrow: () => {
        console.log(`Hola, soy ${this.nombre}`); // undefined!
    },
    
    // Método con arrow function dentro
    metodoConArrow: function() {
        setTimeout(() => {
            // Arrow function mantiene el 'this' del método
            console.log(`Método: ${this.nombre}`);
        }, 1000);
    }
};

objeto.saludarTradicional(); // "Hola, soy Juan"
objeto.saludarArrow();       // "Hola, soy undefined"
objeto.metodoConArrow();     // "Método: Juan" (después de 1 segundo)
```

## Parámetros y Argumentos

### Parámetros básicos

```javascript
// Función con parámetros
function presentar(nombre, edad, profesion) {
    console.log(`Soy ${nombre}, tengo ${edad} años y trabajo como ${profesion}`);
}

// Llamada con argumentos
presentar("Ana", 25, "desarrolladora");
// "Soy Ana, tengo 25 años y trabajo como desarrolladora"

// Argumentos faltantes son undefined
presentar("Luis", 30);
// "Soy Luis, tengo 30 años y trabajo como undefined"
```

### Parámetros por defecto - ES2015

```javascript
// Valores por defecto
function saludar(nombre = "Usuario", saludo = "Hola") {
    return `${saludo}, ${nombre}!`;
}

console.log(saludar());                    // "Hola, Usuario!"
console.log(saludar("Ana"));               // "Hola, Ana!"
console.log(saludar("Luis", "Buenos días")); // "Buenos días, Luis!"

// Con expresiones como valores por defecto
function crearUsuario(nombre, id = Date.now(), activo = true) {
    return {
        nombre: nombre,
        id: id,
        activo: activo,
        fechaCreacion: new Date()
    };
}

console.log(crearUsuario("María"));
// { nombre: "María", id: 1729123456789, activo: true, fechaCreacion: ... }
```

### Rest Parameters (Parámetros rest) - ES2015

```javascript
// Recoger argumentos extras en un array
function sumarTodos(primero, segundo, ...resto) {
    console.log("Primero:", primero);     // 1
    console.log("Segundo:", segundo);     // 2
    console.log("Resto:", resto);         // [3, 4, 5, 6]
    
    return primero + segundo + resto.reduce((sum, num) => sum + num, 0);
}

console.log(sumarTodos(1, 2, 3, 4, 5, 6)); // 21

// Función que acepta cualquier cantidad de argumentos
function concatenar(...palabras) {
    return palabras.join(" ");
}

console.log(concatenar("Hola", "mundo", "desde", "JavaScript"));
// "Hola mundo desde JavaScript"

// Con arrow function
const multiplicarTodos = (...numeros) => {
    return numeros.reduce((producto, num) => producto * num, 1);
};

console.log(multiplicarTodos(2, 3, 4)); // 24
```

### Destructuring en parámetros - ES2015

```javascript
// Destructuring de objetos
function mostrarUsuario({nombre, edad, email = "No proporcionado"}) {
    console.log(`Nombre: ${nombre}`);
    console.log(`Edad: ${edad}`);
    console.log(`Email: ${email}`);
}

const usuario = {
    nombre: "Ana",
    edad: 28,
    profesion: "Diseñadora"
};

mostrarUsuario(usuario);
// Nombre: Ana
// Edad: 28
// Email: No proporcionado

// Destructuring de arrays
function sumarPrimerosDos([primero, segundo, ...resto]) {
    console.log(`Sumando ${primero} + ${segundo}`);
    console.log(`Ignorando: ${resto}`);
    return primero + segundo;
}

console.log(sumarPrimerosDos([10, 20, 30, 40])); // 30

// Parámetros con destructuring y valores por defecto
function configurarApp({
    tema = "claro",
    idioma = "es",
    notificaciones = true
} = {}) {
    console.log(`Configuración: ${tema}, ${idioma}, notificaciones: ${notificaciones}`);
}

configurarApp(); // Usa todos los valores por defecto
configurarApp({tema: "oscuro"}); // Sobrescribe solo el tema
```

## Return (Valor de retorno)

### Return básico

```javascript
// Función que devuelve un valor
function calcularArea(radio) {
    return Math.PI * radio * radio;
}

// Función que no devuelve nada explícitamente (return undefined)
function mostrarMensaje(mensaje) {
    console.log(mensaje);
    // return undefined; (implícito)
}

const area = calcularArea(5);           // 78.54...
const resultado = mostrarMensaje("Hola"); // undefined

console.log(area);      // 78.54...
console.log(resultado); // undefined
```

### Return temprano

```javascript
function validarEdad(edad) {
    // Validaciones con return temprano
    if (typeof edad !== "number") {
        return "Error: La edad debe ser un número";
    }
    
    if (edad < 0) {
        return "Error: La edad no puede ser negativa";
    }
    
    if (edad > 150) {
        return "Error: Edad no válida";
    }
    
    // Si llega aquí, la edad es válida
    if (edad < 18) {
        return "Menor de edad";
    } else if (edad < 65) {
        return "Adulto";
    } else {
        return "Adulto mayor";
    }
}

console.log(validarEdad(25));     // "Adulto"
console.log(validarEdad(-5));     // "Error: La edad no puede ser negativa"
console.log(validarEdad("abc"));  // "Error: La edad debe ser un número"
```

### Return de objetos y arrays

```javascript
// Devolver objetos
function crearPersona(nombre, edad) {
    return {
        nombre: nombre,
        edad: edad,
        saludar: function() {
            return `Hola, soy ${this.nombre}`;
        }
    };
}

// Devolver arrays
function obtenerEstadisticas(numeros) {
    const suma = numeros.reduce((acc, num) => acc + num, 0);
    const promedio = suma / numeros.length;
    const maximo = Math.max(...numeros);
    const minimo = Math.min(...numeros);
    
    return [suma, promedio, maximo, minimo];
}

// Uso
const persona = crearPersona("Luis", 30);
console.log(persona.saludar()); // "Hola, soy Luis"

const [suma, promedio, max, min] = obtenerEstadisticas([1, 5, 3, 9, 2]);
console.log(`Suma: ${suma}, Promedio: ${promedio}, Máx: ${max}, Mín: ${min}`);
```

## Scope (Ámbito) de las funciones

### Scope local vs global

```javascript
// Variables globales
let variableGlobal = "Soy global";

function miFuncion() {
    // Variables locales
    let variableLocal = "Soy local";
    
    console.log(variableGlobal); // Accede a la global: "Soy global"
    console.log(variableLocal);  // Accede a la local: "Soy local"
}

miFuncion();

console.log(variableGlobal); // "Soy global"
// console.log(variableLocal); // Error! variableLocal is not defined
```

### Block scope con let y const

```javascript
function ejemploScope() {
    let x = 1;
    
    if (true) {
        let x = 2; // Variable diferente (block scope)
        console.log(x); // 2
    }
    
    console.log(x); // 1
    
    // var no respeta block scope
    if (true) {
        var y = 3;
    }
    
    console.log(y); // 3 (accesible fuera del bloque)
}

ejemploScope();
```

### Closures (Clausuras)

```javascript
// La función interna "recuerda" variables del scope externo
function crearContador() {
    let contador = 0;
    
    return function() {
        contador++;
        return contador;
    };
}

const contador1 = crearContador();
const contador2 = crearContador();

console.log(contador1()); // 1
console.log(contador1()); // 2
console.log(contador1()); // 3

console.log(contador2()); // 1 (contador independiente)
console.log(contador2()); // 2

// Ejemplo práctico: Función de configuración
function crearConfiguracion(configuracionInicial) {
    let config = { ...configuracionInicial };
    
    return {
        obtener: function(clave) {
            return config[clave];
        },
        establecer: function(clave, valor) {
            config[clave] = valor;
        },
        obtenerToda: function() {
            return { ...config }; // Devolver copia
        }
    };
}

const miConfig = crearConfiguracion({ tema: "claro", idioma: "es" });
console.log(miConfig.obtener("tema"));        // "claro"
miConfig.establecer("tema", "oscuro");
console.log(miConfig.obtenerToda());          // { tema: "oscuro", idioma: "es" }
```

## Funciones como First-Class Citizens

### Asignación a variables

```javascript
// Las funciones son valores, se pueden asignar
function sumar(a, b) {
    return a + b;
}

const miFuncion = sumar;           // Asignar función a variable
const resultado = miFuncion(3, 5); // Usar la variable como función

console.log(resultado); // 8

// Array de funciones
const operaciones = [
    (a, b) => a + b,    // suma
    (a, b) => a - b,    // resta
    (a, b) => a * b,    // multiplicación
    (a, b) => a / b     // división
];

console.log(operaciones[0](10, 5)); // 15 (suma)
console.log(operaciones[2](4, 3));  // 12 (multiplicación)
```

### Funciones como argumentos (Callbacks)

```javascript
// Función que recibe otra función como parámetro
function procesar(datos, callback) {
    console.log("Procesando datos...");
    const resultado = callback(datos);
    console.log("Procesamiento completado");
    return resultado;
}

// Diferentes callbacks
const duplicar = (arr) => arr.map(x => x * 2);
const sumarTodos = (arr) => arr.reduce((sum, x) => sum + x, 0);
const obtenerPares = (arr) => arr.filter(x => x % 2 === 0);

const numeros = [1, 2, 3, 4, 5];

console.log(procesar(numeros, duplicar));      // [2, 4, 6, 8, 10]
console.log(procesar(numeros, sumarTodos));    // 15
console.log(procesar(numeros, obtenerPares));  // [2, 4]

// Ejemplo con setTimeout (callback asíncrono)
function ejecutarDespues(callback, tiempo) {
    console.log("Configurando temporizador...");
    setTimeout(callback, tiempo);
}

ejecutarDespues(() => console.log("¡Tiempo cumplido!"), 2000);
```

### Funciones que devuelven funciones (Higher-Order Functions)

```javascript
// Función que devuelve una función personalizada
function crearValidador(tipo) {
    switch (tipo) {
        case "email":
            return function(valor) {
                return valor.includes("@") && valor.includes(".");
            };
        case "numero":
            return function(valor) {
                return !isNaN(valor) && valor !== "";
            };
        case "requerido":
            return function(valor) {
                return valor !== null && valor !== undefined && valor !== "";
            };
        default:
            return function() {
                return true;
            };
    }
}

// Crear validadores específicos
const validarEmail = crearValidador("email");
const validarNumero = crearValidador("numero");
const validarRequerido = crearValidador("requerido");

console.log(validarEmail("test@email.com")); // true
console.log(validarNumero("123"));           // true
console.log(validarRequerido(""));           // false

// Factory function más complejo
function crearCalculadora(operacionPorDefecto) {
    return {
        operacion: operacionPorDefecto,
        calcular: function(a, b, operacion = this.operacion) {
            switch (operacion) {
                case "sumar": return a + b;
                case "restar": return a - b;
                case "multiplicar": return a * b;
                case "dividir": return b !== 0 ? a / b : "Error: División por cero";
                default: return "Operación no válida";
            }
        }
    };
}

const calculadoraSuma = crearCalculadora("sumar");
const calculadoraMultiplicacion = crearCalculadora("multiplicar");

console.log(calculadoraSuma.calcular(5, 3));           // 8 (suma por defecto)
console.log(calculadoraSuma.calcular(5, 3, "restar")); // 2 (operación específica)
```

## Recursión

### Recursión básica

```javascript
// Función factorial
function factorial(n) {
    // Caso base (condición de parada)
    if (n <= 1) {
        return 1;
    }
    
    // Caso recursivo
    return n * factorial(n - 1);
}

console.log(factorial(5)); // 120 (5 * 4 * 3 * 2 * 1)

// Secuencia de Fibonacci
function fibonacci(n) {
    if (n <= 1) {
        return n;
    }
    
    return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log(fibonacci(7)); // 13

// Cuenta regresiva
function cuentaRegresiva(numero) {
    console.log(numero);
    
    if (numero > 0) {
        cuentaRegresiva(numero - 1);
    } else {
        console.log("¡Despegue!");
    }
}

cuentaRegresiva(5);
// 5
// 4
// 3
// 2
// 1
// 0
// ¡Despegue!
```

### Recursión con arrays y objetos

```javascript
// Suma recursiva de array
function sumarArray(arr) {
    // Caso base: array vacío
    if (arr.length === 0) {
        return 0;
    }
    
    // Caso recursivo: primer elemento + suma del resto
    return arr[0] + sumarArray(arr.slice(1));
}

console.log(sumarArray([1, 2, 3, 4, 5])); // 15

// Búsqueda recursiva en objeto anidado
function buscarEnObjeto(obj, claveBuscada) {
    for (let clave in obj) {
        if (clave === claveBuscada) {
            return obj[clave];
        }
        
        // Si el valor es un objeto, buscar recursivamente
        if (typeof obj[clave] === "object" && obj[clave] !== null) {
            const resultado = buscarEnObjeto(obj[clave], claveBuscada);
            if (resultado !== undefined) {
                return resultado;
            }
        }
    }
    
    return undefined;
}

const objetoComplejo = {
    nivel1: {
        nivel2: {
            nivel3: {
                valorBuscado: "¡Encontrado!"
            }
        },
        otroValor: "Hola"
    },
    valorSimple: 42
};

console.log(buscarEnObjeto(objetoComplejo, "valorBuscado")); // "¡Encontrado!"

// Aplanar array recursivamente
function aplanarArray(arr) {
    let resultado = [];
    
    for (let elemento of arr) {
        if (Array.isArray(elemento)) {
            // Si es array, aplanar recursivamente
            resultado = resultado.concat(aplanarArray(elemento));
        } else {
            // Si no es array, añadir directamente
            resultado.push(elemento);
        }
    }
    
    return resultado;
}

const arrayAnidado = [1, [2, 3], [4, [5, 6]], 7];
console.log(aplanarArray(arrayAnidado)); // [1, 2, 3, 4, 5, 6, 7]
```

## Métodos de función avanzados

### call(), apply(), bind()

```javascript
const persona1 = { nombre: "Ana", edad: 25 };
const persona2 = { nombre: "Luis", edad: 30 };

function presentarse(saludo, profesion) {
    return `${saludo}, soy ${this.nombre}, tengo ${this.edad} años y trabajo como ${profesion}`;
}

// call() - argumentos separados
console.log(presentarse.call(persona1, "Hola", "desarrolladora"));
// "Hola, soy Ana, tengo 25 años y trabajo como desarrolladora"

// apply() - argumentos en array
console.log(presentarse.apply(persona2, ["Buenos días", "diseñador"]));
// "Buenos días, soy Luis, tengo 30 años y trabajo como diseñador"

// bind() - crear nueva función con this fijo
const presentarAna = presentarse.bind(persona1);
console.log(presentarAna("¡Saludos!", "programadora"));
// "¡Saludos!, soy Ana, tengo 25 años y trabajo como programadora"

// bind() con argumentos predefinidos
const presentarAnaAmigable = presentarse.bind(persona1, "¡Hola!");
console.log(presentarAnaAmigable("estudiante"));
// "¡Hola!, soy Ana, tengo 25 años y trabajo como estudiante"
```

## IIFE (Immediately Invoked Function Expression)

```javascript
// IIFE básico
(function() {
    console.log("Esta función se ejecuta inmediatamente");
})();

// IIFE con parámetros
(function(nombre, edad) {
    console.log(`Hola ${nombre}, tienes ${edad} años`);
})("María", 28);

// IIFE que devuelve un valor
const resultado = (function(x, y) {
    return x * y;
})(5, 4);

console.log(resultado); // 20

// IIFE para crear módulos (patrón module)
const miModulo = (function() {
    // Variables privadas
    let contadorPrivado = 0;
    const configuracionPrivada = { tema: "claro" };
    
    // Función privada
    function incrementarContador() {
        contadorPrivado++;
    }
    
    // API pública (lo que se devuelve)
    return {
        obtenerContador: function() {
            return contadorPrivado;
        },
        incrementar: function() {
            incrementarContador();
        },
        obtenerConfiguracion: function() {
            return { ...configuracionPrivada }; // Devolver copia
        }
    };
})();

console.log(miModulo.obtenerContador()); // 0
miModulo.incrementar();
console.log(miModulo.obtenerContador()); // 1
// miModulo.contadorPrivado; // undefined (privado)
```

## Casos prácticos comunes

### 1. Sistema de validación

```javascript
// Sistema de validación modular
const validadores = {
    requerido: function(valor) {
        return valor !== null && valor !== undefined && valor.trim() !== "";
    },
    
    email: function(valor) {
        const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return regex.test(valor);
    },
    
    longitudMinima: function(minimo) {
        return function(valor) {
            return valor && valor.length >= minimo;
        };
    },
    
    numeroEnRango: function(min, max) {
        return function(valor) {
            const num = parseFloat(valor);
            return !isNaN(num) && num >= min && num <= max;
        };
    }
};

function validarCampo(valor, reglas) {
    const errores = [];
    
    for (const regla of reglas) {
        if (!regla.validador(valor)) {
            errores.push(regla.mensaje);
        }
    }
    
    return {
        valido: errores.length === 0,
        errores: errores
    };
}

// Uso del sistema
const resultadoEmail = validarCampo("usuario@ejemplo.com", [
    { validador: validadores.requerido, mensaje: "Email es requerido" },
    { validador: validadores.email, mensaje: "Email no válido" }
]);

const resultadoPassword = validarCampo("123", [
    { validador: validadores.requerido, mensaje: "Password requerido" },
    { validador: validadores.longitudMinima(8), mensaje: "Mínimo 8 caracteres" }
]);

console.log(resultadoEmail);   // { valido: true, errores: [] }
console.log(resultadoPassword); // { valido: false, errores: ["Mínimo 8 caracteres"] }
```

### 2. Sistema de eventos

```javascript
// Sistema de eventos simple
function crearEmisoreEventos() {
    const eventos = {};
    
    return {
        // Suscribirse a un evento
        on: function(evento, callback) {
            if (!eventos[evento]) {
                eventos[evento] = [];
            }
            eventos[evento].push(callback);
        },
        
        // Emitir un evento
        emit: function(evento, datos) {
            if (eventos[evento]) {
                eventos[evento].forEach(callback => callback(datos));
            }
        },
        
        // Desuscribirse de un evento
        off: function(evento, callback) {
            if (eventos[evento]) {
                eventos[evento] = eventos[evento].filter(cb => cb !== callback);
            }
        }
    };
}

// Uso del sistema de eventos
const emisor = crearEmisoreEventos();

// Definir manejadores
const manejarUsuarioConectado = (usuario) => {
    console.log(`Usuario conectado: ${usuario.nombre}`);
};

const manejarUsuarioDesconectado = (usuario) => {
    console.log(`Usuario desconectado: ${usuario.nombre}`);
};

// Suscribirse a eventos
emisor.on("usuarioConectado", manejarUsuarioConectado);
emisor.on("usuarioDesconectado", manejarUsuarioDesconectado);

// Emitir eventos
emisor.emit("usuarioConectado", { nombre: "Ana", id: 123 });
emisor.emit("usuarioDesconectado", { nombre: "Luis", id: 456 });
```

### 3. Debounce y Throttle

```javascript
// Debounce: Retrasa la ejecución hasta que pasen X milisegundos sin llamadas
function debounce(func, delay) {
    let timeoutId;
    
    return function(...args) {
        // Limpiar timeout anterior
        clearTimeout(timeoutId);
        
        // Configurar nuevo timeout
        timeoutId = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}

// Throttle: Limita la ejecución a una vez cada X milisegundos
function throttle(func, delay) {
    let lastExecution = 0;
    
    return function(...args) {
        const now = Date.now();
        
        if (now - lastExecution >= delay) {
            lastExecution = now;
            func.apply(this, args);
        }
    };
}

// Ejemplos de uso
const buscarConDebounce = debounce(function(termino) {
    console.log(`Buscando: ${termino}`);
    // Aquí iría la llamada a la API
}, 300);

const manejarScrollConThrottle = throttle(function() {
    console.log("Scroll detectado");
    // Aquí iría la lógica del scroll
}, 100);

// Simular uso
// buscarConDebounce("JavaScript"); // Se ejecuta solo si no hay más llamadas en 300ms
// window.addEventListener('scroll', manejarScrollConThrottle); // Máximo una vez cada 100ms
```

## Buenas prácticas

### 1. Nombres descriptivos

```javascript
// ❌ Malo
function calc(x, y) {
    return x * 0.21 + y;
}

// ✅ Bueno
function calcularPrecioConIVA(precioBase, iva) {
    return precioBase * (1 + iva);
}

function calcularDescuento(precio, porcentajeDescuento) {
    return precio * (porcentajeDescuento / 100);
}
```

### 2. Funciones pequeñas y específicas

```javascript
// ❌ Función que hace demasiadas cosas
function procesarUsuario(usuario) {
    // Validar
    if (!usuario.email || !usuario.nombre) {
        throw new Error("Datos incompletos");
    }
    
    // Limpiar datos
    usuario.nombre = usuario.nombre.trim();
    usuario.email = usuario.email.toLowerCase();
    
    // Generar ID
    usuario.id = Date.now();
    
    // Guardar en base de datos
    // ... código de BD
    
    // Enviar email de bienvenida
    // ... código de email
    
    return usuario;
}

// ✅ Dividir en funciones específicas
function validarUsuario(usuario) {
    if (!usuario.email || !usuario.nombre) {
        throw new Error("Email y nombre son requeridos");
    }
}

function limpiarDatosUsuario(usuario) {
    return {
        ...usuario,
        nombre: usuario.nombre.trim(),
        email: usuario.email.toLowerCase()
    };
}

function generarIdUsuario() {
    return `user_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
}

function crearUsuario(datosUsuario) {
    validarUsuario(datosUsuario);
    const usuarioLimpio = limpiarDatosUsuario(datosUsuario);
    const usuarioConId = {
        ...usuarioLimpio,
        id: generarIdUsuario()
    };
    
    return usuarioConId;
}
```

### 3. Evitar efectos secundarios no deseados

```javascript
// ❌ Función con efectos secundarios
let contador = 0;

function incrementar() {
    contador++; // Modifica variable global
    console.log(contador); // Efecto secundario
    return contador;
}

// ✅ Función pura sin efectos secundarios
function incrementarPuro(valor) {
    return valor + 1;
}

// ✅ Si necesitas efectos secundarios, hazlos explícitos
function incrementarYMostrar(contadorActual) {
    const nuevoContador = incrementarPuro(contadorActual);
    console.log(`Contador: ${nuevoContador}`);
    return nuevoContador;
}
```

***

Las funciones son el corazón de JavaScript y dominar estos conceptos te permitirá escribir código más limpio, reutilizable y mantenible. Practica con diferentes tipos de funciones y patrones para encontrar las mejores soluciones a tus problemas de programación.
