# Promises en JavaScript

Las **Promises** (Promesas) son una característica fundamental de JavaScript moderno para manejar operaciones asíncronas de manera más elegante y legible que los callbacks tradicionales. Una Promise representa un valor que puede estar disponible ahora, en el futuro, o nunca.

## Operaciones Síncronas vs Asíncronas

Antes de entender las Promises, es crucial comprender la diferencia entre **operaciones síncronas** y **asíncronas** en JavaScript.

### Operaciones Síncronas

Las **operaciones síncronas** se ejecutan de manera **secuencial** y **bloquean** la ejecución del código hasta completarse. El programa espera a que termine cada operación antes de continuar con la siguiente.

```javascript
// Ejemplo de código síncrono
console.log("1. Inicio");

function operacionSincrona() {
    console.log("2. Procesando...");
    
    // Simulación de trabajo pesado (bloquea el hilo)
    for (let i = 0; i < 1000000000; i++) {
        // Operación que consume tiempo
    }
    
    console.log("3. Procesamiento completado");
    return "Resultado síncrono";
}

const resultado = operacionSincrona(); // El código espera aquí
console.log("4. Resultado:", resultado);
console.log("5. Fin");

// Salida:
// 1. Inicio
// 2. Procesando...
// 3. Procesamiento completado
// 4. Resultado: Resultado síncrono
// 5. Fin
```

**Características de operaciones síncronas:**

- ✅ **Predecibles**: Se ejecutan en el orden exacto que las escribes
- ✅ **Simples de entender**: Flujo lineal de ejecución
- ❌ **Bloquean el hilo principal**: Pueden hacer que la aplicación se "congele"
- ❌ **Ineficientes para operaciones lentas**: Como consultas a base de datos o APIs

### Operaciones Asíncronas

Las **operaciones asíncronas** se ejecutan de manera **no bloqueante**. El programa no espera a que terminen, sino que continúa ejecutando el siguiente código mientras la operación asíncrona se procesa en segundo plano.

```javascript
// Ejemplo de código asíncrono
console.log("1. Inicio");

function operacionAsincrona() {
    console.log("2. Iniciando operación asíncrona...");
    
    // setTimeout es asíncrono - no bloquea
    setTimeout(() => {
        console.log("4. Operación asíncrona completada");
    }, 2000);
    
    console.log("3. Operación iniciada, continuando...");
}

operacionAsincrona(); // No bloquea aquí
console.log("5. Fin del programa principal");

// Salida:
// 1. Inicio
// 2. Iniciando operación asíncrona...
// 3. Operación iniciada, continuando...
// 5. Fin del programa principal
// 4. Operación asíncrona completada (después de 2 segundos)
```

**Características de operaciones asíncronas:**

- ✅ **No bloquean**: El programa continúa ejecutándose
- ✅ **Eficientes**: Ideales para operaciones que toman tiempo
- ✅ **Mejor experiencia de usuario**: La interfaz no se congela
- ❌ **Más complejas**: Requieren manejo especial del flujo de datos
- ❌ **Orden de ejecución impredecible**: Los resultados pueden llegar en cualquier momento

### Comparación Práctica: Descarga de Archivos

```javascript
// ❌ Enfoque síncrono (problemático)
console.log("📥 Iniciando descarga síncrona...");

function descargarArchivoSincrono(url) {
    console.log(`🔄 Descargando ${url}...`);
    
    // Simulación de descarga que bloquea (¡MAL!)
    const inicio = Date.now();
    while (Date.now() - inicio < 3000) {
        // Bloquea por 3 segundos
    }
    
    console.log(`✅ ${url} descargado`);
    return `Contenido de ${url}`;
}

// Descargar 3 archivos síncronamente (9 segundos total)
const archivo1 = descargarArchivoSincrono("archivo1.pdf");
const archivo2 = descargarArchivoSincrono("archivo2.pdf"); 
const archivo3 = descargarArchivoSincrono("archivo3.pdf");

console.log("🏁 Todas las descargas síncronas completadas");
console.log("⚠️ La aplicación estuvo congelada por 9 segundos!");

// ✅ Enfoque asíncrono (mejor)
console.log("\n📥 Iniciando descargas asíncronas...");

function descargarArchivoAsincrono(url) {
    console.log(`🔄 Iniciando descarga de ${url}...`);
    
    return new Promise((resolve) => {
        // Simulación de descarga asíncrona
        setTimeout(() => {
            console.log(`✅ ${url} descargado`);
            resolve(`Contenido de ${url}`);
        }, 3000);
    });
}

// Descargar 3 archivos en paralelo (3 segundos total)
Promise.all([
    descargarArchivoAsincrono("archivo1.pdf"),
    descargarArchivoAsincrono("archivo2.pdf"),
    descargarArchivoAsincrono("archivo3.pdf")
])
.then(resultados => {
    console.log("🏁 Todas las descargas asíncronas completadas");
    console.log("🚀 Tiempo total: 3 segundos (¡3x más rápido!)");
    console.log("✨ La aplicación nunca se congeló");
});

console.log("📱 El programa continúa ejecutándose mientras se descargan los archivos...");
```

