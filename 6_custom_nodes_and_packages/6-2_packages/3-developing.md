# Desenvolver um pacote

O Dynamo oferece diversas formas de criar um pacote para seu uso pessoal ou para compartilhar com a comunidade do Dynamo. No estudo de caso abaixo, vamos analisar como um pacote é configurado desconstruindo um existente. Este estudo de caso baseia-se nas lições do capítulo anterior, fornecendo um conjunto de nós personalizados para o mapeamento de geometria, por coordenadas UV, de uma superfície do Dynamo para outra.

## Pacote MapToSurface

Vamos trabalhar com um pacote de amostra que demonstra o mapeamento UV de pontos de uma superfície para outra. Já desenvolvemos os conceitos básicos da ferramenta na seção [Criar um nó personalizado](../10\_custom-nodes/10-2\_creating.md) desta introdução. Os arquivos abaixo demonstram como podemos usar o conceito de mapeamento UV e desenvolver um conjunto de ferramentas para uma biblioteca publicável.

Nessa imagem, mapeamos um ponto de uma superfície para outra usando as coordenadas UV. O pacote é baseado nesse conceito, mas com uma geometria mais complexa.

![](../images/6-2/3/uvMap.jpg)

### Instalar o pacote

No capítulo anterior, exploramos formas de criar painéis em uma superfície no Dynamo com base nas curvas definidas no plano XY. Este estudo de caso estende esses conceitos para mais cotas de geometria. Vamos instalar esse pacote conforme construído para demonstrar como ele foi desenvolvido. Na próxima seção, demonstraremos como esse pacote foi publicado.

No Dynamo, clique em _Pacotes>Procurar um pacote... e p_rocure o pacote “MapToSurface” (uma só palavra). Clique em Instalar para iniciar o download e adicionar o pacote à biblioteca.

![](../images/6-2/3/developpackage-installpackage01.jpg)

Após a instalação, os nós personalizados devem estar disponíveis na seção Complementos > Dynamo Primer.

