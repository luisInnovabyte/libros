# Async/Await en JavaScript

**Async/Await** es la sintaxis moderna de JavaScript para manejar operaciones asíncronas de manera más limpia y legible que las Promises tradicionales. Introducido en ES2017 (ES8), async/await permite escribir código asíncrono que se lee como código síncrono.

> **📚 Fundamentos previos**: Para entender completamente async/await, es recomendable conocer [`promises.md`](./promises.md) ya que async/await es "syntactic sugar" sobre las Promises.

## ¿Qué son Async/Await?

**Async/Await** es una forma más elegante de trabajar con Promises que elimina la necesidad de cadenas `.then()` y hace que el código asíncrono sea más fácil de leer, escribir y mantener.

### Conceptos clave

- **`async`**: Palabra clave que convierte una función en asíncrona
- **`await`**: Palabra clave que pausa la ejecución hasta que una Promise se resuelve
- **Syntactic Sugar**: Es una abstracción sobre Promises, no los reemplaza

```javascript
// ❌ Promises tradicionales (más verboso)
function obtenerDatosConPromises() {
    return fetch('/api/usuario/123')
        .then(response => response.json())
        .then(usuario => {
            return fetch(`/api/perfil/${usuario.id}`);
        })
        .then(response => response.json())
        .then(perfil => {
            return fetch(`/api/configuracion/${perfil.id}`);
        })
        .then(response => response.json())
        .then(configuracion => {
            return {
                usuario: usuario,
                perfil: perfil, 
                configuracion: configuracion
            };
        })
        .catch(error => {
            console.error("Error:", error);
            throw error;
        });
}

// ✅ Async/Await (más limpio y legible)
async function obtenerDatosConAsyncAwait() {
    try {
        const responseUsuario = await fetch('/api/usuario/123');
        const usuario = await responseUsuario.json();
        
        const responsePerfil = await fetch(`/api/perfil/${usuario.id}`);
        const perfil = await responsePerfil.json();
        
        const responseConfiguracion = await fetch(`/api/configuracion/${perfil.id}`);
        const configuracion = await responseConfiguracion.json();
        
        return {
            usuario: usuario,
            perfil: perfil,
            configuracion: configuracion
        };
        
    } catch (error) {
        console.error("Error:", error);
        throw error;
    }
}
```

## Sintaxis Básica de Async

### Función async básica

```javascript
// Declaración de función async
async function miFuncionAsincrona() {
    // Esta función automáticamente devuelve una Promise
    return "¡Hola desde función async!";
}

// Expresión de función async
const miFuncionAsync2 = async function() {
    return "¡Hola desde expresión async!";
};

// Arrow function async
const miFuncionAsync3 = async () => {
    return "¡Hola desde arrow function async!";
};

// Método async en objeto
const miObjeto = {
    async miMetodo() {
        return "¡Hola desde método async!";
    }
};

// Método async en clase
class MiClase {
    async miMetodo() {
        return "¡Hola desde clase async!";
    }
}

// Todas las funciones async devuelven automáticamente una Promise
console.log(miFuncionAsincrona()); // Promise<pending>

// Para obtener el valor, usar .then() o await
miFuncionAsincrona().then(resultado => {
    console.log(resultado); // "¡Hola desde función async!"
});

// O mejor aún, usar await
async function usarFuncionAsync() {
    const resultado = await miFuncionAsincrona();
    console.log(resultado); // "¡Hola desde función async!"
}

usarFuncionAsync();
```

### Conversión automática a Promise

```javascript
// Las funciones async siempre devuelven una Promise
async function ejemplosRetorno() {
    // Devolver valor directo
    return 42;
    // Equivale a: return Promise.resolve(42);
}

async function ejemplosRetorno2() {
    // Devolver Promise explícita
    return Promise.resolve("Valor resuelto");
    // La Promise se "unwraps" automáticamente
}

async function ejemplosRetorno3() {
    // Lanzar error
    throw new Error("Algo salió mal");
    // Equivale a: return Promise.reject(new Error("Algo salió mal"));
}

// Verificar que todas devuelven Promises
console.log(ejemplosRetorno()); // Promise<pending>
console.log(ejemplosRetorno2()); // Promise<pending>
console.log(ejemplosRetorno3()); // Promise<pending>

// Consumir los resultados
async function consumirResultados() {
    try {
        const resultado1 = await ejemplosRetorno();
        console.log("Resultado 1:", resultado1); // 42
        
        const resultado2 = await ejemplosRetorno2();
        console.log("Resultado 2:", resultado2); // "Valor resuelto"
        
        const resultado3 = await ejemplosRetorno3();
        console.log("Resultado 3:", resultado3); // No se ejecuta
        
    } catch (error) {
        console.error("Error capturado:", error.message); // "Algo salió mal"
    }
}

consumirResultados();
```

## Sintaxis de Await

### Uso básico de await

```javascript
// await solo puede usarse dentro de funciones async
async function ejemploAwait() {
    // Simular operación asíncrona
    function operacionAsincrona() {
        return new Promise((resolve) => {
            setTimeout(() => {
                resolve("Operación completada");
            }, 2000);
        });
    }
    
    console.log("🚀 Iniciando operación...");
    
    // await pausa la ejecución hasta que la Promise se resuelve
    const resultado = await operacionAsincrona();
    
    console.log("✅ Resultado:", resultado);
    console.log("🏁 Operación terminada");
}

// Ejecutar ejemplo
ejemploAwait();
// Salida:
// 🚀 Iniciando operación...
// (pausa 2 segundos)
// ✅ Resultado: Operación completada
// 🏁 Operación terminada
```

