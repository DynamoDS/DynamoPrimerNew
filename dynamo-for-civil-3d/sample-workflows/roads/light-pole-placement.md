# Posicionamento de postes de luz

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Um dos muitos casos de uso excelentes do Dynamo é posicionar objetos separados dinamicamente ao longo de um modelo de corredor. Geralmente, os objetos precisam ser posicionados em localizações independentes das montagens inseridas ao longo do corredor, o que é uma tarefa muito entediante para ser realizada manualmente. E, quando a geometria horizontal ou vertical do corredor muda, isso introduz uma quantidade significativa de retrabalho.

## Objetivo

> :dart: Posicionar referências de bloco de postes de luz ao longo de um corredor em valores de estaca especificados em um arquivo do Excel. 

## Principais conceitos

> * Ler dados de um arquivo externo (neste caso, Excel)
> * Organizar dados em dicionários
> * Usar sistemas de coordenadas para controlar a posição/escala/rotação
> * Posicionar referências de bloco
> * Visualizar a geometria no Dynamo

## Compatibilidade de versão

{% hint style="success" %}
 Este gráfico será executado no **Civil 3D 2020** e versões superiores. 
{% endhint %}

## Conjunto de dados

Comece fazendo o download dos arquivos de amostra abaixo e, em seguida, abrindo o arquivo DWG e o gráfico do Dynamo.

