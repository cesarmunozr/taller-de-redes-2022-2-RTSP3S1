# Taller de Redes 2022-2-RTSP3S1
Los siguientes archivos de tipo Dockerfile corresponden a las imágenes necesarias para levantar los contenedores dentro del software Docker, con el fin de poder visualizar y analizar tráfico relacionado con el protocolo RTSP mediante el uso de FFmpeg, el cual corresponde a un software libre para reproducción y grabación de archivos multimedia. Este tráfico será generado entre un servidor y un cliente al generar una solicitud por este último, y será analizado mediante el uso del software Wireshark.
## Características

   Fácil de ejecutar
   
   Compatibilidad con amplia gama de SO.
   
  ## Instalación
  
### Docker

   1. Identificar el sistema operativo con el cuál se va a trabajar.
   2. Buscar dentro de la documentación de Docker para encontrar la instalación recomendada.
   3. Seguir los pasos indicados dentro de la documentación.
### Wireshark
   1. Buscar dentro de la documentación de Wireshark para encontrar la instalación recomendada.
   2. Seguir los pasos indicados dentro de la documentación.
### Dockerfiles
   1. Copiar el Dockerfile contenido dentro de la carpeta de Servidor en el escritorio
   2. Copiar el Dockerfile contenido dentro de la carpeta de Servidor en el escritorio
(Advertencia: copiar cada Dockerfile dentro de carpetas que puedan ser diferenciadas para no tener problemas a la hora de realizar la instalación)

## Ejecución
Procederemos con la instalación del contenedor del servidor.
1) Dentro de la carpeta que contenga el Dockerfile de “Servidor” se debe abrir una terminal y se debe ejecutar el siguiente comando (esto puede tomar          varios minutos):
   
     ``$sudo docker build .``
      
   Este comando creara una imagen para el Dockerfile de servidor.
    
2) Para mayor comodidad, obtenemos la ID de la imagen creada para asignarle un TAG el cual sea fácil de reconocer, para esto solo necesitamos los primeros    2 caracteres del ID. Para obtener esto, se debe ejecutar el siguiente comando:

    ``$sudo docker images``

3) Una vez encontrada la imagen deseada, escribimos en la consola:

    ``$sudo docker tag Char1Char2 server``

4) Para el caso de Cliente se debe realizar el mismo procedimiento, pero con el archivo correspondiente a cliente, dentro de la carpeta en que se encuentre contenido. Construimos la imagen:

    ``$sudo docker build .``

5) Obtenemos su ID para modificar su TAG:

    ``$sudo docker images``

6) Y le asignamos el TAG de preferencia:

    ``$sudo docker tag Char1Char2 client``

7) Ahora procedemos con la ejecución de cada imagen para que luego crear una interacción entre cliente y servidor.
Para el caso del servidor:

    ``$sudo docker run -it  --rm server``
  
Para el caso de cliente:

``$sudo docker run -it  --rm client``

8) Para la crear la conexión entre cliente y servidor, se debe obtener la dirección IP del Servidor, esto es posible mediante la ejecución del siguiente comando en una terminal dentro de la carpeta que contenga el archivo de servidor:

    ``$sudo docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id``

9) Para identificar el trafico debemos abrir la aplicación de Wireshark y realizar una captura en la red “docker 0”, esta contiene el tráfico correspondiente a las imágenes de Docker.

10) Una vez obtenida la dirección IP, la reemplazamos en el siguiente comando, para así entablar una conexión entre el cliente y el servidor. Este comando debe ser ejecutado en la consola en la cual se realizó el paso 7:

    ``:/# ffmpeg -rtsp_transport tcp -i rtsp://[IP_DOCKER_SERVER]:8554/mystream -c copy output.mp4``

## Agradecimientos

Esta documentación fue provista por ayudantes de la Universidad Diego Portales:
    • @mygeone2


## Contacto
En caso de consultas, comunicarse con:

-Francisco Gálvez / francisco.galvez_p@mail.udp.cl

-Cesar Muñoz / cesar.munoz_r@mail.udp.cl
