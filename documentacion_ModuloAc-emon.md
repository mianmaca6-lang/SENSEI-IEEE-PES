# documentacion\_ModuloAc-emon

Nov 11, 2026 · @Miguel Martínez

Guía para conectar, acceder y recuperar la clave de un emonPi de OpenEnergyMonitor. El modelo "emonP1 v2.9" no apareció en la documentación, así que se asumió que es un emonPi.

## Datos clave

| Dato | Valor |
| --- | --- |
| Red WiFi del emonPi | emonsd |
| Clave de esa red | emonsd2022 |
| IP en modo punto de acceso | 192.168.42.1 |
| Hostname en tu red | http://emonpi, http://emonpi.local, http://emonpi.lan |
| Usuario SSH | emonsd (o pi en imágenes viejas) |
| Clave SSH | emonpi2016 |
| Login de emonCMS | Sin usuario por defecto: se crea el primero con Register |

## Instalación física

1. Pinza CT en el cable Line **o** Neutro, nunca en ambos. Si la lectura sale negativa, gira la pinza.
2. No alargues el cable de la pinza CT: mete ruido.
3. Enchufa el adaptador AC-AC y conéctalo al emonPi. Da la referencia de onda para medir potencia real.
4. Fuente USB 5 V de mínimo 1.2 A, con el conector mini-B.
5. Conecta todos los sensores antes de encender.
6. Opcionales: sensor óptico de pulsos y sensores de temperatura por RJ45.

## Seguridad y protecciones

- Las pinzas no tocan la red directamente, pero instalarlas implica trabajar cerca de cables con alto voltaje. Apaga la energía antes y, si dudas, busca un profesional.
- **Apagado correcto:** mantén el botón de shutdown 5 segundos y espera 30 s. Desconectar sin eso puede corromper la tarjeta SD.
- La documentación consultada no menciona fusibles ni protecciones eléctricas internas, así que no se pueden confirmar.
- Cambia la clave de SSH y la de emonCMS al terminar la configuración.

## Especificaciones

| Parámetro | Valor |
| --- | --- |
| Circuitos monitoreados | 2 monofásicos con pinzas CT |
| Corriente CT | 50 mA a 96 A |
| Voltaje | 110-254 VAC (1ª) y 9-12 VAC (2ª) |
| Error por tolerancias | menos de 11% |
| Extras | temperatura DS18B20 y pulsos por RJ45, radio RFM69 de 433 MHz |

## Conexión por WiFi

1. Enciende el emonPi sin cable Ethernet. Puede tardar varios minutos en emitir su WiFi; el LCD marca "WiFi: YES" con la IP 192.168.42.1 cuando está listo.
2. Conecta tu PC o celular a la red **emonsd** con la clave **emonsd2022**. Si tu imagen es anterior a noviembre 2022, la clave puede ser otra (en emonPi2 es emonpi2016).
3. Abre el navegador. Si no sale el asistente solo, entra a http://192.168.42.1.
4. Elige tu red WiFi y escribe su contraseña. Revísala bien: si te equivocas, solo se corrige conectando por Ethernet.
5. El emonPi apaga su punto de acceso, reinicia y se une a tu red. Tu equipo se desconecta de él, es normal.
6. Vuelve a tu WiFi de siempre y entra a http://emonpi.local o a la IP que muestre el LCD.
7. Actualiza en Setup > Admin > Update.

## Recuperar acceso a emonCMS

El emonCMS local no trae usuario por defecto. Si no aparece el botón Register, ya existe una cuenta creada antes. El usuario emonsd es del SSH, no del login web.

1. Entra por SSH: `ssh emonsd@192.168.42.1` (o `pi@...`) con la clave emonpi2016. Si no conecta, activa SSH con el botón del emonPi, que cicla el menú del LCD.
2. Corre `php /opt/emoncms/modules/usefulscripts/resetpassword.php`. Si no existe, prueba `php ~/usefulscripts/resetpassword.php`.
3. Escribe el usuario y la clave nueva que pide el script.
4. Entra a http://192.168.42.1 con esos datos.

**Plan B:** reflashear la tarjeta SD con la imagen emonSD-10Nov22. Borra todo, pero deja registrar una cuenta nueva. Un factory reset por SSH también existe, pero destruye todos los datos.

**Pendiente:** si no sabes qué usuario existe, hay que listarlos con MySQL; ese paso aún no está confirmado.

## Fuentes

- [Install emonPi](https://docs.openenergymonitor.org/emonpi/install.html)
- [Connect (emonPi)](https://docs.openenergymonitor.org/emonpi/connect.html)
- [emonPi Technical Overview](https://docs.openenergymonitor.org/emonpi/technical.html)
- [emonPi2 Install Guide](https://docs.openenergymonitor.org/emonpi2/install.html)
- [Unable to access EMONCMS nor Pi SSH](https://community.openenergymonitor.org/t/unable-to-access-emoncms-nor-pi-ssh/26370)
- [Unable to login to EmonCMS - no register button](https://community.openenergymonitor.org/t/unable-to-login-to-emoncms-no-register-button/27616)
- [Command Line Cheatsheet](https://community.openenergymonitor.org/t/command-line-cheatsheet/650)
