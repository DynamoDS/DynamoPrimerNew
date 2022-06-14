# Nós e fios

## Nós

No Dynamo, os **Nós** são os objetos que você conecta para formar um programa visual. Cada **Nó** executa uma operação: às vezes pode ser tão simples quanto armazenar um número ou pode ser uma ação mais complexa, como criar ou consultar geometria.

### Anatomia de um nó

A maioria dos nós no Dynamo são compostos de cinco partes. Embora existam exceções, como os nós de entrada, a anatomia de cada nó pode ser descrita da seguinte maneira:

![](<images/nodes and wires - nodes anatomy.jpg>)

> 1. Nome – O nome do nó com uma convenção de nomenclatura `Category.Name`.
> 2. Corpo principal – O corpo principal do nó: se clicar aqui com o botão direito do mouse, serão apresentadas opções de todo o nó
> 3. Portas (entrada e saída) – Os destinatários dos fios que fornecem os dados de entrada para o nó, assim como os resultados da ação do nó
> 4. Valor padrão – Clique com o botão direito do mouse em uma porta de entrada: alguns nós têm valores padrão que podem ser usados ou não.
> 5. Ícone de amarra – Indica a [opção de amarra](../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md#lacing) especificada para as entradas de lista coincidentes (mais informações adiante)

### Portas de entrada/saída de nós

As entradas e saídas nos nós são chamadas de portas e agem como receptores para os fios. Os dados entram o nó através das portas à esquerda e saem do nó após ele ter executado sua operação à direita.

As portas esperam receber dados de um determinado tipo. Por exemplo, conectar um número como _2.75_ às portas em um nó Point By Coordinates resultará na criação com êxito de um ponto; no entanto, se fornecermos _“Red”_ à mesma porta, resultará em um erro.

{% hint style="info" %}
Dica: Passe o cursor do mouse sobre uma porta para ver uma dica de ferramenta contendo o tipo de dados esperado.
{% endhint %}

![](<images/nodes and wires - nodes input and tooltip.jpg>)

> 1. Legenda da porta
> 2. Dica de ferramenta
> 3. Tipo de dados
> 4. Valor padrão

### Estados do nó

O Dynamo fornece uma indicação do estado da execução do programa visual ao renderizar os nós com diferentes esquemas de cores com base no status de cada nó. A hierarquia de estados segue esta sequência: Erro > Aviso > Informações > Visualização.

Passar o cursor do mouse ou clicar com o botão direito do mouse sobre o nome ou as portas apresenta informações e opções adicionais.

![](<images/nodes and wires - node states.jpg>)

> 1. Ativo – Os nós com um plano de fundo do nome cinza escuro estão bem conectados e têm todas as suas entradas conectadas com êxito
> 2. Estado de erro – A barra de status vermelha abaixo do nó indica que o nó está em um estado de erro
> 3. Congelar – Um nó transparente tem a função Congelar ativada, suspendendo a execução do nó
> 4. Visualização do plano de fundo – A barra de status cinza abaixo do nó e o ícone de olho ![](<images/nodes and wires - preview off.jpg>) indica que a visualização da geometria está desativada.
> 5. Selecionado – Os nós atualmente selecionados têm um realce azul-claro em sua borda
> 6. Aviso – A barra de status amarela abaixo do nó indica o estado de Aviso, o que significa que o nó não tem dados de entrada ou pode ter tipos de dados incorretos.

#### Lidar com nós com estado de erro ou de aviso

Se o programa visual tiver avisos ou erros, o Dynamo fornecerá informações adicionais sobre o problema. Qualquer nó exibido em amarelo também terá uma dica acima do nome. Passe o cursor do mouse sobre o ícone da dica de ferramenta de aviso ![](<images/nodes and wires - node warning icon.png>) ou de erro ![](<images/nodes and wires - node error icon.png>) para expandi-lo.

{% hint style="info" %}
Dica: Com esta informação da dica de ferramenta, examine os nós a montante para ver se o tipo de dados ou a estrutura de dados está com erro.
{% endhint %}

![](<images/nodes and wires - nodes with warning tooltip.jpg>)

> 1. Dica de ferramenta de aviso – “Nulo” ou nenhum dado não pode ser entendido como um Duplo, ou seja, um número
> 2. Use o nó Watch para examinar os dados de entrada
> 3. A montante do nó Number está indicando “Red”, não um número

## Fios

Os fios se conectam entre os nós para criar relações e estabelecer o fluxo do nosso programa visual. Podemos pensar neles literalmente como fios elétricos que carregam impulsos de dados de um objeto para o seguinte.

### Fluxo do programa <a href="#program-flow" id="program-flow"></a>

Os fios conectam a porta de saída de um nó à porta de entrada de outro nó. Essa direcionalidade estabelece o **Fluxo de dados** no programa visual.

As portas de entrada estão no lado esquerdo e as portas de saída estão localizadas no lado direito dos nós; portanto, geralmente podemos dizer que o fluxo do programa se move da esquerda para a direita.

![](<images/nodes and wires - flow of data.jpg>)

### Criar fios <a href="#creating-wires" id="creating-wires"></a>

Crie um fio clicando com o botão esquerdo do mouse em uma porta e, em seguida, clicando com o botão esquerdo do mouse na porta de outro nó para criar uma conexão. Enquanto estamos no processo de criar uma conexão, o fio aparecerá a tracejado e se tornará numa linha contínua quando for conectado com êxito.

Os dados sempre fluirão por este fio da saída para a entrada; no entanto, podemos criar o fio em qualquer direção em termos da sequência de clicar nas portas conectadas.

![](<images/nodes and wires - creating a wire.gif>)

### Editar fios <a href="#editing-wires" id="editing-wires"></a>

Frequentemente, queremos ajustar o fluxo do programa em nosso programa visual editando as conexões representadas pelos fios. Para editar um fio, clique com o botão esquerdo do mouse na porta de entrada do nó que já está conectado. Você agora tem duas opções:

* Para alterar a conexão para uma porta de entrada, clique com o botão esquerdo do mouse em outra porta de entrada

![](<images/nodes and wires - edit wire change port (2).gif>)

* Para remover o fio, afaste o fio e clique com o botão esquerdo do mouse no espaço de trabalho

![](<images/nodes and wires - edit wires remove.gif>)

* Reconecte vários fios usando Shift+clique com o botão esquerdo do mouse

![](<images/nodes and wires - edit multi ports.gif>)

* Duplique um fio usando Ctrl+clique com o botão esquerdo do mouse

![](<images/nodes and wires - duplicate wire.gif>)

#### Fios padrão vs. realçados <a href="#wire-previews" id="wire-previews"></a>

Por padrão, nossos fios serão visualizados com um traço cinza. Quando um nó é selecionado, ele renderizará qualquer fio de conexão com o mesmo realce azul-claro do nó.

![](<images/nodes and wires - default vs highlighted wires.jpg>)

> 1. Fio realçado
> 2. Fio padrão

**Ocultar fios por padrão**

Caso você prefira ocultar os fios no gráfico, pode encontrar essa opção em Vista > Conectores > desmarque Mostrar conectores.

Com essa configuração, somente os nós selecionados e seus fios unidos serão mostrados no realce azul claro.

![](<images/nodes and wires - hide wires setting (1).gif>)

#### Ocultar somente fio individual

Também é possível ocultar somente o fio selecionado clicando com o botão direito do mouse na saída Nós > e selecionando Ocultar fios

![](<images/nodes and wires - hide selected wire.gif>)
