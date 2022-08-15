# Documentação

A edição de parâmetros da documentação segue as lições aprendidas nas seções anteriores. Nesta seção, vamos examinar os parâmetros de edição que não afetam as propriedades geométricas de um elemento, mas, em vez disso, preparam um arquivo do Revit para a documentação.

### Desvio

No exercício abaixo, vamos usar um desvio básico do nó do plano para criar uma folha do Revit para documentação. Cada painel em nossa estrutura de telhado definida parametricamente tem um valor diferente para o desvio, e queremos chamar o intervalo de valores usando cores e programando os pontos adaptativos para entregar a um consultor de fachadas, engenheiro ou empreiteiro.

![desvio](images/6/deviation.jpg)

> O desvio do nó do plano calculará a distância pela qual o conjunto de quatro pontos varia em relação ao plano de melhor ajuste entre eles. Essa é uma maneira rápida e fácil de estudar a construtibilidade.

## Exercício

### Parte I: Definir a relação de abertura dos painéis com base no desvio do nó do plano

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="datasets/6/Revit-Documenting.zip" %}

Inicie com o arquivo do Revit nesta seção (ou continue da seção anterior). Esse arquivo tem uma matriz de painéis ETFE no telhado. Vamos fazer referência a esses painéis para este exercício.

\![](<images/6/documenting - exercise I - 01.jpg>)

> 1. Adicione um nó _Tipos de família_ à tela e escolha _“ROOF-PANEL-4PT”_.
> 2. Conecte esse nó a um nó _Todos os elementos do tipo de família_ para selecionar todos os elementos do Revit para o Dynamo.

\![](<images/6/documenting - exercise I - 02.jpg>)

> 1. Consulte a localização dos pontos adaptativos de cada elemento com o nó _AdaptiveComponent.Locations_.
> 2. Crie um polígono com base nesses quatro pontos com o nó _Polygon.ByPoints_. Observe que agora temos uma versão abstrata do sistema de painéis no Dynamo sem ter que importar toda a geometria do elemento do Revit.
> 3. Calcule o desvio do plano com o nó _Polygon.PlaneDeviation_.

Só por diversão, como no exercício anterior, vamos definir a relação de abertura de cada painel com base em seu desvio do plano.

\![](<images/6/documenting - exercise I - 03.jpg>)

> 1. Adicione um nó _Element.SetParameterByName_ à tela e conecte os componentes adaptativos à entrada _elemento_. Conecte um _Bloco de código_ com a inscrição _“Proporção de abertura”_ à entrada _parameterName_.
> 2. Não é possível conectar diretamente os resultados do desvio à entrada de valor porque precisamos remapear os valores para o intervalo de parâmetros.

\![](<images/6/documenting - exercise I - 04.jpg>)

> 1. Usando _Math.RemapRange_, remapeie os valores de desvio para um domínio entre 0,15 e 0_,_45 inserindo `0.15; 0.45;` no _Bloco de código_.
> 2. Conecte esses resultados à entrada de valor de _Element.SetParameterByName_.

De volta ao Revit, podemos compreender _de certa forma_ a mudança de abertura na superfície.

![Exercício](../.gitbook/assets/13.jpg)

Aproximando o zoom, torna-se mais claro que os painéis fechados se concentram nos cantos da superfície. Os cantos abertos estão na parte superior. Os cantos representam áreas de desvio maior, enquanto a saliência tem uma curvatura mínima; portanto, isso faz sentido.

![Exercício](../.gitbook/assets/13a.jpg)

### Parte II: Cor e documentação

Definir a Proporção de abertura não demonstra claramente o desvio dos painéis no telhado. Também estamos alterando a geometria do elemento real. Suponha que só desejamos estudar o desvio do ponto de vista da viabilidade de fabricação. Seria útil colorir os painéis com base no intervalo de desvio para nossa documentação. Podemos fazer isso com a série de etapas abaixo e em um processo muito semelhante às etapas acima.

\![](<images/6/documenting - exercise II - 01.jpg>)

> 1. Remova o _Element.SetParameterByName_ e seus nós de entrada e adicione _Element.OverrideColorInView_.
> 2. Adicione um nó _Intervalo de cores_ à tela e conecte-o à entrada de cor _Element.OverrideColorInView_. Ainda precisamos conectar os valores de desvio ao intervalo de cores para criar o gradiente.
> 3. Se você passar o mouse sobre a entrada _valor_, será possível ver que os valores da entrada devem estar entre _0_ e _1_ para mapear uma cor para cada valor. Precisamos remapear os valores de desvio para esse intervalo.

\![](<images/6/documenting - exercise II - 02.jpg>)

> 1. Usando _Math.RemapRange_, remapeie os valores de desvio do plano para um intervalo entre* 0* e _1_ (observação: também é possível usar o nó _“MapTo”_ para definir um domínio de origem).
> 2. Conecte os resultados a um nó _Intervalo de cores_.
> 3. Observe que nossa saída é um intervalo de cores, em vez de um intervalo de números.
> 4. Se a definição estiver como Manual, pressione _Executar_. A partir desse ponto, você deve conseguir estabelecer a definição como Automático.

