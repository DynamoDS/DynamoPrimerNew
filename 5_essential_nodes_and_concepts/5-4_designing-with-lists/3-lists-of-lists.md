# Listas de listas

### Listas de listas

Vamos adicionar mais um nível à hierarquia. Se pegarmos o maço de cartas do exemplo original e criarmos uma caixa que contém vários maços, a caixa agora representará uma lista de maços e cada maço representará uma lista de cartas. Esta é uma lista de listas. Para a analogia nesta seção, a imagem abaixo contém uma lista de rolos de moeda e cada rolo contém uma lista de moedas de um centavo.

![Moedas](../images/5-4/3/coins-521245\_640.jpg)

> Foto de [Dori](https://commons.wikimedia.org/wiki/File:Stack\_of\_coins\_0214.jpg).

### Consulta

Que **consultas** podemos fazer na lista de listas? Isso acessa as propriedades existentes.

* Quantidade de tipos de moeda? 2.
* Valores de tipos de moeda? $ 0,01 e $ 0,25.
* Material de moedas de vinte e cinco centavos? 75% de cobre e 25% de níquel.
* Material de moedas de um centavo? 97,5% de zinco e 2,5% de cobre.

### Ação

Que **ações** podemos executar na lista de listas? Isso altera a lista de listas com base em uma operação fornecida.

* Selecione uma pilha específica de moedas de vinte e cinco centavos ou de um centavo.
* Selecione uma moeda específica de vinte e cinco centavos ou um centavo.
* Reorganize as pilhas de moedas de vinte e cinco centavos e de um centavo.
* Misture as pilhas.

Novamente, o Dynamo tem um nó analógico para cada uma das operações acima. Como estamos trabalhando com dados abstratos e não com objetos físicos, precisamos de um conjunto de regras para controlar como movemos para cima e para baixo na hierarquia de dados.

Ao lidar com listas de listas, os dados são complexos e dispostos em camadas, mas isso proporciona a oportunidade de executar algumas operações paramétricas incríveis. Vamos separar os conceitos básicos e discutir mais algumas operações nas lições abaixo.

## Exercício

### Hierarquia de cima para baixo

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/3/Top-Down-Hierarchy.dyn" %}

O conceito fundamental a ser aprendido nesta seção: **o Dynamo trata as listas intrinsecamente como objetos**. Essa hierarquia de cima para baixo é desenvolvida considerando a programação orientada a objetos. Em vez de selecionar subelementos com um comando como **List.GetItemAtIndex**, o Dynamo seleciona o índice da lista principal na estrutura de dados. E esse item pode ser outra lista. Vamos explicar em detalhes com uma imagem de exemplo:

![top-down](<../images/5-4/3/lists of lists - top down hierachy.jpg>)

> 1. Com o **Bloco de código**, definimos dois intervalos: `0..2; 0..3;`
> 2. Esses intervalos são conectados a um nó **Point.ByCoordinates** com amarra definida como _“Produto transversal”_. Isso cria um eixo de pontos e também retorna uma lista de listas como uma saída.
> 3. Observe que o nó de **Inspeção** fornece três listas com quatro itens cada.
> 4. Ao usar **List.GetItemAtIndex**, com um índice de 0, o Dynamo seleciona a primeira lista e todo o seu conteúdo. Outros programas podem selecionar o primeiro item de cada lista na estrutura de dados, mas o Dynamo emprega uma hierarquia de cima para baixo ao lidar com os dados.

### List.Flatten

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/3/Flatten.dyn" %}

Mesclar remove todos os níveis de dados de uma estrutura de dados. Isso é útil quando as hierarquias de dados não são necessárias para sua operação, mas pode ser arriscado porque remove as informações. O exemplo abaixo mostra o resultado da mesclagem de uma lista de dados.

![Exercise](<../images/5-4/3/lists of lists - flatten 01.jpg>)

> 1. Insira uma linha de código para definir um intervalo em **Bloco de código**: `-250..-150..#4;`
> 2. Conectando o _bloco de código_ à entrada _x_ e _y_ de um nó **Point.ByCoordinates**, definimos a amarra como _“Produto transversal”_ para obter um eixo de pontos.
> 3. O nó de **Inspeção** mostra que temos uma lista de listas.
> 4. Um nó **PolyCurve.ByPoints** fará referência a cada lista e criará uma PolyCurve respectiva. Observe na visualização do Dynamo que temos quatro policurvas que representam cada linha no eixo.

![Exercise](<../images/5-4/3/lists of lists - flatten 02.jpg>)

> 1. Ao inserir uma _mesclagem_ antes do nó PolyCurve, criamos uma única lista para todos os pontos. O nó **PolyCurve.ByPoints** faz referência a uma lista para criar uma curva e, como todos os pontos estão em uma lista, obtemos uma policurva em zigue-zague que segue em toda a lista de pontos.

Há também opções para mesclar níveis isolados de dados. Usando o nó **List.Flatten**, é possível definir um número definido de níveis de dados para mesclar do topo da hierarquia. Essa será uma ferramenta realmente útil se você estiver lidando com estruturas de dados complexas, que não são necessariamente relevantes para o fluxo de trabalho. Outra opção é usar o nó de mesclagem como uma função em **List.Map**. Discutiremos mais sobre **List.Map** abaixo.

### Cortar

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/3/Chop.dyn" %}

Durante a modelagem paramétrica, também há ocasiões em que você desejará modificar a estrutura de dados para uma lista existente. Há muitos nós disponíveis para isso também, e o corte é a versão mais básica. Com o corte, é possível particionar uma lista em sublistas com um número definido de itens.

O comando de corte divide as listas com base em um determinado comprimento de lista. De certa forma, o corte é o oposto da mesclagem: em vez de remover a estrutura de dados, ele adiciona novos níveis a ela. Essa é uma ferramenta útil para operações geométricas como o exemplo abaixo.

![Exercise](<../images/5-4/3/lists of lists - chop.jpg>)

### List.Map

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/3/Map.dyn" %}

**List.Map/Combine** aplica uma função definida a uma lista de entrada, mas uma etapa abaixo na hierarquia. As combinações são o mesmo que os mapas, exceto que as combinações podem ter várias entradas correspondentes à entrada de uma determinada função.

_Observação: Este exercício foi criado com uma versão anterior do Dynamo. A maior parte da funcionalidade de_ **List.Map** _foi resolvida com a adição do recurso_ **List@Level** _. Para obter mais informações, consulte_ [_List@Level_](6-3\_lists-of-lists.md#listlevel) _, abaixo._

Como uma rápida introdução, vamos revisar o nó **List.Count** de uma seção anterior.

O nó **List.Count** conta todos os itens em uma lista. Usaremos isso para demonstrar como **List.Map** funciona.

![](<../images/5-4/3/lists of lists - map 01.jpg>)

> 1. Insira duas linhas de código no **Bloco de código**: `-50..50..#Nx; -50..50..#Ny;`
>
>    Após digitar esse código, o bloco de código criará duas entradas para Nx e Ny.
> 2. Com dois _controles deslizantes de número inteiro_, defina os valores _Nx_ e _Ny_ conectando-os ao **Bloco de código**.
> 3. Conecte cada linha do bloco de código às respectivas entradas _X_ e _Y_ de um nó **Point.ByCoordinates**. Clique com o botão direito do mouse no nó, selecione “Amarra” e selecione _“Produto transversal”_. Isso cria um eixo de pontos. Como definimos o intervalo de -50 a 50, estamos expandindo o eixo padrão do Dynamo.
> 4. Um nó de _**Inspeção**_ revela os pontos criados. Observe a estrutura de dados. Criamos uma lista de listas. Cada lista representa uma linha de pontos do eixo.

![Exercise](<../images/5-4/3/lists of lists - map 02 (1).jpg>)

> 1. Anexe um nó **List.Count** à saída do nó de inspeção da etapa anterior.
> 2. Conecte um nó de **Inspeção** à saída** List.Count**.

Observe que o nó List.Count fornece um valor igual a 5. Isso é igual à variável “Nx”, conforme definido no bloco de código. Por que isso ocorre?

* Primeiro, o nó **Point.ByCoordinates** usa a entrada “x” como entrada principal para criar listas. Quando Nx é 5 e Ny é 3, obtemos uma lista de 5 listas, cada uma com 3 itens.
* Como o Dynamo trata as listas intrinsecamente como objetos, um nó **List.Count** é aplicado à lista principal na hierarquia. O resultado é um valor igual a 5 ou o número de listas na lista principal.

![Exercise](<../images/5-4/3/lists of lists - map 03.jpg>)

> 1. Usando um nó **List.Map**, descemos um degrau na hierarquia e executamos uma _“função”_ neste nível.
> 2. Observe que o nó **List.Count** não tem nenhuma entrada. Ele está sendo usado como uma função para que o nó **List.Count** seja aplicado a cada lista individual um degrau abaixo na hierarquia. A entrada em branco de **List.Count** corresponde à entrada de lista de **List.Map**.
> 3. Os resultados de **List.Count** agora fornecem uma lista de 5 itens, cada um com valor de 3. Isso representa o tamanho de cada sublista.

### **List.Combine**

_Observação: Este exercício foi criado com uma versão anterior do Dynamo. A maior parte da funcionalidade de List.Combine foi resolvida com a adição do recurso_ **List@Level** _. Para obter mais informações, consulte_ [_List@Level_](6-3\_lists-of-lists.md#listlevel) _, abaixo._

Neste exercício, usaremos **List.Combine** para demonstrar como pode ser usado para aplicar uma função em listas separadas de objetos.

Comece configurando duas listas de pontos.

![Exercise](<../images/5-4/3/lists of lists - combined 01.jpg>)

> 1. Use o nó **Sequência** para gerar 10 valores, cada um com um incremento de 10 etapas.
> 2. Conecte o resultado à entrada x de um nó **Point.ByCoordinates**. Isso criará uma lista de pontos no Dynamo.
> 3. Adicione um segundo nó **Point.ByCoordinates** ao espaço de trabalho, use a mesma saída de **Sequência** como sua entrada x, mas use um **Controle deslizante de número inteiro** como sua entrada y e defina seu valor como 31 (pode ser qualquer valor, desde que não se sobreponha ao primeiro conjunto de pontos) para que os dois conjuntos de pontos não se sobreponham um ao outro.

Em seguida, usaremos **List.Combine** para aplicar uma função em objetos em duas listas separadas. Neste caso, será uma função de linha de desenho simples.

![Exercise](<../images/5-4/3/lists of lists - combined 02.jpg>)

> 1. Adicione **List.Combine** ao espaço de trabalho e conecte os dois conjuntos de pontos como suas entradas list0 e list1.
> 2. Use **Line.ByStartPointEndPoint** como a função de entrada para **List.Combine**.

Após a conclusão, os dois conjuntos de pontos são compactados/emparelhados por meio de uma função **Line.ByStartPointEndPoint** e retornam 10 linhas no Dynamo.

{% hint style="info" %}
Consulte o exercício em Listas n-dimensionais para ver outro exemplo de uso de List.Combine.
{% endhint %}

### List@Level

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/3/Listatlevel.dyn" %}

Com preferência em relação ao **List.Map**, o recurso **List@Level** permite selecionar diretamente com qual nível de lista você deseja trabalhar diretamente na porta de entrada do nó. Esse recurso pode ser aplicado a qualquer entrada de nó e permitirá que você acesse os níveis de suas listas com mais rapidez e facilidade do que outros métodos. Basta informar ao nó qual nível da lista você deseja usar como entrada e deixar que o nó faça o restante.

Neste exercício, usaremos o recurso **List@Level** para isolar um nível de dados específico.

![List@Level](<../images/5-4/3/lists of lists - list at level 01.jpg>)

Começaremos com um eixo de pontos 3D simples.

> 1. Como o eixo é construído com um intervalo para X, Y e Z, sabemos que os dados estão estruturados com três níveis: uma lista X, uma lista Y e uma lista Z.
> 2. Esses níveis existem em diferentes **Níveis**. Os níveis são indicados na parte inferior da bolha de visualização. As colunas de níveis da lista correspondem aos dados da lista acima para ajudar a identificar em qual nível se deve trabalhar.
> 3. Os níveis da lista estão organizados em ordem inversa para que os dados de menor nível estejam sempre em “L1”. Isso ajudará a garantir que seus gráficos funcionarão conforme planejado, mesmo que algo seja alterado a montante.

![List@Level](<../images/5-4/3/lists of lists - list at level 02.jpg>)

> 1. Para usar a função **List@Level**, clique em “>”. Neste menu, você verá duas caixas de seleção.
> 2. **Usar níveis** – Ativa a funcionalidade **List@Level**. Após clicar nessa opção, você poderá clicar e selecionar os níveis da lista de entrada que deseja que o nó use. Com esse menu, você pode experimentar rapidamente diferentes opções de nível clicando para cima ou para baixo.
> 3. _Manter a estrutura da lista_ – se estiver ativada, você terá a opção de manter a estrutura do nível de entrada. Às vezes, você pode ter organizado intencionalmente seus dados em sublistas. Ao marcar essa opção, é possível manter a organização da lista intacta e não perder nenhuma informação.

Com nosso eixo 3D simples, podemos acessar e visualizar a estrutura de lista alternando entre os níveis de lista. Cada combinação de nível de lista e índice retorna um conjunto de pontos diferente do nosso conjunto 3D original.

![](<../images/5-4/3/lists of lists - list at level 03.jpg>)

> 1. “@L2” em DesignScript nos permite selecionar somente a lista no Nível 2. A lista no Nível 2 com o índice 0 inclui somente o primeiro conjunto de pontos Y, retornando somente o eixo XZ.
> 2. Se alterarmos o filtro Nível para “L1”, poderemos ver tudo no primeiro nível da lista. A lista no Nível 1 com índice 0 inclui todos os nossos pontos 3D em uma lista plana.
> 3. Se tentarmos fazer o mesmo para “L3”, veremos somente os pontos do terceiro nível da lista. A lista no Nível 3 com índice 0 inclui somente o primeiro conjunto de pontos Z, retornando somente um eixo XY.
> 4. Se tentarmos o mesmo para “L4”, veremos somente os pontos do terceiro nível da lista. A lista no Nível 4 com índice 0 inclui somente o primeiro conjunto de pontos X, retornando somente um eixo YZ.

Embora este exemplo específico também possa ser criado com **List.Map**, o recurso **List@Level** simplifica muito a interação, facilitando o acesso aos dados do nó. Veja abaixo uma comparação entre os métodos**List.Map** e **List@Level**:

![](<../images/5-4/3/lists of lists - list at level 04.jpg>)

> 1. Embora ambos os métodos ofereçam acesso aos mesmos pontos, o método **List@Level** permite alternar facilmente entre as camadas de dados em um único nó.
> 2. Para acessar um eixo de pontos com **List.Map**, precisamos de um nó **List.GetItemAtIndex** junto com **List.Map**. Para cada nível de lista que descermos, precisaremos usar um nó **List.Map** adicional. Dependendo da complexidade das listas, isso pode exigir a adição de uma quantidade significativa de nós **List.Map** ao gráfico para acessar o nível de informações correto.
> 3. Neste exemplo, um nó **List.GetItemAtIndex** com um nó **List.Map** retorna o mesmo conjunto de pontos com a mesma estrutura de lista que o **List.GetItemAtIndex** com “@L3” selecionado.

### Transpor

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/3/Transpose.dyn" %}

Transpor é uma função fundamental ao lidar com listas de listas. Assim como nos programas de planilha, uma transposição inverte as colunas e linhas de uma estrutura de dados. Vamos demonstrar isso com uma matriz básica abaixo e, na seção a seguir, vamos demonstrar como uma transposição pode ser usada para criar relações geométricas.

![Transpor](../images/5-4/3/transpose1.jpg)

Vamos excluir os nós **List.Count** do exercício anterior e usar alguma geometria para ver como os dados se estruturaram.

![](<../images/5-4/3/lists of lists - transpose 01.jpg>)

> 1. Conecte um **PolyCurve.ByPoints** à saída do nó de inspeção de **Point.ByCoordinates**.
> 2. A saída mostra 5 PolyCurves e podemos ver as curvas na visualização do Dynamo. O nó do Dynamo está procurando uma lista de pontos (ou uma lista de listas de pontos neste caso) e criando uma única PolyCurve com base neles. Essencialmente, cada lista foi convertida em uma curva na estrutura de dados.

![](<../images/5-4/3/lists of lists - transpose 02.jpg>)

> 1. Um nó **List.Transpose** alternará todos os itens com todas as listas em uma lista de listas. Isso parece complicado, mas é a mesma lógica que Transpor no Microsoft Excel: alternar colunas com linhas em uma estrutura de dados.
> 2. Observe o resultado abstrato: a transposição alterou a estrutura da lista de 5 listas com 3 itens cada uma para 3 listas com 5 itens cada uma.
> 3. Observe o resultado geométrico: usando **PolyCurve.ByPoints**, obtemos 3 PolyCurves na direção perpendicular às curvas originais.

## Bloco de código para criação de lista

A abreviação do bloco de código usa “[]” para definir uma lista. Essa é uma maneira muito mais rápida e fluida de criar uma lista do que com o nó **List.Create**. O **Bloco de código** é discutido com mais detalhes em [Blocos de código e DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/). Consulte a imagem abaixo para observar como uma lista com várias expressões pode ser definida com o bloco de código.

![](<../images/5-4/3/lists of lists - codeblock for list creation 01.jpg>)

#### Consulta do bloco de código

A abreviação do **Bloco de código** usa “[]” como uma forma rápida e fácil de selecionar itens específicos que você deseja de uma estrutura de dados complexa. Os **Blocos de código** são discutidos com mais detalhes no [capítulo Bloco de código e DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/). Consulte a imagem abaixo para observar como uma lista com vários tipos de dados pode ser consultada com o bloco de código.

![](<../images/5-4/3/lists of lists - codeblock for list creation 02.jpg>)

## Exercício – Consultar e inserir dados

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/3/ReplaceItems.dyn" %}

Este exercício usa uma lógica estabelecida no exercício anterior para editar uma superfície. Nosso objetivo aqui é intuitivo, mas a navegação na estrutura de dados estará mais envolvida. Desejamos articular uma superfície movendo um ponto de controle.

Comece com a cadeia de caracteres dos nós acima. Estamos criando uma superfície básica que abrange o eixo padrão do Dynamo.

![](<../images/5-4/3/list of lists - exercise cb insert & query 01.jpg>)

> 1. Usando o **Bloco de código**, insira essas duas linhas de código e conecte-se às entradas _u_ e _v_ de **Surface.PointAtParameter**, respectivamente: `-50..50..#3;` `-50..50..#5;`
> 2. Certifique-se de configurar a Amarra de **Surface.PointAtParameter** como _“Produto transversal”_.
> 3. O nó de **Inspeção** mostra que temos uma lista de 3 listas, cada uma com 5 itens.

Nesta etapa, queremos consultar o ponto central no eixo que criamos. Para fazer isso, vamos selecionar o ponto médio na lista do meio. Faz sentido, certo?

![](<../images/5-4/3/list of lists - exercise cb insert & query 02.jpg>)

> 1. Para confirmar que esse é o ponto correto, também podemos clicar nos itens do nó de inspeção para confirmar que estamos selecionando o item correto.
> 2. Usando o **Bloco de código**, vamos escrever uma linha básica de código para consultar uma lista de listas:\
>    `points[1][2];`
> 3. Usando **Geometry.Translate**, moveremos o ponto selecionado para cima na direção _Z_ por _20_ unidades.

![](<../images/5-4/3/list of lists - exercise cb insert & query 03.jpg>)

> 1. Vamos também selecionar a linha média de pontos com um nó **List.GetItemAtIndex**. Observação: De forma similar a uma etapa anterior, também é possível consultar a lista com o **Bloco de código**, usando uma linha de `points[1];`

Até agora, consultamos com êxito o ponto central e o movemos para cima. Agora, queremos inserir esse ponto movido de volta para a estrutura de dados original.

![](<../images/5-4/3/list of lists - exercise cb insert & query 04.jpg>)

> 1. Primeiro, queremos substituir o item da lista que isolamos em uma etapa anterior.
> 2. Usando **List.ReplaceItemAtIndex**, vamos substituir o item do meio e o índice de _“2”_ pelo item de substituição conectado ao ponto movido (**Geometry.Translate**).
> 3. A saída mostra que inserimos o ponto movido no item do meio da lista.

Agora que modificamos a lista, precisamos inseri-la de volta na estrutura de dados original: a lista de listas.

![](<../images/5-4/3/list of lists - exercise cb insert & query 05.jpg>)

> 1. Seguindo a mesma lógica, use **List.ReplaceItemAtIndex** para substituir a lista do centro pela nossa lista modificada.
> 2. Observe que os **Blocos de código**__ que definem o índice para estes dois nós são 1 e 2, que coincidem com a consulta original do **Blocos de código** (_pontos[1][2]_).
> 3. Ao selecionar a lista no _índice 1_, vemos a estrutura de dados realçada na visualização do Dynamo. Mesclamos com êxito o ponto movido para a estrutura de dados original.

Existem várias maneiras de criar uma superfície com base nesse conjunto de pontos. Neste caso, vamos criar uma superfície por meio da elevação de curvas.

![](<../images/5-4/3/list of lists - exercise cb insert & query 06.jpg>)

> 1. Crie um nó **NurbsCurve.ByPoints** e conecte a nova estrutura de dados para criar três curvas NURBS.

![](<../images/5-4/3/list of lists - exercise cb insert & query 07.jpg>)

> 1. Conecte uma **Surface.ByLoft** à saída de **NurbsCurve.ByPoints**. Agora, temos uma superfície modificada. É possível alterar o valor _Z_ original da geometria. Converta e observe a atualização da geometria.