### Await con diferentes tipos de valores

```javascript
async function ejemplosAwaitVariados() {
    // 1. Await con Promise
    const promesa = Promise.resolve("Valor de Promise");
    const resultadoPromesa = await promesa;
    console.log("Promise:", resultadoPromesa); // "Valor de Promise"
    
    // 2. Await con valor no-Promise (se convierte automáticamente)
    const valorDirecto = await 42;
    console.log("Valor directo:", valorDirecto); // 42
    
    // 3. Await con función que devuelve Promise
    function obtenerDatos() {
        return Promise.resolve("Datos obtenidos");
    }
    const datos = await obtenerDatos();
    console.log("Función Promise:", datos); // "Datos obtenidos"
    
    // 4. Await con función async
    async function procesarDatos() {
        return "Datos procesados";
    }
    const procesados = await procesarDatos();
    console.log("Función async:", procesados); // "Datos procesados"
    
    // 5. Await con expresión
    const suma = await (2 + 3);
    console.log("Expresión:", suma); // 5
    
    return "Todos los ejemplos completados";
}

ejemplosAwaitVariados().then(console.log);
```

### Operaciones secuenciales vs paralelas

```javascript
// Simulación de APIs
function consultarAPI(endpoint, tiempo) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(`Datos de ${endpoint}`);
        }, tiempo);
    });
}

// ❌ Operaciones secuenciales (más lento)
async function operacionesSecuenciales() {
    console.log("🐌 Iniciando operaciones secuenciales...");
    const inicio = Date.now();
    
    const usuarios = await consultarAPI("usuarios", 1000);    // 1 segundo
    const productos = await consultarAPI("productos", 1500);  // 1.5 segundos  
    const pedidos = await consultarAPI("pedidos", 800);      // 0.8 segundos
    
    const tiempoTotal = Date.now() - inicio;
    console.log("⏱️ Tiempo total secuencial:", tiempoTotal + "ms"); // ~3300ms
    
    return { usuarios, productos, pedidos };
}

// ✅ Operaciones paralelas (más rápido)
async function operacionesParalelas() {
    console.log("🚀 Iniciando operaciones paralelas...");
    const inicio = Date.now();
    
    // Iniciar todas las operaciones al mismo tiempo
    const promesaUsuarios = consultarAPI("usuarios", 1000);
    const promesaProductos = consultarAPI("productos", 1500);
    const promesaPedidos = consultarAPI("pedidos", 800);
    
    // Esperar a que todas terminen
    const usuarios = await promesaUsuarios;
    const productos = await promesaProductos;
    const pedidos = await promesaPedidos;
    
    const tiempoTotal = Date.now() - inicio;
    console.log("⏱️ Tiempo total paralelo:", tiempoTotal + "ms"); // ~1500ms
    
    return { usuarios, productos, pedidos };
}

// ✅ Usando Promise.all() con async/await (la mejor forma)
async function operacionesConPromiseAll() {
    console.log("⚡ Iniciando operaciones con Promise.all...");
    const inicio = Date.now();
    
    const [usuarios, productos, pedidos] = await Promise.all([
        consultarAPI("usuarios", 1000),
        consultarAPI("productos", 1500),
        consultarAPI("pedidos", 800)
    ]);
    
    const tiempoTotal = Date.now() - inicio;
    console.log("⏱️ Tiempo total Promise.all:", tiempoTotal + "ms"); // ~1500ms
    
    return { usuarios, productos, pedidos };
}

// Comparar rendimiento
async function compararRendimiento() {
    await operacionesSecuenciales();
    await operacionesParalelas();
    await operacionesConPromiseAll();
}

// compararRendimiento();
```

## Manejo de Errores con Try/Catch

### Try/catch básico

```javascript
async function ejemploTryCatch() {
    try {
        // Operación que puede fallar
        const resultado = await operacionQuePuedeFallar();
        console.log("✅ Éxito:", resultado);
        
    } catch (error) {
        // Manejar cualquier error de la operación
        console.error("❌ Error capturado:", error.message);
        
    } finally {
        // Código que siempre se ejecuta
        console.log("🔄 Limpieza completada");
    }
}

function operacionQuePuedeFallar() {
    return new Promise((resolve, reject) => {
        const exito = Math.random() > 0.5;
        
        setTimeout(() => {
            if (exito) {
                resolve("Operación exitosa");
            } else {
                reject(new Error("La operación falló"));
            }
        }, 1000);
    });
}

// Probar el manejo de errores
ejemploTryCatch();
```

### Manejo de errores específicos

