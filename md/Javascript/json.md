# JSON en JavaScript

**JSON** (JavaScript Object Notation) es un formato de intercambio de datos ligero y fácil de leer que se ha convertido en el estándar para la comunicación entre aplicaciones web. Aunque nació de JavaScript, es independiente del lenguaje y se usa universalmente en APIs, configuración de aplicaciones y almacenamiento de datos.

## ¿Qué es JSON?

JSON es un formato de texto que representa datos estructurados usando una sintaxis derivada de JavaScript. Es completamente independiente del lenguaje pero utiliza convenciones familiares para programadores de la familia de lenguajes C (JavaScript, C, C++, Java, Python, etc.).

### Características principales

- **📝 Legible por humanos**: Fácil de leer y escribir
- **🚀 Ligero**: Menos verboso que XML
- **🌐 Universal**: Soportado por todos los lenguajes modernos
- **📊 Estructurado**: Permite datos jerárquicos complejos
- **🔒 Seguro**: No ejecuta código, solo datos

```javascript
// Ejemplo básico de JSON
const datosJSON = `{
    "nombre": "María García",
    "edad": 28,
    "activo": true,
    "direccion": {
        "calle": "Calle Principal 123",
        "ciudad": "Madrid",
        "codigoPostal": "28001"
    },
    "hobbies": ["lectura", "cine", "cocina"],
    "telefono": null
}`;

console.log("📄 JSON como string:", datosJSON);
```

## Sintaxis de JSON

### Tipos de datos soportados

JSON soporta seis tipos de datos básicos:

```javascript
// Tipos de datos en JSON
const ejemploTipos = {
    // 1. String (cadenas de texto)
    "texto": "Hola mundo",
    "nombre": "Juan Pérez",
    "descripcion": "Un texto con \"comillas\" y \n saltos de línea",
    
    // 2. Number (números)
    "entero": 42,
    "decimal": 3.14159,
    "negativo": -100,
    "cientifico": 1.23e-4,
    
    // 3. Boolean (booleanos)
    "activo": true,
    "completado": false,
    
    // 4. null (valor nulo)
    "valorVacio": null,
    
    // 5. Object (objetos)
    "usuario": {
        "id": 123,
        "nombre": "Ana",
        "configuracion": {
            "tema": "oscuro",
            "idioma": "es"
        }
    },
    
    // 6. Array (arreglos)
    "numeros": [1, 2, 3, 4, 5],
    "mixto": ["texto", 42, true, null, {"clave": "valor"}],
    "vacio": []
};

console.log("🔧 Tipos de datos JSON:", ejemploTipos);
```

### Reglas importantes de sintaxis

```javascript
// ✅ JSON válido
const jsonValido = `{
    "clave": "valor",
    "numero": 123,
    "booleano": true,
    "array": [1, 2, 3],
    "objeto": {
        "anidado": "valor"
    },
    "nulo": null
}`;

// ❌ JSON inválido - errores comunes
const erroresComunes = `
// Error 1: Claves sin comillas
{
    clave: "valor"  // ❌ Debe ser "clave"
}

// Error 2: Comillas simples
{
    "clave": 'valor'  // ❌ Debe usar comillas dobles
}

// Error 3: Coma final
{
    "clave": "valor",  // ❌ Coma después del último elemento
}

// Error 4: Comentarios
{
    "clave": "valor"  // ❌ Los comentarios no están permitidos
}

// Error 5: Funciones
{
    "funcion": function() { return "hola"; }  // ❌ No se permiten funciones
}

// Error 6: undefined
{
    "valor": undefined  // ❌ Usar null en su lugar
}
`;

// Demostrar diferencias entre JavaScript y JSON
console.log("=== DIFERENCIAS JAVASCRIPT VS JSON ===");

// JavaScript (más flexible)
const objetoJS = {
    nombre: 'Juan',              // Sin comillas en claves
    edad: 30,
    activo: true,
    funcion: () => "hola",       // Funciones permitidas
    valorIndefinido: undefined,   // undefined permitido
    // comentarios permitidos
};

// JSON (más estricto)
const objetoJSON = {
    "nombre": "Juan",            // Claves con comillas
    "edad": 30,
    "activo": true,
    // "funcion": NO PERMITIDO
    "valorVacio": null           // null en lugar de undefined
    // comentarios NO permitidos
};

console.log("📝 Objeto JavaScript:", objetoJS);
console.log("📄 Objeto JSON válido:", objetoJSON);
```

## JSON.stringify() - Convertir a JSON

### Uso básico de stringify

```javascript
// Convertir objetos JavaScript a JSON
const usuario = {
    id: 123,
    nombre: "Carlos López",
    email: "carlos@email.com",
    activo: true,
    fechaRegistro: new Date("2023-01-15"),
    configuracion: {
        tema: "claro",
        notificaciones: true,
        idioma: "es"
    },
    hobbies: ["programación", "música", "deportes"]
};

// Conversión básica
const usuarioJSON = JSON.stringify(usuario);
console.log("📤 Usuario convertido a JSON:");
console.log(usuarioJSON);

// Conversión con formato legible (indentación)
const usuarioJSONFormateado = JSON.stringify(usuario, null, 2);
console.log("📄 JSON formateado:");
console.log(usuarioJSONFormateado);

// Conversión con indentación personalizada
const usuarioJSONTabs = JSON.stringify(usuario, null, "\t");
console.log("📄 JSON con tabs:");
console.log(usuarioJSONTabs);
```

### Parámetro replacer - Filtrar propiedades

