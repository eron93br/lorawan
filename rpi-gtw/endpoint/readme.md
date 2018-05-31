# Documentação endpoint LoRaWAN

![alt tag](https://www.thethingsnetwork.org/docs/assets/images/architecture.png)

Testes realizados por [Marcus Felipe](https://github.com/mfrr), [Eronides Neto](github.com/eron93br) e [Thiago Campos](https://github.com/tcampos123).

Editado para funcionar com parametros em rede brasileira

Funcionalidades
--------
- Mensagens de uplink
- SF7 to SF12
- Payload variavel
- Intervalo de envio variavel

Nao suporta
--------
- Mensagens de downlink

Connections (Pinout)
===========
![](./pinout.png)

Tutorial Parte 1 - Configurando o endpoint na The Things Network
-------------

a

Tutorial Parte 2 - Configurando o firmware do endpoint 
-------------

### Material  necessario:

- Arduino UNO ou NodeMCU (ESP8266)
- Registro na TTN (The Things Network)
- Transceiver LoRa Semtech SX1272 (HopeRF RFM92W), ou SX1276 (HopeRF RFM95W).

A) Atualize o Raspberry Pi para a versao mais recente do S.O.
````
sudo apt-get update

sudo apt-get upgrade
````


License
-------
The source files in this repository are made available under the Eclipse
Public License v1.0, except for the base64 implementation, that has been
copied from the Semtech Packet Forwader.
