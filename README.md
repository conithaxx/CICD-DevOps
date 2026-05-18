🐶 DEVOPS PROYECTO: TIENDA PERRITOS
📊 INFORMACIÓN DEL PROYECTO

Asignatura: Introducción a Herramientas DevOps (ISY1101).

Caso de Negocio: Innovatech Chile.

🛠️ FASE 1: CONTENEDORIZACIÓN LOCAL
💻 Componente Frontend

Tecnología: Archivos estáticos planos servidos a través de Nginx.


Optimización: Implementación de multi-stage build para asegurar la limpieza absoluta de capas.


Seguridad: Configuración obligatoria de un usuario no-root (nginx) para mitigar riesgos.


Puerto Interno: Configurado en el puerto seguro 8080 debido a restricciones de privilegios de Linux.

⚙️ Componente Backend

Tecnología: Microservicios basados en la plataforma Node.js.


Optimización: Estructura en dos etapas mediante multi-stage build local.


Seguridad: Operación bajo el principio de mínimo privilegio utilizando el usuario no-root node.


Puerto Interno: Exposición formal en el puerto 3001.






**************************************************************
.

🎛️ FASE 1 Y 2: ORQUESTACIÓN Y PERSISTENCIA
El archivo docker-compose.yml unifica por completo el stack de servicios del proyecto. Permite que los componentes se ejecuten tanto de manera conjunta como de forma independiente.

🌐 Configuración de Redes y Arranque

Red Interna: Aislamiento de contenedores mediante una red con driver bridge (tienda-network).


Dependencias: Uso de depends_on para asegurar que el frontend espere al backend, y este a la base de datos.


Comunicación: Enlace directo entre componentes utilizando los nombres internos de los servicios.

💾 Persistencia de Datos

Mecanismo: Aplicación de persistencia mediante volúmenes Docker en la base de datos MySQL.


Volumen Utilizado: Uso de un named volume nativo llamado dbdata.


Continuidad: Configuración diseñada para asegurar que la información crítica no se pierda al reiniciar el entorno.

**************************************************************************
🚀 FASE 3: AUTOMATIZACIÓN DEL PIPELINE (CI/CD)
Flujo automatizado implementado en GitHub Actions para compilar y desplegar el código sin intervenciones manuales.

🎯 Punto de Activación (Trigger)
El pipeline está configurado de manera estricta para reaccionar de forma exclusiva ante un push en la rama deploy.

Filtra los caminos mediante paths para activarse únicamente si existen cambios reales dentro de la ruta frontend/.

🏃‍♂️ Etapas Obligatorias del Flujo

Construcción (Build): Compilación y construcción automatizada de la imagen Docker optimizada.


Publicación (Push): Autenticación y subida de la imagen hacia el registro privado de Amazon ECR.


Despliegue (Deploy): Conexión automatizada mediante AWS Systems Manager (SSM) para actualizar la versión del contenedor en tiempo real

******************************************
## 🔒 Gestión de Seguridad (GitHub Secrets)

Toda la gestión de credenciales temporales y variables sensibles se realiza de manera estricta mediante **GitHub Secrets**. Para este laboratorio, se utilizan los secretos actualizados recientemente con terminación en **1**:

* **`AWS_ACCESS_KEY_ID1`**: Clave de acceso temporal de AWS Academy.
* **`AWS_SECRET_ACCESS_KEY1`**: Clave secreta proporcionada por el laboratorio.
* **`AWS_SESSION_TOKEN1`**: Token gigante de sesión activa (requiere renovación).
* **`AWS_REGION1`**: Región oficial del despliegue (`us-east-1`).
* **`ECR_REGISTRY1`**: ID de cuenta de AWS de 12 dígitos para Amazon ECR.
* **`EC2_FRONTEND_INSTANCE_ID1`**: ID único de la instancia EC2 pública del Frontend.
* **`EC2_BACKEND_INSTANCE_ID1`**: ID único de la instancia EC2 privada del Backend.
* **`EC2_DB_INSTANCE_ID1`**: ID único de la instancia EC2 de la Base de Datos.

******************************************************************************************
## ☁️ FASE 4: INFRAESTRUCTURA Y REDES EN AWS

El despliegue final se realiza sobre una arquitectura distribuida por capas en Amazon Web Services:

* **🏢 Instancia de Frontend:**
* **Ubicación:** Desplegado sobre una instancia EC2 pública configurada previamente.
* **Acceso:** Completamente accesible desde internet mediante su IP pública a través del puerto HTTP `80`.


* **🛡️ Instancia de Backend y Base de Datos:**
* **Ubicación del Backend:** Servidor EC2 ubicado estratégicamente dentro de una subred privada.
* **Seguridad de Red:** Acceso externo completamente restringido y conectividad estable hacia la base de datos.
* **Políticas:** Toda la comunicación entre componentes respeta rigurosamente las reglas de los Security Groups.



---

## 📋 GUÍA DE EJECUCIÓN LOCAL RÁPIDA

Si deseas probar y validar la aplicación de forma local en tu computadora, abre la terminal en la raíz y ejecuta:

```bash
# 1. Levantar todo el entorno en segundo plano
docker compose up -d --build

# 2. Validar que los tres servicios estén corriendo (Up)
docker compose ps

```

> 🌐 **Validación:** Abre tu navegador web en `http://localhost:8080` para interactuar con la interfaz del sistema.



  