```javascript
// Diferentes tipos de errores
class ErrorRed extends Error {
    constructor(message) {
        super(message);
        this.name = "ErrorRed";
        this.code = "NETWORK_ERROR";
    }
}

class ErrorAutorizacion extends Error {
    constructor(message) {
        super(message);
        this.name = "ErrorAutorizacion";
        this.code = "AUTH_ERROR";
    }
}

class ErrorValidacion extends Error {
    constructor(message, campo) {
        super(message);
        this.name = "ErrorValidacion";
        this.code = "VALIDATION_ERROR";
        this.campo = campo;
    }
}

async function operacionConDiferentesErrores(tipoError) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            switch (tipoError) {
                case "red":
                    reject(new ErrorRed("Sin conexión a internet"));
                    break;
                case "auth":
                    reject(new ErrorAutorizacion("Token de acceso inválido"));
                    break;
                case "validacion":
                    reject(new ErrorValidacion("Email inválido", "email"));
                    break;
                case "generico":
                    reject(new Error("Error desconocido"));
                    break;
                default:
                    resolve("Operación exitosa");
            }
        }, 500);
    });
}

async function manejarErroresEspecificos(tipoError) {
    try {
        const resultado = await operacionConDiferentesErrores(tipoError);
        console.log("✅ Éxito:", resultado);
        
    } catch (error) {
        // Manejar diferentes tipos de errores
        if (error instanceof ErrorRed) {
            console.error("🌐 Error de red:", error.message);
            console.log("💡 Sugerencia: Verificar conexión a internet");
            
        } else if (error instanceof ErrorAutorizacion) {
            console.error("🔒 Error de autorización:", error.message);
            console.log("💡 Sugerencia: Volver a iniciar sesión");
            
        } else if (error instanceof ErrorValidacion) {
            console.error("📝 Error de validación:", error.message);
            console.log(`💡 Campo problemático: ${error.campo}`);
            
        } else {
            console.error("❓ Error desconocido:", error.message);
            console.log("💡 Contactar soporte técnico");
        }
        
        // Re-lanzar el error si es necesario
        // throw error;
    }
}

// Probar diferentes tipos de errores
async function probarManejoerrores() {
    const tipos = ["exito", "red", "auth", "validacion", "generico"];
    
    for (const tipo of tipos) {
        console.log(`\n--- Probando: ${tipo} ---`);
        await manejarErroresEspecificos(tipo);
    }
}

// probarManejoerrores();
```

### Try/catch anidados y propagación de errores

```javascript
async function operacionNivel1() {
    console.log("📍 Nivel 1: Iniciando");
    
    try {
        const resultado = await operacionNivel2();
        console.log("📍 Nivel 1: Éxito -", resultado);
        return resultado;
        
    } catch (error) {
        console.error("📍 Nivel 1: Error capturado -", error.message);
        
        // Decidir si manejar aquí o propagar
        if (error.code === "RECOVERABLE") {
            console.log("📍 Nivel 1: Error recuperable, reintentando...");
            return await operacionNivel2(); // Reintentar
        } else {
            console.log("📍 Nivel 1: Error crítico, propagando...");
            throw error; // Propagar error
        }
    }
}

async function operacionNivel2() {
    console.log("📍 Nivel 2: Iniciando");
    
    try {
        const resultado = await operacionNivel3();
        console.log("📍 Nivel 2: Éxito -", resultado);
        return `Procesado en nivel 2: ${resultado}`;
        
    } catch (error) {
        console.error("📍 Nivel 2: Error capturado -", error.message);
        
        // Agregar contexto al error
        const errorConContexto = new Error(`Error en nivel 2: ${error.message}`);
        errorConContexto.originalError = error;
        errorConContexto.code = error.code || "UNKNOWN";
        
        throw errorConContexto;
    }
}

async function operacionNivel3() {
    console.log("📍 Nivel 3: Procesando...");
    
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const random = Math.random();
            
            if (random > 0.7) {
                resolve("Datos procesados exitosamente");
            } else if (random > 0.3) {
                const error = new Error("Error temporal");
                error.code = "RECOVERABLE";
                reject(error);
            } else {
                const error = new Error("Error crítico del sistema");
                error.code = "CRITICAL";
                reject(error);
            }
        }, 1000);
    });
}

async function ejecutarOperacionCompleta() {
    try {
        const resultado = await operacionNivel1();
        console.log("🎉 Operación completada exitosamente:", resultado);
        
    } catch (error) {
        console.error("💥 Error final no manejado:", error.message);
        
        if (error.originalError) {
            console.error("🔍 Error original:", error.originalError.message);
        }
    }
}

// ejecutarOperacionCompleta();
```

## Casos Prácticos Avanzados

### Sistema de autenticación y autorización

