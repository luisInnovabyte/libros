# API de Notificaciones en JavaScript

La **Notifications API** permite a las aplicaciones web mostrar notificaciones del sistema al usuario, incluso cuando la página no está visible o activa. Esta API es fundamental para crear experiencias web modernas que mantienen a los usuarios informados sobre eventos importantes.

## ¿Qué son las Notificaciones Web?

Las **notificaciones web** son mensajes que aparecen fuera del contexto de la página web, típicamente en el área de notificaciones del sistema operativo. Permiten que las aplicaciones web comuniquen información importante al usuario sin requerir que la página esté en primer plano.

### Características principales

- **📱 Nativas del sistema**: Aparecen como notificaciones del SO
- **🔔 Persistentes**: Pueden permanecer visibles hasta que el usuario las cierre
- **⚡ Instantáneas**: Se muestran inmediatamente al ser creadas
- **🎯 Interactivas**: Pueden incluir acciones y responder a clics
- **🔒 Seguras**: Requieren permiso explícito del usuario

```javascript
// Ejemplo básico de notificación
if ("Notification" in window) {
    // Verificar permisos
    if (Notification.permission === "granted") {
        // Crear notificación simple
        new Notification("¡Hola!", {
            body: "Esta es tu primera notificación web",
            icon: "https://via.placeholder.com/64"
        });
    }
}
```

## Verificación de Soporte y Permisos

### Comprobar soporte del navegador

```javascript
function verificarSoporteNotificaciones() {
    if (!("Notification" in window)) {
        console.log("❌ Este navegador no soporta notificaciones");
        return false;
    }
    
    if (!("serviceWorker" in navigator)) {
        console.log("⚠️ Service Workers no disponibles (necesarios para notificaciones avanzadas)");
    }
    
    console.log("✅ Notificaciones soportadas");
    return true;
}

// Verificar soporte
const soporteDisponible = verificarSoporteNotificaciones();

if (soporteDisponible) {
    console.log("🚀 Puedes usar notificaciones en este navegador");
}
```

### Estados de permisos

La API maneja tres estados de permisos:

```javascript
function verificarEstadoPermisos() {
    const estado = Notification.permission;
    
    switch (estado) {
        case "granted":
            console.log("✅ Permisos concedidos - Puedes enviar notificaciones");
            return true;
            
        case "denied":
            console.log("❌ Permisos denegados - No puedes enviar notificaciones");
            console.log("💡 El usuario debe habilitar manualmente en configuración del navegador");
            return false;
            
        case "default":
            console.log("❓ Permisos no solicitados aún - Debes pedirlos al usuario");
            return false;
            
        default:
            console.log("❓ Estado de permiso desconocido:", estado);
            return false;
    }
}

// Verificar estado actual
verificarEstadoPermisos();

// Mostrar información detallada
function mostrarInfoPermisos() {
    const info = {
        soporteNavegador: "Notification" in window,
        estadoPermiso: Notification.permission,
        puedeNotificar: Notification.permission === "granted"
    };
    
    console.table(info);
    return info;
}

mostrarInfoPermisos();
```

### Solicitar permisos al usuario

```javascript
async function solicitarPermisos() {
    // Verificar si ya tenemos permisos
    if (Notification.permission === "granted") {
        console.log("✅ Ya tienes permisos para notificaciones");
        return true;
    }
    
    // Si están denegados, no podemos hacer nada
    if (Notification.permission === "denied") {
        console.log("❌ Permisos denegados previamente");
        alert("Para recibir notificaciones, por favor habilítalas en la configuración del navegador");
        return false;
    }
    
    try {
        // Solicitar permisos (puede mostrar popup del navegador)
        console.log("🔔 Solicitando permisos de notificación...");
        const permiso = await Notification.requestPermission();
        
        if (permiso === "granted") {
            console.log("✅ ¡Permisos concedidos!");
            
            // Mostrar notificación de bienvenida
            new Notification("¡Notificaciones activadas!", {
                body: "Ahora recibirás notificaciones de esta aplicación",
                icon: "https://via.placeholder.com/64",
                tag: "bienvenida" // Evita duplicados
            });
            
            return true;
        } else {
            console.log("❌ Permisos denegados por el usuario");
            return false;
        }
        
    } catch (error) {
        console.error("Error solicitando permisos:", error);
        return false;
    }
}

// Función para uso en botones o eventos
function configurarNotificaciones() {
    const boton = document.createElement('button');
    boton.textContent = "Activar Notificaciones";
    boton.style.padding = "10px 20px";
    boton.style.margin = "10px";
    boton.style.cursor = "pointer";
    
    boton.addEventListener('click', async () => {
        boton.disabled = true;
        boton.textContent = "Solicitando permisos...";
        
        const concedido = await solicitarPermisos();
        
        if (concedido) {
            boton.textContent = "✅ Notificaciones Activadas";
            boton.style.backgroundColor = "#4CAF50";
            boton.style.color = "white";
        } else {
            boton.textContent = "❌ Permisos Denegados";
            boton.style.backgroundColor = "#f44336";
            boton.style.color = "white";
            boton.disabled = false;
        }
    });
    
    return boton;
}

// Crear botón de ejemplo (descomenta para usar en HTML)
// document.body.appendChild(configurarNotificaciones());
```

