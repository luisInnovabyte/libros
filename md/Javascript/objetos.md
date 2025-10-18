# Objetos en JavaScript - Conceptos Básicos

Los **objetos** en JavaScript son estructuras de datos fundamentales que permiten almacenar múltiples valores relacionados en forma de pares **clave-valor** (key-value pairs).

***

## ¿Qué son los Objetos?

Un **objeto** es una colección de **propiedades**, donde cada propiedad está definida por un par clave-valor:
- La **clave** (también llamada propiedad o key) es siempre un string
- El **valor** puede ser cualquier tipo de dato: string, number, boolean, array, función, otro objeto, etc.

```javascript
// Ejemplo básico de objeto
const persona = {
    nombre: "Juan",    // clave: "nombre", valor: "Juan"
    edad: 30,          // clave: "edad", valor: 30
    activo: true       // clave: "activo", valor: true
};
```

## Sintaxis de Declaración

### Notación literal de objetos (Object Literal)

La forma más común y recomendada:

```javascript
// Objeto vacío
const objetoVacio = {};

// Objeto con propiedades
const estudiante = {
    nombre: "María",
    edad: 22,
    carrera: "Ingeniería",
    activo: true
};

// Objeto con diferentes tipos de valores
const usuario = {
    id: 123,
    nombre: "Carlos",
    email: "carlos@example.com",
    hobbies: ["leer", "programar", "viajar"],
    direccion: {
        calle: "Av. Principal 123",
        ciudad: "Madrid",
        codigoPostal: "28001"
    }
};
```

## Tipos de Propiedades

### Propiedades con nombres simples

```javascript
const coche = {
    marca: "Toyota",
    modelo: "Corolla",
    año: 2023,
    color: "rojo"
};
```

### Propiedades con nombres especiales

```javascript
const objeto = {
    "nombre completo": "Juan Pérez",     // Espacios en el nombre
    "123": "número como string",         // Número como string
    "mi-propiedad": "con guiones",       // Con guiones
    "@symbol": "con símbolo"             // Con símbolos especiales
};
```

### Propiedades computadas

```javascript
const propiedad = "dinamica";
const valor = "Este es un valor dinámico";

const objeto = {
    [propiedad]: valor,                  // Propiedad dinámica
    [`${propiedad}_2`]: "otro valor",    // Con template literals
    [1 + 2]: "suma como clave"           // Expresión como clave
};

console.log(objeto);
// {
//   dinamica: "Este es un valor dinámico",
//   dinamica_2: "otro valor",
//   3: "suma como clave"
// }
```

## Acceso a Propiedades

### Notación de punto (Dot Notation)

La más común para propiedades con nombres válidos:

```javascript
const persona = {
    nombre: "Luis",
    edad: 28,
    trabajo: "Desarrollador"
};

// Leer propiedades
console.log(persona.nombre);    // "Luis"
console.log(persona.edad);      // 28
console.log(persona.trabajo);   // "Desarrollador"

// Acceder a propiedades anidadas
const empresa = {
    nombre: "TechCorp",
    direccion: {
        calle: "Calle Mayor 1",
        ciudad: "Barcelona"
    }
};

console.log(empresa.nombre);              // "TechCorp"
console.log(empresa.direccion.calle);     // "Calle Mayor 1"
console.log(empresa.direccion.ciudad);    // "Barcelona"
```

### Notación de corchetes (Bracket Notation)

Necesaria para propiedades con nombres especiales o dinámicos:

```javascript
const datos = {
    "nombre completo": "Ana García",
    "123": "número",
    edad: 30
};

// Acceso con corchetes
console.log(datos["nombre completo"]);   // "Ana García"
console.log(datos["123"]);               // "número"
console.log(datos["edad"]);              // 30

// Acceso dinámico
const propiedad = "edad";
console.log(datos[propiedad]);           // 30

// Con variables
const campo = "nombre completo";
console.log(datos[campo]);               // "Ana García"
```

### Comparación de notaciones