```javascript
class AuthService {
    constructor() {
        this.token = localStorage.getItem('authToken');
        this.usuario = null;
    }
    
    // Iniciar sesión
    async iniciarSesion(email, password) {
        try {
            console.log("🔐 Iniciando sesión...");
            
            // Validar datos
            if (!email || !password) {
                throw new Error("Email y contraseña requeridos");
            }
            
            // Simular llamada a API
            const response = await this.simularAPIAuth('/login', {
                email: email,
                password: password
            });
            
            // Guardar token
            this.token = response.token;
            localStorage.setItem('authToken', this.token);
            
            // Obtener datos del usuario
            this.usuario = await this.obtenerPerfilUsuario();
            
            console.log("✅ Sesión iniciada exitosamente");
            return {
                exito: true,
                usuario: this.usuario,
                mensaje: "Bienvenido de vuelta"
            };
            
        } catch (error) {
            console.error("❌ Error iniciando sesión:", error.message);
            
            // Limpiar datos en caso de error
            this.token = null;
            this.usuario = null;
            localStorage.removeItem('authToken');
            
            return {
                exito: false,
                mensaje: error.message
            };
        }
    }
    
    // Obtener perfil del usuario autenticado
    async obtenerPerfilUsuario() {
        try {
            if (!this.token) {
                throw new Error("No hay token de autenticación");
            }
            
            console.log("👤 Obteniendo perfil del usuario...");
            
            const response = await this.simularAPIAuth('/perfil', null, this.token);
            
            console.log("✅ Perfil obtenido:", response.usuario.nombre);
            return response.usuario;
            
        } catch (error) {
            console.error("❌ Error obteniendo perfil:", error.message);
            
            if (error.status === 401) {
                // Token inválido, cerrar sesión
                await this.cerrarSesion();
                throw new Error("Sesión expirada, por favor inicia sesión nuevamente");
            }
            
            throw error;
        }
    }
    
    // Actualizar perfil
    async actualizarPerfil(nuevosDatos) {
        try {
            if (!this.token) {
                throw new Error("Debe iniciar sesión primero");
            }
            
            console.log("📝 Actualizando perfil...");
            
            // Validar datos
            const datosValidados = await this.validarDatosPerfil(nuevosDatos);
            
            // Actualizar en servidor
            const response = await this.simularAPIAuth('/perfil', datosValidados, this.token);
            
            // Actualizar datos locales
            this.usuario = { ...this.usuario, ...response.usuario };
            
            console.log("✅ Perfil actualizado exitosamente");
            return {
                exito: true,
                usuario: this.usuario,
                mensaje: "Perfil actualizado correctamente"
            };
            
        } catch (error) {
            console.error("❌ Error actualizando perfil:", error.message);
            
            return {
                exito: false,
                mensaje: error.message
            };
        }
    }
    
    // Cerrar sesión
    async cerrarSesion() {
        try {
            if (this.token) {
                console.log("🚪 Cerrando sesión...");
                
                // Notificar al servidor (opcional)
                await this.simularAPIAuth('/logout', null, this.token);
            }
            
            // Limpiar datos locales
            this.token = null;
            this.usuario = null;
            localStorage.removeItem('authToken');
            
            console.log("✅ Sesión cerrada exitosamente");
            return true;
            
        } catch (error) {
            console.error("⚠️ Error cerrando sesión:", error.message);
            
            // Limpiar datos locales aunque haya error
            this.token = null;
            this.usuario = null;
            localStorage.removeItem('authToken');
            
            return false;
        }
    }
    
    // Validar datos del perfil
    async validarDatosPerfil(datos) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                const errores = [];
                
                if (datos.nombre && datos.nombre.length < 2) {
                    errores.push("El nombre debe tener al menos 2 caracteres");
                }
                
                if (datos.email && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(datos.email)) {
                    errores.push("Email inválido");
                }
                
                if (datos.edad && (datos.edad < 13 || datos.edad > 120)) {
                    errores.push("Edad debe estar entre 13 y 120 años");
                }
                
                if (errores.length > 0) {
                    reject(new Error(`Errores de validación: ${errores.join(", ")}`));
                } else {
                    resolve(datos);
                }
            }, 300);
        });
    }
    
    // Simular API (reemplazar con fetch real)
    async simularAPIAuth(endpoint, datos = null, token = null) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                // Simular diferentes respuestas
                if (endpoint === '/login') {
                    if (datos.email === 'user@test.com' && datos.password === '123456') {
                        resolve({
                            token: 'jwt_token_' + Date.now(),
                            expiracion: Date.now() + 24 * 60 * 60 * 1000 // 24 horas
                        });
                    } else {
                        reject(new Error("Credenciales inválidas"));
                    }
                    
                } else if (endpoint === '/perfil') {
                    if (!token) {
                        const error = new Error("Token requerido");
                        error.status = 401;
                        reject(error);
                    } else if (token.startsWith('jwt_token_')) {
                        if (datos) {
                            // Actualizar perfil
                            resolve({
                                usuario: {
                                    id: 123,
                                    nombre: datos.nombre || 'Usuario Test',
                                    email: datos.email || 'user@test.com',
                                    edad: datos.edad || 25,
                                    fechaActualizacion: new Date()
                                }
                            });
                        } else {
                            // Obtener perfil
                            resolve({
                                usuario: {
                                    id: 123,
                                    nombre: 'Usuario Test',
                                    email: 'user@test.com',
                                    edad: 25,
                                    fechaRegistro: new Date()
                                }
                            });
                        }
                    } else {
                        const error = new Error("Token inválido");
                        error.status = 401;
                        reject(error);
                    }
                    
                } else if (endpoint === '/logout') {
                    resolve({ mensaje: "Sesión cerrada exitosamente" });
                }
            }, Math.random() * 1000 + 500); // 500-1500ms
        });
    }
    
    // Verificar si está autenticado
    estaAutenticado() {
        return !!this.token && !!this.usuario;
    }
}

// Ejemplo de uso del sistema de autenticación
async function ejemploAuth() {
    const auth = new AuthService();
    
    try {
        console.log("=== EJEMPLO DE AUTENTICACIÓN ===\n");
        
        // Intentar iniciar sesión
        console.log("1. Iniciando sesión...");
        const loginResult = await auth.iniciarSesion('user@test.com', '123456');
        
        if (!loginResult.exito) {
            console.log("❌ Login falló:", loginResult.mensaje);
            return;
        }
        
        console.log("✅ Login exitoso:", loginResult.mensaje);
        console.log("👤 Usuario:", loginResult.usuario.nombre);
        
        // Actualizar perfil
        console.log("\n2. Actualizando perfil...");
        const updateResult = await auth.actualizarPerfil({
            nombre: "Usuario Actualizado",
            edad: 30
        });
        
        if (updateResult.exito) {
            console.log("✅ Perfil actualizado:", updateResult.mensaje);
        } else {
            console.log("❌ Error actualizando:", updateResult.mensaje);
        }
        
        // Cerrar sesión
        console.log("\n3. Cerrando sesión...");
        await auth.cerrarSesion();
        
        console.log("🏁 Ejemplo de autenticación completado");
        
    } catch (error) {
        console.error("💥 Error inesperado:", error.message);
    }
}

// Ejecutar ejemplo
// ejemploAuth();
```

