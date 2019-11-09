# Hyperfido
Configuración de Hyperfido U2F Securiry Key para usar en Linux Mint.

Configuración de Firefox para usar Hyperfido Security Key con cuentas de Google, Dropbox, Github, Facebook, Twitter, etc.
- Teclear en el omnibox de Firefox: about:config
- Nos pedirá que confirmemos que deseamos acceder a la configuración avanzada de Firefox
- En la caja de búsqueda escribimos: u2f
- Deberá aparecer una línea con la clave: security.webauth.u2f
- Hacemos doble click sobre ella y ponemos el valor a TRUE

Declaración de tipos de llaves ante el sistema operativo
- Habrá que crear un archivo denominado 70-u2f.rules dentro del directorio /etc/udev/rules.d
- Podemos descargar este archivo desde https://github.com/Yubico/libu2f-host/blob/master/70-u2f.rules Este archivo contiene las definiciones de los tipos de llaves u2f más habituales del mercado.

Configuración de Linux Mint para reforzar la seguridad del sistema, haciendo que sea necesario pinchar la llave u2f, tanto al hacer login como al ejecutar cualquier orden con sudo, además de teclear la contraseña. 

**IMPORTANTE: Si el sistema de archivos está encriptado, no debemos seguir hasta asegurarnos de que los archivos que vamos a crear, se encontrarán en una partición no encriptada o de lo contrario no podremos acceder al sistema de ninguna manera.**

- En primer lugar habrá que comprobar que el sistema operativo reconoce la llave u2f. Para ello ejecutamos: 
  - lsub | grep 2ccf:0880
- Debe aparece una línea similar a esta: 
  - Bus 001 Device 007: ID 2ccf:0880 Lo que significará que el sistema reconoce la llave u2f
- El siguiente paso será instalar los paquetes pamu2fcfg y libpam-u2f usando: 
  - sudo apt install pamu2fcfg libpam-u2f
- Los paquetes disponibles en los repositorios de Ubuntu, suelen estar menos actualizados que los disponibles en el PPA de Yubico, por lo que si se desea disponer de la última versión, se pueden instalar de la siguiente forma:

  - sudo add-apt-repository ppa:yubico/stable
  - sudo apt update
  - sudo apt install pamu2fcfg libpam-u2f
  
- A continuación crearemos la contraseña para la llave principal. Para ello ejecutamos:
  - pamu2fcfg | sudo tee /etc/u2f_keys (pulsaremos el botón de la llave cuando parpadee el led).
- Una buena idea es disponer de una regunda llave u2f como reserva, por si acaso perdiéramos la primera. Para añadir sucesivas contraseñas, retiraremos la llave principal y pincharemos la segunda llave. A contimuación ejecutamos:
  - pamu2fcfg -n | sudo tee -a /etc/u2f_keys (pulsaremos el botón de la llave cuando parpadee el led).
- Ahora vamos a forzar que sea necesario introducir la llave y pulsar el botón cuando realicemos login o ejecutemos cualquier comando con sudo. Es importante quitar la llave del puerto USB y la tenemos pinchada.
- Editamos el archivo /etc/pam.d/common-auth ejecutando: 
  - sudo pico /etc/pam.d/common-auth
- Al final del archivo introducimos las siguiente línea:
  - auth required pam_u2f.so nouserok authfile=/etc/u2f_keys cue (Toda la información sobre la sintaxis de libpam-u2f, se encuentra disponible en https://github.com/Yubico/pam-u2f)
  
Ahora reiniciamos el ordenador y tras introducir el usuario y la contraseña, pueden pasar varias cosas:
  1. Accedemos normalmente al sistema. Esto es síntoma de que algo de todo lo anterior ha fallado. Posiblemente no hemos guardado el archivo /etc/pam.d/common-auth o hemos accedido con un usuario distinto del que hemos creado la contraseña.
  2. El sistema nos dice que la contraseña no es válida. Lo más probable es que la llave 2uf no esté pinchada.
  3. El sistema nos dice: "Please touch the device". Es la situación normal. Debemos pulsar el botón de la llave u2f y ya habremos accedido al sistema.

Si nos encontramos con que no es posible acceder al sistema y éste entra en un bucle pidiendo la contraseña y sin reconocer la que introducimos, el problema es que tenemos encriptado el sistema de archivos. En este caso, acabamos de perder todo el contenido del disco. Si estamos seguros de que el sistema de archivos no está encriptado y no podemos acceder al sistema, bastará con arrancar el ordenador con un LiveCD y comentar o eliminar la línea que introducimos en el archivo /etc/pam.d/common-auth
