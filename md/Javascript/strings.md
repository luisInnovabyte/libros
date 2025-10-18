# Strings en JavaScript

Los **strings** (cadenas de texto) en JavaScript son secuencias de caracteres utilizadas para representar y manipular texto.

***

## ¿Qué son los Strings?

Un **string** es un tipo de dato primitivo que representa una secuencia de caracteres. En JavaScript, los strings son **inmutables**, lo que significa que no pueden ser modificados directamente, sino que se crean nuevos strings.

```javascript
const saludo = "Hola";
const nombre = 'Juan';
const mensaje = `Bienvenido`;
```

## Declaración de Strings

### Métodos de declaración:

#### 1. Comillas simples (`'`):
```javascript
const texto1 = 'Hola mundo';
const texto2 = 'Puedo incluir "comillas dobles" aquí';
```

#### 2. Comillas dobles (`"`):
```javascript
const texto3 = "Hola mundo";
const texto4 = "Puedo incluir 'comillas simples' aquí";
```

#### 3. Template literals (`` ` ``):
```javascript
const nombre = "Ana";
const edad = 25;
const presentacion = `Mi nombre es ${nombre} y tengo ${edad} años`;
```

#### 4. Constructor String (menos común):
```javascript
const texto5 = new String("Hola"); // Crea un objeto String
const texto6 = String(123);        // Convierte a string primitivo
```

### Diferencias importantes:

```javascript
// String primitivo vs String objeto
const primitivo = "Hola";
const objeto = new String("Hola");

console.log(typeof primitivo); // "string"
console.log(typeof objeto);    // "object"

console.log(primitivo === objeto);        // false
console.log(primitivo === objeto.valueOf()); // true
```

## Caracteres de Escape

Para incluir caracteres especiales en strings:

```javascript
const ejemplos = {
    nuevaLinea: "Primera línea\nSegunda línea",
    tab: "Columna1\tColumna2",
    comillaSimple: 'Don\'t worry',
    comillaDoble: "Él dijo: \"Hola\"",
    backslash: "Ruta: C:\\Users\\Documents",
    unicode: "Corazón: \u2764",
    emoji: "Sonrisa: \u{1F600}"
};

console.log(ejemplos.nuevaLinea);
// Primera línea
// Segunda línea
```

## Propiedades Básicas

### Length (longitud):
```javascript
const texto = "JavaScript";
console.log(texto.length); // 10

const vacio = "";
console.log(vacio.length); // 0

const espacios = "   ";
console.log(espacios.length); // 3
```

### Acceso a caracteres:
```javascript
const palabra = "Programación";

// Notación de corchetes (recomendado)
console.log(palabra[0]);  // "P"
console.log(palabra[4]);  // "r"
console.log(palabra[11]); // "n"

// Método charAt()
console.log(palabra.charAt(0));  // "P"
console.log(palabra.charAt(15)); // "" (fuera de rango)

// Último carácter
console.log(palabra[palabra.length - 1]); // "n"
```

## Métodos Básicos de Strings

### 1. Conversión de caso:

```javascript
const texto = "JavaScript es Genial";

// Mayúsculas y minúsculas
console.log(texto.toLowerCase());    // "javascript es genial"
console.log(texto.toUpperCase());    // "JAVASCRIPT ES GENIAL"

// Solo primera letra (no existe método nativo)
function capitalizarPrimera(str) {
    return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}
console.log(capitalizarPrimera(texto)); // "Javascript es genial"
```

### 2. Búsqueda en strings:

```javascript
const frase = "JavaScript es un lenguaje de programación";

// indexOf() - devuelve la posición de la primera coincidencia
console.log(frase.indexOf("Script"));      // 4
console.log(frase.indexOf("Python"));      // -1 (no encontrado)
console.log(frase.indexOf("a"));           // 1 (primera 'a')

// lastIndexOf() - última coincidencia
console.log(frase.lastIndexOf("a"));       // 35 (última 'a')

// includes() - devuelve true/false
console.log(frase.includes("JavaScript")); // true
console.log(frase.includes("Python"));     // false

// startsWith() y endsWith()
console.log(frase.startsWith("Java"));     // true
console.log(frase.endsWith("ción"));       // true
console.log(frase.startsWith("Script"));   // false
```