```javascript
const usuario = {
    nombre: "Pedro",
    "apellido completo": "Rodríguez García",
    123: "número como propiedad"
};

// Notación de punto - solo para nombres válidos
console.log(usuario.nombre);             // ✅ "Pedro"
// console.log(usuario.apellido completo); // ❌ Error de sintaxis
// console.log(usuario.123);               // ❌ Error de sintaxis

// Notación de corchetes - para cualquier nombre
console.log(usuario["nombre"]);             // ✅ "Pedro"
console.log(usuario["apellido completo"]); // ✅ "Rodríguez García"
console.log(usuario["123"]);               // ✅ "número como propiedad"
console.log(usuario[123]);                 // ✅ "número como propiedad" (conversión automática)
```

## Propiedades Inexistentes

### Acceso a propiedades que no existen

```javascript
const persona = {
    nombre: "Carlos",
    edad: 35
};

console.log(persona.nombre);        // "Carlos"
console.log(persona.apellido);      // undefined (no existe)
console.log(persona.trabajo);       // undefined (no existe)

// Verificar si existe una propiedad
console.log("nombre" in persona);    // true
console.log("apellido" in persona);  // false

// Usando hasOwnProperty
console.log(persona.hasOwnProperty("nombre"));   // true
console.log(persona.hasOwnProperty("apellido")); // false
```

### Valores por defecto para propiedades inexistentes

```javascript
const configuracion = {
    tema: "oscuro",
    idioma: "es"
};

// Usando operador OR para valores por defecto
const tema = configuracion.tema || "claro";           // "oscuro"
const fuente = configuracion.fuente || "Arial";       // "Arial" (por defecto)

// Usando nullish coalescing (ES2020)
const notificaciones = configuracion.notificaciones ?? true; // true (por defecto)

// Función helper para obtener valores
function obtenerConfiguracion(config, clave, porDefecto) {
    return config[clave] !== undefined ? config[clave] : porDefecto;
}

const tamaño = obtenerConfiguracion(configuracion, "tamaño", "mediano");
```

## Objetos Anidados

### Estructuras complejas

```javascript
const empresa = {
    nombre: "Innovate Corp",
    fundacion: 2015,
    empleados: 150,
    
    // Objeto anidado
    direccion: {
        calle: "Av. Innovación 456",
        ciudad: "Madrid",
        pais: "España",
        coordenadas: {
            latitud: 40.4168,
            longitud: -3.7038
        }
    },
    
    // Array de objetos
    departamentos: [
        {
            nombre: "Desarrollo",
            empleados: 80,
            presupuesto: 2000000
        },
        {
            nombre: "Marketing", 
            empleados: 30,
            presupuesto: 800000
        },
        {
            nombre: "Recursos Humanos",
            empleados: 15,
            presupuesto: 400000
        }
    ],
    
    // Función como propiedad (método)
    obtenerInfo: function() {
        return `${this.nombre} - Fundada en ${this.fundacion}`;
    }
};

// Acceso a datos anidados
console.log(empresa.nombre);                        // "Innovate Corp"
console.log(empresa.direccion.ciudad);              // "Madrid"
console.log(empresa.direccion.coordenadas.latitud); // 40.4168
console.log(empresa.departamentos[0].nombre);       // "Desarrollo"
console.log(empresa.departamentos[1].presupuesto);  // 800000
console.log(empresa.obtenerInfo());                 // "Innovate Corp - Fundada en 2015"
```

### Navegación segura en objetos anidados

```javascript
const usuario = {
    nombre: "María",
    perfil: {
        avatar: "avatar.jpg",
        configuracion: {
            tema: "oscuro"
        }
    }
};

// Acceso normal (puede causar errores)
// console.log(usuario.perfil.redes.twitter); // Error si 'redes' no existe

// Verificación manual
if (usuario.perfil && usuario.perfil.redes && usuario.perfil.redes.twitter) {
    console.log(usuario.perfil.redes.twitter);
}

// Optional chaining (ES2020) - Recomendado
console.log(usuario.perfil?.redes?.twitter);          // undefined (sin error)
console.log(usuario.perfil?.configuracion?.tema);     // "oscuro"
console.log(usuario.perfil?.configuracion?.idioma);   // undefined

// Con valores por defecto
const twitter = usuario.perfil?.redes?.twitter ?? "No especificado";
const idioma = usuario.perfil?.configuracion?.idioma ?? "es";
```

