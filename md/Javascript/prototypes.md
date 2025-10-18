# Prototypes en JavaScript

Los **prototypes** son uno de los conceptos más fundamentales y únicos de JavaScript. Entender cómo funcionan es esencial para dominar la programación orientada a objetos en JavaScript y comprender cómo funciona la herencia en el lenguaje.

## ¿Qué es un Prototype?

Un **prototype** es un objeto del cual otros objetos pueden heredar propiedades y métodos. En JavaScript, cada objeto tiene una referencia interna a otro objeto llamado su "prototype". Cuando intentas acceder a una propiedad de un objeto, JavaScript primero busca en el objeto mismo, y si no la encuentra, busca en su prototype.

```javascript
// Cada función en JavaScript tiene una propiedad 'prototype'
function Persona(nombre) {
    this.nombre = nombre;
}

// El prototype de Persona es un objeto donde podemos agregar métodos
console.log(typeof Persona.prototype); // "object"
console.log(Persona.prototype); // { constructor: [Function: Persona] }

// Agregar método al prototype
Persona.prototype.saludar = function() {
    return `Hola, soy ${this.nombre}`;
};

// Crear instancias
const persona1 = new Persona("Ana");
const persona2 = new Persona("Luis");

// Ambas instancias pueden usar el método del prototype
console.log(persona1.saludar()); // "Hola, soy Ana"
console.log(persona2.saludar()); // "Hola, soy Luis"

// El método no está en las instancias, está en el prototype
console.log(persona1.hasOwnProperty('saludar')); // false
console.log(Persona.prototype.hasOwnProperty('saludar')); // true
```

## La Cadena de Prototipos (Prototype Chain)

JavaScript utiliza una **cadena de prototipos** para la herencia. Cada objeto tiene un enlace interno a su prototype, y ese prototype puede tener su propio prototype, formando una cadena.

```javascript
function Animal(nombre) {
    this.nombre = nombre;
}

Animal.prototype.respirar = function() {
    return `${this.nombre} está respirando`;
};

function Perro(nombre, raza) {
    Animal.call(this, nombre); // Llamar al constructor padre
    this.raza = raza;
}

// Establecer la herencia: Perro hereda de Animal
Perro.prototype = Object.create(Animal.prototype);
Perro.prototype.constructor = Perro;

// Agregar método específico de Perro
Perro.prototype.ladrar = function() {
    return `${this.nombre} dice: ¡Guau!`;
};

const miPerro = new Perro("Rex", "Labrador");

// Busca en esta orden: miPerro → Perro.prototype → Animal.prototype → Object.prototype
console.log(miPerro.ladrar());   // "Rex dice: ¡Guau!" (encontrado en Perro.prototype)
console.log(miPerro.respirar()); // "Rex está respirando" (encontrado en Animal.prototype)
console.log(miPerro.toString()); // "[object Object]" (encontrado en Object.prototype)

// Verificar la cadena de prototipos
console.log(miPerro.__proto__ === Perro.prototype); // true
console.log(Perro.prototype.__proto__ === Animal.prototype); // true
console.log(Animal.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__); // null (fin de la cadena)
```

## Diferencias: Propiedades en la Instancia vs Prototype

### Propiedades en la instancia

```javascript
function Vehiculo(marca, modelo) {
    // Propiedades de instancia - cada objeto tiene su propia copia
    this.marca = marca;
    this.modelo = modelo;
    this.velocidad = 0;
    
    // Método en la instancia - cada objeto tiene su propia copia (NO recomendado)
    this.acelerar = function() {
        this.velocidad += 10;
        return `Acelerando... ${this.velocidad} km/h`;
    };
}

const auto1 = new Vehiculo("Toyota", "Corolla");
const auto2 = new Vehiculo("Honda", "Civic");

// Cada instancia tiene sus propias propiedades
console.log(auto1.marca); // "Toyota"
console.log(auto2.marca); // "Honda"

// Cada instancia tiene su propia función (ineficiente)
console.log(auto1.acelerar === auto2.acelerar); // false (diferentes funciones)
```

### Propiedades en el prototype

