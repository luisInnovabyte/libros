# Números en JavaScript

Los **números** en JavaScript son un tipo de dato fundamental utilizado para representar valores numéricos, tanto enteros como decimales.

***

## ¿Qué son los Números en JavaScript?

JavaScript tiene un **único tipo numérico** que representa tanto números enteros como decimales usando el estándar **IEEE 754 de doble precisión** (64 bits).

```javascript
const entero = 42;
const decimal = 3.14159;
const negativo = -17;
const cientifico = 2.5e10; // 25000000000
```

### Características importantes:

- **Todos los números son de tipo `number`**
- **Precisión limitada** (aproximadamente 15-17 dígitos decimales)
- **Rango limitado**: desde -(2^53 - 1) hasta (2^53 - 1)
- **Incluye valores especiales**: `Infinity`, `-Infinity`, `NaN`

## Declaración y Tipos de Números

### 1. Números literales:

```javascript
// Enteros
const edad = 25;
const temperatura = -5;

// Decimales
const precio = 19.99;
const pi = 3.14159;

// Notación científica
const distancia = 1.5e8;     // 150000000
const pequeno = 2.5e-4;      // 0.00025

// Diferentes bases
const binario = 0b1010;      // 10 en decimal
const octal = 0o755;         // 493 en decimal
const hexadecimal = 0xFF;    // 255 en decimal
```

### 2. Constructor Number:

```javascript
const numero1 = new Number(42);    // Objeto Number
const numero2 = Number(42);        // Primitivo number
const numero3 = Number("123");     // Conversión de string

console.log(typeof numero1); // "object"
console.log(typeof numero2); // "number"
console.log(typeof numero3); // "number"
```

### 3. Valores especiales:

```javascript
// Infinity
console.log(1 / 0);          // Infinity
console.log(-1 / 0);         // -Infinity
console.log(Number.POSITIVE_INFINITY); // Infinity
console.log(Number.NEGATIVE_INFINITY); // -Infinity

// NaN (Not a Number)
console.log(0 / 0);          // NaN
console.log("texto" * 2);    // NaN
console.log(Number.NaN);     // NaN

// Verificar estos valores
console.log(isFinite(42));   // true
console.log(isFinite(1/0));  // false
console.log(isNaN(NaN));     // true
console.log(isNaN("texto")); // true (convierte y evalúa)
```

## Conversión a Números

### 1. Métodos de conversión:

```javascript
// Number() - conversión estricta
console.log(Number("123"));      // 123
console.log(Number("123.45"));   // 123.45
console.log(Number("123abc"));   // NaN
console.log(Number(true));       // 1
console.log(Number(false));      // 0
console.log(Number(null));       // 0
console.log(Number(undefined));  // NaN

// parseInt() - convierte a entero
console.log(parseInt("123"));      // 123
console.log(parseInt("123.45"));   // 123
console.log(parseInt("123abc"));   // 123
console.log(parseInt("abc123"));   // NaN
console.log(parseInt("FF", 16));   // 255 (base 16)
console.log(parseInt("1010", 2));  // 10 (base 2)

// parseFloat() - convierte a decimal
console.log(parseFloat("123.45"));   // 123.45
console.log(parseFloat("123.45abc")); // 123.45
console.log(parseFloat("abc123.45")); // NaN

// Operador unario + (más rápido)
console.log(+"123");         // 123
console.log(+"123.45");      // 123.45
console.log(+"123abc");      // NaN
```

### 2. Verificación de tipos:

