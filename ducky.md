En el siguiente enlace se encuentra una página de creación de Scripts para RubberDucky:

https://ducktoolkit.com/payload/windows

![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky1.jpg)
 
Las tareas a realizar en el Script se organizan en 3 fases:

•	Reconocimiento

•	Explotación

•	Reporte

Para poder crear el Script hay que seleccionar la opción deseada en cada apartado.

![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky2.png)
![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky3.png)
![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky4.png)
![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky5.png)

Se va a crear un Script de ejemplo en el que se reportará vía email el perfil WiFi de la víctima, una captura de pantalla del equipo, también se desactivará el Firewall y por último se creará una Shell inversa.

Y esto habilitará en la parte derecha de la página una sección por cada opción modificable.

De arriba hacia abajo, hemos modificado lo siguiente:

•	El retraso de escritura entre comandos será de 500 ms, y las variables globales serán de idioma Español.

•	Se harán 4 capturas de pantalla.

•	La Shell inversa irá sobre la IP seleccionada en el puerto 4443 elegido.

•	El reporte vía email será desde el correo Email@enviador.com con contraseña “PasswordDeEnvio” hacia Email@receptor.com  usando el servidor de correo smtp.correo.com

![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky6.png)

Tras seleccionar esto hay que ir a “Create Payload”.

![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky7.png)

Después de seleccionarlo saldrá una alerta explicando que es una herramienta de Pentesting para redes autorizadas

![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky8.png)

Aparecerá un menú donde se encontraba la opción de Crear el Payload

![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky9.png)

Vamos a seleccionar la opción “Edit in Encoder” Para ver el código, lo que mostraría es la siguiente página con un editor para ver el Script:

![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky10.png)

Lo que se debe hacer para llevar este Script de RubberDucky a nuestro Digispark es usar el siguiente enlace:
                https://seytonic.com/ducky/ 
                
![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky11.jpg)

Hay que copiar el código mostrado en el editor de scripts en la parte izquierda de esta página, posteriormente hacer click en Compilar y el resultado sería el siguiente:

![](https://github.com/Yradiel/ProyectoPi/blob/master/DuckyFotos/ducky12.jpg)

Ahora solo queda copiar y compilar dicho código (de la parte derecha) en nuestro Digispark. (Se recomienda configurar los “Delay” para reducir al máximo la espera entre ejecuciones teniendo en cuenta la velocidad de escritura del equipo víctima).
