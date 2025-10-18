# Clases en JavaScript (ES2015+)

Las **clases** en JavaScript, introducidas en ES2015 (ES6), proporcionan una sintaxis más limpia y familiar para crear objetos y manejar herencia. Aunque internamente siguen usando el sistema de prototypes de JavaScript, ofrecen una forma más intuitiva de trabajar con programación orientada a objetos.

> **📚 Referencia**: Para entender cómo funcionan las clases internamente, consulta [`prototypes.md`](./prototypes.md) que explica el sistema de prototypes subyacente.

## ¿Qué son las Clases?

Una **clase** es una plantilla para crear objetos que comparten la misma estructura y comportamiento. Es una forma moderna de escribir funciones constructoras con una sintaxis más clara y características adicionales.

```javascript
// Sintaxis tradicional con función constructora
function PersonaTradicional(nombre, edad) {
    this.nombre = nombre;
    this.edad = edad;
}

PersonaTradicional.prototype.saludar = function() {
    return `Hola, soy ${this.nombre}`;
};

// Sintaxis moderna con clase (equivalente)
class Persona {
    constructor(nombre, edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    
    saludar() {
        return `Hola, soy ${this.nombre}`;
    }
}

// Uso idéntico
const persona1 = new PersonaTradicional("Ana", 25);
const persona2 = new Persona("Luis", 30);

console.log(persona1.saludar()); // "Hola, soy Ana"
console.log(persona2.saludar()); // "Hola, soy Luis"
```

## Sintaxis Básica de Clases

### Declaración de clase básica

```javascript
class Usuario {
    // Constructor - se ejecuta al crear una nueva instancia
    constructor(nombre, email, edad = 18) {
        // Validaciones
        if (!nombre || !email) {
            throw new Error('Nombre y email son requeridos');
        }
        
        // Propiedades de instancia
        this.id = Math.random().toString(36).substr(2, 9);
        this.nombre = nombre;
        this.email = email;
        this.edad = edad;
        this.fechaRegistro = new Date();
        this.activo = true;
    }
    
    // Métodos de instancia
    obtenerInfo() {
        return `${this.nombre} (${this.email}) - ${this.edad} años`;
    }
    
    cumplirAños() {
        this.edad++;
        return `${this.nombre} ahora tiene ${this.edad} años`;
    }
    
    desactivar() {
        this.activo = false;
        return `Usuario ${this.nombre} desactivado`;
    }
    
    cambiarEmail(nuevoEmail) {
        if (!nuevoEmail || !nuevoEmail.includes('@')) {
            throw new Error('Email válido requerido');
        }
        
        const emailAnterior = this.email;
        this.email = nuevoEmail;
        return `Email cambiado de ${emailAnterior} a ${nuevoEmail}`;
    }
}

// Crear instancias
const usuario1 = new Usuario("María", "maria@email.com", 28);
const usuario2 = new Usuario("Carlos", "carlos@email.com"); // edad por defecto: 18

console.log(usuario1.obtenerInfo()); // "María (maria@email.com) - 28 años"
console.log(usuario2.cumplirAños()); // "Carlos ahora tiene 19 años"
console.log(usuario1.cambiarEmail("maria.nueva@email.com"));
```

### Métodos getter y setter

```javascript
class Producto {
    constructor(nombre, precioBase) {
        this.nombre = nombre;
        this._precioBase = precioBase; // Convención: _ para propiedades "privadas"
        this._descuento = 0;
        this._iva = 0.21; // 21% IVA por defecto
    }
    
    // Getter - se accede como propiedad
    get precio() {
        const precioConDescuento = this._precioBase * (1 - this._descuento);
        return precioConDescuento * (1 + this._iva);
    }
    
    // Getter para precio sin IVA
    get precioSinIVA() {
        return this._precioBase * (1 - this._descuento);
    }
    
    // Setter - se asigna como propiedad
    set descuento(valor) {
        if (valor < 0 || valor > 1) {
            throw new Error('Descuento debe estar entre 0 y 1');
        }
        this._descuento = valor;
    }
    
    // Getter para descuento
    get descuento() {
        return this._descuento;
    }
    
    // Setter para IVA
    set iva(valor) {
        if (valor < 0) {
            throw new Error('IVA no puede ser negativo');
        }
        this._iva = valor;
    }
    
    get iva() {
        return this._iva;
    }
    
    // Método para mostrar información completa
    mostrarDetalles() {
        return {
            nombre: this.nombre,
            precioBase: this._precioBase,
            descuento: `${(this._descuento * 100)}%`,
            iva: `${(this._iva * 100)}%`,
            precioSinIVA: this.precioSinIVA.toFixed(2),
            precioFinal: this.precio.toFixed(2)
        };
    }
}

// Uso de getters y setters
const laptop = new Producto("Laptop Gaming", 1000);

console.log(laptop.precio); // 1210 (1000 + 21% IVA)

// Usar setter (se asigna como propiedad)
laptop.descuento = 0.15; // 15% descuento
console.log(laptop.precio); // 1028.5 (850 + 21% IVA)

laptop.iva = 0.10; // Cambiar IVA a 10%
console.log(laptop.precio); // 935 (850 + 10% IVA)

console.log(laptop.mostrarDetalles());
// {
//   nombre: "Laptop Gaming",
//   precioBase: 1000,
//   descuento: "15%",
//   iva: "10%",
//   precioSinIVA: "850.00",
//   precioFinal: "935.00"
// }
```

## Métodos Estáticos

Los **métodos estáticos** pertenecen a la clase misma, no a las instancias. Se pueden llamar sin crear un objeto de la clase.