```javascript
// Verificar si es número
function esNumero(valor) {
    return typeof valor === 'number' && !isNaN(valor) && isFinite(valor);
}

console.log(esNumero(42));       // true
console.log(esNumero("42"));     // false
console.log(esNumero(NaN));      // false
console.log(esNumero(Infinity)); // false

// Number.isInteger() - ES6
console.log(Number.isInteger(42));    // true
console.log(Number.isInteger(42.0));  // true
console.log(Number.isInteger(42.5));  // false

// Number.isNaN() - más estricto que isNaN()
console.log(Number.isNaN(NaN));     // true
console.log(Number.isNaN("NaN"));   // false
console.log(isNaN("NaN"));          // true (convierte primero)

// Number.isFinite() - más estricto que isFinite()
console.log(Number.isFinite(42));     // true
console.log(Number.isFinite("42"));   // false
console.log(isFinite("42"));          // true (convierte primero)
```

## Métodos de Números

### 1. Formateo y redondeo:

```javascript
const numero = 123.456789;

// toFixed() - dígitos decimales fijos
console.log(numero.toFixed(2));     // "123.46"
console.log(numero.toFixed(0));     // "123"
console.log((1.005).toFixed(2));    // "1.00" (cuidado con la precisión)

// toPrecision() - dígitos significativos
console.log(numero.toPrecision(5)); // "123.46"
console.log(numero.toPrecision(3)); // "123"
console.log((0.001234).toPrecision(3)); // "0.00123"

// toExponential() - notación científica
console.log(numero.toExponential(2)); // "1.23e+2"
console.log((0.000123).toExponential(2)); // "1.23e-4"

// toString() - convertir a string con base
console.log((255).toString(16));    // "ff" (hexadecimal)
console.log((10).toString(2));      // "1010" (binario)
console.log((493).toString(8));     // "755" (octal)
```

### 2. Redondeo con Math:

```javascript
const numero = 4.7;
const negativo = -4.7;

// Math.round() - redondeo normal
console.log(Math.round(4.4));      // 4
console.log(Math.round(4.5));      // 5
console.log(Math.round(4.6));      // 5
console.log(Math.round(-4.5));     // -4 (redondea hacia arriba en absoluto)

// Math.floor() - redondeo hacia abajo
console.log(Math.floor(4.9));      // 4
console.log(Math.floor(-4.1));     // -5

// Math.ceil() - redondeo hacia arriba
console.log(Math.ceil(4.1));       // 5
console.log(Math.ceil(-4.9));      // -4

// Math.trunc() - elimina decimales (ES6)
console.log(Math.trunc(4.9));      // 4
console.log(Math.trunc(-4.9));     // -4
```

## Operaciones Aritméticas

### 1. Operadores básicos:

```javascript
const a = 10;
const b = 3;

// Operaciones básicas
console.log(a + b);    // 13 (suma)
console.log(a - b);    // 7 (resta)
console.log(a * b);    // 30 (multiplicación)
console.log(a / b);    // 3.3333333333333335 (división)
console.log(a % b);    // 1 (módulo/resto)
console.log(a ** b);   // 1000 (exponenciación ES2016)

// Operadores de incremento y decremento
let contador = 5;
console.log(contador++);  // 5 (post-incremento)
console.log(++contador);  // 7 (pre-incremento)
console.log(contador--);  // 7 (post-decremento)
console.log(--contador);  // 5 (pre-decremento)

// Operadores de asignación
let x = 10;
x += 5;   // x = x + 5 → 15
x -= 3;   // x = x - 3 → 12
x *= 2;   // x = x * 2 → 24
x /= 4;   // x = x / 4 → 6
x %= 4;   // x = x % 4 → 2
x **= 3;  // x = x ** 3 → 8
```

### 2. Problemas de precisión:

```javascript
// Problema clásico de punto flotante
console.log(0.1 + 0.2);              // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3);      // false

// Soluciones
function sumarDecimales(a, b, decimales = 2) {
    return Number((a + b).toFixed(decimales));
}

console.log(sumarDecimales(0.1, 0.2)); // 0.3

// Usando Number.EPSILON para comparaciones
function sonIguales(a, b) {
    return Math.abs(a - b) < Number.EPSILON;
}

console.log(sonIguales(0.1 + 0.2, 0.3)); // true

// Para cálculos monetarios
function calcularConCentavos(cantidad1, cantidad2) {
    return (cantidad1 * 100 + cantidad2 * 100) / 100;
}

console.log(calcularConCentavos(0.1, 0.2)); // 0.3
```

