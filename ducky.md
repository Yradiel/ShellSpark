En el siguiente enlace se encuentra una página de creación de Scripts para RubberDucky:
https://ducktoolkit.com/payload/windows
 
Las tareas a realizar en el Script se organizan en 3 fases:

•	Reconocimiento

•	Explotación

•	Reporte

Para poder crear el Script hay que seleccionar la opción deseada en cada apartado.

Se va a crear un Script de ejemplo en el que se reportará vía email el perfil WiFi de la víctima, una captura de pantalla del equipo, también se desactivará el Firewall y por último se creará una Shell inversa.

Y esto habilitará en la parte derecha de la página una sección por cada opción modificable.

De arriba hacia abajo, hemos modificado lo siguiente:

•	El retraso de escritura entre comandos será de 500 ms, y las variables globales serán de idioma Español.

•	Se harán 4 capturas de pantalla.

•	La Shell inversa irá sobre la IP seleccionada en el puerto 4443 elegido.

•	El reporte vía email será desde el correo Email@enviador.com con contraseña “PasswordDeEnvio” hacia Email@receptor.com  usando el servidor de correo smtp.correo.com

Tras seleccionar esto hay que ir a “Create Payload”.
 
Después de seleccionarlo saldrá una alerta explicando que es una herramienta de Pentesting para redes autorizadas

Aparecerá un menú donde se encontraba la opción de Crear el Payload

Vamos a seleccionar la opción “Edit in Encoder” Para ver el código, lo que mostraría es la siguiente página con un editor para ver el Script:
 
Lo que se debe hacer para llevar este Script de RubberDucky a nuestro Digispark es usar el siguiente enlace:
                https://seytonic.com/ducky/ 
 
Hay que copiar el código mostrado en el editor de scripts en la parte izquierda de esta página, posteriormente hacer click en Compilar y el resultado sería el siguiente:
 

Ahora solo queda copiar y compilar dicho código (de la parte derecha) en nuestro Digispark. (Se recomienda configurar los “Delay” para reducir al máximo la espera entre ejecuciones teniendo en cuenta la velocidad de escritura del equipo víctima).
