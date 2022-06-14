# Criar um nó personalizado

O Dynamo oferece vários métodos diferentes para criar nós personalizados. É possível criar nós personalizados do zero, com base em um gráfico existente ou explicitamente em C#. Nesta seção, vamos abordar a criação de um nó personalizado na IU do Dynamo com base em um gráfico existente. Este método é ideal para limpar o espaço de trabalho, bem como para empacotar uma sequência de nós para reutilização em outro lugar.

## Exercício: Nós personalizados para mapeamento de UV

### Parte I: Começar com um gráfico

Na imagem abaixo, é mapeado um ponto de uma superfície para outra usando as coordenadas UV. Usaremos esse conceito para criar uma superfície de painel que faz referência a curvas no plano XY. Criaremos painéis quadrados aqui, mas usando a mesma lógica, podemos criar uma ampla variedade de painéis com o mapeamento UV. Esta é uma ótima oportunidade para o desenvolvimento de nós personalizados, pois poderemos repetir um processo semelhante mais facilmente neste gráfico ou em outros fluxos de trabalho do Dynamo.

![](<../images/6-1/2/custom node for uv mapping pt I - 01.jpg>)

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/6-1/2/UV-CustomNode.zip" %}

Vamos começar criando um gráfico que desejamos aninhar em um nó personalizado. Neste exemplo, vamos criar um gráfico que mapeia os polígonos de uma superfície base para uma superfície alvo usando as coordenadas UV. Esse processo de mapeamento UV é algo que usamos com frequência, o que faz dele um bom candidato para um nó personalizado. Para obter mais informações sobre as superfícies e o espaço UV, consulte a página [Superfície](../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/5-surfaces.md). O gráfico completo é _UVmapping\_Custom-Node.dyn_ do arquivo .zip obtido por download acima.

![](<../images/6-1/2/custom node for uv mapping pt I - 02.jpg>)

> 1. **Bloco de código:** use essa linha para criar um intervalo de 10 números entre -45 e 45 `45..45..#10;`
> 2. **Point.ByCoordinates:** conecte a saída do **Bloco de código** às entradas “x” e “y” e defina a amarra como referência cruzada. Você agora deve ter uma grade de pontos.
> 3. **Plane.ByOriginNormal:** conecte a saída _“Point”_ à entrada _“origin”_ para criar um plano em cada um dos pontos. Será usado o vetor normal padrão de (0,0,1).
> 4. **Rectangle.ByWidthLength:** conecte os planos da etapa anterior à entrada _“plane”_ e use um **Bloco de código** com um valor de _10_ para especificar a largura e o comprimento.

Você agora deve ver uma grade de retângulos. Vamos mapear esses retângulos para uma superfície alvo usando as coordenadas UV.

![](<../images/6-1/2/custom node for uv mapping pt I - 03.jpg>)

> 1. **Polygon.Points:** conecte a saída de **Rectangle.ByWidthLength** da etapa anterior à entrada _“polygon”_ para extrair os pontos de canto de cada retângulo. Esses são os pontos que serão mapeados para a superfície alvo.
> 2. **Rectangle.ByWidthLength:** use um **Bloco de código** com um valor de _100_ para especificar a largura e o comprimento de um retângulo. Esse será o limite da nossa superfície base.
> 3. **Surface.ByPatch:** conecte o **Rectangle.ByWidthLength** da etapa anterior à entrada _“closedCurve”_ para criar uma superfície base.
> 4. **Surface.UVParameterAtPoint:** conecte a saída _“Point”_ do nó **Polygon.Points** e a saída _“Surface”_ do nó **Surface.ByPatch** para retornar o parâmetro UV em cada ponto.

Agora que temos uma superfície base e um conjunto de coordenadas UV, é possível importar uma superfície alvo e mapear os pontos entre as superfícies.

![](<../images/6-1/2/custom node for uv mapping pt I - 04.jpg>)

> 1. **File Path:** selecione o caminho do arquivo da superfície que você deseja importar. O tipo de arquivo deve ser .SAT. Clique no botão _“Procurar...”_ e navegue até o arquivo _UVmapping\_srf.sat_ do arquivo .zip obtido por download acima.
> 2. **Geometry.ImportFromSAT:** conecte o caminho do arquivo para importar a superfície. Você deve ver a superfície importada na visualização da geometria.
> 3. **UV:** conecte a saída do parâmetro UV a um nó _UV.U_ e a um nó _UV.V_.
> 4. **Surface.PointAtParameter:** conecte a superfície importada, bem como as coordenadas u e v. Você agora deve visualizar uma grade de pontos 3D na superfície alvo.