### Casos de Uso Comunes

#### Operaciones Síncronas (apropiadas para)

```javascript
// Cálculos matemáticos simples
function calcularArea(radio) {
    return Math.PI * radio * radio;
}

// Validaciones de datos
function validarEmail(email) {
    return email.includes('@') && email.includes('.');
}

// Transformaciones de arrays pequeños
const numeros = [1, 2, 3, 4, 5];
const duplicados = numeros.map(n => n * 2);

// Acceso a localStorage
const configuracion = localStorage.getItem('config');
```

#### Operaciones Asíncronas (necesarias para)

```javascript
// Consultas a APIs
fetch('https://api.ejemplo.com/datos')
    .then(response => response.json())
    .then(datos => console.log(datos));

// Operaciones de base de datos
database.query('SELECT * FROM usuarios')
    .then(usuarios => procesarUsuarios(usuarios));

// Lectura/escritura de archivos
fs.readFile('archivo.txt', (error, contenido) => {
    if (!error) console.log(contenido);
});

// Temporizadores
setTimeout(() => {
    console.log("Ejecutado después de 2 segundos");
}, 2000);

// Animaciones
element.animate({ opacity: 0 }, 1000)
    .finished.then(() => {
        console.log("Animación completada");
    });
```

### El Problema del "Callback Hell"

Antes de las Promises, las operaciones asíncronas se manejaban con **callbacks**, lo que llevaba al temido "Callback Hell":

```javascript
// ❌ Callback Hell (difícil de leer y mantener)
obtenerUsuario(userId, (usuario, error) => {
    if (error) {
        console.error("Error obteniendo usuario:", error);
    } else {
        obtenerPerfil(usuario.id, (perfil, error) => {
            if (error) {
                console.error("Error obteniendo perfil:", error);
            } else {
                obtenerPreferencias(perfil.id, (preferencias, error) => {
                    if (error) {
                        console.error("Error obteniendo preferencias:", error);
                    } else {
                        actualizarUI(usuario, perfil, preferencias, (success, error) => {
                            if (error) {
                                console.error("Error actualizando UI:", error);
                            } else {
                                console.log("¡Todo completado exitosamente!");
                            }
                        });
                    }
                });
            }
        });
    }
});

// ✅ Con Promises (mucho más limpio)
obtenerUsuario(userId)
    .then(usuario => obtenerPerfil(usuario.id))
    .then(perfil => obtenerPreferencias(perfil.id))
    .then(preferencias => actualizarUI(usuario, perfil, preferencias))
    .then(() => console.log("¡Todo completado exitosamente!"))
    .catch(error => console.error("Error en el proceso:", error));
```

### ¿Por qué necesitamos Promises?

Las Promises resuelven los principales problemas del código asíncrono tradicional:

1. **📚 Legibilidad**: Código más limpio y fácil de entender
2. **🔧 Mantenibilidad**: Más fácil de modificar y debuggear  
3. **🎯 Control de errores**: Manejo centralizado de errores con `.catch()`
4. **🔄 Composición**: Fácil combinación de múltiples operaciones asíncronas
5. **⚡ Rendimiento**: Mejor gestión de operaciones paralelas con `Promise.all()`

```javascript
// Ejemplo: Las Promises nos permiten este flujo elegante
console.log("🚀 Iniciando proceso de compra...");

validarCarrito(carritoId)
    .then(carrito => {
        console.log("✅ Carrito válido");
        return procesarPago(carrito.total);
    })
    .then(pagoConfirmado => {
        console.log("✅ Pago procesado");
        return generarFactura(pagoConfirmado.transaccionId);
    })
    .then(factura => {
        console.log("✅ Factura generada");
        return enviarEmailConfirmacion(factura);
    })
    .then(emailEnviado => {
        console.log("✅ Email enviado");
        console.log("🎉 ¡Compra completada exitosamente!");
    })
    .catch(error => {
        console.error("❌ Error en el proceso de compra:", error.message);
        // Manejar error (reembolso, notificación, etc.)
    })
    .finally(() => {
        console.log("🔄 Proceso de compra finalizado");
    });

console.log("📱 La aplicación sigue funcionando mientras se procesa la compra...");
```

