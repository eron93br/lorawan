# Documentação endpoint LoRaWAN

![alt tag](https://www.thethingsnetwork.org/docs/assets/images/architecture.png)

Testes realizados por [Marcus Felipe](https://github.com/mfrr), [Eronides Neto](github.com/eron93br) e [Thiago Campos](https://github.com/tcampos123).

Editado para funcionar com parametros em rede brasileira

### Material  necessario:

- Arduino UNO ou NodeMCU (ESP8266)
- Registro na TTN (The Things Network)
- Transceiver LoRa Semtech SX1272 (HopeRF RFM92W), ou SX1276 (HopeRF RFM95W).
- Biblioteca padrão [AU915](https://github.com/tpet93/lmic-Arduino-Australia) ou biblioteca Padrão [US915](https://github.com/tcampos123/LoRa-Node-TTN-LoRaWAN-915)

Funcionalidades
--------
- Mensagens de uplink
- SF7 to SF12
- Payload variavel
- Intervalo de envio variavel

Não suporta
--------
- Mensagens de downlink

Connections (Pinout)
===========
| Semtech SX1276/77/78/79 | Arduino UNO |
| :---------------------: | :------:|
| VCC | 3.3V |
| GND | GND |
| SCK | SCK |
| MISO | MISO |
| MOSI | MOSI (13) |
| NSS | 6 |
| NRESET | 5 |
| DIO0 | 2 |
| DIO1 | 3 |
| DIO2 | 4 |


Tutorial Parte 1 - Configurando o endpoint na The Things Network
-------------

Cada dispositivo (endpoint) conecatando a um gateway LoRaWAN da TTN precisa ser registrado na rede. Assim, cada dispositivo ehh tratado como uma aplicacao diferente. Logo, temos que registrar os diferentes dispostivos (aplicacoes). 

1) Entre no console da TTN e clique adicionar aplicacao (add aplication). 

![alt tag](https://www.thethingsnetwork.org/docs/applications/add-application.png)

2) Escolha para o campo Application ID uma indentificacao unica SEM caps lock.

3) No campo descricacao da aplicacao (Application Description) escreva o que voce desejar fazer...

4) Deixe o checkbox marcado para a sua regiao default (a mesma configurada anteriormente no gateway!). 

5) Clique em  Add Application para finalizar. Agora voce tem os parametros App EUI e Access Key para configurar o firmware do seu endpoint. Ainda torna-se necessario o registro do dispositivo. 

6) Abra a aplicacao que voce acabou de criar e clique em registrar dispositivo (Register device).
````
Defina o ID do device...
Em Device EUI, defina uma sequencia, por exemplo A1 A2 A3 A4 A5 A6 A7 A8...
Clique em ABP activation. 
Deixe o App Key para ser gerado. 
Em App EUI, selecione o EUI gerado da lista. 
````
7) Clique em Register para finalizar. Você será redirecionado para o dispositivo recém registrado, onde poderá encontrar a chave do aplicativo gerada necessária para ativar o dispositivo.

8) Visto que usamos ativacao por ABP 
````
No menu superior direito, selecione Configurações. Para o método de ativação, clique em ABP.
Deixe a chave de sessão de rede e a chave de sessão de aplicativos gerada para você ou clique em personalizá-la se quiser defini-las por conta própria.
Clique em Salvar para finalizar.
Você será redirecionado de volta ao dispositivo, onde você encontrará o endereço do dispositivo e as chaves de sessão necessárias para ativar o dispositivo.
````
Caso voce tenha achado confuso o passo a passo anterior, seguem os links para o passo a passo de como criar a [aplicacao](https://www.thethingsnetwork.org/docs/devices/registration.html) e adicionar um [dispositivo](https://www.thethingsnetwork.org/docs/devices/registration.html)

Tutorial Parte 2 - Configurando o firmware do endpoint 
-------------

1) Baixe e instale a sua IDE do Arduino a LMIC library modificada, [deste link](https://github.com/tcampos123/LoRa-Node-TTN-LoRaWAN-915).

2) Utilize este [código](https://github.com/eron93br/lorawan/blob/master/rpi-gtw/endpoint/lorawan-node.ino) como base e exemplo, colocando seus valores de app session key, network session key e device address.

3) Edite se quiser alguns parâmetros, como o TX_INTERVAL (intervalo de transmissão) e mensagem a ser enviada...

