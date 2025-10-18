# DataTables en JavaScript

**DataTables** es una biblioteca de JavaScript extremadamente poderosa y flexible que transforma las tablas HTML básicas en componentes interactivos avanzados con funcionalidades como ordenamiento, filtrado, paginación, búsqueda y mucho más. Es una de las librerías más populares para el manejo de datos tabulares en aplicaciones web.

## ¿Qué son los DataTables?

**DataTables** es un plugin de jQuery (aunque también tiene versiones para otros frameworks) que mejora las tablas HTML proporcionando:

- **🔍 Búsqueda instantánea** - Filtrado en tiempo real
- **📊 Ordenamiento** - Por cualquier columna
- **📄 Paginación** - Para grandes volúmenes de datos
- **🎨 Temas personalizables** - Múltiples estilos disponibles
- **📱 Responsive** - Adaptable a dispositivos móviles
- **🚀 Carga asíncrona** - Integración con APIs
- **💾 Exportación** - A Excel, PDF, CSV, etc.

```html
<!-- Tabla HTML básica -->
<table id="ejemplo-basico" class="display" style="width:100%">
    <thead>
        <tr>
            <th>Nombre</th>
            <th>Cargo</th>
            <th>Oficina</th>
            <th>Edad</th>
            <th>Fecha inicio</th>
            <th>Salario</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Tiger Nixon</td>
            <td>System Architect</td>
            <td>Edinburgh</td>
            <td>61</td>
            <td>2011/04/25</td>
            <td>$320,800</td>
        </tr>
        <tr>
            <td>Garrett Winters</td>
            <td>Accountant</td>
            <td>Tokyo</td>
            <td>63</td>
            <td>2011/07/25</td>
            <td>$170,750</td>
        </tr>
        <!-- Más filas... -->
    </tbody>
</table>
```

```javascript
// Transformar tabla básica en DataTable
$(document).ready(function() {
    $('#ejemplo-basico').DataTable();
});

// ¡Eso es todo! Ahora la tabla tiene:
// ✅ Búsqueda instantánea
// ✅ Ordenamiento por columnas
// ✅ Paginación automática
// ✅ Información de registros
// ✅ Navegación entre páginas
```

## Instalación y Configuración

### CDN (Método más rápido)

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DataTables Demo</title>
    
    <!-- CSS de DataTables -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.6/css/jquery.dataTables.min.css">
    
    <!-- CSS opcional para Bootstrap integration -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.6/css/dataTables.bootstrap5.min.css">
    
    <!-- CSS para extensiones -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/buttons/2.4.1/css/buttons.dataTables.min.css">
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/responsive/2.5.0/css/responsive.dataTables.min.css">
</head>
<body>
    <!-- Tu contenido aquí -->
    
    <!-- jQuery (requerido) -->
    <script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
    
    <!-- DataTables Core -->
    <script type="text/javascript" src="https://cdn.datatables.net/1.13.6/js/jquery.dataTables.min.js"></script>
    
    <!-- DataTables Extensions -->
    <script type="text/javascript" src="https://cdn.datatables.net/buttons/2.4.1/js/dataTables.buttons.min.js"></script>
    <script type="text/javascript" src="https://cdn.datatables.net/responsive/2.5.0/js/dataTables.responsive.min.js"></script>
    
    <!-- Para exportar a Excel/PDF -->
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.53/pdfmake.min.js"></script>
    <script type="text/javascript" src="https://cdn.datatables.net/buttons/2.4.1/js/buttons.html5.min.js"></script>
</body>
</html>
```

### NPM (Para proyectos con build tools)

```bash
# Instalar DataTables
npm install datatables.net
npm install datatables.net-dt  # Tema por defecto

# Para usar con jQuery
npm install jquery