## ¿Qué son las Promises?

Una **Promise** es un objeto que representa la eventual finalización (o falla) de una operación asíncrona y su valor resultante. Es como un "contrato" que garantiza que se ejecutará alguna acción cuando se complete una tarea asíncrona.

### Estados de una Promise

Una Promise puede estar en uno de tres estados:

1. **Pending** (Pendiente): Estado inicial, ni cumplida ni rechazada
2. **Fulfilled** (Cumplida): La operación se completó exitosamente
3. **Rejected** (Rechazada): La operación falló

```javascript
// Ejemplo visual de los estados
console.log("Estado de Promise:");

const promiseEjemplo = new Promise((resolve, reject) => {
    console.log("Estado: Pending"); // Se ejecuta inmediatamente
    
    setTimeout(() => {
        const exito = Math.random() > 0.5;
        
        if (exito) {
            resolve("¡Operación exitosa!");     // Estado: Fulfilled
        } else {
            reject("Error en la operación");    // Estado: Rejected
        }
    }, 2000);
});

promiseEjemplo
    .then(resultado => console.log("Estado: Fulfilled -", resultado))
    .catch(error => console.log("Estado: Rejected -", error));
```

## Sintaxis Básica

### Crear una Promise

```javascript
// Sintaxis básica
const miPromise = new Promise((resolve, reject) => {
    // Lógica asíncrona aquí
    
    // Si todo sale bien:
    // resolve(valor);
    
    // Si algo sale mal:
    // reject(error);
});

// Ejemplo práctico: simular una API
function obtenerDatosUsuario(id) {
    return new Promise((resolve, reject) => {
        // Simular tiempo de respuesta de API
        setTimeout(() => {
            if (id > 0) {
                const usuario = {
                    id: id,
                    nombre: `Usuario ${id}`,
                    email: `usuario${id}@email.com`,
                    activo: true
                };
                resolve(usuario); // Operación exitosa
            } else {
                reject(new Error("ID de usuario inválido")); // Operación fallida
            }
        }, 1500);
    });
}

// Usar la promise
obtenerDatosUsuario(123)
    .then(usuario => {
        console.log("Usuario obtenido:", usuario);
        // { id: 123, nombre: "Usuario 123", email: "usuario123@email.com", activo: true }
    })
    .catch(error => {
        console.error("Error:", error.message);
    });

// Caso con error
obtenerDatosUsuario(-1)
    .then(usuario => {
        console.log("No se ejecutará");
    })
    .catch(error => {
        console.error("Error capturado:", error.message); // "ID de usuario inválido"
    });
```

### Consumir Promises con then() y catch()

```javascript
// Ejemplo: verificar disponibilidad de producto
function verificarStock(producto, cantidad) {
    return new Promise((resolve, reject) => {
        console.log(`Verificando stock de ${producto}...`);
        
        setTimeout(() => {
            const stockDisponible = Math.floor(Math.random() * 20) + 1; // 1-20 unidades
            
            if (stockDisponible >= cantidad) {
                resolve({
                    producto: producto,
                    solicitado: cantidad,
                    disponible: stockDisponible,
                    mensaje: `Stock suficiente para ${producto}`
                });
            } else {
                reject({
                    producto: producto,
                    solicitado: cantidad,
                    disponible: stockDisponible,
                    mensaje: `Stock insuficiente para ${producto}`
                });
            }
        }, 1000);
    });
}

// Usar then() para manejar éxito
verificarStock("Laptop", 2)
    .then(resultado => {
        console.log("✅ Éxito:", resultado.mensaje);
        console.log(`Solicitado: ${resultado.solicitado}, Disponible: ${resultado.disponible}`);
        
        // Continuar con siguiente paso
        return `Reservando ${resultado.solicitado} unidades de ${resultado.producto}`;
    })
    .then(reserva => {
        console.log("📦", reserva);
    })
    .catch(error => {
        console.error("❌ Error:", error.mensaje);
        console.log(`Solo hay ${error.disponible} unidades disponibles`);
    })
    .finally(() => {
        console.log("🔄 Verificación de stock completada");
    });
```

## Métodos de Promise

### Promise.resolve() y Promise.reject()

