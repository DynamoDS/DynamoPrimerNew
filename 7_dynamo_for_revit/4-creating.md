# Criando

É possível criar uma matriz de elementos do Revit no Dynamo com o controle paramétrico completo. Os nós do Revit no Dynamo oferecem a capacidade de importar elementos de geometrias genéricas para tipos de categoria específicos (como paredes e pisos). Nesta seção, vamos focar na importação de elementos parametricamente flexíveis com componentes adaptativos.

\![](<images/4/creating - dynamo nodes.jpg>)

### Componentes adaptativos

Um componente adaptativo é uma categoria de família flexível que é bem adequada para aplicativos gerativos. Após a instanciação, é possível criar um elemento geométrico complexo que é guiado pela localização fundamental dos pontos adaptativos.

Veja abaixo um exemplo de um componente adaptativo de três pontos no editor de família. Isso gera uma treliça que é definida pela posição de cada ponto adaptativo. No exercício abaixo, usaremos esse componente para gerar uma série de treliças em uma fachada.

![](../.gitbook/assets/ac.jpg)

### Princípios de interoperabilidade

O componente adaptativo é um bom exemplo das práticas recomendadas de interoperabilidade. É possível criar uma matriz de componentes adaptativos definindo os pontos adaptativos fundamentais. E, ao transferir esses dados para outros programas, temos a capacidade de reduzir a geometria a dados simples. A importação e a exportação com um programa como o Excel seguem uma lógica similar.

Suponha que um consultor de fachadas deseje saber a localização dos elementos da treliça sem precisar analisar a geometria totalmente articulada. Na preparação para a fabricação, o consultor pode fazer referência à localização de pontos adaptativos para gerar novamente a geometria em um programa como o Inventor.

O fluxo de trabalho que configuraremos no exercício abaixo nos permite acessar todos esses dados ao criar a definição para a criação de elementos do Revit. Nesse processo, é possível mesclar a conceitualização, a documentação e a fabricação em um fluxo de trabalho contínuo. Isso cria um processo mais inteligente e eficiente para a interoperabilidade.

### Vários elementos e listas