## Constantes de Number

### Propiedades estáticas importantes:

```javascript
// Valores máximos y mínimos
console.log(Number.MAX_VALUE);          // 1.7976931348623157e+308
console.log(Number.MIN_VALUE);          // 5e-324 (más pequeño positivo)
console.log(Number.MAX_SAFE_INTEGER);   // 9007199254740991
console.log(Number.MIN_SAFE_INTEGER);   // -9007199254740991

// Verificar enteros seguros
console.log(Number.isSafeInteger(9007199254740991));  // true
console.log(Number.isSafeInteger(9007199254740992));  // false

// Epsilon - diferencia más pequeña entre 1 y el siguiente número
console.log(Number.EPSILON);            // 2.220446049250313e-16

// Infinitos y NaN
console.log(Number.POSITIVE_INFINITY);  // Infinity
console.log(Number.NEGATIVE_INFINITY);  // -Infinity
console.log(Number.NaN);                // NaN
```

## El Objeto Math

### 1. Constantes matemáticas:

```javascript
// Constantes importantes
console.log(Math.PI);       // 3.141592653589793
console.log(Math.E);        // 2.718281828459045 (Euler)
console.log(Math.LN2);      // 0.6931471805599453 (ln(2))
console.log(Math.LN10);     // 2.302585092994046 (ln(10))
console.log(Math.LOG2E);    // 1.4426950408889634 (log2(e))
console.log(Math.LOG10E);   // 0.4342944819032518 (log10(e))
console.log(Math.SQRT2);    // 1.4142135623730951 (√2)
console.log(Math.SQRT1_2);  // 0.7071067811865476 (√(1/2))
```

### 2. Métodos de redondeo y valor absoluto:

```javascript
// Valor absoluto
console.log(Math.abs(-42));     // 42
console.log(Math.abs(42));      // 42
console.log(Math.abs(-3.14));   // 3.14

// Redondeo (ya vistos anteriormente)
console.log(Math.round(4.5));   // 5
console.log(Math.floor(4.9));   // 4
console.log(Math.ceil(4.1));    // 5
console.log(Math.trunc(4.9));   // 4

// Signo de un número
console.log(Math.sign(42));     // 1
console.log(Math.sign(-42));    // -1
console.log(Math.sign(0));      // 0
console.log(Math.sign(-0));     // -0
console.log(Math.sign(NaN));    // NaN
```

### 3. Potencias y raíces:

```javascript
// Potencias
console.log(Math.pow(2, 3));        // 8 (2^3)
console.log(Math.pow(9, 0.5));      // 3 (√9)
console.log(2 ** 3);                // 8 (operador de exponenciación ES2016)

// Raíces
console.log(Math.sqrt(16));         // 4 (raíz cuadrada)
console.log(Math.cbrt(27));         // 3 (raíz cúbica ES6)

// Logaritmos
console.log(Math.log(Math.E));      // 1 (logaritmo natural)
console.log(Math.log10(100));      // 2 (logaritmo base 10)
console.log(Math.log2(8));          // 3 (logaritmo base 2 ES6)

// Funciones hiperbólicas (ES6)
console.log(Math.sinh(1));          // 1.1752011936438014
console.log(Math.cosh(1));          // 1.5430806348152437
console.log(Math.tanh(1));          // 0.7615941559557649
```

### 4. Funciones trigonométricas:

```javascript
// Nota: todas las funciones trigonométricas usan radianes
const grados45 = 45 * Math.PI / 180; // Convertir grados a radianes

// Funciones básicas
console.log(Math.sin(Math.PI / 2));  // 1 (seno de 90°)
console.log(Math.cos(0));            // 1 (coseno de 0°)
console.log(Math.tan(Math.PI / 4));  // 1 (tangente de 45°)

// Funciones inversas
console.log(Math.asin(1));           // 1.5707963267948966 (π/2 radianes)
console.log(Math.acos(1));           // 0
console.log(Math.atan(1));           // 0.7853981633974483 (π/4 radianes)
console.log(Math.atan2(1, 1));       // 0.7853981633974483 (arctangente de y/x)

// Funciones de conversión
function gradosARadianes(grados) {
    return grados * Math.PI / 180;
}

function radianesAGrados(radianes) {
    return radianes * 180 / Math.PI;
}

console.log(gradosARadianes(45));    // 0.7853981633974483
console.log(radianesAGrados(Math.PI)); // 180
```

### 5. Números aleatorios:

```javascript
// Math.random() - número entre 0 (incluido) y 1 (excluido)
console.log(Math.random());          // Ej: 0.7234567891234567

// Número aleatorio entre min y max (excluye max)
function aleatorioEntre(min, max) {
    return Math.random() * (max - min) + min;
}

console.log(aleatorioEntre(5, 10));  // Ej: 7.234567891234567

// Entero aleatorio entre min y max (incluye ambos)
function enteroAleatorioEntre(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

console.log(enteroAleatorioEntre(1, 6)); // Simular dado: 1-6

// Seleccionar elemento aleatorio de array
function elementoAleatorio(array) {
    return array[Math.floor(Math.random() * array.length)];
}

const colores = ['rojo', 'azul', 'verde', 'amarillo'];
console.log(elementoAleatorio(colores)); // Ej: "verde"

// Barajar array (algoritmo Fisher-Yates)
function barajar(array) {
    const arrayCopiado = [...array];
    for (let i = arrayCopiado.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arrayCopiado[i], arrayCopiado[j]] = [arrayCopiado[j], arrayCopiado[i]];
    }
    return arrayCopiado;
}

console.log(barajar([1, 2, 3, 4, 5])); // Ej: [3, 1, 5, 2, 4]
```

### 6. Máximo y mínimo:

```javascript
// Math.max() y Math.min()
console.log(Math.max(1, 3, 2, 8, 5));     // 8
console.log(Math.min(1, 3, 2, 8, 5));     // 1

// Con arrays (usando spread operator)
const numeros = [1, 3, 2, 8, 5];
console.log(Math.max(...numeros));        // 8
console.log(Math.min(...numeros));        // 1

// Sin argumentos
console.log(Math.max());                  // -Infinity
console.log(Math.min());                  // Infinity

// Filtrar valores no numéricos
function maxSinNaN(...valores) {
    const numericos = valores.filter(v => typeof v === 'number' && !isNaN(v));
    return numericos.length ? Math.max(...numericos) : NaN;
}

console.log(maxSinNaN(1, 'texto', 3, NaN, 2)); // 3
```

## BigInt para Números Grandes (ES2020)

### Cuando los números normales no son suficientes:

```javascript
// Problema con números grandes
console.log(Number.MAX_SAFE_INTEGER);     // 9007199254740991
console.log(Number.MAX_SAFE_INTEGER + 1); // 9007199254740992
console.log(Number.MAX_SAFE_INTEGER + 2); // 9007199254740992 (¡igual!)

// Solución: BigInt
const numeroGrande = BigInt(9007199254740991);
const numeroGrande2 = 9007199254740991n; // Literal BigInt

console.log(numeroGrande + 1n);          // 9007199254740992n
console.log(numeroGrande + 2n);          // 9007199254740993n

// Operaciones con BigInt
const a = 123n;
const b = 456n;

console.log(a + b);    // 579n
console.log(a * b);    // 56088n
console.log(b / a);    // 3n (división entera)
console.log(b % a);    // 87n

// Conversiones
console.log(Number(123n));        // 123
console.log(BigInt(123));         // 123n
console.log(String(123n));        // "123"

// No se pueden mezclar BigInt con Number
// console.log(123n + 456);        // TypeError
console.log(123n + BigInt(456));   // 579n
```