```javascript
// Promise.resolve() - crear promise ya resuelta
const promiseResuelta = Promise.resolve("Valor inmediato");
promiseResuelta.then(valor => console.log(valor)); // "Valor inmediato"

// Promise.reject() - crear promise ya rechazada
const promiseRechazada = Promise.reject("Error inmediato");
promiseRechazada.catch(error => console.log(error)); // "Error inmediato"

// Ejemplos prácticos
function validarEmail(email) {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    
    if (regex.test(email)) {
        return Promise.resolve({
            email: email,
            valido: true,
            mensaje: "Email válido"
        });
    } else {
        return Promise.reject({
            email: email,
            valido: false,
            mensaje: "Email inválido"
        });
    }
}

// Uso
validarEmail("user@example.com")
    .then(resultado => console.log("✅", resultado.mensaje))
    .catch(error => console.log("❌", error.mensaje));

validarEmail("email-invalido")
    .then(resultado => console.log("✅", resultado.mensaje))
    .catch(error => console.log("❌", error.mensaje)); // "Email inválido"
```

### Promise.all() - Ejecutar múltiples promises en paralelo

```javascript
// Promise.all() espera a que TODAS las promises se resuelvan
function descargarArchivo(nombre, tamaño) {
    return new Promise((resolve) => {
        const tiempoDescarga = tamaño * 100; // Simular tiempo basado en tamaño
        
        setTimeout(() => {
            resolve({
                archivo: nombre,
                tamaño: tamaño,
                descargado: true,
                tiempo: tiempoDescarga
            });
        }, tiempoDescarga);
    });
}

// Descargar múltiples archivos en paralelo
const descargas = [
    descargarArchivo("documento.pdf", 5),
    descargarArchivo("imagen.jpg", 3),
    descargarArchivo("video.mp4", 15)
];

console.log("🚀 Iniciando descargas en paralelo...");
const inicioDescargas = Date.now();

Promise.all(descargas)
    .then(resultados => {
        const tiempoTotal = Date.now() - inicioDescargas;
        console.log("✅ Todas las descargas completadas");
        
        resultados.forEach(resultado => {
            console.log(`📁 ${resultado.archivo} (${resultado.tamaño}MB) - ${resultado.tiempo}ms`);
        });
        
        console.log(`⏱️ Tiempo total: ${tiempoTotal}ms`);
    })
    .catch(error => {
        console.error("❌ Error en alguna descarga:", error);
    });

// Promise.all() con manejo de errores
function operacionConFalla(id, fallaEn) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (id === fallaEn) {
                reject(`Error en operación ${id}`);
            } else {
                resolve(`Operación ${id} completada`);
            }
        }, 1000);
    });
}

Promise.all([
    operacionConFalla(1, 999), // Exitosa
    operacionConFalla(2, 2),   // Fallará
    operacionConFalla(3, 999)  // Exitosa (pero no se ejecutará el then)
])
.then(resultados => {
    console.log("Todas exitosas:", resultados);
})
.catch(error => {
    console.error("Alguna falló:", error); // "Error en operación 2"
});
```

### Promise.allSettled() - Esperar todas las promises sin importar el resultado

```javascript
// Promise.allSettled() espera a que todas terminen (exitosas o fallidas)
function procesarPedido(id, fallaEn = null) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (id === fallaEn) {
                reject(`Pedido ${id} falló`);
            } else {
                resolve(`Pedido ${id} procesado correctamente`);
            }
        }, Math.random() * 2000 + 500);
    });
}

const pedidos = [
    procesarPedido(1001),
    procesarPedido(1002, 1002), // Este fallará
    procesarPedido(1003),
    procesarPedido(1004, 1004), // Este también fallará
    procesarPedido(1005)
];

console.log("🔄 Procesando todos los pedidos...");

Promise.allSettled(pedidos)
    .then(resultados => {
        console.log("\n📊 Resumen de procesamiento:");
        
        let exitosos = 0;
        let fallidos = 0;
        
        resultados.forEach((resultado, index) => {
            const pedidoId = 1001 + index;
            
            if (resultado.status === 'fulfilled') {
                exitosos++;
                console.log(`✅ Pedido ${pedidoId}: ${resultado.value}`);
            } else {
                fallidos++;
                console.log(`❌ Pedido ${pedidoId}: ${resultado.reason}`);
            }
        });
        
        console.log(`\n📈 Total: ${exitosos} exitosos, ${fallidos} fallidos`);
    });
```

### Promise.race() - La primera promise que se complete

