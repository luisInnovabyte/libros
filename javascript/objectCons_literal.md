# Object Constructor vs Object Literal en JavaScript

En JavaScript existen dos formas principales de crear objetos: usando **Object Literal** (notación literal) y **Object Constructor** (función constructora). Cada método tiene sus características, ventajas y casos de uso específicos.

> **📚 Nota importante**: Para una explicación completa y detallada sobre **Prototypes** en JavaScript, consulta el archivo dedicado [`prototypes.md`](./prototypes.md) que cubre en profundidad la cadena de prototipos, herencia, y patrones avanzados.

## ¿Qué son los Objetos en JavaScript?

Los objetos son estructuras de datos que permiten almacenar múltiples valores relacionados mediante pares **clave-valor**. Son la base de la programación orientada a objetos en JavaScript.

```javascript
// Ejemplo básico de objeto
const persona = {
    nombre: "Ana",
    edad: 25,
    ciudad: "Madrid"
};
```

## 1. Object Literal (Notación Literal)

La **notación literal** es la forma más común y directa de crear objetos en JavaScript. Se define usando llaves `{}` y especificando las propiedades directamente.

### Sintaxis básica

```javascript
// Objeto literal básico
const objetoLiteral = {
    propiedad1: "valor1",
    propiedad2: "valor2",
    propiedad3: 123
};

// Ejemplo real
const usuario = {
    id: 1,
    nombre: "Carlos",
    email: "carlos@email.com",
    activo: true,
    fechaRegistro: new Date()
};

console.log(usuario.nombre); // "Carlos"
console.log(usuario.activo); // true
```

### Características del Object Literal

```javascript
// 1. Creación directa e inmediata
const producto = {
    id: 101,
    nombre: "Laptop",
    precio: 999.99,
    categoria: "Tecnología",
    
    // Métodos dentro del objeto literal
    mostrarInfo: function() {
        return `${this.nombre} - $${this.precio}`;
    },
    
    // Método con arrow function (no recomendado para métodos)
    calcularDescuento: (porcentaje) => {
        // ⚠️ 'this' no funciona correctamente aquí
        return this.precio * (porcentaje / 100);
    },
    
    // Método con sintaxis ES6 (recomendado)
    aplicarDescuento(porcentaje) {
        return this.precio * (1 - porcentaje / 100);
    }
};

console.log(producto.mostrarInfo()); // "Laptop - $999.99"
console.log(producto.aplicarDescuento(10)); // 899.991
```

### Object Literal con propiedades computadas

```javascript
// Propiedades dinámicas
const campo = "nombre";
const valor = "María";
const timestamp = Date.now();

const objeto = {
    [campo]: valor,                    // nombre: "María"
    [`${campo}_completo`]: "María García", // nombre_completo: "María García"
    [timestamp]: "creado ahora",       // 1697648400000: "creado ahora"
    [`dato_${Math.random()}`]: "aleatorio" // dato_0.123: "aleatorio"
};

console.log(objeto);
```

### Object Literal con ES6+ features

```javascript
// Shorthand properties (propiedades abreviadas)
const nombre = "Luis";
const edad = 30;
const activo = true;

// Forma tradicional
const persona1 = {
    nombre: nombre,
    edad: edad,
    activo: activo
};

// Forma abreviada ES6
const persona2 = {
    nombre,  // equivale a nombre: nombre
    edad,    // equivale a edad: edad
    activo   // equivale a activo: activo
};

// Métodos abreviados ES6
const calculadora = {
    // Forma tradicional
    sumar: function(a, b) {
        return a + b;
    },
    
    // Forma abreviada ES6
    restar(a, b) {
        return a - b;
    },
    
    multiplicar(a, b) {
        return a * b;
    }
};

console.log(calculadora.sumar(5, 3));      // 8
console.log(calculadora.restar(10, 4));    // 6
console.log(calculadora.multiplicar(3, 7)); // 21
```

### Destructuring con Object Literal