## Funciones como Propiedades (Métodos)

### Definición de métodos

```javascript
const calculadora = {
    // Método tradicional
    sumar: function(a, b) {
        return a + b;
    },
    
    // Método con sintaxis de ES6 (shorthand)
    restar(a, b) {
        return a - b;
    },
    
    // Función flecha como propiedad (cuidado con 'this')
    multiplicar: (a, b) => {
        return a * b;
    },
    
    // Método con acceso a otras propiedades
    historial: [],
    
    dividir: function(a, b) {
        if (b === 0) {
            return "Error: División por cero";
        }
        const resultado = a / b;
        this.historial.push(`${a} ÷ ${b} = ${resultado}`);
        return resultado;
    },
    
    // Método para obtener el historial
    obtenerHistorial: function() {
        return this.historial;
    }
};

// Uso de métodos
console.log(calculadora.sumar(5, 3));      // 8
console.log(calculadora.restar(10, 4));    // 6
console.log(calculadora.multiplicar(3, 7)); // 21
console.log(calculadora.dividir(15, 3));   // 5
console.log(calculadora.obtenerHistorial()); // ["15 ÷ 3 = 5"]
```

### El contexto 'this' en métodos

```javascript
const persona = {
    nombre: "Laura",
    apellido: "Martín",
    edad: 32,
    
    // Método que usa 'this'
    presentarse: function() {
        return `Hola, soy ${this.nombre} ${this.apellido} y tengo ${this.edad} años`;
    },
    
    // Método que modifica propiedades
    cumplirAños: function() {
        this.edad++;
        return `¡Feliz cumpleaños! Ahora tienes ${this.edad} años`;
    },
    
    // Función flecha NO tiene su propio 'this'
    saludarFlecha: () => {
        // 'this' aquí se refiere al contexto exterior (window/global)
        return `Hola desde función flecha`; // No puede usar this.nombre
    }
};

console.log(persona.presentarse());  // "Hola, soy Laura Martín y tengo 32 años"
console.log(persona.cumplirAños());  // "¡Feliz cumpleaños! Ahora tienes 33 años"
console.log(persona.edad);           // 33 (se modificó)
console.log(persona.saludarFlecha()); // "Hola desde función flecha"
```

## Ejemplos Prácticos de Uso

### 1. Representar entidades del mundo real

```javascript
// Usuario de una aplicación
const usuario = {
    id: 1001,
    nombreUsuario: "dev_maria",
    email: "maria@example.com",
    nombre: "María",
    apellido: "González",
    fechaRegistro: "2024-01-15",
    activo: true,
    rol: "administrador",
    
    // Información adicional
    configuracion: {
        tema: "oscuro",
        idioma: "es",
        notificaciones: true
    },
    
    // Estadísticas
    estadisticas: {
        sesionesIniciadas: 156,
        ultimoAcceso: "2024-10-17T10:30:00Z",
        proyectosCreados: 23
    }
};

// Producto de un e-commerce
const producto = {
    id: "PROD-001",
    nombre: "Laptop Gaming Pro",
    descripcion: "Laptop de alto rendimiento para gaming",
    precio: 1299.99,
    moneda: "EUR",
    disponible: true,
    stock: 15,
    categoria: "tecnologia",
    
    // Detalles técnicos
    especificaciones: {
        procesador: "Intel i7-12700H",
        memoria: "32GB DDR5",
        almacenamiento: "1TB SSD NVMe",
        grafica: "RTX 4070",
        pantalla: "15.6\" 144Hz"
    },
    
    // Información de envío
    envio: {
        peso: 2.5,
        dimensiones: {
            largo: 35.6,
            ancho: 25.4,
            alto: 2.3
        },
        gratuito: true
    }
};
```

### 2. Configuración de aplicaciones