```javascript
class Utilidades {
    // Método estático para generar IDs únicos
    static generarId() {
        return 'id_' + Date.now() + '_' + Math.random().toString(36).substr(2, 5);
    }
    
    // Método estático para validar email
    static validarEmail(email) {
        const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return regex.test(email);
    }
    
    // Método estático para formatear fecha
    static formatearFecha(fecha, formato = 'es') {
        if (formato === 'es') {
            return fecha.toLocaleDateString('es-ES');
        } else if (formato === 'en') {
            return fecha.toLocaleDateString('en-US');
        } else {
            return fecha.toISOString().split('T')[0];
        }
    }
    
    // Método estático para calcular edad
    static calcularEdad(fechaNacimiento) {
        const hoy = new Date();
        const nacimiento = new Date(fechaNacimiento);
        let edad = hoy.getFullYear() - nacimiento.getFullYear();
        const diferenciaMeses = hoy.getMonth() - nacimiento.getMonth();
        
        if (diferenciaMeses < 0 || (diferenciaMeses === 0 && hoy.getDate() < nacimiento.getDate())) {
            edad--;
        }
        
        return edad;
    }
}

class Empleado {
    constructor(nombre, email, fechaNacimiento) {
        this.id = Utilidades.generarId(); // Usar método estático
        this.nombre = nombre;
        this.fechaNacimiento = new Date(fechaNacimiento);
        
        // Validar email usando método estático
        if (!Utilidades.validarEmail(email)) {
            throw new Error('Email inválido');
        }
        this.email = email;
        
        this.fechaContratacion = new Date();
    }
    
    obtenerEdad() {
        return Utilidades.calcularEdad(this.fechaNacimiento);
    }
    
    obtenerInfo() {
        return {
            id: this.id,
            nombre: this.nombre,
            email: this.email,
            edad: this.obtenerEdad(),
            fechaContratacion: Utilidades.formatearFecha(this.fechaContratacion)
        };
    }
}

// Usar métodos estáticos directamente de la clase
console.log(Utilidades.generarId()); // "id_1697648400000_abc123"
console.log(Utilidades.validarEmail("test@email.com")); // true
console.log(Utilidades.formatearFecha(new Date())); // "18/10/2025"

// Crear empleado (usa métodos estáticos internamente)
const empleado = new Empleado("Ana García", "ana@empresa.com", "1995-03-15");
console.log(empleado.obtenerInfo());

// Los métodos estáticos NO están disponibles en instancias
// console.log(empleado.generarId()); // Error: empleado.generarId is not a function
```

### Propiedades estáticas

```javascript
class Configuracion {
    // Propiedades estáticas
    static version = "1.2.0";
    static nombre = "Mi Aplicación";
    static entornos = ["desarrollo", "staging", "produccion"];
    
    // Objeto estático con configuraciones
    static configuracionesPorDefecto = {
        tema: "claro",
        idioma: "es",
        notificaciones: true,
        timeoutAPI: 5000
    };
    
    constructor(entorno = "desarrollo") {
        this.entorno = entorno;
        this.configuraciones = { ...Configuracion.configuracionesPorDefecto };
        this.fechaInicializacion = new Date();
    }
    
    // Método estático para obtener versión
    static obtenerVersion() {
        return this.version;
    }
    
    // Método estático para validar entorno
    static esEntornoValido(entorno) {
        return this.entornos.includes(entorno);
    }
    
    // Método de instancia que usa propiedades estáticas
    obtenerInfoCompleta() {
        return {
            aplicacion: Configuracion.nombre,
            version: Configuracion.version,
            entorno: this.entorno,
            configuraciones: this.configuraciones,
            esProduccion: this.entorno === "produccion"
        };
    }
}

// Acceder a propiedades estáticas
console.log(Configuracion.version); // "1.2.0"
console.log(Configuracion.nombre);  // "Mi Aplicación"

// Usar métodos estáticos
console.log(Configuracion.obtenerVersion()); // "1.2.0"
console.log(Configuracion.esEntornoValido("staging")); // true

// Crear instancia
const config = new Configuracion("produccion");
console.log(config.obtenerInfoCompleta());
```

## Herencia con Clases

### Herencia básica con extends