```javascript
const producto = {
    id: 1001,
    nombre: "Laptop Gaming",
    precio: 1299.99,
    precioInterno: 800,      // Campo interno
    stock: 15,
    descripcion: "Laptop de alta gama para gaming",
    categoria: "Tecnología",
    proveedor: {
        id: 501,
        nombre: "TechCorp",
        contactoInterno: "secreto@techcorp.com"  // Campo interno
    },
    fechaCreacion: new Date(),
    activo: true
};

// 1. Filtrar con array de propiedades
const camposPublicos = ["id", "nombre", "precio", "stock", "descripcion", "categoria", "activo"];
const productoPublico = JSON.stringify(producto, camposPublicos, 2);

console.log("🔒 Solo campos públicos:");
console.log(productoPublico);

// 2. Filtrar con función replacer
const productoFiltrado = JSON.stringify(producto, (clave, valor) => {
    // Filtrar campos internos y fechas
    if (clave.includes("Interno") || clave.includes("fecha")) {
        return undefined; // Excluir del JSON
    }
    
    // Formatear precio
    if (clave === "precio") {
        return `${valor} EUR`;
    }
    
    // Formatear booleanos
    if (typeof valor === "boolean") {
        return valor ? "Sí" : "No";
    }
    
    return valor;
}, 2);

console.log("⚙️ Producto filtrado y formateado:");
console.log(productoFiltrado);

// 3. Transformar objetos anidados
const productoTransformado = JSON.stringify(producto, (clave, valor) => {
    // Convertir fechas a string legible
    if (valor instanceof Date) {
        return valor.toLocaleDateString("es-ES");
    }
    
    // Transformar objeto proveedor
    if (clave === "proveedor") {
        return {
            nombre: valor.nombre,
            id: valor.id
            // Omitir contactoInterno
        };
    }
    
    return valor;
}, 2);

console.log("🔄 Producto transformado:");
console.log(productoTransformado);
```

### Manejo de valores especiales

```javascript
// Valores que se comportan de manera especial en JSON.stringify
const objetoEspecial = {
    // Valores que se convierten a null
    valorUndefined: undefined,
    
    // Funciones se ignoran completamente
    miFuncion: function() { return "hola"; },
    miFuncionFlecha: () => "mundo",
    
    // Symbol se ignoran
    [Symbol("clave")]: "valor simbólico",
    
    // Fechas se convierten a string ISO
    fecha: new Date("2023-06-15T10:30:00.000Z"),
    
    // NaN e Infinity se convierten a null
    numeroInvalido: NaN,
    infinito: Infinity,
    menosInfinito: -Infinity,
    
    // RegExp se convierte a objeto vacío
    expresionRegular: /[a-z]+/g,
    
    // Arrays mantienen undefined como null
    arrayConUndefined: [1, undefined, 3, null, 5],
    
    // Propiedades válidas
    nombre: "Objeto especial",
    numero: 42,
    booleano: true,
    nulo: null
};

console.log("🔍 Objeto original:");
console.log(objetoEspecial);

const jsonEspecial = JSON.stringify(objetoEspecial, null, 2);
console.log("📄 Convertido a JSON:");
console.log(jsonEspecial);

// Demostrar comportamiento en arrays
const arrayEspecial = [
    "texto",
    42,
    true,
    null,
    undefined,        // Se convierte a null
    function() {},    // Se convierte a null
    new Date(),       // Se convierte a string ISO
    /regex/          // Se convierte a objeto vacío {}
];

console.log("📋 Array original:");
console.log(arrayEspecial);

const jsonArray = JSON.stringify(arrayEspecial, null, 2);
console.log("📄 Array en JSON:");
console.log(jsonArray);
```

### Método toJSON() personalizado

```javascript
// Objetos pueden definir su propia serialización JSON
class Usuario {
    constructor(nombre, email, password) {
        this.id = Math.random().toString(36).substr(2, 9);
        this.nombre = nombre;
        this.email = email;
        this.password = password; // Campo sensible
        this.fechaCreacion = new Date();
        this.activo = true;
    }
    
    // Método personalizado para JSON.stringify
    toJSON() {
        return {
            id: this.id,
            nombre: this.nombre,
            email: this.email,
            // Omitir password por seguridad
            fechaCreacion: this.fechaCreacion.toISOString(),
            activo: this.activo,
            tipo: "Usuario" // Campo adicional
        };
    }
    
    // Método para incluir datos completos (uso interno)
    toJSONCompleto() {
        return {
            id: this.id,
            nombre: this.nombre,
            email: this.email,
            password: this.password, // Incluir para backup/migración
            fechaCreacion: this.fechaCreacion.toISOString(),
            activo: this.activo
        };
    }
}

const usuario = new Usuario("Ana Martínez", "ana@email.com", "password123");

console.log("👤 Usuario completo:");
console.log(usuario);

console.log("🔒 JSON público (automático):");
console.log(JSON.stringify(usuario, null, 2));

console.log("🔓 JSON completo (manual):");
console.log(JSON.stringify(usuario.toJSONCompleto(), null, 2));

// Ejemplo con objeto Date personalizado
class FechaPersonalizada extends Date {
    toJSON() {
        return {
            timestamp: this.getTime(),
            iso: this.toISOString(),
            legible: this.toLocaleDateString("es-ES"),
            hora: this.toLocaleTimeString("es-ES")
        };
    }
}

const fechaEspecial = new FechaPersonalizada();
console.log("📅 Fecha personalizada en JSON:");
console.log(JSON.stringify({ fecha: fechaEspecial }, null, 2));
```

## JSON.parse() - Convertir desde JSON

### Uso básico de parse

