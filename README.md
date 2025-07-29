# Kaminari  <img src="https://d25lcipzij17d.cloudfront.net/badge.svg?id=gh&type=6&v=1.0&x2=0">
Universidade Federal de Itajubá – UNIFEI

<p align= "left">
<img src="./media/image1.png" style="width:2.5in;height:3in"
alt="A yellow and black logo Description automatically generated with low confidence" />

### Build Report – Kaminari

Eric Makiya Lazanha e Marcos André M. S. Filho

Itajubá

2023

## Resume

Kaminari is a redesigned control board developed for mini-sumo robots, transitioning from an Arduino-based system to an STM32F405RGT microcontroller. This redesign was driven by the need for more compact and modular boards for the category.

In addition to being more compact, the board is highly flexible, as the control board is separated from the motor drivers and connected via flat cables. This modular design allows for different driver models to be used, enabling the team to redesign either the control board or the drivers independently, depending on the need.

The board also supports 4S batteries (16.8V), which was not possible with the previously used power board based on the TB6612FNGC8EL.

The design was inspired by the PlayStation Vita (PSVita), which features a main board and two secondary boards arranged horizontally and interconnected by flexible cables.

<p align= "center">
<img src="./media/image2.png" style="width:5.21871in;height:4.57243in"
alt="A picture containing items Description automatically generated" />
<p align= "center">
Figure 1 – Internal layout of a disassembled PSVita, showing the separation between the mainboard and secondary boards.

