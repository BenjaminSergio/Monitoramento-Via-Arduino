# Medição de Solo usando arduino

## Pesquisa dos componentes

Para a confecção de um sistema baseado em arduino é necessário especificar escopo e eficácia a serem alcançados, neste caso o objetivo é a confecção de um medidor de ph, umidade e nutrientes essenciais para a planta, nitrogênio fósforo e potássio (NPK). O medidor terá a função de ser levado a um campo de plantação de café e servir como fonte de monitoramento da plantação.

Tendo em mente que o projeto é destinado a monitoramento de plantações de café temos que trabalhar com os seguintes parametros:
* Durabilidade: O aparelho confeccionado deve ser resistente a colisões e intempéries, tendo resistencia para sobreviver a longos periodos de tempo no campo mantendo funcionalidade e precisão.
* Precisão: O aparelho confeccionado devera ter alta precisão, sendo capaz de coletar dados confiaveis para avaliações técnicas, servindo como apoio para o monitoramento e ciclo de vida da lavoura, idealmente o aparelho não deve substituir mas cooperar  analises de solo com métodos já consolidados.
* comunicação: Como prova de conceito é aceitavel um display, para demonstrar o resultado das medições feitas pelos sensores, em vista do uso planejado para o aparelho é ideal um metodo de comunicação a longa distancia para monitoramento, tanto para captação de dados constantes, quanto para alertas ao responsavel, desta forma é possivel pensar em transmissão via radio ou  outros meios. 
* Autonomia: O aparelho confeccionado devera ter autonomia para longevidade de medição, como é esperado de sua função, o aparelho deve ser capaz de continuar funcionando por periodos longínquos de tempo sem interferencia externa, possiveis soluções energeticas são baterias e placas solares, é necessário que o sistema seja inchuto quanto o consumo de energia.
* Custo: Tendo em vista que a natureza do projeto possivelmente demandara varios sensores espalhados por longas distancias de terra e estando no estagio de prova de conceito, deve se planejar o menor custo possivel para o projeto final, levando em consideração não apenas os pontos mencionados acima, mas também a distribuição dos aparelhos pela area a ser monitorada.


#### Estrutura:
Microcontrolador como o ATmega328P e ou ESP32, apenas para captar os dados dos sensores colocá-los e enviá-los como uma struct para um microcontrolador mestre, também se faz necessário a utilização de um sistema de comunicação a longa distancia, como LoRa e ou ondas de radio, oque for melhor para lavouras.

A comunicação deve ser feita como uma rede de aparelhos conectados, de preferencia com a antena em um poste mais alta que a planta de café, para melhor propagação do sinal pela lavoura.

Os sensores não devem ser muito caros erros ou a nescessidade de trabalho nos dados de entrada podem ser aliviados através de modelos usando um computador central mais robusto.

A comunicação deve ser feita entre o controlador principal (master) que ficara em facil acesso e os controladores secundarios (slaves) que ficarão na lavoura, se necessário os controladores secundarios poderão servir de ligação para com o mestre, funcionando assim como uma rede, com o proposito de permitir maior distancia para comunicação.

A autonomia do controlador é essencial, devera ter a capacidade de passar longos periodos de tempo sem ser carregado e ou ter a capacidade de se recarregar, desta forma torna-se essencial gravitar por uma operação eficiente, podendo variar entre periodos de comunicação e ou tempo entre Medição.

Visibilidade, idealmente o sistema deve ter visibilidade completa, desde a quantidade de bateria quanto a vida util das peças, além de conformidade quanto a tempo de medição e log dos registros.

### Proof of Concept (POC)
No caso da prova de conceito não se faz tão necessário a preocupação com o gasto energetico, idealmente deve se fazer um conjunto de um controlador e três sondas, sendo que o controlador deve ser um controlador tal qual ESP32, pelas funcionalidades com wifi e bluetooth, e as sondas devem ser uma com um ESP32, e as demais com ATmega328p. O minimo para teste e validação é pelo menos um controlador como ESP32 como mestre e um outro controlador, podendo ser mais fraco como servo, este minimo serve para testar comunicação e mensuramento da terra.

ESP32 gasta mais energia, não sendo necessáriamente o ideal para a ser levado ao campo, pórem tem serventia estrategica como um controlador central em um ponto estrategico, tanto para processamento dos dados de chegada quanto para envio para um outro dispositivo como um computador ou por wifi. 

Para teste de comunicação é necessário no minimo um par mestre/servo, idealmente deve se ter um ou dois servos a mais para testar conexão por rede e roteamento/trafego de informação.

Os sensores são um ponto crítico do modelo, para maior entendimento da situação da lavoura durante é planejado a utilização de um sensor como o BME280 capaz de medir temperatuda, umidade e pressão atimosferica, e um sensor como o JXCT RS458 para medição de solo como o Sensor 7 em 1, que mede a umidade, temperatura, ph, condutividade eletrica e NPK.