```javascript
// String JSON para parsear
const jsonString = `{
    "usuario": {
        "id": 456,
        "nombre": "Laura Fernández",
        "email": "laura@email.com",
        "activo": true
    },
    "preferencias": {
        "tema": "oscuro",
        "idioma": "es",
        "notificaciones": {
            "email": true,
            "push": false,
            "sms": null
        }
    },
    "tags": ["premium", "beta-tester", "developer"],
    "ultimoAcceso": "2023-10-18T10:30:00.000Z",
    "sesionesActivas": 3
}`;

// Parsear JSON básico
try {
    const objeto = JSON.parse(jsonString);
    
    console.log("✅ JSON parseado exitosamente:");
    console.log("👤 Nombre del usuario:", objeto.usuario.nombre);
    console.log("🎨 Tema preferido:", objeto.preferencias.tema);
    console.log("🏷️ Tags:", objeto.tags.join(", "));
    console.log("📅 Último acceso:", objeto.ultimoAcceso);
    
} catch (error) {
    console.error("❌ Error parseando JSON:", error.message);
}

// Ejemplo con diferentes tipos de datos
const ejemplosJSON = [
    '{"nombre": "Juan", "edad": 30}',           // Objeto válido
    '[1, 2, 3, "cuatro", true]',               // Array válido
    '"texto simple"',                          // String válido
    '42',                                      // Número válido
    'true',                                    // Boolean válido
    'null',                                    // Null válido
    // '{"nombre": "Juan",}',                  // ❌ Coma final (comentado)
    // '{nombre: "Juan"}',                     // ❌ Sin comillas en clave (comentado)
];

ejemplosJSON.forEach((json, index) => {
    try {
        const resultado = JSON.parse(json);
        console.log(`✅ Ejemplo ${index + 1}:`, typeof resultado, resultado);
    } catch (error) {
        console.log(`❌ Ejemplo ${index + 1} falló:`, error.message);
    }
});
```

### Parámetro reviver - Transformar valores

```javascript
const datosJSON = `{
    "id": 789,
    "nombre": "Pedro González",
    "fechaNacimiento": "1990-03-15T00:00:00.000Z",
    "fechaRegistro": "2023-01-20T14:30:00.000Z",
    "ultimaActividad": "2023-10-18T09:15:00.000Z",
    "configuracion": {
        "precio_producto": "29.99",
        "descuento_aplicado": "15.5",
        "fecha_vencimiento": "2024-12-31T23:59:59.000Z"
    },
    "coordenadas": {
        "latitud": "40.4168",
        "longitud": "-3.7038"
    },
    "activo": "true",
    "contador": "42"
}`;

// Usar reviver para transformar datos durante el parseo
const objetoTransformado = JSON.parse(datosJSON, (clave, valor) => {
    // Convertir strings de fecha a objetos Date
    if (clave.includes("fecha") || clave.includes("Fecha") || clave.includes("Actividad")) {
        return new Date(valor);
    }
    
    // Convertir strings numéricos a números
    if (clave.includes("precio") || clave.includes("descuento") || clave.includes("contador")) {
        return parseFloat(valor);
    }
    
    // Convertir coordenadas a números
    if (clave === "latitud" || clave === "longitud") {
        return parseFloat(valor);
    }
    
    // Convertir string "true"/"false" a boolean
    if (clave === "activo") {
        return valor === "true";
    }
    
    return valor;
});

console.log("🔄 Objeto con transformaciones:");
console.log("📅 Fecha de nacimiento:", objetoTransformado.fechaNacimiento);
console.log("💰 Precio:", typeof objetoTransformado.configuracion.precio_producto, objetoTransformado.configuracion.precio_producto);
console.log("📍 Coordenadas:", typeof objetoTransformado.coordenadas.latitud, objetoTransformado.coordenadas);
console.log("✅ Activo:", typeof objetoTransformado.activo, objetoTransformado.activo);

// Reviver más sofisticado con validaciones
const jsonConValidacion = `{
    "email": "usuario@email.com",
    "telefono": "+34-123-456-789",
    "url": "https://mi-sitio.com",
    "fechaCreacion": "2023-10-18T10:30:00.000Z"
}`;

const objetoValidado = JSON.parse(jsonConValidacion, (clave, valor) => {
    // Validar email
    if (clave === "email") {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailRegex.test(valor)) {
            throw new Error(`Email inválido: ${valor}`);
        }
        return valor.toLowerCase(); // Normalizar a minúsculas
    }
    
    // Validar y transformar teléfono
    if (clave === "telefono") {
        const telefonoLimpio = valor.replace(/\D/g, ""); // Solo dígitos
        if (telefonoLimpio.length < 9) {
            throw new Error(`Teléfono inválido: ${valor}`);
        }
        return telefonoLimpio;
    }
    
    // Validar URL
    if (clave === "url") {
        try {
            return new URL(valor);
        } catch {
            throw new Error(`URL inválida: ${valor}`);
        }
    }
    
    // Convertir fechas
    if (clave.includes("fecha") || clave.includes("Fecha")) {
        return new Date(valor);
    }
    
    return valor;
});

console.log("✅ Objeto validado y transformado:");
console.log(objetoValidado);
```

### Manejo de errores en parse

```javascript
// Ejemplos de JSON inválido y manejo de errores
const jsonInvalidos = [
    '{nombre: "Juan"}',                    // Sin comillas en clave
    '{"nombre": "Juan",}',                 // Coma final
    "{'nombre': 'Juan'}",                  // Comillas simples
    '{"nombre": "Juan" "edad": 30}',       // Falta coma
    '{"nombre": undefined}',               // undefined no válido
    '{función: function(){}}',             // Función no válida
    '{"texto": "sin cerrar',               // String sin cerrar
    '{"numero": 01}',                      // Número con cero inicial
    '// comentario\n{"nombre": "Juan"}',   // Comentarios
    '{',                                   // JSON incompleto
];

function parsearConManejo(jsonString, descripcion) {
    try {
        const objeto = JSON.parse(jsonString);
        console.log(`✅ ${descripcion}:`, objeto);
        return objeto;
    } catch (error) {
        console.error(`❌ ${descripcion}:`);
        console.error(`   Error: ${error.message}`);
        console.error(`   JSON: ${jsonString}`);
        return null;
    }
}

console.log("=== MANEJO DE ERRORES EN JSON.parse ===");

jsonInvalidos.forEach((json, index) => {
    parsearConManejo(json, `Ejemplo ${index + 1}`);
});

// Función robusta para parsear JSON con fallback
function parsearJSONSeguro(jsonString, valorPorDefecto = null) {
    try {
        // Verificar que no esté vacío
        if (!jsonString || jsonString.trim() === "") {
            console.warn("⚠️ JSON string vacío");
            return valorPorDefecto;
        }
        
        const objeto = JSON.parse(jsonString);
        
        // Verificar que no sea null (técnicamente válido pero quizás no deseado)
        if (objeto === null) {
            console.warn("⚠️ JSON es null");
            return valorPorDefecto;
        }
        
        return objeto;
        
    } catch (error) {
        console.error("❌ Error parseando JSON:", error.message);
        console.error("📄 JSON problemático:", jsonString.substring(0, 100) + "...");
        return valorPorDefecto;
    }
}

// Probar función segura
const jsonDudoso = '{"nombre": "válido"}';
const jsonMalo = '{nombre: sin comillas}';
const jsonVacio = '';

console.log("\n=== FUNCIÓN SEGURA ===");
console.log("JSON válido:", parsearJSONSeguro(jsonDudoso, {}));
console.log("JSON inválido:", parsearJSONSeguro(jsonMalo, {}));
console.log("JSON vacío:", parsearJSONSeguro(jsonVacio, {}));
```

