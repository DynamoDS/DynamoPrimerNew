# Personalização

Apesar de anteriormente termos examinado a edição de uma massa de construção básica, desejamos aprofundar o vínculo do Dynamo/Revit editando um grande número de elementos de uma só vez. Personalizar em uma grande escala se torna mais complexo, já que as estruturas de dados requerem operações de lista mais avançadas. No entanto, os princípios subjacentes por trás de sua execução são fundamentalmente os mesmos. Vamos estudar algumas oportunidades para análise com base em um conjunto de componentes adaptativos.

### Localização do ponto

Suponha que criamos uma faixa de componentes adaptativos e que desejamos editar os parâmetros com base nas localizações dos pontos. Os pontos, por exemplo, podem determinar um parâmetro de espessura que está relacionado com a área do elemento. Ou, eles poderiam determinar um parâmetro de opacidade relacionado à exposição solar durante o ano. O Dynamo permite a conexão entre análise e parâmetros por meio de algumas etapas simples. Vamos explorar uma versão básica no exercício abaixo.

![](./images/5/customizing-pointlocation.jpg)

> Consulte os pontos adaptativos de um componente adaptativo selecionado usando o nó **AdaptiveComponent.Locations**. Isso nos permite trabalhar com uma versão abstrata de um elemento do Revit para análise.

Ao extrair a localização dos pontos dos componentes adaptativos, podemos executar uma faixa de análise para aquele elemento. Um componente adaptativo de quatro pontos permitirá estudar o desvio do plano para um determinado painel, por exemplo.

### Análise da orientação solar

![](./images/5/customizing-solarorientationanalysis.jpg)

> Use o remapeamento para mapear um conjunto de dados para um intervalo de parâmetros. Essa é uma ferramenta fundamental usada em um modelo paramétrico e vamos demonstrá-la no exercício abaixo.

Usando o Dynamo, é possível usar as localizações dos pontos dos componentes adaptativos para criar um plano de melhor ajuste para cada elemento. Também podemos consultar a posição do sol no arquivo do Revit e estudar a orientação relativa do plano em relação ao sol em comparação com outros componentes adaptativos. Vamos definir isso no exercício abaixo, criando um ambiente algorítmico.

## Exercício

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="./datasets/5/Revit-Customizing.zip" %}

Este exercício permite expandir as técnicas demonstradas na seção anterior. Neste caso, estamos definindo uma superfície paramétrica com base em elementos do Revit, instanciando componentes adaptativos de quatro pontos e, em seguida, editando-os com base na orientação em relação ao sol.

![](./images/5/customizing-exercise01.jpg)

> 1. Comece selecionando duas arestas com o nó _“Selecionar aresta”_. As duas arestas são os vãos longos do átrio.
> 2. Combine as duas arestas em uma lista com o nó _List.Create_.
> 3. Crie uma superfície entre as duas arestas com _Surface.ByLoft_.

![](./images/5/customizing-exercise02.jpg)

> 1. Usando o _bloco de código_, defina um intervalo de 0 a 1 com 10 valores uniformemente espaçados: `0..1..#10;`
> 2. Conecte o _bloco de código_ às entradas *u *e _v_ de um nó _Surface.PointAtParameter_ e conecte o nó _Surface.ByLoft_ à entrada _superfície_. Clique com o botão direito do mouse no nó e altere a _amarra_ para _Produto transversal_. Isso fornecerá uma grade de pontos na superfície.

Essa grade de pontos serve como os pontos de controle para uma superfície definida parametricamente. Queremos extrair as posições “u” e “v” de cada um desses pontos para que possamos conectá-los em uma fórmula paramétrica e manter a mesma estrutura de dados. É possível fazer isso consultando as localizações dos parâmetros dos pontos que acabamos de criar.

![](./images/5/customizing-exercise03.jpg)

> 1. Adicione um nó_Surface.ParameterAtPoint_ à tela e conecte as entradas como mostrado acima.
> 2. Consulte os valores _u_ desses parâmetros com o nó UV.U.
> 3. Consulte os valores _v_ desses parâmetros com o nó UV.V.
> 4. Os resultados mostram os valores _u_ e _v_ correspondentes a cada ponto da superfície. Agora, temos um intervalo de _0_ a _1_ para cada valor, na estrutura de dados apropriada; portanto, estamos prontos para aplicar um algoritmo paramétrico.

![](./images/5/customizing-exercise04.jpg)

> 1. Adicione um _bloco de código_ à tela e insira o código: `Math.Sin(u*180)*Math.Sin(v*180)*w;` Essa é uma função paramétrica que cria um montículo senoidal de uma superfície plana.
> 2. Conecta o _UV.U_ à entrada _u_ e o UV.V à entrada _v_.
> 3. A entrada _w_ representa a _amplitude_ da forma; portanto, anexamos um _controle deslizante de número_ a ela.

![](./images/5/customizing-exercise05.jpg)

> 1. Agora, temos uma lista de valores conforme definido pelo algoritmo. Vamos usar essa lista de valores para mover os pontos para cima na direção _+Z_. Usando _Geometry.Translate_, conecte o *bloco de código *a _zTranslation_ e _Surface.PointAtParameter_ à entrada _geometria_. Você deve ver os novos pontos exibidos na visualização do Dynamo.
> 2. Finalmente, criamos uma superfície com o nó _NurbsSurface.ByPoints_, conectando o nó da etapa anterior à entrada de pontos. Dessa forma, obtemos uma superfície paramétrica. Sinta-se à vontade para arrastar o controle deslizante para ver o encolhimento e aumento dos montículos.

