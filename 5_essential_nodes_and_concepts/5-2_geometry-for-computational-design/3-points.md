# Pontos

## Pontos no Dynamo

### O que é um ponto?

Um [ponto](3-points.md#deep-dive-into...) é definido por nada mais que um ou mais valores chamados coordenadas. A quantidade de valores de coordenadas que precisamos para definir o ponto depende do sistema de coordenadas ou do contexto em que ele se encontra.

### Ponto 2D/3D

O tipo mais comum de ponto no Dynamo existe em nosso Sistema de coordenadas universais tridimensional e tem três coordenadas [X,Y,Z] (Ponto 3D no Dynamo).

![](../images/5-2/3/points-3dpointindynamo.jpg)

Um ponto 2D no Dynamo tem duas coordenadas [X,Y].

![](../images/5-2/3/points-2dpointindynamo.jpg)

### Ponto em curvas e superfícies

Os parâmetros para curvas e superfícies são contínuos e se estendem além da aresta da geometria fornecida. Como as formas que definem o espaço paramétrico residem em um Sistema de coordenadas universais tridimensional, sempre podemos converter uma coordenada paramétrica em uma coordenada “Universal”. O ponto [0,2; 0,5] na superfície, por exemplo, é o mesmo que o ponto [1,8; 2,0; 4,1] nas coordenadas universais.

![](../images/5-2/3/points-xyzvscoordsysvsuv.jpg)

> 1. Ponto em coordenadas XYZ universais assumidas
> 2. Ponto relativo a um determinado sistema de coordenadas (cilíndrico)
> 3. Ponto como coordenada UV em uma superfície

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-2/3/Geometry for Computational Design - Points.dyn" %}

## Análise abrangente de...

Se a geometria é o idioma de um modelo, então os pontos são o alfabeto. Os pontos são a fundação na qual todas as outras geometrias são criadas: precisamos de ao menos dois pontos para criar uma curva, precisamos de ao menos três pontos para criar um polígono ou uma face de malha, e assim por diante. A definição de posição, ordem e relação entre os pontos (tente uma função de seno) nos permite definir uma geometria de ordem superior como as coisas que reconhecemos como círculos ou curvas.

![Ponto para curva](../images/5-2/3/PointsAsBuildingBlocks-1.jpg)

> 1. Um círculo que usa as funções `x=r*cos(t)` e `y=r*sin(t)`
> 2. Uma curva senoidal que usa as funções `x=(t)` e `y=r*sin(t)`

### Ponto como Coordenadas

Os pontos também podem existir em um sistema de coordenadas bidimensional. A convenção tem uma notação de letra diferente dependendo do tipo de espaço com que estamos trabalhando: podemos usar [X,Y] em um plano ou [U,V] se estivermos em uma superfície.

![Ponto como coordenadas](../images/5-2/3/Coordinates.jpg)

> 1. Um ponto no Sistema de coordenadas euclidianas: [X,Y,Z]
> 2. Um ponto em um sistema de coordenadas de parâmetro de curva: [t]
> 3. Um ponto em um sistema de coordenadas de parâmetro de superfície: [U,V]