# Extensiones opcionales
npm install datatables.net-buttons
npm install datatables.net-responsive
npm install datatables.net-select
```

## Configuración Básica

### DataTable básico

```javascript
$(document).ready(function() {
    // Configuración básica
    $('#mi-tabla').DataTable({
        // Opciones de paginación
        "paging": true,           // Habilitar paginación
        "pageLength": 10,         // Registros por página
        "lengthMenu": [10, 25, 50, 100], // Opciones de registros por página
        
        // Opciones de búsqueda
        "searching": true,        // Habilitar búsqueda
        "searchDelay": 400,       // Delay en búsqueda (ms)
        
        // Opciones de ordenamiento
        "ordering": true,         // Habilitar ordenamiento
        "order": [[ 0, "asc" ]], // Ordenar por primera columna ascendente
        
        // Información y navegación
        "info": true,            // Mostrar información de registros
        "lengthChange": true,    // Permitir cambiar cantidad por página
        
        // Idioma en español
        "language": {
            "decimal": "",
            "emptyTable": "No hay información disponible en la tabla",
            "info": "Mostrando _START_ a _END_ de _TOTAL_ entradas",
            "infoEmpty": "Mostrando 0 a 0 de 0 entradas",
            "infoFiltered": "(filtrado de _MAX_ entradas totales)",
            "infoPostFix": "",
            "thousands": ",",
            "lengthMenu": "Mostrar _MENU_ entradas",
            "loadingRecords": "Cargando...",
            "processing": "Procesando...",
            "search": "Buscar:",
            "zeroRecords": "No se encontraron registros coincidentes",
            "paginate": {
                "first": "Primero",
                "last": "Último",
                "next": "Siguiente",
                "previous": "Anterior"
            },
            "aria": {
                "sortAscending": ": activar para ordenar la columna de forma ascendente",
                "sortDescending": ": activar para ordenar la columna de forma descendente"
            }
        }
    });
});
```

### Configuración de columnas específicas

```javascript
$(document).ready(function() {
    $('#tabla-empleados').DataTable({
        "columnDefs": [
            // Configurar columna específica por índice
            {
                "targets": 0,        // Primera columna (índice 0)
                "orderable": false,  // No se puede ordenar
                "searchable": false, // No se incluye en búsquedas
                "width": "50px"      // Ancho fijo
            },
            // Configurar múltiples columnas
            {
                "targets": [1, 2],   // Columnas 1 y 2
                "className": "text-center" // Clase CSS
            },
            // Configurar por nombre de clase
            {
                "targets": "no-sort",
                "orderable": false
            },
            // Formatear datos
            {
                "targets": 5, // Columna de salario
                "render": function(data, type, row) {
                    if (type === 'display') {
                        return '$' + parseFloat(data).toLocaleString();
                    }
                    return data;
                }
            }
        ],
        
        // Configuración específica por columna
        "columns": [
            { "data": "nombre" },
            { "data": "cargo" },
            { "data": "oficina" },
            { 
                "data": "edad",
                "type": "num"  // Tipo de datos para ordenamiento correcto
            },
            { 
                "data": "fechaInicio",
                "type": "date"
            },
            { 
                "data": "salario",
                "type": "num-fmt" // Número formateado
            }
        ]
    });
});
```

## Carga de Datos

### Datos desde JavaScript (array de objetos)

```javascript
$(document).ready(function() {
    // Datos locales
    const empleados = [
        {
            "nombre": "Juan Pérez",
            "cargo": "Desarrollador Senior",
            "oficina": "Madrid",
            "edad": 32,
            "fechaInicio": "2020-03-15",
            "salario": 45000,
            "activo": true
        },
        {
            "nombre": "María García",
            "cargo": "Diseñadora UX",
            "oficina": "Barcelona", 
            "edad": 28,
            "fechaInicio": "2021-07-10",
            "salario": 38000,
            "activo": true
        },
        {
            "nombre": "Carlos López",
            "cargo": "Project Manager",
            "oficina": "Valencia",
            "edad": 35,
            "fechaInicio": "2019-11-22",
            "salario": 52000,
            "activo": false
        }
    ];
    
    $('#tabla-empleados').DataTable({
        "data": empleados,
        "columns": [
            { 
                "data": "nombre",
                "title": "Nombre Completo"
            },
            { 
                "data": "cargo",
                "title": "Cargo"
            },
            { 
                "data": "oficina",
                "title": "Oficina"
            },
            { 
                "data": "edad",
                "title": "Edad"
            },
            { 
                "data": "fechaInicio",
                "title": "Fecha de Inicio",
                "render": function(data) {
                    return new Date(data).toLocaleDateString('es-ES');
                }
            },
            { 
                "data": "salario",
                "title": "Salario",
                "render": function(data) {
                    return '€' + data.toLocaleString('es-ES');
                }
            },
            {
                "data": "activo",
                "title": "Estado", 
                "render": function(data) {
                    return data ? 
                        '<span class="badge bg-success">Activo</span>' : 
                        '<span class="badge bg-danger">Inactivo</span>';
                }
            }
        ],
        "language": {
            // Configuración de idioma...
        }
    });
});
```

### Carga de datos desde API (AJAX)

```javascript
$(document).ready(function() {
    $('#tabla-dinamica').DataTable({
        "processing": true,    // Mostrar indicador de carga
        "serverSide": false,   // Procesamiento del lado cliente
        "ajax": {
            "url": "/api/empleados",
            "type": "GET",
            "dataSrc": "data",  // Propiedad que contiene los datos
            
            // Manejar errores
            "error": function(xhr, error, code) {
                console.error('Error cargando datos:', error);
                alert('Error al cargar los datos de la tabla');
            },
            
            // Procesar datos antes de mostrarlos
            "dataSrc": function(json) {
                // Procesar la respuesta si es necesario
                console.log('Datos recibidos:', json);
                return json.data;
            }
        },
        
        "columns": [
            { "data": "id" },
            { "data": "nombre" },
            { "data": "email" },
            { 
                "data": "fechaCreacion",
                "render": function(data) {
                    return moment(data).format('DD/MM/YYYY HH:mm');
                }
            },
            {
                "data": null,
                "title": "Acciones",
                "orderable": false,
                "render": function(data, type, row) {
                    return `
                        <button class="btn btn-sm btn-primary btn-editar" data-id="${row.id}">
                            <i class="fas fa-edit"></i> Editar
                        </button>
                        <button class="btn btn-sm btn-danger btn-eliminar" data-id="${row.id}">
                            <i class="fas fa-trash"></i> Eliminar
                        </button>
                    `;
                }
            }
        ]
    });
    
    // Manejar clicks en botones de acción
    $('#tabla-dinamica').on('click', '.btn-editar', function() {
        const id = $(this).data('id');
        editarRegistro(id);
    });
    
    $('#tabla-dinamica').on('click', '.btn-eliminar', function() {
        const id = $(this).data('id');
        eliminarRegistro(id);
    });
});

// Funciones auxiliares
function editarRegistro(id) {
    console.log('Editando registro:', id);
    // Implementar lógica de edición
}