```javascript
const configApp = {
    // Configuración de la aplicación
    nombre: "Mi Aplicación",
    version: "2.1.4",
    entorno: "desarrollo",
    
    // Base de datos
    baseDatos: {
        host: "localhost",
        puerto: 5432,
        nombre: "mi_app_db",
        usuario: "dev_user"
    },
    
    // API
    api: {
        url: "https://api.miapp.com",
        version: "v1",
        timeout: 5000,
        reintentos: 3
    },
    
    // Características habilitadas
    caracteristicas: {
        autenticacion: true,
        notificacionesPush: false,
        modoOffline: true,
        analytics: true
    },
    
    // Límites y configuraciones
    limites: {
        tamañoArchivo: 10485760, // 10MB en bytes
        sesionExpira: 3600,      // 1 hora en segundos
        intentosLogin: 5
    }
};

// Uso de la configuración
console.log(`Aplicación: ${configApp.nombre} v${configApp.version}`);
console.log(`Entorno: ${configApp.entorno}`);
console.log(`URL API: ${configApp.api.url}/${configApp.api.version}`);
console.log(`Autenticación habilitada: ${configApp.caracteristicas.autenticacion}`);
```

### 3. Estado de componentes

```javascript
const componenteModal = {
    // Estado
    abierto: false,
    cargando: false,
    error: null,
    
    // Configuración
    configuracion: {
        cerrarAlClickFuera: true,
        mostrarBotonCerrar: true,
        animacion: true,
        tamaño: "mediano"
    },
    
    // Contenido
    contenido: {
        titulo: "",
        cuerpo: "",
        botones: []
    },
    
    // Métodos para manejar el estado
    abrir: function(titulo, cuerpo) {
        this.abierto = true;
        this.contenido.titulo = titulo;
        this.contenido.cuerpo = cuerpo;
        this.error = null;
    },
    
    cerrar: function() {
        this.abierto = false;
        this.cargando = false;
        this.error = null;
    },
    
    mostrarError: function(mensajeError) {
        this.error = mensajeError;
        this.cargando = false;
    },
    
    iniciarCarga: function() {
        this.cargando = true;
        this.error = null;
    }
};

// Uso del componente
componenteModal.abrir("Confirmar acción", "¿Estás seguro de que quieres continuar?");
console.log(componenteModal.abierto);  // true
console.log(componenteModal.contenido.titulo); // "Confirmar acción"
```

### 4. Datos de formularios

```javascript
const formularioRegistro = {
    // Campos del formulario
    campos: {
        nombre: "",
        email: "",
        contraseña: "",
        confirmarContraseña: "",
        fechaNacimiento: "",
        terminos: false
    },
    
    // Estado de validación
    validacion: {
        nombre: { valido: false, mensaje: "" },
        email: { valido: false, mensaje: "" },
        contraseña: { valido: false, mensaje: "" },
        confirmarContraseña: { valido: false, mensaje: "" },
        fechaNacimiento: { valido: false, mensaje: "" },
        terminos: { valido: false, mensaje: "" }
    },
    
    // Estado del formulario
    estado: {
        enviado: false,
        enviando: false,
        valido: false,
        tocado: false
    },
    
    // Método para validar un campo
    validarCampo: function(campo, valor) {
        this.campos[campo] = valor;
        this.estado.tocado = true;
        
        // Ejemplo de validación simple
        if (campo === "email") {
            const esValido = valor.includes("@");
            this.validacion[campo] = {
                valido: esValido,
                mensaje: esValido ? "" : "Email no válido"
            };
        }
        
        // Verificar si todo el formulario es válido
        this.estado.valido = Object.values(this.validacion)
            .every(validacion => validacion.valido);
    }
};

// Uso del formulario
formularioRegistro.validarCampo("email", "usuario@example.com");
console.log(formularioRegistro.validacion.email.valido); // true
console.log(formularioRegistro.estado.valido);          // false (otros campos no validados)
```

## Verificación de Propiedades

### Comprobar existencia de propiedades