## Casos Prácticos Avanzados

### Sistema de configuración con JSON

```javascript
class GestorConfiguracion {
    constructor() {
        this.configuracionPorDefecto = {
            aplicacion: {
                nombre: "Mi App",
                version: "1.0.0",
                modo: "desarrollo"
            },
            interfaz: {
                tema: "claro",
                idioma: "es",
                animaciones: true,
                notificaciones: {
                    sonido: true,
                    vibrar: false,
                    mostrarEnPantalla: true
                }
            },
            rendimiento: {
                cacheTTL: 300000,
                maxRequestsConcurrentes: 5,
                timeoutPorDefecto: 10000
            },
            funcionalidades: {
                modoOffline: false,
                sincronizacionAuto: true,
                backupAuto: true
            }
        };
        
        this.configuracionActual = null;
    }
    
    // Cargar configuración desde JSON
    cargarConfiguracion(jsonString) {
        try {
            const configuracion = JSON.parse(jsonString, (clave, valor) => {
                // Validar valores numéricos específicos
                if (clave === "cacheTTL" && (valor < 1000 || valor > 3600000)) {
                    console.warn(`⚠️ cacheTTL inválido (${valor}), usando por defecto`);
                    return this.configuracionPorDefecto.rendimiento.cacheTTL;
                }
                
                if (clave === "maxRequestsConcurrentes" && (valor < 1 || valor > 20)) {
                    console.warn(`⚠️ maxRequestsConcurrentes inválido (${valor}), usando por defecto`);
                    return this.configuracionPorDefecto.rendimiento.maxRequestsConcurrentes;
                }
                
                // Validar idioma
                if (clave === "idioma" && !["es", "en", "fr", "de"].includes(valor)) {
                    console.warn(`⚠️ Idioma no soportado (${valor}), usando español`);
                    return "es";
                }
                
                // Validar tema
                if (clave === "tema" && !["claro", "oscuro", "auto"].includes(valor)) {
                    console.warn(`⚠️ Tema no soportado (${valor}), usando claro`);
                    return "claro";
                }
                
                return valor;
            });
            
            // Fusionar con configuración por defecto
            this.configuracionActual = this.fusionarConfiguraciones(
                this.configuracionPorDefecto,
                configuracion
            );
            
            console.log("✅ Configuración cargada exitosamente");
            return true;
            
        } catch (error) {
            console.error("❌ Error cargando configuración:", error.message);
            this.configuracionActual = { ...this.configuracionPorDefecto };
            return false;
        }
    }
    
    // Guardar configuración como JSON
    guardarConfiguracion() {
        try {
            const jsonString = JSON.stringify(this.configuracionActual, (clave, valor) => {
                // Filtrar propiedades temporales o sensibles
                if (clave.startsWith("_temp") || clave.includes("password")) {
                    return undefined;
                }
                
                // Formatear fechas si las hay
                if (valor instanceof Date) {
                    return valor.toISOString();
                }
                
                return valor;
            }, 2);
            
            console.log("💾 Configuración guardada:");
            return jsonString;
            
        } catch (error) {
            console.error("❌ Error guardando configuración:", error.message);
            return null;
        }
    }
    
    // Fusionar configuraciones recursivamente
    fusionarConfiguraciones(porDefecto, personalizada) {
        const resultado = { ...porDefecto };
        
        for (const [clave, valor] in Object.entries(personalizada)) {
            if (valor !== null && typeof valor === "object" && !Array.isArray(valor)) {
                // Fusión recursiva para objetos
                resultado[clave] = this.fusionarConfiguraciones(
                    resultado[clave] || {},
                    valor
                );
            } else {
                // Sobrescribir valor directo
                resultado[clave] = valor;
            }
        }
        
        return resultado;
    }
    
    // Obtener valor de configuración por ruta
    obtener(ruta) {
        const partes = ruta.split(".");
        let valor = this.configuracionActual;
        
        for (const parte of partes) {
            if (valor && typeof valor === "object" && parte in valor) {
                valor = valor[parte];
            } else {
                return undefined;
            }
        }
        
        return valor;
    }
    
    // Establecer valor de configuración por ruta
    establecer(ruta, nuevoValor) {
        const partes = ruta.split(".");
        const claveFinal = partes.pop();
        let objeto = this.configuracionActual;
        
        for (const parte of partes) {
            if (!(parte in objeto)) {
                objeto[parte] = {};
            }
            objeto = objeto[parte];
        }
        
        objeto[claveFinal] = nuevoValor;
        console.log(`⚙️ Configuración actualizada: ${ruta} = ${nuevoValor}`);
    }
}

// Ejemplo de uso del gestor de configuración
const gestor = new GestorConfiguracion();

// JSON de configuración personalizada
const configPersonalizada = `{
    "aplicacion": {
        "modo": "produccion"
    },
    "interfaz": {
        "tema": "oscuro",
        "idioma": "en",
        "notificaciones": {
            "sonido": false
        }
    },
    "rendimiento": {
        "maxRequestsConcurrentes": 10
    },
    "funcionalidades": {
        "modoOffline": true
    }
}`;

// Cargar configuración
gestor.cargarConfiguracion(configPersonalizada);

// Usar configuración
console.log("🎨 Tema actual:", gestor.obtener("interfaz.tema"));
console.log("🔊 Sonido habilitado:", gestor.obtener("interfaz.notificaciones.sonido"));
console.log("📶 Modo offline:", gestor.obtener("funcionalidades.modoOffline"));

// Modificar configuración
gestor.establecer("interfaz.animaciones", false);
gestor.establecer("rendimiento.timeoutPorDefecto", 15000);

// Guardar configuración actualizada
const configGuardada = gestor.guardarConfiguracion();
console.log("💾 JSON de configuración final:");
console.log(configGuardada);
```