De volta ao Revit, vemos um gradiente muito mais legível que é representativo do desvio plano com base em nosso intervalo de cores. Mas, e se quisermos personalizar as cores? Observe que os valores de desvio mínimo são representados em vermelho, o que parece ser o oposto do que esperávamos. Queremos que o desvio máximo esteja em vermelho, com o desvio mínimo representado por uma cor menos intensa. Vamos voltar ao Dynamo e corrigir isso.

![](../.gitbook/assets/09.jpg)

\![](<images/6/documenting - exercise II - 04.jpg>)

> 1. Usando um _Bloco de código_, adicione dois números em duas linhas diferentes: `0;` e `255;`.
> 2. Crie uma cor vermelha e azul ao conectar os valores apropriados em dois nós _Color.ByARGB_.
> 3. Crie uma lista com base nessas duas cores.
> 4. Conecte essa lista à entrada _cores_ do _Intervalo de cores_ e observe a atualização personalizada do intervalo de cores.

De volta ao Revit, agora podemos entender melhor as áreas de desvio máximo nos cantos. Lembre-se: esse nó se destina a substituir uma cor em uma vista; portanto, poderia ser realmente útil se tivéssemos uma folha específica no conjunto de desenhos com foco em um determinado tipo de análise.

\![Exercise](<../.gitbook/assets/07 (6).jpg>)

### Parte III: Programação

Ao selecionar um painel ETFE no Revit, vemos que há quatro parâmetros de instância: XYZ1, XYZ2, XYZ3 e XYZ4. Eles ficam todos em branco depois de serem criados. Esses são parâmetros baseados em texto e precisam de valores. Usaremos o Dynamo para escrever as localizações dos pontos adaptativos em cada parâmetro. Isso ajudará a interoperabilidade se a geometria precisar ser enviada para um engenheiro consultor de fachadas.

\![](<images/6/documenting - exercise III - 01.jpg>)

Em uma folha de amostra, temos uma tabela grande e vazia. Os parâmetros XYZ são parâmetros compartilhados no arquivo do Revit, o que nos permite adicioná-los à tabela.

\![Exercise](<../.gitbook/assets/03 (8).jpg>)

Aproximando o zoom, os parâmetros XYZ ainda devem ser preenchidos. Os dois primeiros parâmetros são preparados pelo Revit.

\![Exercise](<../.gitbook/assets/02 (9).jpg>)

Para gravar esses valores, faremos uma operação de lista complexa. O gráfico em si é simples, mas os conceitos são criados intensamente com base no mapeamento da lista, conforme discutido no capítulo sobre a lista.

\![](<images/6/documenting - exercise III - 04.jpg>)

> 1. Selecione todos os componentes adaptativos com dois nós.
> 2. Extraia a localização de cada ponto com _AdaptiveComponent.Locations_.
> 3. Converta esses pontos em sequências de caracteres. Lembre-se: o parâmetro se baseia em texto; portanto, é preciso inserir o tipo de dados correto.
> 4. Crie uma lista das quatro sequências de caracteres que definem os parâmetros a serem alterados:_XYZ1, XYZ2, XYZ3,_ e _XYZ4_.
> 5. Conecte essa lista à entrada _parameterName_ de _Element.SetParameterByName_.
> 6. Conecte _Element.SetParameterByName_ à entrada _combinador_ de _List.Combine_. Conecte os _componentes adaptativos_ à _list1_. Conecte a _Sequência de caracteres_ do objeto à _list2_.

Estamos mapeando a lista aqui, porque estamos gravando quatro valores para cada elemento, que cria uma estrutura de dados complexa. O nó _List.Combine_ define uma operação uma etapa abaixo na hierarquia de dados. É por isso que as entradas de elemento e de valor _Element.SetParameterByName_ são deixadas em branco. _List.Combine_ está conectando as sublistas de suas entradas às entradas vazias de _Element.SetParameterByName_, com base na ordem em que elas estão conectadas.

Selecionando um painel no Revit, poderemos ver agora que temos valores de sequência de caracteres para cada parâmetro. Realisticamente, criaríamos um formato mais simples para escrever um ponto (X,Y,Z). Isso pode ser feito com operações de sequência de caracteres no Dynamo, mas estamos ignorando isso aqui para permanecer no escopo deste capítulo.

\![](<../.gitbook/assets/04 (5).jpg>)

Uma vista da amostra de cronograma com parâmetros preenchidos.

\![](<../.gitbook/assets/01 (9).jpg>)

Cada painel ETFE agora tem as coordenadas XYZ gravadas para cada ponto adaptativo, representando os cantos de cada painel para fabricação.

\![Exercise](<../.gitbook/assets/00 (8).jpg>)
