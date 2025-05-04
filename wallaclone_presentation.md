# Presentación del proyecto Wallaclone

**Wallaclone** es una plataforma web diseñada para facilitar la compra y venta de artículos de segunda mano entre particulares, inspirada en aplicaciones como Wallapop. El sistema ofrece una experiencia completa tanto para usuarios anónimos como para miembros registrados, permitiendo la gestión de anuncios, favoritos, perfiles y comunicación directa a través de un sistema de chat en tiempo real. Actualmente, todas las funcionalidades están implementadas salvo el sistema de notificaciones, que está en desarrollo.

## 🧩 Patrones y arquitectura de diseño

El proyecto está basado principalmente en el **patrón de arquitectura MVC (Modelo-Vista-Controlador)**:
* **Modelo**: Define las estructuras de datos en MongoDB (por ejemplo, `Advert`, `User`, `Chat`, `Message`).
* **Controlador**: Contiene la lógica de negocio, validación, y manipulación de datos. Divide claramente responsabilidades entre cada tipo de entidad.
* **Vista**: En el frontend, las vistas están desarrolladas con **React**, donde cada página o componente representa una parte visual del sistema.

Además, se ha aplicado una **separación de responsabilidades clara y modular** que recuerda a principios de la **Clean Architecture**:
* **Frontend**: Dividido por funcionalidades (auth, chat, adverts, user) y con servicios centralizados para las llamadas API (`chat-service.js`, `auth-service.js`, etc.).
* **Backend**: Rutas agrupadas, middlewares especializados, controladores por dominio y lógica desacoplada.

Se ha respetado el enfoque **full-stack feature-driven**, en el que una misma persona desarrolla una funcionalidad de extremo a extremo (del backend al frontend).

## 👥 Organización del equipo

Somos un equipo de **4 personas** que ha trabajado en igualdad de condiciones: **todos hemos actuado como líderes técnicos y managers** en diferentes momentos. Cada integrante ha asumido la responsabilidad de definir, desarrollar y documentar funcionalidades completas, desde la base de datos hasta la interfaz de usuario.

## 🗂️ Gestión y seguimiento del trabajo

* Usamos **Kanban Flow** como sistema dinámico para organizar el estado de las tareas principales.
* Llevamos un **Excel compartido y en constante actualización**, donde registramos:
   * Tareas realizadas
   * Bugs detectados
   * Observaciones de UX/UI
   * Estado actual (por hacer, en progreso, en revisión, terminado)
   * Fecha de creación, responsables y prioridad

## 💬 Comunicación y documentación interna

Toda la comunicación y documentación del proyecto está organizada en un **servidor de Discord estructurado por canales**, separados por áreas clave:
* **Backend**
   * Avances por feature
   * Errores comunes y soluciones
   * Librerías utilizadas y decisiones técnicas
   * Enlaces a documentación técnica
* **Frontend**
   * Prototipos y diseño UI
   * Implementación de componentes
   * Estados y props compartidos
   * Feedback visual y bugs de interfaz
* **Deploy**
   * Comandos, scripts y entornos
   * Registro de pruebas en producción
   * Hosting, dominios, DNS y SSL
* **Git**
   * Flujo de ramas: `main` > `develop` > `feature/*`
   * Resolución de conflictos
   * Buenas prácticas de commits
   * Registro de merges y revisiones

También incluimos:
* **Diarios de seguimiento** individuales y por equipo.
* **Actas de reuniones internas**, decisiones tomadas y próximas tareas.
* **Canal de avisos urgentes** para bloqueos o incidencias críticas.

## Despliegue en producción (AWS + Nginx + Certbot)

El proyecto **Wallaclone** está desplegado en **AWS EC2**, utilizando **dos instancias separadas** para frontend y backend:
* 🔧 **Backend** (Node.js + Express): Corre localmente en la instancia, sin exponer puertos públicos. Está protegido por **CORS**, aceptando peticiones únicamente desde:
* `const allowedOrigins = [ 'https://bananapeels.duckdns.org', 'http://localhost:5173', ];`
* Se configuró NGINX como proxy inverso con rutas `/api/` y `/socket.io/` apuntando al backend en el puerto 4444, permitiendo tráfico HTTP y WebSockets. El tráfico está asegurado con HTTPS mediante Certbot y se redirige todo HTTP a HTTPS automáticamente.

Todas las peticiones externas son gestionadas a través de **Nginx como proxy inverso**.
* 🖥️ **Frontend** (React + Vite): Compilado y servido como sitio estático desde Nginx.

Se ha asignado una **Elastic IP** fija a la instancia y configurado un dominio dinámico con **DuckDNS**: 🔗 `https://bananapeels.duckdns.org/`

## 🔐 HTTPS con Certbot

Ambas instancias están protegidas con **certificados SSL gratuitos de Let's Encrypt** mediante **Certbot**. Nginx está configurado para redirigir automáticamente todas las peticiones HTTP a HTTPS.

## 🔄 Actualización del código

Cada instancia incluye un **script interno (**`update.sh`) que automatiza:
* Pull del repositorio (`git pull`)
* Instalación de dependencias (`npm install`)
* Reinicio del backend con **Supervisor** o build del frontend
* Recarga de Nginx si es necesario