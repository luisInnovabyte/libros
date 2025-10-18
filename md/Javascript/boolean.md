# Variables Booleanas en JavaScript

Las **variables booleanas** representan valores de verdad en JavaScript, pudiendo ser únicamente `true` (verdadero) o `false` (falso). Son fundamentales para la lógica de programación y el control de flujo.

***

## ¿Qué son las Variables Booleanas?

Un **booleano** es un tipo de dato primitivo que puede tener solo dos valores posibles:
- `true` (verdadero)
- `false` (falso)

```javascript
const esVerdadero = true;
const esFalso = false;

console.log(typeof esVerdadero); // "boolean"
console.log(typeof esFalso);     // "boolean"
```

## Declaración de Variables Booleanas

### Declaración directa

```javascript
// Usando let (si el valor puede cambiar)
let estaLogueado = false;
let esAdministrador = true;

// Usando const (si el valor no cambiará)
const esProduccion = true;
const modoDebug = false;

// Usando var (no recomendado, pero válido)
var esActivo = true;
```

## Conversión a Boolean

### Conversión explícita

```javascript
// Método 1: Función Boolean()
console.log(Boolean(1));          // true
console.log(Boolean(0));          // false
console.log(Boolean("texto"));    // true
console.log(Boolean(""));         // false

// Método 2: Doble negación (!!)
console.log(!!1);                 // true
console.log(!!0);                 // false
console.log(!!"texto");           // true
console.log(!!"");                // false

// Método 3: Comparación con valores booleanos
console.log("texto" ? true : false);  // true
console.log("" ? true : false);       // false
```

### Conversión implícita

JavaScript convierte automáticamente a boolean en estos contextos:

```javascript
const nombre = "Juan";
const edad = 0;

// En estructuras condicionales
if (nombre) {
    console.log("Nombre existe"); // Se ejecuta (nombre es truthy)
}

if (edad) {
    console.log("Edad existe"); // NO se ejecuta (0 es falsy)
}

// En operadores lógicos
const resultado1 = nombre && "Nombre válido";   // "Nombre válido"
const resultado2 = edad && "Edad válida";       // 0 (falsy)

// En operador ternario
const mensaje = nombre ? "Hola " + nombre : "Hola invitado";
```

## Operadores Lógicos

### Operador AND (&&)

```javascript
const a = true;
const b = false;

console.log(a && b);        // false
console.log(true && true);  // true
console.log(true && false); // false
console.log(false && true); // false

// Con valores no booleanos (short-circuit)
console.log("texto" && "otro");  // "otro" (devuelve el último truthy)
console.log("texto" && "");      // "" (devuelve el primer falsy)
console.log("" && "texto");      // "" (devuelve el primer falsy)

// Casos prácticos
const usuario = { nombre: "Ana" };
const saludo = usuario && usuario.nombre && `Hola ${usuario.nombre}`;
console.log(saludo); // "Hola Ana"

// Evitar errores
const elemento = document.getElementById("miElemento");
elemento && elemento.addEventListener("click", miFuncion);
```

### Operador OR (||)

```javascript
const a = true;
const b = false;

console.log(a || b);        // true
console.log(true || true);  // true
console.log(true || false); // true
console.log(false || false); // false

// Con valores no booleanos
console.log("" || "defecto");      // "defecto"
console.log("valor" || "defecto"); // "valor"
console.log(null || undefined || "defecto"); // "defecto"

// Valores por defecto (antes de ES2020)
function saludar(nombre) {
    nombre = nombre || "Invitado";
    return `Hola ${nombre}`;
}

console.log(saludar());        // "Hola Invitado"
console.log(saludar("Luis"));  // "Hola Luis"

// Configuración con valores por defecto
const config = {
    tema: configuracionUsuario.tema || "claro",
    idioma: configuracionUsuario.idioma || "es",
    notificaciones: configuracionUsuario.notificaciones || true
};
```

### Operador NOT (!)