### 3. Extracción de substrings:

```javascript
const texto = "JavaScript Programming";

// slice(inicio, fin) - no modifica el original
console.log(texto.slice(0, 4));    // "Java"
console.log(texto.slice(4));       // "Script Programming"
console.log(texto.slice(-11));     // "Programming" (desde el final)
console.log(texto.slice(-11, -5)); // "Progra"

// substring(inicio, fin) - similar a slice pero sin índices negativos
console.log(texto.substring(0, 4)); // "Java"
console.log(texto.substring(4, 0)); // "Java" (intercambia automáticamente)

// substr(inicio, longitud) - OBSOLETO, usar slice()
console.log(texto.substr(4, 6));    // "Script"
```

### 4. Modificación de strings:

```javascript
const nombre = "  Juan Carlos  ";

// trim() - elimina espacios al inicio y final
console.log(nombre.trim());       // "Juan Carlos"
console.log(nombre.trimStart());  // "Juan Carlos  "
console.log(nombre.trimEnd());    // "  Juan Carlos"

// replace() - reemplaza la primera coincidencia
const frase = "Me gusta JavaScript y JavaScript es genial";
console.log(frase.replace("JavaScript", "Python"));
// "Me gusta Python y JavaScript es genial"

// replaceAll() - reemplaza todas las coincidencias (ES2021)
console.log(frase.replaceAll("JavaScript", "Python"));
// "Me gusta Python y Python es genial"

// Con expresiones regulares
console.log(frase.replace(/JavaScript/g, "Python"));
// "Me gusta Python y Python es genial"
```

### 5. División y unión:

```javascript
// split() - convierte string en array
const lista = "manzana,naranja,plátano,uva";
const frutas = lista.split(",");
console.log(frutas); // ["manzana", "naranja", "plátano", "uva"]

const palabras = "Hola mundo JavaScript".split(" ");
console.log(palabras); // ["Hola", "mundo", "JavaScript"]

// Con límite
const limitado = "a-b-c-d-e".split("-", 3);
console.log(limitado); // ["a", "b", "c"]

// join() es el método de array para unir
const unido = frutas.join(" | ");
console.log(unido); // "manzana | naranja | plátano | uva"
```

## Template Literals (Plantillas de String)

### Interpolación de variables:

```javascript
const nombre = "María";
const edad = 28;
const profesion = "Desarrolladora";

// Forma antigua
const presentacion1 = "Hola, soy " + nombre + ", tengo " + edad + " años y soy " + profesion;

// Con template literals
const presentacion2 = `Hola, soy ${nombre}, tengo ${edad} años y soy ${profesion}`;

console.log(presentacion2);
// "Hola, soy María, tengo 28 años y soy Desarrolladora"
```

### Expresiones en template literals:

```javascript
const a = 10;
const b = 5;

console.log(`La suma de ${a} y ${b} es ${a + b}`);
// "La suma de 10 y 5 es 15"

console.log(`El mayor entre ${a} y ${b} es ${a > b ? a : b}`);
// "El mayor entre 10 y 5 es 10"

// Llamadas a funciones
function formatearFecha(fecha) {
    return fecha.toLocaleDateString('es-ES');
}

const hoy = new Date();
console.log(`Hoy es ${formatearFecha(hoy)}`);
```

### Strings multilínea:

```javascript
// Antes (complicado)
const html1 = "<div>\n" +
              "  <h1>Título</h1>\n" +
              "  <p>Párrafo</p>\n" +
              "</div>";

// Con template literals (fácil)
const html2 = `
<div>
  <h1>Título</h1>
  <p>Párrafo</p>
</div>
`;

const poema = `
Roses are red,
Violets are blue,
JavaScript is awesome,
And so are you!
`;
```