```javascript
// Clase padre
class Animal {
    constructor(nombre, especie, edad) {
        this.nombre = nombre;
        this.especie = especie;
        this.edad = edad;
        this.vivo = true;
        this.energia = 100;
    }
    
    dormir(horas = 8) {
        this.energia = Math.min(100, this.energia + horas * 10);
        return `${this.nombre} durmió ${horas} horas. Energía: ${this.energia}`;
    }
    
    comer(alimento, cantidad = 1) {
        this.energia = Math.min(100, this.energia + cantidad * 5);
        return `${this.nombre} comió ${cantidad} ${alimento}(s). Energía: ${this.energia}`;
    }
    
    moverse() {
        if (this.energia >= 10) {
            this.energia -= 10;
            return `${this.nombre} se movió. Energía restante: ${this.energia}`;
        } else {
            return `${this.nombre} está muy cansado para moverse`;
        }
    }
    
    obtenerInfo() {
        return {
            nombre: this.nombre,
            especie: this.especie,
            edad: this.edad,
            energia: this.energia,
            estado: this.vivo ? 'vivo' : 'muerto'
        };
    }
}

// Clase hija que hereda de Animal
class Perro extends Animal {
    constructor(nombre, raza, edad) {
        // super() llama al constructor del padre
        super(nombre, "Canis lupus", edad);
        this.raza = raza;
        this.entrenado = false;
        this.trucos = [];
    }
    
    ladrar(intensidad = "normal") {
        const sonidos = {
            suave: "guau",
            normal: "¡Guau guau!",
            fuerte: "¡GUAU GUAU GUAU!"
        };
        
        this.energia -= 5;
        return `${this.nombre} ladra: ${sonidos[intensidad] || sonidos.normal}`;
    }
    
    aprenderTruco(truco) {
        if (!this.trucos.includes(truco)) {
            this.trucos.push(truco);
            this.entrenado = true;
            return `${this.nombre} aprendió: ${truco}`;
        } else {
            return `${this.nombre} ya conoce el truco: ${truco}`;
        }
    }
    
    hacerTruco(truco) {
        if (this.trucos.includes(truco)) {
            this.energia -= 15;
            return `${this.nombre} hace el truco: ${truco}`;
        } else {
            return `${this.nombre} no conoce el truco: ${truco}`;
        }
    }
    
    // Sobrescribir método del padre
    obtenerInfo() {
        // Llamar al método del padre y extender la información
        const infoBase = super.obtenerInfo();
        return {
            ...infoBase,
            raza: this.raza,
            entrenado: this.entrenado,
            trucos: this.trucos,
            totalTrucos: this.trucos.length
        };
    }
}

// Clase nieta
class PerroGuia extends Perro {
    constructor(nombre, raza, edad, propietario) {
        super(nombre, raza, edad);
        this.propietario = propietario;
        this.trabajando = false;
        this.certificado = false;
        
        // Los perros guía ya vienen entrenados
        this.entrenado = true;
        this.trucos = ["sentado", "quieto", "ven", "guiar", "parar"];
    }
    
    iniciarTrabajo() {
        if (this.certificado) {
            this.trabajando = true;
            return `${this.nombre} ha iniciado su trabajo de guía`;
        } else {
            return `${this.nombre} necesita certificación antes de trabajar`;
        }
    }
    
    certificar() {
        if (this.trucos.length >= 5 && this.entrenado) {
            this.certificado = true;
            return `${this.nombre} ha sido certificado como perro guía`;
        } else {
            return `${this.nombre} necesita más entrenamiento`;
        }
    }
    
    guiar(direccion) {
        if (this.trabajando) {
            this.energia -= 20;
            return `${this.nombre} está guiando hacia ${direccion}`;
        } else {
            return `${this.nombre} no está trabajando actualmente`;
        }
    }
    
    obtenerInfo() {
        const infoPerro = super.obtenerInfo();
        return {
            ...infoPerro,
            propietario: this.propietario,
            certificado: this.certificado,
            trabajando: this.trabajando,
            tipo: "Perro Guía"
        };
    }
}

// Usar la herencia
const animal = new Animal("Genérico", "Desconocida", 5);
const perro = new Perro("Rex", "Labrador", 3);
const perroGuia = new PerroGuia("Luna", "Pastor Alemán", 4, "María García");

// Métodos heredados funcionan en todas las clases
console.log(perro.dormir(6)); // "Rex durmió 6 horas. Energía: 100"
console.log(perroGuia.comer("croquetas", 2)); // "Luna comió 2 croquetas(s). Energía: 100"

// Métodos específicos de cada clase
console.log(perro.ladrar("fuerte")); // "Rex ladra: ¡GUAU GUAU GUAU!"
console.log(perro.aprenderTruco("sentado")); // "Rex aprendió: sentado"

console.log(perroGuia.certificar()); // "Luna ha sido certificado como perro guía"
console.log(perroGuia.iniciarTrabajo()); // "Luna ha iniciado su trabajo de guía"
console.log(perroGuia.guiar("derecha")); // "Luna está guiando hacia derecha"

// Información completa (método sobrescrito)
console.log(animal.obtenerInfo());
console.log(perro.obtenerInfo());
console.log(perroGuia.obtenerInfo());
```

### Super en métodos

```javascript
class Vehiculo {
    constructor(marca, modelo, año) {
        this.marca = marca;
        this.modelo = modelo;
        this.año = año;
        this.velocidad = 0;
        this.encendido = false;
    }
    
    encender() {
        this.encendido = true;
        return `${this.marca} ${this.modelo} encendido`;
    }
    
    acelerar(incremento = 10) {
        if (this.encendido) {
            this.velocidad += incremento;
            return `Acelerando... Velocidad: ${this.velocidad} km/h`;
        } else {
            return "Debe encender el vehículo primero";
        }
    }
    
    frenar(decremento = 15) {
        this.velocidad = Math.max(0, this.velocidad - decremento);
        return `Frenando... Velocidad: ${this.velocidad} km/h`;
    }
    
    obtenerEstado() {
        return `${this.marca} ${this.modelo} (${this.año}) - ${this.velocidad} km/h - ${this.encendido ? 'Encendido' : 'Apagado'}`;
    }
}

class Coche extends Vehiculo {
    constructor(marca, modelo, año, puertas) {
        super(marca, modelo, año);
        this.puertas = puertas;
        this.marchaActual = 0; // 0 = neutral, -1 = reversa
        this.marchaMaxima = 6;
    }
    
    encender() {
        // Llamar al método del padre y agregar funcionalidad
        const resultado = super.encender();
        this.marchaActual = 0; // Siempre inicia en neutral
        return `${resultado}. En neutral`;
    }
    
    cambiarMarcha(marcha) {
        if (!this.encendido) {
            return "Debe encender el coche primero";
        }
        
        if (marcha >= -1 && marcha <= this.marchaMaxima) {
            this.marchaActual = marcha;
            const marchaTexto = marcha === -1 ? "Reversa" : 
                               marcha === 0 ? "Neutral" : `${marcha}ª marcha`;
            return `Cambio a: ${marchaTexto}`;
        } else {
            return "Marcha inválida";
        }
    }
    
    acelerar(incremento = 10) {
        if (this.marchaActual === 0) {
            return "No se puede acelerar en neutral";
        }
        
        // Llamar al método del padre
        const resultado = super.acelerar(incremento);
        
        // Agregar lógica específica del coche
        if (this.marchaActual === -1) {
            return `${resultado} (marcha atrás)`;
        } else {
            return `${resultado} (${this.marchaActual}ª marcha)`;
        }
    }
    
    obtenerEstado() {
        // Extender el método del padre
        const estadoBase = super.obtenerEstado();
        const marchaTexto = this.marchaActual === -1 ? "Reversa" : 
                           this.marchaActual === 0 ? "Neutral" : `${this.marchaActual}ª marcha`;
        return `${estadoBase} - ${this.puertas} puertas - ${marchaTexto}`;
    }
}

class Moto extends Vehiculo {
    constructor(marca, modelo, año, cilindrada) {
        super(marca, modelo, año);
        this.cilindrada = cilindrada;
        this.caballito = false;
    }
    
    hacerCaballito() {
        if (this.encendido && this.velocidad > 20) {
            this.caballito = true;
            return `${this.marca} ${this.modelo} haciendo caballito!`;
        } else {
            return "Necesita más velocidad para hacer caballito";
        }
    }
    
    acelerar(incremento = 15) {
        // Las motos aceleran más rápido
        const resultado = super.acelerar(incremento);
        
        if (this.caballito) {
            this.caballito = false; // Se baja el caballito al acelerar más
            return `${resultado} - Caballito terminado`;
        }
        
        return resultado;
    }
    
    obtenerEstado() {
        const estadoBase = super.obtenerEstado();
        const cilindradaTexto = `${this.cilindrada}cc`;
        const caballitoTexto = this.caballito ? " - ¡Haciendo caballito!" : "";
        return `${estadoBase} - ${cilindradaTexto}${caballitoTexto}`;
    }
}

// Uso de la herencia con super
const coche = new Coche("Toyota", "Corolla", 2023, 4);
const moto = new Moto("Honda", "CBR600", 2023, 600);

// Coche
console.log(coche.encender());           // "Toyota Corolla encendido. En neutral"
console.log(coche.cambiarMarcha(1));     // "Cambio a: 1ª marcha"
console.log(coche.acelerar());           // "Acelerando... Velocidad: 10 km/h (1ª marcha)"
console.log(coche.obtenerEstado());      // "Toyota Corolla (2023) - 10 km/h - Encendido - 4 puertas - 1ª marcha"

// Moto
console.log(moto.encender());            // "Honda CBR600 encendido"
console.log(moto.acelerar(30));          // "Acelerando... Velocidad: 30 km/h"
console.log(moto.hacerCaballito());      // "Honda CBR600 haciendo caballito!"
console.log(moto.acelerar());            // "Acelerando... Velocidad: 45 km/h - Caballito terminado"
console.log(moto.obtenerEstado());       // "Honda CBR600 (2023) - 45 km/h - Encendido - 600cc"
```

