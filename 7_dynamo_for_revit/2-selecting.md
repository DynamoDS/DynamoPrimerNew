# Seleção

### Selecionar os elementos do Revit

O Revit é um ambiente com abundância de dados. Isso nos dá uma gama de capacidades de seleção que se expande muito além de “apontar e clicar”. Podemos consultar o banco de dados do Revit e vincular dinamicamente os elementos do Revit à geometria do Dynamo durante a execução de operações paramétricas.

A biblioteca do Revit na interface do usuário oferece uma categoria “Seleção” que possibilita várias maneiras de selecionar a geometria.

![](../.gitbook/assets/select\_revit\_elements\_01.jpg)

### Hierarquia do Revit

Para selecionar os elementos do Revit corretamente, é importante ter um entendimento completo da hierarquia de elementos do Revit. Deseja selecionar todas as paredes em um projeto? Selecione por categoria. Deseja selecionar todas as cadeiras Eames em seu lobby moderno de meados do século? Selecione por família.

Vamos fazer uma rápida revisão da hierarquia do Revit.

![](images/2/hierarchy.png)

Você se lembra da taxonomia da biologia? Reino, Filo, Classe, Ordem, Família, Gênero, Espécie? Os elementos do Revit são categorizados de forma similar. Em um nível básico, é possível dividir a hierarquia do Revit em Categorias, Famílias, Tipos* e Instâncias. Uma instância é um elemento de modelo individual (com uma ID exclusiva), enquanto uma categoria define um grupo genérico (como “paredes” ou “pisos”). Com o banco de dados do Revit organizado dessa forma, é possível selecionar um elemento e escolher todos os elementos similares com base em um nível especificado na hierarquia.

{% hint style="warning" %}
 *Os tipos no Revit são definidos de forma diferente dos tipos na programação. No Revit, um tipo se refere a uma ramificação da hierarquia, em vez de um “tipo de dados”. 
{% endhint %}

### Navegação no banco de dados com os nós do Dynamo

As três imagens abaixo mostram as categorias principais para a seleção de elementos do Revit no Dynamo. Essas são ótimas ferramentas para usar em combinação, e vamos explorar algumas delas nos exercícios seguintes.

_Apontar e clicar_ é a forma mais fácil de selecionar diretamente um elemento do Revit. É possível selecionar um elemento do modelo completo ou partes de sua topologia (como uma face ou uma aresta). Isso permanece vinculado dinamicamente ao objeto do Revit. Portanto, quando o arquivo do Revit atualizar sua localização ou parâmetros, o elemento do Dynamo referenciado será atualizado no gráfico.

![](../.gitbook/assets/selecting\_database\_navigation\_with\_dynamo\_nodes\_01.jpg)

Os _menus suspensos_ criam uma lista de todos os elementos acessíveis em um projeto do Revit. É possível usar essa opção para referenciar elementos do Revit que não são necessariamente visíveis em uma vista. Essa é uma ótima ferramenta para consultar elementos existentes ou criar novos em um projeto do Revit ou em um editor de família.

![](../.gitbook/assets/selecting _database_navigation_with_dynamo_nodes_02.png)

Também é possível selecionar o elemento do Revit por camadas específicas na _hierarquia do Revit_. Essa é uma opção eficaz para personalizar grandes matrizes de dados na preparação da documentação ou da instanciação generativa e personalização.

![IU](../.gitbook/assets/allelements.jpg)

Com as três imagens acima em mente, vamos nos aprofundar em um exercício que seleciona elementos de um projeto básico do Revit na preparação para os aplicativos paramétricos que criaremos nas seções restantes deste capítulo.

## Exercício

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="datasets/2/Revit-Selecting.zip" %}

Neste arquivo de exemplo do Revit, temos três tipos de elementos de uma construção simples. Usaremos isso como exemplo para selecionar elementos do Revit no contexto da hierarquia do Revit.

![](<../.gitbook/assets/selecting_exercise_01 (1) (1).jpg>)

> 1. Massa de construção
> 2. Vigas (Framing estrutural)
> 3. Treliças (componentes adaptativos)

Quais conclusões podemos tirar dos elementos atualmente na vista do projeto do Revit? E a que distância da hierarquia precisamos ir para selecionar os elementos apropriados? Isso, é claro, se tornará uma tarefa mais complexa ao trabalhar em um projeto grande. Há muitas opções disponíveis: é possível selecionar elementos por categorias, níveis, famílias, instâncias etc.