### Sistema de carga de datos con cache y retry

```javascript
class DataLoader {
    constructor(options = {}) {
        this.cache = new Map();
        this.config = {
            cacheTTL: options.cacheTTL || 5 * 60 * 1000, // 5 minutos
            maxRetries: options.maxRetries || 3,
            retryDelay: options.retryDelay || 1000,
            timeout: options.timeout || 10000
        };
        this.requestsEnProceso = new Map();
    }
    
    // Cargar datos con cache, retry y deduplicación
    async cargarDatos(url, opciones = {}) {
        const cacheKey = this.generarCacheKey(url, opciones);
        
        try {
            // 1. Verificar cache
            const datosCacheados = this.obtenerDeCache(cacheKey);
            if (datosCacheados) {
                console.log(`📋 Cache HIT para ${url}`);
                return datosCacheados;
            }
            
            // 2. Verificar si ya hay una request en proceso (deduplicación)
            if (this.requestsEnProceso.has(cacheKey)) {
                console.log(`⏳ Request en proceso para ${url}, esperando...`);
                return await this.requestsEnProceso.get(cacheKey);
            }
            
            // 3. Crear nueva request con retry
            console.log(`🔍 Cache MISS para ${url}, cargando...`);
            const requestPromise = this.cargarConRetry(url, opciones);
            this.requestsEnProceso.set(cacheKey, requestPromise);
            
            // 4. Ejecutar request
            const datos = await requestPromise;
            
            // 5. Guardar en cache y limpiar request en proceso
            this.guardarEnCache(cacheKey, datos);
            this.requestsEnProceso.delete(cacheKey);
            
            return datos;
            
        } catch (error) {
            // Limpiar request en proceso en caso de error
            this.requestsEnProceso.delete(cacheKey);
            throw error;
        }
    }
    
    // Cargar con reintentos automáticos
    async cargarConRetry(url, opciones) {
        let ultimoError;
        
        for (let intento = 1; intento <= this.config.maxRetries; intento++) {
            try {
                console.log(`🔄 Intento ${intento}/${this.config.maxRetries} para ${url}`);
                
                const datos = await this.realizarRequest(url, opciones);
                
                if (intento > 1) {
                    console.log(`✅ Éxito en intento ${intento} para ${url}`);
                }
                
                return datos;
                
            } catch (error) {
                ultimoError = error;
                console.log(`❌ Intento ${intento} falló para ${url}:`, error.message);
                
                // No reintentar en el último intento
                if (intento < this.config.maxRetries) {
                    const delay = this.calcularDelayRetry(intento);
                    console.log(`⏰ Reintentando en ${delay}ms...`);
                    await this.esperar(delay);
                }
            }
        }
        
        throw new Error(`Falló después de ${this.config.maxRetries} intentos: ${ultimoError.message}`);
    }
    
    // Realizar request individual con timeout
    async realizarRequest(url, opciones) {
        const controller = new AbortController();
        const timeoutId = setTimeout(() => controller.abort(), this.config.timeout);
        
        try {
            const response = await fetch(url, {
                ...opciones,
                signal: controller.signal
            });
            
            clearTimeout(timeoutId);
            
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}: ${response.statusText}`);
            }
            
            const datos = await response.json();
            return {
                datos: datos,
                timestamp: Date.now(),
                url: url
            };
            
        } catch (error) {
            clearTimeout(timeoutId);
            
            if (error.name === 'AbortError') {
                throw new Error(`Timeout después de ${this.config.timeout}ms`);
            }
            
            throw error;
        }
    }
    
    // Gestión de cache
    obtenerDeCache(clave) {
        const item = this.cache.get(clave);
        
        if (item && Date.now() - item.timestamp < this.config.cacheTTL) {
            return item.datos;
        }
        
        if (item) {
            this.cache.delete(clave); // Eliminar item expirado
        }
        
        return null;
    }
    
    guardarEnCache(clave, datos) {
        this.cache.set(clave, {
            datos: datos,
            timestamp: Date.now()
        });
        
        console.log(`💾 Datos cacheados para ${clave}`);
    }
    
    limpiarCache() {
        const antes = this.cache.size;
        this.cache.clear();
        console.log(`🗑️ Cache limpiado (${antes} items removidos)`);
    }
    
    // Utilidades
    generarCacheKey(url, opciones) {
        const key = url + JSON.stringify(opciones || {});
        return btoa(key).slice(0, 32); // Base64 truncado
    }
    
    calcularDelayRetry(intento) {
        // Backoff exponencial con jitter
        const baseDelay = this.config.retryDelay;
        const exponential = baseDelay * Math.pow(2, intento - 1);
        const jitter = Math.random() * 1000; // Añadir variabilidad
        return Math.min(exponential + jitter, 30000); // Max 30 segundos
    }
    
    async esperar(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
    
    // Obtener estadísticas
    obtenerEstadisticas() {
        const ahoraMs = Date.now();
        const itemsValidos = Array.from(this.cache.entries())
            .filter(([_, item]) => ahoraMs - item.timestamp < this.config.cacheTTL).length;
            
        return {
            itemsEnCache: this.cache.size,
            itemsValidos: itemsValidos,
            itemsExpirados: this.cache.size - itemsValidos,
            requestsEnProceso: this.requestsEnProceso.size,
            configuracion: this.config
        };
    }
}