## Campos Privados (Private Fields) - ES2022

JavaScript moderno permite crear verdaderos campos privados usando la sintaxis `#`.

```javascript
class CuentaBancaria {
    // Campos privados (no accesibles desde fuera)
    #saldo = 0;
    #numeroCuenta;
    #pin;
    #movimientos = [];
    
    // Campo privado estático
    static #contadorCuentas = 0;
    
    constructor(titular, pinInicial, saldoInicial = 0) {
        this.titular = titular;
        this.fechaApertura = new Date();
        this.activa = true;
        
        // Asignar valores a campos privados
        this.#numeroCuenta = this.#generarNumeroCuenta();
        this.#pin = pinInicial;
        this.#saldo = saldoInicial;
        
        // Incrementar contador estático privado
        CuentaBancaria.#contadorCuentas++;
        
        this.#registrarMovimiento('apertura', saldoInicial);
    }
    
    // Método privado
    #generarNumeroCuenta() {
        return 'ES' + Date.now().toString().slice(-10) + Math.random().toString(36).substr(2, 4);
    }
    
    // Método privado para registrar movimientos
    #registrarMovimiento(tipo, cantidad, descripcion = '') {
        this.#movimientos.push({
            fecha: new Date(),
            tipo: tipo,
            cantidad: cantidad,
            saldo: this.#saldo,
            descripcion: descripcion
        });
    }
    
    // Método privado para validar PIN
    #validarPin(pinIngresado) {
        return this.#pin === pinIngresado;
    }
    
    // Método público para obtener saldo
    obtenerSaldo(pin) {
        if (!this.#validarPin(pin)) {
            throw new Error('PIN incorrecto');
        }
        return this.#saldo;
    }
    
    // Método público para depositar
    depositar(cantidad, pin) {
        if (!this.#validarPin(pin)) {
            throw new Error('PIN incorrecto');
        }
        
        if (cantidad <= 0) {
            throw new Error('Cantidad debe ser positiva');
        }
        
        if (!this.activa) {
            throw new Error('Cuenta inactiva');
        }
        
        this.#saldo += cantidad;
        this.#registrarMovimiento('depósito', cantidad);
        
        return {
            mensaje: `Depósito realizado correctamente`,
            nuevoSaldo: this.#saldo
        };
    }
    
    // Método público para retirar
    retirar(cantidad, pin) {
        if (!this.#validarPin(pin)) {
            throw new Error('PIN incorrecto');
        }
        
        if (cantidad <= 0) {
            throw new Error('Cantidad debe ser positiva');
        }
        
        if (cantidad > this.#saldo) {
            throw new Error('Fondos insuficientes');
        }
        
        if (!this.activa) {
            throw new Error('Cuenta inactiva');
        }
        
        this.#saldo -= cantidad;
        this.#registrarMovimiento('retiro', cantidad);
        
        return {
            mensaje: `Retiro realizado correctamente`,
            nuevoSaldo: this.#saldo
        };
    }
    
    // Método público para cambiar PIN
    cambiarPin(pinActual, pinNuevo) {
        if (!this.#validarPin(pinActual)) {
            throw new Error('PIN actual incorrecto');
        }
        
        if (pinNuevo.length < 4) {
            throw new Error('PIN debe tener al menos 4 caracteres');
        }
        
        this.#pin = pinNuevo;
        this.#registrarMovimiento('cambio_pin', 0, 'PIN modificado');
        
        return 'PIN cambiado correctamente';
    }
    
    // Método público para obtener movimientos
    obtenerMovimientos(pin, limite = 10) {
        if (!this.#validarPin(pin)) {
            throw new Error('PIN incorrecto');
        }
        
        return this.#movimientos
            .slice(-limite)
            .map(mov => ({
                fecha: mov.fecha.toLocaleDateString(),
                tipo: mov.tipo,
                cantidad: mov.cantidad,
                descripcion: mov.descripcion
            }));
    }
    
    // Método público para obtener información general (sin PIN)
    obtenerInfoGeneral() {
        return {
            titular: this.titular,
            numeroCuenta: this.#numeroCuenta.slice(0, 6) + '****', // Ocultar parte del número
            fechaApertura: this.fechaApertura.toLocaleDateString(),
            activa: this.activa,
            totalMovimientos: this.#movimientos.length
        };
    }
    
    // Método estático público
    static obtenerTotalCuentas() {
        return CuentaBancaria.#contadorCuentas;
    }
}

// Uso de campos privados
const cuenta = new CuentaBancaria("María García", "1234", 1000);

// Acceso a información pública
console.log(cuenta.obtenerInfoGeneral());
// {
//   titular: "María García",
//   numeroCuenta: "ES1697****",
//   fechaApertura: "18/10/2025",
//   activa: true,
//   totalMovimientos: 1
// }

// Operaciones con PIN
console.log(cuenta.depositar(500, "1234")); // { mensaje: "Depósito realizado correctamente", nuevoSaldo: 1500 }
console.log(cuenta.retirar(200, "1234"));   // { mensaje: "Retiro realizado correctamente", nuevoSaldo: 1300 }
console.log(cuenta.obtenerSaldo("1234"));   // 1300

// Cambiar PIN
console.log(cuenta.cambiarPin("1234", "5678")); // "PIN cambiado correctamente"

// Ver movimientos
console.log(cuenta.obtenerMovimientos("5678", 5));

// Intentar acceso no autorizado
try {
    console.log(cuenta.obtenerSaldo("0000")); // Error: PIN incorrecto
} catch (error) {
    console.log("Error:", error.message);
}

// Los campos privados NO son accesibles desde fuera
// console.log(cuenta.#saldo); // SyntaxError: Private field '#saldo' must be declared in an enclosing class
// console.log(cuenta.#pin);   // SyntaxError: Private field '#pin' must be declared in an enclosing class

// Método estático
console.log(CuentaBancaria.obtenerTotalCuentas()); // 1
```