\![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Com o pacote já instalado, vamos analisar como ele está configurado.

### Nós personalizados

O pacote que estamos criando usa cinco nós personalizados que criamos para referência. Vamos examinar o que cada nó faz abaixo. Alguns nós personalizados são criados com base em outros nós personalizados, e os gráficos têm um layout para que outros usuários compreendam de forma simples.

Esse é um pacote simples com cinco nós personalizados. Nas etapas abaixo, falaremos brevemente sobre a configuração de cada nó personalizado.

\![](<../images/6-2/3/develop package - custom nodes 01 (1) (3).jpg>)

#### **PointsToSurface**

Esse é um nó personalizado básico e no qual todos os outros nós de mapeamento estão baseados. Em termos simples, o nó mapeia um ponto de uma coordenada UV da superfície de origem para o local da coordenada UV a superfície de destino. Como os pontos são a geometria mais primitiva, a partir da qual se constrói uma geometria mais complexa, podemos usar essa lógica para mapear a geometria 2D e até mesmo a geometria 3D de uma superfície para outra.

![](../images/6-2/3/developpackage-pointToSurface.jpg)

#### **PolygonsToSurface**

A lógica de estender pontos mapeados da geometria 1D para a geometria 2D é demonstrada aqui simplesmente com polígonos. Observe que aninhamos o nó _“PointsToSurface”_ nesse nó personalizado. Dessa forma, é possível mapear os pontos de cada polígono para a superfície e, em seguida, regenerar o polígono com base nesses pontos mapeados. Mantendo a estrutura de dados apropriada (uma lista de listas de pontos), podemos manter os polígonos separados após serem reduzidos a um conjunto de pontos.

![](../images/6-2/3/developpackage-polygonsToSurface.jpg)

#### **NurbsCrvtoSurface**

A mesma lógica do nó _“PolygonsToSurface”_ se aplica aqui. Mas, em vez de mapear pontos poligonais, estamos mapeando pontos de controle de uma curva Nurbs.

![](../images/6-2/3/developpackage-nurbsCrvtoSurface.jpg)

**OffsetPointsToSurface**

Esse nó é um pouco mais complexo, mas o conceito é simples: como o nó _“PointsToSurface”_, esse nó mapeia pontos de uma superfície para outra. No entanto, ele também considera os pontos que não estão na superfície de origem, obtém sua distância até o parâmetro UV mais próximo e mapeia essa distância para a normal da superfície-alvo na coordenada UV correspondente. Isso fará mais sentido ao analisar os arquivos de exemplo.

![](../images/6-2/3/developpackage-OffsetPointsToSurface.jpg)

#### **SampleSrf**

Esse é um nó simples que cria uma superfície paramétrica para mapear da grade de origem para uma superfície ondulada nos arquivos de exemplo.

![](../images/6-2/3/developpackage-sampleSrf.jpg)

### Arquivos de exemplo

Os arquivos de exemplo podem ser encontrados na pasta raiz do pacote. Clique em Dynamo > Preferências > Gerenciador de pacotes

Ao lado de MapToSurface, clique no menu de pontos de verticais > Mostrar diretório raiz

![](../images/6-2/3/developpackage-examplefiles01.jpg)

Em seguida, abra a pasta _“extra”_, que contém todos os arquivos do pacote que não são nós personalizados. Esse é o local onde os arquivos de exemplos (se existirem) são armazenados para pacotes do Dynamo. As capturas de tela abaixo apresentam os conceitos demonstrados em cada arquivo de exemplo.

#### **01-PanelingWithPolygons**

Esse arquivo de exemplo demonstra como _“PointsToSurface”_ pode ser usado para criar painéis em uma superfície com base em uma grade de retângulos. Isso deve ser familiar, pois demonstramos um fluxo de trabalho similar no [capítulo anterior](../10\_custom-nodes/10-2\_creating.md).

![](../images/6-2/3/developpackage-samplefile01.jpg)

#### **02-PanelingWithPolygons-II**

Usando um fluxo de trabalho semelhante, esse arquivo de exercício mostra uma configuração para o mapeamento de círculos (ou polígonos que representam círculos) de uma superfície para outra. Isso usa o nó _“PolygonsToSurface”_.

![](../images/6-2/3/developpackage-samplefile02.jpg)

#### **03-NurbsCrvsAndSurface**

Esse arquivo de exemplo adiciona alguma complexidade ao trabalhar com o nó “NurbsCrvToSurface”. A superfície alvo é deslocada uma determinada distância e a curva Nurbs é mapeada para a superfície alvo original e para a superfície de deslocamento. A partir daí, as duas curvas mapeadas são elevadas para criar uma superfície, que então se torna mais espessa. Esse sólido resultante apresenta uma ondulação representativa das normais da superfície-alvo.

![](../images/6-2/3/developpackage-samplefile03.jpg)

#### **04-PleatedPolysurface-OffsetPoints**

Esse arquivo de exemplo demonstra como mapear uma PolySurface plissada de uma superfície de origem para uma superfície-alvo. A superfície de origem e a de destino são uma superfície retangular que se estende pela grade e uma superfície de revolução, respectivamente.

![](../images/6-2/3/developpackage-samplefile04a.jpg)

A PolySurface de origem é mapeada da superfície de origem para a superfície de destino.

![](../images/6-2/3/developpackage-samplefile04b.jpg)

#### **05-SVG-Import**

Como os nós personalizados são capazes de mapear diferentes tipos de curvas, esse último arquivo faz referência a um arquivo SVG exportado do Illustrator e mapeia as curvas importadas para uma superfície-alvo.

![](../images/6-2/3/developpackage-samplefile05a.jpg)

Durante a análise da sintaxe de um arquivo .svg, as curvas são convertidas do formato .xml para policurvas do Dynamo.

![](../images/6-2/3/developpackage-samplefile05b.jpg)

As curvas importadas são mapeadas para uma superfície-alvo. Isso nos permite projetar explicitamente (apontar e clicar) uma panorâmica no Illustrator, importar para o Dynamo e aplicar a uma superfície de destino.

![](../images/6-2/3/developpackage-samplefile05c.jpg)