// Ejemplo de uso del DataLoader
async function ejemploDataLoader() {
    // Configurar loader
    const loader = new DataLoader({
        cacheTTL: 10000,     // 10 segundos de cache
        maxRetries: 3,
        retryDelay: 500,
        timeout: 5000
    });
    
    // URLs de ejemplo (algunas fallarán para demostrar retry)
    const urls = [
        'https://jsonplaceholder.typicode.com/posts/1',
        'https://jsonplaceholder.typicode.com/posts/2',
        'https://jsonplaceholder.typicode.com/posts/3',
        'https://url-que-falla.ejemplo.com/datos', // Esta fallará
        'https://jsonplaceholder.typicode.com/posts/4'
    ];
    
    console.log("=== EJEMPLO DE DATA LOADER ===\n");
    
    // Cargar datos en paralelo
    const resultados = await Promise.allSettled(
        urls.map(async (url) => {
            try {
                const datos = await loader.cargarDatos(url);
                return { url, exito: true, datos };
            } catch (error) {
                return { url, exito: false, error: error.message };
            }
        })
    );
    
    // Mostrar resultados
    console.log("\n📊 Resultados:");
    resultados.forEach((resultado, index) => {
        if (resultado.status === 'fulfilled') {
            const { url, exito, datos, error } = resultado.value;
            if (exito) {
                console.log(`✅ ${url}: Datos cargados (ID: ${datos.datos.id})`);
            } else {
                console.log(`❌ ${url}: ${error}`);
            }
        }
    });
    
    // Mostrar estadísticas
    console.log("\n📈 Estadísticas del loader:");
    console.table(loader.obtenerEstadisticas());
    
    // Probar cache - cargar de nuevo una URL exitosa
    console.log("\n🔄 Probando cache...");
    const urlParaCache = urls[0];
    const datosCache = await loader.cargarDatos(urlParaCache);
    console.log(`📋 Datos desde cache: ID ${datosCache.datos.id}`);
}

// Ejecutar ejemplo
// ejemploDataLoader();
```

## Patrones Avanzados con Async/Await

### Async/Await con iteradores y generadores

```javascript
// Async Generator para procesamiento de streams
async function* generarDatosAsync() {
    for (let i = 1; i <= 5; i++) {
        // Simular carga asíncrona
        await new Promise(resolve => setTimeout(resolve, 1000));
        
        yield {
            id: i,
            datos: `Datos del item ${i}`,
            timestamp: new Date().toISOString()
        };
    }
}

// Procesar async generator
async function procesarStreamDatos() {
    console.log("🌊 Iniciando procesamiento de stream...");
    
    for await (const item of generarDatosAsync()) {
        console.log(`📦 Item recibido:`, item);
        
        // Procesar cada item individualmente
        await procesarItem(item);
    }
    
    console.log("✅ Stream completado");
}

async function procesarItem(item) {
    // Simular procesamiento
    await new Promise(resolve => setTimeout(resolve, 500));
    console.log(`⚙️ Item ${item.id} procesado`);
}

// Async Iterator personalizado
class AsyncDataIterator {
    constructor(urls) {
        this.urls = urls;
        this.index = 0;
    }
    
    async *[Symbol.asyncIterator]() {
        for (const url of this.urls) {
            try {
                const response = await fetch(url);
                const datos = await response.json();
                yield { url, datos, exito: true };
            } catch (error) {
                yield { url, error: error.message, exito: false };
            }
        }
    }
}

// Usar async iterator
async function ejemploAsyncIterator() {
    const urls = [
        'https://jsonplaceholder.typicode.com/posts/1',
        'https://jsonplaceholder.typicode.com/posts/2',
        'https://jsonplaceholder.typicode.com/posts/3'
    ];
    
    const iterator = new AsyncDataIterator(urls);
    
    for await (const resultado of iterator) {
        if (resultado.exito) {
            console.log(`✅ Datos de ${resultado.url}:`, resultado.datos.title);
        } else {
            console.log(`❌ Error con ${resultado.url}:`, resultado.error);
        }
    }
}