## Casos Prácticos Completos

### Sistema de Gestión de Empleados

```javascript
// Clase base para personas
class Persona {
    constructor(nombre, apellido, fechaNacimiento, email) {
        this.nombre = nombre;
        this.apellido = apellido;
        this.fechaNacimiento = new Date(fechaNacimiento);
        this.email = email;
        this.id = this.constructor.generarId();
    }
    
    static generarId() {
        return 'PERS_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4);
    }
    
    obtenerNombreCompleto() {
        return `${this.nombre} ${this.apellido}`;
    }
    
    calcularEdad() {
        const hoy = new Date();
        let edad = hoy.getFullYear() - this.fechaNacimiento.getFullYear();
        const diferenciaMeses = hoy.getMonth() - this.fechaNacimiento.getMonth();
        
        if (diferenciaMeses < 0 || (diferenciaMeses === 0 && hoy.getDate() < this.fechaNacimiento.getDate())) {
            edad--;
        }
        
        return edad;
    }
    
    obtenerInfo() {
        return {
            id: this.id,
            nombreCompleto: this.obtenerNombreCompleto(),
            email: this.email,
            edad: this.calcularEdad()
        };
    }
}

// Clase para empleados
class Empleado extends Persona {
    static #contadorEmpleados = 0;
    static #salarioMinimo = 18000; // Salario mínimo anual
    
    #salario;
    #departamento;
    
    constructor(nombre, apellido, fechaNacimiento, email, departamento, salario) {
        super(nombre, apellido, fechaNacimiento, email);
        
        this.numeroEmpleado = ++Empleado.#contadorEmpleados;
        this.fechaContratacion = new Date();
        this.activo = true;
        this.#departamento = departamento;
        
        // Validar salario
        if (salario < Empleado.#salarioMinimo) {
            throw new Error(`Salario debe ser al menos ${Empleado.#salarioMinimo}`);
        }
        this.#salario = salario;
    }
    
    // Getter para salario
    get salario() {
        return this.#salario;
    }
    
    // Setter para salario con validación
    set salario(nuevoSalario) {
        if (nuevoSalario < Empleado.#salarioMinimo) {
            throw new Error(`Salario debe ser al menos ${Empleado.#salarioMinimo}`);
        }
        this.#salario = nuevoSalario;
    }
    
    get departamento() {
        return this.#departamento;
    }
    
    set departamento(nuevoDepartamento) {
        this.#departamento = nuevoDepartamento;
    }
    
    calcularSalarioMensual() {
        return this.#salario / 12;
    }
    
    darAumento(porcentaje) {
        if (porcentaje <= 0) {
            throw new Error('Porcentaje debe ser positivo');
        }
        
        const salarioAnterior = this.#salario;
        this.#salario *= (1 + porcentaje / 100);
        
        return {
            salarioAnterior: salarioAnterior,
            salarioNuevo: this.#salario,
            aumento: this.#salario - salarioAnterior,
            porcentaje: porcentaje
        };
    }
    
    obtenerAntiguedad() {
        const hoy = new Date();
        const años = hoy.getFullYear() - this.fechaContratacion.getFullYear();
        return años;
    }
    
    obtenerInfo() {
        const infoBase = super.obtenerInfo();
        return {
            ...infoBase,
            numeroEmpleado: this.numeroEmpleado,
            departamento: this.#departamento,
            salarioAnual: this.#salario,
            salarioMensual: this.calcularSalarioMensual(),
            fechaContratacion: this.fechaContratacion.toLocaleDateString(),
            antiguedad: this.obtenerAntiguedad(),
            activo: this.activo
        };
    }
    
    static obtenerSalarioMinimo() {
        return Empleado.#salarioMinimo;
    }
    
    static establecerSalarioMinimo(nuevoMinimo) {
        if (nuevoMinimo <= 0) {
            throw new Error('Salario mínimo debe ser positivo');
        }
        Empleado.#salarioMinimo = nuevoMinimo;
    }
    
    static obtenerTotalEmpleados() {
        return Empleado.#contadorEmpleados;
    }
}

// Clase para gerentes
class Gerente extends Empleado {
    #equipoACargo = [];
    #presupuesto;
    
    constructor(nombre, apellido, fechaNacimiento, email, departamento, salario, presupuesto) {
        super(nombre, apellido, fechaNacimiento, email, departamento, salario);
        this.#presupuesto = presupuesto;
        this.esGerente = true;
    }
    
    get presupuesto() {
        return this.#presupuesto;
    }
    
    set presupuesto(nuevoPresupuesto) {
        if (nuevoPresupuesto < 0) {
            throw new Error('Presupuesto no puede ser negativo');
        }
        this.#presupuesto = nuevoPresupuesto;
    }
    
