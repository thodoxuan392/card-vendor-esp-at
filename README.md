# ESP-AT
[![Documentation Version](./docs/_static/at_doc_latest.svg)](https://docs.espressif.com/projects/esp-at/en/latest/)

- [中文版](#esp-at-项目)

esp-at project was started and powered by Espressif Systems (@[espressif](https://github.com/espressif/)) as an official project, for the **ESP32-C3**, **ESP32**, **ESP8266**, and **ESP32-S2** Series SoCs provided for Windows, Linux, and macOS.  
It is now supported and maintained by Espressif esp-at team (@[esp-at](https://github.com/espressif/esp-at)).

esp-at is Free Software under a MIT license.

# Introduction
Espressif Wi-Fi and Bluetooth® chipsets are often used as add-on modules to seamlessly integrate wireless connectivity features into new and existing products.  
In an effort to facilitate this and cut down on engineering costs, Espressif Systems has developed a set of AT commands that can be used to interface with Espressif products.

"AT" means 'attention'. Each command string is prefixed with "AT", and a number of discrete commands can be concatenated after the "AT".

The AT command firmware allows for rapid integration by providing:

- In-built TCP/IP stack and data buffering
- Easy integration with resource-constrained host platforms
- Easy-to-parse command-response protocols
- Customized, user-defined AT commands

# Build
```
git checkout release/v2.4.0.0
python3 build.py build
python3 build.py flash
```

# Resources
- There are several guides for esp-at developers and users. These guides can be rendered in a number of formats, like HTML and PDF.  
  Documentation for the latest version: [https://docs.espressif.com/projects/esp-at/en/latest/index.html](https://docs.espressif.com/projects/esp-at/en/latest/index.html). This documentation is built from the [docs directory](https://github.com/espressif/esp-at/tree/master/docs) of this repository.

- The Changelogs of historic released versions: https://github.com/espressif/esp-at/releases

- [Check the Issues section on GitHub](https://github.com/espressif/esp-at/issues) if you find a bug or have a feature request. Please check existing Issues before opening a new one.

- The [esp-at forum](https://www.esp32.com/viewforum.php?f=42) is a place to ask questions and find community resources.

# ESP-AT Support Policy for ESP Chip Series

- ESP32-C3 Series
  - **Preferred recommended chip for using ESP-AT on**
  - Recommended released version: [v2.4.2.0](https://github.com/espressif/esp-at/releases/tag/v2.4.2.0)

- ESP32 Series
  - Recommended released version: [v2.4.0.0](https://github.com/espressif/esp-at/releases/tag/v2.4.0.0)

- ESP8266 Series
  - **ESP32-C3 is recommended to use instead**
  - ESP-AT will not release the major version for ESP8266.
  - ESP-AT no longer adds new features to ESP8266.
  - [v2.2.1.0_esp8266](https://github.com/espressif/esp-at/releases/tag/v2.2.1.0_esp8266) is the last version of ESP-AT for ESP8266, corresponding to branch [release/v2.2.0.0_esp8266](https://github.com/espressif/esp-at/tree/release/v2.2.0.0_esp8266), corresponding to documentation [https://docs.espressif.com/projects/esp-at/en/release-v2.2.0.0_esp8266](https://docs.espressif.com/projects/esp-at/en/release-v2.2.0.0_esp8266/).
  - ESP-AT will regularly update [release/v2.2.0.0_esp8266](https://github.com/espressif/esp-at/tree/release/v2.2.0.0_esp8266) branch for ESP8266. Update includes vital bugfix and security repair.

- ESP32-S2 Series
  - **ESP32-C3 is recommended to use instead**
  - ESP-AT will not release the major version for ESP32-S2.
  - ESP-AT no longer adds new features to ESP32-S2.
  - [v2.1.0.0_esp32s2](https://github.com/espressif/esp-at/releases/tag/v2.1.0.0_esp32s2) is the last version of ESP-AT for ESP32-S2, corresponding to branch [release/v2.1.0.0_esp32s2](https://github.com/espressif/esp-at/tree/release/v2.1.0.0_esp32s2), corresponding to documentation [https://docs.espressif.com/projects/esp-at/en/release-v2.1.0.0_esp32s2](https://docs.espressif.com/projects/esp-at/en/release-v2.1.0.0_esp32s2/).
  - ESP-AT will regularly update [release/v2.2.0.0_esp32](https://github.com/espressif/esp-at/tree/release/v2.2.0.0_esp32) branch for ESP32-S2. Update includes vital bugfix and security repair.

- Other Series
  - ESP-AT will support ESP32-C2 and ESP32-C6 series of chips.
  - ESP-AT will not support ESP32-S and ESP32-H series of chips.
