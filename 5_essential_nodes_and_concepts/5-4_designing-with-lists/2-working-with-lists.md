# Trabalhar com listas

### Trabalhar com listas

Agora que estabelecemos o que é uma lista, vamos falar sobre as operações que podemos realizar nela. Imagine uma lista como um baralho de cartas. O baralho é a lista e cada carta representa um item.

![cartas](../images/5-4/2/Playing_cards_modified.jpg)

> Foto de [Christian Gidlöf](https://commons.wikimedia.org/wiki/File:Playing_cards_modified.jpg)

### Consulta

Quais **consultas** podemos fazer na lista? Isso acessa as propriedades existentes.

* Número de cartas no baralho? 52\.
* Número de naipes? 4\.
* Material? Papel.
* Comprimento? 3,5 pol. ou 89 mm.
* Largura? 2,5 pol. ou 64 mm.

### Ação

Quais **ações** podemos executar na lista? Isso altera a lista com base em uma determinada operação.

* Podemos embaralhar as cartas.
* Podemos classificar as cartas por valor.
* Podemos classificar as cartas por naipe.
* Podemos dividir o baralho.
* Podemos dividir o baralho, distribuindo cada uma das mãos da rodada.
* É possível selecionar uma carta específica no baralho.

Todas as operações listadas acima têm nós análogos do Dynamo para trabalhar com listas de dados genéricos. As lições abaixo demonstrarão algumas das operações fundamentais que podemos executar nas listas.

## **Exercício**

### **Operações de lista**

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/2/List-Operations.dyn" %}

A imagem abaixo é o gráfico de base no qual estamos desenhando linhas entre dois círculos para representar operações básicas de lista. Vamos explorar como gerenciar dados em uma lista e demonstrar os resultados visuais através das ações da lista abaixo.

![](../images/5-4/2/workingwithlist-listoperation.jpg)

> 1. Comece com um **Bloco de código** com um valor de `500;`
> 2. Conecte-se à entrada x de um nó **Point.ByCoordinates**.
> 3. Conecte o nó da etapa anterior à entrada de origem de um nó **Plane.ByOriginNormal**.
> 4. Usando um nó **Circle.ByPlaneRadius**, conecte o nó da etapa anterior à entrada do plano.
> 5. Usando o **Bloco de código**, designe um valor de `50;` para o raio. Esse é o primeiro círculo que criaremos.
> 6. Com um nó **Geometry.Translate**, movimente o círculo 100 unidades para cima na direção Z.
> 7. Com um nó de **Bloco de código**, defina um intervalo de dez números entre 0 e 1 com esta linha de código: `0..1..#10;`
> 8. Conecte o bloco de código da etapa anterior à entrada _parâmetro_ de dois nós **Curve.PointAtParameter**. Conecte **Circle.ByPlaneRadius** à entrada de curva do nó superior e **Geometry.Translate** à entrada de curva do nó abaixo dele.
> 9. Usando **Line.ByStartPointEndPoint**, conecte os dois nós **Curve.PointAtParameter**.

### List.Count

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/2/List-Count.dyn" %}

O nó _List.Count_ é simples: conta o número de valores em uma lista e retorna esse número. Esse nó ganha mais nuances à medida que trabalhamos com listas de listas, mas vamos demonstrar isso nas seções a seguir.

![Contagem](../images/5-4/2/workingwithlist-listoperation-listcount.jpg)

> 1. O nó **List.Count** retorna o número de linhas no nó **Line.ByStartPointEndPoint**. Neste caso, o valor é 10, que concorda com o número de pontos criados no nó original do **Bloco de código**.

### List.GetItemAtIndex

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/2/List-GetItemAtIndex.dyn" %}

**List.GetItemAtIndex** é uma forma fundamental de consultar um item na lista.

![Exercício](../images/5-4/2/workingwithlist-getitemindex01.jpg)

> 1. Primeiro, clique com o botão direito do mouse no nó **Line.ByStartPointEndPoint** para desativar sua visualização.
> 2. Usando o nó **List.GetItemAtIndex**, estamos selecionando o índice _“0”_ ou o primeiro item na lista de linhas.

Altere o valor do controle deslizante entre 0 e 9 para selecionar outro item usando **List.GetItemAtIndex**.

![](../images/5-4/2/workingwithlist-getitemindex02.gif)

### List.Reverse

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/2/List-Reverse.dyn" %}

_List.Reverse_ inverte a ordem de todos os itens em uma lista.

![Exercício](../images/5-4/2/workingwithlist-listreverse.jpg)

> 1. Para visualizar corretamente a lista invertida de linhas, crie mais linhas alterando o **Bloco de código** para `0..1..#50;`
> 2. Duplique o nó **Line.ByStartPointEndPoint**, insira um nó List.Reverse entre **Curve.PointAtParameter** e o segundo **Line.ByStartPointEndPoint**
> 3. Use os nós **Watch3D** para visualizar dois resultados diferentes. O primeiro mostra o resultado sem uma lista invertida. As linhas se conectam verticalmente aos pontos adjacentes. A lista invertida, no entanto, conectará todos os pontos à ordem oposta na outra lista.

### List.ShiftIndices <a href="#listshiftindices" id="listshiftindices"></a>

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/2/List-ShiftIndices.dyn" %}

**List.ShiftIndices** é uma boa ferramenta para criar padrões de torções ou helicoidais, ou qualquer outra manipulação de dados semelhante. Esse nó altera os itens em uma lista por um determinado número de índices.

![Exercício](../images/5-4/2/workingwithlist-shiftIndices01.jpg)

> 1. No mesmo processo que a lista inversa, insira um **List.ShiftIndices** no **Curve.PointAtParameter** e **Line.ByStartPointEndPoint**.
> 2. Usando um **Bloco de código**, foi designado o valor de “1” para mudar a lista em um índice.
> 3. Observe que a alteração é sutil, mas todas as linhas no nó inferior **Watch3D** mudaram um índice ao se conectarem ao outro conjunto de pontos.

Se o **Bloco de código** for alterado para um valor maior, _“30”_, por exemplo, perceberemos uma diferença significativa nas linhas diagonais. A mudança está funcionando como a íris da câmera nesse caso, criando uma torção na forma cilíndrica original.

![](../images/5-4/2/workingwithlist-shiftIndices02.jpg)

### List.FilterByBooleanMask <a href="#listfilterbybooleanmask" id="listfilterbybooleanmask"></a>

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/2/List-FilterByBooleanMask.dyn" %}

![](../images/5-4/2/ListFilterBool.png)

**List.FilterByBooleanMask** removerá determinados itens com base em uma lista de booleanos ou valores “true” ou “false”.

![Exercício](../images/5-4/2/workingwithlist-filterbyboolmask.jpg)

Para criar uma lista de valores “true” ou “false”, é necessário um pouco mais de trabalho...

> 1. Usando um **Bloco de código**, defina uma expressão com a sintaxe: `0..List.Count(list);`. Conecte o nó **Curve.PointAtParameter** à entrada _lista_. Vamos abordar mais essa configuração no capítulo do bloco de código, mas a linha de código nesse caso fornece uma lista que representa cada índice do nó **Curve.PointAtParameter**.
> 2. Usando um nó _**%**_** (módulo)**, conecte a saída do _bloco de código_ à entrada _x_ e um valor de _4_ à entrada _y_. Isso vai gerar o resto ao dividir a lista de índices por 4. O módulo é um nó muito útil para a criação do padrão. Todos os valores serão lidos como os possíveis restos de 4: 0, 1, 2, 3.
> 3. No nó _**%**_** (módulo)**, sabemos que um valor de 0 significa que o índice é divisível por 4 (0, 4, 8 etc...). Usando um nó **==**, podemos testar a divisibilidade com um valor de _“0”_.
> 4. O nó **Watch** revela apenas isso: temos um padrão true/false com a indicação: _true,false,false,false..._.
> 5. Usando esse padrão true/false, conecte-se à entrada de máscara dos dois nós **List.FilterByBooleanMask**.
> 6. Conecte o nó **Curve.PointAtParameter** a cada entrada da lista do **List.FilterByBooleanMask**.
> 7. A saída de **Filter.ByBooleanMask** é _“in”_ e _“out”_. _“In”_ representa valores que tinham um valor de máscara _“true”_, enquanto _“out”_ representa valores que tinham um valor _“false”_. Conectando as saídas _“in”_ às entradas _startPoint_ e _endPoint_ de um nó **Line.ByStartPointEndPoint**, criamos uma lista de linhas filtrada.
> 8. O nó **Watch3D** revela que temos menos linhas do que pontos. Selecionamos somente 25% dos nós filtrando somente os valores true.
