## Proyecto NextcloudPi Seguro

Vamos a habilitar SSH y elegir la distribución de teclado con el comando

 `raspi-config`

![Image 2](https://github.com/Yradiel/ProyectoPi/blob/master/Image%202.png)

![Image 3](https://github.com/Yradiel/ProyectoPi/blob/master/Image%203.png)![Image 1](C:\Users\Rober\Pictures\RASPBERRY\Image 1.png)Ejecutamos para descargar Docker y posteriormente usarlo para desplegar nextcloud.

`sudo curl -sSL https://get.docker.com/ | sh`

![Image 5](https://github.com/Yradiel/ProyectoPi/blob/master/Image%205.png)

Ahora vamos a instalar python-pip y python3-pip

`apt install python-pip python3-pip`

Tras instalarlo vamos a descargar docker-compose a través de pip.

`pip install docker-compose`

Tras esto lanzaremos el siguiente comando para montar NextCloudPi en Docker ARM.

`docker run -d -p 4443:4443 -p 443:443 -p 80:80 -v ncdata:/data --name nextcloudpi ownyourbits/nextcloudpi-armhf $DOMAIN`

![Image 6](https://github.com/Yradiel/ProyectoPi/blob/master/Image%206.png)

El usuario es ncp

La contraseña para https://nextcloudpi.local:4443 es: xxxxxxxxxxxxxx

La contraseña  para https://nextcloudpi.local es: xxxxxxxxxxxxxx

Ahora vamos a desactivar el funcionamiento exclusivo por https:

![Image 7](https://github.com/Yradiel/ProyectoPi/blob/master/Image%207.png)

Para descargar desde powershell: `Start-BitsTransfer http://192.168.1.11/index.php/s/m3HQMpYZBZewB8N/download -Destination ./DESCARGABERRY`