// procesarStreamDatos();
// ejemploAsyncIterator();
```

### Pool de conexiones async

```javascript
class AsyncPool {
    constructor(maxConcurrencia = 3) {
        this.maxConcurrencia = maxConcurrencia;
        this.trabajosActivos = 0;
        this.cola = [];
    }
    
    async ejecutar(funcion, ...args) {
        return new Promise((resolve, reject) => {
            this.cola.push({
                funcion,
                args,
                resolve,
                reject
            });
            
            this.procesarCola();
        });
    }
    
    async procesarCola() {
        if (this.trabajosActivos >= this.maxConcurrencia || this.cola.length === 0) {
            return;
        }
        
        const trabajo = this.cola.shift();
        this.trabajosActivos++;
        
        try {
            const resultado = await trabajo.funcion(...trabajo.args);
            trabajo.resolve(resultado);
        } catch (error) {
            trabajo.reject(error);
        } finally {
            this.trabajosActivos--;
            this.procesarCola(); // Procesar siguiente trabajo
        }
    }
    
    async ejecutarTodos(funciones) {
        const promesas = funciones.map(({ funcion, args }) => 
            this.ejecutar(funcion, ...args)
        );
        
        return await Promise.allSettled(promesas);
    }
    
    obtenerEstado() {
        return {
            trabajosActivos: this.trabajosActivos,
            trabajosEnCola: this.cola.length,
            maxConcurrencia: this.maxConcurrencia
        };
    }
}

// Ejemplo de uso del pool
async function ejemploAsyncPool() {
    const pool = new AsyncPool(2); // Máximo 2 trabajos concurrentes
    
    // Función de trabajo que simula carga pesada
    async function trabajoPesado(id, duracion) {
        console.log(`🏗️ Iniciando trabajo ${id} (${duracion}ms)`);
        
        await new Promise(resolve => setTimeout(resolve, duracion));
        
        console.log(`✅ Trabajo ${id} completado`);
        return `Resultado del trabajo ${id}`;
    }
    
    // Crear múltiples trabajos
    const trabajos = [
        { funcion: trabajoPesado, args: [1, 2000] },
        { funcion: trabajoPesado, args: [2, 1500] },
        { funcion: trabajoPesado, args: [3, 3000] },
        { funcion: trabajoPesado, args: [4, 1000] },
        { funcion: trabajoPesado, args: [5, 2500] }
    ];
    
    console.log("🚀 Ejecutando trabajos con pool (máximo 2 concurrentes)...");
    console.log("📊 Estado inicial:", pool.obtenerEstado());
    
    const resultados = await pool.ejecutarTodos(trabajos);
    
    console.log("🏁 Todos los trabajos completados");
    console.log("📊 Estado final:", pool.obtenerEstado());
    
    // Mostrar resultados
    resultados.forEach((resultado, index) => {
        if (resultado.status === 'fulfilled') {
            console.log(`✅ Trabajo ${index + 1}:`, resultado.value);
        } else {
            console.log(`❌ Trabajo ${index + 1}:`, resultado.reason.message);
        }
    });
}

// ejemploAsyncPool();
```

## Mejores Prácticas con Async/Await

### ✅ Buenas prácticas

```javascript
// 1. Siempre usar try/catch para manejar errores
async function buenaPractica1() {
    try {
        const datos = await operacionAsincrona();
        return datos;
    } catch (error) {
        console.error("Error:", error);
        throw error; // Re-lanzar si es necesario
    }
}

// 2. Ejecutar operaciones independientes en paralelo
async function buenaPractica2() {
    // ✅ Paralelo - más rápido
    const [usuarios, productos, pedidos] = await Promise.all([
        obtenerUsuarios(),
        obtenerProductos(), 
        obtenerPedidos()
    ]);
    
    return { usuarios, productos, pedidos };
}

// 3. Usar Promise.allSettled() cuando algunas operaciones pueden fallar
async function buenaPractica3() {
    const resultados = await Promise.allSettled([
        operacionQuePuedeFallar1(),
        operacionQuePuedeFallar2(),
        operacionQuePuedeFallar3()
    ]);
    
    const exitosos = resultados
        .filter(r => r.status === 'fulfilled')
        .map(r => r.value);
        
    const fallidos = resultados
        .filter(r => r.status === 'rejected')
        .map(r => r.reason.message);
        
    return { exitosos, fallidos };
}

// 4. Crear funciones auxiliares para operaciones complejas
async function operacionCompleja() {
    const datos = await obtenerDatos();
    const validados = await validarDatos(datos);
    const procesados = await procesarDatos(validados);
    return procesados;
}

// 5. Usar async/await en métodos de clase
class MiClase {
    async inicializar() {
        try {
            this.configuracion = await cargarConfiguracion();
            this.datos = await cargarDatos();
            console.log("✅ Clase inicializada");
        } catch (error) {
            console.error("❌ Error inicializando:", error);
            throw error;
        }
    }
}