## Crear Notificaciones Básicas

### Notificación simple

```javascript
function notificacionSimple() {
    if (Notification.permission !== "granted") {
        console.log("❌ No tienes permisos para notificaciones");
        return;
    }
    
    const notificacion = new Notification("Título de la notificación");
    
    console.log("📨 Notificación enviada");
    return notificacion;
}

// Usar la función
// notificacionSimple();
```

### Notificación con opciones

```javascript
function notificacionCompleta() {
    if (Notification.permission !== "granted") {
        console.log("❌ Sin permisos para notificaciones");
        return;
    }
    
    const opciones = {
        // Contenido básico
        body: "Este es el cuerpo de la notificación con más detalles",
        icon: "https://via.placeholder.com/64/0066cc/ffffff?text=APP", // Icono de 64x64px
        
        // Identificación y agrupación
        tag: "mensaje-importante", // Evita duplicados con el mismo tag
        
        // Apariencia
        badge: "https://via.placeholder.com/24/ff6600/ffffff?text=!",  // Icono pequeño (24x24px)
        image: "https://via.placeholder.com/300x150/00cc66/ffffff?text=Imagen", // Imagen grande
        
        // Comportamiento
        requireInteraction: false, // Si true, no se cierra automáticamente
        silent: false,             // Si true, no hace sonido
        
        // Datos personalizados
        data: {
            id: "msg_001",
            tipo: "promocion",
            url: "https://mi-app.com/ofertas",
            timestamp: Date.now()
        },
        
        // Configuración de tiempo
        timestamp: Date.now(), // Marca de tiempo personalizada
        
        // Vibración (en dispositivos móviles)
        vibrate: [200, 100, 200] // Patrón de vibración en milisegundos
    };
    
    const notificacion = new Notification("🎉 ¡Oferta Especial!", opciones);
    
    // Manejar eventos de la notificación
    notificacion.onclick = function(event) {
        console.log("👆 Usuario hizo clic en la notificación");
        console.log("📊 Datos:", event.target.data);
        
        // Abrir URL si está definida
        if (event.target.data && event.target.data.url) {
            window.open(event.target.data.url, '_blank');
        }
        
        // Cerrar la notificación
        event.target.close();
    };
    
    notificacion.onshow = function() {
        console.log("👁️ Notificación mostrada");
    };
    
    notificacion.onclose = function() {
        console.log("❌ Notificación cerrada");
    };
    
    notificacion.onerror = function(error) {
        console.error("💥 Error en notificación:", error);
    };
    
    return notificacion;
}

// Crear notificación completa
// notificacionCompleta();
```

### Sistema de notificaciones con diferentes tipos