```javascript
const configuracion = {
    tema: "oscuro",
    idioma: "español",
    notificaciones: true,
    usuario: {
        nombre: "Ana",
        rol: "admin"
    }
};

// Destructuring básico
const { tema, idioma } = configuracion;
console.log(tema, idioma); // "oscuro" "español"

// Destructuring con renombrado
const { tema: temaSeleccionado, idioma: idiomaActual } = configuracion;

// Destructuring anidado
const { usuario: { nombre, rol } } = configuracion;
console.log(nombre, rol); // "Ana" "admin"

// Destructuring con valores por defecto
const { sonido = true, animaciones = false } = configuracion;
```

## 2. Object Constructor (Función Constructora)

Una **función constructora** es una función especial que se usa para crear múltiples objetos con la misma estructura y comportamiento. Se invoca con la palabra clave `new`.

### Sintaxis básica

```javascript
// Definir función constructora (convención: PascalCase)
function Persona(nombre, edad, ciudad) {
    // 'this' se refiere al nuevo objeto que se está creando
    this.nombre = nombre;
    this.edad = edad;
    this.ciudad = ciudad;
    
    // Método dentro del constructor
    this.saludar = function() {
        return `Hola, soy ${this.nombre} y tengo ${this.edad} años`;
    };
    
    this.cumplirAños = function() {
        this.edad++;
        return `Ahora tengo ${this.edad} años`;
    };
}

// Crear objetos usando el constructor
const persona1 = new Persona("Carlos", 28, "Madrid");
const persona2 = new Persona("Ana", 32, "Barcelona");
const persona3 = new Persona("Luis", 25, "Valencia");

console.log(persona1.saludar()); // "Hola, soy Carlos y tengo 28 años"
console.log(persona2.saludar()); // "Hola, soy Ana y tengo 32 años"

// Cada objeto es independiente
persona1.cumplirAños();
console.log(persona1.edad); // 29
console.log(persona2.edad); // 32 (no cambió)
```

### Constructor con validaciones

```javascript
function Usuario(nombre, email, edad) {
    // Validaciones
    if (!nombre || typeof nombre !== 'string') {
        throw new Error('Nombre es requerido y debe ser string');
    }
    
    if (!email || !email.includes('@')) {
        throw new Error('Email válido es requerido');
    }
    
    if (edad < 0 || edad > 120) {
        throw new Error('Edad debe estar entre 0 y 120');
    }
    
    // Propiedades
    this.nombre = nombre;
    this.email = email.toLowerCase();
    this.edad = edad;
    this.fechaCreacion = new Date();
    this.activo = true;
    
    // Métodos
    this.desactivar = function() {
        this.activo = false;
        return `Usuario ${this.nombre} desactivado`;
    };
    
    this.cambiarEmail = function(nuevoEmail) {
        if (!nuevoEmail || !nuevoEmail.includes('@')) {
            throw new Error('Email válido requerido');
        }
        this.email = nuevoEmail.toLowerCase();
        return `Email actualizado a ${this.email}`;
    };
}

// Uso con manejo de errores
try {
    const usuario1 = new Usuario("María", "maria@email.com", 27);
    const usuario2 = new Usuario("Juan", "juan@email.com", 35);
    
    console.log(usuario1);
    console.log(usuario2.cambiarEmail("juan.nuevo@email.com"));
    
    // Esto generará un error
    const usuarioInvalido = new Usuario("", "email-inválido", -5);
} catch (error) {
    console.log("Error:", error.message);
}
```

### Prototype en constructores