```javascript
function VehiculoMejorado(marca, modelo) {
    // Solo propiedades específicas de cada instancia
    this.marca = marca;
    this.modelo = modelo;
    this.velocidad = 0;
}

// Métodos compartidos en el prototype (eficiente)
VehiculoMejorado.prototype.acelerar = function() {
    this.velocidad += 10;
    return `${this.marca} ${this.modelo} acelerando... ${this.velocidad} km/h`;
};

VehiculoMejorado.prototype.frenar = function() {
    this.velocidad = Math.max(0, this.velocidad - 15);
    return `${this.marca} ${this.modelo} frenando... ${this.velocidad} km/h`;
};

VehiculoMejorado.prototype.obtenerInfo = function() {
    return `${this.marca} ${this.modelo} - Velocidad: ${this.velocidad} km/h`;
};

const auto3 = new VehiculoMejorado("Ford", "Focus");
const auto4 = new VehiculoMejorado("BMW", "X3");

// Las funciones son compartidas (eficiente)
console.log(auto3.acelerar === auto4.acelerar); // true (misma función)

// Pero 'this' se refiere a cada instancia correctamente
console.log(auto3.acelerar()); // "Ford Focus acelerando... 10 km/h"
console.log(auto4.acelerar()); // "BMW X3 acelerando... 10 km/h"
```

## Métodos de Prototype

### Verificar propiedades y prototypes

```javascript
function Producto(nombre, precio) {
    this.nombre = nombre;
    this.precio = precio;
}

Producto.prototype.categoria = "General";
Producto.prototype.mostrarInfo = function() {
    return `${this.nombre} - $${this.precio}`;
};

const laptop = new Producto("Laptop", 999);

// hasOwnProperty() - verifica propiedades propias (no del prototype)
console.log(laptop.hasOwnProperty('nombre'));     // true (propiedad propia)
console.log(laptop.hasOwnProperty('categoria'));  // false (está en prototype)
console.log(laptop.hasOwnProperty('mostrarInfo')); // false (está en prototype)

// in operator - verifica si existe en objeto o prototype
console.log('nombre' in laptop);     // true
console.log('categoria' in laptop);  // true
console.log('mostrarInfo' in laptop); // true

// isPrototypeOf() - verifica si un objeto es prototype de otro
console.log(Producto.prototype.isPrototypeOf(laptop)); // true
console.log(Object.prototype.isPrototypeOf(laptop)); // true

// instanceof - verifica si un objeto es instancia de un constructor
console.log(laptop instanceof Producto); // true
console.log(laptop instanceof Object);   // true

// getPrototypeOf() - obtiene el prototype de un objeto
console.log(Object.getPrototypeOf(laptop) === Producto.prototype); // true
```

### Object.create() - Crear objetos con prototype específico

```javascript
// Crear objeto base que servirá como prototype
const vehiculoBase = {
    tipo: "Vehículo",
    moverse: function() {
        return `${this.nombre} se está moviendo`;
    },
    detenerse: function() {
        return `${this.nombre} se ha detenido`;
    }
};

// Crear objetos que heredan de vehiculoBase
const bicicleta = Object.create(vehiculoBase);
bicicleta.nombre = "Bicicleta";
bicicleta.pedales = 2;

const coche = Object.create(vehiculoBase);
coche.nombre = "Coche";
coche.ruedas = 4;
coche.motor = "V6";

// Ambos heredan métodos de vehiculoBase
console.log(bicicleta.moverse());  // "Bicicleta se está moviendo"
console.log(coche.detenerse());    // "Coche se ha detenido"

// Verificar herencia
console.log(vehiculoBase.isPrototypeOf(bicicleta)); // true
console.log(vehiculoBase.isPrototypeOf(coche)); // true

// Object.create() con propiedades definidas
const moto = Object.create(vehiculoBase, {
    nombre: {
        value: "Motocicleta",
        writable: true,
        enumerable: true,
        configurable: true
    },
    cilindrada: {
        value: 600,
        writable: false,  // No se puede modificar
        enumerable: true,
        configurable: false
    }
});

console.log(moto.nombre);      // "Motocicleta"
console.log(moto.cilindrada);  // 600
// moto.cilindrada = 800;      // No tiene efecto (writable: false)
console.log(moto.cilindrada);  // 600
```

## Modificar Prototypes Existentes

### Extender prototypes nativos (usar con precaución)