```javascript
class GestorNotificaciones {
    constructor() {
        this.notificacionesActivas = new Map();
        this.configuracion = {
            iconoApp: "https://via.placeholder.com/64/0066cc/ffffff?text=APP",
            badgeApp: "https://via.placeholder.com/24/0066cc/ffffff?text=!",
            duracionPorDefecto: 5000 // 5 segundos
        };
    }
    
    // Verificar si se pueden enviar notificaciones
    async verificarDisponibilidad() {
        if (!("Notification" in window)) {
            throw new Error("Notificaciones no soportadas en este navegador");
        }
        
        if (Notification.permission === "denied") {
            throw new Error("Permisos de notificación denegados");
        }
        
        if (Notification.permission === "default") {
            const permiso = await Notification.requestPermission();
            if (permiso !== "granted") {
                throw new Error("Usuario denegó los permisos de notificación");
            }
        }
        
        return true;
    }
    
    // Notificación de éxito
    exito(titulo, mensaje, opciones = {}) {
        return this._crearNotificacion(titulo, mensaje, {
            icon: "https://via.placeholder.com/64/4CAF50/ffffff?text=✓",
            badge: "https://via.placeholder.com/24/4CAF50/ffffff?text=✓",
            tag: "exito",
            ...opciones
        });
    }
    
    // Notificación de error
    error(titulo, mensaje, opciones = {}) {
        return this._crearNotificacion(titulo, mensaje, {
            icon: "https://via.placeholder.com/64/f44336/ffffff?text=✗",
            badge: "https://via.placeholder.com/24/f44336/ffffff?text=✗",
            tag: "error",
            requireInteraction: true, // No se cierra automáticamente
            ...opciones
        });
    }
    
    // Notificación de advertencia
    advertencia(titulo, mensaje, opciones = {}) {
        return this._crearNotificacion(titulo, mensaje, {
            icon: "https://via.placeholder.com/64/ff9800/ffffff?text=⚠",
            badge: "https://via.placeholder.com/24/ff9800/ffffff?text=⚠",
            tag: "advertencia",
            ...opciones
        });
    }
    
    // Notificación de información
    info(titulo, mensaje, opciones = {}) {
        return this._crearNotificacion(titulo, mensaje, {
            icon: "https://via.placeholder.com/64/2196F3/ffffff?text=ℹ",
            badge: "https://via.placeholder.com/24/2196F3/ffffff?text=ℹ",
            tag: "info",
            ...opciones
        });
    }
    
    // Notificación de mensaje/chat
    mensaje(remitente, contenido, opciones = {}) {
        return this._crearNotificacion(`Mensaje de ${remitente}`, contenido, {
            icon: opciones.avatarRemitente || "https://via.placeholder.com/64/9C27B0/ffffff?text=💬",
            badge: "https://via.placeholder.com/24/9C27B0/ffffff?text=💬",
            tag: `mensaje_${remitente.toLowerCase().replace(/\s+/g, '_')}`,
            data: {
                tipo: "mensaje",
                remitente: remitente,
                ...opciones.data
            },
            ...opciones
        });
    }
    
    // Método privado para crear notificaciones
    async _crearNotificacion(titulo, mensaje, opciones = {}) {
        try {
            await this.verificarDisponibilidad();
            
            const configuracionFinal = {
                body: mensaje,
                icon: this.configuracion.iconoApp,
                badge: this.configuracion.badgeApp,
                timestamp: Date.now(),
                data: {
                    id: Date.now().toString(),
                    timestamp: Date.now(),
                    ...opciones.data
                },
                ...opciones
            };
            
            const notificacion = new Notification(titulo, configuracionFinal);
            
            // Registrar notificación activa
            if (configuracionFinal.tag) {
                this.notificacionesActivas.set(configuracionFinal.tag, notificacion);
            }
            
            // Auto-cerrar después del tiempo especificado
            if (configuracionFinal.duracion !== false) {
                const duracion = configuracionFinal.duracion || this.configuracion.duracionPorDefecto;
                setTimeout(() => {
                    notificacion.close();
                }, duracion);
            }
            
            // Event listeners por defecto
            notificacion.onclick = (event) => {
                console.log("🔔 Notificación clickeada:", event.target.data);
                window.focus(); // Traer ventana al frente
                event.target.close();
            };
            
            notificacion.onclose = () => {
                if (configuracionFinal.tag) {
                    this.notificacionesActivas.delete(configuracionFinal.tag);
                }
            };
            
            console.log(`📨 Notificación "${titulo}" enviada`);
            return notificacion;
            
        } catch (error) {
            console.error("Error creando notificación:", error.message);
            throw error;
        }
    }
    
    // Cerrar todas las notificaciones
    cerrarTodas() {
        this.notificacionesActivas.forEach(notificacion => {
            notificacion.close();
        });
        this.notificacionesActivas.clear();
        console.log("🔕 Todas las notificaciones cerradas");
    }
    
    // Obtener notificaciones activas
    obtenerActivas() {
        return Array.from(this.notificacionesActivas.entries());
    }
}

// Usar el gestor de notificaciones
const notificaciones = new GestorNotificaciones();

// Ejemplos de uso
async function ejemplosNotificaciones() {
    try {
        // Diferentes tipos de notificaciones
        await notificaciones.exito("¡Guardado!", "Los cambios se guardaron correctamente");
        
        setTimeout(() => {
            notificaciones.info("Información", "Hay 3 tareas pendientes por completar");
        }, 2000);
        
        setTimeout(() => {
            notificaciones.advertencia("Atención", "La sesión expirará en 5 minutos");
        }, 4000);
        
        setTimeout(() => {
            notificaciones.mensaje("Juan Pérez", "¿Nos vemos para la reunión?", {
                avatarRemitente: "https://via.placeholder.com/64/FF5722/ffffff?text=JP"
            });
        }, 6000);
        
        // Mostrar error después de 8 segundos
        setTimeout(() => {
            notificaciones.error("Error", "No se pudo conectar con el servidor");
        }, 8000);
        
    } catch (error) {
        console.error("Error en ejemplos:", error.message);
    }
}

// Ejecutar ejemplos (descomenta para probar)
// ejemplosNotificaciones();
```

## Notificaciones Avanzadas con Service Workers

### ¿Por qué usar Service Workers?

Los **Service Workers** permiten notificaciones más avanzadas que pueden:
- Funcionar cuando la página está cerrada
- Manejar interacciones complejas
- Sincronizarse con el servidor
- Trabajar offline