## Casos de Uso Prácticos

### 1. Formateo de números:

```javascript
// Formatear números con separadores de miles
function formatearNumero(numero, decimales = 2) {
    return numero.toLocaleString('es-ES', {
        minimumFractionDigits: decimales,
        maximumFractionDigits: decimales
    });
}

console.log(formatearNumero(1234567.89)); // "1.234.567,89"

// Formatear como moneda
function formatearMoneda(cantidad, moneda = 'EUR') {
    return new Intl.NumberFormat('es-ES', {
        style: 'currency',
        currency: moneda
    }).format(cantidad);
}

console.log(formatearMoneda(1234.56));    // "1.234,56 €"

// Formatear como porcentaje
function formatearPorcentaje(decimal) {
    return new Intl.NumberFormat('es-ES', {
        style: 'percent',
        minimumFractionDigits: 1
    }).format(decimal);
}

console.log(formatearPorcentaje(0.1234)); // "12,3 %"
```

### 2. Cálculos matemáticos:

```javascript
// Calcular distancia entre dos puntos
function distancia(x1, y1, x2, y2) {
    return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
}

console.log(distancia(0, 0, 3, 4)); // 5

// Calcular área de círculo
function areaCirculo(radio) {
    return Math.PI * Math.pow(radio, 2);
}

console.log(areaCirculo(5)); // 78.53981633974483

// Generar secuencia de Fibonacci
function fibonacci(n) {
    if (n <= 1) return n;
    
    let a = 0, b = 1;
    for (let i = 2; i <= n; i++) {
        [a, b] = [b, a + b];
    }
    return b;
}

console.log(fibonacci(10)); // 55

// Factorial
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

console.log(factorial(5)); // 120
```

### 3. Validaciones numéricas:

```javascript
// Validar rango
function estaEnRango(numero, min, max) {
    return typeof numero === 'number' && 
           !isNaN(numero) && 
           numero >= min && 
           numero <= max;
}

console.log(estaEnRango(5, 1, 10));   // true
console.log(estaEnRango(15, 1, 10));  // false

// Validar si es entero positivo
function esEnteroPositivo(numero) {
    return Number.isInteger(numero) && numero > 0;
}

console.log(esEnteroPositivo(5));     // true
console.log(esEnteroPositivo(5.5));   // false
console.log(esEnteroPositivo(-5));    // false

// Redondear a múltiplo más cercano
function redondearAMultiplo(numero, multiplo) {
    return Math.round(numero / multiplo) * multiplo;
}

console.log(redondearAMultiplo(23, 5)); // 25
console.log(redondearAMultiplo(22, 5)); // 20
```

### 4. Generadores de números:

```javascript
// Generar número con formato específico
function generarCodigo(longitud = 6) {
    let codigo = '';
    for (let i = 0; i < longitud; i++) {
        codigo += Math.floor(Math.random() * 10);
    }
    return codigo;
}

console.log(generarCodigo()); // Ej: "749382"

// Generar UUID simple (no cryptográficamente seguro)
function generarUUID() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        const r = Math.random() * 16 | 0;
        const v = c === 'x' ? r : (r & 0x3 | 0x8);
        return v.toString(16);
    });
}

console.log(generarUUID()); // Ej: "f47ac10b-58cc-4372-a567-0e02b2c3d479"

// Clase para generar secuencias
class GeneradorSecuencia {
    constructor(inicio = 1, paso = 1) {
        this.actual = inicio;
        this.paso = paso;
    }
    
    siguiente() {
        const valor = this.actual;
        this.actual += this.paso;
        return valor;
    }
    
    reiniciar(inicio = 1) {
        this.actual = inicio;
    }
}

const gen = new GeneradorSecuencia(100, 5);
console.log(gen.siguiente()); // 100
console.log(gen.siguiente()); // 105
console.log(gen.siguiente()); // 110
```