A última etapa é usar os pontos 3D para construir as correções de superfície retangulares.

![](<../images/6-1/2/custom node for uv mapping pt I - 05.jpg>)

> 1. **PolyCurve.ByPoints:** conecte os pontos na superfície para construir uma policurva através dos pontos.
> 2. **Booleano:** adicione um **valor booleano** ao espaço de trabalho e conecte-o à entrada _“connectLastToFirst”_ e alterne para Verdadeiro para fechar as policurvas. Você agora deve ver os retângulos mapeados para a superfície.
> 3. **Surface.ByPatch:** conecte as policurvas à entrada _“closedCurve”_ para construir as correções de superfície.

### Parte II: Do gráfico ao nó personalizado

Agora, vamos selecionar os nós que desejamos aninhar em um nó personalizado, pensando no que desejamos que sejam as entradas e saídas do nosso nó. Desejamos que nosso nó personalizado seja o mais flexível possível, para que possa mapear qualquer polígono, não apenas retângulos.

Selecione os seguintes nós (começando com Polygon.Points), clique com o botão direito do mouse no espaço de trabalho e selecione “Criar nó personalizado”.

![](<../images/6-1/2/custom node for uv mapping pt II - 01.jpg>)

Na caixa de diálogo Propriedades do nó personalizado, atribua um nome, uma descrição e uma categoria ao nó personalizado.

![](<../images/6-1/2/custom node for uv mapping pt II - 02.jpg>)

> 1. Nome: MapPolygonsToSurface
> 2. Descrição: Mapear polígonos de uma superfície base para a superfície alvo
> 3. Categoria de complementos: Geometry.Curve

O nó personalizado limpou consideravelmente o espaço de trabalho. Observe que as entradas e saídas foram nomeadas com base nos nós originais. Vamos editar o nó personalizado para tornar os nomes mais descritivos.

![](<../images/6-1/2/custom node for uv mapping pt II - 03.jpg>)

Clique duas vezes no nó personalizado para editá-lo. Isso abrirá um espaço de trabalho com um fundo amarelo que representa o interior do nó.

![](<../images/6-1/2/custom node for uv mapping pt II - 04.jpg>)

> 1. **Entradas:** altere os nomes das entradas para _baseSurface_ e _targetSurface_.
> 2. **Saídas:** adicione mais uma saída para os polígonos mapeados.

Salve o nó personalizado e volte para o espaço de trabalho inicial. Observe que o nó **MapPolygonsToSurface** reflete as alterações que acabamos de fazer.

![](<../images/6-1/2/custom node for uv mapping pt II - 05.jpg>)

Também é possível aumentar a robustez do nó personalizado adicionando **Comentários personalizados**. Os comentários podem ajudar a indicar os tipos de entrada e saída ou explicar a funcionalidade do nó. Os comentários aparecerão quando o usuário passar o cursor sobre uma entrada ou saída de um nó personalizado.

Clique duas vezes no nó personalizado para editá-lo. Isso reabrirá o espaço de trabalho de fundo amarelo.

![](<../images/6-1/2/custom node for uv mapping pt II - 06.jpg>)

> 1. Comece a editar o **Bloco de código** de entrada. Para iniciar um comentário, digite “//” seguido do texto do comentário. Digite qualquer coisa que possa ajudar a esclarecer o nó. Aqui, descreveremos a _targetSurface_.
> 2. Também definiremos o valor padrão da _inputSurface_ definindo o tipo de entrada igual a um valor. Aqui, definiremos o valor padrão como o conjunto **Surface.ByPatch** original.

Também é possível aplicar os comentários às saídas.

![](<../images/6-1/2/custom node for uv mapping pt II - 07.jpg>)

> Edite o texto no bloco de código de saída. Digite “//” seguido do texto do comentário. Aqui, esclareceremos as saídas _Polygons_ e _surfacePatches_ adicionando uma descrição mais detalhada.

![](<../images/6-1/2/custom node for uv mapping pt II - 08.jpg>)

> 1. Passe o cursor sobre as entradas dos nós personalizados para ver os comentários.
> 2. Com o valor padrão definido em _inputSurface_, também é possível executar a definição sem uma entrada de superfície.