```javascript
// Registrar Service Worker para notificaciones
async function registrarServiceWorker() {
    if (!('serviceWorker' in navigator)) {
        console.log("❌ Service Workers no soportados");
        return false;
    }
    
    try {
        console.log("🔧 Registrando Service Worker...");
        
        const registration = await navigator.serviceWorker.register('/sw-notificaciones.js');
        
        console.log("✅ Service Worker registrado:", registration);
        
        // Esperar a que esté activo
        await navigator.serviceWorker.ready;
        console.log("🚀 Service Worker activo y listo");
        
        return registration;
        
    } catch (error) {
        console.error("❌ Error registrando Service Worker:", error);
        return false;
    }
}

// Crear notificación desde Service Worker
async function notificacionDesdeServiceWorker(titulo, opciones = {}) {
    try {
        const registration = await navigator.serviceWorker.ready;
        
        if (!registration) {
            throw new Error("Service Worker no disponible");
        }
        
        const configuracion = {
            body: "Notificación desde Service Worker",
            icon: "/icon-192.png",
            badge: "/badge-72.png",
            tag: "sw-notification",
            data: {
                timestamp: Date.now(),
                source: "service-worker"
            },
            actions: [
                {
                    action: "ver",
                    title: "Ver detalles",
                    icon: "/action-view.png"
                },
                {
                    action: "cerrar",
                    title: "Cerrar",
                    icon: "/action-close.png"
                }
            ],
            ...opciones
        };
        
        await registration.showNotification(titulo, configuracion);
        console.log("📨 Notificación SW enviada:", titulo);
        
    } catch (error) {
        console.error("Error enviando notificación SW:", error);
    }
}

// Ejemplo de Service Worker (contenido para sw-notificaciones.js)
const serviceWorkerCode = `
// sw-notificaciones.js
self.addEventListener('notificationshow', function(event) {
    console.log('Notificación mostrada:', event.notification.title);
});

self.addEventListener('notificationclick', function(event) {
    console.log('Notificación clickeada:', event.notification.title);
    
    event.notification.close();
    
    // Manejar diferentes acciones
    if (event.action === 'ver') {
        // Abrir ventana específica
        event.waitUntil(
            clients.openWindow('/detalles')
        );
    } else if (event.action === 'cerrar') {
        // Solo cerrar (ya se cerró arriba)
        console.log('Notificación cerrada por acción del usuario');
    } else {
        // Clic en el cuerpo de la notificación
        event.waitUntil(
            clients.openWindow('/')
        );
    }
});

self.addEventListener('notificationclose', function(event) {
    console.log('Notificación cerrada:', event.notification.title);
    
    // Opcional: enviar analíticos sobre notificación cerrada
    // fetch('/analytics/notification-closed', {
    //     method: 'POST',
    //     body: JSON.stringify({ tag: event.notification.tag })
    // });
});
`;

// Función para crear el archivo del Service Worker
function crearServiceWorker() {
    console.log("📁 Código del Service Worker:");
    console.log(serviceWorkerCode);
    console.log("\n💡 Guarda este código en un archivo llamado 'sw-notificaciones.js' en la raíz de tu proyecto");
}
```

## Sistema de Notificaciones Push

### Configuración básica para Push Notifications