```javascript
const libro = {
    titulo: "JavaScript: The Good Parts",
    autor: "Douglas Crockford",
    año: 2008,
    paginas: 176
};

// Método 1: Operador 'in'
console.log("titulo" in libro);        // true
console.log("editorial" in libro);     // false

// Método 2: hasOwnProperty (solo propiedades propias, no heredadas)
console.log(libro.hasOwnProperty("titulo"));     // true
console.log(libro.hasOwnProperty("editorial"));  // false

// Método 3: Comparación con undefined
console.log(libro.titulo !== undefined);    // true
console.log(libro.editorial !== undefined); // false

// Método 4: Object.hasOwn() (ES2022)
console.log(Object.hasOwn(libro, "titulo"));     // true
console.log(Object.hasOwn(libro, "editorial"));  // false

// Cuidado con valores falsy
const configuracion = {
    debug: false,
    limite: 0,
    mensaje: ""
};

// Estas verificaciones pueden dar falsos positivos
console.log(!!configuracion.debug);    // false (pero la propiedad existe)
console.log(!!configuracion.limite);   // false (pero la propiedad existe)

// Mejor verificación
console.log("debug" in configuracion);           // true
console.log(configuracion.debug !== undefined);  // true
```

### Obtener listas de propiedades

```javascript
const persona = {
    nombre: "Ana",
    edad: 28,
    trabajo: "Diseñadora"
};

// Obtener todas las claves (propiedades)
const claves = Object.keys(persona);
console.log(claves); // ["nombre", "edad", "trabajo"]

// Obtener todos los valores
const valores = Object.values(persona);
console.log(valores); // ["Ana", 28, "Diseñadora"]

// Obtener pares clave-valor
const entradas = Object.entries(persona);
console.log(entradas); 
// [["nombre", "Ana"], ["edad", 28], ["trabajo", "Diseñadora"]]

// Iterar sobre las propiedades
Object.keys(persona).forEach(clave => {
    console.log(`${clave}: ${persona[clave]}`);
});

// Usando for...in
for (const clave in persona) {
    console.log(`${clave}: ${persona[clave]}`);
}
```

## Casos de Uso Comunes

### Agrupar datos relacionados

```javascript
// En lugar de variables separadas
let nombreUsuario = "Carlos";
let edadUsuario = 30;
let emailUsuario = "carlos@example.com";

// Mejor: agrupar en un objeto
const usuario = {
    nombre: "Carlos",
    edad: 30,
    email: "carlos@example.com"
};
```

### Pasar múltiples parámetros a funciones

```javascript
// En lugar de muchos parámetros
function crearCuenta(nombre, email, contraseña, fechaNacimiento, telefono, direccion) {
    // ... código
}

// Mejor: usar un objeto
function crearCuenta(datosUsuario) {
    const { nombre, email, contraseña, fechaNacimiento, telefono, direccion } = datosUsuario;
    // ... código
}

// Uso más claro
crearCuenta({
    nombre: "María",
    email: "maria@example.com",
    contraseña: "secreto123",
    fechaNacimiento: "1990-05-15",
    telefono: "+34123456789",
    direccion: "Calle Principal 123"
});
```

### Representar entidades complejas

```javascript
const pedido = {
    id: "PED-2024-001",
    fecha: "2024-10-17",
    estado: "pendiente",
    
    cliente: {
        id: 456,
        nombre: "Laura Sánchez",
        email: "laura@example.com"
    },
    
    productos: [
        {
            id: "PROD-001",
            nombre: "Camiseta",
            precio: 29.99,
            cantidad: 2
        },
        {
            id: "PROD-045", 
            nombre: "Pantalón",
            precio: 59.99,
            cantidad: 1
        }
    ],
    
    calcularTotal: function() {
        return this.productos.reduce((total, producto) => {
            return total + (producto.precio * producto.cantidad);
        }, 0);
    }
};

console.log(`Total del pedido: ${pedido.calcularTotal()}€`); // Total del pedido: 119.97€
```

***

## Resumen de Conceptos Básicos

