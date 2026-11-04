# T147 DualBoot + EBYTE E22P-868M30S

**Perfil OTA vigente: excitación directa +22dBm en ambas apps**, publicado por petición explícita de Enmanuel. La potencia final del PA no está calibrada a1W.

Factory privada para el módulo **E22P-868M30S**, revisión del **1-nov-2026**, con excitación directa del SX1262 a **+22 dBm de fábrica y como máximo**. Incluye **loader con Source vacío y editable**, WadaMesh beta_101 / v1.18.0.1-touch y Meshtastic2.8.2.3593f31 con el arreglo GPS validado. El nombre completo de la imagen y su SHA256 figuran en `SHA256SUMS` y `factory-receipt.json`, entregados junto a esta guía.

Esta variante se preparó aparte de los builds publicados. No se modificó ninguna PCB ni se flasheó la placa de Enmanuel. Es un candidato compilado y verificado por software; falta probar arranque y comunicación RF en el montaje E22P-868 del destinatario. No se certifica potencia RF medida ni alcance.

## Cableado T147 → E22P-868M30S

Los números GPIO son del ESP32-S3 de la Waveshare T147. Los números de pad corresponden al módulo EBYTE, no al header de la T147. Localizarlos con el dibujo del fabricante; no contar contactos desde una fotografía invertida.

| Señal E22P | Pad del módulo | Conectar en T147 |
|---|---:|---|
| NSS / CS |19|GPIO7|
| SCK |18|GPIO9|
| MOSI |17|GPIO11|
| MISO |16|GPIO10|
| NRST |15|GPIO2|
| BUSY |14|GPIO4|
| DIO1 |13|GPIO1|
| EN |6|3V3 mediante resistencia pull-up de10kΩ|
| T/R CTRL |7|Unir al pad8 DIO2 del propio E22P|
| DIO2 |8|Unir al pad7 T/R CTRL; sin GPIO ESP32 adicional|
| VCC |9 y10|Alimentación regulada5V|
| GND |1–5,11,12,20,22|Masa común con la T147 y la fuente5V|
| ANT |IPEX o pad21|Antena adecuada para868MHz,50Ω|

**EN alto y DIO2→T/R CTRL son necesarios en el montaje cableado.** El chip coloca DIO2 alto para TX y bajo para RX; ambos firmwares lo configuran como RF switch. No hace falta añadir GPIO TX_EN/RX_EN separados ni usar GPIO5. El TCXO recibe1.8V por DIO3 **internamente**: no hay que sacar otro cable DIO3 a la T147.

El fabricante especifica VCC3.3–5.25V, lógica3.3V y consumo TX600–670mA a máxima potencia. Para este perfil se recomienda5V con capacidad de al menos1A para el radio, cableado corto y desacoplo cerca de los pads de alimentación. No alimentar el PA desde un GPIO ni asumir que el regulador3V3 de la pantalla puede suministrar su consumo. VCC a5V no convierte la interfaz SPI en lógica5V: las señales de control permanecen a3.3V. Conectar la antena antes de transmitir.

Si también se monta GPS: TX del GPS→GPIO44; RX del GPS←GPIO43; UART9600. GPS viene habilitado de fábrica en ambas apps. Elegir su alimentación según el módulo GPS real; esta guía no le asigna5V por defecto.

## Correspondencia con nuestra PCB de915MHz

La referencia **JungleMesh V1 R4 E22P del8-oct-2026** conecta exactamente estos siete GPIO a los mismos pads. Su R4 mantiene EN alto y el cobre une DIO2 con T/R CTRL. El manual EBYTE agrupa **E22P-868M30S y E22P-915M30S** en el mismo dibujo de22pads,38.5×24mm: el pinout del módulo868 coincide con el915 de esa PCB. La banda y la antena deben ser868; una placa con módulo915 no se vuelve868 cambiando solamente el firmware.

Correspondencia de headers de esa PCB: DIO1=J1.6, NRST=J1.7, BUSY=J1.9, MOSI=J5.7, MISO=J5.8, SCK=J5.9, NSS=J5.11. GPIO5/R10 es una opción no montada para EN; no se requiere para este cableado.

Existe además una revisión **V2 Wio del16-oct-2026**, con huella Wio distinta. Comparte GPIO, pero no los números de pad ni EN/DIO2 externos del E22P. No soldar un E22P a esa huella por equivalencia de nombre. Aquí se entregan únicamente firmware y cableado; ningún archivo PCB/esquema fue modificado.

## Configuración inicial de esta factory

- **Meshtastic:** región inicial `EU_868`, preset habitual del firmware; soporte SX1262, DIO2 como RF switch y TCXO1.8V. Los otros nodos deben usar región/preset/canal compatibles. Este módulo está especificado para863.3–873.3MHz; no seleccionarUS915/ANZ915.
- **WadaMesh:**869.525MHz, BW250kHz, SF11, CR5; comprobar que coincide con la red MeshCore del destinatario. WadaMesh y Meshtastic son protocolos distintos: cada app se conecta a su propia red.
- **Potencia del chip:** ambas apps arrancan con solicitud **+22 dBm al SX1262**, su máximo. Meshtastic usa `SX126X_MAX_POWER=22`, `TX_GAIN_LORA=0` y `USERPREFS_LORACONFIG_TX_POWER=22`. Wada usa su wrapper estándar `CustomSX1262Wrapper`, `LORA_TX_POWER=22` y `MAX_LORA_TX_POWER=22`. Se retiró la conversión privada anterior y su techo de8 dBm. En esta revisión el selector describe la excitación del chip: todavía permite bajarla, pero no solicita30 dBm al SX1262.

