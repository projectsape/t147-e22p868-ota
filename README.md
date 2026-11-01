# T147 DualBoot · E22P-868M30S · OTA experimental

Fuente OTA independiente para la Waveshare T147 DualBoot con módulo **EBYTE E22P-868M30S**, alimentación del radio a **5V** y el loader privado con Source editable.

**Source:** https://projectsape.github.io/t147-e22p868-ota

## Actualizar la primera versión

1. Entrar al loader y conectar Wi-Fi. Esperar la sincronización de hora.
2. Abrir **OTA → Source → Edit**.
3. Escribir `https://projectsape.github.io/t147-e22p868-ota` y guardar con **Done**.
4. Pulsar **Check**. Actualizar **WadaMesh**, siguiendo **Install / Test / Confirm**.
5. Repetir la actualización para **Meshtastic**.
6. En cada app, seleccionar potencia **30** una vez. Si tenía guardado27, la OTA conserva ese ajuste hasta cambiarlo.

Se actualizan las apps por separado. Se conserva configuración, GPS, redes y canales; no hace falta instalar otra factory ni cambiar el loader.

## Perfil incluido

- E22P-868M30S, GPIO y control del módulo documentados en [la guía](LEEME-PINOUT.md).
- Solicitud nominal **30dBm RF**, excitación máxima **10dBm**. Referencia tomada de la curva EBYTE a5V: aproximadamente1W cerca del ajuste10. **Sin medición RF ni calibración de la unidad real.**
- Perfil para **experimentos cerrados**: frecuenciasEU_868, con excepción privada al límite regional de potencia de Meshtastic. Licencia/cifrado y duty cycle sin cambios.
- GPS de fábrica habilitado, RX44/TX43 a9600, con detección NMEA corregida en Meshtastic. Se respetan preferencias guardadas.
- Loader compatible con la primera factory privada deSource vacío/editable. Manifiestos ECDSA P-256/SHA-256, placa y layoutOTA-v2 originales.

Este feed corresponde exclusivamente a esta varianteE22P868. La factory habitual paraWio utiliza otro perfil. El loader permanece conSource vacío en instalaciones nuevas: se configura esta URL manualmente.

[Metadatos y hashes de las apps](release.json) · [Checksums del paquete](SHA256SUMS) · [Guía y pinout](LEEME-PINOUT.md).

Referencia primaria: [manual EBYTE, §4.2](https://www.cdebyte.com/pdf-down.aspx?id=4333). El PA no añade siempre8dB ni limita automáticamente a1W; la gráfica muestra cerca de32dBm con excitación22.
