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