    agregarEmpleado(empleado) {
        if (!(empleado instanceof Empleado)) {
            throw new Error('Solo se pueden agregar empleados');
        }
        
        if (empleado.esGerente) {
            throw new Error('No se puede agregar otro gerente al equipo');
        }
        
        if (!this.#equipoACargo.includes(empleado)) {
            this.#equipoACargo.push(empleado);
            return `${empleado.obtenerNombreCompleto()} agregado al equipo`;
        } else {
            return `${empleado.obtenerNombreCompleto()} ya está en el equipo`;
        }
    }
    
    removerEmpleado(empleado) {
        const indice = this.#equipoACargo.indexOf(empleado);
        if (indice !== -1) {
            this.#equipoACargo.splice(indice, 1);
            return `${empleado.obtenerNombreCompleto()} removido del equipo`;
        } else {
            return `${empleado.obtenerNombreCompleto()} no está en el equipo`;
        }
    }
    
    obtenerEquipo() {
        return this.#equipoACargo.map(emp => ({
            nombre: emp.obtenerNombreCompleto(),
            numeroEmpleado: emp.numeroEmpleado,
            departamento: emp.departamento
        }));
    }
    
    calcularCostoEquipo() {
        return this.#equipoACargo.reduce((total, emp) => total + emp.salario, 0);
    }
    
    darAumentoAEquipo(porcentaje) {
        const resultados = this.#equipoACargo.map(emp => {
            const resultado = emp.darAumento(porcentaje);
            return {
                empleado: emp.obtenerNombreCompleto(),
                ...resultado
            };
        });
        
        return {
            aumentosRealizados: resultados.length,
            detalles: resultados,
            nuevoCostoTotal: this.calcularCostoEquipo()
        };
    }
    
    obtenerInfo() {
        const infoBase = super.obtenerInfo();
        return {
            ...infoBase,
            tipoEmpleado: 'Gerente',
            presupuesto: this.#presupuesto,
            equipoACargo: this.#equipoACargo.length,
            costoEquipo: this.calcularCostoEquipo(),
            presupuestoRestante: this.#presupuesto - this.calcularCostoEquipo()
        };
    }
}

// Uso del sistema
try {
    // Crear empleados
    const emp1 = new Empleado("Juan", "Pérez", "1990-05-15", "juan@empresa.com", "IT", 25000);
    const emp2 = new Empleado("Ana", "García", "1988-09-20", "ana@empresa.com", "IT", 28000);
    const emp3 = new Empleado("Carlos", "López", "1985-12-10", "carlos@empresa.com", "Marketing", 30000);
    
    // Crear gerente
    const gerente = new Gerente("María", "Rodríguez", "1980-03-25", "maria@empresa.com", "IT", 45000, 200000);
    
    // Construir equipo
    console.log(gerente.agregarEmpleado(emp1)); // "Juan Pérez agregado al equipo"
    console.log(gerente.agregarEmpleado(emp2)); // "Ana García agregado al equipo"
    
    // Ver información
    console.log("=== INFORMACIÓN DE EMPLEADOS ===");
    console.log(emp1.obtenerInfo());
    console.log(gerente.obtenerInfo());
    
    // Dar aumento
    console.log("\n=== AUMENTO INDIVIDUAL ===");
    console.log(emp1.darAumento(10)); // 10% de aumento
    
    // Dar aumento al equipo
    console.log("\n=== AUMENTO AL EQUIPO ===");
    console.log(gerente.darAumentoAEquipo(8)); // 8% de aumento al equipo
    
    // Ver equipo
    console.log("\n=== EQUIPO DEL GERENTE ===");
    console.log(gerente.obtenerEquipo());
    
    // Estadísticas generales
    console.log("\n=== ESTADÍSTICAS ===");
    console.log("Total empleados:", Empleado.obtenerTotalEmpleados());
    console.log("Salario mínimo:", Empleado.obtenerSalarioMinimo());
    
} catch (error) {
    console.error("Error:", error.message);
}
```

### Sistema de E-commerce con Clases

```javascript
// Clase base para productos
class Producto {
    static #contadorProductos = 0;
    
    #precio;
    #stock;
    
    constructor(nombre, descripcion, precio, categoria, stock = 0) {
        this.id = ++Producto.#contadorProductos;
        this.nombre = nombre;
        this.descripcion = descripcion;
        this.categoria = categoria;
        this.fechaCreacion = new Date();
        this.activo = true;
        this.valoraciones = [];
        
        this.precio = precio; // Usar setter para validación
        this.stock = stock;   // Usar setter para validación
    }
    
    get precio() {
        return this.#precio;
    }
    
    set precio(nuevoPrecio) {
        if (nuevoPrecio < 0) {
            throw new Error('Precio no puede ser negativo');
        }
        this.#precio = nuevoPrecio;
    }
    
    get stock() {
        return this.#stock;
    }
    
    set stock(nuevoStock) {
        if (nuevoStock < 0) {
            throw new Error('Stock no puede ser negativo');
        }
        this.#stock = nuevoStock;
    }
    
    agregarStock(cantidad) {
        this.#stock += cantidad;
        return `Stock actualizado. Nuevo stock: ${this.#stock}`;
    }
    
    reducirStock(cantidad) {
        if (cantidad > this.#stock) {
            throw new Error('Stock insuficiente');
        }
        this.#stock -= cantidad;
        return `Stock reducido. Stock restante: ${this.#stock}`;
    }
    
    agregarValoracion(puntuacion, comentario = '') {
        if (puntuacion < 1 || puntuacion > 5) {
            throw new Error('Puntuación debe estar entre 1 y 5');
        }
        
        this.valoraciones.push({
            puntuacion: puntuacion,
            comentario: comentario,
            fecha: new Date()
        });
        
        return 'Valoración agregada correctamente';
    }
    
    obtenerPromedio() {
        if (this.valoraciones.length === 0) return 0;
        
        const suma = this.valoraciones.reduce((total, val) => total + val.puntuacion, 0);
        return (suma / this.valoraciones.length).toFixed(1);
    }
    
