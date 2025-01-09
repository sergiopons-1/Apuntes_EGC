### Resumen: Comandos y Conceptos Clave de Docker

---

#### **1. Instalación de Docker en Ubuntu 22.04**

- Actualizar lista de paquetes:
  ```bash
  sudo apt update
  ```

- Instalar paquetes necesarios:
  ```bash
  sudo apt install apt-transport-https ca-certificates curl software-properties-common
  ```

- Añadir la clave GPG del repositorio oficial de Docker:
  ```bash
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  ```

- Añadir el repositorio de Docker:
  ```bash
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```

- Instalar Docker:
  ```bash
  sudo apt update
  sudo apt install docker-ce
  ```

- Verificar que Docker está activo:
  ```bash
  sudo systemctl status docker
  ```

---

#### **2. Uso de Docker sin `sudo`**

- Añadir tu usuario al grupo `docker`:
  ```bash
  sudo usermod -aG docker ${USER}
  ```
  Cierra la sesión y vuelve a iniciarla para aplicar los cambios.

- Alternativamente, aplica los cambios sin cerrar sesión:
  ```bash
  su - ${USER}
  ```

---

#### **3. Docker Básico: Primeros Comandos**

1. **Ejecutar tu primer contenedor "Hello World":**
   ```bash
   docker run hello-world
   ```

2. **Listar imágenes locales:**
   ```bash
   docker images
   ```

3. **Descargar una imagen de Ubuntu:**
   ```bash
   docker pull ubuntu
   ```

4. **Ejecutar un contenedor interactivo con Ubuntu:**
   ```bash
   docker run -it ubuntu bash
   ```

5. **Listar contenedores activos:**
   ```bash
   docker ps
   ```

6. **Listar todos los contenedores (incluidos los detenidos):**
   ```bash
   docker ps -a
   ```

7. **Eliminar un contenedor:**
   ```bash
   docker rm <container_id>
   ```

8. **Eliminar todos los contenedores detenidos:**
   ```bash
   docker rm $(docker ps -aq)
   ```

9. **Eliminar una imagen:**
   ```bash
   docker rmi <image_id>
   ```

---

#### **4. Trabajando de Manera Interactiva con Contenedores**

- **Ejecutar un contenedor en segundo plano (modo "detached")**:
  ```bash
  docker run -td ubuntu bash
  ```

- **Acceder a un contenedor en segundo plano:**
  ```bash
  docker exec -ti <container_id> bash
  ```

---

#### **5. Puertos y Volúmenes**

- **Crear un contenedor NGINX y mapear puertos:**
  ```bash
  docker run -it --rm -d -p 8080:80 --name web nginx
  ```

- **Montar un volumen local en un contenedor:**
  ```bash
  docker run -it --rm -d -p 8080:80 --name web -v ~/site-content:/usr/share/nginx/html nginx
  ```

- **Crear un volumen persistente:**
  ```bash
  docker volume create nginx-data
  ```

- **Usar un volumen persistente con un contenedor:**
  ```bash
  docker run -it --rm -d -p 8080:80 --name web -v nginx-data:/usr/share/nginx/html nginx
  ```

---

#### **6. Limpiar Docker**

- **Eliminar imágenes y capas no utilizadas:**
  ```bash
  docker image prune
  ```

- **Eliminar contenedores detenidos:**
  ```bash
  docker container prune
  ```

---

#### **7. Construir y Ejecutar Imágenes**

- **Construir una imagen desde un Dockerfile:**
  ```bash
  docker build -t my-image .
  ```

- **Ejecutar un contenedor desde una imagen construida:**
  ```bash
  docker run -p 5000:5000 my-image
  ```

---

Este resumen cubre los comandos esenciales para trabajar con Docker en tareas básicas y avanzadas. Si necesitas más ejemplos o detalles, házmelo saber.
