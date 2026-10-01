# Integrantes 
### William Villa
### Alejandro Ibarra
### Juan Pablo Rubiano
---

# Diagrama de clase Mermaid

```mermaid
classDiagram
    direction TB

    %% -----------------------------------------
    %% 1. JERARQUÍA DE USUARIOS
    %% -----------------------------------------
    class Usuario {
        #String idUsuario
        #String nombre
        #String apellido
        #String correo
        #String telefono
        #String contrasena
        #String estado
        +Usuario()
        +boolean iniciarSesion()
        +void cerrarSesion()
        +void actualizarDatos()
    }

    class Administrador {
        -String nivelAcceso
        -String departamento
        +Administrador()
        +void gestionarUsuarios()
        +void configurarSistema()
        +void generarReportesGlobales()
    }

    class Bibliotecario {
        -String turno
        -String codigoEmpleado
        +Bibliotecario()
        +void registrarPrestamo()
        +void registrarDevolucion()
        +void verificarInventario()
    }

    class Lector {
        -String codigoEstudiantilOColaborador
        -String tipoLector
        -int limitePrestamos
        +Lector()
        +void consultarCatalogo()
        +void verHistorialPrestamos()
    }

    Usuario <|-- Administrador
    Usuario <|-- Bibliotecario
    Usuario <|-- Lector

    %% -----------------------------------------
    %% 2. GESTIÓN DE CATÁLOGO Y LIBROS
    %% -----------------------------------------
    class Libro {
        #String isbn
        #String titulo
        #String autor
        #String editorial
        #int anioPublicacion
        #int stockTotal
        #int stockDisponible
        +Libro()
        +boolean verificarDisponibilidad()
        +void actualizarStock()
    }

    %% -----------------------------------------
    %% 3. GESTIÓN DE PRÉSTAMOS Y MULTAS
    %% -----------------------------------------
    class Prestamo {
        -String idPrestamo
        -String fechaPrestamo
        -String fechaDevolucionTentativa
        -String fechaDevolucionReal
        -String estadoPrestamo
        +Prestamo()
        +void calcularFechaLimite()
        +void marcarComoDevuelto()
    }

    class Multa {
        -String idMulta
        -int diasRetraso
        -double montoTotal
        -String estadoPago
        +Multa()
        +double calcularMonto()
        +void actualizarEstadoPago()
    }

    %% -----------------------------------------
    %% 4. CAPAS MVC Y BASE DE DATOS
    %% -----------------------------------------
    class ConexionBD {
        -String url
        -String usuario
        -String contrasena
        -ConexionBD instancia
        -ConexionBD()
        +static ConexionBD obtenerInstancia()
        +void conectar()
        +void desconectar()
        +void ejecutarConsulta()
    }

    class ControladorLogin {
        +boolean autenticar(String correo, String contrasena)
        +String validarRol(Usuario usuario)
    }

    class ControladorLibros {
        +void agregarLibro(Libro libro)
        +Libro buscarLibro(String isbn)
        +void actualizarLibro(Libro libro)
        +void eliminarLibro(String isbn)
    }

    class ControladorPrestamos {
        +void registrarPrestamo(Lector lector, Libro libro)
        +void registrarDevolucion(Prestamo prestamo)
        +void calcularMulta(Prestamo prestamo)
    }

    class VistaLogin {
        +void mostrarVentana()
        +void capturarCredenciales()
    }

    class VistaAdminDashboard {
        +void mostrarPanelAdmin()
        +void desplegarReportes()
    }

    class VistaBibliotecarioDashboard {
        +void mostrarPanelPrestamos()
        +void actualizarTablas()
    }

    class VistaCatalogo {
        +void mostrarLibros()
        +void filtrarBusqueda()
    }

    %% -----------------------------------------
    %% RELACIONES ENTRE CLASES
    %% -----------------------------------------
    Lector "1" --> "*" Prestamo : realiza
    Bibliotecario "1" --> "*" Prestamo : aprueba
    Prestamo "*" --> "1" Libro : incluye
    Prestamo "1" --> "0..1" Multa : genera
    
    ```