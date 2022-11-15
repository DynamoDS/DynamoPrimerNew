# Sintaxe do DesignScript

Você pode ter notado um tema comum nos nomes dos nós no Dynamo: cada nó usa uma sintaxe de _“.”_ sem espaços. Isso ocorre porque o texto na parte superior de cada nó representa a sintaxe real dos scripts, e _“.”_ (ou _notação de ponto_) separa um elemento dos métodos possíveis que podemos chamar. Isso cria uma conversão fácil de scripts visuais em scripts baseados em texto.

![NodeNames](../images/8-1/2/apple.jpg)

Como uma analogia geral da notação de ponto, como podemos lidar com uma maçã paramétrica no Dynamo? Abaixo estão alguns métodos que iremos executar na maçã antes de decidir comê-la. (Observação: Estes não são métodos reais do Dynamo):

| Humanamente legível                 | Notação de ponto              | Saída |
| ------------------------------ | ------------------------- | ------ |
| Qual é a cor da maçã?       | Apple.color               | vermelho    |
| A maçã está madura?             | Apple.isRipe              | true   |
| Quanto a maçã pesa? | Apple.weight              | 170 g  |
| De onde veio a maçã? | Apple.parent              | árvore   |
| O que a maçã cria?    | Apple.children            | sementes  |
| Esta maçã foi cultivada localmente?   | Apple.distanceFromOrchard | 96 km |

Não sei o que você acha, mas com base nos resultados da tabela acima, parece uma maçã saborosa. Acho que vou nomeá-la _Apple.eat()_.

### Notação de ponto no bloco de código

Com a analogia da maçã em mente, vamos examinar _Point.ByCoordinates_ e mostrar como podemos criar um ponto usando o bloco de código.

A sintaxe do _bloco de código_ `Point.ByCoordinates(0,10);` fornece o mesmo resultado que um nó _Point.ByCoordinates_ no Dynamo, exceto que podemos criar um ponto usando um nó. Isso é mais eficiente do que fazer a conexão com um nó separado em _“X”_ e _“Y”_.

![](../images/8-1/2/codeblockdotnotation.jpg)

> 1. Usando _Point.ByCoordinates_ no bloco de código, estamos especificando as entradas na mesma ordem que o nó pronto para uso _(X,Y)_.

### Nós de chamada – Criar, Ações, Consulta

É possível chamar qualquer nó normal na biblioteca através de um bloco de código, desde que o nó não seja um _nó da “IU”_ especial: aqueles com um recurso especial da interface do usuário. Por exemplo, é possível chamar _Circle.ByCenterPointRadius_, mas não faria muito sentido chamar um nó _Watch 3D_.

Os nós normais (a maioria da biblioteca) geralmente são de três tipos. Você verá que a biblioteca está organizada tendo em mente essas categorias. Os métodos ou nós desses três tipos são tratados de forma diferente quando executados dentro de um bloco de código.

![](../images/8-1/2/actioncreatequerycategory.jpg)

> 1. **Criar** – Cria (ou constrói) algo
> 2. **Ação** – Realiza uma ação sobre algo
> 3. **Consulta** – Obtém uma propriedade de algo que já existe

#### Criar

A categoria “Criar” construirá a geometria do zero. Inserimos valores no bloco de código da esquerda para a direita. Essas entradas estão na mesma ordem que as entradas no nó de cima para baixo.

Comparando o nó _Line.ByStartPointEndPoint_ e a sintaxe correspondente no bloco de código, obtemos os mesmos resultados.

![](../images/8-1/2/create.jpg)

#### Ação

Uma ação é algo que você faz em relação a um objeto desse tipo. O Dynamo usa a _notação de ponto_, comum a muitas linguagens de codificação, para aplicar uma ação a um item. Uma vez que você tenha o item, digite um ponto e, em seguida, o nome da ação. A entrada do método do tipo de ação é colocada entre parênteses como acontece nos métodos do tipo de criação, com a diferença de não ser necessário especificar a primeira entrada que você vê no nó correspondente. Em vez disso, especificamos o elemento no qual estamos executando a ação:

![](../images/8-1/2/DesignScript-action.jpg)

