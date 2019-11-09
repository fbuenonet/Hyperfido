# Hyperfido
Configuración de Hyperfido U2F Securiry Key para usar en Linux Mint

Configuración de Firefox para usar Hyperfido Security Key con cuentas de Google, Dropbox, Github, Facebook, Twitter, etc.
- Teclear en el omnibox de Firefox: about:config
- Nos pedirá que confirmemos que deseamos acceder a la configuración avanzada de Firefox
- En la caja de búsqueda escribimos: uf2
- Deberá aparecer una línea con la clave: security.webauth.u2f
- Hacemos doble click sobre ella y ponemos el valor a TRUE

Declaración de tipos de llaves ante el sistema operativo
- Habrá que crear un archivo denominado 70-u2f.rules dentro del directorio /etc/udev/rules.d
- Podemos descargar este archivo desde https://github.com/Yubico/libu2f-host/blob/master/70-u2f.rules Este archivo contiene las definiciones de los tipos de llaves u2f más habituales del mercado.

Configuración de Linux Mint para reforzar la seguridad del sistema, haciendo que sea necesario pinchar la llave u2f, tanto al hacer login como al ejecutar cualquier orden con sudo, además de teclear la contraseña.
- En primer lugar habrá que comprobar que el sistema operativo reconoce la llave u2f. Para ello ejecutamos: lsub | grep 2ccf:0880
- Debe aparece una línea similar a esta: Bus 001 Device 007: ID 2ccf:0880 lo que significará que el sistema reconoce la llave u2f
- El siguiente paso será instalar los paquetes pamu2fcfg y libpam-u2f usando: Sudo apt install pamu2fcfg libpam-u2f
- Los paquetes disponibles en los repositorios de Ubuntu, suelen estar menos actualizados que los disponibles en el PPA de Yubico, por lo que si se desea disponer de la última versión, se pueden instalar de la siguiente forma:
sudo add-apt-repository ppa:yubico/stable && sudo apt-get update
sudo apt-get install libpam-u2f
