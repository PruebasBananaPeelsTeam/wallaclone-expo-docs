# Presentación del Proyecto Wallaclone

![Wallaclone Logo](https://via.placeholder.com/150)

## 📱 Descripción General

**Wallaclone** es una plataforma web de compra-venta de artículos de segunda mano entre particulares, inspirada en aplicaciones como Wallapop. El sistema ofrece una experiencia completa que incluye:

- Navegación de anuncios para usuarios anónimos
- Sistema completo de registro y autenticación
- Gestión de anuncios (creación, edición, eliminación)
- Sistema de favoritos
- Perfiles de usuario personalizables
- Sistema de chat en tiempo real entre compradores y vendedores
- *(En desarrollo)* Sistema de notificaciones

## 🧩 Arquitectura y Patrones de Diseño

### MVC (Modelo-Vista-Controlador)
El proyecto está estructurado siguiendo el patrón MVC:

- **Modelo**: Esquemas de MongoDB (Advert, User, Chat, Message)
- **Controlador**: Lógica de negocio, validación y manipulación de datos
- **Vista**: Componentes React que muestran la interfaz al usuario

### Clean Architecture
Se ha implementado una separación clara de responsabilidades:

#### Frontend (React + Vite)
- Organización por funcionalidades:
  - `/auth`: Componentes de registro, login y recuperación
  - `/adverts`: Listado, detalle y gestión de anuncios
  - `/user`: Perfil de usuario y preferencias
  - `/chat`: Sistema de mensajería en tiempo real
- Servicios API centralizados:
  - `auth-service.js`
  - `adverts-service.js`
  - `chat-service.js`
  - `user-service.js`

#### Backend (Node.js + Express)
- API RESTful con recursos claramente definidos
- Middlewares especializados (autenticación, validación, manejo de errores)
- Controladores específicos por dominio
- Servicios de lógica de negocio desacoplados
- Gestión de base de datos MongoDB

### Enfoque Full-Stack Feature-Driven
Cada desarrollador ha sido responsable de funcionalidades completas:
- Diseño del modelo de datos
- Implementación de la API backend
- Desarrollo de componentes frontend
- Pruebas de integración

## 👥 Organización del Equipo

Equipo de **4 desarrolladores** con roles dinámicos:
- Liderazgo técnico rotativo
- Responsabilidades compartidas y equitativas
- Toma de decisiones consensuada
- Revisión de código entre pares

## 🗂️ Gestión y Seguimiento del Trabajo

### Metodología
- **Kanban Flow** para visualización de tareas
- Sprints semanales con reuniones de revisión
- Flujo de trabajo ágil adaptado a las necesidades del equipo

### Herramientas
- **Excel compartido** como registro central con:
  - Tareas y subtareas
  - Estado actual (por hacer, en progreso, en revisión, terminado)
  - Responsables y fechas
  - Prioridades y dependencias
  - Registro de bugs y mejoras

## 💬 Comunicación y Documentación

### Servidor Discord Estructurado
- **Backend**:
  - Avances por funcionalidad
  - Documentación técnica
  - Soluciones a problemas comunes
- **Frontend**:
  - Prototipos UI/UX
  - Componentes compartidos
  - Feedback visual
- **Deploy**:
  - Procedimientos de despliegue
  - Configuración de servidores
  - Pruebas en producción
- **Git**:
  - Flujo de ramas (`main` > `develop` > `feature/*`)
  - Resolución de conflictos
  - Buenas prácticas

### Documentación Interna
- Diarios de seguimiento
- Actas de reuniones
- Decisiones técnicas
- Canal de avisos urgentes

## 🚀 Despliegue en Producción

### Infraestructura AWS
- **2 instancias EC2** separadas:
  - Backend (Node.js + Express)
  - Frontend (React + Vite)
- **Elastic IP** asignada
- Dominio configurado con **DuckDNS**: [bananapeels.duckdns.org](https://bananapeels.duckdns.org/)

### Configuración de Seguridad
- **CORS** configurado para orígenes permitidos:
  ```javascript
  const allowedOrigins = [
    'https://bananapeels.duckdns.org',
    'http://localhost:5173',
  ];
  ```
- **HTTPS** mediante certificados Let's Encrypt (Certbot)
- Redirección automática HTTP → HTTPS

### Servidor Nginx
- Configurado como **proxy inverso**
- Rutas `/api/` y `/socket.io/` redirigidas al backend
- Soporte para WebSockets
- Frontend servido como archivos estáticos

### Proceso de Actualización
Script automatizado `update.sh`:
1. Pull desde repositorio Git
2. Instalación de dependencias
3. Reinicio del backend (Supervisor)
4. Reconstrucción del frontend
5. Recarga de configuración Nginx

## 💻 Tecnologías Utilizadas

### Frontend
- React
- Redux para gestión del estado
- React Router
- Socket.io (cliente)
- Vite como bundler

### Backend
- Node.js
- Express
- MongoDB
- Mongoose
- Socket.io (servidor)
- JWT para autenticación

### DevOps
- Git/GitHub
- AWS EC2
- Nginx
- Certbot (Let's Encrypt)
- Supervisor

## 🔮 Próximas Mejoras

- Sistema de notificaciones en tiempo real
- Búsqueda avanzada con filtros
- Optimización de rendimiento
- Integración de pasarela de pagos
- Aplicación móvil nativa

## 📊 Métricas y Resultados

- Tiempo de respuesta API < 300ms
- Tiempo de carga inicial < 2s
- Cobertura de pruebas > 80%
- Disponibilidad del servicio > 99.5%