# Prototipação
Projeto da prototipação inicial do sistema

## Objetivo e Escopo

### Objetivo
O objetivo do prototipo inicial é a ciração e validação do sistema projetado, tem como
função analisar a viabilidade de determinadas abordagens e dificuldade de implementação
de uma rede de sondas, além da analise da energia consumida no setup e a funcionalidade 
do MOSFET.

### Escopo
Como o objetivo é apenas teste de software e não tem o comprometimento com performance 
é panejado o uso de componentes baratas que sejam capazes de simular o prototipo final,
desta forma busca economia no preço do prototipo e no preço da perda de componentes.

## Componentes

### Software

Arduino IDE: utilizado para programar o controlador.

### Hardware
As componentes do prototipo são apenas para teste de conexão e logica do codigo, abaixo estão 
componentes de entrada tais quais sensor de temperatura e trasnmissor, eles são versões baratas 
de entradas, neste estágio não buscamos precisão nas leituras apénas teste de conexão e design,
os componentes escolhidos foram escolhidos pelo preço, e com exceção dos controladores estão destinados
a serem excluidos do projeto final. 

A quantidade de componentes é apenas a minima nescessaria para testar o setup, sendo nescessario, eventualmente, 
mais componentes para montar o setup de teste completo.

| **Componente**  | **Quantidade** | **Preço Unitario** |
| :-------------- | :------------: | :----------------: |
| ESP32 DevKit    |       2        |        ~50         |
| Pro Mini 3.3v   |       2        |	    ~30         |
| Adaptador FTDI  | 	  1 	   |	    ~15		    |
| BME280 		  | 	  3        |	    ~35		    |
| Ra-02 433Mhz    |       3	       |	    ~40		    |
| MOSFET 		  |      10	       |	 	 ~2		    |
| Regulador MB102 | 	  1 	   |	    ~15	        |
| ProtoBorad 830  |       2	       |	    ~10		    | 
| Kit Jumpers     | 	  1        | 	    ~30		    |
| INA219 		  | 	  1        | 	    ~20		    |
| **Total**		  |     **24**	   |     **~250**  	    |

Componentes a depender das escolhas anteriores:

| **Componente**            | **Quantidade** | **Preço Unitario** |
| :------------------------ | :------------: | :----------------: |
| DHT22                     |       2        |        ~20         |
| Antena helicoidal 433 MHz |       3        |         ~5         |

## Arquitetura

A arquitetura consiste de um controlador mestre composto de um ESP32 com e um transmissoe Ra-02 433Mhz e sua Antena helicoidal, ele devera fazer a comunicação com os demais para obter suas leituras. O ESP32 é escolhido como mestre por sua capacidade de comunicação por wifi e bluetoof, assim permitindo ser uma controlador central em um ponto estrategico da propriedade.

Além do controlador mestre, é previsto a utilização de até três servos, sendo um deles um ESP32 e os demais ATmegas320p Pro mini 3.3v, o Pro mini tem gasto menor de energia sendo a escolha ideal para o projeto, mas ainda se faz nescessario a utilização de um ESP32 como servo para avaliar seu consumo e performace, tendo em vista que ele oferece muito mais funcionalidade por um preço quase identico.

O consumo energetico é essencial para a atividade, o mestre tendo em vista que é visado a ser colocado em um ponto vantajoso não nescessariamente precisa economizar energia, mas para os servos é essencial, neste prototipo a utilização de um Regulador MB102 como fonte de energia e um INA219 como monitor de consumo servirão para coleta de dados que servirão de benchmark para uso de energia, definindo assim o escopo de consumo para poder ser feita o planejamento da fonte de energia, podendo variar entre pilhas e ou uma bateria recarregavel com uma pequena placa solar.


## Teste

Busca-se fazer os seguintes testes:
- Comunicação: Mestre-Servo, Servo-Mestre, Servo-Servo, Servo-...-Mestre, Criptografia dos Dados, Validade da Informação, Validação do Requisitante, Validação do Requisitado;
- Energia: Gasto em Stand By, Gasto em Atividade Total, Gasto em Medição, Gasto em Trasnmissão, em Gasto Deep Sleep, Capacidade de Inanição do MOSFET;
- Medição: Warm Up do sensor, Precisão do Sensor, Capacidade do Sensor*, Velocidade do Sensor*;
- Conectividade: Conexão do Mestre com WiFi e Bluetooth;
- Arquitetura: Validação da arquitetura física e locação dos componentes.

Alguns testes não se fazem obrigatórios, podendo levar em consideração as especificações da fabricante


## Materiais que preciso comprar

Módulo MOSFET IRF520 x 3
(Modulo do mosfet ja com placa adaptadora) 5 ~ 10 R$

Sx1278 módulo lora 433m 10km Ra-02 módulo sem fio ai-thinker spread espectro transmissão eletrônica kit diy x 3
(Kit do transmissor LoRa com uma antena de 433MHz) 10 ~ 20 R$

INA219 x3
(Módulo sensor de corrente) 15 ~ 25 R$

ESP32 x1
(Arduino com Bluetooth e WiFi) 10 ~ 50 R$

ATmega328 x1
(Arduino simples de baixo consumo, o Pro mini 3.3v precisa de um adaptador serial) 5 ~ 40 R$

FTDI
(Adaptador para o Pro mini) 3 ~ 10 R$

AMS1117-3.3V x3 ?????
(Regulador de voltagem) 3 ~ 10 R$