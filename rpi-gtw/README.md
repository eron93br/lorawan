Gateway LoRaWAN de unico canal baseado em Raspberry Pi

Baseado na implementacao de [Thomas Telkamp](https://github.com/tftelkamp/single_chan_pkt_fwd)
Contribuicao principal feita por [Thiago Campos](https://github.com/tcampos123)

Testes realizados por [Marcus Felipe](https://github.com/mfrr) e [Eronides Neto](github.com/eron93br)

Editado para funcionar com parametros em rede brasileira

Funcionalidades
--------
- listen on configurable frequency and spreading factor
- SF7 to SF12
- status updates
- can forward to two servers

Nao suporta
--------
- PACKET_PUSH_ACK processing
- SF7BW250 modulation
- FSK modulation
- downstream messages (tx)

Dependencies
------------
- SPI needs to be enabled on the Raspberry Pi (use raspi-config)
- WiringPi: a GPIO access library written in C for the BCM2835 
  used in the Raspberry Pi.
  sudo apt-get install wiringpi
  see http://wiringpi.com
- Run packet forwarder as root

Connections
-----------
SX1276 - Raspberry

3.3V   - 3.3V (header pin #1) 
GND	   - GND (pin #6)
MISO   - MISO (pin #21)
MOSI   - MOSI (pin #19)
SCK    - CLK (pin #23)
NSS    - GPIO6 (pin #22)
DIO0   - GPIO7 (pin #7)
RST    - GPIO0 (pin #11)

Configuration
-------------

Defaults:

- LoRa:   SF7 at 868.1 Mhz
- Server: 54.229.214.112, port 1700  (The Things Network: croft.thethings.girovito.nl)

Edit source node (main.cpp) to change configuration (look for: "Configure these values!").

Please set location, email and description.

License
-------
The source files in this repository are made available under the Eclipse
Public License v1.0, except for the base64 implementation, that has been
copied from the Semtech Packet Forwader.