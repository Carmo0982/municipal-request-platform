# municipal-request-platform

### Arquitectura del Sistema — Plataforma Municipal de Gestión de Solicitudes Ciudadanas

![Diagrama de arquitectura](img/architecture_diagram.jpg)


La plataforma está diseñada bajo una arquitectura de microservicios en capas, lo que garantiza modularidad, escalabilidad e independencia entre componentes. A continuación se describe cada capa del sistema.
1. **Capa de Actores:**
El sistema contempla dos tipos de usuarios: el ciudadano, quien interactúa con la plataforma para registrar solicitudes, adjuntar evidencias y hacer seguimiento a sus casos; y el administrador/funcionario municipal, quien gestiona las solicitudes, las asigna a dependencias y monitorea el desempeño operativo.

2. **Capa Frontend:**
El frontend está compuesto por dos aplicaciones web, ambas con diseño adaptable a dispositivos móviles. El Portal Ciudadano permite a los ciudadanos registrar solicitudes, consultar su estado y comunicarse con las autoridades. El Panel Administrativo ofrece a los funcionarios herramientas para gestionar solicitudes, asignar tareas a departamentos y generar reportes de desempeño.

3. **Capa de Gateway:**
Toda comunicación entre el frontend y el backend pasa obligatoriamente por esta capa. El Load Balancer distribuye el tráfico entrante de forma equitativa entre los servidores disponibles, garantizando disponibilidad y rendimiento. El API Gateway (REST/GraphQL) actúa como punto de entrada único al backend, encargándose del enrutamiento de peticiones, control de acceso y gestión de versiones de la API.

4. **Capa de Microservicios Backend:**
El backend está organizado en cinco microservicios independientes, cada uno con una responsabilidad bien definida:

    - **Auth Service:** Gestiona la autenticación y autorización de usuarios mediante OAuth 2.0 y tokens JWT, integrándose con el proveedor de identidad gubernamental vía SSO.

    - **Request Management Service:** Es el núcleo del sistema. Administra el ciclo de vida completo de las solicitudes ciudadanas, desde su creación hasta su resolución.

    - **Notification Service:** Envía alertas y actualizaciones a los ciudadanos a través de correo electrónico (SendGrid) y mensajes de texto (Twilio).

    - **Reporting & Analytics Service:** Genera reportes de desempeño, tiempos de resolución y estadísticas operativas para los administradores.

    - **File Upload Service:** Gestiona la carga y almacenamiento de archivos adjuntos como fotografías o documentos relacionados con las solicitudes.
    

5. **Capa de Datos:**
La persistencia de información se gestiona mediante tres tecnologías complementarias. PostgreSQL es la base de datos relacional principal, donde se almacenan solicitudes, usuarios y registros del sistema. Redis actúa como caché de alta velocidad y gestor de sesiones activas, reduciendo la carga sobre la base de datos principal. S3 Object Storage almacena los archivos adjuntos subidos por los ciudadanos, como imágenes o documentos de soporte.
6. **Integraciones con Terceros:**
El sistema se conecta con servicios externos para ampliar sus capacidades: SendGrid para el envío de notificaciones por correo electrónico, Twilio para notificaciones por SMS, Google Maps API para la geolocalización de las solicitudes en el mapa municipal, y el Proveedor de Identidad Gubernamental (SSO) para la autenticación unificada de funcionarios públicos.
7. **Infraestructura Cloud (AWS / Azure):**
Toda la plataforma está desplegada en la nube, lo que garantiza alta disponibilidad y escalabilidad. Se utiliza Cloud Hosting (AWS o Azure) como proveedor de infraestructura, un Pipeline CI/CD con GitHub Actions para la integración y despliegue continuo de nuevas versiones, y herramientas de Monitoreo para el registro de logs y métricas de rendimiento en tiempo real.