O [primeiro exercício](8-4\_creating.md#exercise) abaixo vai detalhar como o Dynamo referencia os dados para a criação de elementos do Revit. Para gerar vários componentes adaptativos, definiremos uma lista de listas em que cada lista tem três pontos que representam cada ponto do componente adaptativo. Isso será levado em consideração à medida que gerenciarmos as estruturas de dados no Dynamo.

\![](<images/4/creating - multiple elements and lists 01.jpg>)

### Elementos DirectShape

Outro método para importar a geometria paramétrica do Dynamo no Revit é com o DirectShape. Em resumo, o elemento DirectShape e as classes relacionadas oferecem suporte à capacidade de armazenar formas geométricas criadas externamente em um documento do Revit. A geometria pode incluir sólidos ou malhas fechadas. O DirectShape foi projetado principalmente para importar formas de outros formatos de dados, como IFC ou STEP, onde não há informações suficientes disponíveis para criar um elemento “real” do Revit. Como o fluxo de trabalho IFC e STEP, a funcionalidade DirectShape funciona bem com a importação de geometrias criadas pelo Dynamo em projetos do Revit como elementos reais.

Vamos detalhar no [segundo exercício](8-4\_creating.md#exercise-directshape-elements) a importação da geometria do Dynamo como uma DirectShape em nosso projeto do Revit. Usando esse método, podemos atribuir a categoria, o material e o nome de uma geometria importada, mantendo um vínculo paramétrico com o gráfico do Dynamo.

## Exercício: Gerar elementos e listas

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="datasets/4/Revit-Creating.zip" %}

Começando com o arquivo de exemplo desta seção (ou continuando com o arquivo do Revit da sessão anterior), vemos a mesma massa do Revit.

\![](<images/4/creating - exercise 01.jpg>)

> 1. Esse é o arquivo conforme aberto.
> 2. Esse é o sistema de treliça que criamos com o Dynamo, vinculado de forma inteligente à massa do Revit.

Usamos os nós _“Selecionar o elemento do modelo”_ e _“Selecionar face”_, agora estamos descendo uma etapa na hierarquia da geometria e usando _“Selecionar aresta”_. Com o solucionador do Dynamo definido para executar _“Automático”_, o gráfico será continuamente atualizado com as alterações no arquivo do Revit. A aresta que estamos selecionando está vinculada dinamicamente à topologia do elemento do Revit. Desde que a topologia* não seja alterada, a conexão permanecerá vinculada entre o Revit e o Dynamo.

\![](<images/4/creating - exercise 02.jpg>)

> 1. Selecione a curva superior da fachada de vidraça. Ela se expande por todo o comprimento da construção. Se você estiver tendo problemas para selecionar a aresta, lembre-se de escolher a seleção no Revit ao passar o mouse sobre a aresta e pressionar _“Tab”_ até que a aresta desejada seja realçada.
> 2. Usando dois nós _“Selecionar aresta”_, selecione cada aresta que representa o declive transversal no meio da fachada.
> 3. Faça o mesmo para as arestas inferiores da fachada no Revit.
> 4. Os nós de _Inspeção_ revelam que agora temos linhas no Dynamo. Isso é convertido automaticamente na geometria do Dynamo, já que as próprias arestas não são elementos do Revit. Essas curvas são as referências que usaremos para instanciar as treliças adaptativas na fachada.

{% hint style="info" %}\\*Para manter uma topologia consistente, estamos nos referindo a um modelo que não tenha mais faces ou arestas adicionadas. Embora os parâmetros possam alterar sua forma, a maneira como ele é construído permanece consistente. {% endhint %}

Primeiro, precisamos unir as curvas e mesclá-las em uma lista. Dessa forma, podemos _“agrupar”_ as curvas para realizar operações de geometria.

\![](<images/4/creating - exercise 03.jpg>)

> 1. Crie uma lista para as duas curvas no meio da fachada.
> 2. Una as duas curvas numa policurva conectando o componente _List.Create_ a um nó _Polycurve.ByJoinedCurves_.
> 3. Crie uma lista para as duas curvas na parte inferior da fachada.
> 4. Una as duas curvas numa policurva conectando o componente _List.Create_ a um nó _Polycurve.ByJoinedCurves_.
> 5. Por fim, una as três curvas principais (uma linha e duas policurvas) em uma lista.

Desejamos aproveitar a curva superior, que é uma linha, e representa a extensão completa da fachada. Vamos criar planos ao longo dessa linha para fazer a interseção com o conjunto de curvas que agrupamos em uma lista.

\![](<images/4/creating - exercise 04.jpg>)

> 1. Com um _bloco de código_, defina um intervalo usando a sintaxe: `0..1..#numberOfTrusses;`
> 2. Conecte um *controle deslizante de número inteiro *à entrada do bloco de código. Como talvez você tenha adivinhado, isso representará o número de treliças. Observe que a barra deslizante controla o número de itens no intervalo definido de *0 *a _1_.
> 3. Conecte o _bloco de código_ à entrada _param_ de um nó _“Curve.PlaneAtParameter”_ e conecte a aresta superior à entrada da _curva_. Isso gerará dez planos uniformemente distribuídos pela extensão da fachada.

Um plano é uma parte abstrata da geometria, representando um espaço bidimensional infinito. Os planos são excelentes para contorno e interseção, conforme estamos configurando nesta etapa.

\![](<images/4/creating - exercise 05.jpg>)

> 1. Usando o nó _Geometry.Intersect_ (defina a opção de amarra como produto transversal), conecte _Curve.PlaneAtParameter_ à entrada _entidade_ do nó _Geometry.Intersect_ . Conecte o nó principal _List.Create_ à entrada _geometria_. Agora, vemos pontos na viewport do Dynamo que representam a interseção de cada curva com os planos definidos.

Observe que a saída é uma lista de listas de listas. Para nossos fins, há muitas listas. Desejamos fazer uma mesclagem parcial aqui. Precisamos descer uma etapa na lista e nivelar o resultado. Para fazer isso, usaremos a operação _List.Map_, conforme discutido no capítulo da lista do manual.

\![](<images/4/creating - exercise 06.jpg>)

> 1. Conecte o nó _Geometry.Intersect_ à entrada da lista de _List.Map_.
> 2. Conecte um nó _Aplainar_ à entrada f(x) de _List.Map_. Os resultados fornecem três listas, cada uma com uma contagem igual ao número de treliças.
> 3. Precisamos alterar esses dados. Se desejarmos instanciar a treliça, precisaremos usar o mesmo número de pontos adaptativos, conforme definido na família. Esse é um componente adaptativo de três pontos, portanto, em vez de três listas com 10 itens cada (numberOfTrusses), desejamos 10 listas de três itens cada. Desta forma, podemos criar 10 componentes adaptativos.
> 4. Conecte o _List.Map_ a um nó _List.Transpose_. Agora, temos a saída de dados desejada.
> 5. Para confirmar se os dados estão corretos, adicione um nó _Polygon.ByPoints_ à tela e verifique novamente com a visualização do Dynamo.

Da mesma forma que criamos os polígonos, é possível organizar os componentes adaptativos.

\![](<images/4/creating - exercise 07.jpg>)

> 1. Adicione um nó _AdaptiveComponent.ByPoints_ à tela, conecte o nó _List.Transpose_ à entrada _pontos_.
> 2. Usando um nó _Tipos de família_, selecione a família _“AdaptiveTruss”_ e conecte-a à entrada _FamilyType_ do nó _AdaptiveComponent.ByPoints_.

No Revit, agora temos as dez treliças espaçadas uniformemente na fachada.

“Flexibilizar” o gráfico, aumentamos numberOfTrusses para 30 alterando o controle deslizante. Muitas treliças, o que não é muito realista, mas o vínculo paramétrico está funcionando. Depois de verificar, defina numberOfTrusses como 15.

\![](<images/4/creating - exercise 08.gif>)

E, para o teste final, selecionando a massa no Revit e editando os parâmetros de instância, será possível alterar a forma da construção e observar a treliça fazer o mesmo. Lembre-se: esse gráfico do Dynamo precisa estar aberto para que essa atualização seja exibida. O vínculo será interrompido assim que for fechado.

\![](<images/4/creating - exercise 09.jpg>)

## Exercício: Elementos DirectShape

> Faça o download do arquivo de exemplo clicando no link abaixo.
>
> É possível encontrar uma lista completa de arquivos de exemplo no Apêndice.

{% file src="datasets/4/Revit-Creating-DirectShape.zip" %}

Comece abrindo o arquivo de exemplo desta lição: ARCH-DirectShape-BaseFile.rvt.

\![](<images/4/creating - exercise II - 01.jpg>)

> 1. Na vista 3D, vemos a massa da construção da lição anterior.
> 2. Ao longo da aresta do átrio há uma curva de referência, vamos usá-la como uma curva para fazer referência no Dynamo.
> 3. Ao longo da aresta oposta do átrio, há outra curva de referência à qual também vamos fazer referência no Dynamo.

\![](<images/4/creating - exercise II - 02.jpg>)

> 1. Para fazer referência à nossa geometria no Dynamo, usaremos _Selecionar o elemento do modelo_ para cada membro do Revit. Selecione a massa no Revit e importe a geometria para o Dynamo usando _Element.Faces_. A massa agora deve estar visível na visualização do Dynamo.
> 2. Importe uma curva de referência para o Dynamo usando _Selecionar o elemento do modelo_ e _CurveElement.Curve_.
> 3. Importe a outra curva de referência para o Dynamo usando _Selecionar o elemento do modelo_ e _CurveElement.Curve_.

\![](<images/4/creating - exercise II - 03.jpg>)

> 1. Diminuindo o zoom e efetuando o pan à direita no gráfico de exemplo, vemos um grupo grande de nós: são operações geométricas que geram a estrutura do telhado da treliça visível na visualização do Dynamo. Esses nós são criados usando a funcionalidade _Nó para código_, conforme discutido na [seção bloco de código](../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md#Node) do manual.
> 2. A estrutura é guiada por três parâmetros principais: Deslocamento diagonal, Curvatura e Raio.

Efetue o zoom em uma visualização próxima dos parâmetros para este gráfico. Podemos flexibilizá-los para obter resultados de geometria diferentes.

\![](<images/4/creating - exercise II - 04.jpg>)

\![](<images/4/creating - exercise II - 05.jpg>)

> 1. Soltando o nó _DirectShape.ByGeometry_ na tela, vemos que ele tem quatro entradas: _geometria_**,** _categoria_**,** _material_ e _nome_.
> 2. A geometria será o sólido criado com base na parte de criação da geometria do gráfico.
> 3. A entrada de categoria é selecionada usando o nó suspenso _Categorias_. Neste caso, usaremos “Framing estrutural”.
> 4. A entrada de material é selecionada por meio da matriz de nós acima. Embora possa ser mais simplesmente definida como “Padrão” neste caso.

Após executar o Dynamo, de volta no Revit, temos a geometria importada no telhado em nosso projeto. Esse é um elemento do framing estrutural em vez de um modelo genérico. O vínculo paramétrico para o Dynamo permanece intacto.

\![](<images/4/creating - exercise II - 06.jpg>)