```javascript
class NotificacionesPush {
    constructor() {
        this.registration = null;
        this.subscription = null;
    }
    
    // Inicializar sistema push
    async inicializar() {
        try {
            // Registrar Service Worker
            this.registration = await navigator.serviceWorker.register('/sw-push.js');
            console.log("✅ Service Worker para Push registrado");
            
            // Esperar a que esté listo
            await navigator.serviceWorker.ready;
            
            return true;
        } catch (error) {
            console.error("❌ Error inicializando Push:", error);
            return false;
        }
    }
    
    // Suscribirse a notificaciones push
    async suscribirse(clavePublicaVAPID) {
        try {
            if (!this.registration) {
                throw new Error("Service Worker no registrado");
            }
            
            // Verificar si ya está suscrito
            this.subscription = await this.registration.pushManager.getSubscription();
            
            if (this.subscription) {
                console.log("✅ Ya suscrito a Push notifications");
                return this.subscription;
            }
            
            // Crear nueva suscripción
            this.subscription = await this.registration.pushManager.subscribe({
                userVisibleOnly: true,
                applicationServerKey: this._urlBase64ToUint8Array(clavePublicaVAPID)
            });
            
            console.log("🔔 Suscrito a Push notifications");
            console.log("📋 Suscripción:", this.subscription);
            
            // Enviar suscripción al servidor
            await this._enviarSuscripcionAlServidor(this.subscription);
            
            return this.subscription;
            
        } catch (error) {
            console.error("❌ Error suscribiéndose a Push:", error);
            throw error;
        }
    }
    
    // Desuscribirse
    async desuscribirse() {
        try {
            if (this.subscription) {
                await this.subscription.unsubscribe();
                console.log("🔕 Desuscrito de Push notifications");
                
                // Notificar al servidor
                await this._removerSuscripcionDelServidor(this.subscription);
                
                this.subscription = null;
                return true;
            }
            return false;
        } catch (error) {
            console.error("❌ Error desuscribiéndose:", error);
            throw error;
        }
    }
    
    // Obtener estado de suscripción
    async obtenerEstado() {
        if (!this.registration) {
            return { suscrito: false, error: "Service Worker no registrado" };
        }
        
        try {
            const subscription = await this.registration.pushManager.getSubscription();
            
            return {
                suscrito: !!subscription,
                subscription: subscription,
                endpoint: subscription ? subscription.endpoint : null
            };
        } catch (error) {
            return { suscrito: false, error: error.message };
        }
    }
    
    // Convertir clave VAPID
    _urlBase64ToUint8Array(base64String) {
        const padding = '='.repeat((4 - base64String.length % 4) % 4);
        const base64 = (base64String + padding)
            .replace(/\-/g, '+')
            .replace(/_/g, '/');
            
        const rawData = window.atob(base64);
        const outputArray = new Uint8Array(rawData.length);
        
        for (let i = 0; i < rawData.length; ++i) {
            outputArray[i] = rawData.charCodeAt(i);
        }
        
        return outputArray;
    }
    
    // Enviar suscripción al servidor
    async _enviarSuscripcionAlServidor(subscription) {
        try {
            const response = await fetch('/api/push/subscribe', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    subscription: subscription,
                    timestamp: Date.now()
                })
            });
            
            if (!response.ok) {
                throw new Error(`Error del servidor: ${response.status}`);
            }
            
            console.log("📤 Suscripción enviada al servidor");
        } catch (error) {
            console.error("❌ Error enviando suscripción:", error);
        }
    }
    
    // Remover suscripción del servidor
    async _removerSuscripcionDelServidor(subscription) {
        try {
            await fetch('/api/push/unsubscribe', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ endpoint: subscription.endpoint })
            });
            
            console.log("📤 Suscripción removida del servidor");
        } catch (error) {
            console.error("❌ Error removiendo suscripción:", error);
        }
    }
}

// Service Worker para Push (sw-push.js)
const pushServiceWorkerCode = `
// sw-push.js
self.addEventListener('push', function(event) {
    console.log('Push recibido:', event);
    
    let datos = {};
    
    if (event.data) {
        try {
            datos = event.data.json();
        } catch (error) {
            datos = { title: 'Notificación', body: event.data.text() };
        }
    }
    
    const opciones = {
        body: datos.body || 'Nueva notificación',
        icon: datos.icon || '/icon-192.png',
        badge: datos.badge || '/badge-72.png',
        data: datos.data || {},
        tag: datos.tag || 'push-notification',
        requireInteraction: datos.requireInteraction || false,
        actions: datos.actions || []
    };
    
    event.waitUntil(
        self.registration.showNotification(datos.title || 'Notificación', opciones)
    );
});

self.addEventListener('notificationclick', function(event) {
    event.notification.close();
    
    const urlToOpen = event.notification.data.url || '/';
    
    event.waitUntil(
        clients.matchAll({ type: 'window', includeUncontrolled: true })
        .then(function(clientList) {
            // Si ya hay una ventana abierta, enfocarla
            for (let i = 0; i < clientList.length; i++) {
                const client = clientList[i];
                if (client.url === urlToOpen && 'focus' in client) {
                    return client.focus();
                }
            }
            
            // Si no hay ventana abierta, abrir nueva
            if (clients.openWindow) {
                return clients.openWindow(urlToOpen);
            }
        })
    );
});
`;

// Ejemplo de uso del sistema Push
async function configurarNotificacionesPush() {
    const push = new NotificacionesPush();
    
    try {
        // Clave pública VAPID (debes obtenerla de tu servidor)
        const clavePublicaVAPID = 'TU_CLAVE_PUBLICA_VAPID_AQUI';
        
        // Inicializar
        await push.inicializar();
        
        // Suscribirse
        const subscription = await push.suscribirse(clavePublicaVAPID);
        
        console.log("🎉 Sistema Push configurado correctamente");
        console.log("📋 Detalles de suscripción:", subscription);
        
        return push;
        
    } catch (error) {
        console.error("❌ Error configurando Push:", error.message);
        return null;
    }
}

// Función para mostrar código del Service Worker
function mostrarCodigoPushSW() {
    console.log("📁 Código del Service Worker para Push:");
    console.log(pushServiceWorkerCode);
    console.log("\n💡 Guarda este código en 'sw-push.js' en la raíz del proyecto");
}
```

## Casos Prácticos Completos

### Sistema de notificaciones para aplicación de tareas