La potencia seleccionada **no mide la salida del PA**. La curva EBYTE a5V muestra aproximadamente **+32 dBm RF con ajuste +22 dBm** en el E22P-868: alrededor de1.6W, no un limitador automático a1W. Es una lectura aproximada de la gráfica, no una medición de este montaje. La salida real depende del módulo y su alimentación; no se garantiza una potencia final concreta. La compensación0 indica que este perfil no calcula la salida RF del PA; no significa que el PA carezca de ganancia.

Meshtastic conserva su región inicial `EU_868`, cuyo límite nominal documentado es27 dBm. **Sin compensación de PA, ese límite del software no garantiza limitar la salida final a27 dBm.** Esta variante entrega excitación máxima del chip por solicitud del owner; no se presenta como perfil calibrado de27 dBm ni como certificación de cumplimiento regional. Usar la región permitida en la ubicación real; `EU_868` no determina el país del destinatario. No se cambia `is_licensed`, cifrado, tabla regional ni duty cycle.

## Instalación de la factory

Imagen completa **16MiB**, para **ESP32-S3, flash16MB, PSRAM8MB**, escrita desde **offset0x0**. Contiene loader, particiones y ambas apps. **La instalación factory reemplaza la flash y borra la configuración anterior del destinatario.** No usarla como una app OTA ni escribirla en0x10000/0x1a0000/0x520000.

Con esptool5, reemplazar `PUERTO` y `ARCHIVO.factory.bin` por los valores reales:

```bash
python3 -m esptool --chip esp32s3 --port PUERTO write-flash --flash-mode keep --flash-freq keep --flash-size keep 0x0 ARCHIVO.factory.bin
```

Verificar `SHA256SUMS` antes de instalar. El loader inicia su menú DualBoot; elegir WadaMesh o Meshtastic. Comprobar reconocimiento SX1262, recepción y envío con un segundo nodo compatible de868MHz antes de dar el montaje por validado.

## Source de actualizaciones del loader

**El Source inicial es texto vacío.** Conectarse al Wi-Fi no consulta ni descarga nuestros updates. Tampoco Check ni Install inician una operación con el campo vacío. La URL estándar APE no está incluida en el binario de este loader privado como fuente por defecto.

Para configurarlo en la pantalla: **Loader → OTA → Source → Edit**. Escribir la URL HTTPS raíz del feed compatible y pulsar **Done** en el teclado. Se guarda en NVS y permanece tras reiniciar. **Clear** borra la URL y conserva el Wi-Fi guardado; también se puede guardar el campo vacío desde Edit. **Cancel** sale sin guardar. El campo admite hasta191 caracteres.

La URL es la raíz que contiene las carpetas `wadamesh` y `meshtastic`, no un enlace directo a un `.bin`. Se conservan la comprobación de firmas, placa y particiones: configurar un Source no habilita instalar binarios arbitrarios ni feeds sin las firmas compatibles. No introducir el feed Wio estándar APE en este montaje E22P; reemplazaría la adaptación privada si se elige instalar ese firmware. El loader no instala automáticamente después de guardar un Source: los pasos Install/Test/Confirm siguen siendo manuales.

La factory de respaldo sigue siendo local; las dos apps SX22 se publican en el feed OTA independiente. Se conserva el loader original.

## Documentación técnica utilizada

- [EBYTE, producto E22P-868M30S](https://www.cdebyte.com/products/E22P-868M30S).
- [Manual EBYTE descargado desde la ficha oficial](https://www.cdebyte.com/pdf-down.aspx?id=4333): §3.2, pinout común868/915; §4.2, tabla EN/T-R, DIO2, TCXO1.8V y curva de potencia. La copia privada y su hash quedan en la evidencia de compilación.
- [Meshtastic SX126xInterface](https://github.com/meshtastic/firmware/blob/332b5b0a2c4fec2896de3c5fa63f46d5cd05ea98/src/mesh/SX126xInterface.cpp): control DIO2/TCXO y límites del chip.
- [Meshtastic RadioInterface::limitPower](https://github.com/meshtastic/firmware/blob/332b5b0a2c4fec2896de3c5fa63f46d5cd05ea98/src/mesh/RadioInterface.cpp#L1495): compensación del PA y límite final.
- [Meshtastic configuración LoRa](https://meshtastic.org/docs/configuration/radio/lora/): región, preset y canal.

Meshtastic ya dispone del driver y mecanismos necesarios. En el código oficial revisado332b5b0 no hay un preset específico E22P-868M30S para nuestra T147 DualBoot; las opciones de otros E22 no equivalen a este módulo. Esta factory aplica su configuración de placa/potencia sin cambiar los protocolos del driver, y conserva el arreglo GPS local previamente validado.

## Feed OTA vigente

Actualización `E22P868-Sel30-WadaFixed22-20261104`: Meshtastic con la escala nominal del selector (30 =22 dBm) y WadaMesh con excitación fija en22 dBm. Se conserva configuración y GPS.

Fuente: `https://projectsape.github.io/t147-e22p868-ota`.
En el loader, conectarWi-Fi, guardar esa fuente en **OTA→Source→Edit**, hacer **Check** y actualizar **WadaMesh y Meshtastic por separado**, siguiendo **Install/Test/Confirm**. Después seleccionar potencia **22** en ambas apps. Se conserva la configuración y el GPS; no se reemplaza el loader. Quien ya usa esa fuente solo necesitaCheck.

La versiónSX22 sustituye la referencia anterior con excitación10. Los valores del selector ahora describen potencia del chip, sin compensación de software del PA. La salida final del módulo puede superar1W según la curva del fabricante; no es una medición de la unidad real.
