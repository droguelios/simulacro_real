📘 README.md — SISTEMA WEB CRUD REUTILIZABLE
📌 Descripción del sistema

Este proyecto es una aplicación web SPA desarrollada con HTML, CSS y JavaScript puro, que simula un sistema de gestión adaptable a diferentes dominios (restaurante, biblioteca, tienda, cine, etc.).

El sistema implementa:

Gestión de usuarios con roles

Visualización de catálogo (menú, libros, productos, etc.)

Creación y seguimiento de pedidos / préstamos / solicitudes

Panel administrativo para control del flujo

Persistencia de datos simulada

La lógica del sistema es genérica y reutilizable, permitiendo cambiar el tema sin modificar la estructura principal.

👥 Roles del sistema
Usuario

Visualiza el catálogo

Realiza pedidos / solicitudes

Consulta solo SUS registros

Visualiza su perfil

Administrador

Visualiza TODOS los registros

Cambia estados del proceso

Gestiona el flujo general del sistema

🧠 Estructura de datos base
Usuario
{
  id,
  name,
  email,
  role // "admin" | "user"
}

Elemento del catálogo (tema adaptable)
{
  id,
  name,
  price,
  category
}


Ejemplo de adaptación:

Restaurante → platos

Biblioteca → libros

Cine → boletas

Tienda → productos

Pedido / Solicitud
{
  id,
  userId,
  items: [],
  total,
  status,
  createdAt
}

🔄 Estados del proceso

Los estados son configurables según el dominio del sistema.

Ejemplo base:

pendiente → preparando → listo → entregado


Ejemplo biblioteca:

solicitado → aprobado → prestado → devuelto


Los estados se definen en un solo lugar del código, permitiendo fácil adaptación.

🗂️ Estructura del proyecto
/Proyecto
  index.html
  styles.css
  app.js
  data.json (opcional)
  README.md

🔧 Cómo cambiar el tema del sistema (IMPORTANTE)

Para adaptar el sistema a otro contexto (biblioteca, tienda, cine, etc.), solo se deben modificar:

Datos iniciales del catálogo

Estados del proceso

Textos visibles (labels)

La lógica de:

autenticación

control de roles

renderizado

persistencia
no se modifica.

Esto garantiza un diseño desacoplado y reutilizable.

💾 Persistencia de datos

El sistema utiliza:

LocalStorage para persistir:

Usuarios

Pedidos

Sesión activa

JSON simulado como fuente inicial de datos (opcional)

🧪 Inicialización de JSON (cuando lo pidan)

Si el ejercicio solicita inicializar datos desde un archivo JSON:

Crear archivo data.json

{
  "users": [],
  "items": [],
  "orders": []
}


Cargarlo desde JavaScript usando fetch() (modo servidor).

🚀 Uso de servidor local (cuando lo pidan)

Para permitir carga de archivos JSON, se requiere un servidor local.

Opción 1: Node.js
npm init -y
npm install -g serve
serve .

Opción 2: Live Server (VS Code)

Click derecho en index.html

Open with Live Server

▶️ Cómo ejecutar el proyecto

Abrir la carpeta del proyecto

Iniciar servidor local

Acceder desde el navegador

Iniciar sesión con un usuario existente

El sistema redirige automáticamente según el rol

🧠 Flujo de uso

Login de usuario

Visualización del catálogo

Creación de pedido / solicitud

Confirmación

Administrador gestiona estados

Usuario visualiza actualizaciones

📌 Consideraciones finales

Este sistema está diseñado con separación entre:

lógica

datos

presentación

Lo que permite reutilizar el código base para distintos contextos sin reescribir la aplicación.

🏁 Conclusión (frase clave)

“La lógica del sistema es genérica; el dominio se adapta mediante configuración.”


