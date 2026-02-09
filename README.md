# Sistema de Gestión de Tareas (To-Do List) - Proyecto Dockerizado

Este proyecto es una aplicación de tareas de tres capas (Fullstack) diseñada para demostrar la orquestación de microservicios mediante **Docker Compose**. La arquitectura permite un despliegue rápido, seguro y persistente.

---

## Arquitectura de Servicios

La aplicación se divide en tres contenedores independientes:

1.  **Frontend (Nginx):** Servidor web que entrega la interfaz de usuario (HTML, CSS, JS). Configurado con un *Bind Mount* para desarrollo en tiempo real.
2.  **Backend (Node.js):** API REST que gestiona la lógica de las tareas y la conexión a la base de datos.
3.  **Base de Datos (PostgreSQL):** Motor de base de datos relacional que utiliza un *Volumen* para garantizar que los datos no se pierdan.

---

## Configuración del Entorno (.env)

Por seguridad, las credenciales no están incluidas en el código. **Debes crear un archivo llamado `.env` en la raíz del proyecto** con el siguiente contenido:

```env
# -# Configuración de PostgreSQL
POSTGRES_USER=tu_usuario
POSTGRES_PASSWORD=tu_contraseña_segura
POSTGRES_DB=nombre_base_datos

# Configuración del Backend
DB_HOST=database
DB_PORT=5432
BACKEND_PORT=4000
```
## Comandos para el Despliegue

Abre tu terminal en la carpeta raíz del proyecto y utiliza los siguientes comandos de Docker para gestionar el ciclo de vida de la aplicación:

| Acción | Comando | Descripción |
| :--- | :--- | :--- |
| **Levantar la App** | `docker compose up -d --build` | Construye las imágenes y levanta los servicios en segundo plano. |
| **Detener la App** | `docker compose stop` | Detiene los contenedores sin eliminarlos. |
| **Apagar y Limpiar** | `docker compose down` | Detiene y elimina los contenedores y las redes. |
| **Reset Total** | `docker compose down -v` | **Cuidado**: Borra contenedores y el volumen de datos (limpia la BD). |
| **Ver Logs** | `docker compose logs -f backend` | Muestra la salida de consola del servidor Node.js en tiempo real. |

---

## Enlaces de Acceso

Una vez que los contenedores estén en estado **Running**, puedes interactuar con el sistema a través de las siguientes URLs:

* **Aplicación Web (Interfaz):** [http://localhost:8080](http://localhost:8080)
* **API de Tareas (JSON):** [http://localhost:4000/tasks](http://localhost:4000/tasks)
* **Estado del Servidor (Health):** [http://localhost:4000/health](http://localhost:4000/health)

---

## Detalles de Implementación Técnica

Para cumplir con los requisitos del proyecto, se han aplicado las siguientes configuraciones de Docker:

* **Persistencia (Volumes):** Se utiliza el volumen `db_data` vinculado a `/var/lib/postgresql/data` para que las tareas no se borren al reiniciar los contenedores.
* **Desarrollo en vivo (Bind Mounts):** La carpeta `./frontend` está montada directamente en el contenedor de Nginx. Esto permite editar el código HTML/CSS y ver los cambios al refrescar el navegador sin reconstruir la imagen.
* **Redes (Networks):** Se ha definido una red personalizada llamada `todo_network` que aísla la comunicación entre el Backend y la Base de Datos, mejorando la seguridad.
* **Orquestación:** El servicio del backend incluye una instrucción `depends_on` con `condition: service_healthy`, asegurando que Node.js no intente conectar con la base de datos hasta que PostgreSQL esté totalmente listo.

---
