> This project has been archived. My new project is about using friend' and family's Android phones as seed-signing devices.

# Sonica ⚡🔉

**Sonica** is a cheap seed-signing device for Bitcoin multi-sig wallets.

**Sonica** is an air-gapped, open-source hardware wallet based on an [Arduino ESP32 board](https://github.com/esp8266/Arduino). To sign transactions, the user will use any smartphone to visit the web app and connect to Sonica using *soundwaves*. All smartphones come with a microphone and speakers—which makes it accessible for almost everyone. The accompanying webapp will guide the user through the whole process. Sonica will be cheap to build, easy to use, and totally offline.

**Inspiration**

This project is heavily inspired by [Ben Arc](https://twitter.com/arcbtc). After playing with his [Offline LNURLPoS](https://github.com/arcbtc/LNURLPoS), I'd set out to use that same development board for a "seed signer" (offline hardware wallet). You see, Raspberry Pis are too expensive for a lot of people---and they're also hard to come by---so an ESP32 board made a lot of sense to me. The board comes with an embedded screen which can be used to display QR Codes, so I thought: "That's awesome! I'm pretty sure we can fit a transaction signature in those pixels". But I hit a problem: data entry. How would the user insert the transaction data to the wallet? I wanted it to be air gapped, so anything to do with WiFi, Bluetooth, USB, or serial communication was off the table. I tried running the numbers for a scenario where the user uses the keypad to input the hex data (~7kB) but that would take way too long. That's when I thought of Sonica.

**\
Budget**

-   $10: [ESP8266 board or similar](https://www.amazon.com/HiLetgo-ESP-WROOM-32-Development-Microcontroller-Integrated/dp/B0718T232Z/ref=sr_1_3?keywords=esp32+wroom&qid=1636566791&qsid=135-7559709-6169261&sr=8-3&sres=B0718T232Z%2CB08246MCL5%2CB07QCP2451%2CB085BNHPW5%2CB083RGHB7P%2CB07L4NL1L3%2CB084KWNMM4%2CB07T6J3PXZ%2CB08PCPJ12M%2CB0895GVQRW%2CB0924K91NK%2CB083Q8GR99%2CB07DKD79Y9%2CB077KJNVFP%2CB09BS1XVZN%2CB079PVCF2G&srpt=SINGLE_BOARD_COMPUTER) (a version with no WiFi/Bluetooth would be preferable)

-   $6: [Arduino microphone MAX4466](https://www.amazon.com/outstanding-Microphone-Amplifier-GY-MAX4466-Adjustable/dp/B08K2W1RBB/) (input)

-   $0.40: [Piezo speaker/buzzer](https://arduinogetstarted.com/tutorials/arduino-piezo-buzzer) (output)

-   A smartphone

**Milestones**

-   Month 1: Board pieces delivered and soldered and basic hardware and software tests pass

-   Month 2: Implement a prototype of the soundwave protocol

-   Month 3: Get the [uBitcoin library](https://github.com/micro-bitcoin/uBitcoin) working on the board

-   Month 4: Implement the seed generation algorithm based on sound entropy

-   Month 5: Create prototype of the webapp and implement the basic soundwave protocol

-   Month 6: Get the soundwave protocol locked-in and stable

-   Month 7--8: Create the "Export public key" functionality

-   Month 9--10: Create the "Sign transaction" functionality

-   Month 11: Polish the webapp's UI/UX

-   Month 12: Finalize writing documentation

**User journey**

Setting it up Sonica:

1.  Buy the equipment

2.  Download and compile the source code

3.  Upload hex code to the board (USB-C)

4.  Connect USB-C power cable to turn it on

Creating a seed:

1.  Visit the webapp

2.  Click "Connect"

    1.  Phone will send a CONNECT request through soundwaves

    2.  Sonica will reply with a CONNECTED command

    3.  Now the user will be prompted to generate entropy, using sound (e.g. claps, talking, music, anything goes)

    4.  The phone display a progress bar which fills up as entropy to acquired

    5.  When the bar is full, enough entropy was acquired

    6.  The main menu is displayed

Exporting the extended public key:

1.  On the phone, click "Export XPUB key"

    1.  Phone sends a EXPORT_XPUB request through soundwaves

    2.  Sonica returns a XPUB command with the extended public key

    3.  Phone displays the extended public key as both string and QR Code

Signing a transaction:

1.  On another wallet (e.g. electrum), create the unsigned transaction

2.  Fire up the webapp

3.  Click "Connect"

    1.  If the seed had already been generated we bypass the seed generation step and we're shown the main menu directly

4.  On the main menu, click "Load transaction file" and select the file from the phones local filesystem

5.  Click "Sign transaction"

    1.  Phone sends a SIGN request through soundwaves

    2.  Sonica returns a SIGNED command with the signature

    3.  Phone displays the signed transaction as a QR Code and offers a download button for the hex file

6.  Broadcast the signed transaction using electrum or any open bitcoin transaction broadcasting website

**Related work**

-   [https://github.com/arcbtc/](https://github.com/arcbtc/bowser-bitcoin-hardware-wallet)

-   [https://github.com/ggerganov/](https://github.com/ggerganov/wave-share)

-   [https://github.com/esp8266/](https://github.com/esp8266/Arduino)

-   [https://github.com/micro-bitcoin/uBitcoin](https://github.com/micro-bitcoin/uBitcoin)