[1. Schematic](#schematic)

[1.1 Control Board](#control-board)

[1.1.1 5V Regulator](#5v-regulator)  
[1.1.2 3.3V Regulator](#33v-regulator)  
[1.1.3 Logic Circuit](#logic-circuit)  
[1.1.4 Peripherals](#peripherals)  
[1.1.5 Bluetooth](#bluetooth)  
[1.1.6 Microstarter](#microstarter)  
[1.1.7 Strategy Switch](#strategy-switch)  
[1.1.8 Secondary Power Supply](#secondary-power-supply)  
[1.1.9 Battery Meter](#battery-meter)  
[1.1.10 Debug LED](#debug-led)  
[1.1.11 Infrared Sensor](#infrared-sensor)  
[1.1.12 Flag](#flag)  
[1.1.13 Driver Connector](#driver-connector)  
[1.1.14 Driver](#driver)  
[1.1.15 Left DRV8871](#left-drv8871)  
[1.1.16 L9958SBTR](#l9958sbtr)

[2. Layouts](#layouts)

[2.1 Control Board Layout](#control-board-layout)  
[2.1.1 5V Power Supply Layout with Switch](#5v-power-supply-layout-with-switch)  
[2.1.2 3.3V Power Supply Layout](#33v-power-supply-layout)

[2.2 Auxiliary Boards Layout](#auxiliary-boards-layout)  
[2.2.1 Infrared Sensors Layout](#infrared-sensors-layout)  
[2.2.2 Microstarter Layout](#microstarter-layout)  
[2.2.3 Flag Servo Layout](#flag-servo-layout)  
[2.2.4 STM Loader Layout](#stm-loader-layout)  
[2.2.5 Strategy Switch Layout](#strategy-switch-layout)  
[2.2.6 Debug LEDs and Bluetooth Layout](#debug-leds-and-bluetooth-layout)  
[2.2.7 Control Board Layout Summary](#control-board-layout-summary)

[2.3 Driver Layouts](#driver-layouts)  
[2.3.1 DRV8871 Driver Layout](#drv8871-driver-layout)  
[2.3.2 L9958SBTR Layout](#l9958sbtr-layout)

[3. Conclusion](#conclusion)

# Schematic

## Control Board

### 5V Regulator

To step down the voltage from 12.6V (3S battery voltage) to 5V, a linear power supply was used with two voltage regulators (L7805CD2T-TR) connected in parallel, in order to prevent overheating issues and improve heat dissipation.

<p align= "center">
<img src="./media/image3.png" style="width:5.82947in;height:4.88662in"
alt="Diagram, schematic Description automatically generated" />
<p align= "center">
Figure 2 – 5V Linear Regulator

In addition to the voltage regulators, there is also a switch (JS202011SCQN) and a MOSFET (FDS6576) that allow the board to be powered off without disconnecting the battery. The MOSFET also serves as reverse polarity protection. A debug LED is also included in the circuit.

### 3.3V Regulator

<p align= "center">
<img src="./media/image4.png" style="width:5.11613in;height:1.73583in"
alt="A picture containing text, indoor, tiled Description automatically generated" />
<p align= "center">
Figure 3 – 3.3V Linear Regulator

To step down from 5V to 3.3V, a linear regulator (LDL1117S33R) was used. The design was inspired by the 3.3V regulator from the Raijin board, which had already been validated.

### Logic Circuit

<p align= "center">
<img src="./media/image5.png" style="width:7.34727in;height:4.15187in"
alt="A picture containing timeline Description automatically generated" />
<p align= "center">
Figure 4 – Microcontroller Schematic

The choice of this microcontroller was crucial for the overall development of the autonomous robots. In our category — line follower — we selected the same microcontroller, but in a 64-pin package, which increases code compatibility and facilitates collaboration between both categories. The line follower robots require more components than mini sumo robots, making this choice even more appropriate.

The decision to use an ST microcontroller was made jointly with members of the line follower team, aiming to foster greater interaction within the autonomous category. This also allowed us to standardize key aspects of the robots, such as motors and sensors, thus promoting mutual development between the categories.
Para a programação deste micro é utilizado um barramento de *headers*
que, por sua vez vão conectados a um STlink que nos permite a conexão
dele a porta USB dos computadores

<p align= "center">
<img src="./media/image6.png" style="width:2.84894in;height:2.23522in"
alt="A picture containing calendar Description automatically generated" />
<p align= "center">
Figura 5 STM bootloader 

<p align= "center">
<img src="./media/image7.png" style="width:2.42886in;height:2.16581in"
alt="Text, whiteboard Description automatically generated" />
<p align= "center">
Figura 6 STlink

### Peripherals 

Agora por fim dos esquemáticos passamos as placas que ajudam as decisões
que o robô necessita tomar.

Para barramentos de 3 pinos uma forma de proteção que adotamos e
recomendamos fortemente para todos os projetos é colocar pino do meio
como GND.

<p align= "center">
<img src="./media/image8.png" style="width:4.02119in;height:2.50721in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 7 Antes de aderir forma de proteção

<p align= "center">
<img src="./media/image9.png" style="width:4.40383in;height:2.13379in"
alt="Chart Description automatically generated with low confidence" />
<p align= "center">
Figura 8 Depois de aderir forma de proteção

Por que fazer essa alteração?

Imagine que você está em um pré-guerra faltando uma semana para a
competição, tentando conciliar a vida de universitário da UNIFEI e a
vida de um membro da equipe UAI!RRIOR, em um momento de cansaço e você
poderia acabar fechando um curto GND VCC com o modelo antigo, queimando
seus componentes e tendo que soldar outra placa (em um cenário bom onde
tem componentes reserva). No entanto, se você colocar o GND no meio você
apenas vai trocar o pino do sinal com o VCC, dessa forma sua única
preocupação vai ser trocar os dois de lugar.

Além disso, é de suma importância posicionar os capacitores de
desacoplamento e o resistor de *boot* próximos aos seus pinos, com o
intuito de evitar perdas como ocorreu em projetos passados.

<p align= "center">
<img src="./media/image10.png" style="width:4.71875in;height:4.5in"
alt="A screenshot of a computer Description automatically generated with medium confidence" />
<p align= "center">
Figura 9 Resistor de boot e capacitores de desacoplamento

### Bluetooth

<p align= "center">
<img src="./media/image11.png" style="width:3.88542in;height:2.20833in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 10 Bluetooth

Facilitar e profissionalizar as escolhas e ajustes das estratégias por
meio de um aplicativo, também foi implementado no *follow line*, para
não ser necessário o uso dos computadores ao levar os sumos para
temporização e o *follow line* para ajustes na pista.

### *Microstarter*

<p align= "center">
<img src="./media/image12.png" style="width:4.07292in;height:2.08333in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 11 Micro starter

O *microstarter* é uma placa comprada e sua função é receber o sinal um
infravermelho e decodificá-lo, enviando essa informação para o micro.
Portanto, seu objetivo é receber o sinal enviado pelo controle do juiz
para tomar uma decisão podendo ser inicializar, parar ou paridade (serve
para verificar se o robô realmente está recebendo um sinal).

###  *Strategy Switch*

<p align= "center">
<img src="./media/image13.png" style="width:6.5in;height:3.1625in"
alt="Diagram, schematic Description automatically generated" />
<p align= "center">
Figura 12 Switches de estratégias

Caso tenha algum problema com *bluetooth* pode ser utilizado *switches*
de estratégias para escolher a estratégia manualmente por meio de uma
placa separada que se encaixa na placa de controle (circulada de
vermelho na imagem a seguir).

<p align= "center">
<img src="./media/image14.png" style="width:5.92091in;height:3.30711in"
alt="A picture containing text, electronics, circuit Description automatically generated" />
<p align= "center">
Figura 13 Switch estratégia na placa de controle

Essa placa tem a capacidade de escolher até 32 estratégias por meio de 5
*switches* que funcionam por meio do conceito de números binários. Onde
a posição do *switch* define se ele representa 0 ou 1.

<p align= "center">
<img src="./media/image15.jfif" style="width:1.58819in;height:2.58056in"
alt="Table Description automatically generated with medium confidence" />
<p align= "center">
Figura 14 Placa dos switches

<p align= "center">
<img src="./media/image16.png" style="width:5.2785in;height:1.49333in"
alt="Graphical user interface, application Description automatically generated" />
<p align= "center">
Figura 15 Decimal para binário

<p align= "center">
<img src="./media/image17.png" style="width:6.5in;height:1.69167in"
alt="Scatter chart Description automatically generated with low confidence" />
<p align= "center">
Figura 16 Esquemático switches de estratégias

Uma placa bem simples que poderá ser conectada por *headers* com a placa
de controle.

### Secondary Power Supply

<p align= "center">
<img src="./media/image18.png" style="width:5.40564in;height:2.52956in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 17 Alimentação secundária

Utilizada para alimentar diretamente a placa de controle, sem
necessidade de acoplar os drivers. A não precisa ser alimentado com 5V,
pois ainda passará pelos reguladores de tensão.

### Battery Meter

<p align= "center">
<img src="./media/image19.png" style="width:3.57123in;height:2.75664in"
alt="Diagram, schematic Description automatically generated" />
<p align= "center">
Figura 18 Medidor de bateria

É utilizado um divisor de tensão para medir a tensão na bateria que vai
para uma entrada ADC.

Exemplo para uma bateria 3S (12.6V):

- A corrente vai ser 12,6 /13k3 = 0,95mA.

- A tensão no resistor q vai para o micro vai ser 3k3\*0,95m = 3,13 V.

- Com a tensão da bateria em 11,4 por exemplo temos 11,4/13k3 = 0,86mA.

- A tensão no resistor ia ser 3k3\*0,86m = 2,84 V.

Podemos usar 3.13V como 255 e 0V como 0 através da programação. Assim,

a partir de um certo ponto pode-se programar os LED´s para sinalizar de
alguma forma que a bateria deve ser trocada.

Vale ressaltar que a entrada ADC (*Analog-to-digital converter*)
suportam apenas 3.3V na entrada de alimentação conforme o *datasheet*,
por isso foi necessário diminuir a tensão. O capacitor só serve para
manter o sinal estável (capacitor de desacoplamento).

<p align= "center">
<img src="./media/image20.png" style="width:6.56801in;height:3.74543in"
alt="Table Description automatically generated" />
<p align= "center">
Figura 19 Datasheet tensão máxima na entrada do micro

Também é possível utilizar uma bateria 4S (16.8V), porém é necessário
trocar o resistor de 3k3 ohms para 2k2 ohms para tensão de entrada no
micro ser menor que 3.3V. E seria preciso alterar o cálculo da bateria
na programação também.

### Debug LED

<p align= "center">
<img src="./media/image21.png" style="width:7.41145in;height:1.20921in"
alt="A picture containing timeline Description automatically generated" />
<p align= "center">
Figura 20 LED debug

Para facilitar a utilização e de codagem foram colocados 4 LED´s de
debug, que ajuda aquele que estiver programando para solucionar
possíveis erros de código.

### Infrared Sensor

<p align= "center">
<img src="./media/image22.png" style="width:6.43367in;height:2.52591in"
alt="Timeline Description automatically generated" />
<p align= "center">
Figura 21 Sensores infravermelho

Possui disponibilidade para 7 sensores, sendo 5 digitais e 2 analógicos.
É de extrema importância que os sensores JS40F e EM3 sejam alimentados
por 5V para sensor ler com a capacidade total da distância que é
informada pelo fabricante.

Foi necessário fazer um divisor de tensão diminuindo a alimentação nas
entradas ADC (*Analog-to-digital converter*) de 5V para 3.3V, pois a
entrada ADC do micro aguenta no máximo 3.3V, da mesma maneira que foi
comentada no medidor de bateria. Porém, a alimentação não vem
diretamente da bateria, ela passa pelo regulador de tensão que regula
para 5V.

<p align= "center">
<img src="./media/image23.png" style="width:4.44601in;height:3.66388in"
alt="Timeline Description automatically generated with medium confidence" />
<p align= "center">
Figura 22 Divisor de tensão sensor analógico

### Flag

<p align= "center">
<img src="./media/image24.png" style="width:4.38887in;height:3.04648in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 23 Servo para flag

Servo é específico para robôs que usam *flag*, que tem como objetivo
“enganar” o sensor do adversário.

### Driver Connector

<p align= "center">
<img src="./media/image25.png" style="width:5.12005in;height:3.1322in"
alt="Diagram, schematic Description automatically generated" />
<p align= "center">
Figura 24 Esquemático driver

Houve uma troca dos *headers* que eram usados em projetos anteriores
para os conectores devido a mau contato e eles saiam frequentemente.

É importante ter muito cuidado na hora de projetar os esquemáticos dos
drivers, o lado direito deve ser feito invertido para encaixar
corretamente na placa, dessa forma o projeto dos drivers se torna um
projeto de certa forma separado, possibilitando a projeção de diversos
drivers e juntar com a placa de controle por meio de dois conectores
(505278-1033) e dois cabos *flat*. Posicionados conforme ilustrado na
imagem a seguir:

<p align= "center">
<img src="./media/image26.png" style="width:6.53624in;height:3.18851in"
alt="A picture containing text, electronics, circuit Description automatically generated" />
<p align= "center">
Figura 25 Conector cabo flat

### Driver

Foram projetados 2 modelos de drivers diferentes (DRV8871 e L9958SBTR)
para serem utilizados nessa placa, essa escolha foi feita para caso
houvesse algum problema com um dos drivers ainda seria possível validar
a placa caso o problema não seja a placa de controle. Ademais, isso
permite uma grande flexibilidade nas escolhas da equipe.

### 

### Left DRV8871

<p align= "center">
<img src="./media/image27.png" style="width:6.66596in;height:4.71175in"
alt="A picture containing diagram Description automatically generated" />
<p align= "center">
Figura 26 Esquemático DRV8871

Esse foi um driver recomendado pela Trincabotz e já tinha
disponibilidade na salinha devido ao seu uso no *follow line.* Vale
ressaltar que os esquemáticos são iguais, mudando apenas a parte de
controle.

O driver funcionou conforme o esperado.

<p align= "center">
<img src="./media/image28.png" style="width:2.49803in;height:2.50987in"
alt="Diagram Description automatically generated with medium confidence" />
<p align= "center">
Figura 27 Conector DRV8871 esquerdo 

<p align= "center">
<img src="./media/image29.png" style="width:2.63019in;height:2.42349in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 28 Conector DRV8871 direito

Os conectores estão espelhados conforme o encaixe na placa de controle.

### L9958SBTR

<p align= "center">
<img src="./media/image30.png" style="width:5.6597in;height:3.11405in"
alt="Diagram, schematic Description automatically generated" />
<p align= "center">
Figura 29 Esquemático L9958SBTR

Esse driver foi escolhido por já ter sido utilizado em outros projetos
da equipe, além de estar disponível na salinha.

No primeiro momento a placa não funcionou corretamente, então fechamos
curtos nos resistores circulado abaixo e funcionou perfeitamente. Porém,
esse mau funcionamento pode ter sido um problema de programação

<p align= "center">
<img src="./media/image31.png" style="width:1.45755in;height:3.00645in"
alt="Icon Description automatically generated" />
<p align= "center">
Figura 30 3D L9958SBTR direito 

<p align= "center">
<img src="./media/image32.png" style="width:1.47257in;height:3.0048in"
alt="Graphical user interface Description automatically generated" />
<p align= "center">
Figura 31 3D L9958SBTR esquerdo

<p align= "center">
<img src="./media/image33.png" style="width:2.60241in;height:2.34782in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 34 Exemplo PCB da Kaminari

<p align= "center">
<img src="./media/image34.png" style="width:2.58535in;height:2.37852in"
alt="Diagram, schematic Description automatically generated" />
<p align= "center">
Figura 33 Conector L9958SBTR direito

> Os conectores estão espelhados conforme o encaixe na placa de
> controle.

# *Layouts*

O *Layou*t de uma PCB, nada mais é que sua forma final de como irá ficar
traçado suas conexões e posicionamento dos componentes, além da
definição de seu tamanho.

<p align= "center">
<img src="./media/image35.png" style="width:6.5in;height:3.45139in"
alt="A picture containing text, electronics, circuit Description automatically generated" />
<p align= "center">
Figura 35 Exemplo PCB da Kaminari

## Control Board Layout

### 5V Power Supply Layout with Switch

<p align= "center">
<img src="./media/image36.png" style="width:6.01395in;height:2.68254in"
alt="A picture containing graphical user interface Description automatically generated" />
<p align= "center">
Figura 35 3D switch fonte 5V linear

<p align= "center">
<img src="./media/image37.png" style="width:5.70833in;height:2.86458in"
alt="Text Description automatically generated with medium confidence" />
<p align= "center">
Figura 36 Switch fonte 5V linear

O *switch* deve ficar na *top layer* para ser acessível para ligar e
desligar a placa facilmente.

### 5V Power Supply Layout

<p align= "center">
<img src="./media/image38.png" style="width:6.5in;height:2.91181in"
alt="Graphical user interface Description automatically generated" />
<p align= "center">
Figura 37 3D fonte 5V linear

<p align= "center">
<img src="./media/image39.png" style="width:6.53125in;height:3.18958in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 38 Fonte linear 5V

É encontrada na *bottom layer*, quando posicionamos componentes na PCB
sempre devemos colocar os componentes que são ligados entre si o mais
próximo possível para evitar fazer muitas trilhas desnecessárias.

### 3.3V Power Supply Layout

<p align= "center">
<img src="./media/image40.png"
style="width:3.63007in;height:2.85863in" />
<p align= "center">
Figura 39 3D fonte 3.3V

<p align= "center">
<img src="./media/image41.png" style="width:3.48958in;height:2.98958in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 40 Fonte linear 3.3V

Idealmente utilizamos a *bottom layer* para passar um plano GND extenso,
porém não foi possível nesse caso devido a grande quantidade de
componentes e ao espaço limitado da placa.

<p align= "center">
<img src="./media/image42.png" style="width:6.79335in;height:3.13757in"
alt="A picture containing text, electronics, circuit Description automatically generated" />
<p align= "center">
Figura 41 3D da bottom layer

<p align= "center">
<img src="./media/image43.png" style="width:7.13731in;height:3.43588in"
alt="A screenshot of a computer Description automatically generated with medium confidence" />
<p align= "center">
Figura 42 Bottom Layer

Nesse caso podemos ver que a *bottom layer* foi basicamente dedicada a
parte da fonte e tentamos priorizar ao máximo o plano GND.

## Auxiliary Boards Layout

### Infrared Sensors Layout

<p align= "center">
<img src="./media/image44.png" style="width:6.5in;height:2.26875in"
alt="A picture containing text, electronics, vending machine Description automatically generated" />
<p align= "center">
Figura 43 3D sensores infravermelhos

Os 5 *headers* circulados em vermelho são os sensores infravermelhos
digitais e os dois circulados em azul são os sensores analógicos, é
muito importante que os sensores fiquem todos do mesmo lado e o mais
próximo possível para deixar a placa mais organizada.

<p align= "center">
<img src="./media/image45.png" style="width:6.5in;height:3.9in"
alt="Graphical user interface, application Description automatically generated" />
<p align= "center">
Figura 44 Trilhas sensores

### Microstarter Layout

<p align= "center">
<img src="./media/image46.png" style="width:4.32212in;height:3.23651in"
alt="A screenshot of a computer Description automatically generated with low confidence" />
<p align= "center">
Figura 45 3D microstarter

<p align= "center">
<img src="./media/image47.png" style="width:4.84941in;height:3.12984in"
alt="A screenshot of a game Description automatically generated with medium confidence" />
<p align= "center">
Figura 46 Trilha microstarter

### Flag Servo Layout

<p align= "center">
<img src="./media/image44.png" style="width:6.02363in;height:2.10248in"
alt="A picture containing text, electronics, vending machine Description automatically generated" />
<p align= "center">
Figura 47 3D servo flag

<p align= "center">
<img src="./media/image48.png" style="width:2.54719in;height:4.97874in"
alt="Diagram, schematic Description automatically generated" />
<p align= "center">
Figura 48 Trilha servo flag

### STM Loader Layout

<p align= "center">
<img src="./media/image49.png" style="width:6.04295in;height:2.60054in"
alt="Graphical user interface Description automatically generated with low confidence" />
<p align= "center">
Figura 49 3D STM loader

<p align= "center">
<img src="./media/image50.png" style="width:5.65298in;height:3.65149in"
alt="A screenshot of a map Description automatically generated with medium confidence" />
<p align= "center">
Figura 50 Trilha STM loader

Levar em consideração que será preciso inserir um *STlink* que será
conectado na entrada USB do seu computador. Então, deixá-lo em um canto
da placa de fácil acesso.

### Strategy Switch Layout

<p align= "center">
<img src="./media/image51.png" style="width:4.54069in;height:3.19268in"
alt="A screenshot of a video game Description automatically generated with medium confidence" />
<p align= "center">
Figura 51 3D switches placa de controle

<p align= "center">
<img src="./media/image52.png" style="width:4.83345in;height:3.0178in"
alt="Diagram Description automatically generated" />
<p align= "center">
Figura 52 Trilha switches placa de controle

Lembrar de deixar longe dos conectores para facilitar o uso, pois se for
necessário utilizar será preciso encaixar a placa com os *switches.*

<p align= "center">
<img src="./media/image53.png" style="width:6.89794in;height:1.96916in"
alt="Graphical user interface, application Description automatically generated" />
<p align= "center">
Figura 53 Placa de switches de estratégias

<p align= "center">
<img src="./media/image54.png" style="width:6.53125in;height:1.86111in"
alt="A screenshot of a computer Description automatically generated with medium confidence" />
<p align= "center">
Figura 54 Trilha placa de switches de estratégias

Um dos *headers* será substituído por um header fêmea para o encaixe.

### Debug LEDs and Bluetooth Layout

<p align= "center">
<img src="./media/image55.png" style="width:4.93253in;height:4.06658in"
alt="A picture containing text, electronics Description automatically generated" />
<p align= "center">
Figura 55 3D Leds debug e bluetooth

O ideal seria o módulo *bluetooth* não ficar muito perto dos leds de
debug para facilitar a visualização dos LED´s, porém devido a limitação
de espaço foi necessário esse posicionamento.

### Control Board Layout Summary

<p align= "center">
<img src="./media/image56.png" style="width:6.59712in;height:3.30349in"
alt="A picture containing text, electronics, circuit Description automatically generated" />
<p align= "center">
Figura 56 3D placa de controle top layer

<p align= "center">
<img src="./media/image57.png" style="width:6.63869in;height:3.0399in"
alt="A close-up of a circuit board Description automatically generated with medium confidence" />
<p align= "center">
Figura 57 Placa de controle bottom layer

<p align= "center">
<img src="./media/image58.png" style="width:6.93983in;height:3.1941in"
alt="A screenshot of a computer Description automatically generated with medium confidence" />
<p align= "center">
Figura 58 Trilhas e planos da placa de controle

É uma placa bem compacta onde organização é extremamente importante para
facilitar manutenção, não foi possível usar uma *layer* só para o plano
GND. Foi utilizada uma *layer* só para a fonte de 5V e 3.3V, mas deu
para conciliar bem com o plano GND.

Sempre priorizar os planos GND e de alimentação (principalmente GND),
para ter um referencial bem presente tornando a placa mais estável e
dissipando melhor o calor. As trilhas de sinais devem ser o mais curtas
possíveis e evitando ao máximo o uso de vias para trilhas, devido ao
fato de ocupar o espaço de um possível plano sólido GND ou alimentação.

## Driver Layouts

> Devemos priorizar o plano VCC e GND e é importante se atentar no lado
> em que o cabo flat está saindo e não colocar componentes em seu
> caminho.
>
> Exemplo: caso esteja projetando o driver esquerdo coloque o conector
> no canto direito para ficar mais fácil de conectar o cabo flat e
> ganhar mais espaço na placa.

### DRV8871 Driver Layout

<p align= "center">
<img src="./media/image59.png" style="width:2.15884in;height:3.72933in"
alt="Graphical user interface Description automatically generated with medium confidence" />
<p align= "center">
Figura 59 3D DRV8871 esquerdo Figura 

<p align= "center">
<img src="./media/image60.png" style="width:2.21202in;height:3.70662in"
alt="Diagram Description automatically generated" />
<p align= "center">
60 3D DRV8871 direito

<p align= "center">
<img src="./media/image61.png" style="width:2.13528in;height:3.6282in"
alt="Application Description automatically generated with low confidence" />
<p align= "center">
Figura 61 3D DRV8871 esquerdo 

<p align= "center">
<img src="./media/image62.png" style="width:2.12982in;height:3.64055in"
alt="A screenshot of a cell phone Description automatically generated with medium confidence" />
<p align= "center">
Figura 62 3D DRV8871 direito

###  L9958SBTR Layout 

<p align= "center">
<img src="./media/image63.png" style="width:1.83015in;height:3.72408in"
alt="Graphical user interface Description automatically generated with medium confidence" />
<p align= "center">
Figura 63 3D L9958SBTR esquerdo Figura

<p align= "center">
<img src="./media/image64.png" style="width:1.94271in;height:3.90396in"
alt="A picture containing diagram Description automatically generated" />
<p align= "center">
 64 3D L9958SBTR direito

<p align= "center">
<img src="./media/image31.png" style="width:1.63841in;height:3.37951in"
alt="Icon Description automatically generated" />
<p align= "center">
Figura 653D L9958SBTR esquerdo   

<p align= "center">
<img src="./media/image32.png" style="width:1.65133in;height:3.36955in"
alt="Graphical user interface Description automatically generated" />
<p align= "center">
Figura 66 3D L9958SBTR direito

# Conclusion

Obteve um resultado muito satisfatório, uma placa compacta e
extremamente flexível. Resolveu o problema da placa anterior (Raijin)
que era uma placa muito grande para os mini sumos e trouxe algumas
melhorias como o medidor de bateria.

Porém a equipe ainda possui algumas dificuldades para soldar os
conectores que são utilizados para fazer a conexão dos drivers com a
placa de controle por meio de cabos flat, sendo interessante a pesquisa
de outros meios para fazer a conexão entre os drivers e a placa de
controle.