### API REST con JSON

```javascript
class ClienteAPI {
    constructor(baseURL) {
        this.baseURL = baseURL;
        this.headers = {
            "Content-Type": "application/json",
            "Accept": "application/json"
        };
    }
    
    // GET - Obtener datos
    async get(endpoint, parametros = {}) {
        try {
            const url = new URL(endpoint, this.baseURL);
            
            // Agregar parámetros de consulta
            Object.entries(parametros).forEach(([clave, valor]) => {
                if (valor !== null && valor !== undefined) {
                    url.searchParams.append(clave, valor);
                }
            });
            
            console.log(`📥 GET ${url.toString()}`);
            
            const response = await fetch(url.toString(), {
                method: "GET",
                headers: this.headers
            });
            
            return await this.procesarRespuesta(response);
            
        } catch (error) {
            console.error("❌ Error en GET:", error.message);
            throw error;
        }
    }
    
    // POST - Crear datos
    async post(endpoint, datos) {
        try {
            const url = new URL(endpoint, this.baseURL);
            
            // Convertir datos a JSON
            const body = JSON.stringify(datos, (clave, valor) => {
                // Filtrar campos internos
                if (clave.startsWith("_") || clave === "password") {
                    return undefined;
                }
                
                // Formatear fechas
                if (valor instanceof Date) {
                    return valor.toISOString();
                }
                
                return valor;
            });
            
            console.log(`📤 POST ${url.toString()}`);
            console.log("📄 Datos enviados:", body);
            
            const response = await fetch(url.toString(), {
                method: "POST",
                headers: this.headers,
                body: body
            });
            
            return await this.procesarRespuesta(response);
            
        } catch (error) {
            console.error("❌ Error en POST:", error.message);
            throw error;
        }
    }
    
    // PUT - Actualizar datos
    async put(endpoint, datos) {
        try {
            const url = new URL(endpoint, this.baseURL);
            const body = JSON.stringify(datos);
            
            console.log(`🔄 PUT ${url.toString()}`);
            
            const response = await fetch(url.toString(), {
                method: "PUT",
                headers: this.headers,
                body: body
            });
            
            return await this.procesarRespuesta(response);
            
        } catch (error) {
            console.error("❌ Error en PUT:", error.message);
            throw error;
        }
    }
    
    // DELETE - Eliminar datos
    async delete(endpoint) {
        try {
            const url = new URL(endpoint, this.baseURL);
            
            console.log(`🗑️ DELETE ${url.toString()}`);
            
            const response = await fetch(url.toString(), {
                method: "DELETE",
                headers: this.headers
            });
            
            return await this.procesarRespuesta(response);
            
        } catch (error) {
            console.error("❌ Error en DELETE:", error.message);
            throw error;
        }
    }
    
    // Procesar respuesta de la API
    async procesarRespuesta(response) {
        const contentType = response.headers.get("content-type");
        
        if (!response.ok) {
            let errorMessage = `HTTP ${response.status}: ${response.statusText}`;
            
            // Intentar obtener mensaje de error del JSON
            if (contentType && contentType.includes("application/json")) {
                try {
                    const errorData = await response.json();
                    errorMessage = errorData.message || errorData.error || errorMessage;
                } catch {
                    // Si no se puede parsear, usar mensaje por defecto
                }
            }
            
            throw new Error(errorMessage);
        }
        
        // Procesar respuesta exitosa
        if (contentType && contentType.includes("application/json")) {
            const data = await response.json();
            
            // Transformar datos recibidos
            return this.transformarDatos(data);
        } else {
            return await response.text();
        }
    }
    
    // Transformar datos recibidos de la API
    transformarDatos(data) {
        return JSON.parse(JSON.stringify(data), (clave, valor) => {
            // Convertir fechas ISO a objetos Date
            if (typeof valor === "string" && /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}/.test(valor)) {
                return new Date(valor);
            }
            
            // Convertir strings numéricos a números si es apropiado
            if (clave.includes("id") && typeof valor === "string" && /^\d+$/.test(valor)) {
                return parseInt(valor, 10);
            }
            
            return valor;
        });
    }
}

// Ejemplo de uso del cliente API
async function ejemploAPI() {
    const api = new ClienteAPI("https://jsonplaceholder.typicode.com");
    
    try {
        // GET - Obtener usuarios
        console.log("=== OBTENER USUARIOS ===");
        const usuarios = await api.get("/users", { _limit: 3 });
        console.log("👥 Usuarios obtenidos:", usuarios.length);
        
        // POST - Crear usuario
        console.log("\n=== CREAR USUARIO ===");
        const nuevoUsuario = {
            name: "María García",
            username: "mgarcia",
            email: "maria@email.com",
            phone: "123-456-789",
            website: "maria-garcia.com",
            address: {
                street: "Calle Principal",
                suite: "123",
                city: "Madrid",
                zipcode: "28001"
            },
            fechaRegistro: new Date()
        };
        
        const usuarioCreado = await api.post("/users", nuevoUsuario);
        console.log("✅ Usuario creado:", usuarioCreado);
        
        // PUT - Actualizar usuario
        console.log("\n=== ACTUALIZAR USUARIO ===");
        const datosActualizacion = {
            name: "María García López",
            email: "maria.lopez@email.com"
        };
        
        const usuarioActualizado = await api.put("/users/1", datosActualizacion);
        console.log("🔄 Usuario actualizado:", usuarioActualizado);
        
        // GET - Obtener posts de un usuario
        console.log("\n=== OBTENER POSTS ===");
        const posts = await api.get("/posts", { userId: 1, _limit: 2 });
        console.log("📝 Posts obtenidos:", posts);
        
    } catch (error) {
        console.error("💥 Error en ejemplo API:", error.message);
    }
}

// Ejecutar ejemplo (descomenta para probar con API real)
// ejemploAPI();
```