## Rendimiento y Optimización

### 1. Comparación de operaciones:

```javascript
// Potenciación: ** vs Math.pow()
console.time('Operador **');
for (let i = 0; i < 1000000; i++) {
    2 ** 3;
}
console.timeEnd('Operador **'); // Generalmente más rápido

console.time('Math.pow()');
for (let i = 0; i < 1000000; i++) {
    Math.pow(2, 3);
}
console.timeEnd('Math.pow()');

// Para cuadrados: x * x vs Math.pow(x, 2)
function cuadradoRapido(x) {
    return x * x; // Más rápido
}

function cuadradoLento(x) {
    return Math.pow(x, 2); // Más lento
}
```

### 2. Caché de cálculos costosos:

```javascript
// Memoización para cálculos costosos
function memoizar(fn) {
    const cache = new Map();
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const resultado = fn.apply(this, args);
        cache.set(key, resultado);
        return resultado;
    };
}

// Factorial con memoización
const factorialMemo = memoizar(function(n) {
    if (n <= 1) return 1;
    return n * factorialMemo(n - 1);
});

console.log(factorialMemo(100)); // Primera vez: calcula
console.log(factorialMemo(100)); // Segunda vez: usa caché
```

## Errores Comunes y Mejores Prácticas

### 1. Errores de precisión:

```javascript
// ❌ Comparación directa de decimales
if (0.1 + 0.2 === 0.3) {
    console.log("Son iguales"); // No se ejecuta
}

// ✅ Comparación con tolerancia
function sonIguales(a, b, tolerancia = Number.EPSILON) {
    return Math.abs(a - b) < tolerancia;
}

if (sonIguales(0.1 + 0.2, 0.3)) {
    console.log("Son iguales"); // Se ejecuta
}
```

### 2. Validación robusta:

```javascript
// ❌ Validación insuficiente
function dividir(a, b) {
    return a / b; // Puede devolver Infinity o NaN
}

// ✅ Validación completa
function dividirSeguro(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
        throw new Error('Ambos argumentos deben ser números');
    }
    
    if (!isFinite(a) || !isFinite(b)) {
        throw new Error('Los números deben ser finitos');
    }
    
    if (b === 0) {
        throw new Error('No se puede dividir entre cero');
    }
    
    return a / b;
}
```

### 3. Manejo de parseInt:

```javascript
// ❌ Sin especificar base
console.log(parseInt("08"));     // 8 (correcto en ES5+)
console.log(parseInt("08", 8));  // 0 (octal)

// ✅ Siempre especificar base
console.log(parseInt("08", 10)); // 8
console.log(parseInt("FF", 16)); // 255
console.log(parseInt("1010", 2)); // 10
```

***

## Resumen Final

### Métodos más utilizados:
- **Conversión**: `Number()`, `parseInt()`, `parseFloat()`, `+`
- **Validación**: `isNaN()`, `Number.isInteger()`, `Number.isFinite()`
- **Formateo**: `toFixed()`, `toPrecision()`, `toLocaleString()`
- **Math básico**: `Math.round()`, `Math.abs()`, `Math.max()`, `Math.min()`
- **Math avanzado**: `Math.random()`, `Math.pow()`, `Math.sqrt()`

### Constantes importantes:
- **Number**: `MAX_SAFE_INTEGER`, `MIN_SAFE_INTEGER`, `EPSILON`
- **Math**: `PI`, `E`, `SQRT2`

### Mejores prácticas:
- **Usa comparaciones con tolerancia** para decimales
- **Valida entrada** antes de operaciones
- **Especifica la base** en `parseInt()`
- **Considera BigInt** para números muy grandes
- **Usa métodos de formateo** para mostrar números al usuario

Los números son fundamentales en JavaScript y dominar estos conceptos te permitirá realizar cálculos precisos y manejar datos numéricos de manera efectiva.
