# Despliegue de una aplicación escalable. 

Nicolás García Sobrino nicolas.garciasobrino@alumnos.upm.es,
Santiago Rayán Castro 
Javier de Ponte Hernando. 

---

# Bloque 1. Despliegue de la Aplicación Monolítica en VM de Google Cloud

Este documento detalla el proceso de desarrollo del script en Python para desplegar una aplicación monolítica en una máquina virtual pesada, conforme a los requisitos de la práctica creativa 2 utilizando Google Cloud como infraestructura de VM. 

El objetivo es desplegar una aplicación monolítica escrita en Python sobre una máquina virtual en Google Cloud Platform (GCP). La aplicación consiste en una página web que muestra información sobre libros y se ejecuta mediante el archivo `productpage_monolith.py`.

La instalación incluye:
- Clonación del repositorio con el código de la aplicación.
- Instalación de dependencias.
- Configuración del entorno.
- Despliegue y ejecución de la aplicación en la VM.

---

## **Estructura del Script**

El script desarrollado (`deploy_app.py`) realiza las siguientes funciones:

1. **`install_dependencies()`**: Instala paquetes esenciales como Python, Pip y Git.
2. **`clone_repository()`**: Clona el repositorio de la aplicación desde GitHub.
3. **`modify_application_title()`**: Modifica el título de la aplicación con el nombre del grupo.
4. **`configure_application(group_num, port)`**: Configura la aplicación con los valores especificados y la inicia.
5. **`delete_application()`**: Detiene la aplicación y elimina los archivos generados.
6. **`deploy_application(group_num, port)`**: Orquesta todo el proceso de instalación y despliegue.
7. **`main()`**: Controla la ejecución del script, permitiendo las opciones de despliegue y eliminación.

---

## **Uso del Script**

Para iniciar la aplicación en la máquina virtual, ejecutar:

```bash
python3 deploy_app.py
```

Esto realizará:
1. Instalación de dependencias.
2. Clonación del repositorio.
3. Configuración del entorno.
4. Despliegue de la aplicación.

La aplicación será accesible en la URL:

```
http://34.175.77.229:9080/productpage
```

Para detener la aplicación y eliminar los archivos generados, ejecutar:

```bash
python3 deploy_monolith_vm.py delete
```

Este comando realiza:
- Finalización del proceso de la aplicación.
- Eliminación del repositorio clonado.

---

## **Configuración de Variables de Entorno**
El script permite personalizar la configuración a través de variables de entorno:

```bash
export GROUP_NUM=14
export APP_PORT=9080
```

Las variables disponibles son:
- `GROUP_NUM`: Número de grupo para personalizar el título de la aplicación.
- `APP_PORT`: Puerto en el que se ejecuta la aplicación.

---

## **Configuración de Firewall en GCP**
Para garantizar el acceso a la aplicación desde el exterior, es necesario crear una regla de firewall para permitir el puerto 9080 ajustada manualmente en Google Cloud
<img width="1285" alt="Captura de pantalla 2025-01-21 a las 18 35 25" src="https://github.com/user-attachments/assets/1825a7a3-7852-4c30-a222-cd7d48755f99" />

---

## **Consideraciones Finales**

La app se puede ejecutar en la propia maquina virtual de Google Cloud ejecutando el comando normal o ejecutando acompañado de un & para tener acceso posteriormente al terminal y poder ejecutar el comando delete de manera más limpia. (sin el & la consola se queda dentro de la ejecución y hay que hacer controlC para salir). 

---

# Bloque 2. Despliegue de Aplicación Monolítica usando Docker

Este documento proporciona una guía paso a paso para desplegar una aplicación monolítica utilizando Docker de forma automatizada mediante un script en Python.
El scrypt realiza todo, comprueba que docker este descargado, si no, lo instala, clona el repositorio, ejecuta los comando correspondientes y despliega la app en local. 
Tambien se puede desplegar en la VM de google cloud como hemos hecho en el bloque1 pero en este caso queríamos cambiar para ver otro enfoque. 
En el caso de desplegar en VM habría que subir los archivos y añadir una nueva regla del FW para que permita peticiones al puerto 5080. 