    obtenerInfo() {
        return {
            id: this.id,
            nombre: this.nombre,
            descripcion: this.descripcion,
            precio: this.#precio,
            categoria: this.categoria,
            stock: this.#stock,
            activo: this.activo,
            promedio: this.obtenerPromedio(),
            totalValoraciones: this.valoraciones.length
        };
    }
    
    static obtenerTotalProductos() {
        return Producto.#contadorProductos;
    }
}

// Clase para productos digitales
class ProductoDigital extends Producto {
    #urlDescarga;
    #claveLicencia;
    
    constructor(nombre, descripcion, precio, categoria, urlDescarga, tipoLicencia = 'individual') {
        super(nombre, descripcion, precio, categoria, Infinity); // Stock infinito para digitales
        this.#urlDescarga = urlDescarga;
        this.tipoLicencia = tipoLicencia;
        this.#claveLicencia = this.#generarClaveLicencia();
        this.esDigital = true;
    }
    
    #generarClaveLicencia() {
        return 'LIC-' + Math.random().toString(36).toUpperCase().substr(2, 8);
    }
    
    obtenerAccesoDescarga(compraId) {
        // Simular validación de compra
        if (!compraId) {
            throw new Error('ID de compra requerido');
        }
        
        return {
            urlDescarga: this.#urlDescarga,
            claveLicencia: this.#claveLicencia,
            tipoLicencia: this.tipoLicencia,
            validoHasta: new Date(Date.now() + 365 * 24 * 60 * 60 * 1000) // 1 año
        };
    }
    
    reducirStock(cantidad) {
        // Los productos digitales no reducen stock
        return 'Producto digital - Stock ilimitado';
    }
    
    obtenerInfo() {
        const infoBase = super.obtenerInfo();
        return {
            ...infoBase,
            tipoProducto: 'Digital',
            tipoLicencia: this.tipoLicencia,
            stock: 'Ilimitado'
        };
    }
}

// Clase para productos físicos
class ProductoFisico extends Producto {
    constructor(nombre, descripcion, precio, categoria, stock, peso, dimensiones) {
        super(nombre, descripcion, precio, categoria, stock);
        this.peso = peso; // en gramos
        this.dimensiones = dimensiones; // { largo, ancho, alto } en cm
        this.esDigital = false;
    }
    
    calcularCostoEnvio(destino = 'nacional') {
        const tarifasPorKg = {
            local: 3,
            nacional: 8,
            internacional: 25
        };
        
        const pesoKg = this.peso / 1000;
        const tarifa = tarifasPorKg[destino] || tarifasPorKg.nacional;
        
        return Math.max(5, pesoKg * tarifa); // Mínimo 5€
    }
    
    obtenerVolumen() {
        const { largo, ancho, alto } = this.dimensiones;
        return (largo * ancho * alto) / 1000; // en litros
    }
    
    obtenerInfo() {
        const infoBase = super.obtenerInfo();
        return {
            ...infoBase,
            tipoProducto: 'Físico',
            peso: `${this.peso}g`,
            dimensiones: this.dimensiones,
            volumen: `${this.obtenerVolumen()}L`,
            costoEnvioNacional: `${this.calcularCostoEnvio('nacional')}€`
        };
    }
}

// Clase para el carrito de compras
class CarritoCompras {
    #items = [];
    
    constructor(cliente) {
        this.cliente = cliente;
        this.fechaCreacion = new Date();
        this.descuento = 0;
    }
    
    agregarItem(producto, cantidad = 1) {
        if (!(producto instanceof Producto)) {
            throw new Error('Solo se pueden agregar productos');
        }
        
        if (!producto.activo) {
            throw new Error('Producto no disponible');
        }
        
        if (!producto.esDigital && cantidad > producto.stock) {
            throw new Error(`Stock insuficiente. Disponible: ${producto.stock}`);
        }
        
        // Buscar si el producto ya está en el carrito
        const itemExistente = this.#items.find(item => item.producto.id === producto.id);
        
        if (itemExistente) {
            itemExistente.cantidad += cantidad;
            return `Cantidad actualizada. Total: ${itemExistente.cantidad}`;
        } else {
            this.#items.push({
                producto: producto,
                cantidad: cantidad,
                precioUnitario: producto.precio
            });
            return `Producto agregado al carrito`;
        }
    }
    
    removerItem(productoId) {
        const indice = this.#items.findIndex(item => item.producto.id === productoId);
        
        if (indice !== -1) {
            const item = this.#items.splice(indice, 1)[0];
            return `${item.producto.nombre} removido del carrito`;
        } else {
            throw new Error('Producto no encontrado en el carrito');
        }
    }
    
    actualizarCantidad(productoId, nuevaCantidad) {
        if (nuevaCantidad <= 0) {
            return this.removerItem(productoId);
        }
        
        const item = this.#items.find(item => item.producto.id === productoId);
        
        if (item) {
            if (!item.producto.esDigital && nuevaCantidad > item.producto.stock) {
                throw new Error(`Stock insuficiente. Disponible: ${item.producto.stock}`);
            }
            
            item.cantidad = nuevaCantidad;
            return `Cantidad actualizada a ${nuevaCantidad}`;
        } else {
            throw new Error('Producto no encontrado en el carrito');
        }
    }
    
    aplicarDescuento(porcentaje) {
        if (porcentaje < 0 || porcentaje > 100) {
            throw new Error('Descuento debe estar entre 0 y 100');
        }
        
        this.descuento = porcentaje;
        return `Descuento del ${porcentaje}% aplicado`;
    }
    
    calcularSubtotal() {
        return this.#items.reduce((total, item) => {
            return total + (item.precioUnitario * item.cantidad);
        }, 0);
    }
    
    calcularDescuento() {
        return this.calcularSubtotal() * (this.descuento / 100);
    }
    
    calcularCostoEnvio() {
        // Solo productos físicos tienen costo de envío
        const productsFisicos = this.#items.filter(item => !item.producto.esDigital);
        
        if (productsFisicos.length === 0) {
            return 0; // Productos digitales no tienen costo de envío
        }
        
        // Calcular envío basado en el producto más caro de enviar
        return Math.max(...productsFisicos.map(item => 
            item.producto.calcularCostoEnvio('nacional')
        ));
    }
    
    calcularTotal() {
        const subtotal = this.calcularSubtotal();
        const descuento = this.calcularDescuento();
        const envio = this.calcularCostoEnvio();
        
        return {
            subtotal: subtotal,
            descuento: descuento,
            envio: envio,
            total: subtotal - descuento + envio
        };
    }
    
    obtenerItems() {
        return this.#items.map(item => ({
            producto: {
                id: item.producto.id,
                nombre: item.producto.nombre,
                precio: item.precioUnitario,
                esDigital: item.producto.esDigital
            },
            cantidad: item.cantidad,
            subtotal: item.precioUnitario * item.cantidad
        }));
    }
    
    vaciar() {
        this.#items = [];
        this.descuento = 0;
        return 'Carrito vaciado';
    }
    
    obtenerResumen() {
        const totales = this.calcularTotal();
        
        return {
            cliente: this.cliente,
            items: this.obtenerItems(),
            cantidadItems: this.#items.length,
            cantidadProductos: this.#items.reduce((total, item) => total + item.cantidad, 0),
            descuentoAplicado: `${this.descuento}%`,
            ...totales
        };
    }
}