```javascript
function Producto(nombre, precio, categoria) {
    this.nombre = nombre;
    this.precio = precio;
    this.categoria = categoria;
    this.fechaCreacion = new Date();
}

// Agregar métodos al prototype (más eficiente)
Producto.prototype.mostrarInfo = function() {
    return `${this.nombre} - $${this.precio} (${this.categoria})`;
};

Producto.prototype.aplicarDescuento = function(porcentaje) {
    const descuento = this.precio * (porcentaje / 100);
    this.precio -= descuento;
    return `Descuento aplicado. Nuevo precio: $${this.precio.toFixed(2)}`;
};

Producto.prototype.esBarato = function() {
    return this.precio < 50;
};

// Crear productos
const producto1 = new Producto("Mouse", 25.99, "Tecnología");
const producto2 = new Producto("Teclado", 89.99, "Tecnología");

console.log(producto1.mostrarInfo()); // "Mouse - $25.99 (Tecnología)"
console.log(producto1.esBarato());    // true

console.log(producto2.aplicarDescuento(15)); // "Descuento aplicado. Nuevo precio: $76.49"

// Todos los objetos comparten los métodos del prototype
console.log(producto1.mostrarInfo === producto2.mostrarInfo); // true
```

## 3. Comparación: Object Literal vs Constructor

### Tabla comparativa

| Aspecto | Object Literal | Object Constructor |
|---------|---------------|-------------------|
| **Sintaxis** | `const obj = {}` | `function Obj() {}; const obj = new Obj()` |
| **Uso** | Objetos únicos | Múltiples objetos similares |
| **Eficiencia** | Directo y rápido | Más memoria por métodos duplicados |
| **Reutilización** | No reutilizable | Altamente reutilizable |
| **Prototype** | Object.prototype | Prototype personalizable |
| **Validación** | Manual | Integrada en constructor |
| **Flexibilidad** | Alta para casos simples | Alta para casos complejos |

### Cuándo usar Object Literal

```javascript
// ✅ Configuración única
const config = {
    apiUrl: "https://api.ejemplo.com",
    timeout: 5000,
    retries: 3,
    headers: {
        "Content-Type": "application/json"
    }
};

// ✅ Objeto de una sola instancia
const singleton = {
    nombre: "MiSingleton",
    valor: 42,
    metodo() {
        return this.valor * 2;
    }
};

// ✅ Datos específicos sin reutilización
const respuestaAPI = {
    status: 200,
    message: "OK",
    data: {
        usuarios: [...],
        total: 150
    },
    timestamp: new Date()
};

// ✅ Namespace para funciones relacionadas
const utilidades = {
    formatearFecha(fecha) {
        return fecha.toLocaleDateString();
    },
    
    validarEmail(email) {
        return email.includes('@');
    },
    
    generarId() {
        return Math.random().toString(36).substr(2, 9);
    }
};
```

### Cuándo usar Object Constructor

```javascript
// ✅ Múltiples objetos similares
function Tarea(titulo, descripcion, prioridad) {
    this.id = Math.random().toString(36).substr(2, 9);
    this.titulo = titulo;
    this.descripcion = descripcion;
    this.prioridad = prioridad || 'media';
    this.completada = false;
    this.fechaCreacion = new Date();
}

Tarea.prototype.completar = function() {
    this.completada = true;
    this.fechaCompletada = new Date();
};

Tarea.prototype.cambiarPrioridad = function(nuevaPrioridad) {
    this.prioridad = nuevaPrioridad;
};

// Crear múltiples tareas
const tarea1 = new Tarea("Estudiar JavaScript", "Repasar objetos y constructores", "alta");
const tarea2 = new Tarea("Hacer ejercicio", "Correr 30 minutos", "media");
const tarea3 = new Tarea("Comprar comida", "Lista de supermercado", "baja");

// ✅ Cuando necesitas validación y lógica compleja
function CuentaBancaria(titular, saldoInicial) {
    if (!titular || typeof titular !== 'string') {
        throw new Error('Titular requerido');
    }
    
    if (saldoInicial < 0) {
        throw new Error('Saldo inicial no puede ser negativo');
    }
    
    this.numero = Math.random().toString().substr(2, 10);
    this.titular = titular;
    this.saldo = saldoInicial || 0;
    this.movimientos = [];
    this.fechaApertura = new Date();
    this.activa = true;
}

CuentaBancaria.prototype.depositar = function(cantidad) {
    if (cantidad <= 0) {
        throw new Error('Cantidad debe ser positiva');
    }
    
    this.saldo += cantidad;
    this.movimientos.push({
        tipo: 'depósito',
        cantidad: cantidad,
        fecha: new Date(),
        saldoResultante: this.saldo
    });
    
    return `Depósito realizado. Nuevo saldo: $${this.saldo}`;
};

CuentaBancaria.prototype.retirar = function(cantidad) {
    if (cantidad <= 0) {
        throw new Error('Cantidad debe ser positiva');
    }
    
    if (cantidad > this.saldo) {
        throw new Error('Fondos insuficientes');
    }
    
    this.saldo -= cantidad;
    this.movimientos.push({
        tipo: 'retiro',
        cantidad: cantidad,
        fecha: new Date(),
        saldoResultante: this.saldo
    });
    
    return `Retiro realizado. Nuevo saldo: $${this.saldo}`;
};
```