### Selecionar a massa e as superfícies

![](../.gitbook/assets/selecting\_exercise\_02.jpg)

> 1. Como estamos trabalhando com uma configuração básica, vamos selecionar a massa da construção escolhendo _“Massa”_ no nó suspenso Categorias. Isso pode ser encontrado na guia Seleção do Revit.
> 2. A saída da categoria Massa é apenas a própria categoria. Precisamos selecionar os elementos. Para fazer isso, usamos o nó _“Todos os elementos da categoria”_.

Neste ponto, observe que não vemos nenhuma geometria no Dynamo. Selecionamos um elemento do Revit, mas não convertemos o elemento na geometria do Dynamo. Essa é uma separação importante. Se você selecionasse um grande número de elementos, não seria uma boa ideia visualizá-los no Dynamo, pois isso deixaria o sistema inteiro lento. O Dynamo é uma ferramenta para gerenciar um projeto do Revit sem executar necessariamente operações de geometria, e vamos examinar isso na próxima seção deste capítulo.

Neste caso, estamos trabalhando com geometria simples, por isso queremos trazer a geometria para a visualização do Dynamo. Há um número verde ao lado de “BldgMass” no nó de inspeção acima. Isso representa a ID do elemento e nos informa que estamos lidando com um elemento do Revit, não com a geometria do Dynamo. A próxima etapa é converter esse elemento do Revit em geometria no Dynamo.

![](../.gitbook/assets/selecting\_exercise\_03.jpg)

> 1. Usando o nó _Element.Faces_, obtemos uma lista de superfícies que representam cada face da massa do Revit. Agora, podemos ver a geometria na viewport do Dynamo e começar a referenciar a face para operações paramétricas.

Veja a seguir um método alternativo. Nesse caso, estamos deixando de lado a seleção através da hierarquia do Revit _(“Todos os elementos da categoria”)_ e optando por selecionar explicitamente a geometria no Revit.

![](../.gitbook/assets/selecting\_exercise\_04.jpg)

> 1. Usando o nó _“Selecionar o elemento do modelo”_, clique no botão *“selecionar” *(ou _“alterar”_). Na viewport do Revit, selecione o elemento desejado. Neste caso, estamos selecionando a massa da construção.
> 2. Em vez de _Element.Faces_, é possível selecionar a massa completa como uma geometria sólida usando _Element.Geometry_. Isso seleciona toda a geometria contida naquela massa.
> 3. Usando _Geometry.Explode_, podemos obter a lista de superfícies novamente. Esses dois nós funcionam da mesma forma que _Element.Faces_, mas oferecem opções alternativas para examinar a geometria de um elemento do Revit.

Usando algumas operações básicas de lista, podemos consultar uma face de interesse.

![](images/2/selecting - exercise 05.jpg)

> 1. Primeiro, gere os elementos selecionados anteriormente para o nó Element.Faces.
> 2. Em seguida, use o nó _List.Count_ que revela que estamos trabalhando com 23 superfícies na massa.
> 3. Referenciando esse número, alteramos o Valor máximo de um *controle deslizante de número inteiro* para _“22”_.
> 4. Usando _List.GetItemAtIndex_, inserimos as listas e o *controle deslizante de número inteiro* para o _índice_. Deslizando a seleção, paramos quando chegamos ao _índice 9_ e isolamos a fachada principal que hospeda as treliças.

A etapa anterior era um pouco complicada. Podemos fazer isso muito mais rápido com o nó _“Selecionar face”_. Isso nos permite isolar uma face que não é um elemento em si no projeto do Revit. Aplica-se a mesma interação como em _“Selecionar o elemento do modelo”_, exceto pelo fato de que selecionamos a superfície em vez do elemento completo.

![](../.gitbook/assets/selecting\_exercise\_06.jpg)

Suponha que desejamos isolar as paredes da fachada principal do edifício. É possível usar o nó _“Selecionar faces”_ para fazer isso. Clique no botão “Selecionar” e, em seguida, selecione as quatro fachadas principais no Revit.

![](../.gitbook/assets/selecting\_exercise\_07.jpg)

