# Concatenación y Template Strings en JavaScript

La **concatenación** es el proceso de unir dos o más strings para formar uno nuevo. Los **template strings** (también llamados template literals) son una forma moderna y potente de crear y formatear strings en JavaScript.

***

## ¿Qué es la Concatenación?

La **concatenación** es la operación de unir múltiples strings en uno solo. JavaScript ofrece varias formas de concatenar strings, cada una con sus propias ventajas y casos de uso.

```javascript
// Ejemplo básico
const nombre = "Juan";
const apellido = "Pérez";
const nombreCompleto = nombre + " " + apellido; // "Juan Pérez"
```

## Métodos de Concatenación

### 1. Operador de suma (+)

El método más básico y tradicional:

```javascript
const saludo = "Hola";
const nombre = "María";
const mensaje = saludo + ", " + nombre + "!";
console.log(mensaje); // "Hola, María!"

// Con diferentes tipos de datos
const edad = 25;
const presentacion = "Tengo " + edad + " años";
console.log(presentacion); // "Tengo 25 años"

// Concatenación múltiple
const parte1 = "JavaScript";
const parte2 = "es";
const parte3 = "genial";
const frase = parte1 + " " + parte2 + " " + parte3;
console.log(frase); // "JavaScript es genial"
```

#### Ventajas del operador +:
- Simple y directo
- Amplio soporte en navegadores
- Fácil de entender

#### Desventajas del operador +:
- Código difícil de leer con múltiples variables
- Propenso a errores con espacios y formato
- No permite multilínea fácilmente

### 2. Operador de asignación combinada (+=)

Útil para construir strings incrementalmente:

```javascript
let mensaje = "Lista de compras:";
mensaje += "\n- Leche";
mensaje += "\n- Pan";
mensaje += "\n- Huevos";

console.log(mensaje);
// "Lista de compras:
// - Leche
// - Pan
// - Huevos"

// En bucles
let numeros = "";
for (let i = 1; i <= 5; i++) {
    numeros += i;
    if (i < 5) numeros += ", ";
}
console.log(numeros); // "1, 2, 3, 4, 5"
```

### 3. Método concat()

Método nativo de los strings:

```javascript
const str1 = "Hola";
const str2 = "mundo";

// Método concat()
const resultado = str1.concat(" ", str2, "!");
console.log(resultado); // "Hola mundo!"

// Con múltiples argumentos
const mensaje = "".concat("JavaScript", " ", "es", " ", "increíble");
console.log(mensaje); // "JavaScript es increíble"

// Encadenamiento
const frase = "Programar"
    .concat(" en ")
    .concat("JavaScript")
    .concat(" es divertido");
console.log(frase); // "Programar en JavaScript es divertido"
```

#### Ventajas de concat():
- Más funcional que el operador +
- Acepta múltiples argumentos
- No modifica el string original

#### Desventajas de concat():
- Menos legible que otras opciones
- Menos usado en la práctica
- No tan eficiente como el operador +

## Template Strings (Template Literals)

Los **template strings** son una característica de ES6 (ES2015) que revolucionó la forma de trabajar con strings en JavaScript.

### Sintaxis básica

