# O que é uma lista?

### O que é uma lista?

Uma lista é uma coleção de elementos ou de itens. Por exemplo, uma penca de bananas. Cada banana é um item na lista (ou penca). É mais fácil pegar uma penca de bananas em vez de cada banana individualmente, e o mesmo vale para agrupar elementos por relações paramétricas em uma estrutura de dados.

![Bananas](../images/5-4/1/Bananas\_white\_background\_DS.jpg)

> Foto de [Augustus Binu](https://commons.wikimedia.org/wiki/File:Bananas\_white\_background\_DS.jpg?fastcci\_from=11404890\&c1=11404890\&d1=15\&s=200\&a=list).

Quando fazemos compras, colocamos todos os itens comprados em uma sacola. Essa sacola também é uma lista. Se estivermos fazendo pão de banana, precisaremos de três pencas de bananas (estamos fazendo _muito_ de pão de banana). O saco representa uma lista de pencas de bananas e cada penca representa uma lista de bananas. O saco é uma lista de listas (bidimensionais) e a penca de bananas é uma lista (unidimensional).

No Dynamo, os dados da lista são ordenados e o primeiro item em cada lista tem um índice “0”. Abaixo, vamos discutir como as listas são definidas no Dynamo e como várias listas se relacionam entre si.

### Índices com base em zero

Uma coisa que pode parecer estranha no início é que o primeiro índice de uma lista é sempre 0; não 1. Portanto, quando falamos sobre o primeiro item de uma lista, na verdade, queremos dizer o item que corresponde ao índice 0.

Por exemplo, se você contasse o número de dedos que temos na mão direita, as chances são de que você teria contado de 1 a 5. No entanto, se você colocar seus dedos em uma lista, o Dynamo teria dado índices de 0 a 4. Embora isso possa parecer um pouco estranho para programadores iniciantes, o índice com base em zero é uma prática padrão na maioria dos sistemas de cálculo.

Observe que ainda temos cinco itens na lista; é apenas que a lista está usando um sistema de contagem baseado em zero. E os itens que estão sendo armazenados na lista não precisam ser apenas números. Eles podem ser qualquer tipo de dados que o Dynamo suporta, como pontos, curvas, superfícies, famílias etc.

![](<../images/5-4/1/what's a list - zero based indices.jpg>)

> a. Índice
>
> b. Ponto
>
> c. Item

Muitas vezes, a maneira mais fácil de analisar o tipo de dados armazenados em uma lista é conectar um nó de inspeção à saída de outro nó. Por padrão, o nó de inspeção exibe automaticamente todos os índices no lado esquerdo da lista e exibe os itens de dados na direita.

Esses índices são um elemento crucial ao trabalhar com listas.

### Entradas e saídas

Nas listas, as entradas e as saídas variam de acordo com o nó do Dynamo que está sendo usado. Como exemplo, vamos usar uma lista de cinco pontos e conectar essa saída a dois nós diferentes do Dynamo: **PolyCurve.ByPoints** e **Circle.ByCenterPointRadius**:

![Input Examples](<../images/5-4/1/what's a list - inputs and outputs.jpg>)

> 1. A entrada _points_ para **PolyCurve.ByPoints** está procurando _“Point\[]”_. Isso representa uma lista de pontos
> 2. A saída para **PolyCurve.ByPoints** é uma policurva única criada com base em uma lista de cinco pontos.
> 3. A entrada _centerPoint_ para **Circle.ByCenterPointRadius** solicita _“Ponto”_.
> 4. A saída para **Circle.ByCenterPointRadius** é uma lista de cinco círculos, cujos centros correspondem à lista original de pontos.

Os dados de entrada para **PolyCurve.ByPoints** e **Circle.ByCenterPointRadius** são os mesmos. No entanto, o nó **Polycurve.ByPoints** nos fornece uma policurva enquanto o nó **Circle.ByCenterPointRadius** nos fornece cinco círculos com centros em cada ponto. Isso faz sentido intuitivamente: a policurva é desenhada como uma curva conectando os cinco, enquanto os círculos criam um círculo diferente em cada ponto. O que está acontecendo com os dados?

Passando o mouse sobre a entrada _points_ de **Polycurve.ByPoints**, vemos que a entrada está procurando _“Point\[]”_. Observe os colchetes no final. Isso representa uma lista de pontos e, para criar uma policurva, a entrada precisa ser uma lista para cada policurva. Esse nó, portanto, condensará cada lista em uma policurva.

Por outro lado, a entrada _centerPoint_ para **Circle.ByCenterPointRadius** solicita _“Ponto”_. Esse nó procura um ponto, como um item, para definir o ponto central do círculo. É por isso que obtemos cinco círculos com base nos dados de entrada. Reconhecer essa diferença com as entradas no Dynamo ajuda a compreender melhor como os nós funcionam ao gerenciar os dados.

### Amarra

A correspondência de dados é um problema sem uma solução fácil. Isso ocorre quando um nó tem acesso a entradas de tamanhos diferentes. Alterar o algoritmo de correspondência de dados pode levar a resultados muito diferentes.

Imagine um nó que cria segmentos de linha entre pontos (**Line.ByStartPointEndPoint**). Ele terá dois parâmetros de entrada que fornecem as coordenadas do ponto:

#### Lista mais curta

A maneira mais simples é conectar as entradas individualmente até que um dos fluxos se esgote. Isso é denominado algoritmo “Lista mais curta”. Este é o comportamento padrão dos nós do Dynamo:

![](<../images/5-4/1/what's a list - lacing - shortest.jpg>)

#### Lista mais longa

O algoritmo “Lista mais longa” continua conectando entradas, reutilizando elementos, até que todos os fluxos se esgotem:

![](<../images/5-4/1/what's a list - lacing - longest.jpg>)

#### Produto transversal

Por fim, o método “Produto transversal” torna todas as conexões possíveis:

![](<../images/5-4/1/what's a list - lacing - cross.jpg>)

Como você pode ver, há diferentes maneiras de desenhar linhas entre estes conjuntos de pontos. É possível encontrar as opções de amarra clicando com o botão direito do mouse no centro de um nó e selecionando o menu “Amarra”.

![](<../images/5-4/1/what's a list - right click lacing opt.jpg>)

## Exercício

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/5-4/1/Lacing.dyn" %}

Para demonstrar as operações de amarra abaixo, vamos usar esse arquivo base para definir a lista mais curta, a lista mais longa e o produto transversal.

Vamos alterar a amarra em **Point.ByCoordinates**, mas não alteraremos mais nada no gráfico acima.

### Lista mais curta

Escolhendo _lista mais curta_ como a opção de amarra (também a opção padrão), obtemos uma linha diagonal básica composta de cinco pontos. Cinco pontos são o comprimento da lista menor, portanto, a amarra da lista mais curta é interrompida após atingir o final de uma lista.

![Input Examples](<../images/5-4/1/what's a list - lacing exercise 01.jpg>)

### **Lista mais longa**

Ao alterar a amarra para _lista mais longa_, obtemos uma linha diagonal que se estende verticalmente. Pelo mesmo método que o diagrama conceitual, o último item na lista de cinco itens será repetido para alcançar o comprimento da lista mais longa.

![Input Examples](<../images/5-4/1/what's a list - lacing exercise 02.jpg>)

### **Produto transversal**

Ao alterar a amarra para _Produto transversal_, temos todas as combinações entre cada lista, dando-nos uma grade de pontos 5x10. Essa é uma estrutura de dados equivalente ao produto transversal, como mostrado no diagrama do conceito acima, exceto que nossos dados agora são uma lista de listas. Ao conectar uma policurva, podemos ver que cada lista é definida pelo seu valor X, dando-nos uma linha de linhas verticais.

![Input Examples](<../images/5-4/1/what's a list - lacing exercise 03.jpg>)