// Uso del sistema de e-commerce
try {
    // Crear productos
    const laptop = new ProductoFisico(
        "Laptop Gaming",
        "Laptop para gaming de alta gama",
        1299.99,
        "Tecnología",
        5,
        2500, // 2.5 kg
        { largo: 35, ancho: 25, alto: 3 }
    );
    
    const software = new ProductoDigital(
        "Adobe Photoshop",
        "Software de edición de imágenes",
        299.99,
        "Software",
        "https://adobe.com/download/ps",
        "individual"
    );
    
    const mouse = new ProductoFisico(
        "Mouse Gaming",
        "Mouse óptico para gaming",
        59.99,
        "Tecnología",
        15,
        150, // 150g
        { largo: 12, ancho: 7, alto: 4 }
    );
    
    // Agregar valoraciones
    laptop.agregarValoracion(5, "Excelente laptop");
    laptop.agregarValoracion(4, "Muy buena calidad");
    software.agregarValoracion(5, "El mejor software de edición");
    
    // Crear carrito
    const carrito = new CarritoCompras("cliente@email.com");
    
    // Agregar productos al carrito
    console.log(carrito.agregarItem(laptop, 1));     // "Producto agregado al carrito"
    console.log(carrito.agregarItem(software, 1));   // "Producto agregado al carrito"
    console.log(carrito.agregarItem(mouse, 2));      // "Producto agregado al carrito"
    
    // Aplicar descuento
    console.log(carrito.aplicarDescuento(10));       // "Descuento del 10% aplicado"
    
    // Ver resumen del carrito
    console.log("\n=== RESUMEN DEL CARRITO ===");
    console.log(JSON.stringify(carrito.obtenerResumen(), null, 2));
    
    // Información de productos
    console.log("\n=== INFORMACIÓN DE PRODUCTOS ===");
    console.log(laptop.obtenerInfo());
    console.log(software.obtenerInfo());
    
    // Acceso a descarga de producto digital (simulando compra exitosa)
    const accesoDescarga = software.obtenerAccesoDescarga("COMPRA-123");
    console.log("\n=== ACCESO A DESCARGA ===");
    console.log(accesoDescarga);
    
} catch (error) {
    console.error("Error:", error.message);
}
```

## Mejores Prácticas con Clases

### ✅ Buenas prácticas

```javascript
// 1. Nombres descriptivos en PascalCase
class GestorInventario {
    constructor() {
        this.productos = new Map();
    }
}

// 2. Usar campos privados para encapsulación
class CuentaSegura {
    #saldo = 0;
    
    depositar(cantidad) {
        this.#saldo += cantidad;
    }
}

// 3. Validación en constructores
class Usuario {
    constructor(email, edad) {
        if (!email || !email.includes('@')) {
            throw new Error('Email válido requerido');
        }
        if (edad < 0 || edad > 120) {
            throw new Error('Edad debe estar entre 0 y 120');
        }
        
        this.email = email;
        this.edad = edad;
    }
}

// 4. Usar super() correctamente en herencia
class EmpleadoTiempoCompleto extends Empleado {
    constructor(nombre, salario) {
        super(nombre); // Llamar PRIMERO al constructor padre
        this.tipoContrato = 'tiempo completo';
        this.salario = salario;
    }
}

// 5. Métodos estáticos para funcionalidades de clase
class Matematicas {
    static PI = 3.14159;
    
    static calcularAreaCirculo(radio) {
        return this.PI * radio * radio;
    }
}
```

### ❌ Prácticas a evitar

```javascript
// 1. NO usar arrow functions para métodos de clase
class Incorrecto {
    // ❌ Arrow function no tiene 'this' propio
    metodo = () => {
        console.log(this); // Puede no ser lo que esperas
    }
    
    // ✅ Método normal
    metodoCorrect() {
        console.log(this); // 'this' se refiere a la instancia
    }
}

// 2. NO acceder a campos privados desde fuera
class ConCampoPrivado {
    #secreto = "valor secreto";
}

const obj = new ConCampoPrivado();
// console.log(obj.#secreto); // ❌ SyntaxError

// 3. NO olvidar super() en constructores de clases hijas
class Padre {
    constructor(valor) {
        this.valor = valor;
    }
}

class HijoIncorrecto extends Padre {
    constructor(valor, extra) {
        // ❌ Falta super()
        this.extra = extra; // ReferenceError: Must call super constructor
    }
}

class HijoCorrecto extends Padre {
    constructor(valor, extra) {
        super(valor); // ✅ Llamar primero a super()
        this.extra = extra;
    }
}
```

***

Las clases en JavaScript proporcionan una sintaxis moderna y limpia para la programación orientada a objetos. Aunque internamente siguen usando prototypes, ofrecen características como campos privados, métodos estáticos y una herencia más intuitiva que hacen el código más legible y mantenible.
