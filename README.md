# T147 DualBoot · E22P-868M30S · SX22

**Versión vigente: E22P868-SX22-20261104.** Meshtastic se actualiza a la base nueva de upstream (`2.8.3.73eee6b`, 24 commits revisados); WadaMesh conserva el binario publicado exacto. Ambas apps usan excitación directa delSX1262, con default y máximo **+22dBm**, sin compensación de software delPA.

**Source OTA:** https://projectsape.github.io/t147-e22p868-ota

## Actualización

1. ConectarWi-Fi en el loader y esperar la sincronización de hora.
2. Si todavía no está configurada la fuente, abrir **OTA→Source→Edit**, escribir elURL completo y guardar con **Done**.
3. Pulsar **Check** y actualizar **WadaMesh y Meshtastic por separado**, siguiendo **Install/Test/Confirm**.
4. Seleccionar potencia **22** en ambas apps para dejar explícito el ajuste deseado.

Quien ya tiene esta fuente guardada conserva el mismoURL. La OTA mantiene configuración, GPS, redes y canales; el loader permanece idéntico. No necesita otra factory.

**Alternativa por USB:** paquete privado `APE-T147-E22P868-USB-E22P868-SX22-20261104.zip`, con el binario app-only y el script que escribe solo el slot de la app (Meshtastic `0x520000`, WadaMesh `0x1a0000`) y verifica tabla, digest y relectura. No borra configuración ni reemplaza el loader.

Perfil exclusivo paraT147 con E22P-868M30S, alimentación del radio a5V y experimentos cerrados. Conserva frecuenciasEU_868, licencia, cifrado y duty cycle. El selector describe potencia del chip; la salida delPA no está calibrada ni medida.

GPS de fábrica habilitado, RX44/TX43 a9600, con corrección de detecciónNMEA enMeshtastic. Se respetan los ajustes guardados.

[Versiones y hashes](release.json) · [Guía y cableado](LEEME-PINOUT.md) · [Checksums](SHA256SUMS).