```javascript
// Promise.race() se resuelve con la primera promise que termine
function servidor(nombre, latencia) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({
                servidor: nombre,
                datos: `Datos desde ${nombre}`,
                latencia: latencia
            });
        }, latencia);
    });
}

// Competencia entre servidores
const servidores = [
    servidor("Servidor-US", 800),
    servidor("Servidor-EU", 600),
    servidor("Servidor-ASIA", 1200)
];

console.log("🏁 Iniciando competencia de servidores...");

Promise.race(servidores)
    .then(ganador => {
        console.log("🏆 Servidor más rápido:", ganador.servidor);
        console.log("📡 Datos:", ganador.datos);
        console.log("⚡ Latencia:", ganador.latencia + "ms");
    });

// Timeout con Promise.race()
function operacionLenta() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("Operación completada después de 5 segundos");
        }, 5000);
    });
}

function timeout(ms) {
    return new Promise((_, reject) => {
        setTimeout(() => {
            reject(new Error(`Timeout después de ${ms}ms`));
        }, ms);
    });
}

// Limitar tiempo máximo de espera
Promise.race([
    operacionLenta(),
    timeout(3000) // Timeout de 3 segundos
])
.then(resultado => {
    console.log("✅ Completado:", resultado);
})
.catch(error => {
    console.log("⏰ Timeout:", error.message); // Se ejecutará este
});
```

## Cadenas de Promises (Promise Chaining)

```javascript
// Ejemplo: proceso de registro de usuario
function validarDatos(datos) {
    return new Promise((resolve, reject) => {
        console.log("1️⃣ Validando datos...");
        
        setTimeout(() => {
            if (datos.email && datos.password && datos.password.length >= 6) {
                resolve({
                    ...datos,
                    validado: true
                });
            } else {
                reject(new Error("Datos inválidos"));
            }
        }, 500);
    });
}

function verificarEmailUnico(userData) {
    return new Promise((resolve, reject) => {
        console.log("2️⃣ Verificando email único...");
        
        setTimeout(() => {
            // Simular verificación en base de datos
            const emailExiste = userData.email === "admin@test.com";
            
            if (!emailExiste) {
                resolve({
                    ...userData,
                    emailUnico: true
                });
            } else {
                reject(new Error("Email ya registrado"));
            }
        }, 800);
    });
}

function crearUsuario(userData) {
    return new Promise((resolve) => {
        console.log("3️⃣ Creando usuario en base de datos...");
        
        setTimeout(() => {
            const nuevoUsuario = {
                ...userData,
                id: Math.random().toString(36).substr(2, 9),
                fechaRegistro: new Date(),
                activo: true
            };
            
            resolve(nuevoUsuario);
        }, 1000);
    });
}

function enviarEmailConfirmacion(usuario) {
    return new Promise((resolve) => {
        console.log("4️⃣ Enviando email de confirmación...");
        
        setTimeout(() => {
            resolve({
                ...usuario,
                emailEnviado: true,
                codigoConfirmacion: Math.random().toString(36).substr(2, 6).toUpperCase()
            });
        }, 600);
    });
}

// Cadena de promises para registro completo
const datosRegistro = {
    nombre: "Juan Pérez",
    email: "juan@example.com",
    password: "123456789"
};

console.log("🚀 Iniciando proceso de registro...");

validarDatos(datosRegistro)
    .then(datosValidados => {
        console.log("✅ Datos válidos");
        return verificarEmailUnico(datosValidados);
    })
    .then(datosVerificados => {
        console.log("✅ Email único");
        return crearUsuario(datosVerificados);
    })
    .then(usuarioCreado => {
        console.log("✅ Usuario creado con ID:", usuarioCreado.id);
        return enviarEmailConfirmacion(usuarioCreado);
    })
    .then(usuarioFinal => {
        console.log("✅ Registro completado exitosamente!");
        console.log("👤 Usuario:", usuarioFinal.nombre);
        console.log("📧 Email:", usuarioFinal.email);
        console.log("🔑 Código confirmación:", usuarioFinal.codigoConfirmacion);
    })
    .catch(error => {
        console.error("❌ Error en el registro:", error.message);
    })
    .finally(() => {
        console.log("🔄 Proceso de registro finalizado");
    });

// Ejemplo con transformación de datos en cada paso
function obtenerPedido(id) {
    return Promise.resolve({
        id: id,
        productos: ["laptop", "mouse"],
        estado: "pendiente"
    });
}

function calcularTotal(pedido) {
    return new Promise((resolve) => {
        setTimeout(() => {
            const precios = { laptop: 1000, mouse: 50 };
            const total = pedido.productos.reduce((sum, producto) => {
                return sum + (precios[producto] || 0);
            }, 0);
            
            resolve({
                ...pedido,
                total: total,
                moneda: "EUR"
            });
        }, 500);
    });
}

function aplicarDescuento(pedido) {
    return Promise.resolve({
        ...pedido,
        descuento: pedido.total > 500 ? 50 : 0,
        totalFinal: pedido.total > 500 ? pedido.total - 50 : pedido.total
    });
}

function generarFactura(pedido) {
    return Promise.resolve({
        ...pedido,
        facturaId: `FAC-${Date.now()}`,
        fechaFactura: new Date().toLocaleDateString()
    });
}

// Cadena con transformaciones
obtenerPedido(12345)
    .then(pedido => {
        console.log("📦 Pedido obtenido:", pedido);
        return calcularTotal(pedido);
    })
    .then(pedidoConTotal => {
        console.log("💰 Total calculado:", pedidoConTotal.total + " " + pedidoConTotal.moneda);
        return aplicarDescuento(pedidoConTotal);
    })
    .then(pedidoConDescuento => {
        console.log("🏷️ Descuento aplicado:", pedidoConDescuento.descuento + " " + pedidoConDescuento.moneda);
        console.log("💯 Total final:", pedidoConDescuento.totalFinal + " " + pedidoConDescuento.moneda);
        return generarFactura(pedidoConDescuento);
    })
    .then(facturaFinal => {
        console.log("📄 Factura generada:", facturaFinal.facturaId);
        console.log("📅 Fecha:", facturaFinal.fechaFactura);
        return facturaFinal;
    });
```

