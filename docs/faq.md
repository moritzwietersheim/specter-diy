## SPECTER-DIY

## *What does the Specter-DIY consist of?*

It consists of:

 - STM32F469 discovery board
 - QR Code scanner from Waveshare
 - Power Bank (small)
 - miniUSB and microUSB cable
 - Prototype Shield
 - A few pin connectors 

[Shopping list link](https://github.com/cryptoadvance/specter-diy/blob/master/docs/shopping.md) + [assembly link](https://github.com/cryptoadvance/specter-diy/blob/master/docs/assembly.md)
Waveshare QR scanner is recommended as it has a good quality/price ratio.

## *Is specter-DIY safe to use?*

Do not use it on mainnet yet unless it's only being used as one of the signers in multisig setup! 
Feel free to experiment with it on testnet, regtest or signet.

## *I'm wondering what if someone takes the device? How does Specter-DIY approach this scenario?*

It supports passphrases as an additional security layer, but currently it has two modes of operation:
- agnostic: when your secrets are not stored on the device and you need to enter recovery phrase every time you use the device
- reckless: when the secret is stored on flash and can be extracted. 

We are working on smartcard support so you could store your keys on removable secure element in a credit card form factor, as well as an option to encrypt secrets with a key stored on the SD card. See this recently opened [issue](https://github.com/cryptoadvance/specter-diy/issues/64) thanks to @Thomas1378 in the Telegram chat!

## *Currently there is a `specter_hwi.py` file, which implements the HWIClient for Specter-DIY. Is there any reason you didn't add that directly to HWI?*

Putting it into HWI means: "this is a hardware wallet people should consider using for real". 
Currently, we would strongly advice NOT to use USB with specter-DIY, but to use QR codes instead.

We will make a PR to HWI when we think it's safe enough. In particular when we will have a secure bootloader that verifies signatures of the firmware, and USB communication is more reliable. 

## *Do you have a physical security design?*

No security at the moment, but it also doesn't store the private key. Working on integration of secure element similar to the ColdCard's (mikroe secure click). At the moment it's more like a toy.

## *Is there a simulator I can try the Specter-DIY with?*

Yes. Specter-DIY in simulator-mode simulates QR code scanner over TCP, see [here](https://diybitcoinhardware.com/f469-disco/simulator/?script=https://raw.githubusercontent.com/diybitcoinhardware/f469-disco/master/docs/tutorial/4_miniwallet/main.py)

## *Is there a goal to get Specter-DIY loading firmware updates from the SD card?*

At the moment we don't have a proper bootloader and secure element integration yet, but we're moving in that direction. 
I think SD card is a good choice, also QR codes might be possible, but we need to experiment with them a bit.

## *Can Specter-DIY register cosigner xpubs like Coldcard? I know you wipe private keys on shutdown, but do you save stuff like that?*

Yes, we keep wallet descriptors and other public info.

## *Once you add the javacard (secure element) you'll save the private keys, too?*

With the secure element you will have three options:

 - agnostic mode, forgets key after shutdown
 - store key on the smartcard but do all crypto on application MCU
 - store key and do crypto on the secure element

Last seems to be the most secure, but then you trust proprietary crypto implementation. Second option saves private key on the secure element under pin protection, but also encrypted, so secure element never knows the private key.

## SPECTER-DEVKIT

## *Can I buy the Specter-devkit pre-built?*

Not yet. There are still a few things to implement before we can say it's secure - bootloader and integration with a secure element. We also need to fix a few things with the housing. So for now, it's DIY only.

With that being said, we are working on a kit (extension board) that includes a QR scanner, battery, charging circuit and a smartcard (secure element) slot. Together with a 3D printed case it is really just plug and play!

No supply-chain risks as you buy the board and a smartcard from normal electronics stores. We will start selling ready to use wallets when we consider it secure enough and when we remove (WIP) from the repo description. Devkits will be available earlier than that.

# VIDEOS

## **1** [Getting started with Specter-DIY and Specter-Desktop](https://twitter.com/CryptoAdvance/status/1235151027348926464)

How to flash and set up an airgapped hardware wallet that uses QR codes to communicate with the host.

In the video: 

 - Flashing the firmware
 - Generating a new key 
 - Importing keys to Specter-Desktop software 
 - Using single-key wallets 
 - Creating a multisignature wallet and importing it to the device 
 - Signing multisig transactions 

## **2** [Assembling Specter-DIY](https://www.youtube.com/watch?v=1H7FqG_FmCw)

Specter-DIY hardware wallet: 

 - off-the-shelf components
 - costs 100$ - assemble in 5 minutes 
 - no soldering 
 - forgets your private key when powered off 

## **3** [Specter-DIY air-gapped open source bitcoin hardware wallet overview](https://twitter.com/KeithMukai/status/1189565099259944961)

## **4** [Build your own bitcoin hardware-wallet YT series](https://www.youtube.com/playlist?list=PLn2qRQUAAg0z_-R0swVuSsNS9bzRu6oP5&app=desktop)

## *What is the difference between the project "diybitcoinhardware.com" & Specter-DIY? Is DIYbitcoinhardware sort of a prerequisite for Specter-DIY?*

Specter-DIY is built on top of that micropython build. DIYbitcoinhardware is focusing more on the toolbox without actual application logic, Specter implements logic and GUI on top of it. 

Yes, that's why the video was recorded - to give some introduction about the tools we use, and hardware wallets logic in general.
