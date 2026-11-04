# T147 DualBoot · E22P-868M30S · Selector30 + WadaFixed22

**Versión vigente: E22P868-Sel30-WadaFixed22-20261104.** Meshtastic expone el selector nominal hasta30 y **30 equivale a22 dBm de excitación del SX1262**; WadaMesh queda **fijo en22 dBm**. El loader de esta ronda nace apuntando a este feed.

**Source OTA:** https://projectsape.github.io/t147-e22p868-ota

## Actualización

1. ConectarWi-Fi en el loader y esperar la sincronización de hora.
2. Si todavía no está configurada la fuente, abrir **OTA→Source→Edit**, escribir elURL completo y guardar con **Done**.
3. Pulsar **Check** y actualizar **WadaMesh y Meshtastic por separado**, siguiendo **Install/Test/Confirm**.
4. En Meshtastic, seleccionar **30** para obtener **22 dBm de excitación**. WadaMesh usa excitación fija de **22 dBm**, independientemente de la preferencia guardada.

Quien ya tiene esta fuente guardada conserva el mismoURL. La OTA mantiene configuración, GPS, redes y canales; el loader permanece idéntico. No necesita otra factory.

**Instalación limpia:** factory privada `APE-T147-E22P868-Selector30-WadaFixed22-SourceFeed-20261104.zip`, que reemplaza la flash completa con loader, tabla y las dos apps ya configuradas.

Perfil exclusivo para T147 con E22P-868M30S, alimentación del radio a 5 V y experimentos cerrados. Conserva frecuencias EU_868, licencia, cifrado y duty cycle. El selector de Meshtastic es un indicador nominal: 30→22, 28→20, 26→18, 22→14 y 20→12 dBm de excitación; no mide la salida del PA.

GPS de fábrica habilitado, RX44/TX43 a9600, con corrección de detecciónNMEA enMeshtastic. Se respetan los ajustes guardados.

[Versiones y hashes](release.json) · [Guía y cableado](LEEME-PINOUT.md) · [Checksums](SHA256SUMS).