## Manejo de Errores con Promises

```javascript
// Manejo de errores en diferentes puntos de la cadena
function paso1() {
    return Promise.resolve("Paso 1 completado");
}

function paso2() {
    return Promise.reject(new Error("Error en paso 2"));
}

function paso3() {
    return Promise.resolve("Paso 3 completado");
}

// Ejemplo 1: Error detiene la cadena
paso1()
    .then(resultado1 => {
        console.log("✅", resultado1);
        return paso2(); // Este fallará
    })
    .then(resultado2 => {
        console.log("✅", resultado2); // No se ejecutará
        return paso3();
    })
    .then(resultado3 => {
        console.log("✅", resultado3); // No se ejecutará
    })
    .catch(error => {
        console.error("❌ Error capturado:", error.message); // "Error en paso 2"
    });

// Ejemplo 2: Manejar errores específicos y continuar
function procesarConRecuperacion() {
    return paso1()
        .then(resultado1 => {
            console.log("✅", resultado1);
            return paso2().catch(error => {
                console.log("⚠️ Error en paso 2, usando valor por defecto");
                return "Valor por defecto para paso 2";
            });
        })
        .then(resultado2 => {
            console.log("✅", resultado2); // Se ejecutará con valor por defecto
            return paso3();
        })
        .then(resultado3 => {
            console.log("✅", resultado3); // Se ejecutará normalmente
        })
        .catch(error => {
            console.error("❌ Error final:", error.message);
        });
}

procesarConRecuperacion();

// Ejemplo 3: Múltiples puntos de error
function operacionCompleja(datos) {
    return new Promise((resolve, reject) => {
        if (!datos) {
            reject(new Error("Datos requeridos"));
            return;
        }
        
        if (!datos.tipo) {
            reject(new Error("Tipo de datos requerido"));
            return;
        }
        
        if (datos.tipo === "especial") {
            reject(new Error("Tipo especial no soportado"));
            return;
        }
        
        resolve(`Procesado: ${datos.tipo}`);
    });
}

// Casos de prueba
const casosPrueba = [
    null,
    {},
    { tipo: "especial" },
    { tipo: "normal" }
];

casosPrueba.forEach((caso, index) => {
    console.log(`\n--- Caso ${index + 1} ---`);
    
    operacionCompleja(caso)
        .then(resultado => {
            console.log("✅ Éxito:", resultado);
        })
        .catch(error => {
            console.log("❌ Error:", error.message);
        });
});

// Ejemplo 4: Re-lanzar errores después de logging
function operacionConLogging(data) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (data.valido) {
                resolve(`Datos procesados: ${data.valor}`);
            } else {
                reject(new Error(`Datos inválidos: ${data.valor}`));
            }
        }, 500);
    });
}

function procesarConLogging(data) {
    return operacionConLogging(data)
        .catch(error => {
            // Log del error
            console.log(`📝 Log: Error procesando ${data.valor} - ${error.message}`);
            
            // Re-lanzar el error para que lo maneje el siguiente catch
            throw error;
        });
}

// Usar función con logging
procesarConLogging({ valido: false, valor: "datos incorrectos" })
    .then(resultado => {
        console.log("✅", resultado);
    })
    .catch(error => {
        console.log("🔥 Error final manejado:", error.message);
    });
```

## Casos Prácticos Avanzados

### Sistema de cache con Promises