```javascript
// Agregar método a Array.prototype
Array.prototype.ultimo = function() {
    return this[this.length - 1];
};

Array.prototype.primero = function() {
    return this[0];
};

// Agregar método a String.prototype
String.prototype.capitalizar = function() {
    return this.charAt(0).toUpperCase() + this.slice(1).toLowerCase();
};

// Usar los nuevos métodos
const numeros = [1, 2, 3, 4, 5];
console.log(numeros.primero()); // 1
console.log(numeros.ultimo());  // 5

const texto = "hola mundo";
console.log(texto.capitalizar()); // "Hola mundo"

// ⚠️ ADVERTENCIA: Modificar prototypes nativos puede causar problemas
// - Conflictos con otras librerías
// - Problemas de compatibilidad
// - Efectos secundarios inesperados

// Es mejor crear funciones utilitarias
const ArrayUtils = {
    ultimo: (arr) => arr[arr.length - 1],
    primero: (arr) => arr[0]
};

console.log(ArrayUtils.ultimo(numeros)); // 5 (más seguro)
```

### Polyfills - Implementar métodos faltantes

```javascript
// Polyfill para Array.includes() (si no existe)
if (!Array.prototype.includes) {
    Array.prototype.includes = function(searchElement, fromIndex) {
        'use strict';
        
        if (this == null) {
            throw new TypeError('Array.prototype.includes called on null or undefined');
        }
        
        const obj = Object(this);
        const len = parseInt(obj.length) || 0;
        
        if (len === 0) {
            return false;
        }
        
        let n = parseInt(fromIndex) || 0;
        let k;
        
        if (n >= 0) {
            k = n;
        } else {
            k = len + n;
            if (k < 0) k = 0;
        }
        
        while (k < len) {
            if (obj[k] === searchElement) {
                return true;
            }
            k++;
        }
        
        return false;
    };
}

// Polyfill para String.startsWith() (si no existe)
if (!String.prototype.startsWith) {
    String.prototype.startsWith = function(searchString, position) {
        position = position || 0;
        return this.substr(position, searchString.length) === searchString;
    };
}
```

## Herencia con Prototypes

### Herencia clásica con prototypes

```javascript
// Clase padre
function Animal(nombre, especie) {
    this.nombre = nombre;
    this.especie = especie;
    this.vivo = true;
}

Animal.prototype.dormir = function() {
    return `${this.nombre} está durmiendo`;
};

Animal.prototype.comer = function(alimento) {
    return `${this.nombre} está comiendo ${alimento}`;
};

Animal.prototype.mostrarInfo = function() {
    return `${this.nombre} es un ${this.especie}`;
};

// Clase hija - Mamifero
function Mamifero(nombre, especie, tipoPelaje) {
    // Llamar al constructor padre
    Animal.call(this, nombre, especie);
    this.tipoPelaje = tipoPelaje;
    this.temperatura = "caliente";
}

// Establecer herencia
Mamifero.prototype = Object.create(Animal.prototype);
Mamifero.prototype.constructor = Mamifero;

// Agregar métodos específicos de Mamifero
Mamifero.prototype.amamantar = function() {
    return `${this.nombre} está amamantando a sus crías`;
};

// Sobrescribir método del padre
Mamifero.prototype.mostrarInfo = function() {
    // Llamar al método del padre y extenderlo
    const infoBase = Animal.prototype.mostrarInfo.call(this);
    return `${infoBase} con pelaje ${this.tipoPelaje}`;
};

// Clase nieta - Perro
function Perro(nombre, raza, tipoPelaje) {
    // Llamar al constructor padre
    Mamifero.call(this, nombre, "Canis lupus", tipoPelaje);
    this.raza = raza;
}

// Establecer herencia
Perro.prototype = Object.create(Mamifero.prototype);
Perro.prototype.constructor = Perro;

// Métodos específicos de Perro
Perro.prototype.ladrar = function() {
    return `${this.nombre} dice: ¡Guau guau!`;
};

Perro.prototype.jugar = function(juguete) {
    return `${this.nombre} está jugando con ${juguete}`;
};

// Sobrescribir método
Perro.prototype.mostrarInfo = function() {
    const infoMamifero = Mamifero.prototype.mostrarInfo.call(this);
    return `${infoMamifero}, raza ${this.raza}`;
};

// Usar la herencia
const miPerro = new Perro("Rex", "Labrador", "corto");

console.log(miPerro.mostrarInfo()); // "Rex es un Canis lupus con pelaje corto, raza Labrador"
console.log(miPerro.dormir());      // "Rex está durmiendo" (de Animal)
console.log(miPerro.amamantar());   // "Rex está amamantando a sus crías" (de Mamifero)
console.log(miPerro.ladrar());      // "Rex dice: ¡Guau guau!" (de Perro)

// Verificar instanceof
console.log(miPerro instanceof Perro);    // true
console.log(miPerro instanceof Mamifero); // true
console.log(miPerro instanceof Animal);   // true
console.log(miPerro instanceof Object);   // true
```

