# Medição de Solo usando arduino

## Pesquisa dos componentes

Para a confecção de um sistema baseado em arduino é nescessario especificar escopo e eficacia a serem alcançados, neste caso o objetivo é a confecção de um medidor de ph, humidade e nutrientes essenciais para a planta, nitrato fósforo e potássio (NPK). O medidor terá a função de ser levado a um campo de plantação de café e servir como fonte de monitoramento da plantação.

Tendo em mente que o projeto é destinado a monitoramento de plantações de café temos que trabalhar com os seguintes parametros:
* Durabilidade: O aparelho confeccionado deve ser resistente a colisões e intemperies, tendo resistencia para sobreviver a longos periodos de tempo no campo mantendo funcionalidade e precisão.
* Precisão: O aparelho confeccionado devera ter alta precisão, sendo capaz de coletar dados confiaveis para avaliações tecnicas, servindo como apoio para o monitoramento e ciclo de vida da lavoura, idealmente o aparelho não deve substituir mas cooperar  analises de solo com metodos já consolidados.
* Comunição: Como prova de conceito é aceitavel um display, para demonstrar o resultado das medições feitas pelos sensores, em vista do uso planejado para o aparelho é ideal um metodo de comunicação a longa distancia para monitoramento, tanto para captção de dados constantes, quanto para alertas a o responsavel, desta forma é possivel pensar em transmissão via radio ou outrem. 
* Autonomia: O aparelho confeccionado devera ter autonomia para longevidade de medição, como é esperado de sua função, o aparelho deve ser capaz de continuar funcionando por periodos longinquos de tempo sem interferencia externa, possiveis soluções energeticas são baterias e placas solares, é nescessario que o sistema seja inchuto quanto o consumo de energia.
* Custo: Tendo em vista que a natureza do projeto possivelmente demandara varios sensores espalhados por longas distancias de terra e estando no estagio de prova de conceito, deve se planejar o menor custo possivel para o projeto final, levando em consideração não apénas os pontos mencionados acima, mas também a distribuição dos aparelhos pela area a ser monitorada.


#### Estrutura:
Microcontrolador como o ATmega328P, apénas para captar os dados dos sensores colocalos em um modelo de registro de dados como JSON, também se faz nescessario a utilização de um sistema de comuniação a longa distancia, como LoRa e ou ondas de radio, oque for melhor para lavouras.
Os sensores não devem ser muito caros erros ou a nescessidade de trabalho nos dados de entrada podem ser aliviados atravez de modelos usando um computador central mais robusto.
A comunicação deve ser feita entre o controlador principal (master) que ficara em facil acesso e os controladores secundarios (slaves) que ficarão na lavoura, se nescessario os controladores secundarios poderão servir de ligação para com o mestre, funcionando assim como uma rede, com o proposito de permitir maior distancia para comunicação.
A autonomia do controlador é essencial, devera ter a capacidade de passar longos periodos de tempo sem ser carregado e ou ter a capacidade de se recarregar, desta forma torna-se essencial gravitar por uma operação eficiente, podendo variar entre periodos de comunicação e ou tempo entre Medição.
Visibilidade, idealmente o sistema deve ter visibilidade completa, desde a quantidade de bateria quanto a vida util das peças, além de conformidade quanto a tempo de medição e log dos registros.

### Prove of Concept (POC)
No caso da prova de conceito pode-se utilizar peças mais caras para produção industrial e ou com maior consumo de energia, tendo em vista que serve para testar a funcionalidade do todo quanto a confiabilidade das medições

### Testes
Deverão ser feito testes para o sistema de comunicação, sistema de medição, sistema de hibernação, sistema energetico e durabilidade:
- sistema de comunicação: deverão ser feitos testes de cominicação, perda de pacotes e perda de informação;
- sistema de medição: deverão ser realizados teste de precisão dos sensores e testes de durabilidade dos sensores, e de suas partes fisicas;
- sistema de hibernação: deverão ser realizados teste para o sistema de hibernação do microcontrolador e suas adições em prol de impedir consumo exacerbado de energia;
- sistema energetico: deverão ser feitos testes de consumo, capacidade e durabilidade;
- durabilidade: para garantir as medições realizadas e calculo de custos devera ser realizada amplos teste quanto a durabilidade e tempo de vida do aparelho, com objetivo de registrar quanto tempo de vida confiavel possui e ciclos de intervenção/monitoramento tecnico.