### Validación y esquemas JSON

```javascript
class ValidadorJSON {
    constructor() {
        this.esquemas = new Map();
    }
    
    // Registrar esquema de validación
    registrarEsquema(nombre, esquema) {
        this.esquemas.set(nombre, esquema);
        console.log(`📋 Esquema '${nombre}' registrado`);
    }
    
    // Validar objeto contra esquema
    validar(objeto, nombreEsquema) {
        const esquema = this.esquemas.get(nombreEsquema);
        
        if (!esquema) {
            throw new Error(`Esquema '${nombreEsquema}' no encontrado`);
        }
        
        const errores = [];
        this.validarRecursivo(objeto, esquema, "", errores);
        
        return {
            valido: errores.length === 0,
            errores: errores
        };
    }
    
    // Validación recursiva
    validarRecursivo(objeto, esquema, ruta, errores) {
        // Validar tipo
        if (esquema.tipo && typeof objeto !== esquema.tipo) {
            errores.push({
                ruta: ruta,
                mensaje: `Se esperaba tipo '${esquema.tipo}', recibido '${typeof objeto}'`
            });
            return;
        }
        
        // Validar requerido
        if (esquema.requerido && (objeto === null || objeto === undefined)) {
            errores.push({
                ruta: ruta,
                mensaje: "Campo requerido"
            });
            return;
        }
        
        // Validar string
        if (esquema.tipo === "string") {
            if (esquema.minLength && objeto.length < esquema.minLength) {
                errores.push({
                    ruta: ruta,
                    mensaje: `Longitud mínima: ${esquema.minLength}`
                });
            }
            
            if (esquema.maxLength && objeto.length > esquema.maxLength) {
                errores.push({
                    ruta: ruta,
                    mensaje: `Longitud máxima: ${esquema.maxLength}`
                });
            }
            
            if (esquema.patron && !esquema.patron.test(objeto)) {
                errores.push({
                    ruta: ruta,
                    mensaje: `No coincide con el patrón requerido`
                });
            }
        }
        
        // Validar number
        if (esquema.tipo === "number") {
            if (esquema.min !== undefined && objeto < esquema.min) {
                errores.push({
                    ruta: ruta,
                    mensaje: `Valor mínimo: ${esquema.min}`
                });
            }
            
            if (esquema.max !== undefined && objeto > esquema.max) {
                errores.push({
                    ruta: ruta,
                    mensaje: `Valor máximo: ${esquema.max}`
                });
            }
        }
        
        // Validar array
        if (esquema.tipo === "object" && Array.isArray(objeto)) {
            if (esquema.items) {
                objeto.forEach((item, index) => {
                    this.validarRecursivo(
                        item,
                        esquema.items,
                        `${ruta}[${index}]`,
                        errores
                    );
                });
            }
        }
        
        // Validar object
        if (esquema.tipo === "object" && !Array.isArray(objeto) && objeto !== null) {
            if (esquema.propiedades) {
                Object.entries(esquema.propiedades).forEach(([prop, subEsquema]) => {
                    const nuevaRuta = ruta ? `${ruta}.${prop}` : prop;
                    
                    if (prop in objeto) {
                        this.validarRecursivo(objeto[prop], subEsquema, nuevaRuta, errores);
                    } else if (subEsquema.requerido) {
                        errores.push({
                            ruta: nuevaRuta,
                            mensaje: "Propiedad requerida no encontrada"
                        });
                    }
                });
            }
        }
    }
    
    // Validar y limpiar JSON
    validarYLimpiar(jsonString, nombreEsquema) {
        try {
            // Parsear JSON
            const objeto = JSON.parse(jsonString);
            
            // Validar contra esquema
            const validacion = this.validar(objeto, nombreEsquema);
            
            if (!validacion.valido) {
                return {
                    exito: false,
                    errores: validacion.errores,
                    objeto: null
                };
            }
            
            // Limpiar objeto según esquema
            const objetoLimpio = this.limpiarObjeto(objeto, this.esquemas.get(nombreEsquema));
            
            return {
                exito: true,
                errores: [],
                objeto: objetoLimpio
            };
            
        } catch (error) {
            return {
                exito: false,
                errores: [{ ruta: "", mensaje: `Error de JSON: ${error.message}` }],
                objeto: null
            };
        }
    }
    
    // Limpiar objeto según esquema (remover propiedades no definidas)
    limpiarObjeto(objeto, esquema) {
        if (esquema.tipo === "object" && esquema.propiedades) {
            const objetoLimpio = {};
            
            Object.entries(esquema.propiedades).forEach(([prop, subEsquema]) => {
                if (prop in objeto) {
                    objetoLimpio[prop] = this.limpiarObjeto(objeto[prop], subEsquema);
                }
            });
            
            return objetoLimpio;
        }
        
        return objeto;
    }
}

// Ejemplo de uso del validador
const validador = new ValidadorJSON();

// Registrar esquema de usuario
validador.registrarEsquema("usuario", {
    tipo: "object",
    propiedades: {
        id: {
            tipo: "number",
            requerido: true,
            min: 1
        },
        nombre: {
            tipo: "string",
            requerido: true,
            minLength: 2,
            maxLength: 50
        },
        email: {
            tipo: "string",
            requerido: true,
            patron: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
        },
        edad: {
            tipo: "number",
            min: 0,
            max: 120
        },
        activo: {
            tipo: "boolean",
            requerido: true
        },
        direccion: {
            tipo: "object",
            propiedades: {
                calle: {
                    tipo: "string",
                    requerido: true
                },
                ciudad: {
                    tipo: "string",
                    requerido: true
                },
                codigoPostal: {
                    tipo: "string",
                    patron: /^\d{5}$/
                }
            }
        }
    }
});

// Datos de prueba
const jsonValido = `{
    "id": 123,
    "nombre": "Ana García",
    "email": "ana@email.com",
    "edad": 28,
    "activo": true,
    "direccion": {
        "calle": "Calle Principal 123",
        "ciudad": "Madrid",
        "codigoPostal": "28001"
    },
    "propiedadExtra": "Esta será removida"
}`;

const jsonInvalido = `{
    "id": -1,
    "nombre": "A",
    "email": "email-invalido",
    "edad": 150,
    "activo": "true",
    "direccion": {
        "calle": "Calle Principal 123"
    }
}`;

console.log("=== VALIDACIÓN JSON ===");

// Validar JSON válido
console.log("✅ Validando JSON válido:");
const resultadoValido = validador.validarYLimpiar(jsonValido, "usuario");
if (resultadoValido.exito) {
    console.log("✅ Validación exitosa");
    console.log("🧹 Objeto limpio:", resultadoValido.objeto);
} else {
    console.log("❌ Errores encontrados:", resultadoValido.errores);
}

// Validar JSON inválido
console.log("\n❌ Validando JSON inválido:");
const resultadoInvalido = validador.validarYLimpiar(jsonInvalido, "usuario");
if (resultadoInvalido.exito) {
    console.log("✅ Validación exitosa");
} else {
    console.log("❌ Errores encontrados:");
    resultadoInvalido.errores.forEach(error => {
        console.log(`   ${error.ruta}: ${error.mensaje}`);
    });
}
```