function eliminarRegistro(id) {
    if (confirm('¿Está seguro de eliminar este registro?')) {
        console.log('Eliminando registro:', id);
        // Implementar lógica de eliminación
        // Recargar tabla después de eliminar
        $('#tabla-dinamica').DataTable().ajax.reload();
    }
}
```

### Server-Side Processing (para grandes volúmenes de datos)

```javascript
$(document).ready(function() {
    $('#tabla-servidor').DataTable({
        "processing": true,     // Mostrar indicador de procesamiento
        "serverSide": true,     // Procesamiento del lado servidor
        "ajax": {
            "url": "/api/empleados/datatables",
            "type": "POST",
            "data": function(d) {
                // Agregar parámetros personalizados
                d.filtroOficina = $('#filtro-oficina').val();
                d.filtroActivo = $('#filtro-activo').val();
                return d;
            }
        },
        
        "columns": [
            { "data": "id", "name": "id" },
            { "data": "nombre", "name": "nombre" },
            { "data": "cargo", "name": "cargo" },
            { "data": "oficina", "name": "oficina" },
            { 
                "data": "salario", 
                "name": "salario",
                "render": function(data) {
                    return '€' + parseFloat(data).toLocaleString('es-ES');
                }
            },
            {
                "data": "acciones",
                "name": "acciones", 
                "orderable": false,
                "searchable": false
            }
        ],
        
        // Configuración de ordenamiento por defecto
        "order": [[ 1, "asc" ]],
        
        // Longitud de página
        "pageLength": 25,
        "lengthMenu": [[10, 25, 50, 100], [10, 25, 50, 100]]
    });
    
    // Recargar tabla cuando cambien los filtros
    $('#filtro-oficina, #filtro-activo').change(function() {
        $('#tabla-servidor').DataTable().ajax.reload();
    });
});
```

## Extensiones y Funcionalidades Avanzadas

### Botones de exportación

```javascript
$(document).ready(function() {
    $('#tabla-exportable').DataTable({
        "dom": 'Bfrtip', // B = botones, f = filtro, r = procesando, t = tabla, i = info, p = paginación
        "buttons": [
            {
                "extend": 'copy',
                "text": '<i class="fas fa-copy"></i> Copiar',
                "className": 'btn btn-primary btn-sm'
            },
            {
                "extend": 'excel',
                "text": '<i class="fas fa-file-excel"></i> Excel',
                "className": 'btn btn-success btn-sm',
                "filename": function() {
                    return 'empleados_' + new Date().toISOString().slice(0,10);
                },
                "title": 'Listado de Empleados',
                "exportOptions": {
                    "columns": [0, 1, 2, 3, 4] // Solo exportar ciertas columnas
                }
            },
            {
                "extend": 'pdf',
                "text": '<i class="fas fa-file-pdf"></i> PDF',
                "className": 'btn btn-danger btn-sm',
                "orientation": 'landscape',
                "pageSize": 'A4',
                "customize": function(doc) {
                    // Personalizar PDF
                    doc.content[1].table.widths = ['15%', '25%', '20%', '15%', '25%'];
                    doc.styles.tableHeader.fontSize = 12;
                    doc.styles.tableBodyEven.fontSize = 10;
                    doc.styles.tableBodyOdd.fontSize = 10;
                }
            },
            {
                "extend": 'csv',
                "text": '<i class="fas fa-file-csv"></i> CSV',
                "className": 'btn btn-info btn-sm'
            },
            {
                "extend": 'print',
                "text": '<i class="fas fa-print"></i> Imprimir',
                "className": 'btn btn-secondary btn-sm',
                "customize": function(win) {
                    // Personalizar vista de impresión
                    $(win.document.body)
                        .css('font-size', '10pt')
                        .prepend('<h1>Listado de Empleados</h1>');
                }
            }
        ],
        
        // Datos y columnas...
        "data": empleados,
        "columns": [
            { "data": "nombre", "title": "Nombre" },
            { "data": "cargo", "title": "Cargo" },
            { "data": "oficina", "title": "Oficina" },
            { "data": "edad", "title": "Edad" },
            { "data": "salario", "title": "Salario" }
        ]
    });
});
```

### Responsive (adaptable a móviles)

```javascript
$(document).ready(function() {
    $('#tabla-responsive').DataTable({
        "responsive": {
            "details": {
                "type": 'column',      // Mostrar detalles en columna
                "target": 'tr'         // Target para expandir
            }
        },
        
        "columnDefs": [
            {
                "className": 'control', // Columna de control para expandir
                "orderable": false,
                "targets": 0
            },
            {
                "responsivePriority": 1, // Prioridad alta (siempre visible)
                "targets": 1
            },
            {
                "responsivePriority": 2, // Prioridad media
                "targets": [2, 3]
            }
            // Las columnas sin prioridad se ocultan primero
        ],
        
        "columns": [
            { 
                "data": null,
                "defaultContent": '',
                "className": 'control',
                "orderable": false
            },
            { "data": "nombre", "title": "Nombre" },
            { "data": "cargo", "title": "Cargo" },
            { "data": "oficina", "title": "Oficina" },
            { "data": "telefono", "title": "Teléfono" },
            { "data": "email", "title": "Email" }
        ]
    });
});
```

### Selección de filas

```javascript
$(document).ready(function() {
    const tabla = $('#tabla-seleccionable').DataTable({
        "select": {
            "style": 'multi',        // Selección múltiple
            "selector": 'td:first-child' // Solo primera columna selecciona
        },
        
        "columnDefs": [
            {
                "targets": 0,
                "checkboxes": {
                    "selectRow": true    // Checkbox para seleccionar
                }
            }
        ],
        
        "columns": [
            { "data": null },  // Columna de checkbox
            { "data": "nombre" },
            { "data": "cargo" },
            { "data": "oficina" }
        ]
    });
    
    // Eventos de selección
    tabla.on('select', function(e, dt, type, indexes) {
        const filaSeleccionada = tabla.rows(indexes).data().toArray();
        console.log('Fila seleccionada:', filaSeleccionada);
        actualizarBotones();
    });
    
    tabla.on('deselect', function(e, dt, type, indexes) {
        console.log('Fila deseleccionada');
        actualizarBotones();
    });
    
    // Botones para trabajar con selección
    $('#btn-obtener-seleccionados').click(function() {
        const filasSeleccionadas = tabla.rows({ selected: true }).data().toArray();
        console.log('Filas seleccionadas:', filasSeleccionadas);
        
        if (filasSeleccionadas.length === 0) {
            alert('No hay filas seleccionadas');
        } else {
            alert(`${filasSeleccionadas.length} filas seleccionadas`);
        }
    });
    
    $('#btn-eliminar-seleccionados').click(function() {
        const filasSeleccionadas = tabla.rows({ selected: true });
        
        if (filasSeleccionadas.count() === 0) {
            alert('No hay filas seleccionadas para eliminar');
            return;
        }
        
        if (confirm(`¿Eliminar ${filasSeleccionadas.count()} registros seleccionados?`)) {
            // Eliminar filas seleccionadas
            filasSeleccionadas.remove().draw();
            actualizarBotones();
        }
    });
    
    function actualizarBotones() {
        const cantidadSeleccionada = tabla.rows({ selected: true }).count();
        $('#btn-eliminar-seleccionados').prop('disabled', cantidadSeleccionada === 0);
        $('#contador-seleccionados').text(cantidadSeleccionada);
    }
    
    // Inicializar botones
    actualizarBotones();
});
```

## Personalización Avanzada

### Filtros personalizados

```javascript
$(document).ready(function() {
    const tabla = $('#tabla-con-filtros').DataTable({
        "data": empleados,
        "columns": [
            { "data": "nombre" },
            { "data": "cargo" },
            { "data": "oficina" },
            { "data": "edad" },
            { "data": "salario" },
            { "data": "activo" }
        ]
    });
    
    // Filtro por oficina
    $('#filtro-oficina').on('change', function() {
        const valorFiltro = this.value;
        
        if (valorFiltro === '') {
            tabla.column(2).search('').draw(); // Limpiar filtro
        } else {
            tabla.column(2).search('^' + valorFiltro + '$', true, false).draw();
        }
    });
    
    // Filtro por rango de edad
    $('#filtro-edad-min, #filtro-edad-max').on('input', function() {
        const min = parseInt($('#filtro-edad-min').val()) || 0;
        const max = parseInt($('#filtro-edad-max').val()) || 999;
        
        // Filtro personalizado
        $.fn.dataTable.ext.search.push(function(settings, data, dataIndex) {
            if (settings.nTable.id !== 'tabla-con-filtros') {
                return true; // No aplicar a otras tablas
            }
            
            const edad = parseInt(data[3]) || 0;
            return edad >= min && edad <= max;
        });
        
        tabla.draw();
        
        // Remover filtro después de usar
        $.fn.dataTable.ext.search.pop();
    });
    
    // Filtro por estado activo/inactivo
    $('#filtro-activo').on('change', function() {
        const estado = this.value;
        
        if (estado === '') {
            tabla.column(5).search('').draw();
        } else {
            const valorBusqueda = estado === 'true' ? 'true' : 'false';
            tabla.column(5).search(valorBusqueda).draw();
        }
    });
    
    // Limpiar todos los filtros
    $('#limpiar-filtros').on('click', function() {
        $('#filtro-oficina').val('');
        $('#filtro-activo').val('');
        $('#filtro-edad-min').val('');
        $('#filtro-edad-max').val('');
        
        tabla.search('').columns().search('').draw();
        
        // Limpiar filtros personalizados
        $.fn.dataTable.ext.search = [];
        tabla.draw();
    });
});
```

### Edición inline

```javascript
$(document).ready(function() {
    const tabla = $('#tabla-editable').DataTable({
        "data": empleados,
        "columns": [
            { 
                "data": "nombre",
                "title": "Nombre",
                "className": 'editable'
            },
            { 
                "data": "cargo",
                "title": "Cargo",
                "className": 'editable'
            },
            { 
                "data": "oficina",
                "title": "Oficina",
                "className": 'editable'
            },
            { 
                "data": "edad",
                "title": "Edad",
                "className": 'editable'
            },
            {
                "data": null,
                "title": "Acciones",
                "render": function(data, type, row, meta) {
                    return `
                        <button class="btn btn-sm btn-primary btn-editar" data-row="${meta.row}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="btn btn-sm btn-success btn-guardar" data-row="${meta.row}" style="display:none;">
                            <i class="fas fa-save"></i>
                        </button>
                        <button class="btn btn-sm btn-secondary btn-cancelar" data-row="${meta.row}" style="display:none;">
                            <i class="fas fa-times"></i>
                        </button>
                    `;
                }
            }
        ]
    });
    
    // Datos originales para cancelar edición
    let datosOriginales = {};
    
    // Hacer fila editable
    $('#tabla-editable').on('click', '.btn-editar', function() {
        const fila = $(this).data('row');
        const $tr = tabla.row(fila).node();
        const datos = tabla.row(fila).data();
        
        // Guardar datos originales
        datosOriginales[fila] = { ...datos };
        
        // Convertir celdas editables a inputs
        $($tr).find('.editable').each(function(index) {
            const $celda = $(this);
            const valor = $celda.text();
            const nombreCampo = tabla.settings()[0].aoColumns[index].data;
            
            $celda.html(`<input type="text" class="form-control form-control-sm" value="${valor}" data-field="${nombreCampo}">`);
        });
        
        // Cambiar botones
        $(this).hide();
        $($tr).find('.btn-guardar, .btn-cancelar').show();
    });
    
    // Guardar cambios
    $('#tabla-editable').on('click', '.btn-guardar', function() {
        const fila = $(this).data('row');
        const $tr = tabla.row(fila).node();
        const datosActualizados = {};
        
        // Recopilar nuevos valores
        $($tr).find('.editable input').each(function() {
            const campo = $(this).data('field');
            const valor = $(this).val();
            datosActualizados[campo] = valor;
        });
        
        // Validar datos (ejemplo básico)
        if (!validarDatos(datosActualizados)) {
            alert('Por favor, complete todos los campos correctamente');
            return;
        }
        
        // Actualizar datos en la tabla
        const datosCompletos = { ...tabla.row(fila).data(), ...datosActualizados };
        tabla.row(fila).data(datosCompletos);
        
        // Simular guardado en servidor
        guardarEnServidor(datosCompletos).then(() => {
            mostrarMensaje('Registro actualizado correctamente', 'success');
        }).catch(() => {
            mostrarMensaje('Error al actualizar el registro', 'error');
        });
        
        restaurarFila($tr, fila);
    });
    
    // Cancelar edición
    $('#tabla-editable').on('click', '.btn-cancelar', function() {
        const fila = $(this).data('row');
        const $tr = tabla.row(fila).node();
        
        // Restaurar datos originales
        tabla.row(fila).data(datosOriginales[fila]);
        delete datosOriginales[fila];
        
        restaurarFila($tr, fila);
    });
    
    function restaurarFila($tr, fila) {
        // Redibujar la fila
        tabla.row(fila).invalidate().draw(false);
    }
    
    function validarDatos(datos) {
        return datos.nombre && datos.nombre.trim() !== '' &&
               datos.cargo && datos.cargo.trim() !== '' &&
               datos.oficina && datos.oficina.trim() !== '' &&
               datos.edad && !isNaN(datos.edad) && datos.edad > 0;
    }
    
    function guardarEnServidor(datos) {
        // Simular llamada AJAX
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                // Simular éxito/fallo
                if (Math.random() > 0.1) { // 90% de éxito
                    resolve(datos);
                } else {
                    reject(new Error('Error del servidor'));
                }
            }, 1000);
        });
    }
    
    function mostrarMensaje(mensaje, tipo) {
        const alertClass = tipo === 'success' ? 'alert-success' : 'alert-danger';
        const $mensaje = $(`
            <div class="alert ${alertClass} alert-dismissible fade show" role="alert">
                ${mensaje}
                <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
            </div>
        `);
        
        $('#mensajes').html($mensaje);
        
        // Auto-ocultar después de 3 segundos
        setTimeout(() => {
            $mensaje.alert('close');
        }, 3000);
    }
});
```

## Casos Prácticos Completos

### Dashboard de empleados con todas las funcionalidades

```javascript
class EmpleadosDashboard {
    constructor() {
        this.tabla = null;
        this.empleados = [];
        this.configuracion = {
            url: '/api/empleados',
            idioma: 'es'
        };
        
        this.inicializar();
    }
    
