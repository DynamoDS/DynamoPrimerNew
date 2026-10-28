# RevitMCP e DynamoMCP

O Autodesk Assistant obtém seus recursos por meio de servidores MCP. Cada servidor fornece ao Assistant um conjunto de ferramentas, e cada ferramenta executa uma função no produto correspondente.

Quando você trabalha no Revit, dois desses servidores são importantes: **RevitMCP** e **DynamoMCP**. Eles não são opções concorrentes, e você não precisa escolher entre eles. Eles atuam em níveis diferentes, e o Assistant usa aquele que melhor atende à solicitação.

### RevitMCP: operações diretas no modelo

O RevitMCP, disponibilizado como [Servidor MCP público do Revit](https://help.autodesk.com/cloudhelp/2027/PTB/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm), agrupa um conjunto selecionado de operações de APIs do Revit como ferramentas MCP. Cada ferramenta é mapeada para uma ação definida no modelo: localizar elementos, verificar parâmetros e contagens, editar parâmetros em massa e capturar instantâneos de vistas.

Esse é o caminho mais curto para uma tarefa já contemplada pelo conjunto de ferramentas. Uma solicitação resulta em uma chamada, e o modelo é alterado imediatamente. Não é necessário criar um gráfico nem executá-lo posteriormente.

### DynamoMCP: ferramentas para criar a operação

O DynamoMCP funciona um nível acima. Suas ferramentas não executam ações de modelagem diretamente. Em vez disso, elas criam um gráfico do Dynamo: colocam nós, conectam-nos entre si, definem valores e executam o resultado. Consulte [O que é o servidor MCP do Dynamo](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md) para obter a lista completa de ferramentas.

Isso é importante devido à natureza dos nós. Cada nó na biblioteca do Dynamo corresponde a uma ou mais chamadas de APIs já escritas, testadas e documentadas. Os nós de pacotes e os nós Python estão disponíveis da mesma maneira. Assim, um pequeno conjunto de ferramentas de criação de gráficos permite acessar toda a biblioteca do Dynamo.

O resultado é que as ferramentas do DynamoMCP criam novos recursos, em vez de apenas usarem os existentes. Em vez de uma lista fixa de ações, o Assistant monta a ação solicitada com base nos nós. Na prática, as ferramentas MCP que escrevem gráficos se comportam como ferramentas que criam mais ferramentas.

### A diferença na prática

Imagine que você solicita a criação de identificadores de portas em um nível.

- Com o RevitMCP, a tarefa é executada por uma chamada direta, e os identificadores são exibidos.
- Com o DynamoMCP, o Assistant coloca os nós que coletam as portas, filtra-os para o nível indicado, cria os identificadores e, em seguida, executa o gráfico.

As duas abordagens identificam as portas. A segunda também deixa algo na tela.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| O que a ferramenta faz | Executa uma operação definida do Revit | Cria e executa um gráfico do Dynamo |
| Alcance | As operações incluídas no conjunto de ferramentas | Qualquer nó da biblioteca, incluindo nós de pacotes e nós Python. |
| O que você obtém | A alteração no modelo | A alteração no modelo, além do gráfico que a gerou |
| Ideal para | Uma única tarefa bem definida | Lógica que você deseja inspecionar, ajustar, reutilizar ou compartilhar |

### Por que vale a pena ter o gráfico

Um gráfico é um registro do trabalho, não apenas o resultado. Quando ele estiver na tela, será possível:

- Analisá-lo e ver exatamente o que foi feito.
- Alterar um valor e executá-lo novamente, sem precisar solicitar a tarefa inteira outra vez.
- Salvá-lo para executá-lo em outro projeto ou compartilhá-lo com um colega.
- Publicá-lo no Reprodutor do Dynamo, permitindo que pessoas que não usam o Dynamo também o executem.

A primeira solicitação é a que mais toma tempo. Cada execução posterior praticamente não tem custo adicional.

{% hint style="info" %}
No Dynamo Sandbox, o Assistant acessa apenas o DynamoMCP, pois não há nenhum modelo do Revit para ele manipular. No Revit, ambos estão disponíveis.
{% endhint %}

### Escolher o que solicitar

Não é necessário nomear um servidor no prompt. Basta descrever o resultado, e o Assistant selecionará as ferramentas.

Se você quiser especificamente um gráfico, deixe isso claro. Prompts como *“crie um gráfico que identifique portas no Nível 2”* indicam ao Assistant que o objetivo é obter uma solução reutilizável, não apenas as portas identificadas.

### Leitura adicional

- [Autodesk Assistant (Visualização técnica)](https://help.autodesk.com/cloudhelp/2027/PTB/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm) — o que o Assistant faz no Revit, na ajuda do Revit 2027.
- [Servidor MCP público do Revit (Visualização técnica)](https://help.autodesk.com/cloudhelp/2027/PTB/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm) — a vertente do MCP para o Revit, na ajuda do Revit 2027.
- [Autodesk Assistant: visão geral e perguntas frequentes](https://www.autodesk.com/br/support/technical/article/caas/sfdcarticles/sfdcarticles/PTB/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html) — quais produtos incluem o Assistant e como usá-lo.