## Mejores Prácticas con JSON

### ✅ Buenas prácticas

```javascript
// 1. Siempre usar try/catch para JSON.parse()
function parsearSeguro(jsonString, valorPorDefecto = null) {
    try {
        return JSON.parse(jsonString);
    } catch (error) {
        console.error("Error parseando JSON:", error.message);
        return valorPorDefecto;
    }
}

// 2. Validar datos antes de stringify
function stringifySeguro(objeto) {
    try {
        // Verificar que el objeto es serializable
        if (objeto === undefined) {
            return JSON.stringify(null);
        }
        
        return JSON.stringify(objeto);
    } catch (error) {
        console.error("Error stringifying:", error.message);
        return null;
    }
}

// 3. Usar replacer para filtrar datos sensibles
function serializar ParaAPI(objeto) {
    return JSON.stringify(objeto, (clave, valor) => {
        // Filtrar campos sensibles
        if (clave.includes("password") || clave.includes("secret") || clave.includes("token")) {
            return undefined;
        }
        
        // Formatear fechas
        if (valor instanceof Date) {
            return valor.toISOString();
        }
        
        return valor;
    });
}

// 4. Normalizar datos al parsear
function parsearDesdeAPI(jsonString) {
    return JSON.parse(jsonString, (clave, valor) => {
        // Convertir fechas ISO
        if (typeof valor === "string" && /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}/.test(valor)) {
            return new Date(valor);
        }
        
        // Normalizar emails a minúsculas
        if (clave === "email" && typeof valor === "string") {
            return valor.toLowerCase();
        }
        
        return valor;
    });
}

// 5. Validar estructura de JSON
function validarEstructura(objeto, esquemaRequerido) {
    const camposRequeridos = Object.keys(esquemaRequerido);
    
    for (const campo of camposRequeridos) {
        if (!(campo in objeto)) {
            throw new Error(`Campo requerido faltante: ${campo}`);
        }
    }
    
    return true;
}
```

### ❌ Prácticas a evitar

```javascript
// ❌ NO parsear JSON sin manejo de errores
function malaPractica1(jsonString) {
    const objeto = JSON.parse(jsonString); // Puede fallar
    return objeto;
}

// ❌ NO incluir datos sensibles en JSON
const malaPractica2 = JSON.stringify({
    usuario: "admin",
    password: "123456", // ❌ Nunca incluir passwords
    token: "abc123"     // ❌ Tokens sensibles
});

// ❌ NO usar eval() como alternativa a JSON.parse()
function malaPractica3(jsonString) {
    return eval("(" + jsonString + ")"); // ❌ PELIGROSO - permite ejecución de código
}

// ❌ NO asumir que JSON.stringify siempre funciona
function malaPractica4(objeto) {
    return JSON.stringify(objeto); // Puede fallar con referencias circulares
}

// ❌ NO ignorar el contexto de los datos
function malaPractica5(datos) {
    // ❌ No validar ni transformar datos
    localStorage.setItem("datos", JSON.stringify(datos));
}

// ✅ Versiones corregidas
function buenasPracticas() {
    // Parseo seguro
    function parsearSeguro(jsonString) {
        try {
            return JSON.parse(jsonString);
        } catch (error) {
            console.error("Error JSON:", error);
            return null;
        }
    }
    
    // Serialización segura
    function serializarSeguro(objeto) {
        return JSON.stringify(objeto, (clave, valor) => {
            // Filtrar campos sensibles
            if (clave.includes("password")) return undefined;
            
            // Manejar referencias circulares básicas
            if (typeof valor === "object" && valor !== null) {
                if (valor.__serializing) return "[Circular]";
                valor.__serializing = true;
            }
            
            return valor;
        });
    }
    
    // Validación antes de guardar
    function guardarDatos(datos) {
        if (!datos || typeof datos !== "object") {
            throw new Error("Datos inválidos");
        }
        
        const json = serializarSeguro(datos);
        if (json) {
            localStorage.setItem("datos", json);
        }
    }
}
```

## Herramientas y Debugging

### Formateo y debugging de JSON