    async inicializar() {
        try {
            await this.cargarDatos();
            this.crearTabla();
            this.configurarEventos();
            this.configurarFiltros();
            
            console.log('✅ Dashboard inicializado correctamente');
        } catch (error) {
            console.error('❌ Error inicializando dashboard:', error);
            this.mostrarError('Error al inicializar el dashboard');
        }
    }
    
    async cargarDatos() {
        try {
            // Simular carga de datos desde API
            this.empleados = await this.simularAPI();
            console.log(`📊 Cargados ${this.empleados.length} empleados`);
        } catch (error) {
            throw new Error('Error cargando datos de empleados');
        }
    }
    
    crearTabla() {
        this.tabla = $('#tabla-empleados-dashboard').DataTable({
            "data": this.empleados,
            "dom": 'Bfrtip',
            
            // Botones de acción
            "buttons": [
                {
                    "text": '<i class="fas fa-plus"></i> Nuevo Empleado',
                    "className": 'btn btn-success btn-sm',
                    "action": () => this.nuevoEmpleado()
                },
                {
                    "extend": 'excel',
                    "text": '<i class="fas fa-file-excel"></i> Exportar Excel',
                    "className": 'btn btn-primary btn-sm',
                    "filename": () => `empleados_${new Date().toISOString().slice(0,10)}`
                },
                {
                    "text": '<i class="fas fa-sync"></i> Recargar',
                    "className": 'btn btn-secondary btn-sm',
                    "action": () => this.recargarDatos()
                }
            ],
            
            // Configuración de columnas
            "columns": [
                { 
                    "data": "id",
                    "title": "ID",
                    "width": "60px"
                },
                { 
                    "data": "foto",
                    "title": "Foto",
                    "orderable": false,
                    "searchable": false,
                    "render": (data) => {
                        return `<img src="${data}" alt="Foto" class="rounded-circle" width="40" height="40">`;
                    }
                },
                { 
                    "data": "nombre",
                    "title": "Nombre Completo"
                },
                { 
                    "data": "cargo",
                    "title": "Cargo"
                },
                { 
                    "data": "departamento",
                    "title": "Departamento"
                },
                { 
                    "data": "oficina",
                    "title": "Oficina"
                },
                { 
                    "data": "telefono",
                    "title": "Teléfono"
                },
                { 
                    "data": "email",
                    "title": "Email",
                    "render": (data) => {
                        return `<a href="mailto:${data}">${data}</a>`;
                    }
                },
                { 
                    "data": "fechaIngreso",
                    "title": "Fecha Ingreso",
                    "render": (data) => {
                        return moment(data).format('DD/MM/YYYY');
                    }
                },
                { 
                    "data": "salario",
                    "title": "Salario",
                    "render": (data) => {
                        return '€' + parseFloat(data).toLocaleString('es-ES');
                    }
                },
                { 
                    "data": "activo",
                    "title": "Estado",
                    "render": (data) => {
                        return data ? 
                            '<span class="badge bg-success">Activo</span>' : 
                            '<span class="badge bg-danger">Inactivo</span>';
                    }
                },
                {
                    "data": null,
                    "title": "Acciones",
                    "orderable": false,
                    "searchable": false,
                    "render": (data, type, row) => {
                        return `
                            <div class="btn-group" role="group">
                                <button class="btn btn-sm btn-outline-primary btn-ver" data-id="${row.id}" title="Ver detalles">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-sm btn-outline-success btn-editar" data-id="${row.id}" title="Editar">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-sm btn-outline-danger btn-eliminar" data-id="${row.id}" title="Eliminar">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        `;
                    }
                }
            ],
            
            // Configuración responsive
            "responsive": true,
            "columnDefs": [
                { "responsivePriority": 1, "targets": [0, 2] }, // ID y Nombre siempre visibles
                { "responsivePriority": 2, "targets": [3, 11] }  // Cargo y Acciones
            ],
            
            // Configuración de paginación
            "pageLength": 15,
            "lengthMenu": [[10, 15, 25, 50, -1], [10, 15, 25, 50, "Todos"]],
            
            // Ordenamiento por defecto
            "order": [[ 2, "asc" ]], // Por nombre
            
            // Idioma
            "language": this.obtenerConfiguracionIdioma()
        });
    }
    