4) Programe o Arduino e seja feliz! Let's connect and make incrible things :) 

# Programando o Arduino sketch

Para programar o Arduino, utilizamos a biblioteca [LMIC](https://github.com/things4u/LoRa-Thing/tree/master/libraries/lmic-v1.5). Vale ressaltar que usamos uma configuracao modificada, para operar na frequencia brasileira. 

Siga o passo (2) anterior e programe o Arduino UNO/NodeMCU.

```
/*******************************************************************************
 * Copyright (c) 2015 Thomas Telkamp and Matthijs Kooijman
 *
 * Permission is hereby granted, free of charge, to anyone
 * obtaining a copy of this document and accompanying files,
 * to do whatever they want with them without any restriction,
 * including, but not limited to, copying, modification and redistribution.
 * NO WARRANTY OF ANY KIND IS PROVIDED.
 *
 * This example sends a valid LoRaWAN packet with payload "Hello,
 * world!", using frequency and encryption settings matching those of
 * the (early prototype version of) The Things Network.
 *
 * Note: LoRaWAN per sub-band duty-cycle limitation is enforced (1% in g1,
 *  0.1% in g2).
 *
 * Change DEVADDR to a unique address!
 * See http://thethingsnetwork.org/wiki/AddressSpace
 *
 * Do not forget to define the radio type correctly in config.h.
 *
 *******************************************************************************/

#include <lmic.h>
#include <hal/hal.h>
#include <SPI.h>

// LoRaWAN NwkSKey, network session key
// This is the default Semtech key, which is used by the prototype TTN
// network initially.
static const PROGMEM u1_t NWKSKEY[16] = {0x9B,0x72,0x2A,0x37,0x69,0x6B,0xB1,0x97,0xD3, 0x26, 0xB3, 0xF2, 0x02, 0x0C, 0xB7, 0x0A};

// LoRaWAN AppSKey, application session key
// This is the default Semtech key, which is used by the prototype TTN
// network initially.
static const u1_t PROGMEM APPSKEY[16] = {0xC0, 0xF3, 0xE9, 0x48, 0x87, 0x59, 0x5D, 0xEF, 0x0C, 0xA9, 0x4B, 0xA5, 0x65, 0xF2, 0xF4, 0xAA};

// LoRaWAN end-device address (DevAddr)
// See http://thethingsnetwork.org/wiki/AddressSpace
static const u4_t DEVADDR = 0x26002580; // <-- Change this address for every node!

// These callbacks are only used in over-the-air activation, so they are
// left empty here (we cannot leave them out completely unless
// DISABLE_JOIN is set in config.h, otherwise the linker will complain).
void os_getArtEui (u1_t* buf) { }
void os_getDevEui (u1_t* buf) { }
void os_getDevKey (u1_t* buf) { }

static uint8_t mydata[] = "H";
static osjob_t sendjob;

// Schedule TX every this many seconds (might become longer due to duty
// cycle limitations).
const unsigned TX_INTERVAL = 60;

// Pin mapping... Change the pins according to microcontroller used

// .dio={DIO0,DIO1,DIO2}

// Pin mapping
const lmic_pinmap lmic_pins = {
    .nss = 6,
    .rxtx = LMIC_UNUSED_PIN,
    .rst = 5,
    .dio = {2, 3, 4},
};

void onEvent (ev_t ev) {
    Serial.print(os_getTime());
    Serial.print(": ");
    switch(ev) {
        case EV_SCAN_TIMEOUT:
            Serial.println(F("EV_SCAN_TIMEOUT"));
            break;
        case EV_BEACON_FOUND:
            Serial.println(F("EV_BEACON_FOUND"));
            break;
        case EV_BEACON_MISSED:
            Serial.println(F("EV_BEACON_MISSED"));
            break;
        case EV_BEACON_TRACKED:
            Serial.println(F("EV_BEACON_TRACKED"));
            break;
        case EV_JOINING:
            Serial.println(F("EV_JOINING"));
            break;
        case EV_JOINED:
            Serial.println(F("EV_JOINED"));
            break;
        case EV_RFU1:
            Serial.println(F("EV_RFU1"));
            break;
        case EV_JOIN_FAILED:
            Serial.println(F("EV_JOIN_FAILED"));
            break;
        case EV_REJOIN_FAILED:
            Serial.println(F("EV_REJOIN_FAILED"));
            break;
            break;
        case EV_TXCOMPLETE:
            Serial.println(F("EV_TXCOMPLETE (includes waiting for RX windows)"));
            if(LMIC.dataLen) {
                // data received in rx slot after tx
                Serial.print(F("Data Received: "));
                Serial.write(LMIC.frame+LMIC.dataBeg, LMIC.dataLen);
                Serial.println();
            }
            // Schedule next transmission
            os_setTimedCallback(&sendjob, os_getTime()+sec2osticks(TX_INTERVAL), do_send);
            break;
        case EV_LOST_TSYNC:
            Serial.println(F("EV_LOST_TSYNC"));
            break;
        case EV_RESET:
            Serial.println(F("EV_RESET"));
            break;
        case EV_RXCOMPLETE:
            // data received in ping slot
            Serial.println(F("EV_RXCOMPLETE"));
            break;
        case EV_LINK_DEAD:
            Serial.println(F("EV_LINK_DEAD"));
            break;
        case EV_LINK_ALIVE:
            Serial.println(F("EV_LINK_ALIVE"));
            break;
         default:
            Serial.println(F("Unknown event"));
            break;
    }
}

void do_send(osjob_t* j){
    // Check if there is not a current TX/RX job running
    if (LMIC.opmode & OP_TXRXPEND) {
        Serial.println(F("OP_TXRXPEND, not sending"));
    } else {
        // Prepare upstream data transmission at the next possible time.
        LMIC_setTxData2(1, mydata, sizeof(mydata)-1, 0);
        Serial.println(F("Packet queued"));
        Serial.println(LMIC.freq);
    }
    // Next TX is scheduled after TX_COMPLETE event.
}

void setup() {
    Serial.begin(9600);
    Serial.println(F("Starting"));

    #ifdef VCC_ENABLE
    // For Pinoccio Scout boards
    pinMode(VCC_ENABLE, OUTPUT);
    digitalWrite(VCC_ENABLE, HIGH);
    delay(1000);
    #endif

    // LMIC init
    os_init();
    // Reset the MAC state. Session and pending data transfers will be discarded.
    LMIC_reset();
   
    // Set static session parameters. Instead of dynamically establishing a session
    // by joining the network, precomputed session parameters are be provided.
    #ifdef PROGMEM
    // On AVR, these values are stored in flash and only copied to RAM
    // once. Copy them to a temporary buffer here, LMIC_setSession will
    // copy them into a buffer of its own again.
    uint8_t appskey[sizeof(APPSKEY)];
    uint8_t nwkskey[sizeof(NWKSKEY)];
    memcpy_P(appskey, APPSKEY, sizeof(APPSKEY));
    memcpy_P(nwkskey, NWKSKEY, sizeof(NWKSKEY));
    LMIC_setSession (0x1, DEVADDR, nwkskey, appskey);
    #else
    // If not running an AVR with PROGMEM, just use the arrays directly 
    LMIC_setSession (0x1, DEVADDR, NWKSKEY, APPSKEY);
    #endif

  for (int channel=0; channel<63; ++channel) {           // set frequency by choosing the active channel (this case 914.9Mhz = channel 63)
    LMIC_disableChannel(channel);
  }
  for (int channel=64; channel<72; ++channel) {          // set frequency by choosing the active channel (this case 914.9Mhz = channel 63)
     LMIC_disableChannel(channel);
  }

    // Disable link check validation
    LMIC_setLinkCheckMode(0);
 
    // Set data rate and transmit power (note: txpow seems to be ignored by the library)
    LMIC_setDrTxpow(DR_SF7,14);

    // Start job
    do_send(&sendjob);
Serial.println("Freq");
Serial.println(LMIC.freq);
}

void loop() 
{
    os_runloop_once();
}
```

Lembramndo, mais uma vez, que para funcionar com seu gateway voce devera colocar os seus valores configurados de:
- DEVEUI
- APPEUI
- APPKEY

License
-------
The source files in this repository are made available under the Eclipse
Public License v1.0, except for the base64 implementation, that has been
copied from the Semtech Packet Forwader.