[JXCT RS485 ModbusJXBS-3001-NPK-RS Manual do usuário do sensor de solo NPK](https://manuals.plus/pt/jxct/rs485-modbusjxbs-3001-npk-rs-soil-npk-sensor-manual)

### Testes
Deverão ser feito testes para o sistema de comunicação, sistema de medição, sistema de hibernação, sistema energetico e durabilidade:
- sistema de comunicação: deverão ser feitos testes de comunicação, perda de pacotes e perda de informação, com e sem postes par aumentar o raio efetivo do sinal;
- sistema de medição: deverão ser realizados teste de precisão dos sensores, testes de durabilidade dos sensores, e de suas partes fisicas, muitos destes sensores não mensuram a quantidade e NPK diretamente, se faz necessário validar o dado coletado e tentar métodos de tratamento dos dados;
- sistema de hibernação: deverão ser realizados teste para o sistema de hibernação do microcontrolador e suas adições em prol de impedir consumo exacerbado de energia;
- sistema energetico: deverão ser feitos testes de consumo, capacidade e durabilidade;
- durabilidade: para garantir as medições realizadas e calculo de custos devera ser realizada amplos teste quanto a durabilidade e tempo de vida do aparelho, com objetivo de registrar quanto tempo de vida confiavel possui e ciclos de intervenção/monitoramento tecnico.

#### Pontos Crítico
Deve levar em consideração os seguintes pontos críticos do projeto capazes de inviabilizar a implanação
- Sensores: Os sensores devem ser testados o maximo possivel, tanto para precisão quanto para vida util, sensores como o BME280 são confiaveis e amplamente utilizados mas sensores de solo particularmente para a medição de NPK podem ser problematicos, tendo em vista que eles fazem a medição de forma indireta, desta forma pode se fazer necessário a transformação dos dados captados por meio de um algoritico. 
- Comunicação: Mesmo LoRa sendo capaz de comunicação em longas distancias apresenta decaimento severo da capacidade de comunicação quando em meios com muita barreira natural, tal qual a folhagem das plantas na lavoura, desta forma deve-se testar a utilização de postes, para colocar a antena numa posição mais vantajosa, e a utilização de um sistema de comunicação distribuido entre as sondas.
- Gasto Energetico: Como o objetivo é a criação de uma sonda capaz de viver por longos periodos de tempos na lavoura se faz necessário a avaliação do gasto energetico, tanto para a utilização de baterias quanto no caso de utilização de placas solares, por isso a utilização de um ATmega328P e um MOSFET para os sensores na lavoura, a ideia é buscar um sono profundo e impedir maiores gastos energeticos.
- Custo: o maior custo do projeto esta na componente do sensor e trasmissor, 
	- os controladores ATmega328P e ESP32 são encontrados nas faixas de 40 a 70 reais, alguns modelos chegando na faixa dos 100 reais, 
	- um sensor bme280 é encontrado até 45 reais, 
	- um transmissor EBYTE E220-900T22D custa cerca de 100 reais, 
	- um mosfet custa cerca de 2 reais a unidade, 
	- o sistema energito fica a depender da escolha, entre um sistema recarregavel, utilizando painel solar e um sistema a pilha com preços variados com preços variados na casa dos 100 reais, 
	- o sensor RS485 tem preços que podem chegar até 500 reais, sendo o componente mais caro.
	- e a impermeabilização do projeto final, para garantir tempo de vida, deve ter seu preço pesquisado  após a confecção de um prototipo. 
- Os preços dos componentes podem variar de acordo com a quantidade encomendada as manufaturas.


### Modelo Planejado
1. Arquitetura do Nó Escravo (Campo)
O equipamento implantado na lavoura tem o único objetivo de acordar, realizar a medição, transmitir os dados e voltar a hibernar rapidamente.
- Microcontrolador: Utiliza-se o ATmega328P (versão 3.3V a 8MHz, como o Arduino Pro Mini). Devido à sua extrema eficiência energética e ausência de componentes parasitas (como conversores USB e LEDs de energia presentes no Arduino Uno), ele atinge um consumo em deep sleep de cerca de 0.1 µA a 3 µA.
- Gestão de Energia Física: A alimentação dos sensores periféricos e do rádio é gerida por um MOSFET Canal-P (AO3401) atuando como chave High-side no polo positivo da alimentação. O microcontrolador aciona o MOSFET apenas no momento da leitura, cortando fisicamente a energia do resto do circuito durante a hibernação e evitando o dreno de 40 a 80 mA contínuos dos sensores industriais.
	- Sensores Empregados:
		- Solo (RS485 Modbus): Sensor capacitivo de nível industrial para medir Umidade, Temperatura e Condutividade Elétrica (CE) verdadeira do solo. Os dados brutos NPK relatados por sensores econômicos são ignorados devido à incapacidade física de medirem compostos específicos.
		- Ar: Sensor BME280 operando via I2C. Ele substitui o antigo DHT11 por possuir altíssima precisão e modo de repouso profundo nativo, medindo temperatura, umidade e pressão atmosférica com consumo mínimo.
	- Transmissão: Comunicação de dados utilizando o módulo rádio LoRa EBYTE E220-900T22D (3.3V). Para penetrar a densa barreira vegetativa do cafezal, o rádio é configurado com a potência máxima de transmissão de 22 dBm e a antena deve ser elevada acima do dossel ( > 2 metros). Os dados são empacotados e enviados em binário (struct), evitando formatos como JSON, para manter o "tempo de rádio ligado" abaixo de 5 segundos por ciclo.
	
2. Arquitetura do Nó Mestre / Gateway (Sede)
O equipamento Mestre fica abrigado na sede da fazenda com acesso a fontes de energia mais robustas e conectividade com a internet.

- Microcontrolador: Baseado no ESP32, escolhido por seu processador robusto de 32 bits, conectividade Wi-Fi/Bluetooth integrada e maior capacidade de memória RAM para gerenciar dezenas de nós no campo.

- Processamento e Inteligência (NPK por IA): Ao receber o pacote de dados brutos (CE, umidade e temperatura) dos nós escravos via LoRa, o ESP32 atua como porta de entrada para algoritmos de Inteligência Artificial (como Florestas Aleatórias/Random Forest). Modelos preditivos correlacionam a medição física precisa do sensor de solo com análises laboratoriais (ground truth), permitindo predizer as concentrações reais de NPK com exatidão que pode chegar a 92%, solucionando a limitação do hardware de baixo custo.

- Interface: Após o processamento, o ESP32 empacota os resultados finais em formato JSON e os encaminha para plataformas em nuvem via Wi-Fi para visualização remota e emissão de alertas.

3. Invólucro e Impermeabilização (Nível IP68)A sobrevivência em campo aberto, exposto a agrotóxicos, variações térmicas violentas e submersão em poças exige impermeabilização absoluta de nível IP68.


#### Papers Pesquisados
[Detection of air temperature, humidity and soil pH by using DHT22 and pH sensor based Arduino nano microcontroller](https://pubs.aip.org/aip/acp/article/2221/1/100008/687623/Detection-of-air-temperature-humidity-and-soil-pH)
	
[Evaluating the Effectiveness of a Solar-Powered Arduino Real-Time Transmitter in Measuring Moisture, Temperature, and Environmental pH Levels in Soil](https://animorepository.dlsu.edu.ph/conf_shsrescon/2025/paper_csr/7/)

[Design and Build a Soil Nutrient Measurement Tool for Citrus Plants Using NPK Soil Sensors Based on the Internet of Things](https://www.researchgate.net/publication/357796648_Design_and_Build_a_Soil_Nutrient_Measurement_Tool_for_Citrus_Plants_Using_NPK_Soil_Sensors_Based_on_the_Internet_of_Things)

[Experimental Study on the Porpagation Characteristics of LoRa Signals in Maize Fields](https://www.mdpi.com/2079-9292/14/11/2156)
	


<h2>TODO</h2>

<details open>
<summary><input type="checkbox"> Prototipo</summary>
<details open>
<summary><input type="checkbox"> Prototipo v.01</summary>
<details open>
<summary><input type="checkbox"> Componentes</summary>
<label><input type="checkbox" checked> Orçamento</label><br>
<label><input type="checkbox"> Compra</label><br>
<label><a href="Prototipo/README.md">Documentação</a></label>
</details>
<label><input type="checkbox"> Montagem</label><br>
<label><input type="checkbox"> Teste</label>
</details>
<label><input type="checkbox"> Prototipo controlador</label><br>
<label><input type="checkbox"> Prototipo comunicador</label><br>
<label><input type="checkbox"> Prototipo sonda</label><br>
<label><input type="checkbox"> Prototipo deep sleep</label>
</details>
<label><input type="checkbox"> Avaliação do custo energetico</label><br>
<label><input type="checkbox"> Avaliação da precisão dos sensores</label><br>
<label><input type="checkbox"> Avaliação das distâncias entre sondas</label><br>
<label><input type="checkbox"> Avaliação da altura dos postes</label><br>
<label><input type="checkbox"> Avaliação da resistencia e durabilidade</label>



<style>
  summary {
    cursor: pointer;
    font-weight: normal;
  }

  details {
    margin-left: 1em;
  }

  label {
    cursor: pointer;
  }
</style>