    configurarEventos() {
        // Eventos de botones de acción
        $('#tabla-empleados-dashboard').on('click', '.btn-ver', (e) => {
            const id = $(e.currentTarget).data('id');
            this.verEmpleado(id);
        });
        
        $('#tabla-empleados-dashboard').on('click', '.btn-editar', (e) => {
            const id = $(e.currentTarget).data('id');
            this.editarEmpleado(id);
        });
        
        $('#tabla-empleados-dashboard').on('click', '.btn-eliminar', (e) => {
            const id = $(e.currentTarget).data('id');
            this.eliminarEmpleado(id);
        });
        
        // Evento de doble click para ver detalles
        $('#tabla-empleados-dashboard').on('dblclick', 'tr', (e) => {
            const datos = this.tabla.row(e.currentTarget).data();
            if (datos) {
                this.verEmpleado(datos.id);
            }
        });
    }
    
    configurarFiltros() {
        // Filtro por departamento
        $('#filtro-departamento').on('change', (e) => {
            const valor = $(e.target).val();
            this.tabla.column(4).search(valor).draw();
        });
        
        // Filtro por oficina
        $('#filtro-oficina').on('change', (e) => {
            const valor = $(e.target).val();
            this.tabla.column(5).search(valor).draw();
        });
        
        // Filtro por estado
        $('#filtro-estado').on('change', (e) => {
            const valor = $(e.target).val();
            if (valor === '') {
                this.tabla.column(10).search('').draw();
            } else {
                const busqueda = valor === 'activo' ? 'Activo' : 'Inactivo';
                this.tabla.column(10).search(busqueda).draw();
            }
        });
        
        // Limpiar filtros
        $('#limpiar-filtros').on('click', () => {
            $('#filtro-departamento, #filtro-oficina, #filtro-estado').val('');
            this.tabla.columns().search('').draw();
        });
    }
    