Após selecionar as quatro paredes, clique no botão “Concluir” no Revit.

![](../.gitbook/assets/selecting\_exercise\_08.jpg)

As faces são importadas para o Dynamo como superfícies.

![](../.gitbook/assets/selecting\_exercise\_09.jpg)

### Selecionar vigas

Agora, vamos analisar as vigas sobre o átrio.

![](../.gitbook/assets/selecting\_exercise\_10.jpg)

> 1. Use o nó _“Selecionar o elemento do modelo”_ para selecionar uma das vigas.
> 2. Conecte o elemento de viga ao nó _Element.Geometry_. Agora, temos a viga na viewport do Dynamo.
> 3. É possível aumentar o zoom na geometria com um nó _Watch3D_ (se você não visualizar a viga no nó de Inspeção 3D, clique com o botão direito do mouse e pressione “zoom para ajustar”).

Uma pergunta que pode surgir frequentemente nos fluxos de trabalho do Revit/Dynamo: como posso selecionar um elemento e obter todos os elementos similares? Como o elemento selecionado do Revit contém todas as suas informações hierárquicas, podemos consultar seu tipo de família e selecionar todos os elementos daquele tipo.

![](../.gitbook/assets/selecting\_exercise\_11.jpg)

> 1. Conecte o elemento de viga a um nó _Element.ElementType_.
> 2. O nó de _Inspeção_ revela que a saída é agora um símbolo de família, em vez de um elemento do Revit.
> 3. _Element.ElementType_ é uma consulta simples; portanto, podemos fazer isso no bloco de código com a mesma facilidade que com `x.ElementType;` e obter os mesmos resultados.

![](../.gitbook/assets/selecting\_exercise\_12.jpg)

> 1. Para selecionar as vigas restantes, usaremos o nó _“Todos os elementos do tipo de família”_.
> 2. O nó de inspeção mostra que selecionamos cinco elementos do Revit.

![](../.gitbook/assets/selecting\_exercise\_13.jpg)

> 1. Também podemos converter todos esses cinco elementos para a geometria do Dynamo.

E se tivéssemos 500 vigas? A conversão de todos esses elementos na geometria do Dynamo seria muito lenta. Se o Dynamo estiver demorando muito para calcular os nós, será recomendável usar a funcionalidade do nó “congelar” para pausar a execução das operações do Revit enquanto desenvolve o gráfico. Para obter mais informações sobre o congelamento de nós, consulte a seção “[Congelar](../essential-nodes-and-concepts/5\_geometry-for-computational-design/5-6\_solids.md#freezing)” no capítulo de sólidos.

Em qualquer caso, se precisarmos importar 500 vigas, precisaremos de todas as superfícies para executar a operação paramétrica prevista? Ou podemos extrair informações básicas das vigas e executar tarefas gerativas com a geometria fundamental? Essa é uma pergunta que vamos ter em mente, à medida que avançamos neste capítulo. Por exemplo, em seguida, vamos analisar o sistema de treliça.

### Selecionar as treliças

Usando o mesmo gráfico de nós, selecione o elemento de treliça ao invés do elemento de viga. Antes de fazer isso, exclua Element.Geometry da etapa anterior.

![](../.gitbook/assets/selecting\_exercise\_14.jpg)

Em seguida, estamos prontos para extrair algumas informações básicas do tipo de família de treliças.

![](../.gitbook/assets/selecting\_exercise\_15.jpg)

> 1. No nó _Inspeção_, podemos ver que temos uma lista de componentes adaptativos selecionados no Revit. Queremos extrair as informações básicas; por isso, começamos com os pontos adaptativos.
> 2. Conecte o nó _“Todos os elementos do tipo de família”_ ao nó _"AdaptiveComponent.Location"_. Isso nos fornece uma lista de listas, cada uma com três pontos que representam as localizações dos pontos adaptativos.
> 3. A conexão de um nó _"Polygon.ByPoints"_ retorna uma policurva. Podemos ver isso na viewport do Dynamo. Com esse método, visualizamos a geometria de um elemento e abstraímos a geometria da matriz restante dos elementos (que pode ser maior em número do que este exemplo).

{% hint style="info" %}
 Dica: Se você clicar no número verde de um elemento do Revit no Dynamo, a viewport do Revit efetuará o zoom para aquele elemento. 
{% endhint %}
