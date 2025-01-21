---
description : >-
 Una guía simplificada para ayudarlo a actualizar sus credenciales de 0x00 de validación BLS para ejecutar credenciales de 0x01 de retiro utilizando la herramienta ETHDO de wealdtech.
---

# 🦉Actualice las claves de retiro para Ethereum Validator (BLS a cambio de ejecución o 0x00 a 0x01) con ETHDO
{% hint style="info" %}
 Los siguientes pasos se alinean con nuestra [ guía de la red principal](guide-or-how-to-setup-a-validator-on-eth2-mainnet/). Es posible que tenga que ajustar los nombres de los archivos y las ubicaciones de los directorios cuando corresponda. Los conceptos básicos siguen siendo los mismos.
{% endhint %}

## :question:Resumen: ¿Qué? ¿Retiros? ¿Buhos?

* ¡Saludos, compañero staker de ETH! Si estabas apostando antes del 2 de abril de 2021, aún no se había publicado la configuración de las credenciales de retiro de ETH (0x01) y, por lo tanto, esta guía es relevante para ti.
* A partir de la actualización de Shapella, los validadores de ETH con credenciales 0x00 deben actualizarse a credenciales de 0x01 para permitir retiros parciales, el barrido del exceso de ETH > 32.
* Si su validador se retiró voluntariamente anteriormente o ahora desea dejar de cumplir con sus deberes de validador, deberá configurar sus credenciales de retiro para reclamar completamente su ETH apostado.

<figure><img src="../../.gitbook/assets/withdrawal-owl.png" alt=""><figcaption><p>Nimbus CL dice: WITHDROWLS ON</p></figcaption></figure>

## :thumbsup:Pre-r:Requisitos previos: Antes de empezar