    // Acciones de CRUD
    verEmpleado(id) {
        const empleado = this.empleados.find(emp => emp.id === id);
        if (empleado) {
            this.mostrarModalDetalles(empleado);
        }
    }
    
    nuevoEmpleado() {
        this.mostrarModalFormulario();
    }
    
    editarEmpleado(id) {
        const empleado = this.empleados.find(emp => emp.id === id);
        if (empleado) {
            this.mostrarModalFormulario(empleado);
        }
    }
    
    async eliminarEmpleado(id) {
        const empleado = this.empleados.find(emp => emp.id === id);
        
        if (!empleado) return;
        
        const confirmacion = await this.confirmarAccion(
            `¿Está seguro de eliminar a ${empleado.nombre}?`,
            'Esta acción no se puede deshacer.'
        );
        
        if (confirmacion) {
            try {
                // Simular eliminación en servidor
                await this.simularEliminacionAPI(id);
                
                // Remover de datos locales
                this.empleados = this.empleados.filter(emp => emp.id !== id);
                
                // Actualizar tabla
                this.tabla.clear().rows.add(this.empleados).draw();
                
                this.mostrarNotificacion('Empleado eliminado correctamente', 'success');
            } catch (error) {
                this.mostrarNotificacion('Error al eliminar empleado', 'error');
            }
        }
    }
    
