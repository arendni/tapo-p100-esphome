# tapo-p100-esphome
install esphome firmware on TP Link/Tapo P100 smart plug

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
2. Connect an USB to UART tool to the Pins of the Tapo P100 Pads:

	UART <-> P100
	GND <-> GND
	TX <-> RX
	RX <-> TX
	3V3 <-> VCC
	3V3 <-> PA00 (to activate Download mode)

3. 
