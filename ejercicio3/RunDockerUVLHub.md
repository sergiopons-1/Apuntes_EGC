### Resumen: Configuración de Docker y Cambios Requeridos

---

1. [Configurar archivos de entorno](#1-configurar-archivos-de-entorno)
2. [Ejecutar los contenedores](#2-ejecutar-los-contenedores)
3. [Verificar contenedores en ejecución](#3-verificar-contenedores-en-ejecución)
4. [Detener los contenedores](#4-detener-los-contenedores)
5. [Detener los contenedores (eliminando también los volúmenes)](#5-detener-los-contenedores-eliminando-también-los-volúmenes)
6. [Recargar configuración](#6-recargar-configuración)
7. [Posibles errores](#7-posibles-errores)

#### **1. Configurar archivos de entorno**
Primero, copia el archivo `.env.docker.example` al archivo `.env` que se usará para establecer las variables de entorno.

```bash
cp .env.docker.example .env
```

---

#### **2. Ejecutar los contenedores**
Para iniciar los contenedores en modo desarrollo, usa el archivo `docker-compose.dev.yml` ubicado en el directorio `docker`. Este comando se ejecutará en segundo plano (`-d`).

```bash
docker compose -f docker/docker-compose.dev.yml up -d
```

---

#### **3. Verificar contenedores en ejecución**
Para verificar que los contenedores están corriendo correctamente, utiliza el siguiente comando:

```bash
docker ps
```

Si todo funciona correctamente, deberías ver la versión desplegada de **uvlhub** en desarrollo en: [http://localhost](http://localhost).

---

#### **4. Detener los contenedores**
Para detener los contenedores, usa el archivo `docker-compose.dev.yml` con el siguiente comando:

```bash
docker compose -f docker/docker-compose.dev.yml down
```

---

#### **5. Detener los contenedores (eliminando también los volúmenes)**
El comando anterior elimina los contenedores, pero no los volúmenes. Esto puede ser problemático en el caso de **MariaDB**, que sigue guardando la configuración anterior y puede generar problemas si queremos cargar una configuración diferente.

Para detener los contenedores y eliminar los volúmenes, utiliza la bandera `-v`:

```bash
docker compose -f docker/docker-compose.dev.yml down -v
```

---

#### **6. Recargar configuración**
Si se ha modificado algún archivo `Dockerfile` o `docker-compose.*.yml`, es necesario reconstruir las imágenes con la bandera `--build`. Para esto, ejecuta:

```bash
docker compose -f docker/docker-compose.dev.yml up -d --build
```

---

### **Cambios Requeridos en Docker**

---

#### **1. Desactivar el modo DEBUG**
Para garantizar que el modo `DEBUG` esté desactivado en despliegues de Docker:

1. **Modificar el archivo `docker-compose.yml` o el correspondiente (`docker-compose.dev.yml`):**

   Añade la variable de entorno `FLASK_DEBUG=False` (si usas Flask) o `DEBUG=False` (si usas Django) al servicio del contenedor:

   ```yaml
   services:
     web:
       build: .
       environment:
         - FLASK_DEBUG=False  # Si es Flask
         - DEBUG=False        # Si es Django
   ```

2. **Reconstruir los contenedores:**

   ```bash
   docker compose -f docker/docker-compose.dev.yml up -d --build
   ```

3. **Verificar:**
   Comprueba que el contenedor está corriendo y que `DEBUG` está desactivado:

   ```bash
   docker ps
   ```

4. **Hacer commit y push de los cambios:**

   ```bash
   git add docker-compose.dev.yml
   git commit -m "Desactivar DEBUG para despliegues en Docker"
   git push
   ```

---

#### **2. Configurar 4 workers para Gunicorn**
Para ajustar el número de workers de **Gunicorn** en el contenedor web:

1. **Editar el archivo `docker-compose.yml`:**

   Añade la configuración de Gunicorn al comando de inicio del servicio. Por ejemplo:

   ```yaml
   services:
     web:
       build: .
       environment:
         - FLASK_DEBUG=False
       command: gunicorn -w 4 -b 0.0.0.0:5000 app:app  # Ajustar "app:app" según tu aplicación
   ```

2. **Reconstruir los contenedores:**

   ```bash
   docker compose -f docker/docker-compose.dev.yml up -d --build
   ```

3. **Verificar que Gunicorn está usando 4 workers:**

   ```bash
   docker logs <nombre_del_contenedor>
   ```

   Busca un mensaje que indique cuántos workers se están ejecutando.

4. **Hacer commit y push de los cambios:**

   ```bash
   git add docker-compose.dev.yml
   git commit -m "Configurar 4 workers de Gunicorn en Docker"
   git push
   ```

---

#### **3. Cambiar el puerto al 8003**
Para cambiar el puerto en el cual se expone el servicio en Docker:

1. **Editar el archivo `docker-compose.yml`:**

   Cambia el mapeo de puertos en el servicio web:

   ```yaml
   services:
     web:
       build: .
       ports:
         - "8003:5000"  # 8003 será el puerto externo; 5000 es el puerto interno del contenedor
   ```

2. **Reconstruir y reiniciar los contenedores:**

   ```bash
   docker compose -f docker/docker-compose.dev.yml up -d --build
   ```

3. **Verificar:**
   Abre el navegador en `http://localhost:8003` para comprobar que el servicio está disponible en el puerto correcto.

4. **Hacer commit y push de los cambios:**

   ```bash
   git add docker-compose.dev.yml
   git commit -m "Configurar puerto 8003 para servicio web en Docker"
   git push
   ```

---

### **Resumen Final**
- **Desactivar DEBUG:** Añadir `FLASK_DEBUG=False` o `DEBUG=False` en las variables de entorno.
- **Configurar Gunicorn:** Usar el comando `gunicorn -w 4 -b 0.0.0.0:5000`.
- **Cambiar el puerto:** Actualizar `ports` a `8003:5000`.

Recuerda realizar un `commit` y un `push` después de cada cambio para mantener un historial claro.

#### **7. Posibles errores**
1. Puerto ya en uso: Error response from daemon: driver failed programming external connectivity on endpoint mariadb_container (d502ae05fd7776f78b7af1f4bf2e4ec4d0dfef53e53a37a97272bda49fe16d0f): failed to bind port 0.0.0.0:3306/tcp: Error starting userland proxy: listen tcp4 0.0.0.0:3306: bind: address already in use. Solución:
   ```bash
   sudo lsof -i :3306
   sudo systemctl stop mysql (nombre del proceso en el puerto)
   ```
      o tambien se puede sudo kill PID