    async recargarDatos() {
        try {
            this.mostrarCargando(true);
            await this.cargarDatos();
            this.tabla.clear().rows.add(this.empleados).draw();
            this.mostrarNotificacion('Datos recargados correctamente', 'success');
        } catch (error) {
            this.mostrarNotificacion('Error al recargar datos', 'error');
        } finally {
            this.mostrarCargando(false);
        }
    }
    
    // Métodos auxiliares para UI
    mostrarModalDetalles(empleado) {
        $('#modal-detalles-empleado .modal-title').text(`Detalles de ${empleado.nombre}`);
        $('#modal-detalles-empleado .modal-body').html(`
            <div class="row">
                <div class="col-md-4 text-center">
                    <img src="${empleado.foto}" alt="Foto" class="rounded-circle mb-3" width="120" height="120">
                </div>
                <div class="col-md-8">
                    <table class="table table-borderless">
                        <tr><th>ID:</th><td>${empleado.id}</td></tr>
                        <tr><th>Nombre:</th><td>${empleado.nombre}</td></tr>
                        <tr><th>Cargo:</th><td>${empleado.cargo}</td></tr>
                        <tr><th>Departamento:</th><td>${empleado.departamento}</td></tr>
                        <tr><th>Oficina:</th><td>${empleado.oficina}</td></tr>
                        <tr><th>Teléfono:</th><td>${empleado.telefono}</td></tr>
                        <tr><th>Email:</th><td><a href="mailto:${empleado.email}">${empleado.email}</a></td></tr>
                        <tr><th>Fecha Ingreso:</th><td>${moment(empleado.fechaIngreso).format('DD/MM/YYYY')}</td></tr>
                        <tr><th>Salario:</th><td>€${empleado.salario.toLocaleString('es-ES')}</td></tr>
                        <tr><th>Estado:</th><td>
                            <span class="badge ${empleado.activo ? 'bg-success' : 'bg-danger'}">
                                ${empleado.activo ? 'Activo' : 'Inactivo'}
                            </span>
                        </td></tr>
                    </table>
                </div>
            </div>
        `);
        $('#modal-detalles-empleado').modal('show');
    }
    
    mostrarModalFormulario(empleado = null) {
        const esEdicion = empleado !== null;
        const titulo = esEdicion ? `Editar Empleado: ${empleado.nombre}` : 'Nuevo Empleado';
        
        $('#modal-formulario-empleado .modal-title').text(titulo);
        
        if (esEdicion) {
            // Rellenar formulario con datos existentes
            $('#form-empleado')[0].reset();
            Object.keys(empleado).forEach(key => {
                const $input = $(`#form-empleado [name="${key}"]`);
                if ($input.length) {
                    $input.val(empleado[key]);
                }
            });
        } else {
            $('#form-empleado')[0].reset();
        }
        
        $('#modal-formulario-empleado').modal('show');
    }
    
    confirmarAccion(titulo, mensaje) {
        return new Promise((resolve) => {
            $('#modal-confirmacion .modal-title').text(titulo);
            $('#modal-confirmacion .modal-body').text(mensaje);
            
            $('#modal-confirmacion .btn-confirmar').off('click').on('click', () => {
                $('#modal-confirmacion').modal('hide');
                resolve(true);
            });
            
            $('#modal-confirmacion .btn-cancelar').off('click').on('click', () => {
                $('#modal-confirmacion').modal('hide');
                resolve(false);
            });
            
            $('#modal-confirmacion').modal('show');
        });
    }
    
    mostrarNotificacion(mensaje, tipo) {
        const iconos = {
            success: 'fas fa-check-circle',
            error: 'fas fa-exclamation-circle',
            warning: 'fas fa-exclamation-triangle',
            info: 'fas fa-info-circle'
        };
        
        const colores = {
            success: 'alert-success',
            error: 'alert-danger',
            warning: 'alert-warning',
            info: 'alert-info'
        };
        
        const $notificacion = $(`
            <div class="alert ${colores[tipo]} alert-dismissible fade show" role="alert">
                <i class="${iconos[tipo]}"></i> ${mensaje}
                <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
            </div>
        `);
        
        $('#notificaciones').prepend($notificacion);
        
        setTimeout(() => {
            $notificacion.alert('close');
        }, 5000);
    }
    
    mostrarCargando(mostrar) {
        if (mostrar) {
            $('#loading').show();
        } else {
            $('#loading').hide();
        }
    }
    
    mostrarError(mensaje) {
        $('#error-container').html(`
            <div class="alert alert-danger" role="alert">
                <h4 class="alert-heading">Error</h4>
                <p>${mensaje}</p>
            </div>
        `).show();
    }
    