## 4. Patrones Híbridos y Avanzados

### Factory Pattern (Alternativa a constructores)

```javascript
// Factory function que devuelve object literals
function crearPersona(nombre, edad, profesion) {
    // Validaciones
    if (!nombre) throw new Error('Nombre requerido');
    if (edad < 0) throw new Error('Edad inválida');
    
    // Retornar object literal
    return {
        nombre: nombre,
        edad: edad,
        profesion: profesion || 'No especificada',
        fechaCreacion: new Date(),
        
        // Métodos
        saludar() {
            return `Hola, soy ${this.nombre}`;
        },
        
        cumplirAños() {
            this.edad++;
            return `Ahora tengo ${this.edad} años`;
        },
        
        cambiarProfesion(nuevaProfesion) {
            this.profesion = nuevaProfesion;
            return `Ahora soy ${this.profesion}`;
        }
    };
}

// Uso similar a constructor pero sin 'new'
const persona1 = crearPersona("Ana", 25, "Desarrolladora");
const persona2 = crearPersona("Luis", 30, "Diseñador");

console.log(persona1.saludar());
console.log(persona2.cambiarProfesion("Gerente"));
```

### Module Pattern con IIFE

```javascript
// Módulo que combina object literal con función constructora
const ModuloUsuarios = (function() {
    // Variables privadas
    const usuarios = [];
    let contadorId = 1;
    
    // Constructor privado
    function Usuario(nombre, email) {
        this.id = contadorId++;
        this.nombre = nombre;
        this.email = email;
        this.fechaRegistro = new Date();
        this.activo = true;
    }
    
    Usuario.prototype.desactivar = function() {
        this.activo = false;
    };
    
    // API pública (object literal)
    return {
        crear(nombre, email) {
            if (!nombre || !email) {
                throw new Error('Nombre y email requeridos');
            }
            
            const usuario = new Usuario(nombre, email);
            usuarios.push(usuario);
            return usuario;
        },
        
        obtenerTodos() {
            return [...usuarios]; // Copia para evitar mutación
        },
        
        obtenerPorId(id) {
            return usuarios.find(usuario => usuario.id === id);
        },
        
        obtenerActivos() {
            return usuarios.filter(usuario => usuario.activo);
        },
        
        contar() {
            return usuarios.length;
        },
        
        desactivarTodos() {
            usuarios.forEach(usuario => usuario.desactivar());
            return 'Todos los usuarios desactivados';
        }
    };
})();

// Uso del módulo
const usuario1 = ModuloUsuarios.crear("Carlos", "carlos@email.com");
const usuario2 = ModuloUsuarios.crear("María", "maria@email.com");

console.log(ModuloUsuarios.obtenerTodos());
console.log(ModuloUsuarios.contar()); // 2

usuario1.desactivar();
console.log(ModuloUsuarios.obtenerActivos()); // Solo María
```

### Object.create() - Creación con prototype específico

