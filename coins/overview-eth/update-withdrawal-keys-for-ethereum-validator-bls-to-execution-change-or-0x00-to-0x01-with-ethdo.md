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

## :fast\_forward:Next Steps

#### For your information:

* Up to 16 BLS to Execution changes are included in each proposed block.
* Depending on the withdrawal queue size, your withdrawal change may take up to a few days to be finalized.
* Terminology: prefix of 0x01 = "Type 1" = execution withdrawal credentials = Withdrawals enabled
* As a partial withdrawal, periodically every few days any amount of ETH over 32 will be automatically swept to your withdrawal address.

#### Learn more from:

* Official Consensus Layer Withdrawal References
  * Prysm: [https://docs.prylabs.network/docs/wallet/withdraw-validator](https://docs.prylabs.network/docs/wallet/withdraw-validator)
  * Nimbus: [https://nimbus.guide/withdrawals.html](https://nimbus.guide/withdrawals.html)
  * Lighthouse: [https://lighthouse-book.sigmaprime.io/voluntary-exit.html#withdrawal-of-exited-funds](https://lighthouse-book.sigmaprime.io/voluntary-exit.html#withdrawal-of-exited-funds)
  * Teku: [https://docs.teku.consensys.net/HowTo/Withdrawal-Keys](https://docs.teku.consensys.net/HowTo/Withdrawal-Keys)
  * Lodestar: [https://chainsafe.github.io/lodestar/reference/cli/#validator-bls-to-execution-change](https://chainsafe.github.io/lodestar/reference/cli/#validator-bls-to-execution-change)
* Ethdo official withdrawals guide: [https://github.com/wealdtech/ethdo/blob/master/docs/changingwithdrawalcredentials.md](https://github.com/wealdtech/ethdo/blob/master/docs/changingwithdrawalcredentials.md)
* Attestant's Post: [https://www.attestant.io/posts/understanding-withdrawals/](https://www.attestant.io/posts/understanding-withdrawals/)

#### **Need extra live support?**&#x20;

* Find Ethstaker frens on the [Ethstaker](https://discord.io/ethstaker) Discord!
* Use reddit: [r/Ethstaker](https://www.reddit.com/r/ethstaker/), or [DMs](https://www.reddit.com/user/coincashew), or [r/coincashew](https://www.reddit.com/r/coincashew/)

#### Like these guides?

* [Tips much appreciated](../../donations.md) :pray:
* [**Support us on Gitcoin Grants**](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew)**:** We build this guide exclusively by community support!🙏
* Feedback or pull-requests: [https://github.com/coincashew/coincashew](https://github.com/coincashew/coincashew)

## :books:FAQ

<details>

<summary>Using my node, how can I check if my change is pending in the withdrawals queue?</summary>

Replace \<MY VALIDATOR INDEX>. Adjust the REST API port number, if needed.&#x20;

Lighthouse/Nimbus=5052. Prysm=3500. Lodestar=9596. Teku=5051.

```
curl -s "http://localhost:5052/eth/v1/beacon/pool/bls_to_execution_changes" | jq '.data | map(select(.message.validator_index=="<MY VALIDATOR INDEX>"))'
```

Example output:

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

However, if the output shows \[], this means your change is complete and no longer in the queue.

</details>

<details>

<summary>How much longer do I need to wait for this change to take effect?</summary>

Each block can add 16 `blstoexecutionchange`messages and the time to process a BLS change depends on the size of the withdraw queue.&#x20;

Find the size of the queue with the following command.&#x20;

Adjust the REST API port number, if needed.&#x20;

Lighthouse/Nimbus=5052. Prysm=3500. Lodestar=9596. Teku=5051.

```
curl -s http://localhost:5052/eth/v1/beacon/pool/bls_to_execution_changes | jq '.data | length'
```

</details>

<details>

<summary>How do I know the credential change worked?</summary>

Replace `<MyValidatorIndex>` and run the following ethdo command:

```
ethdo validator credentials get --validator=<MyValidatorIndex>
```

The resulting output will start with: `Ethereum execution address`

Alternatively, check your favorite beacon chain explorer such as [beaconcha.in](https://beaconcha.in/validators/withdrawals) and [etherscan.io](https://etherscan.io/) for the 0x01 credentials.

</details>

<details>

<summary>I voluntary exited a while ago and don't have a synced node anymore. What are my options?</summary>

Use Ethdo on an offline computer to create the exit message, as shown in step 2 above, and then perform step 3 using the alternative broadcast method with beaconcha.in

</details>

<details>

<summary>Can I use a Gnosis Safe address as my ETH withdrawal address?</summary>

Yes -- in fact, this is also a great idea as it allows you to rotate private keys (and keep the same public address) or use other more multi-sig strategies.

reference: [https://safe.global](https://safe.global/)

</details>

<details>

<summary>Is the fee-recipient address the same as this withdrawal address?</summary>

They can both be set to the same ETH address; however, understand that these are independent and **withdrawal credentials** have a different purpose than your **fee recipient**, which receives transaction fee tips from proposed blocks.

</details>

<details>

<summary>Partial Withdrawals vs Full Withdrawals?</summary>

* **Full validator withdrawal:** To withdraw your entire stake on Ethereum and no longer perform validator duties. Exit your validator, and then after your exit request progresses through the withdraw queue while finally your full validator balance is transferred to your withdrawal address.
* **Partial validator withdrawal:** To withdraw your validator’s earnings only. For a validator, any amount over the initial 32 ETH deposit is the earnings and is automatically swept every few days to the withdrawal address.

</details>

<details>

<summary>I don't have a mnemonic phrase. I used a private key.</summary>

In step 2, use this credentials set command instead.

```
ethdo validator credentials set --private-key=<my-priv-key> --withdrawal-address=<my-eth-withdrawal-address>
```

</details>

<details>

<summary>I want different withdrawal addresses for each of my validators.</summary>

In step 2, use this credentials set command instead.

```
ethdo validator credentials set --mnemonic="<my-mnemonic-phrase>" --path='m/12381/3600/<my iTH validator>/0' --withdrawal-address=<my-eth-withdrawal-address>
```

Where the path is the derivation path to your withdrawal key.

* For example, `m/12381/3600/`_`i`_`/0` is the path to a withdrawal key, where _i_ starts at 0 for your 1st validator, 1 for your 2nd validator ...

</details>

<details>

<summary>I need to change my withdrawal address.</summary>

The only way to change withdrawal addresses is to perform a full withdrawal by exiting a validator and then, creating a new validator key as if starting the staking journey over again.

</details>
