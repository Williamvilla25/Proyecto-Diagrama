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

    class Profesor {
        -String especialidad
        -String codigoProfesor
        +Profesor()
        +void registrarNotas()
        +void verMateriasAsignadas()
    }

    class Estudiante {
        -String codigoEstudiantil
        -String programaAcademico
        -int semestreActual
        +Estudiante()
        +void consultarNotas()
        +void verHistorialAcademico()
    }

    Usuario <|-- Administrador
    Usuario <|-- Profesor
    Usuario <|-- Estudiante

    %% -----------------------------------------
    %% 2. GESTIÓN ACADÉMICA Y MATERIAS
    %% -----------------------------------------
    class Materia {
        -String codigoMateria
        -String nombreMateria
        -int creditos
        -int semestre
        +Materia()
        +String getCodigoMateria()
        +void setCodigoMateria(String codigoMateria)
        +String getNombreMateria()
        +void setNombreMateria(String nombreMateria)
        +void imprimirInformacion()
    }

    class Matricula {
        -String idMatricula
        -String fechaMatricula
        -String estado
        +Matricula()
        +void registrarMatricula()
        +void cancelarMatricula()
    }

    %% -----------------------------------------
    %% 3. GESTIÓN DE CALIFICACIONES (TRANSACCIONAL)
    %% -----------------------------------------
    class Calificacion {
        -String idCalificacion
        -double notaParcial1
        -double notaParcial2
        -double notaFinal
        -String observaciones
        +Calificacion()
        +double calcularPromedioFinal()
        +boolean verificarAprobacion()
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

    class ControladorAcademico {
        +void matricularMateria(Estudiante estudiante, Materia materia)
        +void gestionarMaterias(Materia materia)
    }

    class ControladorNotas {
        +void registrarCalificacion(Estudiante estudiante, Materia materia, double nota)
        +void actualizarCalificacion(Calificacion calificacion)
    }

    class VistaLogin {
        +void mostrarVentana()
        +void capturarCredenciales()
    }

    class VistaAdminDashboard {
        +void mostrarPanelAdmin()
        +void desplegarReportesGlobales()
    }

    class VistaProfesorDashboard {
        +void mostrarPanelNotas()
        +void actualizarTablaEstudiantes()
    }

    class VistaEstudianteDashboard {
        +void mostrarCalificaciones()
        +void filtrarMaterias()
    }

    %% -----------------------------------------
    %% RELACIONES ENTRE CLASES
    %% -----------------------------------------
    Estudiante "1" --> "*" Matricula : realiza
    Profesor "1" --> "*" Materia : dicta
    Matricula "*" --> "1" Materia : incluye
    Estudiante "1" --> "*" Calificacion : posee
    Materia "1" --> "*" Calificacion : evalua
    
    ```