Se definen usando **backticks** (`` ` ``) en lugar de comillas:

```javascript
// Sintaxis básica
const mensaje = `Hola mundo`;
console.log(mensaje); // "Hola mundo"

// Equivalente con comillas
const mensaje2 = "Hola mundo";
```

### Interpolación de variables

La característica más poderosa: insertar variables y expresiones directamente:

```javascript
const nombre = "Ana";
const edad = 28;

// Con template strings
const presentacion = `Mi nombre es ${nombre} y tengo ${edad} años`;
console.log(presentacion); // "Mi nombre es Ana y tengo 28 años"

// Comparación con concatenación tradicional
const presentacionTradicional = "Mi nombre es " + nombre + " y tengo " + edad + " años";

// Con expresiones
const a = 10;
const b = 5;
console.log(`La suma de ${a} y ${b} es ${a + b}`);
// "La suma de 10 y 5 es 15"
```

### Strings multilínea

Una de las ventajas más evidentes:

```javascript
// Con template strings (fácil)
const poema = `
Roses are red,
Violets are blue,
JavaScript is awesome,
And so are you!
`;

// Sin template strings (complicado)
const poemaTradicional = "Roses are red,\n" +
                        "Violets are blue,\n" +
                        "JavaScript is awesome,\n" +
                        "And so are you!";

// Para HTML
const html = `
<div class="card">
    <h2>${titulo}</h2>
    <p>${descripcion}</p>
    <button onclick="accion()">Click me</button>
</div>
`;
```

### Expresiones complejas

Los template strings pueden evaluar cualquier expresión JavaScript:

```javascript
const productos = [
    { nombre: "Laptop", precio: 999 },
    { nombre: "Mouse", precio: 25 }
];

// Operaciones matemáticas
const precio = 100;
const mensaje = `El precio con IVA es ${precio * 1.21}€`;

// Operadores ternarios
const edad = 17;
const estado = `Eres ${edad >= 18 ? 'mayor' : 'menor'} de edad`;

// Llamadas a funciones
function formatearFecha(fecha) {
    return fecha.toLocaleDateString('es-ES');
}

const ahora = new Date();
const fechaTexto = `Hoy es ${formatearFecha(ahora)}`;

// Acceso a propiedades de objetos
const usuario = { nombre: "Carlos", activo: true };
const info = `Usuario: ${usuario.nombre} (${usuario.activo ? 'Activo' : 'Inactivo'})`;

// Métodos de array
const numeros = [1, 2, 3, 4, 5];
const estadisticas = `Total: ${numeros.length}, Suma: ${numeros.reduce((a, b) => a + b)}`;
```

## Casos de Uso Prácticos

### 1. Generación de HTML dinámico

```javascript
function crearTarjetaUsuario(usuario) {
    return `
    <div class="user-card" data-id="${usuario.id}">
        <img src="${usuario.avatar}" alt="Avatar de ${usuario.nombre}">
        <div class="user-info">
            <h3>${usuario.nombre}</h3>
            <p>${usuario.email}</p>
            <span class="status ${usuario.activo ? 'online' : 'offline'}">
                ${usuario.activo ? 'En línea' : 'Desconectado'}
            </span>
        </div>
        <div class="user-actions">
            <button onclick="verPerfil(${usuario.id})">Ver perfil</button>
            <button onclick="enviarMensaje(${usuario.id})">Mensaje</button>
        </div>
    </div>
    `;
}

const usuario = {
    id: 123,
    nombre: "Ana García",
    email: "ana@example.com",
    avatar: "avatar.jpg",
    activo: true
};

document.getElementById('contenedor').innerHTML = crearTarjetaUsuario(usuario);
```

### 2. Construcción de URLs

```javascript
class URLBuilder {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
        this.params = new Map();
    }
    
    addParam(key, value) {
        this.params.set(key, value);
        return this;
    }
    
    build() {
        const paramString = Array.from(this.params.entries())
            .map(([key, value]) => `${encodeURIComponent(key)}=${encodeURIComponent(value)}`)
            .join('&');
            
        return paramString ? `${this.baseUrl}?${paramString}` : this.baseUrl;
    }
}

// Uso
const url = new URLBuilder('https://api.example.com/users')
    .addParam('page', 1)
    .addParam('limit', 10)
    .addParam('search', 'Juan Pérez')
    .build();

console.log(url); // "https://api.example.com/users?page=1&limit=10&search=Juan%20P%C3%A9rez"

// Con template strings para APIs REST
function crearEndpoint(recurso, id = null, accion = null) {
    let url = `/api/${recurso}`;
    if (id) url += `/${id}`;
    if (accion) url += `/${accion}`;
    return url;
}

console.log(crearEndpoint('users'));           // "/api/users"
console.log(crearEndpoint('users', 123));      // "/api/users/123"
console.log(crearEndpoint('users', 123, 'edit')); // "/api/users/123/edit"
```

### 3. Formateo de mensajes

```javascript
// Sistema de mensajes dinámicos
const mensajes = {
    bienvenida: (nombre) => `¡Bienvenido, ${nombre}! Es genial tenerte aquí.`,
    despedida: (nombre) => `Hasta luego, ${nombre}. ¡Que tengas un buen día!`,
    error: (tipo, detalles) => `Error ${tipo}: ${detalles}`,
    exito: (accion) => `✅ ${accion} completado exitosamente`,
    notificacion: (usuario, accion, tiempo) => 
        `${usuario} ${accion} hace ${tiempo}`,
    estadisticas: (datos) => `
📊 Estadísticas:
- Usuarios activos: ${datos.usuarios}
- Ventas del día: ${datos.ventas}€
- Pedidos pendientes: ${datos.pedidos}
    `.trim()
};

// Uso
console.log(mensajes.bienvenida("María"));
console.log(mensajes.error("404", "Página no encontrada"));
console.log(mensajes.estadisticas({
    usuarios: 1250,
    ventas: 15430.50,
    pedidos: 23
}));
```

### 4. Plantillas de email

```javascript
function crearEmailBienvenida(usuario, empresa) {
    return `
Hola ${usuario.nombre},

¡Bienvenido a ${empresa.nombre}!

Estamos emocionados de tenerte como parte de nuestra comunidad. Tu cuenta ha sido creada exitosamente con el email: ${usuario.email}

Próximos pasos:
1. Completa tu perfil visitando: ${empresa.url}/perfil
2. Explora nuestras funcionalidades en: ${empresa.url}/inicio
3. Si tienes preguntas, contáctanos en: ${empresa.soporte}

¡Que disfrutes tu experiencia!

El equipo de ${empresa.nombre}
${empresa.url}

---
Este es un email automático, por favor no respondas a este mensaje.
    `.trim();
}