```javascript
class CacheConPromises {
    constructor(ttl = 60000) { // TTL por defecto: 1 minuto
        this.cache = new Map();
        this.ttl = ttl;
    }
    
    obtener(clave, funcionCarga) {
        const item = this.cache.get(clave);
        
        // Verificar si existe y no ha expirado
        if (item && Date.now() - item.timestamp < this.ttl) {
            console.log(`📋 Cache HIT para ${clave}`);
            return Promise.resolve(item.valor);
        }
        
        console.log(`🔍 Cache MISS para ${clave}, cargando...`);
        
        // Cargar datos y cachear
        return funcionCarga()
            .then(valor => {
                this.cache.set(clave, {
                    valor: valor,
                    timestamp: Date.now()
                });
                console.log(`💾 Datos cacheados para ${clave}`);
                return valor;
            });
    }
    
    limpiar() {
        this.cache.clear();
        console.log("🗑️ Cache limpiado");
    }
    
    obtenerEstadisticas() {
        const ahora = Date.now();
        const validos = Array.from(this.cache.entries())
            .filter(([_, item]) => ahora - item.timestamp < this.ttl).length;
            
        return {
            totalItems: this.cache.size,
            itemsValidos: validos,
            itemsExpirados: this.cache.size - validos
        };
    }
}

// Simular API lenta
function consultarAPI(endpoint) {
    return new Promise((resolve) => {
        console.log(`🌐 Consultando API: ${endpoint}`);
        
        setTimeout(() => {
            const datos = {
                endpoint: endpoint,
                datos: `Datos de ${endpoint}`,
                timestamp: new Date().toISOString(),
                numeroRandom: Math.floor(Math.random() * 1000)
            };
            resolve(datos);
        }, 2000); // 2 segundos de latencia simulada
    });
}

// Usar el cache
const cache = new CacheConPromises(5000); // 5 segundos TTL

async function ejemploCache() {
    console.log("=== EJEMPLO DE CACHE CON PROMISES ===\n");
    
    // Primera consulta (cache miss)
    console.log("1. Primera consulta a /usuarios");
    const datos1 = await cache.obtener("usuarios", () => consultarAPI("/usuarios"));
    console.log("Resultado:", datos1.numeroRandom, "\n");
    
    // Segunda consulta inmediata (cache hit)
    console.log("2. Segunda consulta inmediata a /usuarios");
    const datos2 = await cache.obtener("usuarios", () => consultarAPI("/usuarios"));
    console.log("Resultado:", datos2.numeroRandom, "\n");
    
    // Consulta diferente endpoint
    console.log("3. Consulta a /productos");
    const datos3 = await cache.obtener("productos", () => consultarAPI("/productos"));
    console.log("Resultado:", datos3.numeroRandom, "\n");
    
    // Estadísticas
    console.log("📊 Estadísticas:", cache.obtenerEstadisticas(), "\n");
    
    // Esperar expiración
    console.log("4. Esperando expiración del cache (6 segundos)...");
    await new Promise(resolve => setTimeout(resolve, 6000));
    
    // Consulta después de expiración (cache miss)
    console.log("5. Consulta después de expiración");
    const datos4 = await cache.obtener("usuarios", () => consultarAPI("/usuarios"));
    console.log("Resultado:", datos4.numeroRandom, "\n");
    
    console.log("📊 Estadísticas finales:", cache.obtenerEstadisticas());
}

// Ejecutar ejemplo (descomenta para probar)
// ejemploCache().catch(console.error);
```

### Retry automático con Promises

```javascript
function retry(funcion, maxIntentos = 3, delay = 1000) {
    return new Promise((resolve, reject) => {
        let intentos = 0;
        
        function ejecutarIntento() {
            intentos++;
            console.log(`🔄 Intento ${intentos}/${maxIntentos}`);
            
            funcion()
                .then(resolve) // Si tiene éxito, resolvemos
                .catch(error => {
                    console.log(`❌ Intento ${intentos} falló:`, error.message);
                    
                    if (intentos >= maxIntentos) {
                        reject(new Error(`Falló después de ${maxIntentos} intentos. Último error: ${error.message}`));
                    } else {
                        console.log(`⏰ Reintentando en ${delay}ms...`);
                        setTimeout(ejecutarIntento, delay);
                    }
                });
        }
        
        ejecutarIntento();
    });
}

// Función que falla ocasionalmente
function operacionInestable() {
    return new Promise((resolve, reject) => {
        const exito = Math.random() > 0.7; // 30% probabilidad de éxito
        
        setTimeout(() => {
            if (exito) {
                resolve("¡Operación exitosa!");
            } else {
                reject(new Error("Falla temporal del servicio"));
            }
        }, 500);
    });
}

// Ejemplo de uso del retry
console.log("🚀 Iniciando operación con retry automático...");

retry(operacionInestable, 5, 1500) // 5 intentos, 1.5 segundos entre intentos
    .then(resultado => {
        console.log("✅ Éxito final:", resultado);
    })
    .catch(error => {
        console.log("🔥 Fallo definitivo:", error.message);
    });

// Retry con backoff exponencial
function retryConBackoff(funcion, maxIntentos = 3, delayBase = 1000) {
    return new Promise((resolve, reject) => {
        let intentos = 0;
        
        function ejecutarIntento() {
            intentos++;
            const delay = delayBase * Math.pow(2, intentos - 1); // Backoff exponencial
            
            console.log(`🔄 Intento ${intentos}/${maxIntentos}`);
            
            funcion()
                .then(resolve)
                .catch(error => {
                    console.log(`❌ Intento ${intentos} falló:`, error.message);
                    
                    if (intentos >= maxIntentos) {
                        reject(new Error(`Falló después de ${maxIntentos} intentos`));
                    } else {
                        console.log(`⏰ Reintentando en ${delay}ms (backoff exponencial)...`);
                        setTimeout(ejecutarIntento, delay);
                    }
                });
        }
        
        ejecutarIntento();
    });
}
```

