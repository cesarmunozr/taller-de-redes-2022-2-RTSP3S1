# Taller de Redes 2022-2-RTSP3S1
Los siguientes archivos de tipo Dockerfile corresponden a las imágenes necesarias para levantar los contenedores dentro del software Docker, con el fin de poder visualizar y analizar tráfico relacionado con el protocolo RTSP mediante el uso de FFmpeg, el cual corresponde a un software libre para reproducción y grabación de archivos multimedia. Este tráfico será generado entre un servidor y un cliente al generar una solicitud por este último, y será analizado mediante el uso del software Wireshark.
## Características

   Fácil de ejecutar
   
   Compatibilidad con amplia gama de SO.
   
  ## Instalación
  
### Docker

   1. Identificar el sistema operativo con el cuál se va a trabajar.
   2. Buscar dentro de la documentación de [Docker](https://docs.docker.com/engine/install/) para encontrar la instalación recomendada.
   3. Seguir los pasos indicados dentro de la documentación.
### Wireshark
   1. Buscar dentro de la documentación de [Wireshark](https://www.wireshark.org/download.html) para encontrar la instalación recomendada.
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
    
    
      ### Tarea 3

## Buildear nuevos dockers files (server-arp, cliente-arp y scapy)

1)
Iniciar docker Server_ARP
sudo docker run -it --rm server-arp

Iniciar docker Cliente_ARP
sudo docker run -it --rm cliente-arp

Iniciar docker Scapy
sudo docker run -it --rm scapy

3) 

Abrimos Wireshark

4) 

Hacemos la conexión de cliente y server


ffmpeg -rtsp_transport tcp -i rtsp://IP:8554/mystream -c copy output.mp4


5) Fuzzing

En el docker de scapy, creamos la variable ip_server con la IP asignada para el docker server

ip_server ="172.17.0.x"

I)

f1 = socket.socket()
f1.connect ((ip_server, 8554))
fuzz1 = StreamSocket(f1,Raw)
fuzz1.sr1(Raw("DESCRIBE rtsp://172.17.0.5:8554/mystream RTSP/1.0\r\nAccept: application/sdp\r\nCSeq: 2\r\nUser-Agent: 사회과학원어학연구소\r\n\r\n"))


II)

f2 = socket.socket()
f2.connect ((ip_server, 8554))
fuzz2 = StreamSocket(f2,Raw)
fuzz2.sr1(Raw("PLAY rtsp://172.17.0.2:8554/mystream/ RTSP/1.0\r\nRange: npt=0.000-\r\nCSeq: 98791982739187293719821973\r\nUser-Agent: Lavf58.45.100\r\nSession: 12345678\r\n\r\n"))


6) Modificaciones de campos especificas

I)

ip_server = "172.17.0.X"

s1 = socket.socket()
s1.connect ((ip_server, 8554))
ss1 = StreamSocket(s1, Raw)
ss1.sr1(Raw("PLAY rtsp://172.17.0.2:8554/mystream/ RTSP/1.0\r\nRange: npt=0.000-\r\nCSeq: 4\r\nUser-Agent: Lavf58.45.100\r\nSession: 11111111\r\n\r\n"))

II)
s2 = socket.socket()
s2.connect ((ip_server, 8554))
ss2 = StreamSocket(s2, Raw)
ss2.sr1(Raw("DESCRIBE rtsp://172.17.0.5:8554/mystream RTSP/1.0\r\nAccept: application/sdp\r\nCSeq: 2\r\nUser-Agent: 12345678\r\n\r\n"))

III)
s3 = socket.socket()
s3.connect ((ip_server, 8554))
ss3 = StreamSocket(s3, Raw)
ss3.sr1(Raw("TEARDOWN rtsp://172.17.0.5:8554/mystream/ RTSP/1.0\r\nCSeq: 6\r\nUser-Agent: 12345678\r\nSession: Lavf58.45.1001\r\n\r\n"))


## Agradecimientos

Esta documentación fue provista por ayudantes de la Universidad Diego Portales:
    • @mygeone2


## Contacto
En caso de consultas, comunicarse con:

-Francisco Gálvez / francisco.galvez_p@mail.udp.cl

-Cesar Muñoz / cesar.munoz_r@mail.udp.cl