### Mixin Pattern con Prototypes

```javascript
// Mixins - comportamientos que se pueden "mezclar" en otros objetos
const Volador = {
    volar: function() {
        return `${this.nombre} está volando`;
    },
    aterrizar: function() {
        return `${this.nombre} ha aterrizado`;
    }
};

const Nadador = {
    nadar: function() {
        return `${this.nombre} está nadando`;
    },
    bucear: function() {
        return `${this.nombre} está buceando`;
    }
};

const Corredor = {
    correr: function() {
        return `${this.nombre} está corriendo`;
    },
    saltar: function() {
        return `${this.nombre} está saltando`;
    }
};

// Función para mezclar comportamientos
function mezclar(constructorDestino, ...mixins) {
    mixins.forEach(mixin => {
        Object.keys(mixin).forEach(prop => {
            constructorDestino.prototype[prop] = mixin[prop];
        });
    });
}

// Clase base
function Ave(nombre, especie) {
    Animal.call(this, nombre, especie);
}

Ave.prototype = Object.create(Animal.prototype);
Ave.prototype.constructor = Ave;

// Mezclar comportamiento de Volador
mezclar(Ave, Volador);

// Clase Pato que hereda de Ave
function Pato(nombre) {
    Ave.call(this, nombre, "Pato");
}

Pato.prototype = Object.create(Ave.prototype);
Pato.prototype.constructor = Pato;

// Los patos también nadan
mezclar(Pato, Nadador);

// Crear instancias
const aguila = new Ave("Águila", "Águila real");
const pato = new Pato("Donald");

console.log(aguila.volar());     // "Águila está volando"
console.log(pato.volar());       // "Donald está volando"
console.log(pato.nadar());       // "Donald está nadando"
// console.log(aguila.nadar());  // Error: no tiene este método
```

## Prototype vs Class (ES2015+)

### Sintaxis tradicional con Prototype

```javascript
// Usando prototypes (ES5)
function CuentaBancaria(titular, saldoInicial) {
    this.numero = Math.random().toString(36).substr(2, 10);
    this.titular = titular;
    this.saldo = saldoInicial || 0;
    this.movimientos = [];
    this.fechaApertura = new Date();
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
        saldo: this.saldo
    });
    
    return this.saldo;
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
        saldo: this.saldo
    });
    
    return this.saldo;
};

CuentaBancaria.prototype.obtenerBalance = function() {
    return {
        titular: this.titular,
        numero: this.numero,
        saldo: this.saldo,
        movimientos: this.movimientos.length
    };
};
```

### Sintaxis moderna con Class (ES2015+)

```javascript
// Usando class (ES6+) - syntactic sugar sobre prototypes
class CuentaBancariaModerna {
    constructor(titular, saldoInicial = 0) {
        this.numero = Math.random().toString(36).substr(2, 10);
        this.titular = titular;
        this.saldo = saldoInicial;
        this.movimientos = [];
        this.fechaApertura = new Date();
    }
    
    depositar(cantidad) {
        if (cantidad <= 0) {
            throw new Error('Cantidad debe ser positiva');
        }
        
        this.saldo += cantidad;
        this.movimientos.push({
            tipo: 'depósito',
            cantidad: cantidad,
            fecha: new Date(),
            saldo: this.saldo
        });
        
        return this.saldo;
    }
    
    retirar(cantidad) {
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
            saldo: this.saldo
        });
        
        return this.saldo;
    }
    
    obtenerBalance() {
        return {
            titular: this.titular,
            numero: this.numero,
            saldo: this.saldo,
            movimientos: this.movimientos.length
        };
    }
}

// Ambas implementaciones son funcionalmente equivalentes
const cuenta1 = new CuentaBancaria("Juan", 1000);
const cuenta2 = new CuentaBancariaModerna("María", 1000);

// Los métodos están en el prototype en ambos casos
console.log(cuenta1.depositar === CuentaBancaria.prototype.depositar); // true
console.log(cuenta2.depositar === CuentaBancariaModerna.prototype.depositar); // true
```

## Casos Prácticos Avanzados

### Sistema de Plugins con Prototypes