    obtenerConfiguracionIdioma() {
        return {
            "decimal": "",
            "emptyTable": "No hay información disponible en la tabla",
            "info": "Mostrando _START_ a _END_ de _TOTAL_ entradas",
            "infoEmpty": "Mostrando 0 a 0 de 0 entradas",
            "infoFiltered": "(filtrado de _MAX_ entradas totales)",
            "infoPostFix": "",
            "thousands": ",",
            "lengthMenu": "Mostrar _MENU_ entradas",
            "loadingRecords": "Cargando...",
            "processing": "Procesando...",
            "search": "Buscar:",
            "zeroRecords": "No se encontraron registros coincidentes",
            "paginate": {
                "first": "Primero",
                "last": "Último",
                "next": "Siguiente",
                "previous": "Anterior"
            }
        };
    }
    
    // Simulación de API
    async simularAPI() {
        return new Promise((resolve) => {
            setTimeout(() => {
                resolve([
                    {
                        id: 1,
                        foto: 'https://via.placeholder.com/40',
                        nombre: 'Juan Pérez García',
                        cargo: 'Desarrollador Senior',
                        departamento: 'Tecnología',
                        oficina: 'Madrid',
                        telefono: '+34 666 123 456',
                        email: 'juan.perez@empresa.com',
                        fechaIngreso: '2020-03-15',
                        salario: 45000,
                        activo: true
                    },
                    {
                        id: 2,
                        foto: 'https://via.placeholder.com/40',
                        nombre: 'María García López',
                        cargo: 'Diseñadora UX',
                        departamento: 'Diseño',
                        oficina: 'Barcelona',
                        telefono: '+34 666 234 567',
                        email: 'maria.garcia@empresa.com',
                        fechaIngreso: '2021-07-10',
                        salario: 38000,
                        activo: true
                    },
                    {
                        id: 3,
                        foto: 'https://via.placeholder.com/40',
                        nombre: 'Carlos López Martín',
                        cargo: 'Project Manager',
                        departamento: 'Gestión',
                        oficina: 'Valencia',
                        telefono: '+34 666 345 678',
                        email: 'carlos.lopez@empresa.com',
                        fechaIngreso: '2019-11-22',
                        salario: 52000,
                        activo: false
                    }
                    // Agregar más datos de ejemplo...
                ]);
            }, 1000);
        });
    }
    
    async simularEliminacionAPI(id) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                // Simular éxito/fallo
                if (Math.random() > 0.1) {
                    resolve({ success: true });
                } else {
                    reject(new Error('Error del servidor'));
                }
            }, 500);
        });
    }
}

// Inicializar dashboard cuando el documento esté listo
$(document).ready(function() {
    window.empleadosDashboard = new EmpleadosDashboard();
});
```

## Mejores Prácticas

### ✅ Buenas prácticas

```javascript
// 1. Configurar idioma apropiadamente
$('#tabla').DataTable({
    "language": {
        "url": "//cdn.datatables.net/plug-ins/1.13.6/i18n/Spanish.json"
    }
});

// 2. Usar responsive design
$('#tabla').DataTable({
    "responsive": true,
    "columnDefs": [
        { "responsivePriority": 1, "targets": [0, -1] } // Primera y última columna siempre visibles
    ]
});

// 3. Optimizar para grandes datasets
$('#tabla').DataTable({
    "serverSide": true,      // Procesamiento del servidor
    "deferRender": true,     // Renderizado diferido
    "scrollY": "400px",      // Scroll vertical
    "scrollCollapse": true,  // Colapsar scroll si hay pocos datos
    "scroller": true         // Virtual scrolling
});

// 4. Manejar errores apropiadamente
$('#tabla').DataTable({
    "ajax": {
        "url": "/api/datos",
        "error": function(xhr, error, code) {
            console.error('Error DataTable:', error);
            mostrarNotificacionError('Error cargando datos');
        }
    }
});

// 5. Limpiar recursos al destruir
function destruirTabla() {
    if ($.fn.DataTable.isDataTable('#tabla')) {
        $('#tabla').DataTable().destroy();
        $('#tabla').empty();
    }
}
```

### ❌ Prácticas a evitar

```javascript
// ❌ NO inicializar múltiples veces
$('#tabla').DataTable(); // Primera inicialización
$('#tabla').DataTable(); // ¡Error! Ya está inicializada

// ✅ Verificar si ya está inicializada
if (!$.fn.DataTable.isDataTable('#tabla')) {
    $('#tabla').DataTable();
}

// ❌ NO cargar todos los datos en el cliente para datasets grandes
$('#tabla').DataTable({
    "ajax": "/api/millones-de-registros" // Puede bloquear el navegador
});

// ✅ Usar server-side processing para datasets grandes
$('#tabla').DataTable({
    "serverSide": true,
    "ajax": "/api/datos-paginados"
});

// ❌ NO olvidar limpiar event listeners
// Esto puede causar memory leaks
$('#tabla').on('click', '.btn-accion', function() {
    // Handler sin limpiar
});

// ✅ Usar delegación de eventos correctamente
$('#tabla').off('click', '.btn-accion').on('click', '.btn-accion', function() {
    // Handler limpio
});
```

***

DataTables es una herramienta extremadamente poderosa que puede transformar completamente la experiencia de usuario al trabajar con datos tabulares. Con la configuración adecuada, puedes crear interfaces de datos profesionales y altamente funcionales con relativamente poco código.

> **🚀 Próximos pasos**: Explora las extensiones avanzadas como Editor (para edición inline completa), FixedColumns, FixedHeader, y la integración con frameworks modernos como React, Vue o Angular.