```javascript
class NotificadorTareas {
    constructor() {
        this.gestorNotificaciones = new GestorNotificaciones();
        this.tareasConRecordatorio = new Map();
    }
    
    // Configurar recordatorio de tarea
    configurarRecordatorio(tarea, fechaRecordatorio) {
        const ahora = new Date();
        const tiempoRecordatorio = new Date(fechaRecordatorio);
        
        if (tiempoRecordatorio <= ahora) {
            throw new Error("La fecha de recordatorio debe ser futura");
        }
        
        const msHastaRecordatorio = tiempoRecordatorio.getTime() - ahora.getTime();
        
        const timeoutId = setTimeout(() => {
            this.notificarTarea(tarea);
            this.tareasConRecordatorio.delete(tarea.id);
        }, msHastaRecordatorio);
        
        this.tareasConRecordatorio.set(tarea.id, {
            tarea: tarea,
            fechaRecordatorio: tiempoRecordatorio,
            timeoutId: timeoutId
        });
        
        console.log(`⏰ Recordatorio configurado para "${tarea.titulo}" el ${tiempoRecordatorio.toLocaleString()}`);
    }
    
    // Notificar tarea pendiente
    async notificarTarea(tarea) {
        const tiempo = this._calcularTiempoRestante(tarea.fechaVencimiento);
        let mensaje, tipo;
        
        if (tiempo.vencida) {
            mensaje = `Vencida hace ${tiempo.texto}`;
            tipo = "error";
        } else if (tiempo.minutos <= 30) {
            mensaje = `Vence en ${tiempo.texto} ⚠️`;
            tipo = "advertencia";
        } else {
            mensaje = `Vence en ${tiempo.texto}`;
            tipo = "info";
        }
        
        const opciones = {
            data: {
                tareaId: tarea.id,
                tipo: "recordatorio-tarea",
                prioridad: tarea.prioridad
            },
            requireInteraction: tiempo.vencida || tiempo.minutos <= 15,
            actions: [
                { action: "completar", title: "✅ Marcar completada" },
                { action: "posponer", title: "⏰ Posponer 15 min" },
                { action: "ver", title: "👁️ Ver detalles" }
            ]
        };
        
        return await this.gestorNotificaciones[tipo](
            `📋 ${tarea.titulo}`,
            mensaje,
            opciones
        );
    }
    
    // Notificar tarea completada
    async notificarComplecion(tarea) {
        return await this.gestorNotificaciones.exito(
            "¡Tarea completada!",
            `"${tarea.titulo}" ha sido marcada como completada`,
            {
                data: {
                    tareaId: tarea.id,
                    tipo: "tarea-completada"
                }
            }
        );
    }
    
    // Notificar nueva tarea asignada
    async notificarNuevaTarea(tarea, asignadaPor = null) {
        const mensaje = asignadaPor 
            ? `Nueva tarea asignada por ${asignadaPor}`
            : "Nueva tarea creada";
            
        return await this.gestorNotificaciones.info(
            `📝 ${tarea.titulo}`,
            mensaje,
            {
                data: {
                    tareaId: tarea.id,
                    tipo: "nueva-tarea"
                }
            }
        );
    }
    
    // Resumen diario de tareas
    async notificarResumenDiario(resumen) {
        const { pendientes, completadas, vencidas } = resumen;
        
        let mensaje = `📊 Resumen del día:\n`;
        mensaje += `✅ ${completadas} completadas\n`;
        mensaje += `📋 ${pendientes} pendientes`;
        
        if (vencidas > 0) {
            mensaje += `\n⚠️ ${vencidas} vencidas`;
        }
        
        return await this.gestorNotificaciones.info(
            "📅 Resumen diario",
            mensaje,
            {
                data: {
                    tipo: "resumen-diario",
                    resumen: resumen
                }
            }
        );
    }
    
    // Cancelar recordatorio
    cancelarRecordatorio(tareaId) {
        const recordatorio = this.tareasConRecordatorio.get(tareaId);
        
        if (recordatorio) {
            clearTimeout(recordatorio.timeoutId);
            this.tareasConRecordatorio.delete(tareaId);
            console.log(`❌ Recordatorio cancelado para tarea ${tareaId}`);
            return true;
        }
        
        return false;
    }
    
    // Obtener recordatorios activos
    obtenerRecordatoriosActivos() {
        return Array.from(this.tareasConRecordatorio.values()).map(r => ({
            tarea: r.tarea,
            fechaRecordatorio: r.fechaRecordatorio
        }));
    }
    
    // Calcular tiempo restante
    _calcularTiempoRestante(fechaVencimiento) {
        const ahora = new Date();
        const vencimiento = new Date(fechaVencimiento);
        const diferencia = vencimiento.getTime() - ahora.getTime();
        
        if (diferencia < 0) {
            const tiempoVencido = Math.abs(diferencia);
            const horas = Math.floor(tiempoVencido / (1000 * 60 * 60));
            const minutos = Math.floor((tiempoVencido % (1000 * 60 * 60)) / (1000 * 60));
            
            return {
                vencida: true,
                texto: horas > 0 ? `${horas}h ${minutos}m` : `${minutos}m`,
                minutos: -Math.floor(tiempoVencido / (1000 * 60))
            };
        }
        
        const horas = Math.floor(diferencia / (1000 * 60 * 60));
        const minutos = Math.floor((diferencia % (1000 * 60 * 60)) / (1000 * 60));
        
        return {
            vencida: false,
            texto: horas > 0 ? `${horas}h ${minutos}m` : `${minutos}m`,
            minutos: Math.floor(diferencia / (1000 * 60))
        };
    }
}

// Ejemplo de uso del notificador de tareas
const notificadorTareas = new NotificadorTareas();

// Datos de ejemplo
const tareaEjemplo = {
    id: "tarea_001",
    titulo: "Completar presentación",
    descripcion: "Preparar slides para reunión del cliente",
    fechaVencimiento: new Date(Date.now() + 2 * 60 * 60 * 1000), // 2 horas
    prioridad: "alta"
};

// Configurar notificaciones de ejemplo
async function ejemploNotificadorTareas() {
    try {
        // Notificar nueva tarea
        await notificadorTareas.notificarNuevaTarea(tareaEjemplo, "María García");
        
        // Configurar recordatorio para 1 hora antes del vencimiento
        const fechaRecordatorio = new Date(tareaEjemplo.fechaVencimiento.getTime() - 60 * 60 * 1000);
        notificadorTareas.configurarRecordatorio(tareaEjemplo, fechaRecordatorio);
        
        // Simular completación después de 5 segundos
        setTimeout(async () => {
            await notificadorTareas.notificarComplecion(tareaEjemplo);
        }, 5000);
        
        // Resumen diario de ejemplo
        setTimeout(async () => {
            await notificadorTareas.notificarResumenDiario({
                completadas: 5,
                pendientes: 3,
                vencidas: 1
            });
        }, 7000);
        
        console.log("✅ Ejemplos de notificador de tareas configurados");
        
    } catch (error) {
        console.error("❌ Error en ejemplos:", error.message);
    }
}

// Ejecutar ejemplo (descomenta para probar)
// ejemploNotificadorTareas();
```