const usuario = { nombre: "Carlos", email: "carlos@example.com" };
const empresa = { 
    nombre: "TechCorp", 
    url: "https://techcorp.com",
    soporte: "soporte@techcorp.com" 
};

console.log(crearEmailBienvenida(usuario, empresa));
```

### 5. Query builders para bases de datos

```javascript
class QueryBuilder {
    constructor() {
        this.query = {
            type: '',
            table: '',
            fields: [],
            conditions: [],
            joins: [],
            orderBy: [],
            limit: null
        };
    }
    
    select(fields = '*') {
        this.query.type = 'SELECT';
        this.query.fields = Array.isArray(fields) ? fields : [fields];
        return this;
    }
    
    from(table) {
        this.query.table = table;
        return this;
    }
    
    where(condition) {
        this.query.conditions.push(condition);
        return this;
    }
    
    join(table, on) {
        this.query.joins.push(`JOIN ${table} ON ${on}`);
        return this;
    }
    
    orderBy(field, direction = 'ASC') {
        this.query.orderBy.push(`${field} ${direction}`);
        return this;
    }
    
    limit(count) {
        this.query.limit = count;
        return this;
    }
    
    build() {
        const { type, fields, table, conditions, joins, orderBy, limit } = this.query;
        
        let sql = `${type} ${fields.join(', ')} FROM ${table}`;
        
        if (joins.length) {
            sql += ` ${joins.join(' ')}`;
        }
        
        if (conditions.length) {
            sql += ` WHERE ${conditions.join(' AND ')}`;
        }
        
        if (orderBy.length) {
            sql += ` ORDER BY ${orderBy.join(', ')}`;
        }
        
        if (limit) {
            sql += ` LIMIT ${limit}`;
        }
        
        return sql;
    }
}

// Uso
const query = new QueryBuilder()
    .select(['name', 'email', 'created_at'])
    .from('users')
    .join('profiles', 'users.id = profiles.user_id')
    .where('users.active = 1')
    .where('users.created_at > "2024-01-01"')
    .orderBy('created_at', 'DESC')
    .limit(10)
    .build();

console.log(query);
// SELECT name, email, created_at FROM users JOIN profiles ON users.id = profiles.user_id WHERE users.active = 1 AND users.created_at > "2024-01-01" ORDER BY created_at DESC LIMIT 10
```

## Mejores Prácticas

### 1. Cuándo usar cada método

```javascript
// ✅ Template strings para interpolación
const mensaje = `Hola ${nombre}, tu saldo es ${saldo}€`;

// ✅ Operador + para concatenaciones simples
const nombreCompleto = nombre + " " + apellido;

// ✅ Array.join() para muchos elementos
const lista = items.map(item => `<li>${item}</li>`).join('');

// ✅ concat() para programación funcional
const resultado = str1.concat(" - ").concat(str2);
```

### 2. Evitar errores comunes

```javascript
// ❌ Problemas con tipos
const edad = 25;
const mensaje1 = "Tengo " + edad + " años"; // Correcto
const mensaje2 = `Tengo ${edad} años`;      // Correcto
const mensaje3 = "Tengo " + edad + 5 + " años"; // "Tengo 255 años" ❌

// ✅ Forzar orden de operaciones
const mensaje4 = "Tengo " + (edad + 5) + " años"; // "Tengo 30 años"
const mensaje5 = `Tengo ${edad + 5} años`;        // "Tengo 30 años"

// ❌ Olvidar espacios
const mal = nombre + apellido;        // "JuanPérez"
const bien = nombre + " " + apellido; // "Juan Pérez"
const mejor = `${nombre} ${apellido}`; // "Juan Pérez"

// ❌ Concatenación ineficiente en bucles
let resultado = "";
for (let i = 0; i < 1000; i++) {
    resultado += `Item ${i} `; // Lento
}

// ✅ Método eficiente
const resultado2 = Array.from({length: 1000}, (_, i) => `Item ${i}`).join(' ');
```

### 3. Sanitización y seguridad

```javascript
// ⚠️ Peligro de XSS en aplicaciones web
function crearMensajeInseguro(usuario, mensaje) {
    return `<div>Usuario ${usuario}: ${mensaje}</div>`;
}

// Si usuario = "<script>alert('hack')</script>", es peligroso

// ✅ Función de escape
function escapeHtml(str) {
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
}