> 1. O nó **Point.Add** é um nó de tipo de ação, de modo que a sintaxe funciona de forma um pouco diferente.
> 2. As entradas são (1) o _ponto_ e (2) o _vetor_ a ser adicionado. Em um **Bloco de código**, nomeamos o ponto (o item) _“pt”_. Para adicionar um vetor chamado \\*“vec” \\* a _“pt”_, podemos escrever _pt.Add(vec)_ ou: item, ponto, ação. A ação Adicionar tem somente uma entrada ou todas as entradas do nó **Point.Add**, menos a primeira. A primeira entrada do nó **Point.Add** é o ponto em si.

#### Consulta

Os métodos de tipo consulta obtêm uma propriedade de um objeto. Como o próprio objeto é a entrada, não é necessário especificar nenhuma entrada. Nenhum parêntese é necessário.

![](../images/8-1/2/query.jpg)

### E a amarra?

A amarra com nós é um pouco diferente da amarra com bloco de código. Com nós, o usuário clica com o botão direito do mouse no nó e seleciona a opção de amarra a ser executada. Com o bloco de código, o usuário tem muito mais controle sobre como os dados são estruturados. O método abreviado do bloco de código usa _guias de replicação_ para definir como várias listas unidimensionais devem ser emparelhadas. Os números entre colchetes angulares “<>” definem a hierarquia da lista aninhada resultante: <1>,<2>,<3> etc.

![](../images/8-1/2/DesignScript-lacing.jpg)

> 1. Neste exemplo, usaremos uma abreviação para definir dois intervalos (obtenha mais informações sobre abreviação na seção seguinte deste capítulo). Em resumo, `0..1;` é equivalente a `{0,1}` e `-3..-7` é equivalente a `{-3,-4,-5,-6,-7}`. O resultado nos fornece listas de dois valores x e cinco valores y. Se não usarmos guias de replicação com estas listas não coincidentes, obteremos uma lista de dois pontos, que é o comprimento da lista mais curta. Usando guias de replicação, podemos encontrar todas as possíveis combinações de coordenadas 2 e 5 (ou um Produto transversal).
> 2. Usando a sintaxe **Point.ByCoordinates**`(x_vals<1>,y_vals<2>);`, obtemos _duas_ listas com _cinco_ itens em cada lista.
> 3. Usando a sintaxe **Point.ByCoordinates**`(x_vals<2>,y_vals<1>);`, obtemos _cinco_ listas com _dois_ itens em cada lista.

Com essa notação, também podemos especificar qual lista será dominante: 2 listas de 5 itens ou 5 listas de 2 itens. No exemplo, alterar a ordem dos guias de replicação resulta em uma lista de linhas de pontos ou em uma lista de colunas de pontos em um eixo.

### Nó para código

Embora os métodos de bloco de código acima possam ter algumas informações úteis, há um recurso no Dynamo chamado “Nó para código” que tornará o processo mais fácil. Para usar esse recurso, selecione uma matriz de nós no gráfico do Dynamo, clique com o botão direito do mouse na tela e selecione “Nó para código”. O Dynamo condensa esses nós em um bloco de código, com todas as entradas e saídas. Não só essa é uma ótima ferramenta para a aprendizagem do bloco de código, como também permite que você trabalhe com um gráfico do Dynamo mais eficiente e paramétrico. Concluiremos o exercício abaixo usando “Nó para código”, por isso, continue acompanhando.

![](../images/8-1/2/DesignScript-nodetocode.jpg)

## Exercício: Atração de superfície

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/8-1/2/Dynamo-Syntax_Attractor-Surface.dyn" %}

Para mostrar as capacidades do bloco de código, vamos converter uma definição de campo atrativo existente em formato de bloco de código. O trabalho com uma definição existente demonstra como o bloco de código se relaciona com os scripts visuais e é útil para aprender a sintaxe do DesignScript.

Comece recriando a definição na imagem acima (ou abrindo o arquivo de amostra).

![](../images/8-1/2/DesignScript-exercise-01.jpg)

> 1. Observe que a amarra em **Point.ByCoordinates** foi definida como _Produto transversal_.
> 2. Cada ponto em um eixo é movido para cima na direção Z com base em sua distância até o ponto de referência.
> 3. Uma superfície é recriada e espessada, criando uma convexidade na geometria relacionada à distância até o ponto de referência.

![](../images/8-1/2/DesignScript-exercise-02.jpg)