```javascript
// Objeto base que servirá como prototype
const vehiculoBase = {
    acelerar() {
        this.velocidad += 10;
        return `Acelerando... Velocidad: ${this.velocidad} km/h`;
    },
    
    frenar() {
        this.velocidad = Math.max(0, this.velocidad - 15);
        return `Frenando... Velocidad: ${this.velocidad} km/h`;
    },
    
    obtenerInfo() {
        return `${this.marca} ${this.modelo} - ${this.velocidad} km/h`;
    }
};

// Crear objetos con prototype específico
const coche = Object.create(vehiculoBase);
coche.marca = "Toyota";
coche.modelo = "Corolla";
coche.velocidad = 0;
coche.puertas = 4;

const moto = Object.create(vehiculoBase);
moto.marca = "Honda";
moto.modelo = "CBR600";
moto.velocidad = 0;
moto.cilindrada = 600;

// Ambos heredan métodos de vehiculoBase
console.log(coche.acelerar()); // "Acelerando... Velocidad: 10 km/h"
console.log(moto.acelerar());  // "Acelerando... Velocidad: 10 km/h"
console.log(coche.obtenerInfo()); // "Toyota Corolla - 10 km/h"

// Factory function con Object.create
function crearVehiculo(marca, modelo, tipo) {
    const vehiculo = Object.create(vehiculoBase);
    vehiculo.marca = marca;
    vehiculo.modelo = modelo;
    vehiculo.tipo = tipo;
    vehiculo.velocidad = 0;
    vehiculo.fechaFabricacion = new Date();
    
    return vehiculo;
}

const auto = crearVehiculo("Ford", "Focus", "Automóvil");
const camion = crearVehiculo("Volvo", "FH16", "Camión");
```

## 5. Casos Prácticos Reales

### Sistema de Productos con Object Constructor

```javascript
function Producto(datos) {
    // Validación de datos requeridos
    if (!datos || typeof datos !== 'object') {
        throw new Error('Datos del producto requeridos');
    }
    
    const { nombre, precio, categoria, stock = 0 } = datos;
    
    if (!nombre || !precio || !categoria) {
        throw new Error('Nombre, precio y categoría son requeridos');
    }
    
    if (precio < 0) {
        throw new Error('Precio no puede ser negativo');
    }
    
    // Propiedades
    this.id = `prod_${Date.now()}_${Math.random().toString(36).substr(2, 5)}`;
    this.nombre = nombre;
    this.precio = precio;
    this.categoria = categoria;
    this.stock = stock;
    this.fechaCreacion = new Date();
    this.activo = true;
    this.historialPrecios = [{ precio: precio, fecha: new Date() }];
}

// Métodos en el prototype
Producto.prototype.cambiarPrecio = function(nuevoPrecio) {
    if (nuevoPrecio < 0) {
        throw new Error('Precio no puede ser negativo');
    }
    
    this.historialPrecios.push({
        precio: this.precio,
        fecha: new Date()
    });
    
    this.precio = nuevoPrecio;
    return `Precio actualizado a $${this.precio}`;
};

Producto.prototype.agregarStock = function(cantidad) {
    if (cantidad < 0) {
        throw new Error('Cantidad no puede ser negativa');
    }
    
    this.stock += cantidad;
    return `Stock actualizado: ${this.stock} unidades`;
};

Producto.prototype.vender = function(cantidad = 1) {
    if (cantidad > this.stock) {
        throw new Error('Stock insuficiente');
    }
    
    this.stock -= cantidad;
    return {
        vendido: cantidad,
        stockRestante: this.stock,
        total: this.precio * cantidad
    };
};

Producto.prototype.desactivar = function() {
    this.activo = false;
    return 'Producto desactivado';
};

Producto.prototype.obtenerResumen = function() {
    return {
        id: this.id,
        nombre: this.nombre,
        precio: this.precio,
        categoria: this.categoria,
        stock: this.stock,
        activo: this.activo,
        diasDesdeCreacion: Math.floor((Date.now() - this.fechaCreacion) / (1000 * 60 * 60 * 24))
    };
};

// Uso del sistema
const producto1 = new Producto({
    nombre: "Laptop Gaming",
    precio: 1299.99,
    categoria: "Tecnología",
    stock: 5
});

const producto2 = new Producto({
    nombre: "Mouse Inalámbrico",
    precio: 29.99,
    categoria: "Tecnología",
    stock: 25
});

// Operaciones
console.log(producto1.agregarStock(3)); // "Stock actualizado: 8 unidades"
console.log(producto1.vender(2)); // { vendido: 2, stockRestante: 6, total: 2599.98 }
console.log(producto1.cambiarPrecio(1199.99)); // "Precio actualizado a $1199.99"
console.log(producto1.obtenerResumen());
```