## Métodos Avanzados

### 1. Repetición:

```javascript
// repeat() - repite el string n veces
console.log("Na".repeat(8) + " Batman!"); // "NaNaNaNaNaNaNaNa Batman!"
console.log("-".repeat(20));              // "--------------------"
console.log("🎉".repeat(5));              // "🎉🎉🎉🎉🎉"
```

### 2. Relleno (padding):

```javascript
const numero = "42";

// padStart() - rellena al inicio
console.log(numero.padStart(5, "0"));    // "00042"
console.log(numero.padStart(10, "*"));   // "********42"

// padEnd() - rellena al final
console.log(numero.padEnd(5, "0"));      // "42000"
console.log("ID:".padEnd(10, "-"));      // "ID:-------"

// Casos prácticos
const formatearId = (id) => String(id).padStart(6, "0");
console.log(formatearId(123));           // "000123"

const formatearPrecio = (precio) => `$${precio.toFixed(2).padStart(8)}`;
console.log(formatearPrecio(12.5));      // "$   12.50"
```

### 3. Normalización:

```javascript
// normalize() - normaliza caracteres Unicode
const texto1 = "café";
const texto2 = "cafe\u0301"; // e + acento combinado

console.log(texto1 === texto2);                    // false
console.log(texto1.normalize() === texto2.normalize()); // true

// Útil para comparaciones
function compararTexto(str1, str2) {
    return str1.normalize().toLowerCase() === str2.normalize().toLowerCase();
}
```

## Expresiones Regulares con Strings

### Métodos que usan regex:

```javascript
const texto = "Mi teléfono es 123-456-7890 y mi email es juan@example.com";

// match() - encuentra coincidencias
const telefonos = texto.match(/\d{3}-\d{3}-\d{4}/g);
console.log(telefonos); // ["123-456-7890"]

const emails = texto.match(/\w+@\w+\.\w+/g);
console.log(emails); // ["juan@example.com"]

// search() - encuentra la posición
const posicionEmail = texto.search(/\w+@\w+\.\w+/);
console.log(posicionEmail); // 38

// test() es método de RegExp, no de String
const esEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test("usuario@dominio.com");
console.log(esEmail); // true
```

## Conversiones y Comparaciones

### Conversión a otros tipos:

```javascript
// String a Number
console.log(Number("123"));      // 123
console.log(parseInt("123px"));  // 123
console.log(parseFloat("3.14")); // 3.14
console.log(+"456");             // 456 (operador unario)

// Number a String
console.log(String(123));        // "123"
console.log((123).toString());   // "123"
console.log(123 + "");           // "123"

// Boolean a String
console.log(String(true));       // "true"
console.log(false.toString());   // "false"
```

### Comparaciones:

```javascript
// Comparación básica
console.log("a" < "b");          // true
console.log("Apple" < "apple");  // true (mayúsculas van antes)

// localeCompare() - comparación según el idioma
console.log("ñ".localeCompare("o", 'es-ES')); // 1 (ñ va después de o en español)
console.log("Apple".localeCompare("apple", 'en', { 
    sensitivity: 'base' 
})); // 0 (ignora mayúsculas)

// Comparación de longitud
const nombres = ["Ana", "Roberto", "Luis", "María"];
nombres.sort((a, b) => a.length - b.length);
console.log(nombres); // ["Ana", "Luis", "María", "Roberto"]
```

## Casos de Uso Prácticos

### 1. Validaciones:

```javascript
// Validar email simple
function esEmailValido(email) {
    return email.includes("@") && 
           email.includes(".") && 
           email.indexOf("@") < email.lastIndexOf(".");
}

// Validar contraseña
function esContrasenaSegura(password) {
    return password.length >= 8 &&
           /[A-Z]/.test(password) &&    // Al menos una mayúscula
           /[a-z]/.test(password) &&    // Al menos una minúscula
           /\d/.test(password) &&       // Al menos un número
           /[!@#$%^&*]/.test(password); // Al menos un carácter especial
}

console.log(esContrasenaSegura("MiPassword123!")); // true
```