---

## **Descripción del Script**

El script automatiza las siguientes tareas:

1. **Verificación de Docker:** Comprueba si Docker está instalado.
2. **Instalación de Docker (opcional):** Instala Docker si no está disponible (solo en Linux). 
3. **Clonación del Repositorio:** Descarga la aplicación desde GitHub.
4. **Creación del Dockerfile:** Genera el Dockerfile para construir la imagen.
5. **Construcción de la Imagen Docker:** Construye la imagen de la aplicación.
6. **Ejecución del Contenedor:** Levanta la aplicación con la configuración correcta.
7. **Eliminación del Contenedor:** Permite borrar la aplicación desplegada.

---

## **Uso del Script**

1. Clona este repositorio o guarda el script Python en tu entorno.

2. Ejecuta el script:

    ```bash
    python3 deploy_docker.py
    ```

3. Selecciona la acción deseada:

    - Para desplegar la aplicación, escribe: `deploy`
    - Para eliminar la aplicación, escribe: `delete`

4. Una vez desplegado, accede a la aplicación a través de:

    ```
    http://localhost:5080
    ```

---

## **Descripción del Dockerfile Generado**

El Dockerfile generado por el script contiene los siguientes pasos:

```dockerfile
FROM python:3.7.7-slim
WORKDIR /app
COPY bookinfo/src/productpage /app
RUN pip install --upgrade urllib3 chardet requests
RUN pip install --no-cache-dir -r /app/requirements.txt
ENV GROUP_NUM=14
EXPOSE 5080
CMD ["sh", "-c", "sed -i 's|BookInfo Sample|Grupo ${GROUP_NUM}|g' /app/templates/productpage.html && python3 /app/productpage_monolith.py 5080"]
```

### **Explicación de las instrucciones:**

- `FROM python:3.7.7-slim`: Usa una imagen base ligera de Python.
- `WORKDIR /app`: Establece el directorio de trabajo dentro del contenedor.
- `COPY bookinfo/src/productpage /app`: Copia el código fuente de la aplicación.
- `RUN pip install`: Instala las dependencias necesarias.
- `ENV GROUP_NUM=14`: Establece una variable de entorno con el número del grupo.
- `EXPOSE 5080`: Expone el puerto en el contenedor.
- `CMD [...]`: Ejecuta la aplicación reemplazando el texto "BookInfo Sample" por "Grupo 14".

---

## **Funciones Principales del Script**

### `check_docker()`
Verifica si Docker está instalado en el sistema.

### `install_docker()`
Instala Docker en sistemas Linux si no está presente.

### `clone_repository()`
Clona el repositorio de la aplicación si no existe, o lo actualiza si ya está clonado.

### `create_dockerfile()`
Genera un Dockerfile para construir la imagen Docker.

### `build_image()`
Construye la imagen Docker a partir del Dockerfile generado.

### `run_container()`
Ejecuta un contenedor basado en la imagen creada.

### `delete_container()`
Elimina el contenedor desplegado y borra el repositorio.

### `main()`
Controla la ejecución del script según la acción elegida por el usuario.

---

## **Eliminación del Contenedor**

Para detener y eliminar la aplicación de Docker, puedes ejecutar el script y elegir la opción `delete` o hacerlo manualmente con los siguientes comandos:

```bash
# Detener el contenedor
    docker stop product-page-14

# Eliminar el contenedor
    docker rm product-page-14

# Eliminar la imagen
    docker rmi product-page/14
```

---

## **Conclusión**

Este script proporciona una forma sencilla de automatizar el despliegue de una aplicación monolítica en Docker. Se encarga de todas las tareas necesarias, desde la instalación de dependencias hasta la ejecución de la aplicación en un contenedor accesible desde el navegador.



Tambien se puede ejecutar desde un terminal remoto utilizando ssh usuario@34.175.77.229 (ip externa) accediendo con la clave pública.

---