## Mejores Prácticas

### ✅ Buenas prácticas

```javascript
// 1. Siempre verificar permisos antes de notificar
async function notificarConVerificacion(titulo, opciones) {
    if (Notification.permission !== "granted") {
        console.log("⚠️ Sin permisos de notificación");
        return false;
    }
    
    new Notification(titulo, opciones);
    return true;
}

// 2. Usar tags para evitar spam
function notificarSinSpam(titulo, opciones) {
    const configuracion = {
        tag: "no-spam", // Solo una notificación con este tag
        renotify: true, // Actualizar si ya existe
        ...opciones
    };
    
    new Notification(titulo, configuracion);
}

// 3. Proporcionar controles al usuario
class ControladorNotificaciones {
    constructor() {
        this.configuracion = {
            habilitadas: localStorage.getItem('notificaciones') === 'true',
            tipos: {
                mensajes: localStorage.getItem('notif_mensajes') !== 'false',
                recordatorios: localStorage.getItem('notif_recordatorios') !== 'false',
                sistema: localStorage.getItem('notif_sistema') !== 'false'
            }
        };
    }
    
    habilitar(tipo = null) {
        if (tipo) {
            this.configuracion.tipos[tipo] = true;
            localStorage.setItem(`notif_${tipo}`, 'true');
        } else {
            this.configuracion.habilitadas = true;
            localStorage.setItem('notificaciones', 'true');
        }
    }
    
    deshabilitar(tipo = null) {
        if (tipo) {
            this.configuracion.tipos[tipo] = false;
            localStorage.setItem(`notif_${tipo}`, 'false');
        } else {
            this.configuracion.habilitadas = false;
            localStorage.setItem('notificaciones', 'false');
        }
    }
    
    puedeNotificar(tipo = null) {
        if (!this.configuracion.habilitadas) return false;
        if (tipo && !this.configuracion.tipos[tipo]) return false;
        return Notification.permission === "granted";
    }
}

// 4. Manejar errores gracefully
function notificarConFallback(titulo, mensaje, fallbackCallback = null) {
    if (Notification.permission === "granted") {
        try {
            new Notification(titulo, { body: mensaje });
        } catch (error) {
            console.error("Error creando notificación:", error);
            if (fallbackCallback) fallbackCallback(titulo, mensaje);
        }
    } else {
        console.log("Sin permisos, usando fallback");
        if (fallbackCallback) fallbackCallback(titulo, mensaje);
    }
}

// 5. Optimizar para móviles
function notificarResponsive(titulo, opciones) {
    const esMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
    
    const configuracion = {
        ...opciones,
        // En móviles, usar vibración
        vibrate: esMobile ? [200, 100, 200] : undefined,
        // En desktop, mantener visible más tiempo
        requireInteraction: !esMobile
    };
    
    new Notification(titulo, configuracion);
}
```

### ❌ Prácticas a evitar