// 6. Manejar timeouts apropiadamente
async function conTimeout(promesa, timeout = 5000) {
    return Promise.race([
        promesa,
        new Promise((_, reject) => 
            setTimeout(() => reject(new Error('Timeout')), timeout)
        )
    ]);
}
```

### ❌ Prácticas a evitar

```javascript
// ❌ NO usar async sin await (innecesario)
async function malaPractica1() {
    return "valor"; // No necesita async si no usa await
}
// ✅ Mejor
function malaPractica1Corregido() {
    return "valor";
}

// ❌ NO ejecutar operaciones independientes secuencialmente
async function malaPractica2() {
    const usuarios = await obtenerUsuarios();     // 1 segundo
    const productos = await obtenerProductos();   // 1 segundo  
    const pedidos = await obtenerPedidos();       // 1 segundo
    // Total: 3 segundos
    
    return { usuarios, productos, pedidos };
}

// ✅ Mejor - en paralelo
async function malaPractica2Corregido() {
    const [usuarios, productos, pedidos] = await Promise.all([
        obtenerUsuarios(),
        obtenerProductos(),
        obtenerPedidos()
    ]);
    // Total: 1 segundo
    
    return { usuarios, productos, pedidos };
}

// ❌ NO usar await en bucles cuando no es necesario
async function malaPractica3(urls) {
    const resultados = [];
    
    for (const url of urls) {
        const datos = await fetch(url); // Secuencial - lento
        resultados.push(datos);
    }
    
    return resultados;
}

// ✅ Mejor - procesamiento paralelo
async function malaPractica3Corregido(urls) {
    const promesas = urls.map(url => fetch(url));
    const resultados = await Promise.all(promesas);
    return resultados;
}

// ❌ NO olvidar manejar errores
async function malaPractica4() {
    const datos = await operacionQuePuedeFallar(); // Puede lanzar error no capturado
    return datos;
}

// ✅ Mejor - con manejo de errores
async function malaPractica4Corregido() {
    try {
        const datos = await operacionQuePuedeFallar();
        return datos;
    } catch (error) {
        console.error("Error:", error);
        return null; // O manejar apropiadamente
    }
}

// ❌ NO crear promises innecesarias
async function malaPractica5() {
    return new Promise(async (resolve) => {
        const resultado = await operacionAsincrona();
        resolve(resultado); // Innecesario
    });
}

// ✅ Mejor - directo
async function malaPractica5Corregido() {
    return await operacionAsincrona();
    // O mejor aún:
    // return operacionAsincrona();
}
```

## Comparación: Callbacks vs Promises vs Async/Await

```javascript
// Mismo ejemplo implementado con las 3 técnicas

// 1. ❌ Callbacks (Callback Hell)
function obtenerDatosUsuarioCallback(id, callback) {
    obtenerUsuario(id, (error, usuario) => {
        if (error) {
            callback(error, null);
            return;
        }
        
        obtenerPerfil(usuario.id, (error, perfil) => {
            if (error) {
                callback(error, null);
                return;
            }
            
            obtenerConfiguracion(perfil.id, (error, configuracion) => {
                if (error) {
                    callback(error, null);
                    return;
                }
                
                callback(null, {
                    usuario: usuario,
                    perfil: perfil,
                    configuracion: configuracion
                });
            });
        });
    });
}

// 2. ✅ Promises (Mejor)
function obtenerDatosUsuarioPromises(id) {
    return obtenerUsuario(id)
        .then(usuario => {
            return obtenerPerfil(usuario.id)
                .then(perfil => {
                    return obtenerConfiguracion(perfil.id)
                        .then(configuracion => {
                            return {
                                usuario: usuario,
                                perfil: perfil,
                                configuracion: configuracion
                            };
                        });
                });
        })
        .catch(error => {
            console.error("Error:", error);
            throw error;
        });
}

// 3. ✅ Async/Await (El mejor)
async function obtenerDatosUsuarioAsync(id) {
    try {
        const usuario = await obtenerUsuario(id);
        const perfil = await obtenerPerfil(usuario.id);
        const configuracion = await obtenerConfiguracion(perfil.id);
        
        return {
            usuario: usuario,
            perfil: perfil,
            configuracion: configuracion
        };
        
    } catch (error) {
        console.error("Error:", error);
        throw error;
    }
}

// Comparación de uso:
console.log("=== COMPARACIÓN DE TÉCNICAS ===");

// Callbacks
obtenerDatosUsuarioCallback(123, (error, datos) => {
    if (error) {
        console.error("Callback error:", error);
    } else {
        console.log("Callback datos:", datos);
    }
});

// Promises
obtenerDatosUsuarioPromises(123)
    .then(datos => console.log("Promise datos:", datos))
    .catch(error => console.error("Promise error:", error));

// Async/Await
async function usarAsyncAwait() {
    try {
        const datos = await obtenerDatosUsuarioAsync(123);
        console.log("Async/Await datos:", datos);
    } catch (error) {
        console.error("Async/Await error:", error);
    }
}

usarAsyncAwait();
```

***

Async/Await es la evolución natural del manejo asíncrono en JavaScript. Proporciona una sintaxis más limpia y legible que hace que el código asíncrono se comporte de manera similar al código síncrono, manteniendo todas las ventajas de las operaciones no bloqueantes.

> **🚀 Próximos pasos**: Una vez que domines async/await, puedes explorar temas avanzados como Web Workers, Streams API, y patrones de concurrencia más sofisticados.
