---
description: >-
  A simplified guide to help you update your validators BLS 0x00 credentials to
  execution withdrawal 0x01 credentials using the ETHDO tool by wealdtech.
---

# 🦉 Update Withdrawal Keys for Ethereum Validator (BLS to Execution Change or 0x00 to 0x01) with ETHDO

{% hint style="info" %}
The following steps align with our [mainnet guide](guide-or-how-to-setup-a-validator-on-eth2-mainnet/). You may need to adjust file names and directory locations where appropriate. The core concepts remain the same.
{% endhint %}

## :question:Overview: What? Withdrawals? Owls?

* Greetings, fellow ETH staker! If you were staking before April 2, 2021, setting ETH withdrawal (0x01) credentials was not yet released and so, this guide is relevant for you.
* As of the Shapella upgrade, ETH validators with 0x00 credentials should update to 0x01 credentials to enable partial withdrawals, the sweeping of excess ETH > 32.
* If your validator previously voluntarily exited or you now would like to stop validator duties, you'll need to set your withdrawal credentials to fully reclaim your staked ETH.

<figure><img src="../../.gitbook/assets/withdrawal-owl.png" alt=""><figcaption><p>Nimbus CL says: WITHDROWLS ON</p></figcaption></figure>

## :thumbsup:Pre-requisites: Before you begin