```javascript
const verdadero = true;
const falso = false;

console.log(!verdadero);  // false
console.log(!falso);      // true

// Doble negación para conversión
console.log(!!"texto");   // true
console.log(!!"");        // false
console.log(!!0);         // false
console.log(!!1);         // true

// Invertir booleanos
let estaVisible = true;
estaVisible = !estaVisible; // false

// En condicionales
const usuarios = [];
if (!usuarios.length) {
    console.log("No hay usuarios"); // Se ejecuta
}
```

## Comparaciones Booleanas

### Comparaciones de igualdad

```javascript
// Comparación con == (con conversión de tipo)
console.log(true == 1);       // true
console.log(false == 0);      // true
console.log(true == "1");     // true
console.log(false == "");     // true
console.log(false == null);   // false
console.log(false == undefined); // false

// Comparación con === (sin conversión de tipo)
console.log(true === 1);      // false
console.log(false === 0);     // false
console.log(true === true);   // true
console.log(false === false); // true

// Mejores prácticas: usar siempre ===
const esActivo = true;
if (esActivo === true) {
    console.log("Usuario activo");
}
```

### Comparaciones complejas

```javascript
// Arrays y objetos (comparación por referencia)
console.log([] == []);        // false (diferentes referencias)
console.log({} == {});        // false (diferentes referencias)

const array1 = [];
const array2 = array1;
console.log(array1 == array2); // true (misma referencia)

// Conversiones extrañas con ==
console.log([] == false);     // true ([] se convierte a "")
console.log("" == false);     // true (ambos se convierten a 0)
console.log(0 == false);      // true (false se convierte a 0)

// Por eso siempre usar ===
console.log([] === false);    // false
console.log("" === false);    // false
console.log(0 === false);     // false
```

## Casos de Uso Prácticos

### 1. Control de flujo

```javascript
// Estados de aplicación
let estaLogueado = false;
let esAdministrador = false;
let tienePermisos = false;

function verificarAcceso() {
    if (!estaLogueado) {
        return "Debe iniciar sesión";
    }
    
    if (!esAdministrador && !tienePermisos) {
        return "Sin permisos suficientes";
    }
    
    return "Acceso permitido";
}

// Validaciones
function validarFormulario(datos) {
    const nombreValido = datos.nombre && datos.nombre.trim().length > 0;
    const emailValido = datos.email && datos.email.includes("@");
    const edadValida = datos.edad && datos.edad >= 18;
    
    return nombreValido && emailValido && edadValida;
}
```

### 2. Configuraciones

```javascript
// Configuración de aplicación
const configuracion = {
    // Características
    modoOscuro: true,
    notificacionesPush: false,
    sonidosActivados: true,
    
    // Permisos
    puedeEditar: false,
    puedeEliminar: false,
    puedeCompartir: true,
    
    // Estados
    esPremium: false,
    esBeta: true,
    esProduccion: false
};

// Función para alternar configuraciones
function alternar(propiedad) {
    if (propiedad in configuracion) {
        configuracion[propiedad] = !configuracion[propiedad];
    }
}

alternar('modoOscuro');        // Cambia a false
alternar('notificacionesPush'); // Cambia a true
```

### 3. Validación de datos

```javascript
// Validador de campos
class ValidadorCampos {
    static esTextoValido(texto, minimo = 1) {
        return typeof texto === 'string' && 
               texto.trim().length >= minimo;
    }
    
    static esEmailValido(email) {
        return typeof email === 'string' && 
               email.includes('@') && 
               email.includes('.');
    }
    
    static esNumeroValido(numero, min = -Infinity, max = Infinity) {
        return typeof numero === 'number' && 
               !isNaN(numero) && 
               numero >= min && 
               numero <= max;
    }
    
    static esArrayNoVacio(array) {
        return Array.isArray(array) && array.length > 0;
    }
}

// Uso del validador
const datosUsuario = {
    nombre: "Juan",
    email: "juan@example.com",
    edad: 25,
    hobbies: ["programar", "leer"]
};

const esValido = ValidadorCampos.esTextoValido(datosUsuario.nombre) &&
                ValidadorCampos.esEmailValido(datosUsuario.email) &&
                ValidadorCampos.esNumeroValido(datosUsuario.edad, 0, 120) &&
                ValidadorCampos.esArrayNoVacio(datosUsuario.hobbies);

console.log(`Datos válidos: ${esValido}`);
```