```javascript
// ❌ NO hacer spam de notificaciones
function malaPractica_spam() {
    for (let i = 0; i < 10; i++) {
        new Notification(`Notificación ${i}`); // ¡MAL!
    }
}

// ❌ NO notificar sin contexto claro
function malaPractica_sinContexto() {
    new Notification("Algo pasó"); // ¿Qué pasó?
}

// ❌ NO asumir que los permisos están dados
function malaPractica_sinVerificar() {
    new Notification("Mensaje"); // Puede fallar
}

// ❌ NO usar notificaciones para todo
function malaPractica_abusoUso() {
    new Notification("Usuario hizo clic"); // Innecesario
    new Notification("Página cargada");    // Molesto
    new Notification("Hora actual: " + new Date()); // Spam
}

// ✅ Correcciones
function buenasPracticasCorregidas() {
    // Verificar permisos
    if (Notification.permission !== "granted") return;
    
    // Una sola notificación con tag
    new Notification("Mensajes importantes", {
        body: "Tienes 5 mensajes nuevos",
        tag: "mensajes-resumen",
        data: { count: 5 }
    });
    
    // Solo para eventos importantes
    new Notification("Reunión en 5 minutos", {
        body: "Sala de juntas - Revisión del proyecto",
        requireInteraction: true
    });
}
```

## Consideraciones de Privacidad y UX

### Solicitar permisos en el momento adecuado

```javascript
class GestorPermisosUX {
    constructor() {
        this.yasolicitado = localStorage.getItem('notif_solicitado') === 'true';
    }
    
    // Mostrar explicación antes de solicitar permisos
    async solicitarConContexto(razon) {
        if (this.yasolicitado && Notification.permission === "denied") {
            this.mostrarInstruccionesManuales();
            return false;
        }
        
        if (Notification.permission === "granted") {
            return true;
        }
        
        // Mostrar explicación primero
        const acepta = await this.mostrarExplicacion(razon);
        
        if (!acepta) {
            return false;
        }
        
        // Ahora sí solicitar permisos
        const permiso = await Notification.requestPermission();
        this.yasolicitado = true;
        localStorage.setItem('notif_solicitado', 'true');
        
        return permiso === "granted";
    }
    
    async mostrarExplicacion(razon) {
        return new Promise((resolve) => {
            // Crear modal de explicación
            const modal = document.createElement('div');
            modal.style.cssText = `
                position: fixed; top: 0; left: 0; width: 100%; height: 100%;
                background: rgba(0,0,0,0.5); display: flex; align-items: center;
                justify-content: center; z-index: 10000;
            `;
            
            modal.innerHTML = `
                <div style="background: white; padding: 30px; border-radius: 8px; max-width: 400px; text-align: center;">
                    <h3>🔔 Activar Notificaciones</h3>
                    <p>${razon}</p>
                    <p><small>Puedes desactivarlas en cualquier momento desde la configuración.</small></p>
                    <div style="margin-top: 20px;">
                        <button id="aceptar-notif" style="background: #4CAF50; color: white; border: none; padding: 10px 20px; margin: 5px; border-radius: 4px; cursor: pointer;">
                            ✅ Activar Notificaciones
                        </button>
                        <button id="rechazar-notif" style="background: #f44336; color: white; border: none; padding: 10px 20px; margin: 5px; border-radius: 4px; cursor: pointer;">
                            ❌ No, gracias
                        </button>
                    </div>
                </div>
            `;
            
            document.body.appendChild(modal);
            
            document.getElementById('aceptar-notif').onclick = () => {
                document.body.removeChild(modal);
                resolve(true);
            };
            
            document.getElementById('rechazar-notif').onclick = () => {
                document.body.removeChild(modal);
                resolve(false);
            };
        });
    }
    
    mostrarInstruccionesManuales() {
        alert(`
🔔 Notificaciones Bloqueadas

Para recibir notificaciones, por favor:

1. Haz clic en el ícono de candado en la barra de direcciones
2. Selecciona "Permitir" para Notificaciones
3. Recarga la página

O ve a Configuración del navegador > Privacidad y seguridad > Configuración del sitio > Notificaciones
        `);
    }
}

// Uso contextual
const gestorUX = new GestorPermisosUX();

// Solicitar cuando sea relevante
async function configurarRecordatorios() {
    const concedido = await gestorUX.solicitarConContexto(
        "Te notificaremos cuando tengas tareas próximas a vencer para que no olvides nada importante."
    );
    
    if (concedido) {
        console.log("✅ Usuario activó notificaciones para recordatorios");
    } else {
        console.log("ℹ️ Usuario prefiere no recibir notificaciones");
    }
}
```

***

Las notificaciones web son una herramienta poderosa para mantener a los usuarios comprometidos con tu aplicación. El uso responsable y contextual es clave para una buena experiencia de usuario. Recuerda siempre respetar las preferencias del usuario y proporcionar valor real con cada notificación.
