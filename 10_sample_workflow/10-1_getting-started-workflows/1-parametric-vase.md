---
description: suggested exercise
---

# Vaso paramétrico

Criar um vaso paramétrico é uma ótima maneira de começar a aprender a usar o Dynamo.

Este fluxo de trabalho ensinará o seguinte:

* Usar os controles deslizantes de número para controlar as variáveis no projeto.
* Criar e modificar elementos geométricos usando nós.
* Visualizar os resultados do projeto em tempo real.

![](../../1\_introduction/images/1-2/vase1.gif)

## Definição dos nossos objetivos

Antes de começar com o Dynamo, vamos projetar o nosso vaso de forma conceitual.

Digamos que vamos projetar um vaso de argila que leve em conta as práticas de fabricação usadas por ceramistas. Os ceramistas normalmente usam uma roda de cerâmica para fabricar vasos cilíndricos. Em seguida, aplicando pressão em várias alturas do vaso, eles podem alterar a forma do vaso e criar diferentes projetos.

Podemos usar uma metodologia semelhante para definir o nosso vaso. Vamos criar quatro círculos com diferentes alturas e raios e depois uma superfície elevando esses círculos.

![](../images/10-1/1/vase2.png)

## Introdução

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="../datasets/10-1/1/DynamoSampleWorkflow-vase.dyn" %}

Precisamos dos nós que representarão a sequência de ações que o Dynamo executará. Como sabemos que estamos tentando criar um círculo, vamos começar localizando um nó que faz isso. Use o campo **Pesquisar** ou navegue através da **Biblioteca** para localizar o nó **Circle.ByCenterPointRadius** e adicione-o ao espaço de trabalho

![](../images/10-1/1/vase8.png)

> 1. Pesquisar > “Círculo...”
> 2. Selecione > “ByCenterPointRadius”
> 3. O nó aparece no espaço de trabalho

Vamos dar uma olhada mais detalhada nesse nó. No lado esquerdo, encontram-se as entradas do nó (_centerPoint_ e _raio_) e, no lado direito, a saída do nó (Círculo). Observe que as saídas têm uma linha azul clara. Isso significa que a entrada tem um valor padrão. Para obter mais informações sobre a entrada, passe o cursor do mouse sobre seu nome. A entrada _raio_ precisa de uma entrada dupla e tem um valor padrão de 1.

![](../images/10-1/1/vase10.png)

Vamos manter o valor padrão de _centerPoint_, mas adicionaremos um **Controle deslizante de número** para controlar o raio. Como fizemos com o nó **Circle.ByCenterPointRadius**, use a biblioteca para procurar o **Controle deslizante de número** e adicioná-lo ao gráfico.

Esse nó é um pouco diferente do nó anterior, pois contém um controle deslizante. É possível usar a interface para alterar o valor de saída do controle deslizante.

![](../images/10-1/1/vase13\(1\).gif)

É possível configurar o controle deslizante usando o botão do menu suspenso à esquerda do nó. Vamos limitar o controle deslizante a um valor máximo de 15.

![](../images/10-1/1/vase11.png)

Vamos colocá-lo à esquerda do nó **Circle.ByCenterPointRadius** e conectar ambos os nós selecionando a saída **Controle deslizante de número** e conectando-a à entrada Raio.

![](../images/10-1/1/vase12.png)

Também vamos alterar o nome do controle deslizante de número para “Raio superior”, clicando duas vezes no nome do nó.

![](../images/10-1/1/vase14.png)

## Próximas etapas

Vamos continuar adicionando alguns nós e conexões à nossa lógica para definir o vaso.

### Criar círculos de diferentes raios

Vamos copiar esses nós quatro vezes para que os círculos definam nossa superfície. Altere os nomes do Controle deslizante de número, como mostrado abaixo.

\![](<../images/10-1/1/vase4 (1).png>)

> 1. Os círculos são criados por um ponto central e um raio

### Mover círculos ao longo da altura do vaso

Falta um parâmetro-chave para o nosso vaso: a altura. Para controlar a altura do vaso, criamos outro controle deslizante de número. Também adicionamos um nó **Bloco de código**. Os blocos de código podem ajudar durante a adição de fragmentos de código personalizados ao nosso fluxo de trabalho. Usaremos o bloco de código para multiplicar o controle deslizante de altura por diferentes fatores, para que possamos posicionar nossos círculos ao longo da altura do vaso.

![](../images/10-1/1/vase15\(1\).png)

Em seguida, usamos um nó **Geometry.Translate** para inserir círculos na altura desejada. Como queremos distribuir nossos círculos ao longo do vaso, usamos blocos de código para multiplicar o parâmetro de altura por um fator.

![](../images/10-1/1/vase5.png)

> 2\. Os círculos são convertidos (movidos) por uma variável no eixo z.

### Criar a superfície

Para criar uma superfície usando o nó **Surface.ByLoft**, precisamos combinar todos os nossos círculos convertidos em uma lista. Usamos **List.Create** para combinar todos os nossos círculos em uma única lista e, por fim, geramos a saída dessa lista para o nó **Surface.ByLoft** para visualizar os resultados.

Também vamos desativar a visualização em outros nós para exibir somente a exibição Surface.ByLoft.

\![](<../images/10-1/1/vase6 (1).png>)

> 3\. Uma superfície é criada elevando os círculos convertidos.

## Resultados

Nosso fluxo de trabalho está pronto. Agora podemos usar os **Controles deslizantes de número** que definimos em nosso script para criar diferentes projetos de vasos.

![](../../1\_introduction/images/1-2/vase1.gif)

![](../images/10-1/1/vase7.png)