### 4. Estados de componentes

```javascript
// Simulación de un componente React/Vue
class ComponenteFormulario {
    constructor() {
        this.estado = {
            cargando: false,
            enviado: false,
            errores: false,
            habilitado: true,
            visible: true
        };
    }
    
    enviarFormulario() {
        this.estado.cargando = true;
        this.estado.enviado = false;
        this.estado.errores = false;
        
        // Simular envío
        setTimeout(() => {
            this.estado.cargando = false;
            this.estado.enviado = true;
        }, 2000);
    }
    
    obtenerClasesCSS() {
        const clases = [];
        
        if (this.estado.cargando) clases.push('cargando');
        if (this.estado.enviado) clases.push('enviado');
        if (this.estado.errores) clases.push('error');
        if (!this.estado.habilitado) clases.push('deshabilitado');
        if (!this.estado.visible) clases.push('oculto');
        
        return clases.join(' ');
    }
    
    puedeEnviar() {
        return !this.estado.cargando && 
               !this.estado.enviado && 
               this.estado.habilitado;
    }
}
```

### 5. Filtros y búsquedas

```javascript
// Sistema de filtros
const productos = [
    { nombre: "Laptop", categoria: "tecnologia", disponible: true, precio: 999 },
    { nombre: "Libro", categoria: "educacion", disponible: false, precio: 20 },
    { nombre: "Mouse", categoria: "tecnologia", disponible: true, precio: 25 },
    { nombre: "Curso", categoria: "educacion", disponible: true, precio: 50 }
];

class FiltroProductos {
    constructor(productos) {
        this.productos = productos;
    }
    
    filtrar(opciones = {}) {
        return this.productos.filter(producto => {
            // Filtro por disponibilidad
            if (opciones.soloDisponibles && !producto.disponible) {
                return false;
            }
            
            // Filtro por categoría
            if (opciones.categoria && producto.categoria !== opciones.categoria) {
                return false;
            }
            
            // Filtro por precio
            if (opciones.precioMaximo && producto.precio > opciones.precioMaximo) {
                return false;
            }
            
            // Filtro por búsqueda de texto
            if (opciones.busqueda) {
                const coincide = producto.nombre
                    .toLowerCase()
                    .includes(opciones.busqueda.toLowerCase());
                if (!coincide) return false;
            }
            
            return true;
        });
    }
}

const filtro = new FiltroProductos(productos);

// Buscar productos disponibles de tecnología con precio máximo 100
const resultados = filtro.filtrar({
    soloDisponibles: true,
    categoria: "tecnologia",
    precioMaximo: 100
});

console.log(resultados); // [{ nombre: "Mouse", ... }]
```

## Errores Comunes y Mejores Prácticas

### Errores comunes

```javascript
// ❌ Error 1: Usar new Boolean()
const mal = new Boolean(false);
if (mal) console.log("Se ejecuta incorrectamente");

// ✅ Correcto: Usar Boolean() o literal
const bien = Boolean(false);
const mejor = false;

// ❌ Error 2: Comparar con == en lugar de ===
if (usuario.activo == "true") { } // Problemático
if (usuario.activo === true) { }  // Correcto

// ❌ Error 3: No entender truthy/falsy
if (array.length) { }  // Puede fallar si length es 0
if (array.length > 0) { }  // Más claro

// ❌ Error 4: Confundir nullish con falsy
const config = opciones.limite || 100;    // Problema si limite es 0
const config2 = opciones.limite ?? 100;   // Correcto

// ❌ Error 5: No validar tipos
function procesar(activo) {
    if (activo) { }  // ¿Qué pasa si activo es "false"?
}

// ✅ Mejor validación
function procesar(activo) {
    if (typeof activo === 'boolean' && activo) { }
    // O convertir explícitamente
    if (Boolean(activo)) { }
}
```