function crearMensajeSeguro(usuario, mensaje) {
    return `<div>Usuario ${escapeHtml(usuario)}: ${escapeHtml(mensaje)}</div>`;
}

// ✅ O usar librerías como DOMPurify para casos complejos
```

### 4. Internacionalización

```javascript
// ✅ Preparar strings para traducción
const mensajes = {
    es: {
        bienvenida: (nombre) => `Bienvenido, ${nombre}`,
        despedida: (nombre) => `Adiós, ${nombre}`
    },
    en: {
        bienvenida: (nombre) => `Welcome, ${nombre}`,
        despedida: (nombre) => `Goodbye, ${nombre}`
    }
};

function obtenerMensaje(idioma, clave, ...args) {
    const mensaje = mensajes[idioma]?.[clave];
    return mensaje ? mensaje(...args) : `Mensaje no encontrado: ${clave}`;
}

console.log(obtenerMensaje('es', 'bienvenida', 'Ana')); // "Bienvenido, Ana"
console.log(obtenerMensaje('en', 'bienvenida', 'Ana')); // "Welcome, Ana"
```

## Herramientas y Utilidades

### 1. Funciones helper para strings

```javascript
// Capitalizar primera letra
const capitalize = (str) => str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();

// Pluralizar palabras (inglés básico)
const pluralize = (word, count) => count === 1 ? word : `${word}s`;

// Truncar texto
const truncate = (str, length, suffix = '...') => 
    str.length > length ? str.slice(0, length) + suffix : str;

// Slug para URLs
const slugify = (str) => str
    .toLowerCase()
    .replace(/[^\w\s-]/g, '')
    .replace(/\s+/g, '-')
    .replace(/--+/g, '-')
    .trim('-');

// Uso con template strings
const producto = { nombre: "Smartphone Galaxy", cantidad: 3, precio: 299.99 };

const descripcion = `
${capitalize(producto.nombre)} - ${pluralize('item', producto.cantidad)}
Precio: ${producto.precio}€ cada uno
Total: ${(producto.precio * producto.cantidad).toFixed(2)}€
URL: /productos/${slugify(producto.nombre)}
`.trim();

console.log(descripcion);
```

### 2. Validadores con template strings

```javascript
// Validador de emails con mensajes personalizados
function validarEmail(email, nombreCampo = 'Email') {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    
    if (!email) {
        return { valido: false, mensaje: `${nombreCampo} es requerido` };
    }
    
    if (!regex.test(email)) {
        return { 
            valido: false, 
            mensaje: `${nombreCampo} "${email}" no tiene un formato válido` 
        };
    }
    
    return { valido: true, mensaje: `${nombreCampo} es válido` };
}

// Uso
console.log(validarEmail('', 'Correo electrónico'));
console.log(validarEmail('invalid-email', 'Email personal'));
console.log(validarEmail('user@example.com', 'Email de contacto'));
```

### 3. Logger con template strings

```javascript
class Logger {
    static log(nivel, mensaje, ...extras) {
        const timestamp = new Date().toISOString();
        const logMessage = `[${timestamp}] ${nivel.toUpperCase()}: ${mensaje}`;
        
        if (extras.length) {
            console.log(logMessage, ...extras);
        } else {
            console.log(logMessage);
        }
    }
    
    static info(mensaje, ...extras) {
        this.log('info', mensaje, ...extras);
    }
    
    static warn(mensaje, ...extras) {
        this.log('warn', mensaje, ...extras);
    }
    
    static error(mensaje, ...extras) {
        this.log('error', mensaje, ...extras);
    }
}

// Uso
Logger.info('Aplicación iniciada');
Logger.warn(`Usuario ${usuario.id} intentó acceso no autorizado`);
Logger.error(`Error en base de datos`, { query: 'SELECT * FROM users', error: 'Connection timeout' });
```

***

## Resumen Final

### Cuándo usar cada método:

- **Template strings (`` ` ``)**: Para la mayoría de casos, especialmente con variables
- **Operador (+)**: Para concatenaciones simples y rápidas
- **Array.join()**: Para unir múltiples elementos con separadores
- **concat()**: Para programación funcional o encadenamiento

### Ventajas de template strings:
- **Interpolación de variables** con `${}`
- **Strings multilínea** naturales
- **Expresiones complejas** dentro de `${}`
- **Tagged templates** para procesamiento avanzado
- **Mejor legibilidad** del código

### Mejores prácticas:
- **Usar template strings** para interpolación
- **Validar y sanitizar** entrada de usuarios
- **Ser eficiente** en bucles largos (usar arrays + join)
- **Preparar para internacionalización**
- **Mantener legibilidad** sobre micro-optimizaciones

Los template strings han revolucionado el trabajo con strings en JavaScript, proporcionando una sintaxis más limpia y poderosa para la mayoría de casos de uso.