```javascript
// Sistema base extensible
function Editor(contenido = "") {
    this.contenido = contenido;
    this.historial = [contenido];
    this.posicionHistorial = 0;
}

Editor.prototype.escribir = function(texto) {
    this.contenido += texto;
    this._guardarEnHistorial();
    return this;
};

Editor.prototype.borrar = function(cantidad = 1) {
    this.contenido = this.contenido.slice(0, -cantidad);
    this._guardarEnHistorial();
    return this;
};

Editor.prototype.obtenerContenido = function() {
    return this.contenido;
};

Editor.prototype._guardarEnHistorial = function() {
    this.historial = this.historial.slice(0, this.posicionHistorial + 1);
    this.historial.push(this.contenido);
    this.posicionHistorial++;
};

// Plugin de Deshacer/Rehacer
const UndoRedoPlugin = {
    deshacer: function() {
        if (this.posicionHistorial > 0) {
            this.posicionHistorial--;
            this.contenido = this.historial[this.posicionHistorial];
        }
        return this;
    },
    
    rehacer: function() {
        if (this.posicionHistorial < this.historial.length - 1) {
            this.posicionHistorial++;
            this.contenido = this.historial[this.posicionHistorial];
        }
        return this;
    }
};

// Plugin de Formato
const FormatPlugin = {
    enNegritas: function(texto) {
        return this.escribir(`**${texto}**`);
    },
    
    enCursiva: function(texto) {
        return this.escribir(`*${texto}*`);
    },
    
    crearEnlace: function(texto, url) {
        return this.escribir(`[${texto}](${url})`);
    }
};

// Plugin de Estadísticas
const StatsPlugin = {
    contarPalabras: function() {
        return this.contenido.split(/\s+/).filter(palabra => palabra.length > 0).length;
    },
    
    contarCaracteres: function() {
        return this.contenido.length;
    },
    
    obtenerEstadisticas: function() {
        return {
            caracteres: this.contarCaracteres(),
            palabras: this.contarPalabras(),
            lineas: this.contenido.split('\n').length
        };
    }
};

// Función para instalar plugins
function instalarPlugin(Constructor, plugin) {
    Object.keys(plugin).forEach(metodo => {
        Constructor.prototype[metodo] = plugin[metodo];
    });
}

// Instalar plugins
instalarPlugin(Editor, UndoRedoPlugin);
instalarPlugin(Editor, FormatPlugin);
instalarPlugin(Editor, StatsPlugin);

// Usar el editor con plugins
const editor = new Editor("Hola mundo\n");

editor
    .escribir("Este es un ")
    .enNegritas("texto en negritas")
    .escribir(" y esto es ")
    .enCursiva("en cursiva")
    .escribir(".\n")
    .crearEnlace("Google", "https://google.com");

console.log(editor.obtenerContenido());
console.log(editor.obtenerEstadisticas());

// Probar deshacer
editor.deshacer().deshacer();
console.log("Después de deshacer:", editor.obtenerContenido());

// Rehacer
editor.rehacer();
console.log("Después de rehacer:", editor.obtenerContenido());
```

### Cache Inteligente con Prototype