* Las claves mnemotécnicas de tu validador (los secretos de 24 palabras fuera de línea)
* Una dirección de retiro de ETH a la que controle las claves privadas, idealmente una de una billetera de hardware. :octagonal_sign::octagonal_sign: ¡NO USE UNA DIRECCIÓN DE INTERCAMBIO! :octagonal_sign::octagonal_sign:
* Un **ordenador sin conexión con espacio de aire**. Crea un Linux Live USB como [Ubuntu](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) o [Tails](https://tails.boum.org/install/download/index.en.html); necesita una llave USB.
* Un nodo de staking de ETH que usa Ubuntu o Linux, también conocido como **computadora en línea**.
* Una llave de almacenamiento USB para mover archivos entre la computadora en línea y sin conexión.
* Familiarízate con la [guía de Ethereum.org Retiros de Staking](https://launchpad.ethereum.org/en/withdrawals).

### Paso 1: Preparar la información de la cadena

* Si ya no tiene un nodo completo sincronizado, utilice la opción 1.
* La opción 2 utiliza su propio cliente de consenso para generar información de la cadena.
<details>

<summary>Opción 1: Descargue el archivo "offline-preparation.json", guárdelo en una llave USB, transfiéralo a una computadora sin conexión.</summary>

1. En su **computadora en línea**,  abra una ventana de terminal o shell. Atajo: CTRL + ALT + T

<!---->

2. Descargar Ethdo v1.30.0 desde Github [https://github.com/wealdtech/ethdo/releases](https://github.com/wealdtech/ethdo/releases)

```
cd ~
wget https://github.com/wealdtech/ethdo/releases/download/v1.30.0/ethdo-1.30.0-linux-amd64.tar.gz
```

3. Compruebe que la suma de comprobación sea válida. Ubicada en la página de versión, la cadena de suma de comprobación se encuentra en el archivo sha256 correspondiente.
```
echo "6fbe587f522ad2eb8d6ce22dfdb15f7d163b491a670bf50e5acf12dd0f58125c ethdo-1.30.0-linux-amd64.tar.gz" | sha256sum -c
```

La verificación correcta se produce si ve "OK" en la salida resultante.

```
ethdo-1.30.0-linux-amd64.tar.gz: OK
```

4. Extraer ethdo.

```
tar -xvf ethdo-1.30.0-linux-amd64.tar.gz
```

5. Verifique el estado de las credenciales de su validador con su número de índice. Reemplace según corresponda.`<MY-VALIDATOR-INDEX>`

```
./ethdo validator credentials get --validator=<MY-VALIDATOR-INDEX>
```

Ejemplo de salida de un validador con credenciales BLS. :white\_check\_mark:

```
BLS credentials: 0x0002a0addda8106aed690654c7af7af0bc5ccde321c8e5e2319ff432cee70396
```

Si tiene credenciales de BLS, continúe con el resto de esta guía. De lo contrario, deténgase porque ethdo generará "" y eso significa que ya ha establecido su dirección de retiro. "`Ethereum execution address`"

6. Descargue archivos de preparación fuera de línea pregenerados diariamente por EthStaker.&#x20;

<pre class="language-bash"><code class="lang-bash">#mainnet
wget https://files.ethstaker.cc/offline-preparation-mainnet.tar.gz
<strong>wget https://files.ethstaker.cc/offline-preparation-mainnet.tar.gz.sha256
</strong>
#goerli
wget https://files.ethstaker.cc/offline-preparation-goerli.tar.gz
wget https://files.ethstaker.cc/offline-preparation-goerli.tar.gz.sha256
</code></pre>

7. Verifique el hash sha256 del archivo con los archivos sha256 para asegurarse de que sea correcto.

```bash
#mainnet
sha256sum offline-preparation-mainnet.tar.gz

#goerli
sha256sum offline-preparation-goerli.tar.gz
```

La salida debe coincidir con el contenido del archivo .sha256. Ver el contenido:

```bash
#mainnet
cat offline-preparation-mainnet.tar.gz.sha256

#goerli
cat offline-preparation-goerli.tar.gz.sha256
```

8. Extraiga el archivo tar para encontrar `offline-preparation.json`

```bash
#mainnet
tar -xvf offline-preparation-mainnet.tar.gz

#goerli
tar -xvf offline-preparation-goerli.tar.gz
```

9. Con su llave USB, copie ambos

* el ejecutable `ethdo` &#x20;
* and file `offline-preparation.json` &#x20;

a su computadora sin conexión aislada.

</details>

<details>

<summaryOpción 2: Genere un archivo "offline-preparation.json", guárdelo en una llave USB, transfiéralo a una computadora sin conexión.</summary>

1. En su **computadora en línea**, abra una ventana de terminal o shell. Atajo: CTRL + ALT + T

<!---->

2. Descargar Ethdo v1.30.0 desde Github [https://github.com/wealdtech/ethdo/releases](https://github.com/wealdtech/ethdo/releases)

```
cd ~
wget https://github.com/wealdtech/ethdo/releases/download/v1.30.0/ethdo-1.30.0-linux-amd64.tar.gz
```

3. Compruebe que la suma de comprobación sea válida. Ubicada en la página de versión, la cadena de suma de comprobación se encuentra en el archivo sha256 correspondiente.

```
echo "6fbe587f522ad2eb8d6ce22dfdb15f7d163b491a670bf50e5acf12dd0f58125c ethdo-1.30.0-linux-amd64.tar.gz" | sha256sum -c
```

La verificación correcta se produce si ve "OK" en la salida resultante.

```
ethdo-1.30.0-linux-amd64.tar.gz: OK
```

4. Extraer ethdo.

```
tar -xvf ethdo-1.30.0-linux-amd64.tar.gz
```

5.Verifique el estado de las credenciales de su validador con su número de índice. Reemplace según corresponda. `<MY-VALIDATOR-INDEX>`

```
./ethdo validator credentials get --validator=<MY-VALIDATOR-INDEX>
```

Ejemplo de salida de un validador con credenciales BLS.. :white\_check\_mark:

```
BLS credentials: 0x0002a0addda8106aed690654c7af7af0bc5ccde321c8e5e2319ff432cee70396
```

Si tiene credenciales de BLS, continúe con el resto de esta guía. De lo contrario, deténgase porque ethdo generará "" y eso significa que ya ha establecido su dirección de retiro. "`Ethereum execution address`"

6.Ejecute el siguiente comando para llamar al cliente de consenso y generar una lista de validadores activos con información relevante para su uso en el equipo sin conexión. Para generar esta lista desde su nodo de baliza local, [asegúrese de que la API REST](https://github.com/wealdtech/ethdo#setting-up) esté habilitada; de lo contrario, se llamará al nodo de baliza de reserva predeterminado, [http://mainnet-consensus.attestant.io](http://mainnet-consensus.attestant.io),.

```
./ethdo validator credentials set --prepare-offline
```

Después de uno o dos minutos, debería ver el texto, "`offline-preparation.json generated`"

7. Con su llave USB, copie ambos
* El ejecutable `ethdo` &#x20;
* y archivo `offline-preparation.json` &#x20;

a su computadora sin conexión aislada.

</details>

### Paso 2: Crear un archivo de credenciales de cambio

<details>

<summary>Ejecute ethdo con su mnemotecnia y dirección de retiro. Transfiera el archivo "change-operations.json" a través de una llave USB a la computadora en línea.</summary>

1. En su **computadora sin conexión, desconecte**, todos los cables Ethernet de Internet o WiFi / bluetooth antes de continuar. ¡Asegúrate de estar realmente desconectado!

<!---->

2. Abra una ventana de terminal o shell. Atajo: CTRL + ALT + T

<!---->

3. Después de copiar el ejecutable y el archivo en **el directorio HOME de su computadora sin conexión**,asegúrese de que el archivo ethdo tenga permisos de ejecución. `ethdo` executable and `offline-preparation.json`

```
chmod +x ethdo
```

4. Este comando ethdo establece sus credenciales de validador y la salida se almacena en un archivo. Reemplace AND en consecuencia. `change-operations.json` `<MY MNEMONIC PHRASE>` `<MY ETH WITHDRAW ADDRESS>` &#x20;

:octagonal\_sign: ¡Revisa tu trabajo, ya que es permanente una vez configurado! :octagonal\_sign:&#x20;

:octagonal\_sign: RECORDATORIO FINAL: NO UTILICES UNA DIRECCIÓN ETH DE INTERCAMBIO COMO TU DIRECCIÓN DE RETIRO :octagonal\_sign:

```
./ethdo validator credentials set --offline --mnemonic="<MY MNEMONIC PHRASE>” --withdrawal-address=<MY ETH WITHDRAW ADDRRESS>
```

5. Después de unos segundos, se crea. Es normal que no se muestre ningún mensaje. `change-operations.json`
6. Verifique tres veces el archivo resultante para su dirección de retiro.

```
cat change-operations.json
```

7. Asegúrese de que el campo  **"to\_execution\_address":** contiene su dirección de retiro.
<!---->

8. Con su llave USB, copie

* `change-operations.json`  archivo

Regrese a su computadora en línea.

9. Apague su **computadora sin conexión sin conexión.**

</details>

### Paso 3: Transmitir credenciales de cambio

<details>

<summary>Simplemente ejecute el comando set para enviar su cambio. </summary>

:bulb:Si ya no tiene un nodo completo sincronizado, también puede cargar el archivo en  `change-operation.json` [https://beaconcha.in/tools/broadcast](https://beaconcha.in/tools/broadcast)

1. En la **computadora en línea**, copie el directorio de su hogar,  `change-operation.json` donde también se encuentra. `ethdo`
2. Ejecute el siguiente comando para transmitir sus credenciales de retiro. &#x20;

```
./ethdo validator credentials set
```

</details>

{% hint style="success" %}
¡Felicidades! El cambio de BLS a Ejecución ahora está pendiente en una cola, a la espera de ser incluido en un bloque.&#x20;
{% endhint %}

## :fast\_forward:Próximos pasos

#### Para su información:

* Se incluyen hasta 16 cambios de BLS a ejecución en cada bloque propuesto.
* Dependiendo del tamaño de la cola de retiro, el cambio de retiro puede tardar unos días en finalizarse.
* Terminología: prefijo de 0x01 = "Tipo 1" = credenciales de retiro de ejecución = Retiros habilitados
* Como retiro parcial, periódicamente cada pocos días, cualquier cantidad de ETH superior a 32 se transferirá automáticamente a su dirección de retiro.

#### Más información en:

* Referencias oficiales de la capa de consenso sobre la retirada
  * Prysm: [https://docs.prylabs.network/docs/wallet/withdraw-validator](https://docs.prylabs.network/docs/wallet/withdraw-validator)
  * Nimbus: [https://nimbus.guide/withdrawals.html](https://nimbus.guide/withdrawals.html)
  * Lighthouse: [https://lighthouse-book.sigmaprime.io/voluntary-exit.html#withdrawal-of-exited-funds](https://lighthouse-book.sigmaprime.io/voluntary-exit.html#withdrawal-of-exited-funds)
  * Teku: [https://docs.teku.consensys.net/HowTo/Withdrawal-Keys](https://docs.teku.consensys.net/HowTo/Withdrawal-Keys)
  * Lodestar: [https://chainsafe.github.io/lodestar/reference/cli/#validator-bls-to-execution-change](https://chainsafe.github.io/lodestar/reference/cli/#validator-bls-to-execution-change)
* Guía oficial de retiros de Ethdo: [https://github.com/wealdtech/ethdo/blob/master/docs/changingwithdrawalcredentials.md](https://github.com/wealdtech/ethdo/blob/master/docs/changingwithdrawalcredentials.md)
* Puesto del atestado: [https://www.attestant.io/posts/understanding-withdrawals/](https://www.attestant.io/posts/understanding-withdrawals/)

#### **¿Necesitas soporte adicional en vivo?**&#x20;

* ¡Encuentra a los frens de Ethstaker en el Discord de [Ethstaker](https://discord.io/ethstaker) .
* Usa reddit: [r/Ethstaker](https://www.reddit.com/r/ethstaker/), o [DMs](https://www.reddit.com/user/coincashew), o [r/coincashew](https://www.reddit.com/r/coincashew/)

#### ¿Te gustan estas guías?

* [Consejos muy apreciados](../../donations.md) :pray:
* [**Apóyanos en Gitcoin Grants**](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew)**:** ¡Construimos esta guía exclusivamente con el apoyo de la comunidad!🙏
* Comentarios o solicitudes de extracción: [https://github.com/coincashew/coincashew](https://github.com/coincashew/coincashew)

## :books:PREGUNTAS MÁS FRECUENTES

<details>

<summary>Usando mi nodo, ¿cómo puedo verificar si mi cambio está pendiente en la cola de retiros?</summary>

Reemplace <MI ÍNDICE DE VALIDADOR>. Ajuste el número de puerto de la API de REST, si es necesario.&#x20;

Lighthouse/Nimbus=5052. Prysm=3500. Lodestar=9596. Teku=5051.

```
curl -s "http://localhost:5052/eth/v1/beacon/pool/bls_to_execution_changes" | jq '.data | map(select(.message.validator_index=="<MY VALIDATOR INDEX>"))'
```

Ejemplo de salida:

```
[
  {
    "message": {
      "validator_index": "96193",
      "from_bls_pubkey": "0xb67aca71f04b673037b54009b760f1961f3836e5714141c892afdb75ec0834dce6784d9c72ed8ad7db328cff8fe9f13e",
      "to_execution_address": "0xb9d7934878b5fb9610b3fe8a5e441e8fad7e293f"
    },
    "signature": "0x988251748925e7a2966f28230c250e8c37495346d551e86fd89ea53148302b1145eb069647572801a689c9c1c5b8f2071019e652e01d92055d9aa99aa86696eb453889de38733caf2d5dce7a2786fed910365dcb7df082a62b130436fb9a1035"
  }
]
```

Sin embargo, si la salida muestra [], significa que el cambio se ha completado y ya no está en la cola.

</details>

<details>

<summary>¿Cuánto tiempo más tengo que esperar para que este cambio surta efecto?</summary>

Cada bloque puede agregar 16 mensajes y el tiempo para procesar un cambio de BLS depende del tamaño de la cola de retiro. `blstoexecutionchange` &#x20;

Encuentre el tamaño de la cola con el siguiente comando.&#x20;

Ajuste el número de puerto de la API de REST, si es necesario.&#x20;

Lighthouse/Nimbus=5052. Prysm=3500. Lodestar=9596. Teku=5051.

```
curl -s http://localhost:5052/eth/v1/beacon/pool/bls_to_execution_changes | jq '.data | length'
```

</details>

<details>

<summary>¿Cómo sé que el cambio de credencial funcionó?</summary>

Reemplace y ejecute el siguiente comando ethdo: `<MyValidatorIndex>`

```
ethdo validator credentials get --validator=<MyValidatorIndex>
```
La salida resultante comenzará con: `Ethereum execution address`

Alternativamente, consulte su explorador de cadenas de balizas favorito, como [beaconcha.in](https://beaconcha.in/validators/withdrawals) y [etherscan.io](https://etherscan.io/) para obtener las credenciales 0x01.

</details>

<details>

<summary>Salí voluntariamente hace un tiempo y ya no tengo un nodo sincronizado. ¿Cuáles son mis opciones?</summary>

Use Ethdo en un equipo sin conexión para crear el mensaje de salida, como se muestra en el paso 2 anterior, y luego realice el paso 3 usando el método de transmisión alternativo con beaconcha.in

</details>

<details>

<summary>¿Puedo usar una dirección de Gnosis Safe como mi dirección de retiro de ETH?</summary>

Sí, de hecho, esta también es una gran idea, ya que le permite rotar las claves privadas (y mantener la misma dirección pública) o usar otras estrategias más multi-sig.
referencia: [https://safe.global](https://safe.global/)

</details>

<details>

<summary>¿La dirección del destinatario de la tarifa es la misma que esta dirección de retiro?</summary>

Ambos se pueden configurar en la misma dirección ETH; Sin embargo, comprenda que estas son independientes y las **credenciales de retiro** tienen un propósito diferente al de su **destinio de tarifa**, que recibe consejos de tarifas de transacción de los bloques propuestos.

</details>

<details>

<summary>¿Retiros parciales vs retiros completos?</summary>

* **Retiro completo del validador:**Retirar toda su participación en Ethereum y dejar de realizar funciones de validador. Salga de su validador y luego después de que su solicitud de salida avance a través de la cola de retiro, finalmente su saldo total del validador se transfiere a su dirección de retiro.
* **Retirada parcial del validador:** Para retirar solo las ganancias de su validador. Para un validador, cualquier cantidad que supere el depósito inicial de 32 ETH es la ganancia y se transfiere automáticamente cada pocos días a la dirección de retiro.

</details>

<details>

<summary>No tengo una frase mnemotécnica. Usé una clave privada.</summary>

En el paso 2, utilice este comando credentials set en su lugar.

```
ethdo validator credentials set --private-key=<my-priv-key> --withdrawal-address=<my-eth-withdrawal-address>
```

</details>

<details>

<summary>Quiero diferentes direcciones de retiro para cada uno de mis validadores.</summary>

En el paso 2, utilice este comando credentials set en su lugar.

```
ethdo validator credentials set --mnemonic="<my-mnemonic-phrase>" --path='m/12381/3600/<my iTH validator>/0' --withdrawal-address=<my-eth-withdrawal-address>
```

Donde la ruta es la ruta de derivación a su clave de retiro.

* Por ejemplo, _i_ es el camino a una clave de retiro, donde i comienza en 0 para su 1er validador, 1 para su 2do validado ... ´m/12381/3600//0' 


</details>

<details>

<summary>Necesito cambiar mi dirección de retiro.</summary>

La única forma de cambiar las direcciones de retiro es realizar un retiro completo saliendo de un validador y luego, creando una nueva clave de validador como si comenzara el viaje de participación nuevamente.

</details>