### ¿Qué son los objetos?
- **Colecciones de propiedades** en formato clave-valor
- **Estructura de datos fundamental** en JavaScript
- **Flexibles y dinámicos** - pueden cambiar durante la ejecución

### Sintaxis básica:
- **Notación literal**: `{ clave: valor }`
- **Acceso por punto**: `objeto.propiedad`
- **Acceso por corchetes**: `objeto["propiedad"]`

### Tipos de valores:
- **Cualquier tipo**: strings, numbers, booleans, arrays, funciones, otros objetos
- **Anidación**: objetos dentro de objetos
- **Métodos**: funciones como propiedades

### Casos de uso principales:
- **Agrupar datos relacionados**
- **Representar entidades** (usuarios, productos, etc.)
- **Configuraciones** de aplicaciones
- **Estado** de componentes
- **Parámetros** de funciones

## Modificación de Objetos

Los objetos en JavaScript son **mutables**, lo que significa que puedes modificar sus propiedades después de su creación. Esto incluye añadir nuevas propiedades, cambiar valores existentes y eliminar propiedades.

### Añadir nuevas propiedades

```javascript
const persona = {
    nombre: "Ana",
    edad: 25
};

// Añadir propiedades con notación de punto
persona.email = "ana@example.com";
persona.activo = true;

// Añadir propiedades con notación de corchetes
persona["telefono"] = "+34123456789";
persona["fecha_registro"] = "2024-10-17";

console.log(persona);
// {
//   nombre: "Ana",
//   edad: 25,
//   email: "ana@example.com",
//   activo: true,
//   telefono: "+34123456789",
//   fecha_registro: "2024-10-17"
// }
```

### Modificar propiedades existentes

```javascript
const producto = {
    nombre: "Laptop",
    precio: 999.99,
    disponible: true
};

// Cambiar valores existentes
producto.precio = 899.99;          // Nuevo precio
producto.nombre = "Laptop Pro";    // Nuevo nombre
producto.disponible = false;       // Cambiar estado

// Usando notación de corchetes
producto["precio"] = 799.99;

console.log(producto);
// {
//   nombre: "Laptop Pro",
//   precio: 799.99,
//   disponible: false
// }
```

### Eliminar propiedades

```javascript
const usuario = {
    id: 123,
    nombre: "Carlos",
    email: "carlos@example.com",
    temporal: "dato temporal",
    debug: true
};

// Eliminar propiedades con delete
delete usuario.temporal;
delete usuario.debug;

// También funciona con notación de corchetes
delete usuario["email"];

console.log(usuario);
// {
//   id: 123,
//   nombre: "Carlos"
// }

// Verificar que se eliminó
console.log("temporal" in usuario);  // false
console.log(usuario.temporal);       // undefined
```

### Modificación de objetos anidados

```javascript
const empresa = {
    nombre: "TechCorp",
    direccion: {
        calle: "Av. Principal 123",
        ciudad: "Madrid"
    },
    empleados: ["Ana", "Luis", "María"]
};

// Modificar propiedades anidadas
empresa.direccion.calle = "Calle Nueva 456";
empresa.direccion.codigoPostal = "28001";

// Añadir a arrays dentro del objeto
empresa.empleados.push("Pedro");

// Modificar el array completo
empresa.empleados = [...empresa.empleados, "Sandra"];

// Añadir nuevos objetos anidados
empresa.contacto = {
    telefono: "+34987654321",
    email: "info@techcorp.com"
};

console.log(empresa);
```

### Modificación dinámica de propiedades

```javascript
const configuracion = {
    tema: "claro",
    idioma: "es"
};

// Propiedades dinámicas
const nuevaPropiedad = "notificaciones";
const valor = true;

configuracion[nuevaPropiedad] = valor;

// Con template literals
const prefijo = "user";
configuracion[`${prefijo}_id`] = 12345;
configuracion[`${prefijo}_activo`] = true;

// Usando variables para nombres de propiedades
const propiedades = ["limite", "timeout", "reintentos"];
const valores = [100, 5000, 3];

propiedades.forEach((prop, index) => {
    configuracion[prop] = valores[index];
});

console.log(configuracion);
// {
//   tema: "claro",
//   idioma: "es", 
//   notificaciones: true,
//   user_id: 12345,
//   user_activo: true,
//   limite: 100,
//   timeout: 5000,
//   reintentos: 3
// }
```