### Modelo Planejado
1. Arquitetura do Nó Escravo (Campo)
O equipamento implantado na lavoura tem o único objetivo de acordar, realizar a medição, transmitir os dados e voltar a hibernar rapidamente.
- Microcontrolador: Utiliza-se o ATmega328P (versão 3.3V a 8MHz, como o Arduino Pro Mini). Devido à sua extrema eficiência energética e ausência de componentes parasitas (como conversores USB e LEDs de energia presentes no Arduino Uno), ele atinge um consumo em deep sleep de cerca de 0.1 muA a 3 muA.
- Gestão de Energia Físico: A alimentação dos sensores periféricos e do rádio é gerida por um MOSFET Canal-P (AO3401) atuando como chave High-side no polo positivo da alimentação. O microcontrolador aciona o MOSFET apenas no momento da leitura, cortando fisicamente a energia do resto do circuito durante a hibernação e evitando o dreno de 40 a 80 mA contínuos dos sensores industriais.
	- Sensores Empregados:
		- Solo (RS485 Modbus): Sensor capacitivo de nível industrial para medir Umidade, Temperatura e Condutividade Elétrica (CE) verdadeira do solo. Os dados brutos NPK relatados por sensores econômicos são ignorados devido à incapacidade física de medirem compostos específicos.
		- Ar: Sensor BME280 operando via I2C. Ele substitui o antigo DHT11 por possuir altíssima precisão e modo de repouso profundo nativo, medindo temperatura, umidade e pressão atmosférica com consumo mínimo.
	- Transmissão: Comunicação de dados utilizando o módulo rádio LoRa EBYTE E220-900T22D (3.3V). Para penetrar a densa barreira vegetativa do cafezal, o rádio é configurado com a potência máxima de transmissão de 22 dBm e a antena deve ser elevada acima do dossel ( > 2 metros). Os dados são empacotados e enviados em binário (struct), evitando formatos como JSON, para manter o "tempo de rádio ligado" abaixo de 5 segundos por ciclo.
	
2. Arquitetura do Nó Mestre / Gateway (Sede)O equipamento Mestre fica abrigado na sede da fazenda com acesso a fontes de energia mais robustas e conectividade com a internet.
- Microcontrolador: Baseado no ESP32, escolhido por seu processador robusto de 32 bits, conectividade Wi-Fi/Bluetooth integrada e maior capacidade de memória RAM para gerenciar dezenas de nós no campo.
- Processamento e Inteligência (NPK por IA): Ao receber o pacote de dados brutos (CE, umidade e temperatura) dos nós escravos via LoRa, o ESP32 atua como porta de entrada para algoritmos de Inteligência Artificial (como Florestas Aleatórias/Random Forest). Modelos preditivos correlacionam a medição física precisa do sensor de solo com análises laboratoriais (ground truth), permitindo predizer as concentrações reais de NPK com exatidão que pode chegar a 92%, solucionando a limitação do hardware de baixo custo.
- Interface: Após o processamento, o ESP32 empacota os resultados finais em formato JSON e os encaminha para plataformas em nuvem via Wi-Fi para visualização remota e emissão de alertas.

3. Invólucro e Impermeabilização (Nível IP68)A sobrevivência em campo aberto, submetido a agrotóxicos, variações térmicas violentas e submersão em poças exige impermeabilização absoluta de nível IP68.


#### Papers Pesquisados
	[Detection of air temperature, humidity and soil pH by using DHT22 and pH sensor based Arduino nano microcontroller](https://pubs.aip.org/aip/acp/article/2221/1/100008/687623/Detection-of-air-temperature-humidity-and-soil-pH)
	
	[Evaluating the Effectiveness of a Solar-Powered Arduino Real-Time Transmitter in Measuring Moisture, Temperature, and Environmental pH Levels in Soil](https://animorepository.dlsu.edu.ph/conf_shsrescon/2025/paper_csr/7/)
	
#### Peças Pesquisadas
	Sensor de solo RS485 Modbus 7 em 1 mede umidade do solo, temperatura, umidade, EC PH NPK