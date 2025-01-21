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

El script desarrollado (`deploy_mapp.py`) realiza las siguientes funciones:

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

Tambien se puede ejecutar desde un terminal remoto utilizando ssh@34.175.77.229 (ip externa)

---



