>[!WARNING]
>**Please read the safety notes in [README](../README.md) before starting with the manual below!**

## Prerequisite

1. Install ltchiptool

	```bash
	python3 -m venv ~/ltchiptool-venv
	source ~/ltchiptool-venv/bin/activate
	pip install --upgrade pip
	pip install ltchiptool
	```

	**NOTE: For further help simply run:**

	```bash
	ltchiptool --help
	```
2. Disassemble the P100 Case by using some plectrums or other flat items. This can be vary hard an annoying because the plastic clips are very tight. Starting from the "bottom side" of the plug seems to work.
**NOTE: Do not use too much force. That will break the clips and the housing my be too loose which is a potential safety risk because of the 220V inside the plug.**

	![p100_1](images/IMG_6721.HEIC)

	![p100_2](images/IMG_6722.HEIC)

	![p100_3](images/IMG_6723.HEIC)

4. Connect an USB to UART tool to the Pins of the Tapo P100 Pads:

	**NOTE: If your USB to UART tool supports multiple voltages, make sure to use 3.3V, otherwise you could damage the 				P100 plug!**

	<p>UART <-> P100<br>
	GND <-> GND<br>
	TX <-> RX<br>
	RX <-> TX<br>
	3V3 <-> V33<br>
	3V3 <-> GPIO0 (to activate Download mode)</p>

	![p100_4](images/IMG_6724.HEIC)

	![p100_5](images/IMG_6726.HEIC)

## Preparing ESPHome config

The following steps are taken inside the [ESPHome Integration in HomeAssistant](https://www.home-assistant.io/integrations/esphome/). Other ESPHome Installations should also work but the steps to create the config may vary.

1. Go to the ESPHome Dashboard and click on "New Device", choose the "empty configuration" option.
2. Paste the content of the [tapo-p100-esphome.yaml](/esphome/tapo-p100-esphome.yaml) inside the config field and **make sure to edit the Friendly Name, id and WiFi-credentials**
3. click on "save" and the device will appear in the device list.
4. Inside the device Manu, click on "install" and choose the option "manual download".
5. Chose the option "UF2 package" and the file will be downloaded.

## Save the default firmware

It's recommended to dump the default firmware from the P100 to your Computer

1. plug in the UART to USB tool with the P100 attached to it (double check the correct wiring and make sure GPIO0 is connected to 3V3).
2. open a terminal and find your USB Device
	```bash
	ls /dev/tty.usb*
	```
	The output should be something like:
	```bash
	/dev/tty.usbserial-XXXXXXXX
	```
3. dump the original firmware:
	```bash
	ltchiptool flash read rtl8720c tapo-p100-original.bin -d /dev/tty.usbserial-XXXXXXXX
	```


## Flash config on P100 plug

Now you can flash the downloaded UF2 file.

1. plug in the UART to USB tool with the P100 attached to it (double check the correct wiring and make sure GPIO0 is connected to 3V3).

2. Start the flashing process with the path to your UF2-file:
	```bash
	ltchiptool flash write your_downloaded_esphome_file.uf2 -d /dev/tty.usbserial-XXXXXXXX
	```
3. You can see the flashing progress. Once it has finished your terminal should show:
	```bash
	Booting firmware  [################################################################]  100%

	I: |-- Finished in 66.813 s
	```
## Restart the Tapo P100

1. Disconnect the USB to UART tool form your computer.
2. **Before disconnecting all wires from the P100 Pads, I would recommend just disconnect the GPIO0 pad from 3V3 and plug in the USB to UART tool again, to test the P100.**
3. Your Device should work as expected, pressing the on/off button at the side of the P100 should toggle the relay and you should be able to control the P100 via ESPHome.
4. Now you can desolder all pads of the P100 and reassemble the case
5. Finish!
