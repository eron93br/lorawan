# Documentação gateway LoRaWAN com ESP32
![alt tag](https://www.thethingsnetwork.org/docs/assets/images/architecture.png)

Gateway LoRaWAN de unico canal baseado em ESP32

Baseado na implementação de [Thomas Telkamp](https://github.com/tftelkamp/single_chan_pkt_fwd)
Contribuição principal feita por [Thiago Campos](https://github.com/tcampos123)

Testes realizados por [Eronides Neto](github.com/eron93br). Editado para funcionar com parametros em rede brasileira

Funcionalidades básicas 
--------
- Compatível com freqüências configuráveis (915MHz, 902MHz etc) e mudança do fator de espalhamento (spreadin factor)
- Spreading factor (SF7 to SF12)
- Status de update

Não suportado
--------
- TODO

Conexões (Pinout)
===========
| ESP32 Pin | RF95/LoRa Module SX1276 |
| :---------------------: | :------:|
|  3.3 V  |  3.3 V  |
|  GND |  GND |
|  GPIO4 | DIO2 |
|  GPIO5 | CS |
|  GPIO16 | DIO1 |
|  GPIO17 | DIO0 |
|  GPIO18 |  SCLK |
|  GPIO19 |  MISO |
|  GPIO22 | RST |
|  GPIO23 |  MOSI |

Tutorial Parte 1 - Configurando o Gateway na TTN
-------------
### Material  necessário:
- ESP32 (conectado a Internet)
- Registro na TTN (The Things Network)
- Transceiver LoRa Semtech SX1272 (HopeRF RFM92W), ou SX1276 (HopeRF RFM95W).
- Instalação de algumas bibliotecas

Tutorial Parte 2 - Configurando o Gateway na The Things Network
-------------
1) Crie uma conta na The Things Network
````
    Login no site console.thethingsnetwork.org (gratuito!)
    Selecione 'Gateways' 
    Selecione + register gateway (Registrar Gateway)
````
![alt tag](https://hackster.imgix.net/uploads/attachments/251146/screen_shot_2017-01-14_at_4_EwIJz0eE4x.png?auto=compress%2Cformat&w=680&h=510&fit=max)

2) Coloque o gateway ID que vc configurou anteriormente (campo Gateway EUI), passo 6 da Parte 1, no inicio da pagina e selecione registrar gateway.

![alt tag](https://hackster.imgix.net/uploads/attachments/251147/screen_shot_2017-01-14_at_4_pQP29ElNqh.png?auto=compress%2Cformat&w=680&h=510&fit=max)

3) .... TO DO.....

Tutorial Parte 2 - Configurando no ESP32
-------------

1) Realize as conexoes fisicas como descrita na tabela acima (pinout)! 

Para o firmware do ESP32 para gateway LoRaWAN vamos utilizar a [implementação](https://github.com/kersing/ESP-1ch-Gateway-v5.0) do Jac Kersing, alterada por [Thiago Campos](https://github.com/tcampos123/LoRa-Gateway-with-ESP32), cujo link pode ser acessado [aqui](https://github.com/tcampos123/LoRa-Gateway-with-ESP32).

Alem do firmware acima mecionado voce precisara de algumas bibliotecas adicionais para o seu ESP32.
- A biblioteca [SPIFFS](https://github.com/espressif/arduino-esp32/tree/master/libraries/SPIFFS) 
- Biblioteca [ESP8266 Wifi](https://github.com/esp8266/Arduino)
- Biblioteca [U8G2](https://github.com/nkolban/esp32-snippets/tree/master/hardware/displays/U8G2).
- Biblioteca [ArduinoJson](https://github.com/bblanchon/ArduinoJson)
- Biblioteca [Time](https://github.com/PaulStoffregen/Time)
- Biblioteca [Gbase64](https://github.com/adamvr/arduino-base64)
- [Bibliotecas relacionadas ao ESP](https://github.com/esp8266/Arduino/tree/master/libraries)

Os proximos passos serao referentes a configuracao do gateway com os parametros da TTN!

2) No arquivo **ESP-sc-gway.h** altere na linha **181** o router para  ````thethings.meshed.com.au````

3) Ainda no arquivo **ESP-sc-gway.h** altere nas linhas **197-202** os parametros da TTN que voce configurou na **PARTE 1**.

4) Ainda no arquivo **ESP-sc-gway.h** altere na linha **257** as informacoes da SSID da sua conexao Wi-Fi. 

5) **(OPICIONAL)**: No arquivo **loraModem.h** (linha 31) altere as frequencias de operacao do gateway, SE DESEJADO. O default do arquivo editado ira funcionar no padrao 914.9MHz. 

6) **Configuração do Spreading Factor (SF)**: altere na linha 51 do arquivo **ESP-sc-gway.h**. 

Apos realizar as alteracoes e instalar os pacotes, basta verificar a conexao de pinOut e programar o ESP32 via Arduino IDE para testar o gateway!

**Realize o teste com endpoint!** Confira o tutorial para criar um endpoint [aqui](https://github.com/eron93br/lorawan/tree/master/rpi-gtw/endpoint).

Assim que seu gateway estiver ativado apacera um status na sua pagina, console.thethingsnetwork.org

Outros tutoriais 
-------
[Tutorial para criação do endpoint LoRaWAN](https://github.com/eron93br/lorawan/tree/master/rpi-gtw/endpoint)
[Criação de gateway LoRaWAN com Raspberry Pi](https://github.com/eron93br/lorawan/tree/master/rpi-gtw)

License
-------
The source files of the gateway sketch developed by Jac Kersing in this repository is made available under the MIT license. The libraries included in this repository are included for convenience only and all have their own license, and are not part of the ESP 1ch gateway code.