## Mejores Prácticas con Promises

### ✅ Buenas prácticas

```javascript
// 1. Siempre manejar errores
function buenaPractica1() {
    return fetch('/api/datos')
        .then(response => response.json())
        .then(datos => procesarDatos(datos))
        .catch(error => {
            console.error('Error:', error);
            // Manejar error apropiadamente
            throw error; // Re-lanzar si es necesario
        });
}

// 2. Evitar el "callback hell" usando promises encadenadas
function buenaPractica2() {
    return obtenerUsuario()
        .then(usuario => obtenerPerfil(usuario.id))
        .then(perfil => obtenerPreferencias(perfil.id))
        .then(preferencias => ({
            usuario: usuario,
            preferencias: preferencias
        }));
}

// 3. Usar Promise.all() para operaciones paralelas
function buenaPractica3() {
    const promises = [
        obtenerDatosUsuario(),
        obtenerConfiguracion(),
        obtenerPermisos()
    ];
    
    return Promise.all(promises)
        .then(([usuario, config, permisos]) => {
            return { usuario, config, permisos };
        });
}

// 4. Crear promises reutilizables
function crearTimeout(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

function conTimeout(promise, ms) {
    return Promise.race([
        promise,
        crearTimeout(ms).then(() => Promise.reject(new Error('Timeout')))
    ]);
}
```

### ❌ Prácticas a evitar

```javascript
// 1. NO crear promises innecesarias
// ❌ Malo
function malasPracticas1() {
    return new Promise((resolve) => {
        resolve(valor); // Innecesario
    });
}

// ✅ Mejor
function malasPracticas1Corregido() {
    return Promise.resolve(valor);
}

// 2. NO anidar promises (Promise hell)
// ❌ Malo
function malasPracticas2() {
    return obtenerDatos()
        .then(datos => {
            return procesarDatos(datos)
                .then(resultado => {
                    return guardarResultado(resultado)
                        .then(guardado => {
                            return enviarNotificacion(guardado);
                        });
                });
        });
}

// ✅ Mejor
function malasPracticas2Corregido() {
    return obtenerDatos()
        .then(datos => procesarDatos(datos))
        .then(resultado => guardarResultado(resultado))
        .then(guardado => enviarNotificacion(guardado));
}

// 3. NO olvidar return en las cadenas
// ❌ Malo
function malasPracticas3() {
    return obtenerDatos()
        .then(datos => {
            procesarDatos(datos); // Falta return!
        })
        .then(resultado => {
            // resultado será undefined
            console.log(resultado);
        });
}

// ✅ Mejor
function malasPracticas3Corregido() {
    return obtenerDatos()
        .then(datos => {
            return procesarDatos(datos); // Con return
        })
        .then(resultado => {
            console.log(resultado); // Ahora tendrá valor
        });
}

// 4. NO usar promises para operaciones síncronas
// ❌ Malo
function malasPracticas4(numero) {
    return new Promise((resolve) => {
        const resultado = numero * 2; // Operación síncrona
        resolve(resultado);
    });
}

// ✅ Mejor
function malasPracticas4Corregido(numero) {
    return numero * 2; // Directamente síncrono
    // O si necesitas una promise:
    // return Promise.resolve(numero * 2);
}
```

***

Las Promises son fundamentales en JavaScript moderno para manejar operaciones asíncronas de manera elegante. Permiten escribir código más legible y mantenible que los callbacks tradicionales, y son la base para características más avanzadas como async/await.

> **🚀 Siguiente paso**: Una vez que domines las Promises, puedes avanzar a [`async/await`](./async-await.md) que proporciona una sintaxis aún más limpia para trabajar con código asíncrono.
