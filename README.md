# Sistema de Gestión de Aprendizaje (LMS) - Learning-Management-System-LMS
Una API backend para un sistema de gestión de aprendizaje diseñada para administrar y organizar cursos, evaluaciones (quizzes, tareas), inscripción de estudiantes, seguimiento del rendimiento y calificaciones. Construida con Java Spring Boot, implementa una arquitectura por capas para escalabilidad y separación de responsabilidades. Proporciona autenticación robusta de usuarios, gestión de contenido del curso y monitoreo del progreso.

## Funcionalidades

### 1. Autenticación de Usuarios
- Registro e inicio de sesión: Utilizando tokens JWT.
- Acceso basado en roles: Restringe los permisos de acceso según el tipo de rol.

### 2. Gestión de Usuarios
- Tipos de usuarios: Administrador, Instructor, Estudiante.
- Gestión de perfil: Ver y actualizar detalles del perfil.
- Funciones del Administrador:
    - Administrar configuraciones generales del sistema.
    - Crear y gestionar usuarios.
    - Crear y gestionar cursos.
- Funciones del Instructor:
    - Crear y gestionar cursos y su contenido.
    - Agregar tareas y cuestionarios.
    - Calificar a los estudiantes y proporcionar retroalimentación.
    - Eliminar estudiantes de los cursos.
- Funciones del Estudiante:
    - Inscribirse en cursos.
    - Acceder a materiales del curso.
    - Entregar tareas y presentar cuestionarios.
    - Ver calificaciones de tareas y cuestionarios.

### 3. Gestión de Cursos
- Creación de cursos: Los cursos pueden crearse con título, descripción y duración.
- Consulta de cursos: Los cursos pueden ser explorados por instructores y estudiantes.
- Inscripción a cursos: Un estudiante puede inscribirse en cualquier curso disponible.
- Gestión de lecciones: Cada curso puede tener múltiples lecciones accesibles para instructores y estudiantes inscritos.
- Recursos de lecciones: Cada lección puede incluir varios recursos (videos, PDF, audio).
- Asistencia a lecciones: El instructor genera OTPs para registrar asistencia. Los estudiantes la confirman ingresando el OTP.

### 4. Evaluaciones y Calificaciones
- Banco de preguntas: Cada curso tiene un banco de preguntas donde los instructores pueden agregar diferentes tipos de preguntas (Opción múltiple, verdadero/falso, respuestas cortas).
- Creación de evaluaciones: Los instructores pueden publicar quizzes y tareas.
- Envío de quizzes: Cada estudiante recibe preguntas aleatorias del banco de preguntas.
- Envío de tareas: Los estudiantes suben archivos que serán calificados.
- Calificación: Automática para quizzes; revisión manual para tareas.
- Seguimiento de rendimiento: Los estudiantes pueden monitorear sus calificaciones, entregas y asistencia.

### 5. Notificaciones
- Notificaciones a estudiantes: Reciben alertas por correo y en la plataforma por inscripciones, tareas calificadas y actualizaciones de cursos.
- Notificaciones a instructores: Reciben alertas por nuevos inscritos y cambios en sus cursos.

---

## Endpoints de la API

### Autenticación
- **Registrar:** `POST /register` - Registra un nuevo usuario con rol, nombre, correo y contraseña.
- **Iniciar sesión:** `POST /login` - Autentica al usuario y retorna un token JWT.

### Usuarios
- **Obtener perfil:** `GET /profile` - Recupera el perfil del usuario autenticado.
- **Actualizar perfil:** `PUT /profile` - Actualiza el perfil del usuario autenticado.
- **Obtener todos los usuarios:** `GET /users` - Lista todos los usuarios.
- **Obtener usuario por ID:** `GET /users/{id}` - Detalles de un usuario específico por ID.
- **Crear usuario:** `POST /users/create` - Crea un nuevo usuario con los datos proporcionados.
- **Eliminar usuario:** `DELETE /users/delete/{id}` - Elimina un usuario por ID.
- **Notificaciones del usuario:** `GET /notifications` - Lista todas las notificaciones del usuario autenticado.