Com a superfície paramétrica, queremos definir uma forma de aplicar painéis para criar matrizes em quatro pontos de componentes adaptativos. O Dynamo não tem uma funcionalidade pronta para uso para aplicar painéis à superfície; portanto, podemos consultar a comunidade para obter pacotes úteis do Dynamo.

![](./images/5/customizing-exercise06.jpg)

> 1. Vá para _Pacotes>Procurar um pacote..._
> 2. Procure _“LunchBox”_ e instale _“LunchBox for Dynamo”_. Esse pacote tem um conjunto de ferramentas muito útil para operações de geometria, como esta.

> 1. Após o fazer o download, você terá acesso completo ao conjunto LunchBox. Procure _“Quad Grid”_ e selecione _“LunchBox Quad Grid By Face”_. Conecte a superfície paramétrica à entrada _superfície_ e defina as divisões _U_ e _V_ como _15_. Você deve ver uma superfície com painéis quad na visualização do Dynamo.

> Se tiver curiosidade sobre a configuração, clique duas vezes no nó _Lunch Box_ e veja como foi feito.

> De volta ao Revit, vamos dar uma olhada rápida no componente adaptativo que estamos usando aqui. Não é necessário seguir, mas este é o painel do telhado que vamos instanciar. É um componente adaptativo de quatro pontos que é uma representação bruta de um sistema ETFE. A abertura do vazio central está em um parâmetro chamado _“ApertureRatio”_.

> 1. Estamos prestes a instanciar uma grande quantidade de geometria no Revit; portanto, certifique-se de ativar o solucionador do Dynamo na opção _“Manual”_.
> 2. Adicione um nó _Tipos de família_ à tela e selecione _“ROOF-PANEL-4PT”_.
> 3. Adicione um nó _AdaptiveComponent.ByPoints_ à tela, conecte _Pts do painel_ da saída _“LunchBox Quad Grid by Face”_ à entrada _pontos_. Conecte o nó _Tipos de família_ à entrada _familySymbol_.
> 4. Pressione _Executar_. O Revit terá que _pensar_ um pouco enquanto a geometria está sendo criada. Se demorar muito, reduza o número de _“15” do bloco de código_ para um número menor. Isso reduzirá o número de painéis no telhado.

_Observação: Se o Dynamo estiver demorando muito para calcular os nós, talvez seja recomendável usar a funcionalidade do nó “congelar” para pausar a execução das operações do Revit enquanto desenvolve o gráfico. Para obter mais informações sobre o congelamento de nós, consulte a seção “Congelar” no capítulo de sólidos._

> De volta ao Revit, temos a matriz de painéis no telhado.

> Aproximando o zoom, podemos ver mais de perto suas qualidades de superfície.

### Análise

> 1. Continuando com a etapa anterior, vamos avançar mais e determinar a abertura de cada painel com base em sua exposição solar. Aproximando o zoom no Revit e selecionando um painel, vemos que na barra de propriedades há um parâmetro chamado _“Proporção de abertura”_. A família é configurada de forma que a abertura se estenda, aproximadamente, de _0,05_ a _0,45_.

> 1. Se ativarmos o caminho solar, será possível ver a localização atual do sol no Revit.

> 1. É possível referenciar a localização do sol usando o nó _SunSettings.Current_.

1. Conecte as configurações do sol a _Sunsetting.SunDirection_ para obter o vetor solar.
2. Nos _Pts do painel_ usados para criar os componentes adaptativos, use _Plane.ByBestFitThroughPoints_ para aproximar o plano do componente.
3. Consulte a _normal_ desse plano.
4. Use o _produto escalar_ para calcular a orientação solar. O produto escalar é uma fórmula que determina como dois vetores paralelos ou antiparalelos podem ser. Então, estamos tomando a normal do plano de cada componente adaptativo e comparando com o vetor solar para simular aproximadamente a orientação solar.
5. Obtenha o _valor absoluto_ do resultado. Isso assegura que o produto escalar seja preciso se a normal do plano estiver voltada para a direção reversa.
6. Pressione _Executar_.

> 1. Observando o _produto escalar_, temos uma ampla variedade de números. Queremos usar a distribuição relativa deles, mas precisamos condensar os números dentro do intervalo apropriado do parâmetro _“Proporção de abertura”_ que planejamos editar.

1. _Math.RemapRange_ é uma ótima ferramenta para isso. Essa ferramenta analisa uma lista de entradas e remapeia seus limites em dois valores-alvo.
2. Defina os valores-alvo como _0,15_ e _0,45_ em um _bloco de código_.
3. Pressione _Executar_.

> 1. Conecte os valores remapeados a um nó _Element.SetParameterByName_.

1. Conecte a sequência de caracteres _“Proporção de abertura”_ à entrada _parameterName_.
2. Conecte os _componentes adaptativos_ à entrada _elemento_.
3. Pressione _Executar_.

> De volta ao Revit, com base em uma distância, podemos fazer o efeito da orientação solar na abertura dos painéis ETFE.

> Aproximando o zoom, vemos que os painéis ETFE estão mais fechados como a face do sol. Nosso objetivo aqui é reduzir o superaquecimento da exposição solar. Se quiséssemos deixar entrar mais luz com base na exposição solar, apenas precisaríamos alternar o domínio em _Math.RemapRange_.
