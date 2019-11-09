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

Configuración de Linux Mint para reforzar la seguridad del sistema, haciendo que sea necesario pinchar la llave u2f tanto al hacer login como al ejecutar cualquier orden con sudo.