### Cursos
- **Crear curso:** `POST /create-course` - Crea un nuevo curso (solo Instructor).
- **Buscar curso:** `GET /search-course/{courseName}` - Busca por nombre del curso.
- **Obtener curso por ID:** `GET /course/{id}` - Recupera detalles de un curso por ID.
- **Listar todos los cursos:** `GET /get-all-courses` - Lista todos los cursos.
- **Mis cursos:** `GET /get-my-courses` - Lista cursos creados o inscritos por el usuario.

### Lecciones
- **Agregar lección:** `POST /course/{courseName}/add-lesson` - Agrega una lección al curso (solo Instructor).
- **Listar lecciones:** `GET /course/{courseName}/lessons` - Lista lecciones del curso.
- **Ver lección:** `GET /course/{courseName}/lessons/{lessonId}` - Detalles de una lección específica.
- **Agregar recurso:** `POST /course/{courseName}/lessons/{lessonId}/add-resource` - Agrega archivo a una lección (solo Instructor).
- **Listar recursos:** `GET /course/{courseName}/lessons/{lessonId}/resources` - Lista de recursos por lección.
- **Ver recurso:** `GET /course/{courseName}/lessons/{lessonId}/resources/{resourceId}` - Descarga un recurso específico.

### Inscripciones
- **Inscribirse:** `POST /course/{courseName}/enroll` - Inscribirse a un curso (solo Estudiante).
- **Ver inscritos:** `GET /course/{courseName}/enrolled` - Lista de estudiantes inscritos (solo Instructor).
- **Eliminar estudiante:** `DELETE /course/{courseName}/remove-student/{studentId}` - Elimina a un estudiante (solo Instructor).

### Cuestionarios
- **Crear pregunta:** `POST /course/{course-name}/create-question` - Crea una nueva pregunta para un curso.
- **Obtener preguntas:** `GET /course/{course-name}/get-questions` - Lista todas las preguntas del curso.
- **Obtener pregunta por ID:** `GET /course/{course-name}/get-question-by-id` - Ver pregunta específica por ID.
- **Crear quiz:** `POST /course/{course-name}/create-quiz` - Crea un nuevo quiz para un curso.
- **Tomar quiz:** `GET /course/{course-name}/{quizName}/take-quiz` - Inicia un quiz (solo Estudiante).
- **Enviar quiz:** `POST /course/{course-name}/{quizName}/submit-quiz` - Envío de respuestas del quiz.

### Tareas
- **Crear tarea:** `POST /course/{course-name}/create-assignment` - Crea una nueva tarea para el curso.
- **Ver tareas:** `GET /course/{course-name}/assignments` - Lista todas las tareas del curso.
- **Ver tarea específica:** `GET /course/{course-name}/assignment/{assignment_id}/view` - Detalles de una tarea específica.
- **Entregar tarea:** `POST /course/{course-name}/assignment/{assignment_id}/submit` - Entrega de tarea.
- **Ver entregas:** `GET /course/{course-name}/assignment/{assignment_id}/submissions` - Lista de entregas de una tarea.
- **Ver entrega específica:** `GET /course/{course-name}/assignment/{assignment_id}/submission/{submission_id}` - Ver entrega de un estudiante.

### Calificaciones y Rendimiento
- **Ver calificación de quiz:** `GET /course/{course-name}/{quizName}/grade` - Calificación de un quiz específico.
- **Calificar tarea:** `PUT /course/{course-name}/assignment/{assignment_id}/submission/{submission_id}/grade` - Califica una tarea.
- **Ver calificación de tarea:** `GET /course/{course-name}/assignment/{assignment_id}/get-grade` - Calificación de una tarea específica.

---

## Primeros Pasos para realizar las pruebas e instalación 

### **Requisitos previos**
- Java 17+
- Spring Boot
- Maven
- PostgreSQL

### **Instalación**
#### 1. Clonar el repositorio:
```bash
git clone https://github.com/BrendaBravo21/Learning-Management-System-LMS-.git
cd Learning-Management-System
```

#### 2. Configurar `application.yml`:
- Agrega la información de tu base de datos en la propiedad `datasource`.
- Agrega la información de correo en la propiedad `mail`.

#### 3. Compilar el proyecto:
```bash
mvn clean install
```

#### 4. Ejecutar la aplicación:
```bash
mvn spring-boot:run
```
O ejecuta `Application.java` desde el IDE que estés utilizando.