> 1. Começando do início, vamos definir primeiro o ponto de referência: **Point.ByCoordinates**`(x,y,0);` Usamos a mesma sintaxe **Point.ByCoordinates** como especificado na parte superior do nó do ponto de referência.
> 2. As variáveis _x_ e _y_ são inseridas no **Bloco de código** de forma que possamos atualizá-las dinamicamente com controles deslizantes.
> 3. Adicione alguns _controles deslizantes_ às entradas de **Bloco de código** que variam de -50 a 50. Dessa forma, podemos expandir através do eixo padrão do Dynamo.

![](../images/8-1/2/DesignScript-exercise-03.jpg)

> 1. Na segunda linha do **Bloco de código**, definimos uma abreviação para substituir o nó de sequência numérica: `coordsXY = (-50..50..#11);`Discutiremos mais sobre isso na próxima seção. Por enquanto, observe que essa abreviação é equivalente ao nó **Sequência numérica** no script visual.

![](../images/8-1/2/DesignScript-exercise-04.jpg)

> 1. Agora, queremos criar um eixo de pontos com base na sequência _coordsXY_. Para fazer isso, queremos usar a sintaxe **Point.ByCoordinates**, mas também precisamos iniciar um _Produto transversal_ da lista da mesma maneira que fizemos no script visual. Para fazer isso, digitamos a linha: `gridPts = Point.ByCoordinates(coordsXY<1>,coordsXY<2>,0);` Os colchetes angulares indicam a referência do produto transversal.
> 2. Observe no nó **Watch3D** que temos um eixo de pontos no eixo do Dynamo.

![](../images/8-1/2/DesignScript-exercise-05.jpg)

> 1. Agora, a parte difícil: queremos mover o eixo de pontos para cima com base na distância deles até o ponto de referência. Primeiro, vamos chamar esse novo conjunto de pontos _transPts_. E, como uma conversão é uma ação em um elemento existente, em vez de usar `Geometry.Translate...`, vamos usar `gridPts.Translate`
> 2. Lendo o nó real na tela, vemos que há três entradas. A geometria para converter já está declarada porque estamos executando a ação naquele elemento (com _gridPts.Translate_). As duas entradas restantes serão inseridas entre os parênteses da função: direção e _distância_.
> 3. A direção é muito simples, usamos um `Vector.ZAxis()` para mover verticalmente.
> 4. A distância entre o ponto de referência e cada ponto do eixo ainda precisa ser calculada; portanto, fazemos isso como uma ação para o ponto de referência da mesma maneira: `refPt.DistanceTo(gridPts)`
> 5. A linha final do código fornece os pontos convertidos: `transPts=gridPts.Translate(Vector.ZAxis(),refPt.DistanceTo(gridPts));`

![](../images/8-1/2/DesignScript-exercise-06.jpg)

> 1. Agora, temos um eixo de pontos com a estrutura de dados apropriada para criar uma superfície Nurbs. Construímos a superfície usando `srf = NurbsSurface.ByControlPoints(transPts);`

![](../images/8-1/2/DesignScript-exercise-07.jpg)

> 1. E, finalmente, para adicionar alguma profundidade à superfície, construímos um sólido usando `solid = srf.Thicken(5);` Neste caso, aumentamos a espessura da superfície 5 unidades no código, mas sempre podemos declará-la como uma variável (chamando-a de espessura, por exemplo) e, em seguida, controlamos esse valor com um controle deslizante.

#### Simplificar o gráfico com “Nó para código”

O recurso “Nó para código” automatiza todo o exercício que acabamos de concluir com o clique de um botão. Isso não só é eficaz para criar definições personalizadas e blocos de código reutilizáveis, também é uma ferramenta realmente útil para saber como criar um script no Dynamo:

![](../images/8-1/2/DesignScript-exercise-08.jpg)

> 1. Inicie com o script visual existente na etapa 1 do exercício. Selecione todos os nós, clique com o botão direito do mouse na tela e selecione _“Nó para código”_. É tão simples quanto isso.

O Dynamo automatizou uma versão com base em texto do gráfico visual, incluindo a amarra e outros itens. Teste isso em seus scripts visuais e aproveite ao máximo o recurso de bloco de código.

![](../images/8-1/2/DesignScript-exercise-09.jpg)