```javascript
function CacheInteligente(maxSize = 100, ttl = 300000) { // 5 minutos por defecto
    this.cache = new Map();
    this.accesos = new Map();
    this.maxSize = maxSize;
    this.ttl = ttl;
    this.stats = {
        hits: 0,
        misses: 0,
        evictions: 0
    };
}

CacheInteligente.prototype.set = function(clave, valor, ttlPersonalizado) {
    const tiempoExpiracion = Date.now() + (ttlPersonalizado || this.ttl);
    
    // Si el cache está lleno, hacer espacio
    if (this.cache.size >= this.maxSize && !this.cache.has(clave)) {
        this._evictLeastRecentlyUsed();
    }
    
    this.cache.set(clave, {
        valor: valor,
        expiracion: tiempoExpiracion,
        fechaCreacion: Date.now()
    });
    
    this.accesos.set(clave, Date.now());
    return this;
};

CacheInteligente.prototype.get = function(clave) {
    if (!this.cache.has(clave)) {
        this.stats.misses++;
        return null;
    }
    
    const entrada = this.cache.get(clave);
    
    // Verificar expiración
    if (Date.now() > entrada.expiracion) {
        this.delete(clave);
        this.stats.misses++;
        return null;
    }
    
    // Actualizar último acceso
    this.accesos.set(clave, Date.now());
    this.stats.hits++;
    
    return entrada.valor;
};

CacheInteligente.prototype.delete = function(clave) {
    this.cache.delete(clave);
    this.accesos.delete(clave);
    return this;
};

CacheInteligente.prototype._evictLeastRecentlyUsed = function() {
    let claveAntigua = null;
    let tiempoAntiguo = Date.now();
    
    for (const [clave, tiempoAcceso] of this.accesos.entries()) {
        if (tiempoAcceso < tiempoAntiguo) {
            tiempoAntiguo = tiempoAcceso;
            claveAntigua = clave;
        }
    }
    
    if (claveAntigua) {
        this.delete(claveAntigua);
        this.stats.evictions++;
    }
};

CacheInteligente.prototype.clear = function() {
    this.cache.clear();
    this.accesos.clear();
    return this;
};

CacheInteligente.prototype.size = function() {
    return this.cache.size;
};

CacheInteligente.prototype.obtenerEstadisticas = function() {
    const totalAccesos = this.stats.hits + this.stats.misses;
    return {
        ...this.stats,
        hitRate: totalAccesos > 0 ? (this.stats.hits / totalAccesos * 100).toFixed(2) + '%' : '0%',
        size: this.cache.size,
        maxSize: this.maxSize
    };
};

CacheInteligente.prototype.limpiarExpirados = function() {
    const ahora = Date.now();
    let eliminados = 0;
    
    for (const [clave, entrada] of this.cache.entries()) {
        if (ahora > entrada.expiracion) {
            this.delete(clave);
            eliminados++;
        }
    }
    
    return eliminados;
};

// Uso del cache inteligente
const cache = new CacheInteligente(5, 10000); // máximo 5 elementos, 10 segundos TTL

// Agregar datos
cache.set('usuario:1', { nombre: 'Ana', edad: 25 });
cache.set('usuario:2', { nombre: 'Luis', edad: 30 });
cache.set('producto:1', { nombre: 'Laptop', precio: 999 });

// Obtener datos
console.log(cache.get('usuario:1')); // { nombre: 'Ana', edad: 25 }
console.log(cache.get('inexistente')); // null

// Ver estadísticas
console.log(cache.obtenerEstadisticas());

// Probar evicción por tamaño
cache.set('item:1', 'valor1');
cache.set('item:2', 'valor2');
cache.set('item:3', 'valor3'); // Debería evictar el menos usado

console.log('Después de llenar el cache:', cache.obtenerEstadisticas());
```

## Mejores Prácticas con Prototypes

### ✅ Buenas prácticas

```javascript
// 1. Usar prototypes para métodos compartidos
function MiClase(valor) {
    this.valor = valor; // Propiedades específicas en la instancia
}

MiClase.prototype.metodoCompartido = function() {
    return this.valor * 2; // Métodos en el prototype
};

// 2. Verificar tipo antes de extender prototypes nativos
if (typeof Array.prototype.includes !== 'function') {
    Array.prototype.includes = function(elemento) {
        return this.indexOf(elemento) !== -1;
    };
}

// 3. Usar Object.create() para herencia limpia
function Padre() {
    this.tipo = 'padre';
}

function Hijo() {
    Padre.call(this);
    this.subtipo = 'hijo';
}

Hijo.prototype = Object.create(Padre.prototype);
Hijo.prototype.constructor = Hijo;

// 4. Preservar el constructor al modificar prototypes
function MiConstructor() {}
MiConstructor.prototype = {};
MiConstructor.prototype.constructor = MiConstructor; // ¡Importante!
```

### ❌ Prácticas a evitar

```javascript
// 1. NO modificar Object.prototype (afecta todos los objetos)
// Object.prototype.miMetodo = function() {}; // ¡NO HACER!

// 2. NO usar asignación directa para herencia
function Padre() {}
function Hijo() {}
// Hijo.prototype = Padre.prototype; // ¡NO! Ambos comparten el mismo objeto

// 3. NO poner métodos en el constructor (ineficiente)
function Ineficiente() {
    this.metodo = function() { // Nueva función en cada instancia
        return "ineficiente";
    };
}

// 4. NO asumir que hasOwnProperty existe
const obj = Object.create(null); // Sin prototype
// obj.hasOwnProperty('prop'); // Error!
// Usar: Object.prototype.hasOwnProperty.call(obj, 'prop');
```

***

Los prototypes son la base de la herencia en JavaScript y entender su funcionamiento te permite escribir código más eficiente y crear arquitecturas de objetos más elegantes. Aunque las clases de ES2015+ proporcionan una sintaxis más familiar, internamente siguen usando el sistema de prototypes de JavaScript.