### Copia vs Referencia

**Importante**: Los objetos se asignan por referencia, no por valor.

```javascript
const original = {
    nombre: "Juan",
    edad: 30
};

// Asignación por referencia
const referencia = original;
referencia.edad = 31;

console.log(original.edad);   // 31 (¡se modificó!)
console.log(referencia.edad); // 31

// Para hacer una copia superficial
const copia = { ...original };        // Spread operator
// o
const copia2 = Object.assign({}, original);

copia.edad = 32;
console.log(original.edad);   // 31 (no se modificó)
console.log(copia.edad);      // 32

// Copia profunda para objetos anidados
const complejo = {
    usuario: {
        nombre: "Ana",
        configuracion: {
            tema: "oscuro"
        }
    }
};

const copiaSuperficial = { ...complejo };
copiaSuperficial.usuario.nombre = "María"; // ¡Modifica el original!

// Para copia profunda (objetos simples)
const copiaProfunda = JSON.parse(JSON.stringify(complejo));
copiaProfunda.usuario.nombre = "Laura"; // No modifica el original
```

### Métodos para modificar objetos

#### Object.assign()

```javascript
const destino = {
    nombre: "Juan",
    edad: 30
};

const fuente1 = {
    email: "juan@example.com",
    edad: 31  // Sobrescribirá la edad
};

const fuente2 = {
    activo: true,
    rol: "usuario"
};

// Copiar propiedades al objeto destino
Object.assign(destino, fuente1, fuente2);

console.log(destino);
// {
//   nombre: "Juan",
//   edad: 31,
//   email: "juan@example.com", 
//   activo: true,
//   rol: "usuario"
// }

// Crear un nuevo objeto combinando varios
const nuevo = Object.assign({}, destino, { premium: true });
```

#### Spread operator (...) - ES2018

```javascript
const base = {
    id: 1,
    nombre: "Producto"
};

const actualizacion = {
    precio: 99.99,
    disponible: true
};

// Crear nuevo objeto combinando
const resultado = {
    ...base,
    ...actualizacion,
    categoria: "tecnologia"  // Añadir propiedades adicionales
};

console.log(resultado);
// {
//   id: 1,
//   nombre: "Producto",
//   precio: 99.99,
//   disponible: true,
//   categoria: "tecnologia"
// }

// Sobrescribir propiedades específicas
const actualizado = {
    ...base,
    nombre: "Producto Premium",  // Sobrescribe
    ...actualizacion
};
```

### Funciones para modificar objetos

```javascript
// Función para actualizar propiedades
function actualizarObjeto(objeto, actualizaciones) {
    return {
        ...objeto,
        ...actualizaciones
    };
}

// Función para añadir propiedad si no existe
function añadirSiNoExiste(objeto, clave, valor) {
    if (!(clave in objeto)) {
        objeto[clave] = valor;
    }
    return objeto;
}

// Función para eliminar múltiples propiedades
function eliminarPropiedades(objeto, ...claves) {
    const resultado = { ...objeto };
    claves.forEach(clave => delete resultado[clave]);
    return resultado;
}

// Función para limpiar propiedades undefined/null
function limpiarObjeto(objeto) {
    const resultado = {};
    Object.keys(objeto).forEach(clave => {
        if (objeto[clave] != null) {
            resultado[clave] = objeto[clave];
        }
    });
    return resultado;
}

// Uso de las funciones
const usuario = {
    id: 123,
    nombre: "Ana",
    email: null,
    temporal: undefined,
    activo: true
};

const usuarioActualizado = actualizarObjeto(usuario, {
    email: "ana@example.com",
    ultimoAcceso: new Date()
});

const usuarioLimpio = limpiarObjeto(usuario);
console.log(usuarioLimpio); // { id: 123, nombre: "Ana", activo: true }
```