* Your validator's mnemonic keys (the offline 24 word secrets)
* An ETH withdrawal address **you control the private keys** to, ideally one from a hardware wallet. :octagonal\_sign::octagonal\_sign: **DO NOT USE A EXCHANGE ADDRESS!** :octagonal\_sign::octagonal\_sign:
* An **offline air-gapped computer**. Create a Linux Live USB like [Ubuntu](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) or [Tails](https://tails.boum.org/install/download/index.en.html); needs a USB key.
* A ETH staking node using Ubuntu or Linux, also known as **online computer**.
* A USB storage key for moving files between the offline and online computer.
* Familiarize yourself with the [Ethereum.org Staking Withdrawals guide](https://launchpad.ethereum.org/en/withdrawals).

### Step 1: Prepare chain information

* If you no longer have a synced full node, use option 1.
* Option 2 uses your own consensus client to generate chain information.

<details>

<summary>Option 1: Download "offline-preparation.json" file, save to USB key, transfer to offline air-gapped computer.</summary>

1. On your **online computer**, open a terminal window or shell. Shortcut: CTRL + ALT + T

<!---->

2. Download Ethdo v1.30.0 from Github [https://github.com/wealdtech/ethdo/releases](https://github.com/wealdtech/ethdo/releases)

```
cd ~
wget https://github.com/wealdtech/ethdo/releases/download/v1.30.0/ethdo-1.30.0-linux-amd64.tar.gz
```

3. Verify the checksum is valid. Located on the release page, the Checksum string is located in the corresponding sha256 file.

```
echo "6fbe587f522ad2eb8d6ce22dfdb15f7d163b491a670bf50e5acf12dd0f58125c ethdo-1.30.0-linux-amd64.tar.gz" | sha256sum -c
```

Successful verification occurs if you see "OK" in the resulting output.

```
ethdo-1.30.0-linux-amd64.tar.gz: OK
```

4. Extract ethdo.

```
tar -xvf ethdo-1.30.0-linux-amd64.tar.gz
```

5. Verify your validator's credential status with your index number. Replace`<MY-VALIDATOR-INDEX>` accordingly.

```
./ethdo validator credentials get --validator=<MY-VALIDATOR-INDEX>
```

Example output of a validator with BLS credentials. :white\_check\_mark:

```
BLS credentials: 0x0002a0addda8106aed690654c7af7af0bc5ccde321c8e5e2319ff432cee70396
```

If you have BLS credentials, continue with the rest of this guide. Otherwise, stop because ethdo will output "`Ethereum execution address`" and that means you've already set your withdrawal address!

6. Download pre-generated offline preparation files made daily by EthStaker.&#x20;

<pre class="language-bash"><code class="lang-bash">#mainnet
wget https://files.ethstaker.cc/offline-preparation-mainnet.tar.gz
<strong>wget https://files.ethstaker.cc/offline-preparation-mainnet.tar.gz.sha256
</strong>
#goerli
wget https://files.ethstaker.cc/offline-preparation-goerli.tar.gz
wget https://files.ethstaker.cc/offline-preparation-goerli.tar.gz.sha256
</code></pre>

7. Verify the file's sha256 hash against the sha256 files to ensure correctness.

```bash
#mainnet
sha256sum offline-preparation-mainnet.tar.gz

#goerli
sha256sum offline-preparation-goerli.tar.gz
```

The output should match the contents of the .sha256 file. View the contents:

```bash
#mainnet
cat offline-preparation-mainnet.tar.gz.sha256

#goerli
cat offline-preparation-goerli.tar.gz.sha256
```

8. Extract the tar file to find `offline-preparation.json`

```bash
#mainnet
tar -xvf offline-preparation-mainnet.tar.gz

#goerli
tar -xvf offline-preparation-goerli.tar.gz
```

9. Using your USB key, copy both

* the `ethdo` executable&#x20;
* and `offline-preparation.json` file&#x20;

to your offline air-gapped computer.

</details>

<details>

<summary>Option 2: Generate "offline-preparation.json" file, save to USB key, transfer to offline air-gapped computer.</summary>

1. On your **online computer**, open a terminal window or shell. Shortcut: CTRL + ALT + T

<!---->

2. Download Ethdo v1.30.0 from Github [https://github.com/wealdtech/ethdo/releases](https://github.com/wealdtech/ethdo/releases)

```
cd ~
wget https://github.com/wealdtech/ethdo/releases/download/v1.30.0/ethdo-1.30.0-linux-amd64.tar.gz
```

3. Verify the checksum is valid. Located on the release page, the Checksum string is located in the corresponding sha256 file.

```
echo "6fbe587f522ad2eb8d6ce22dfdb15f7d163b491a670bf50e5acf12dd0f58125c ethdo-1.30.0-linux-amd64.tar.gz" | sha256sum -c
```

Successful verification occurs if you see "OK" in the resulting output.

```
ethdo-1.30.0-linux-amd64.tar.gz: OK
```

4. Extract ethdo.

```
tar -xvf ethdo-1.30.0-linux-amd64.tar.gz
```

5. Verify your validator's credential status with your index number. Replace`<MY-VALIDATOR-INDEX>` accordingly.

```
./ethdo validator credentials get --validator=<MY-VALIDATOR-INDEX>
```

Example output of a validator with BLS credentials. :white\_check\_mark:

```
BLS credentials: 0x0002a0addda8106aed690654c7af7af0bc5ccde321c8e5e2319ff432cee70396
```

If you have BLS credentials, continue with the rest of this guide. Otherwise, stop because ethdo will output "`Ethereum execution address`" and that means you've already set your withdrawal address!

6. Run the following command to call your consensus client and generate a list of active validators with relevant information for use on your offline computer. In order to generate this list from your local beacon node, [ensure the REST API](https://github.com/wealdtech/ethdo#setting-up) is enabled; otherwise the default fallback beacon node, [http://mainnet-consensus.attestant.io](http://mainnet-consensus.attestant.io), will be called.

```
./ethdo validator credentials set --prepare-offline
```

After a minute or two, you should see the text, "`offline-preparation.json generated`"

7. Using your USB key, copy both

* the `ethdo` executable&#x20;
* and `offline-preparation.json` file&#x20;

to your offline air-gapped computer.

</details>

### Step 2: Create change credentials file

<details>

<summary>Run ethdo with your mnemonic and withdrawal address. Transfer "change-operations.json" file via USB key back to online computer.</summary>

1. On your **offline air-gapped computer**, disconnect any internet Ethernet cables or WiFi / bluetooth before continuing. Make sure you're truly offline!

<!---->

2. Open a terminal window or shell. Shortcut: CTRL + ALT + T

<!---->

3. After copying the `ethdo` executable and `offline-preparation.json` file to your **offline computer's HOME directory**, ensure the ethdo file has execute permissions.

```
chmod +x ethdo
```

4. This ethdo command sets your validator credentials and the output is stored in a `change-operations.json` file. Replace `<MY MNEMONIC PHRASE>` AND `<MY ETH WITHDRAW ADDRESS>` accordingly.&#x20;

:octagonal\_sign: Double check your work as this is permanent once set! :octagonal\_sign:&#x20;

:octagonal\_sign: FINAL REMINDER: DO NOT USE AN EXCHANGE ETH ADDRESS AS YOUR WITHDRAWAL ADDRESS :octagonal\_sign:

```
./ethdo validator credentials set --offline --mnemonic="<MY MNEMONIC PHRASE>” --withdrawal-address=<MY ETH WITHDRAW ADDRRESS>
```

5. After a few seconds, `change-operations.json` is created. It's normal for no message to be displayed.
6. Triple check the resulting file for your withdraw address.

```
cat change-operations.json
```

7. Ensure the field **"to\_execution\_address":** contains your withdraw address.

<!---->

8. Using your USB key, copy

* `change-operations.json` file

back to your online computer.

9. Power off your **offline air-gapped computer.**

</details>

### Step 3: Broadcast change credentials

<details>

<summary>Simply run the set command to send your change.</summary>

:bulb:If you no longer have a synced full node, you can alternatively upload `change-operation.json` file to [https://beaconcha.in/tools/broadcast](https://beaconcha.in/tools/broadcast)

1. On the **online computer**, copy the `change-operation.json` to your home directory, where `ethdo` is also located.
2. Run the following command to broadcast your withdrawal credentials. &#x20;

```
./ethdo validator credentials set
```

</details>

{% hint style="success" %}
Congrats! Your BLS to Execution change is now pending in a queue, waiting to be included in a block.&#x20;
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