### 2. Formateo de texto:

```javascript
// Capitalizar palabras
function capitalizarPalabras(texto) {
    return texto.split(' ')
                .map(palabra => palabra.charAt(0).toUpperCase() + palabra.slice(1).toLowerCase())
                .join(' ');
}

console.log(capitalizarPalabras("hola mundo javascript"));
// "Hola Mundo Javascript"

// Crear slug para URLs
function crearSlug(texto) {
    return texto.toLowerCase()
                .replace(/[^\w\s-]/g, '') // Eliminar caracteres especiales
                .replace(/\s+/g, '-')     // Espacios por guiones
                .replace(/--+/g, '-')     // Múltiples guiones por uno
                .trim('-');               // Eliminar guiones al inicio/final
}

console.log(crearSlug("¡Hola Mundo en JavaScript!"));
// "hola-mundo-en-javascript"
```

### 3. Manipulación de nombres:

```javascript
function procesarNombre(nombreCompleto) {
    const partes = nombreCompleto.trim().split(/\s+/);
    
    return {
        nombreCompleto: nombreCompleto.trim(),
        nombre: partes[0],
        apellido: partes[partes.length - 1],
        iniciales: partes.map(parte => parte.charAt(0).toUpperCase()).join(''),
        cantidadPalabras: partes.length
    };
}

console.log(procesarNombre("  Juan Carlos García López  "));
// {
//   nombreCompleto: "Juan Carlos García López",
//   nombre: "Juan",
//   apellido: "López",
//   iniciales: "JCGL",
//   cantidadPalabras: 4
// }
```

## Rendimiento y Buenas Prácticas

### 1. Concatenación eficiente:

```javascript
// ❌ Ineficiente para muchas concatenaciones
let resultado = "";
for (let i = 0; i < 1000; i++) {
    resultado += `Item ${i} `;
}

// ✅ Más eficiente
const items = [];
for (let i = 0; i < 1000; i++) {
    items.push(`Item ${i}`);
}
const resultado2 = items.join(' ');

// ✅ Aún mejor para casos simples
const resultado3 = Array.from({length: 1000}, (_, i) => `Item ${i}`).join(' ');
```

### 2. Inmutabilidad:

```javascript
// Los strings son inmutables
let texto = "Hola";
let textoOriginal = texto;

texto = texto + " Mundo"; // Se crea un nuevo string

console.log(texto);         // "Hola Mundo"
console.log(textoOriginal); // "Hola" (no cambió)

// Esto significa que métodos como replace() no modifican el original
const frase = "Me gusta JavaScript";
frase.replace("JavaScript", "Python"); // No hace nada visible
console.log(frase); // Sigue siendo "Me gusta JavaScript"

// Hay que asignar el resultado
const nuevaFrase = frase.replace("JavaScript", "Python");
console.log(nuevaFrase); // "Me gusta Python"
```

### 3. Comparaciones de rendimiento:

```javascript
// Template literals vs concatenación
const nombre = "Juan";
const edad = 30;

// Ambas formas son eficientes para pocos strings
const metodo1 = `Hola ${nombre}, tienes ${edad} años`;
const metodo2 = "Hola " + nombre + ", tienes " + edad + " años";

// Para muchas operaciones, medir y optimizar según el caso
```

## Métodos Modernos (ES2020+)

### 1. String.prototype.matchAll():

```javascript
const texto = "Fecha: 2024-01-15, Fecha: 2024-02-20";
const regex = /(\d{4})-(\d{2})-(\d{2})/g;

// matchAll devuelve un iterador
const matches = [...texto.matchAll(regex)];
console.log(matches[0]); // ["2024-01-15", "2024", "01", "15", ...]
console.log(matches[1]); // ["2024-02-20", "2024", "02", "20", ...]

// Extraer todas las fechas
const fechas = matches.map(match => ({
    fecha: match[0],
    año: match[1],
    mes: match[2],
    dia: match[3]
}));
```

