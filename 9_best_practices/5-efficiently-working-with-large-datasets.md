# Trabalhar de forma eficiente com grandes conjuntos de dados no Dynamo

Esta página apresenta algumas regras práticas para trabalhar de forma eficiente com grandes conjuntos de dados no Dynamo. Será possível usar as dicas para identificar gargalos nos gráficos, de modo que o gráfico seja executado em minutos, em vez de horas.

Conteúdo:
* Geração de geometria vs. triangulação
* Uso de memória
* API do Revit

### Geração de geometria vs. triangulação

No Dynamo, criar uma peça geométrica e desenhá-la são dois eventos completamente diferentes. Em geral, criar geometria é muito mais rápido e usa menos memória do que desenhar o objeto. Você pode pensar na geometria como uma lista de medidas para criar um terno, enquanto a triangulação é o próprio terno. Você pode dizer muita coisa sobre o terno com base em suas medidas: qual o comprimento dos braços, quanto custa, etc., mas quase sempre é preciso ver e experimentar o terno pronto para ver se está bem ou não. Da mesma forma, com a geometria não triangulada, é possível determinar sua caixa delimitadora, área, volume; interseccioná-la com outra geometria; e exportá-la para o SAT ou Revit. No entanto, quase sempre é preciso triangular a geometria para ter uma ideia se ela está correta ou não. 

Se o gráfico do Dynamo tiver muitos objetos e estiver lento durante a execução, você poderá remover as etapas de triangulação do gráfico para acelerar as coisas.  

Os nós de geometria no Dynamo são sempre triangulados*. Isso deixa você com duas opções para trabalhar com geometria não triangulada: nós Python e nós Sem toque. Contanto que você não retorne um objeto de geometria do nó Python ou Sem toque, a geometria não será triangulada. Por exemplo, se o gráfico tiver vários nós de ponto, conectados a vários nós de linha, conectados a vários nós de elevação, conectados a vários nós de espessura, a geometria será triangulada em cada etapa. Em vez disso, será possível agrupar essa lógica em um nó Python ou Sem toque e retornar apenas o objeto final do nó.

Para obter mais informações sobre o uso dos nós Sem toque, consulte a seção [Desenvolvimento do Dynamo](11\_developer\_primer/3\_developing\_for\_dynamo/README.md) deste Manual.

### Uso de memória

Se você não estiver mais triangulando a geometria, poderá encontrar um gargalo de memória devido ao acúmulo excessivo de geometria. Objetos geométricos no Dynamo consomem uma quantidade pequena, mas não insignificante, de memória na criação. Se você estiver trabalhando com centenas de milhares ou milhões de objetos, isso poderá se acumular e causar falha no Dynamo ou no Revit. Na versão 2.5 e posterior do Dynamo, isso é feito implicitamente, descartando os objetos não usados, mas se você estiver usando uma versão anterior à 2.5, uma maneira de evitar a criação de muita geometria será descartar os objetos quando terminar de usá-los. Por exemplo, digamos que você estivesse criando centenas de milhares de NurbsCurves, cada uma exigindo dezenas de pontos. Uma maneira de criá-las é por meio de uma lista bidimensional no Dynamo e alimentá-la em um nó NurbsCurve.ByPoints. No entanto, isso requer a criação de milhões de pontos. Outra maneira é usar um nó Python ou Sem toque. Nesse nó, é possível criar uma dúzia de pontos, alimentá-los em NurbsCurve.ByPoints e depois descartar a dúzia de pontos com a chamada do método .Dispose(). Para obter mais informações sobre o uso dos nós Sem toque, consulte a seção [Desenvolvimento do Dynamo](11\_developer\_primer/3\_developing\_for\_dynamo/README.md) deste Manual. Descartar os objetos de geometria depois de criá-los pode reduzir drasticamente a quantidade de memória usada em determinadas circunstâncias e, embora façamos isso para os usuários do Dynamo 2.5 e versões posteriores, recomendamos que o usuário ainda descarte a geometria explicitamente, se o caso de uso exigir a redução de memória em um momento determinado. Consulte [Melhorias na estabilidade da geometria do Dynamo](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297) para saber mais sobre os novos recursos de estabilidade incluídos no Dynamo 2.5

### API do Revit

Se você estiver descartando objetos agressivamente em um nó Sem toque ou Python e ainda tiver problemas de memória ou desempenho, poderá ser necessário ignorar o Dynamo completamente e criar objetos Revit diretamente por meio da API. Por exemplo, você pode analisar um arquivo Excel para obter informações de pontos e usar essas informações para criar XYZ e outros elementos do Revit por meio de sua API. Neste ponto, o Revit se tornará o maior gargalo, o que não pode ser evitado.