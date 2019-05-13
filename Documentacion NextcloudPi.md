## Proyecto NextcloudPi Seguro

Vamos a habilitar SSH y elegir la distribución de teclado con el comando

 `raspi-config`

![Image 2](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY/Image%202.png)

![Image 3](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY/Image%203.png)![Image 1](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY/Image%201.png)Ejecutamos para descargar Docker y posteriormente usarlo para desplegar nextcloud.

`sudo curl -sSL https://get.docker.com/ | sh`

![Image 5](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY/Image%205.png)

Ahora vamos a instalar python-pip y python3-pip

`apt install python-pip python3-pip`

Tras instalarlo vamos a descargar docker-compose a través de pip.

`pip install docker-compose`

Tras esto lanzaremos el siguiente comando para montar NextCloudPi en Docker ARM.

`docker run -d -p 4443:4443 -p 443:443 -p 80:80 -v ncdata:/data --name nextcloudpi ownyourbits/nextcloudpi-armhf $DOMAIN`

![Image 6](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY/Image%206.png)

El usuario es ncp

La contraseña para https://nextcloudpi.local:4443 es: xxxxxxxxxxxxxx

La contraseña  para https://nextcloudpi.local es: xxxxxxxxxxxxxx

Ahora vamos a desactivar el funcionamiento exclusivo por https:

![Image 7](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY/Image%207.png)

Para descargar desde powershell: `Start-BitsTransfer http://192.168.1.11/index.php/s/m3HQMpYZBZewB8N/download -Destination ./DESCARGABERRY`

También vamos a definir los dominios de confianza de nuestro NextcloudPi:

![Image 9](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY/Image%209.png)

Se va a configurar la Raspberry como punto de acceso, para ello descargamos hostapd y dnsmasq.

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/1.PNG)

Después de instalarlo vamos a parar los servicios y reiniciar.

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/2.PNG)

Configuramos una ip estática para el punto de acceso.

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/3.PNG)

Tras esto comprobamos el estado del servicio dhcpcd.

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/4.PNG)

Se hará un backup de la configuración de dnsmasq y crearemos el nuevo fichero de configuración.

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/4-5.PNG)

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/5.PNG)

Una vez configurado el dnsmasq se procede a configurar el hostapd

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/6-1.PNG)

Ahora hay que indicar en el archivo de configuración por defecto de hostapd la ubicación de la configuración recientemente creada

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/7.PNG)

Ahora se debe comprobar el estado del servicio de hostapd

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/8.PNG)

Se habilitará el ip forwarding para que se pueda enrutar.

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/9.PNG)

También haremos una regla en iptables para el POSTROUTING y después reiniciaremos

![](https://github.com/Yradiel/ProyectoPi/blob/master/RASPBERRY2/10.PNG)

Si se quiere habilitar el punto de acceso tras iniciar el Sistema Operativo habría que ejecutar

``Systemctl enable hostapd``