### Inmutabilidad vs Mutabilidad

#### Enfoque mutable (modifica el original)

```javascript
const configuracion = {
    tema: "claro",
    notificaciones: true
};

// Modifica el objeto original
function cambiarTema(config, nuevoTema) {
    config.tema = nuevoTema;
    return config;
}

cambiarTema(configuracion, "oscuro");
console.log(configuracion.tema); // "oscuro" (se modificó)
```

#### Enfoque inmutable (no modifica el original)

```javascript
const configuracion = {
    tema: "claro", 
    notificaciones: true
};

// No modifica el original, devuelve uno nuevo
function cambiarTema(config, nuevoTema) {
    return {
        ...config,
        tema: nuevoTema
    };
}

const nuevaConfig = cambiarTema(configuracion, "oscuro");
console.log(configuracion.tema);   // "claro" (no se modificó)
console.log(nuevaConfig.tema);     // "oscuro"
```

### Casos prácticos de modificación

#### 1. Actualizar estado de componente

```javascript
const estado = {
    usuario: null,
    cargando: false,
    error: null,
    configuracion: {
        tema: "claro",
        idioma: "es"
    }
};

// Función para actualizar estado inmutablemente
function actualizarEstado(estadoActual, cambios) {
    return {
        ...estadoActual,
        ...cambios
    };
}

// Función para actualizar configuración anidada
function actualizarConfiguracion(estadoActual, nuevaConfig) {
    return {
        ...estadoActual,
        configuracion: {
            ...estadoActual.configuracion,
            ...nuevaConfig
        }
    };
}

// Uso
const estadoConUsuario = actualizarEstado(estado, {
    usuario: { id: 123, nombre: "Ana" },
    cargando: false
});

const estadoConTema = actualizarConfiguracion(estadoConUsuario, {
    tema: "oscuro"
});
```

#### 2. Manejo de formularios

```javascript
const formulario = {
    campos: {
        nombre: "",
        email: "",
        edad: ""
    },
    errores: {},
    valido: false
};

// Función para actualizar campo
function actualizarCampo(form, campo, valor) {
    return {
        ...form,
        campos: {
            ...form.campos,
            [campo]: valor
        }
    };
}

// Función para añadir error
function añadirError(form, campo, mensaje) {
    return {
        ...form,
        errores: {
            ...form.errores,
            [campo]: mensaje
        }
    };
}

// Función para limpiar errores
function limpiarErrores(form) {
    return {
        ...form,
        errores: {}
    };
}

// Uso
let formActual = formulario;
formActual = actualizarCampo(formActual, "nombre", "Juan");
formActual = actualizarCampo(formActual, "email", "juan@example.com");
formActual = añadirError(formActual, "edad", "Edad es requerida");
```

#### 3. Cache y configuración dinámica

```javascript
const cache = {};

// Añadir al cache
function agregarAlCache(clave, valor, expiracion = null) {
    cache[clave] = {
        valor,
        timestamp: Date.now(),
        expiracion
    };
}

// Obtener del cache
function obtenerDelCache(clave) {
    const entrada = cache[clave];
    if (!entrada) return null;
    
    if (entrada.expiracion && Date.now() > entrada.expiracion) {
        delete cache[clave];
        return null;
    }
    
    return entrada.valor;
}

// Limpiar cache expirado
function limpiarCacheExpirado() {
    const ahora = Date.now();
    Object.keys(cache).forEach(clave => {
        const entrada = cache[clave];
        if (entrada.expiracion && ahora > entrada.expiracion) {
            delete cache[clave];
        }
    });
}

// Uso
agregarAlCache("usuario_123", { nombre: "Ana" }, Date.now() + 300000); // 5 min
agregarAlCache("configuracion", { tema: "oscuro" });

console.log(obtenerDelCache("usuario_123")); // { nombre: "Ana" }
```

***

Los objetos son la base de JavaScript y entender estos conceptos básicos es fundamental antes de avanzar a temas como destructuring y métodos avanzados.