### Configuración Global con Object Literal

```javascript
// Sistema de configuración global usando object literal
const AppConfig = {
    // Configuración base
    app: {
        nombre: "Mi Aplicación",
        version: "1.2.0",
        entorno: "desarrollo",
        debug: true
    },
    
    // APIs
    api: {
        baseUrl: "https://api.miapp.com",
        timeout: 5000,
        retries: 3,
        endpoints: {
            usuarios: "/usuarios",
            productos: "/productos",
            pedidos: "/pedidos"
        }
    },
    
    // UI
    ui: {
        tema: "claro",
        idioma: "es",
        animaciones: true,
        notificaciones: {
            posicion: "top-right",
            duracion: 3000,
            mostrarIconos: true
        }
    },
    
    // Métodos de configuración
    obtener(ruta) {
        const partes = ruta.split('.');
        let resultado = this;
        
        for (const parte of partes) {
            resultado = resultado[parte];
            if (resultado === undefined) {
                return null;
            }
        }
        
        return resultado;
    },
    
    establecer(ruta, valor) {
        const partes = ruta.split('.');
        const ultimaParte = partes.pop();
        let objetivo = this;
        
        for (const parte of partes) {
            if (!(parte in objetivo)) {
                objetivo[parte] = {};
            }
            objetivo = objetivo[parte];
        }
        
        objetivo[ultimaParte] = valor;
        return `Configuración ${ruta} actualizada`;
    },
    
    esProduccion() {
        return this.app.entorno === 'produccion';
    },
    
    obtenerUrlCompleta(endpoint) {
        const baseUrl = this.api.baseUrl;
        const ruta = this.api.endpoints[endpoint];
        
        if (!ruta) {
            throw new Error(`Endpoint ${endpoint} no encontrado`);
        }
        
        return `${baseUrl}${ruta}`;
    },
    
    exportarConfiguracion() {
        return JSON.stringify(this, (key, value) => {
            // Excluir métodos del JSON
            return typeof value === 'function' ? undefined : value;
        }, 2);
    }
};

// Uso del sistema de configuración
console.log(AppConfig.obtener('app.nombre')); // "Mi Aplicación"
console.log(AppConfig.obtener('ui.notificaciones.duracion')); // 3000

AppConfig.establecer('ui.tema', 'oscuro');
AppConfig.establecer('api.timeout', 8000);

console.log(AppConfig.obtenerUrlCompleta('usuarios')); // "https://api.miapp.com/usuarios"
console.log(AppConfig.esProduccion()); // false

// Exportar configuración (útil para debug)
console.log(AppConfig.exportarConfiguracion());
```

## Resumen de Mejores Prácticas

### Object Literal ✅
- **Usar para**: Configuraciones, objetos únicos, namespaces
- **Ventajas**: Sintaxis simple, creación directa, flexible
- **ES6+ Features**: Shorthand properties, métodos abreviados, propiedades computadas

### Object Constructor ✅  
- **Usar para**: Múltiples instancias similares, validación compleja, encapsulación
- **Ventajas**: Reutilización, prototype compartido, validación integrada
- **Convención**: PascalCase para nombres de constructores

### Patrones Híbridos ✅
- **Factory Functions**: Flexibilidad sin `new`
- **Module Pattern**: Encapsulación con IIFE
- **Object.create()**: Control específico del prototype

***

Dominar tanto Object Literal como Object Constructor te permite elegir la herramienta correcta según el contexto y crear código JavaScript más efectivo y mantenible.