### 2. String.prototype.replaceAll():

```javascript
// Antes tenías que usar regex o múltiples replace()
const texto = "JavaScript y JavaScript y JavaScript";

// ES2021: replaceAll()
const nuevo = texto.replaceAll("JavaScript", "TypeScript");
console.log(nuevo); // "TypeScript y TypeScript y TypeScript"
```

## Casos Especiales y Trucos

### 1. Strings vacíos y falsy:

```javascript
// Verificar string vacío
const verificarVacio = (str) => {
    return str === "";                    // Exactamente vacío
    // return str.trim() === "";          // Vacío o solo espacios
    // return !str;                       // Falsy (incluye null, undefined)
    // return !str || !str.trim();        // Falsy o solo espacios
};

console.log(verificarVacio(""));     // true
console.log(verificarVacio("   "));  // false (con primera opción)
```

### 2. Trabajo con caracteres Unicode:

```javascript
// Emojis y caracteres especiales
const emoji = "👨‍👩‍👧‍👦";
console.log(emoji.length);           // 11 (¡no es 1!)

// Para contar correctamente caracteres Unicode
console.log([...emoji].length);     // 1
console.log(Array.from(emoji).length); // 1

// Iterar correctamente sobre caracteres
for (const char of emoji) {
    console.log(char); // Muestra el emoji completo
}
```

### 3. Strings como arrays:

```javascript
const texto = "JavaScript";

// Usar métodos de array con strings
const vocales = [...texto].filter(char => 'aeiouAEIOU'.includes(char));
console.log(vocales); // ["a", "a", "i"]

// Reversar un string
const alReves = [...texto].reverse().join('');
console.log(alReves); // "tpircSavaJ"

// Contar caracteres
const conteo = [...texto].reduce((acc, char) => {
    acc[char] = (acc[char] || 0) + 1;
    return acc;
}, {});
console.log(conteo); // {J: 1, a: 2, v: 1, S: 1, c: 1, r: 1, i: 1, p: 1, t: 1}
```

## Errores Comunes

### 1. Modificar strings directamente:

```javascript
// ❌ Esto NO funciona
let texto = "Hola";
texto[0] = "h"; // No hace nada
console.log(texto); // Sigue siendo "Hola"

// ✅ Forma correcta
texto = "h" + texto.slice(1);
console.log(texto); // "hola"
```

### 2. Comparaciones con ==:

```javascript
// Cuidado con las conversiones automáticas
console.log("5" == 5);   // true (conversión automática)
console.log("5" === 5);  // false (comparación estricta)

// Siempre usar === para strings
const esNumero = (str) => str === String(Number(str)) && !isNaN(Number(str));
```

### 3. indexOf vs includes:

```javascript
const texto = "JavaScript";

// ❌ Puede fallar
if (texto.indexOf("Script")) {
    // Este bloque NO se ejecuta porque indexOf devuelve 4 (truthy)
    // pero estamos buscando si existe, no la posición
}

// ✅ Formas correctas
if (texto.indexOf("Script") !== -1) { /* existe */ }
if (texto.includes("Script")) { /* existe */ }
```

***

## Resumen Final

### Métodos más utilizados:
- **Conversión**: `toLowerCase()`, `toUpperCase()`, `trim()`
- **Búsqueda**: `indexOf()`, `includes()`, `startsWith()`, `endsWith()`
- **Extracción**: `slice()`, `substring()`
- **Modificación**: `replace()`, `replaceAll()`, `split()`
- **Template literals**: Para interpolación y strings multilínea

### Mejores prácticas:
- Usa **template literals** para interpolación
- Usa **métodos de búsqueda apropiados** (`includes()` vs `indexOf()`)
- Recuerda que los strings son **inmutables**
- Usa **comparación estricta** (`===`)
- Ten cuidado con **caracteres Unicode** y emojis

Los strings son fundamentales en JavaScript y dominar estos métodos te permitirá manipular texto de manera efectiva en tus aplicaciones.
