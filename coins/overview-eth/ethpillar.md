---
description: >-
  Empowered, inspired, home staker. Free. Open source. Public goods for
  Ethereum.
---

# 🛡️ EthPillar: one-liner setup tool and node management TUI

{% hint style="success" %}
:heart: Support us on **Gitcoin** GR20: [https://explorer.gitcoin.co/#/round/42161/26/34](https://explorer.gitcoin.co/#/round/42161/26/34)
{% endhint %}

## :new: What is EthPillar?

:smile: **Friendly Node Installer**: No node yet? Helps you installs a Ethereum node (Nimbus+Nethermind) stack in just minutes. MEVboost included.

:floppy\_disk: **Ease of use**: No more remembering CLI commands required. Access common node operations via a simple text user interface (TUI).

:owl: **Fast Updates**: Quickly find and download the latest consensus/execution release. Less downtime!

:tada:**Compatibility**: Behind the scenes, node commands and file structure are identical to V2 staking setups.&#x20;

{% hint style="warning" %}
Already a running a Validator? EthPillar is compatible with [a Coincashew V2 Staking Setup.](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet)&#x20;
{% endhint %}

## :sunglasses: Preview

<figure><img src="../../.gitbook/assets/preview02.png" alt=""><figcaption><p>Main Menu</p></figcaption></figure>

<div>

<figure><img src="../../.gitbook/assets/preview01.png" alt=""><figcaption><p>Execution Client</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/preview03.png" alt=""><figcaption><p>Consensus Client</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/preview04.png" alt=""><figcaption><p>Validator</p></figcaption></figure>

</div>

<div>

<figure><img src="../../.gitbook/assets/preview05.png" alt=""><figcaption><p>System Administration</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/preview06.png" alt=""><figcaption><p>Tools</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/preview07.png" alt=""><figcaption><p>Mevboost</p></figcaption></figure>

</div>

## :whale: Prerequisites

* [Review how staking works and the hardware requirements](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/prerequisites.md)
* An [Ubuntu](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/prerequisites.md#setup-ubuntu) installation.&#x20;
  * Tested working with Ubuntu 22.04 LTS
  * Also appears compatible with Linux Mint 21.2, Debian 12

## :triangular\_ruler: Option 1: Automated One-Liner Install

Open a terminal window from anywhere by typing `Ctrl+Alt+T`.&#x20;

To install, paste the following:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/coincashew/EthPillar/main/install.sh)"
```

## :handshake: Option 2: Manual Install

**Install updates and packages:**

```bash
sudo apt-get update && sudo apt-get install git curl ccze bc tmux
```

**Clone the ethpillar repo and install:**

```bash
mkdir -p ~/git/ethpillar
git clone https://github.com/coincashew/ethpillar.git ~/git/ethpillar
sudo ln -s ~/git/ethpillar/ethpillar.sh /usr/local/bin/ethpillar
```

#### Run ethpillar:

```bash
ethpillar
```

## :tada:Next Steps

{% hint style="success" %}
Congrats on installing a EthPillar, making nodes and home staking easier!
{% endhint %}

<details>

<summary>Additional step for new Node operators, new Validators</summary>

**Step 1: Configure your network, port forwarding and firewall.**&#x20;

* With EthPillar, configuration can be changed at:
  * **Tools > UFW Firewall > Enable firewall with default settings**
  * Port forwarding is [manually configured](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-2-configuring-node.md#configure-port-forwarding), depending on your router.
  * Confirm port forwarding is working with **Tools** > **Port Checker**
* Alternatively configure manually per the manual guide. [Click here for detailed network configuration.](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-2-configuring-node.md#network-configuration)

**Step 2: Configure your BIOS to auto power on after power loss**

Actual steps vary depending on your computer's BIOS. General idea here: [https://www.wintips.org/setup-computer-to-auto-power-on-after-power-outage/](https://www.wintips.org/setup-computer-to-auto-power-on-after-power-outage/)

**Step 3: Enable Monitoring and Alerts (Optional)**

Found under:

* **Tools** > **Monitoring**

**Step 4: Benchmark your node (Optional)**

Ensure your node has sufficient CPU/disk/network performance.

* **Tools** > **Yet-Another-Bench-Script**

</details>

<details>

<summary>Additional steps for new Validators</summary>

**Step 1: Setup Validator Keys**

* Familarize yourself with the main guide's section on [setting up your validator keys.](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-5-installing-validator/setting-up-validator-keys.md)
* When ready to generate your keys, go to **EthPillar > Validator Client > Generate / Import Validator Keys**

**Step 2: Upload deposit\_data.json to Launchpad**

* To begin staking on Ethereum as a validator, you need to submit to the Launchpad your  deposit\_data.json file, which includes crucial withdrawal address details, and pay the required deposit of 32ETH per validator.

**Step 3: Congrats!**&#x20;

* Now you're waiting in the Entry Queue [https://www.validatorqueue.com](https://www.validatorqueue.com/)

<!---->

* Check out the [next steps from the main guide](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-5-installing-validator/next-steps) for further knowledge. Especially the FAQ's "Wen staking rewards?"

</details>

## :joy: POAP

¿Eres un EthPillar Enjooyer? [¡Apoye este bien público comprando un POAP de edición limitada!](https://checkout.poap.xyz/169495)

<figure><img src="../../.gitbook/assets/3adf69e9-fb1b-4665-8645-60d71dd01a7b.png" alt=""><figcaption><p>El POAP de su EthPillar Enjoyoor</p></figcaption></figure>

**Enlace de compra:** [https://checkout.poap.xyz/169495](https://checkout.poap.xyz/169495)

ETH aceptado en Mainnet, Arbitrum, Base, Optimism. :pray:

## ☎️ Póngase en contacto con nosotros

¿Tiene preguntas? Chatea con otros stakers de casas en [Discord](https://discord.gg/dEpAVWgFNB) o abre PRs/issues en [Github](https://github.com/coincashew/ethpillar).&#x20;

Código fuente abierto disponible aquí: [https://github.com/coincashew/EthPillar](https://github.com/coincashew/EthPillar)

## :heart: Donaciones

Si quieres apoyar este proyecto de bienes públicos, encuéntranos en las próximas subvenciones de Gitcoin.

Nuestra dirección de donación es [0xCF83d0c22dd54475cC0C52721B0ef07d9756E8C0](https://etherscan.io/address/0xCF83d0c22dd54475cC0C52721B0ef07d9756E8C0) o coincashew.eth

## :ballot\_box\_with\_check: Cómo actualizar

{% tabs %}
{% tab title="TUI Update" %}
Al abrir EthPillar,

* Vaya a Administración del sistema > Actualizar EthPillar** y a continuación, salga y vuelva a iniciar.
{% endtab %}

{% tab title="Manual Update" %}
Desde un terminal, extraiga las últimas actualizaciones de git.

```bash
cd ~/git/ethpillar
git pull
```
{% endtab %}
{% endtabs %}

## :tada: Créditos

Un saludo a [accidental-green](https://github.com/accidental-green/validator-install) por su trabajo pionero en herramientas de validación de Python, que ha encendido involuntariamente la inspiración y la dirección de este proyecto. Estamos construyendo sobre sus bases innovadoras mediante la bifurcación de su código de instalación de validador. ¡Un sincero agradecimiento a accidental-green por sus contribuciones revolucionarias al ecosistema Ethereum de código abierto!