### Mejores prácticas

```javascript
// ✅ 1. Usar nombres descriptivos
const estaLogueado = true;           // Claro
const usuarioTienePermisos = false;  // Descriptivo
const puedeEditar = true;            // Específico

// ✅ 2. Usar funciones para lógica compleja
function puedeAccederRecurso(usuario, recurso) {
    return usuario.activo && 
           usuario.permisos.includes(recurso.tipo) && 
           !recurso.bloqueado;
}

// ✅ 3. Validar entrada
function establecerModo(modoOscuro) {
    if (typeof modoOscuro !== 'boolean') {
        throw new Error('modoOscuro debe ser un boolean');
    }
    this.configuracion.modoOscuro = modoOscuro;
}

// ✅ 4. Usar const para valores que no cambian
const ES_DESARROLLO = process.env.NODE_ENV === 'development';
const TIENE_SOPORTE_LOCAL_STORAGE = 'localStorage' in window;

// ✅ 5. Agrupar booleanos relacionados
const permisos = {
    leer: true,
    escribir: false,
    eliminar: false,
    administrar: false
};

// ✅ 6. Usar métodos descriptivos
class Usuario {
    constructor(datos) {
        this.activo = datos.activo;
        this.administrador = datos.administrador;
    }
    
    puedeAcceder(recurso) {
        return this.estaActivo() && this.tienePermisos(recurso);
    }
    
    estaActivo() {
        return this.activo === true;
    }
    
    esAdministrador() {
        return this.administrador === true;
    }
    
    tienePermisos(recurso) {
        return this.esAdministrador() || recurso.publico;
    }
}
```

### Debugging de booleanos

```javascript
// Función helper para debug
function debugBoolean(valor, descripcion) {
    console.log(`${descripcion}:`);
    console.log(`  Valor: ${valor}`);
    console.log(`  Tipo: ${typeof valor}`);
    console.log(`  Es truthy: ${!!valor}`);
    console.log(`  Es strictly true: ${valor === true}`);
    console.log(`  Es strictly false: ${valor === false}`);
    console.log('---');
}

// Ejemplos de debug
debugBoolean(true, "Boolean true");
debugBoolean(false, "Boolean false");
debugBoolean(1, "Número 1");
debugBoolean(0, "Número 0");
debugBoolean("", "String vacío");
debugBoolean("false", "String 'false'");
debugBoolean([], "Array vacío");
debugBoolean(null, "Null");
```

***

## Resumen Final

### Puntos clave sobre booleanos:

- **Solo dos valores**: `true` y `false`
- **Tipo primitivo**: `typeof boolean`
- **Evitar `new Boolean()`**: Crea objetos, no primitivos
- **Truthy vs Falsy**: Entender qué valores se evalúan como true/false

### Valores falsy (evalúan a false):
- `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`

### Operadores lógicos:
- **AND (`&&`)**: Ambos deben ser truthy
- **OR (`||`)**: Al menos uno debe ser truthy  
- **NOT (`!`)**: Invierte el valor booleano
- **Nullish (`??`)**: Solo para `null` y `undefined`

### Mejores prácticas:
- **Usar `===`** para comparaciones estrictas
- **Nombres descriptivos** para variables booleanas
- **Validar tipos** cuando sea necesario
- **Usar funciones** para lógica compleja
- **Entender la diferencia** entre falsy y nullish

Los booleanos son fundamentales para el control de flujo y la lógica en JavaScript. Dominar estos conceptos te permitirá escribir código más claro y evitar errores comunes.