```javascript
class JSONDebugger {
    static formatear(objeto, opciones = {}) {
        const {
            indentacion = 2,
            mostrarTipos = false,
            colorizar = false
        } = opciones;
        
        let json = JSON.stringify(objeto, (clave, valor) => {
            if (mostrarTipos && valor !== null && valor !== undefined) {
                return {
                    _tipo: typeof valor,
                    _valor: valor
                };
            }
            return valor;
        }, indentacion);
        
        if (colorizar && typeof window !== "undefined") {
            // Colorizar para navegador
            json = json
                .replace(/"([^"]+)":/g, '<span style="color: blue">"$1":</span>')
                .replace(/:\s*"([^"]*)"/g, ': <span style="color: green">"$1"</span>')
                .replace(/:\s*(\d+)/g, ': <span style="color: red">$1</span>')
                .replace(/:\s*(true|false)/g, ': <span style="color: purple">$1</span>');
        }
        
        return json;
    }
    
    static validarYMostrarErrores(jsonString) {
        try {
            const objeto = JSON.parse(jsonString);
            console.log("✅ JSON válido:");
            console.log(this.formatear(objeto, { mostrarTipos: true }));
            return objeto;
        } catch (error) {
            console.error("❌ JSON inválido:");
            console.error("Error:", error.message);
            
            // Intentar mostrar dónde está el error
            const lineas = jsonString.split("\n");
            const match = error.message.match(/position (\d+)/);
            
            if (match) {
                const posicion = parseInt(match[1]);
                let caracterActual = 0;
                
                for (let i = 0; i < lineas.length; i++) {
                    const longitudLinea = lineas[i].length + 1; // +1 por el salto de línea
                    
                    if (caracterActual + longitudLinea > posicion) {
                        const columna = posicion - caracterActual;
                        console.error(`Error aproximadamente en línea ${i + 1}, columna ${columna}:`);
                        console.error(lineas[i]);
                        console.error(" ".repeat(columna - 1) + "^");
                        break;
                    }
                    
                    caracterActual += longitudLinea;
                }
            }
            
            return null;
        }
    }
    
    static compararJSONs(json1, json2, mostrarDiferencias = true) {
        const obj1 = typeof json1 === "string" ? JSON.parse(json1) : json1;
        const obj2 = typeof json2 === "string" ? JSON.parse(json2) : json2;
        
        const diferencias = [];
        this.compararRecursivo(obj1, obj2, "", diferencias);
        
        if (mostrarDiferencias) {
            if (diferencias.length === 0) {
                console.log("✅ Los JSONs son idénticos");
            } else {
                console.log("📊 Diferencias encontradas:");
                diferencias.forEach(diff => {
                    console.log(`  ${diff.ruta}: ${diff.tipo} - ${diff.descripcion}`);
                });
            }
        }
        
        return {
            iguales: diferencias.length === 0,
            diferencias: diferencias
        };
    }
    
    static compararRecursivo(obj1, obj2, ruta, diferencias) {
        if (typeof obj1 !== typeof obj2) {
            diferencias.push({
                ruta: ruta || "raíz",
                tipo: "tipo",
                descripcion: `Tipo diferente: ${typeof obj1} vs ${typeof obj2}`
            });
            return;
        }
        
        if (obj1 === null || obj2 === null) {
            if (obj1 !== obj2) {
                diferencias.push({
                    ruta: ruta || "raíz",
                    tipo: "valor",
                    descripcion: `${obj1} vs ${obj2}`
                });
            }
            return;
        }
        
        if (typeof obj1 === "object") {
            const keys1 = Object.keys(obj1);
            const keys2 = Object.keys(obj2);
            
            // Verificar claves faltantes
            keys1.forEach(key => {
                if (!(key in obj2)) {
                    diferencias.push({
                        ruta: ruta ? `${ruta}.${key}` : key,
                        tipo: "clave",
                        descripcion: "Clave solo existe en el primer objeto"
                    });
                }
            });
            
            keys2.forEach(key => {
                if (!(key in obj1)) {
                    diferencias.push({
                        ruta: ruta ? `${ruta}.${key}` : key,
                        tipo: "clave",
                        descripcion: "Clave solo existe en el segundo objeto"
                    });
                }
            });
            
            // Comparar valores de claves comunes
            keys1.forEach(key => {
                if (key in obj2) {
                    this.compararRecursivo(
                        obj1[key],
                        obj2[key],
                        ruta ? `${ruta}.${key}` : key,
                        diferencias
                    );
                }
            });
        } else if (obj1 !== obj2) {
            diferencias.push({
                ruta: ruta || "raíz",
                tipo: "valor",
                descripcion: `${obj1} vs ${obj2}`
            });
        }
    }
}

// Ejemplos de debugging
console.log("=== HERRAMIENTAS DE DEBUGGING JSON ===");

// JSON con errores
const jsonConErrores = `{
    "nombre": "Juan",
    "edad": 30,
    "activo": true,
    "hobbies": ["lectura", "cine"],
}`;  // Coma extra

console.log("🔍 Validando JSON con errores:");
JSONDebugger.validarYMostrarErrores(jsonConErrores);

// JSON válido
const jsonValido = `{
    "nombre": "Juan",
    "edad": 30,
    "activo": true,
    "hobbies": ["lectura", "cine"]
}`;

console.log("\n✅ Validando JSON válido:");
JSONDebugger.validarYMostrarErrores(jsonValido);

// Comparar JSONs
const json1 = { nombre: "Juan", edad: 30, activo: true };
const json2 = { nombre: "Juan", edad: 31, ciudad: "Madrid" };

console.log("\n📊 Comparando JSONs:");
JSONDebugger.compararJSONs(json1, json2);
```

***

JSON es fundamental en el desarrollo web moderno. Dominar su uso, validación y manejo de errores es esencial para crear aplicaciones robustas que intercambien datos de manera eficiente y segura. Recuerda siempre validar y manejar errores apropiadamente al trabajar con datos JSON.