{% hint style="info" %}
 É melhor que o arquivo do Excel seja salvo no mesmo diretório que o gráfico do Dynamo. 
{% endhint %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## Solução

Apresentamos a seguir uma visão geral da lógica no gráfico.

> 1. Ler o arquivo do Excel e importar os dados para o Dynamo
> 2. Obter as linhas de recurso da linha base do corredor especificada
> 3. Gerar os sistemas de coordenadas ao longo da linha de recurso do corredor nas estacas desejadas
> 4. Usar os sistemas de coordenadas para posicionar referências de bloco no espaço do modelo

Vamos começar

### Obter os dados do Excel

Neste gráfico de exemplo, vamos usar um arquivo do Excel para armazenar os dados que o Dynamo usará para posicionar as referências de bloco dos postes de luz. A tabela tem a aparência a seguir.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Estrutura da tabela de arquivos do Excel</p></figcaption></figure>

{% hint style="info" %}
 Usar o Dynamo para ler dados de um arquivo externo (como um arquivo do Excel) é uma ótima estratégia, especialmente quando os dados precisam ser compartilhados com outros membros da equipe. 
{% endhint %}

Os dados do Excel são importados para o Dynamo da seguinte maneira. 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Importação de dados do Excel para o Dynamo</p></figcaption></figure>

Agora que temos os dados, precisamos dividi-los por coluna (_Corredor_, _Linha base_, _PointCode_ etc.) para que possam ser usados no restante do gráfico. Uma forma comum de fazer isso é usar o nó **List.GetItemAtIndex** e especificar o número de índice de cada coluna que desejamos. Por exemplo, a coluna _Corredor_ está no índice 0, a coluna _Linha base_ está no índice 1 etc.

Parece bom, certo? Mas há um problema potencial com essa abordagem. E se a ordem das colunas no arquivo do Excel for alterada no futuro? Ou uma nova coluna for adicionada entre duas colunas? Nesses casos, o gráfico não funcionará corretamente e exigirá uma atualização. Podemos tornar o gráfico à prova de futuro colocando os dados em um **Dicionário**, com os cabeçalhos de coluna do Excel como _chaves_ e o resto dos dados como _valores_.

{% hint style="info" %}
 Se os dicionários forem algo novo para você, veja a seção [5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>Colocar os dados do Excel em um dicionário</p></figcaption></figure>

Isso torna o gráfico mais resiliente porque permite flexibilidade ao alterar a ordem das colunas no Excel. Enquanto os cabeçalhos de coluna permanecerem os mesmos, os dados poderão ser recuperados do dicionário usando sua _chave_ (ou seja, o cabeçalho da coluna), que é o que fazemos em seguida.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>Recuperação de dados do dicionário</p></figcaption></figure>

### Obter as linhas de recurso do corredor

Agora que temos os dados do Excel importados e prontos para serem usados, vamos começar a usá-los para obter algumas informações do Civil 3D sobre os modelos de corredor.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. Selecione o modelo de corredor pelo nome.
> 2. Obtenha uma linha base específica dentro do corredor.
> 3. Obtenha uma linha do recurso na linha base pelo código de ponto.

### Gerar os sistemas de coordenadas

O que vamos fazer agora é gerar **Sistemas de coordenadas** ao longo das linhas de recurso do corredor nos valores de estaca que especificamos no arquivo do Excel. Esses sistemas de coordenadas serão usados para definir a posição, a rotação e a escala das referências de bloco dos postes de luz.

{% hint style="info" %}
 Se os sistemas de coordenadas forem algo novo para você, veja a seção [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>Obtenção dos sistemas de coordenadas ao longo das linhas de recurso do corredor</p></figcaption></figure>

Observe o uso de um bloco de código para rotacionar os sistemas de coordenadas, dependendo do lado em que eles estejam da linha base. Isso pode ser obtido usando uma sequência de vários nós, mas esse é um bom exemplo de uma situação em que é mais fácil escrever manualmente.

{% hint style="info" %}
 Se os blocos de código forem algo novo para você, veja a seção [8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention"). 
{% endhint %}

### Criar as referências de bloco

Está quase. Temos todas as informações necessárias para podermos realmente posicionar as referências de bloco. A primeira coisa a fazer é obter as definições de bloco que desejamos usando a coluna _BlockName_ do arquivo do Excel.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>Obtenção das definições de bloco que desejamos do documento</p></figcaption></figure>

A partir daí, a última etapa é criar as referências de bloco.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>Criação das referências de bloco no espaço do modelo</p></figcaption></figure>

### Resultado

Quando você executar o gráfico, deverá ver novas referências de bloco mostradas no espaço do modelo ao longo do corredor. E aqui está a parte interessante – se o modo de execução do gráfico estiver definido como Automático e você editar o arquivo do Excel, as referências de bloco serão atualizadas automaticamente.

{% hint style="info" %}
 Você pode ler mais sobre os modos de execução de gráficos na seção [3_user_interface](../../../3\_user\_interface/ "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Realização de atualizações no arquivo do Excel e visualização rápida dos resultados no Civil 3D</p></figcaption></figure>

Veja um exemplo de como executar o gráfico usando o **Reprodutor do Dynamo**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Execução do gráfico usando o Reprodutor do Dynamo e visualização dos resultados no Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Se o Reprodutor do Dynamo for algo novo para você, veja a seção [dynamo-player.md](../../dynamo-player.md "mention"). 
{% endhint %}

> :tada: Missão cumprida.

### Bônus: Visualização no Dynamo

Pode ser útil visualizar a geometria do corredor no Dynamo para fornecer contexto. Este modelo específico tem os sólidos do corredor já extraídos no espaço do modelo, então vamos levá-los para o Dynamo. 

Mas há algo mais que precisamos considerar. Os sólidos são um tipo de geometria relativamente “pesada”, o que significa que essa operação diminuirá a velocidade do gráfico. Seria bom se houvesse uma maneira simples de _escolher_ se desejamos visualizar os sólidos ou não. A resposta óbvia é desconectar o nó **Corridor.GetSolids**, mas isso produzirá avisos para todos os nós a jusante, o que é um pouco confuso. Essa é uma situação em que o nó **ScopeIf** realmente se destaca.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. Observe que o nó **Object.Geometry** tem uma barra cinza na parte inferior. Isso significa que a visualização do nó está desativada (acessível clicando com o botão direito do mouse no nó), o que permite que **GeometryColor.ByGeometryColor** evite “brigar” com outra geometria por prioridade de exibição na visualização do plano de fundo.
> 2. O nó **ScopeIf** basicamente permite executar seletivamente uma ramificação inteira de nós. Se a entrada _test_ for falsa, nenhum nó conectado ao nó **ScopeIf** será executado.

Veja o resultado na visualização do plano de fundo do Dynamo.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Visualização da geometria do corredor no Dynamo</p></figcaption></figure>

## Ideias

Veja a seguir algumas ideias sobre como você pode expandir os recursos desse gráfico.

{% hint style="info" %}
 Adicione uma coluna de **rotação** ao arquivo do Excel e use-a para controlar a rotação dos sistemas de coordenadas. 
{% endhint %}

{% hint style="info" %}
 Adicione **deslocamentos horizontais ou verticais** ao arquivo do Excel para que os postes de luz possam se desviar da linha de recurso do corredor, se necessário. 
{% endhint %}

{% hint style="info" %}
 Em vez de usar um arquivo do Excel com valores de estaca, gere os valores de estaca **diretamente no Dynamo** usando uma estaca inicial e um espaçamento típico. 
{% endhint %}
